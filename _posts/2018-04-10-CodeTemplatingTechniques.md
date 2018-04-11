---
layout: post
title: "Code templating techniques"
date: 2018-04-10
tags: groovy code template regexp generate programming
author: Jiri Hanousek
---
When writing a lot of boiler plate code I eventually come up with some generative method to create code. For this task 
I use several techniques with several tools. In this post I am going to briefly describe methods that I use
when programming on windows. 

# COPY-PASTE
This is the simplest but the most dangerous method. If you forget to replace something in pasted code than it may be valid code
and the compiler won't help you with finding stupid errors. I made many of these mistakes when I was tired.
Copies are very similar and one simply overlooks that some varible was not changed. It is also very tedious. 
Do not do this except for instances when the compiler can help you (e.g. variable already exists error).

# Regexp replace
Regular expressions are useful when generating code from some text or even from code. This technique can be used when the
code will be generated only once. It is very agile but sometimes tedious method but it is available in every code editor.
It can be used to generate code from html tables, xml, file lists.

The process is iterative and consists of several steps
- Isolate useful information 
  - Remove all unnecessary text
  - replace text with empty
  - capture group and replace row with the group
  - replace from start or from end of a row
- Organize
  - Place every entity on separate row 
  - Replace multiple whitespaces with single whitespace
- Decorate = Make the information to be part of repetitive statement
  - put constant text like "public static void "
  - surround with parentheses like characters () [] \{\} " " ' ' 

# IDE templating capabilities
This method is the fastest available because you don't need to leave the editor and you don't need to even go to any menu.
Many editors have macro language which can facilitate this. I never used macro languages because every editor has different
language and different method of giving the macro code inputs. For me it is too difficult to learn these methods.

Advantages include 
- persitence - define once, use many times
- speed - assing keyboard shortcut
- the tool is already in your hand

Disadvantages
- limited capabilities
- learning curve
- not standard

[Netbeans IDE offers code templates]({% post_url 2018-03-28-NetbeansIDECodeTemplates %}). Which is slightly similar to macros.
I love that I only write some letters and then TAB into results.

# Scripting languages
Groovy, Python, Ruby, Perl, Java, ..., any language can be useful for this technique.
Why not use the most powerful tool of programmer which is programming language? Well, there are some obstacles which make
this technique slower or less reachable. 

Sometimes you do not want to:
- think
- leave the editor window
- leave the editor
- install somethink
- run external command
- start command line
- create files
- read output from file

You always should use the most proper tool for the task. For simpler or one time operations use simpler techniques described
before. 

Scripting (programming) is needed for:
- complex generative logic
- string mutating operations
- downloading from internet
- repeatability
- reproduceability
- reading files
- parsing files
- generating without input metadata
- generting multiple files
- automating code generation

I use several approaches for fast code writing and execution.
- Java - know most apis, have IDE support, can use existing codebase for code generating, do not leave the IDE
  - JUnit
  - Main class
- Groovy
  - script file executed from commandline
  - ide or other tool which has groovy embedded
  - copy command output and paste code or write to file
- Perl
  - best for advanced Regexp method
  

