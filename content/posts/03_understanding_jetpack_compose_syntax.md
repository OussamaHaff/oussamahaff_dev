+++ title = "Helping you understand the syntax of Jetpack Compose"  
date = "2021-01-02"  
author = "Oussama Hafferssas"  
cover ="/img/00_the_blog_s_readme/cover_site_readme.svg"  
description = "In this article, I’ll try to demessify the syntax of Jetpack Compose by using pure Kotlin and what you already know about OOP."  
+++


>*[Edit history](https://github.com/hfrsoussama/oussamahaff_dev/commits/master/content/posts/00_the_blog_s_readme.md) of this post can be found in the blog's Github repository.*


>[*Comment*](https://github.com/hfrsoussama/oussamahaff_dev/issues/new/choose) *using Github issues to avoid cross-site trackers.*



I believe that if you have opened this article you already know that
Jetpack Compose is a new Kotlin framework which enables you to build UIs
for Android applications, and also for Kotlin desktop applications.

However, Jetpack Compose brings a syntax that may look strange for some
of us - developers - especially those doing a lot of Object-Oriented
Programming.

Therefore, in this article I’ll try to demessify the syntax of Jetpack
Compose by using pure Kotlin and what you already know about OOP.



# Basic Jetpack Compose syntax

# Demessifying Compose' syntax

Let's say we want to print the result of the multiplication of two
floats. By using the classical approach of Object-Oriented Programming,
we can write something like this in Kotlin :

> *You can run and edit the source code to test it*

{{< playground embeded_link="https://pl.kotl.in/Jo5cBRoEk?from=1&to=25&readOnly=false&theme=darcula" embeded_height="55"/>}}

This Kotlin code could be compressed of course, but we will keep it as
explicit as it could be for our example.

The three lines of code in the `main()` function represent the usual
three basic steps that we generally use in Object-Oriented Programming
- Instantiating an object
- Tell the object to do the work by invoking one if its methods
- Present the result to the user


Now, let's find out a way to implement the same functionality but by
writing the code differently.

## Getting to top-level
- First, we will add a function that multiplies two parameters. But we
  will place it outside any class.
- Instead of giving it a verb as a name like `multiply`, we will give it
  a ***noun*** as a name like `multiplication`.

{{< playground
embeded_link="https://pl.kotl.in/eoAb7djBJ?from=1&to=19&readOnly=false&theme=darcula"
embeded_height="50" />}}

By putting a function outside any class or interface in Kotlin, we are
making it a ***top-level function***. We can understand why it's called
***top-level*** just by rotating the editor's canvas.

{{< figure
src="/img/03_understanding_jetpack_compose_syntax/top_level.jpg"
position="center" style="border-radius: 8px;"  
caption="Why it's called ***top-level*** function"  
captionPosition="center" >}}


- Let's do the same for the function that prints the result. We will
  declare a new one, on the **top-level** and with a **noun name**. Call
  it `PrettyPrinter`.

{{< playground
embeded_link="https://pl.kotl.in/90jJR36l9?from=1&to=22&readOnly=false&theme=darcula"
embeded_height="50" />}}

> *You can run the program to verify that we still have the same result*
> *that will print to the user*.





> [*Comment*](https://github.com/hfrsoussama/oussamahaff_dev/issues/new/choose) *using Github issues to avoid cross-site trackers.*

https://blog.jetbrains.com/kotlin/2015/06/improving-java-interop-top-level-functions-and-properties/