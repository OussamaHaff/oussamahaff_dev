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



# What is Kotlin DSL ?
If you’ve ever used SQL before or Groovy with Gradle, it means that
you’ve used a DSL language before. DSL stands for Domain Specefic
Language, which is a type of programming language that cares mainly
about the readability by enforcing the use of declarative code with
minimal boilerprate (declare to use) instead of imperative code
(explaining how to solve a problem) but as a drawback, it can be used
only inside a specific context or domain.

Kotlin is already proven to be a very powerful and a safe
production-ready programming language especially. As a GPL (General
Purpose Langage) Kotlin has some great features that enables writing
very clean code with minimal syntax, things like
**[Lambdas with receivers](https://kotlinlang.org/docs/reference/lambdas.html#function-literals-with-receiver)**
that can be used
**[outside of method parentheses](https://kotlinlang.org/docs/reference/lambdas.html#passing-a-lambda-to-the-last-parameter)**
and
**[infix function notation](https://kotlinlang.org/docs/reference/functions.html#infix-notation)**
. These features permit writing DSL-like code with minimal effort (*Fré
Dumazy*’s article in ProAndroidDev
[explains this in details](https://proandroiddev.com/writing-dsls-in-kotlin-part-1-7f5d2193f277))

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
Being built on top of the core langage, makes Kotlin DSL considered as
an Internal DSL which means that it does not have a syntax on its own,
rather being a library that offers DSL readability by exploiting the
mother GPL language features, in this case Kotlin.

What we should end up having is :
- A statically typed and a type-safe DSL (inherited from Kotlin)
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
