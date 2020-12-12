+++
title = "Migrating Android build scripts from Groovy to Kotlin DSL"
date = "2018-12-08"
author = "Oussama Haff."
cover = "img/kotlin_dsl.jpeg"
description = "In this article I’ll be sharing with you the process I’ve followed to migrate Gradle build scripts from Groovy to Kotlin DSL in one of my Android side projects, and also my personal opinion on this process."
+++

[TOC levels=1-3]: #

# Table of Contents
- [What is Kotlin DSL ?](#what-is-kotlin-dsl-)
- [Promises made by Kotlin DSL](#promises-made-by-kotlin-dsl)
- [Why should I even care ?](#why-should-i-even-care-)
- [Migration - Step By Step](#migration---step-by-step)
  - [Chapter I - Preparation](#chapter-i---preparation)



# What is Kotlin DSL ?
If you’ve ever used SQL before or Groovy with Gradle, it means that
you’ve used a DSL language before. DSL stands for Domain Specefic
Language, which is a type of programming language that cares mainly
about the readability by enforcing the use of declarative code with
minimal boilerplate (declare to use) instead of imperative code
(explaining how to solve a problem) but as a drawback, it can be used
only inside a specific context or domain.

Kotlin is already proven to be a very powerful and a safe
production-ready programming language especially. As a GPL (General
Purpose Language) Kotlin has some great features that enables writing
very clean code with minimal syntax, things like
**[Lambdas with receivers](https://kotlinlang.org/docs/reference/lambdas.html#function-literals-with-receiver)**
that can be used
**[outside of method parentheses](https://kotlinlang.org/docs/reference/lambdas.html#passing-a-lambda-to-the-last-parameter)**
and
**[infix function notation](https://kotlinlang.org/docs/reference/functions.html#infix-notation)**
. These features permit writing DSL-like code with minimal effort (*Fré
Dumazy*’s article in ProAndroidDev
[explains this in details](https://proandroiddev.com/writing-dsls-in-kotlin-part-1-7f5d2193f277))

Jetbrains’ [Anko](https://github.com/Kotlin/anko) library brought the power of Kotlin DSL to clean verbose Android API especially XML layouts. This is one example :

```kotlin
constraintLayout {

    val sessionStart = textView {
        id = R.id.session_start
        textSize = 18f
        textColor = theme.getColor(R.attr.colorAccent)
    }

    val sessionTitle = textView {
        id = R.id.session_title
        textSize = 18f
        textColor = Color.BLACK
    }.lparams(0, wrapContent)

    textView {
        id = R.id.session_details
        textSize = 16f
    }.lparams(0, wrapContent)
    
  // more code here ..
}
```


# Promises made by Kotlin DSL
Being built on top of the core language, makes Kotlin DSL considered as
an
**[Internal DSL](https://en.wikipedia.org/wiki/Domain-specific_language#Usage_patterns)**
which means that it does not have a syntax on its own, rather being a
library that offers DSL readability by exploiting the mother GPL
language features, in this case Kotlin.

What we should end up having is :
- A statically-typed and a type-safe DSL (inherited from Kotlin)
- An enhanced IDE editing experience (inherited from
  Kotlin)
- Interoperability with existing scripts (as a JVM language)
- Maximum readability (as any DSL) Consistency and super power (by using
  the same language across the project — source code and configuration
  💯 Kotlin)


# Why should I even care ?

Shortly, because Groovy is a dynamically typed language, and because
it’s very likely that you have come across one of these problems when
dealing with build script files for Android :

- Very poor IDE assistant when writing a Groovy DSL script
- Performance issues*
- Errors at build runtime instead of compile time
- Painful build script debugging experience
- Refactoring is a pain in 🤬

>Enough said ! let’s try migrating…


# Migration - Step By Step
{{< figure src="/img/east_gif.gif" position="center"
style="border-radius: 8px;" caption="It will not be that easy !"
captionPosition="center" >}}

The migration from Groovy DSL scripts to Kotlin DSL is a like a battle,
the more you’ve prepared yourself the more you increase your chances to
win !

So, this is out strategy : Groovy DSL and Kotlin DSL have lots of
similarities, therefore we will not re-write build scripts from scratch,
in fact we will be transforming the existing Groovy scripts into Kotlin
DSL by applying minimum amount of modification, once finished, we rename
build scripts file extension and by then we will be done !

Sounds cool.. yeah.. **only, and only** if you have well prepared your build
files, because after renaming files to .gradle.kts the IDE will not be
able to offer support, the lights will go off, and you’ll be alone
against build scripts in your battle — so be prepared !

## Chapter I - Preparation
This is where you should really invest time and effort. The right (and
the latest) IDE — Make sure that you use one of these IDEs (I prefer
IntelliJ IDEA CE for Android)



1. **The right (and the latest) IDE** — Make sure that you use one of
   these IDEs (I prefer IntelliJ IDEA CE for Android)

{{< figure src="/img/compat_table.jpeg" position="center"
style="border-radius: 8px;" caption="IDE Compatibility (2018)"
captionPosition="center" >}}

2. **Update to Gradle Wrapper 5.0 or higher**

```properties
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-5.0-all.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```
> Tip : use gradle-5.0-**all** instead of gradle-5.0-**bin**. The
> “*all*” distribution contains sources that provides IDE with Gradle
> API and Groovy DSL documentation.

3. **Update all your Gradle plugins** — As they may bring support for Kotlin
   DSL if they are not already supporting it.

4. **Stop Gradle auto-sync (optional)** — in the upcoming steps you’ll
   start disambiguating build scripts, we will apply small changes
   practically in every line of the script’s source code. Having Gradle
   syncing each time can cause you high blood pressure 🥲

{{< figure src="/img/disable_auto_synch.png" position="center"
style="border-radius: 8px;"  
caption="Disabling Gradle auto-import in IntelliJ’s preferences"
captionPosition="center" >}}

5. **Fix ALL String quotes** — In Groovy it’s possible to use both single
   quotes or double quotes for strings which is not the case in Kotlin
   DSL. You’ll need to put **every-single-string** in double quotes,
   otherwise it will be very tedious to finalize migration after
   renaming files.

```groovy
proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
// becomes
proguardFiles getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro"


implementation fileTree(include: ['*.jar'], dir: 'libs')
// becomes
implementation fileTree(include: ["*.jar"], dir: "libs")


implementation 'androidx.lifecycle:lifecycle-viewmodel:2.0.0'
//becomes
implementation "androidx.lifecycle:lifecycle-viewmodel:2.0.0"
```

Do not forget the ***settings.gradle*** file
```groovy
include ':app'
// becomes
include ":app"
```

6. **Disambiguating Groovy DSL** — Without knowing the context, the
   expression of type “***xx zzzz***” in Groovy can represent a property
   assignment or a function invocation. These two cases will have
   different syntax for each in Kotlin DSL. Therefore, we need to
   distinguish them by being more explicit. Actually, the explicit
   format is the same format that will be used by Kotlin DSL !

Before Disambiguation — Implicit and not compatible with Kotlin DSL
```groovy
// This is a property assignment
applicationId "com.hfrsoussama.animotion"

// This is a function invocation
implementation "androidx.lifecycle:lifecycle-viewmodel:2.0.0"
```

After Disambiguation — Explicit and compatible with Kotlin DSL
```groovy
// This is a property assignment
applicationId = "com.hfrsoussama.animotion"

// This is a function invocation
implementation("androidx.lifecycle:lifecycle-viewmodel:2.0.0")
```


>Tip : use the IDE assistant to tell the difference, if you cmd+click on
>the word on the left a.e implementation, the IDE will open a source file
>which means that it’s a function but when you try to cmd+click on
>applicationId you will get nothing which means that it’s a property.


7. **Respect patterns** — The goal of DSL is to be as declarative as
   possible, one of the anti-patterns of that is to use `apply plugin`
   instead of the `plugins` bloc. The Kotlin DSL will need this bloc to
   generate static extension functions to make use of these plugins.
   Therefore, we need to move all apply plugin into one `plugins` bloc.

