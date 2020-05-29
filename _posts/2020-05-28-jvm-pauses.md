---
layout: post
title: JVM Pauses - It's more than GC
author: Zac Blanco
keywords: java, jvm, pause, gc, collection, garbage, safepoint, shenandoah, azul, zing, GuaranteedSafepointInterval, RevokeBias,
permalink: /blog/jvm-safepoint-pauses/
date: 2020-05-28
---

* Table of Contents
{:toc}

### Background

Everyone knows and loves the good-ol' JVM
[GC](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
pause. [New GC
algorithms](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/cms.html)
have been
[introduced](https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html)
to [improve GC
latencies](https://wiki.openjdk.java.net/display/zgc/Main) or [prevent
the need](https://wiki.openjdk.java.net/display/shenandoah/Main) to [pause
altogether](https://www.azul.com/resources/azul-technology/azul-c4-garbage-collector/).
However, that's not the end of the story for JVM pauses. Unfortunately,
there are a variety of other housekeeping tasks within the JVM that
force it to stop executing user code[^1].

### Safepoint Operations

It is important to understand the concept of a _safepoint_ within the JVM. A
_safepoint_ is a point in program execution where the state of the
program is known and can be examined. Things like registers, memory,
locks, etc. For the JVM to completely pause and run tasks (such as GC),
**all threads** must come to a safepoint. For example, to retrieve a
stack trace on a thread we must come to a safepoint. This also means
tools like _jstack_ require that all threads of the program being
examined must be able to reach a safepoint.

To be a wee bit more precise than what this inexperienced software
engineer has to say, the [HotSpot
Glossary](https://openjdk.java.net/groups/hotspot/docs/HotSpotGlossary.html)
defines a _safepoint_ as:

> A point during program execution at which all GC roots are known and
> all heap object contents are consistent. From a global point of view,
> all threads must block at a safepoint before the GC can run. (As a
> special case, threads running JNI code can continue to run, because
> they use only handles. During a safepoint they must block instead of
> loading the contents of the handle.) From a local point of view, a
> safepoint is a distinguished point in a block of code where the
> executing thread may block for the GC. Most call sites qualify as
> safepoints. There are strong invariants which hold true at every
> safepoint, which may be disregarded at non-safepoints. Both compiled
> Java code and C/C++ code be optimized between safepoints, but less so
> across safepoints. The JIT compiler emits a GC map at each safepoint.
> C/C++ code in the VM uses stylized macro-based conventions (e.g.,
> TRAPS) to mark potential safepoints.

While GC is one of the most common safepoint operations, there are many
operations[^2] that must be run while threads are at safepoints.

*   User Invoked:
    *   Deadlock detection
    *   JVMTI
    *   Thread Dumps
*   Revoking [Biased
    Locks](https://blogs.oracle.com/dave/biased-locking-in-hotspot)
*   Compiled method Deoptimization
*   GC
*   Run at regular intervals (see `-XX:GuaranteedSafepointInterval`[^opt_ref])
    *   Monitor Deflation
    *   Inline Cache Cleaning
    *   Invocation Counter Delay
    *   Compiled Code Marking

All these operations force the JVM to come to a safepoint in order to
run some kind of thread. With this understanding we can then decompose a
safepoint operation times into two categories

![JVM pause time diagram](/assets/images/jvm-pause/pause-time-diagram.png)

*   Time taken from initiating the safepoint request until all threads
    reach a safepoint
*   Time taken to perform safepoint operation.

You can view this behavior in a JVMs by adding the
`-XX:+PrintGCApplicationStoppedTime` and `-XX:+PrintGCDetails`[^more_gc_flags] flags.
This will print messages to the GC logs that look like:

```
Total time for which application threads were stopped: 0.0572646 seconds, Stopping threads took: 0.0000088 seconds
```

The _0.0572646 seconds_ is the sum total time that threads may have been
stopped for. This includes the blue and red sections above. The
_0.0000088 seconds_ is the time spent in the blue section, where it took
that much time to stop all threads at a safepoint. Even though the JVM
flag is named `-XX:+PrintGCApplicationStoppedTime`[^3] it actually logs
all JVM pauses, not just GC.

So, if applications are not responding, it may be because the JVM has
reached a safepoint and is running some internal operations. May it be
GC, lock bias revocation, cache line invalidation, etc. The issue is
that we need to figure out exactly what is triggering the pause in the
first place if anything, and then investigate exactly why that operation
is taking such a long time.

To do that more logging is required to see exactly which safepoint
operations are occurring. The flags that need to be added to the JVM are
`-XX:+PrintSafepointStatistics  -XX:PrintSafepointStatisticsCount=1`.
Adding these two arguments will print to stdout logs every time a
safepoint operation occurs. The log output consists of two lines and
looks like the following (times are in ms):


```
  vmop [threads: total initially_running wait_to_block] [time: spin block sync cleanup vmop] page_trap_count
155623.484: RevokeBias [ 1595 0 0 ] [ 0 0 127 7 0 ]  0  
```

[Breaking](https://plumbr.io/blog/performance-blog/logging-stop-the-world-pauses-in-jvm)
down the output:

*   **155623.484:** the time in seconds since the JVM started
*   **RevokeBias (vmop)**: The VM operation running during this
    safepoint.
*   **1595 (total)**: The total number of threads in the VM when the
    safepoint was requested
*   **0 (initially running)**: The number of threads running at the
    beginning of the safepoint operation
*   **0 (wait to block)**: The number of threads blocked when the VM
    began the operation execution
*   **0 (spin)**: The time spent spinning waiting for threads to reach a
    safepoint
*   **0 (block)**: The time spent waiting for threads to block for the
    safepoint
*   **127 (sync)**: The total time spent synchronizing the threads for
    the safepoint. This is a combination of spin + block + other
    activity. This value is what I regard as the  "time to safepoint" or
    TTSP. In the GC logs, the TTSP or "Time stopping threads" typically
    only includes the spin + block times based on my analysis.
*   **7 (cleanup)**: Time spent on internal VM cleanup activities
*   **0 (vmop)**: The time spent on the actual safepoint operation
    (RevokeBias). This can be any safepoint operation.
*   **0 (page_trap_count)**: Page trap count

It's important to reiterate that the spin+block+sync times are TTSP, and
that means that many threads can be blocked before even beginning the
safepoint operation which is why the total pause time of a JVM must be
considered TTSP + cleanup + vmop.

With this information we can handily take any JVM logs and figure out
which operations were running. It's also critical to consider the
safepoint logs alongside GC logs. Otherwise it's possible to miss
information about TTSP mentioned above.

### Analyzing Safepoint Pauses

So now that we know all about safepoints and how to get their
statistics, we need to know about what possible causes can prevent Java
threads from coming to a safepoint. Some of those causes are[^1]:

- Large object initialization
  - i.e. initializing a 10GB array. (Single threaded, zeroing thearray)
- Array copying
- JNI Handle Allocation
- JNI Critical Regions
- [Counted Loops](https://psy-lob-saw.blogspot.com/2015/12/safepoints.html)
- NIO Mapped Byte Buffers
- Memory mapped portion of a file

Usually, if a program is taking a long time to reach a safepoint there
is a systemic issue in the code where it performs one or more of the
operations above for extended periods of time without allowing the JVM
to come to a safepoint.

Fortunately, there are even more options that can be added to the JVM to
log when it takes a longer period of time than expected to come to a
safepoint[^4].

```
-XX:+SafepointTimeout -XX:SafepointTimeoutDelay=<timeout in ms>
```

These two options will print to the VM log / stdout any threads which
have failed to reach a safepoint after the specified time period. This
can help developers troubleshoot which threads might be causing extended
pauses of the JVM.

Next, we'll do a real analysis on an example of a large TTSP and what
the JVM and GC logs will look like. The data below was collected from a
long running java server application.

Take the following graph of a GC log from the "Time Application Threads
were Stopped" and the "Time Stopping Application Threads":

![GC Pause Times Over 3-day period](/assets/images/jvm-pause/gc_log_analysis.png "GC Log")

The graph above plots the GC log of a worker. It shows a few spikes with
a max of ~24sec of pause time reported by the JVM, and for each spike
most of the thread pause time was actually spent stopping the threads!
Not performing the operations. Notice the correlation of spikes in
"Threads waiting to stop" vs "Threads stopped time". It's clear
something is amiss.

Now, let's take a look at the safepoint statistics plotted on the same
timeline. Each statistic is represented individually on its own graph.

![Safepoint Logs over a 3-day period](/assets/images/jvm-pause/safepoint_log_analysis.png "Safepoint Statistics")

This graph shows the times for each safepoint statistic. Remember the
sync time is generally equal to the TTSP, but compare the y-scale time
versus the other graph above.

This graph shows spikes correlated at the exact same times as some of
the GC pauses on the graph above, but the GC pause times reported peaked
at a measly 24s. The values for this graph (under the "sync" operation)
show spikes of over 200000ms, or close to 5 minutes. Notice that the
graph for "Op Time" (vmop) has a scale of only 500ms.

This means that the "Time Stopping Application Threads" reported in GC
logs should not always be trusted. The safepoint statistics are more
fine grained, but provide more information about how the application
behaves. The reason we didn't catch issues in the application initially
is because the GC log wasn't giving the full picture of how the
application stopped for long periods of time on the order of minutes.

### Conclusion

Hopefully with this knowledge in-hand it will be easier to detect,
identify, and respond promptly when an application begins to fail due to
a JVM pause. The most helpful JVM flags to gather info from are:


*   `-XX:+PrintGCApplicationStoppedTime`
*   `-XX:+PrintGCDetails`
*   `-XX:+PrintSafepointStatistics`
*   `-XX:PrintSafepointStatisticsCount=1`
*   `-XX:+SafepointTimeout`
*   `-XX:SafepointTimeoutDelay=<ms before timeout log is printed>`

By analyzing the GC log and safepoint statistics together it is easy to
get a holistic view of how the JVM performs with respect to pause times.
Usually, this information is enough to identify a cause or point in the
right direction of a culprit (a thread name from `-XX:SafepointTimeout`). 


## Notes

[^1]: There is a [highly informative
     talk](https://www.youtube.com/watch?v=Y39kllzX1P8) given by an Azul
     (previously Oracle JDK, HP, Intel) engineer explaining the reasons
     (besides GC)  that can pause the JVM. Much of the information in
     this post comes from that video.

[^2]: [All Possible VM Operations in JDK
     8](http://hg.openjdk.java.net/jdk8/jdk8/hotspot/file/87ee5ee27509/src/share/vm/runtime/vm_operations.hpp#l41)

[^3]: There is another flag: `-XX:+PrintGCApplicationConcurrentTime`
     that will print the opposite; The time the application spends
     running between pauses, every time a pause occurs.

[^4]: These guys really thought ahead didn't they? There's also a
     `-XX:+TraceSafepointCleanupTime` which gives more detail about time
     spent on cleanup operations during the safepoint.

[^opt_ref]: [This reference
    application](http://jvm-options.tech.xebia.fr/#) containing all JVM
    options is _very_ useful

[^more_gc_flags]: There is an additional flag which can may be of use:
                `-XX:+PrintGCApplicationConcurrentTime`.
