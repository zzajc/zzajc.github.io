---
layout: post
title: "Netbeans IDE code templates"
date: 2018-03-28
tags: netbeans ide slf4j code template freemarker
author: Jiri Hanousek
---
Templating is useful when writing repetitive boiler plate code. Netbeans ide has code templating implemented by macros or code templates with subset 
of older version of freemarker language.


The documentation did not help me much but reading all these links gave me some overall undestanding. 
- [How to use code templates](http://wiki.netbeans.org/Java_EditorUsersGuide#How_to_use_Code_Templates)
- [Editor code reference](https://netbeans.org/kb/docs/java/editor-codereference.html#codetemplates)
- [Php code templates](htps://netbeans.org/kb/docs/php/code-templates.html)

The best resources for learning are existing templates. You can find them in Netbeans Options in Editor category on Code Templates panel.
This is where custom templates are registered too.

The first template I want to share is trivial. It generates println statement and places cursor between quotes.
```
//abreviation: sout
//TAB to expand and place cursor
System.out.println("${cursor}");
```

The second template I want to share is template to generate logger for slf4j. This template inserts logger as constant field in the class while
automatically using class name for name and adding import if needed.

```FreeMarker
//abbreviation: slflog
private static final ${loggerType type="org.slf4j.Logger" default="Logger" editable="false"} logger = ${loggerFact type="org.slf4j.LoggerFactory" default="LoggerFactory" editable="false"}.getLogger(${classVar editable="false" currClassName default="getClass()"}.class );
```

```Java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class ConnectionUtil {

    private static final Logger logger = LoggerFactory.getLogger(ConnectionUtil.class);
```


Finally the third code template to share. This template helps with creating javafx property with backing field. 
It is friendly to serialization using tools like gson library. This template needs user input and is less sophisticated 
than the logger template. However it presents workaround to missing feature which is capitalizing the first letter. Sadly
using macros would not help because macros lack casing procedures too.
Thay way arount this limitation is using one placeholder for the first letter and second placeholder for the rest of the
name. Templates are case sensitive. Note the case of $\{F\}. Many solve this by using underscore for backing field which
is both unfriendly to serialization and ugly.

For string property with name "environment" one just inputs "e", "E", "nvironment" and "String". This creates a lot of
boiler plate code for javafx property with backing field. The property is initialized lazily. This code is useful
if your data class should be observable and serializable but also have high performance. To manage code readability
the resulting code is wrapped into editor-fold.

```FreeMarker
//  
//<editor-fold defaultstate="collapsed" desc="${F}${name}">
    private transient ${type}Property ${f}${name}Property;
    private ${type} ${f}${name};

    public ${type} get${F}${name}() {
        return (${f}${name}Property != null) ? ${f}${name}Property.getValue() : ${f}${name};
    }

    public void set${F}${name}(${type} ${f}${name}) {
        this.${f}${name} = ${f}${name};
        if (${f}${name}Property != null) {
            ${f}${name}Property.set(${f}${name});
        }
    }

    public final ${type}Property ${f}${name}Property() {
        if (${f}${name}Property == null) {
            ${f}${name}Property = new Simple${type}Property(this, "${f}${name}", ${f}${name});
        }
        return ${f}${name}Property;
    }
//</editor-fold>    
```