+++ title = "Helping you understand the syntax of Jetpack Compose"  
date = "2021-01-02"  
author = "Oussama Hafferssas"  
cover ="/img/00_the_blog_s_readme/cover_site_readme.svg"  
description = "In this article, I’ll try to demessify the syntax of Jetpack Compose by using pure Kotlin and what you already know about OOP."  
+++


>*[Edit history](https://github.com/hfrsoussama/oussamahaff_dev/commits/master/content/posts/00_the_blog_s_readme.md) of this post can be found in the blog's Github repository.*


>[*Comment*](https://github.com/hfrsoussama/oussamahaff_dev/issues/new/choose) *using Github issues to avoid cross-site trackers.*



I believe that if you reading this article you already know that Jetpack
Compose is a new Kotlin framework which enables you to build UIs for
Android applications, and also for Kotlin desktop applications.

However, Jetpack Compose brings a syntax that may look strange for some
of us - developers - especially those doing mainly Object-Oriented
Programming.

Therefore, in this article I’ll try to demessify the syntax of Jetpack
Compose by using basic and pure Kotlin, and what you already know about
OOP. Along the path, you may get impressed also by the simplicity that
hides behind some of the fancy words used to explain Jetpack Compose.



# Basic Jetpack Compose syntax

# Demessifying Compose' syntax

## Basic OOP
Let's say we want to print the result of the multiplication of two
floats. By using the classical approach of Object-Oriented Programming,
we can write something like this in Kotlin :

> *You can **run** the source code to test it*

{{< playground embeded_link="https://pl.kotl.in/Jo5cBRoEk?from=1&to=25&readOnly=false&theme=darcula" embeded_height="55"/>}}

> *This Kotlin code can get more compact, but we will keep it as*
> ***explicit** as it could be for the sake of explication.*

The three lines of code in the `main()` function represent the usual
three basic steps that we generally use in Object-Oriented Programming
- Instantiating an object
- Tell the object to do the work by invoking one if its methods
- Present the result to the user


Now, let's find out a way to implement the same functionality but by
writing the code differently.

## Getting to Top-Level !
- First, we will add a function that multiplies two parameters. But we
  will place it outside of any class.
- Instead of giving it a verb as a name like `multiply`, we will give it
  a ***noun*** as a name like `multiplication`.

{{< playground
embeded_link="https://pl.kotl.in/eoAb7djBJ?from=1&to=19&readOnly=false&theme=darcula"
embeded_height="50" />}}

By putting a function outside of any class or interface in Kotlin, we
are making it a ***top-level function***. We can understand why it's
called ***top-level*** just by rotating the editor's canvas.

{{< figure
src="/img/03_understanding_jetpack_compose_syntax/top_level.jpg"
position="center" style="border-radius: 8px;"  
caption="Why it's called ***top-level*** function"  
captionPosition="center" >}}



Let's do the same for the function that prints the result.
- Declare a new printing function as a **top-level** function
- Give it a **noun name**. Let's call it `prettyPrinter`.

Our code will look like the part in `// The new style`

{{< playground
embeded_link="https://pl.kotl.in/90jJR36l9?from=1&to=22&readOnly=false&theme=darcula"
embeded_height="50" />}}

> *You can run the program to verify that we still have the same result*
> *that will print to the user*.


## Changing your point of view !

Let's say that we don't want to use an intermediate variable to store
the result of multiplication. In our example, this means deleting the
use of the variable `multiplicationResult`.  
There are **two ways** to do it.

**The first**, which is classical OOP and you may already guessed it, is by
deleting it from `prettyPrinter` function and replacing it by a call
(invokation) of the function `multiplicationResult` inside of the
*String* message like this


{{< playground
embeded_link="https://pl.kotl.in/iXGv60DsV?from=1&to=22&readOnly=false&theme=darcula"
embeded_height="50" />}}

It's true that the program will give the same result, but unfortunately
it has nothing to with Jetpack Compose syntax that we are looking for.

Note also that the function `prettyPrinter` now ***prints exclusively***
the result of the function `multiplication` and not any `Float` given to
it. Which makes the function `prettyPrinter` not reusable at all !

Let's try **the second** way of doing things.



By going back to our code we had this :

```kotlin
    // The new style
    val multiplicationResult = multiplication(paramY = 4f, paramX = 5f)
    prettyPrinter(result = multiplicationResult)
```

Now, before deleting the variable `multiplicationResult`, we will play
with its type.

The actual type `multiplicationResult` is `Float`, which is the type
returned by the function `multiplication`. Therefore, we can write this
:
```kotlin
    // The new style
    val multiplicationResult: Float = multiplication(paramY = 4f, paramX = 5f)
    prettyPrinter(result = multiplicationResult)
```


Please **look** at `multiplicationResult` declaration.

The declaration means that you represent **the result of the
invokation** of the function `multiplication` by a variable.

What about the **representation of the function** by itself ? Is there a
way to declare it ?

Fortunately, the answer is yes, and this is how it looks like :
```kotlin
    // The new style
    val operation: ()-> Float = { multiplication(paramY = 4f, paramX = 5f) }
```
You can read it as follows :
- On the left side : the variable `operation` is of type `()-> Float`
  which means that it's of type ***function that does something and
  return Float***.
- On the tight side : ***what this function does*** is represented by what's
  inside `{ }`. In our case, it does multiplication and returns a Float.

Now, lets refactor our code to use this new function representation.


## Going Higher-Order !

Our function `prettyPrint` was a simple function accepting a `Float` as
a parameter like this

```kotlin
fun prettyPrinter(result: Float) {
    println("The result is : $result")
}
```
In order to pass the new representation of our `multiplication` function
as parameter, we need to refactor our function like this :
```kotlin
fun prettyPrinter(operationAsParam: ()-> Float) {
    println("The result is : $operationAsParam")
}
```
Note that :
- We changed the name of the parameter to `operationAsParam` just for clarification
- The type of the parameter has changed from `Float` to `()-> Float`

Our program will look like this after refactoring :

{{< playground
embeded_link="https://pl.kotl.in/TOEJP1D30?from=1&to=22&readOnly=false&theme=darcula"
embeded_height="50" />}}

By doing this refactoring, a fancy expression can be used to describe
the function `prettyPrinter` which is : ***`prettyPrinter` is
Higher-Order Function***. Which just means that it accepts a
representation of a function as a parameter (or simply : it accepts a
function as a parameter)



//TODO

However, the goal is not to show you one of Kotlin features. Instead, we
will use this feature to


## The one last thing to do !
After the last refactoring you may

correct program trainling lmbda

comparaison





> [*Comment*](https://github.com/hfrsoussama/oussamahaff_dev/issues/new/choose) *using Github issues to avoid cross-site trackers.*

https://blog.jetbrains.com/kotlin/2015/06/improving-java-interop-top-level-functions-and-properties/
