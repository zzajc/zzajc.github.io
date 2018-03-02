---
layout: post
title: "How I used groovy to count java line of code"
date: 2018-03-02
tags: java groovy LOC
author: Jiri Hanousek
---
LOC is a code metric which helps me to estimate how large the project is. 
It does not say anything about how much the code does but at least gives some sense of project size.

I used groovy for this task because it makes some operations simpler like using regexp or reading files.

The code does following:

1. The code finds all java files in *src* subfolder and for each file it
2. Removes all line and block comments and javadoc
3. Removes all imports
4. Reduces all whitespaces to single spaces
5. Removes empty braces (like new String[]{})
6. Counts all semicolons ';' opening braces '{' and closing braces '}' which represent a line of code
7. Counts all words

This catches all if, while, for, and try blocks. It does not matter how much whitespace and comments are present. All formatting is removed.

As you can guess the code is not perfect. It assumes standard verbose formatting with all possible braces. I use standard java code formatting
and always reformat all code in all projects with Netbeans code formatting. I force all developers in all projects I am involved to use this style.
This helps to make the code readable and more developer style independent. I got addicted to this while reading tons of JDK and Netbeans IDE/Platform source code.


*Note that as Java developer I am used to write semicolons and other unnecessary stuff that pure groovy programmer can cause their eyes to bleed, sorry for that.*

Running the code against Netbeans source code default branch (2017-10-17) gives:

rows: 5 662 413 

words 18 302 123


```groovy
//call by
countLines("D:/WORK/ProjectX")

//slf4j was used for logging

String countLines(src) {
    def javaFiles = new FileNameByRegexFinder().getFileNames(src, /.*\\src\\.*\.java$/)

    int semiCols = 0;
    int openings = 0;
    int closings = 0;
    int words = 0;
    javaFiles.eachWithIndex{item, index->
        log.info(item)
        println(item);
        String fileTestRaw = new File(item).getText();
        int size = fileTestRaw.length();
        //remove line comments
        fileTestRaw = fileTestRaw.replaceAll(/^\\\\.*$/, "");
        log.info("removed ${size-fileTestRaw.length()} characters with line comments");

        //remove imports
        size = fileTestRaw.length();
        fileTestRaw = fileTestRaw.replaceAll(/import.+;/,"");
        log.info("Removed ${size-fileTestRaw.length()} import characters");
    
        //reduce all spaces to spaces
        size = fileTestRaw.length();
        fileTestRaw = fileTestRaw.replaceAll(/\s+/, " ");
        log.info("removed ${size-fileTestRaw.length()} whitespace characters");

        //remove block comments
        size = fileTestRaw.length();
        fileTestRaw = fileTestRaw.replaceAll(/\/\*.*?\*\//, "");
        log.info("removed ${size-fileTestRaw.length()} block comment characters")
    
        //find characters that are line of code
        //semicolon denotes end of line expression
        size = fileTestRaw.length();
        def semicolons = size - fileTestRaw.replaceAll(";", "").length();
        log.info("Found semicolons: ${semicolons}");
    
        //remove empty curly braces from new String[]{};
        //count class, method, closure opening and closings
        def nonEmptyBrackects = fileTestRaw.replaceAll(/\{ \}|\{\}/,"");
        int noneSize = nonEmptyBrackects.length();
        def blockOpen = noneSize - nonEmptyBrackects.replaceAll(/\}/,"").length();
        def blockClose = noneSize - nonEmptyBrackects.replaceAll(/\{/,"").length();
    
        log.info("Found opening ${blockOpen} curly braces (nonempty) that is ${blockOpen} blocks like method, class, closure, for, if ")
        log.info("Found closing ${blockClose} curly braces (nonempty)")
        semiCols += semicolons;
        openings += blockOpen;
        closings += blockClose;
    
        def wordsOnly = fileTestRaw.replaceAll(/[-\d\(\[\{\)\]\};=\"]/,"");
        wordsOnly = wordsOnly.replaceAll(/\.|,|\\|\//," ");
        wordsOnly = wordsOnly.replaceAll(/\s+/," ");
        wordsOnly = wordsOnly.replaceAll(/\s\W+\s/,"");
        words += wordsOnly.length() - wordsOnly.trim().replaceAll(/\s/, "").length();
    }

    return "${src} contains in total:\nrows: ${ semiCols + openings + closings} \nwords ${words}";
}
```

