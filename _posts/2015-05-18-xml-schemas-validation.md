---
layout: post
title: "Quick Tips: XML Schemas and Validation"
permalink: /blog/markup-language/xml-schema-validation/
keywords: xml, java, schema, validation, namespaces, markup language
author: Zac Blanco
description: How to validate an XML file using a custom schema in Java.
---


A quick side note before the post: I've finally started working on for the summer, so I'm hoping that I will be able to write posts more frequently than I did during the semester.

### XML Schemas

_What the heck is a schema anyway?_

You can think of a schema as almost like writing a class in Java. It basically gives us a layout that we want out XML files to follow. It's basically giving us a blueprint that we want our XML files to conform to.

_Why should I validate my XML files?_

Well it's plain and simple. If your program parses XML files, and it needs to file a certain format, you want to make sure that all the files you process are in the correct format. If you don't validate your files it could break your program. Think of it as like dumping pasta into your toaster oven because you want to cook your pasta. You can't just cook everything by putting it in the toaster. You have to make sure it's in the form of a bread first.


_All right all right, so I get the point of schemas and validation. How do I validate my files? How do I even write a schema?_

Well first, we're going to need to write a schema to validate some kind of xml file.

### Writing a Schema

I'm going to make this _very_ simple. If you want to learn more about writing schemas check out [w3 schools' tutorial on it](http://www.w3schools.com/schema). We're going to write a schema for a person. The person element should contain only 3 simple elements; name, age, and height (in cm). See below for the schema (`Person.xsd`).

{% highlight xml %}

	<?xml version="1.0"?>

	<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

		<xs:element name="Person">
			<xs:complexType>
				<xs:sequence>
					<xs:element name="name" type="xs:string" />
					<xs:element name="age" type="xs:integer" />
					<xs:element name="height" type="xs:integer" />
				</xs:sequence>
			</xs:complexType>
		</xs:element>
	</xs:schema>

{% endhighlight %}

##### _A Quick Note on Namespaces_

If you notice in the above schema I have prefixed each element with the `xs` namespace. This is done to avoid confusion with similar elements over large xml documents that might have conflicting element names. The above document without using namespaces can similarly be written like the following: 

{% highlight xml %}

	<?xml version="1.0"?>

	<schema xmlns="http://www.w3.org/2001/XMLSchema">

		<element name="Person">
			<complexType>
				<sequence>
					<element name="name" type="string" />
					<element name="age" type="integer" />
					<element name="height" type="integer" />
				</sequence>
			</complexType>
		</element>
	</schema>

{% endhighlight %}

Whichever way you write the schemas doesn't matter. If you do have conflicting elements though, it will be beneficial to look more into XML namespaces too. That is not within the scope of this post though.

#### Writing the XML Files

I use eclipse as my IDE and I have set up my project and I have the following folder structure

	/src/xmlValidator
			/schemas
				/Person.xsd
			/xml
				/Bob.xml
				/Penelope.xml


Now that you can see my folder structure let's write some more files. You should have already created `Person.xsd` from the above snippet.

Let's Make a Person. We'll call the file `Bob.xml`

`Bob.xml` will contain the following:

{% highlight xml %}

<?xml version="1.0"?>

<Person>
	<name>Bob</name>
	<age>18</age>
	<height>150</height>
</Person>

{% endhighlight %}

Now here's `Penelope.xml`:

{% highlight xml %}

<?xml version="1.0"?>

<Person>
	<name>Penelope</name>
	<age>nineteen</age>
	<height>150.1</height>
</Person>


{% endhighlight %}


Okay, so now take a look at each of the xml files. Which one do you think is valid? not valid? Pay close attention to the types we defined in our schema.

#### Main Method to Check our Files

Now let's actually write the code to how we can validate our xml files to ensure they follow the schema. 


{% highlight java %}

import java.io.File;
import java.io.IOException;
import java.nio.file.Paths;

import javax.xml.*;
import javax.xml.transform.stream.StreamSource;
import javax.xml.validation.Schema;
import javax.xml.validation.SchemaFactory;
import javax.xml.validation.Validator;

import org.xml.sax.SAXException;

public class validator {

	public static void main(String[] args) {

		String path = System.getProperty("user.dir") + "/bin";
		String filePath = path + "/xmlValidator/xml/Bob.xml";
		String filePath2 = path + "/xmlValidator/xml/Penelope.xml";
		String schemaPath = path + "/xmlValidator/schemas/Person.xsd";

		File f = new File(filePath);
		File f2 = new File(filePath2);
		File xsd = new File(schemaPath);

		System.out.println("XML file at " + f.getName() + " is valid: " + validateXML(xsd, f) + " for schema " + xsd.getName());
		System.out.println("XML file at " + f2.getName() + " is valid: " + validateXML(xsd, f2) + " for schema " + xsd.getName());



	}


	public static boolean validateXML(File xsd, File xml){
		try{
			SchemaFactory fact = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);
			Schema s = fact.newSchema(xsd);
			Validator vdr = s.newValidator();
			vdr.validate(new StreamSource(xml));
		}catch(IOException | SAXException e){
			System.out.println(e.getMessage());
			return false;
		}
		return true;
	}
}

{% endhighlight %}

The heart of validation is done within the `validateXML()` method. It's quite simple and it also tells us the errors we receive.


The problem is that once we receive an error, it quits. We don't find _all_ of the errors which may or may not be a problem depending on the application. If you notice in the `Penelope.xml` file there are two errors, but the program should only return once it finds the first.


And that's simply it. XML Schema validation in a nutshell.










