+++
title = "Migrating Android build scripts from Groovy to Kotlin DSL"
date = "2018-12-08"  
author = "Oussama Hafferssas"
cover = "img/kotlin_groovy_dsl/kotlin_dsl.webp"
description = "In this article I’ll be sharing with you the process I’ve followed to migrate Gradle build scripts from Groovy to Kotlin DSL in one of my Android side projects, and also my personal opinion on this process."
+++

[![AndroidWeekly](https://img.shields.io/badge/Android%20Weekly-%23339-blue.svg?style=flat)](https://androidweekly.net/issues/issue-339)

[![ProAndroidDev](https://img.shields.io/badge/ProAndroidDev.com-Dec%2008,%202018-blue.svg?style=flat)](https://proandroiddev.com/migrating-android-build-scripts-from-groovy-to-kotlin-dsl-f8db79dd6737)

>*Blog post [edit history](https://github.com/hfrsoussama/oussamahaff_dev/commits/master/content/posts/01_groovy_to_kotlin_dsl_2018.md) can be found in the website's Github repository.*

>[*Comment*](https://github.com/hfrsoussama/oussamahaff_dev/issues/new/choose) *using Github issues to avoid cross-site trackers.*


[TOC levels=1-3]: #

# Table of Contents
- [What is Kotlin DSL ?](#what-is-kotlin-dsl-)
- [Promises made by Kotlin DSL](#promises-made-by-kotlin-dsl)
- [Why should I even care ?](#why-should-i-even-care-)
- [Migration - Step By Step](#migration---step-by-step)
  - [Chapter I - Preparation](#chapter-i---preparation)
  - [Chapter II - Conversion](#chapter-ii---conversion)
- [Evaluating promises](#evaluating-promises)
- [Personal opinion](#personal-opinion)



# What is Kotlin DSL ?
If you’ve ever used SQL before or Groovy with Gradle, it means that
you’ve used a DSL language before. DSL stands for Domain Specific
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

{{< figure src="/img/kotlin_groovy_dsl/compat_table.webp" position="center"
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
> **Tip** : use gradle-5.0-**all** instead of gradle-5.0-**bin**. The
> “*all*” distribution contains sources that provides IDE with Gradle
> API and Groovy DSL documentation.

3. **Update all your Gradle plugins** — As they may bring support for Kotlin
   DSL if they are not already supporting it.

4. **Stop Gradle auto-sync (optional)** — in the upcoming steps you’ll
   start disambiguating build scripts, we will apply small changes
   practically in every line of the script’s source code. Having Gradle
   syncing each time can cause you high blood pressure 🥲

{{< figure src="/img/kotlin_groovy_dsl/disable_auto_synch.webp" position="center"
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


>**Tip** : use the IDE assistant to tell the difference, if you cmd+click on
>the word on the left a.e implementation, the IDE will open a source file
>which means that it’s a function but when you try to cmd+click on
>applicationId you will get nothing which means that it’s a property.


7. **Respect patterns** — The goal of DSL is to be as declarative as
   possible, one of the anti-patterns of that is to use `apply plugin`
   instead of the `plugins` bloc. The Kotlin DSL will need this bloc to
   generate static extension functions to make use of these plugins.
   Therefore, we need to move all apply plugin into one `plugins` bloc.


## Chapter II - Conversion
Now we will proceed to the conversion of existing build scripts into
Kotlin DSL by renaming them from `xx.gradle` to `xx.gradle.kts`

>Mixing Groovy scripts and Kotlin DSL scripts in the same project is
>possible 👌

1. It’s better to start by renaming `settings.gradle` files to
   `settings.gradle.kts` and re-syncing the project just after (Do this
   for all of your build settings files) Your IDE should show this :

{{< figure src="/img/kotlin_groovy_dsl/kotlin_dsl_settings.webp" position="center"
style="border-radius: 8px;"  
caption="IDE hints in Kotlin DSL 🙌"
captionPosition="center" >}}


>Because we have already prepared the `settings.gradle` file we did not
>need to do anything to make it compile after converting it to Kotlin DSL
>!

2. Now let’s do the same thing for the top level `build.gradle` file and
   add the extension `.kts` — Try to spot the differences 🔍

{{< figure src="/img/kotlin_groovy_dsl/groovy_build_classic.webp" position="center"
style="border-radius: 8px;"  
caption="IDE view of a standard Gradle Groovy root build script file before conversion"
captionPosition="center" >}}


{{< figure src="/img/kotlin_groovy_dsl/gradl_kotlin_dsl_build.webp" position="center"
style="border-radius: 8px;"  
caption="The IDE view of the script after converting it to Kotlin DSL"
captionPosition="center" >}}

As you can see in terms of syntax, in this fileclean_me_im_ the only
difference between the Groovy DSL and Kotlin DSL version is the
declaration of `cleanMeImNasty` task which you can read as :
`cleanMeImNasty` ***is registered as a Delete task that deletes the
buildDir when executed***.

In terms of IDE assistant, we have way better assistant. The IDE shows
hints for all scopes of the script 👌 It provides also more assistant
when writing the actual Kotlin DSL script. Let’s see..

{{< figure src="/img/kotlin_groovy_dsl/clean_me_im_nasty.gif" position="center"
style="border-radius: 8px;"  
caption="IDE assistant with Kotlin DSL"
captionPosition="center" >}}

>**Tip** : take a look at this [Gradle sample](https://github.com/gradle/kotlin-dsl-samples/blob/master/samples/task-dependencies/build.gradle.kts) for an in depth look.

We still need to convert the last file which is in our case the app
module’s build.gradle file. The screenshot bellow is what the IDE shows
just after renaming the file to `build.gradle.kts`

{{< figure src="/img/kotlin_groovy_dsl/end_with_errors_build_gradle_kts.webp"
position="center" style="border-radius: 8px;"  
caption="IDE assistant with Kotlin DSL" captionPosition="center" >}}

As you can see, after syncing the project there are parts that have been
compiled properly and others not. ⚠️ Sometimes the IDE indicated
visually a lot of errors whereas Gradle indicates much less in the
sync logs. **Always use the log errors**

>**Tip** : The best thing to do is to look to the sync error logs and fix
>them one by one. To make sure that your fix is good, run the command
>`./gradlew tasks`


In our case we have mainly three errors in the log :

- The `release` build type error — under the hood, release is just a
  string passed to a function invoked by Groovy therefore Kotlin will not
  be able to invoke it as a function. We need to use the actual function
  invoked behind which is `getByName(String)`
- The `minifyEnabled` error — minifyEnabled is not the actual name of the
  property therefore Kotlin can’t find it. We need to use its original
  name for assignment which is `isMinifyEnabled`.
- The `fileTree` error — before trying to fix it, it’s better to take a
  look at its declaration (This is why we imported **all** sources and
  docs of Gradle 😉 ). `fileTree(Map<String, ?> args)` The method
  accepts args of map of string keys. We will pass this as **Tuples**
  the Kotlin way.

The result after applying these fixes will look like this..

{{< figure src="/img/kotlin_groovy_dsl/end_without_errors_build_gradle_kts.webp" position="center"
style="border-radius: 8px;"  
caption="An app module build script file in Kotlin DSL that actually compiles 🎉"
captionPosition="center" >}}

If your build scripts still can’t compile, please keep running `./gradlew`
tasks command and fix issues indicated in the log, and double check if
your Gradle plugins are fully compatible with Kotlin DSL. If it compiles
then..

{{< figure src="/img/kotlin_groovy_dsl/congrats.gif" position="center"
style="border-radius: 8px;"  
caption="Congratulations 🎉"  
captionPosition="center" >}}

> ***Note on organizing Gradle project dependency wise*** — In the case of
> Android, it was recommended to use Gradle extra properties to store
> versions of dependencies in order to use them across the project
> modules. This solution has been crippled with the lack of IDE
> assistance do navigate to these versions declaration and properly
> refactor them.
> [Gradle now recommends declaring them](https://docs.gradle.org/current/userguide/organizing_gradle_projects.html#sec:build_sources)
> in a dedicated folder `buildSrc` that contains imperative build code
> to be shared between modules. *Sam Edwards*
> [explained it here](https://caster.io/lessons/gradle-dependency-management-using-kotlin-and-buildsrc-for-buildgradle-autocomplete-in-android-studio).


# Evaluating promises
- ✅  —  **A Type-safe DSL** — Because it’s statically typed, Kotlin DSL
  provides more safety on passed parameters when invoking Gradle build
  functions.
- ✅  —  **An enhanced IDE editing experience** — In terms of IDE
  assistance, it’s much more pleasant to write build scripts in Kotlin
  DSL
- ⚠️   — ***Interoperability with existing scripts*** — because both are JVM
  languages (and because Jetbrains has done a great job) Kotlin DSL
  scripts are compatible with Groovy but **not fully** because of
  Groovy’s dynamic types. Some Gradle plugins will need to be modified
  in order to be used in Kotlin DSL
- ✅  —  **Maximum readability** — As any DSL, I think that Kotlin DSL is very
  readable even if it’s semi-declarative.
- ✅  —  **Consistency and super power** — Having a declarative type-safe
  language for build is very promising. Gradle plugins authors will be
  10 times more creative and productive using such a language. For the
  years to come, I think we will see a lot of 100% Kotlin Android
  projects.

# Personal opinion
I’ve always believed in the proverb : *you’ve got to crack a few eggs to
make an omelette*, but for this kind of migrations it would be better to
have some kind of assistant to indicate to developers at least where
they probably should modify script files in order to make them Kotlin
DSL-conversion-ready.

Performance issues — for now (2018), Kotlin DSL falls short in terms of
build speed because of compile time needed to resolve that extra type
safety. It’s indicated clearly
[here](https://github.com/gradle/kotlin-dsl-samples/issues/902) , and I
noticed it even in my small project. I may benchmark it on a big test
project before using it in a big production project.

If Null Pointer Exception is the Billion dollar mistake, build waiting
time is easily the Trillion Dollar mistake ! Before even thinking about
a migration to Kotlin DSL, it’s mandatory to be already **applying**
build best practices otherwise it will be very tedious.

I agree with [*Christina Lee*](https://twitter.com/RunChristinaRun) when she said,
[strings are danger](https://www.youtube.com/watch?v=-lVVfxsRjcY&feature=youtu.be&t=1211)
. I think it’s time to find a solution for those horrible strings that
represent dependencies and buildTypes (release, debug..). Why not making
a Gradle plugin that gathers all maven dependencies by package name and
exposes them as type-safe Kotlin constants for build scripts !?

Thanks
for the Gradle team and for the community for trying always to make
build smoother, safer and healthier 🙏

*Originally published in
[ProAndroidDev](https://proandroiddev.com/migrating-android-build-scripts-from-groovy-to-kotlin-dsl-f8db79dd6737).
Thanks to Simon Percic for th review.*
