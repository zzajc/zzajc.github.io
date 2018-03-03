---
layout: post
title: "Kotlin tricks for java programmers Runnable by closure"
date: 2018-03-03
tags: kotlin java
author: Jiri Hanousek
---

When you begin learning a new programming language you are always strongly influenced by languages you already know
and transitioning yout mindset to idiomatic use of new language takes some time. I prefer doing things dirty-style
so I am not frozen too long on one thing. Moving forward keeps me motivated and I can always return and clean the filth.


The first example is implementation of runnable to be used e.g. in thread.

In java you can write it this way
```java
new Thread(()-> {someMethod()}).start()
```

In simple cases nearly the same can be applied in kotlin.

```kotlin
Platform.runLater({
    tabPane.tabs.add(Tab("Tab"))
})
```

This will fail if you want to store the runnable in variable. The inference mechanism fails to infer
Runnable because adding tab into JavaFx TabPane has a return value of type Boolean (instead of void)

```
var loader: Runnable

//this fails
loader = {
    tabPane.tabs.add(Tab("Tab"))
}
```

A simple solution is to define function which will wrap the closure to runnable.

```kotlin
fun runnable(f: () -> Unit): Runnable = Runnable { f() }

//use the function
loader = runnable({tabPane.tabs.add(Tab("Tab"))})
```


