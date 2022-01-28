---
title: A Comparison of Software and Hardware Techniques for x86
Virtualization
type: page
conf: asplos
year: 2006
link: https://pdos.csail.mit.edu/6.828/2018/readings/adams06vmware.pdf
---

## Notes

- characteristics of virtualization
  - fidelity (software on vmm executes identically as real machine)
  - performance (majority of insns. are executed w/o VMM interference)
  - safety (vmm manages all HW resources)

- vmm implementation style --> trap-and-emulate, most prevalent in 1970s

- ideas from classically virtualizable architectures
  - de-privileging
  - shadow structures
  - memory traces

- x86 obstacles to virtualization
  - visibility of privileged state
  - lack of traps when privileged instructions run at user level

## Questions

- Why is x86 un-virtualizable with trap-and-emulate? Give one example.
  - one reason that x86 is not virtualizable because traps are not run for the
    guest when it attempt to execute a privileged instruction. A guest OS cannot run in the highest privilege mode, and in x86 when a privileged instruction is run when not in privileged mode, no trap is generated.
- How are jump instructions translated?
  - jump instructions are translated by using a continuation address which is
    calculated by the tranlator. For conditional jumps, it is translated into
    two instructions. The first being the conditional jump with a new address,
    and the 2nd being a jump to the fall-through address of the next instruction
    after original conditional jump
- With hardware virtualization extensions (e.g., Intel VT), do we still need
  binary translation? Why or why not?
    - processors with hardware extensions for virtualization don't require
      binary translation because the instructions run natively, and when
      modifying privileged state (such as in a page fault), the hardware
      triggers the vmexit back to the VMM where it can be properly handled and
      then enter back into the guest. BT isn't required because the processor
      handles triggering vmexits at the appropriate times.


### summary

This paper presents an analysis of VMM performance using first-generation hardware
support and compares it to the software-only VMMs which had been used previously.
They summarize the techniques used in software-only VMMs and compare it to the new
VMM design which uses hardware. The authors show that the hardware support is lacking and doesn't actually result in performance improvement. Through detailed performance analysis the authors suggest improvements to the virtualization hardware support
such as hardware-assisted MMU emulation for guests.
