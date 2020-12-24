+++  
title = "Hunting a bug — A True App Bundle Debugging Story"  
date = "2019-07-27"  
author = "Oussama Hafferssas"  
cover = "img/02_android_app_bundle_debug_story/cover.webp"  
description = "In this article I will focus on the problem-solving mindset behind resolving a mysterious bug that we have faced when trying to publish our Android App using the new format App Bundle instead of APK for the first time"  
+++

[![ProAndroidDev](https://img.shields.io/badge/ProAndroidDev.com-Jul%2027,%202019-blue.svg?style=flat)](https://proandroiddev.com/hunting-a-bug-a-true-app-bundle-debugging-story-b8898767f8e3)

>*Blog post edit history can be found in the website's Github repository* [*here*](https://github.com/hfrsoussama/oussamahaff_dev/commits/master/content/posts/02_android_app_bundle_debug_story.md)

As Android developers, we tend to write about best practices, trending
architectures, new libraries and so on, but we rarely find detailed
articles about bugs despite having them all the time !

Therefore, in
this article I will focus on the problem-solving mindset behind
resolving a mysterious bug that we have faced when trying to publish our
Android App using the new format App Bundle instead of APK for the first
time. I will share all the steps of the investigation, and especially
the reason behind every step.

> I have successfully reproduced the bug with the same symptoms in a
> [small demo project](https://github.com/hfrsoussama/TestingAndroid)
> which I’ll be using in this article.


[TOC levels=1-3]: #

# Table of Contents
- [What is Android App Bundle ?](#what-is-android-app-bundle-)
- [What was the problem that we have faced ?](#what-was-the-problem-that-we-have-faced-)
- [How did we drive the investigation ?](#how-did-we-drive-the-investigation-)
  - [Attempt 1 : Verifying the installation via a normal APK](#attempt-1--verifying-the-installation-via-a-normal-apk)
  - [Attempt 2 : Generating the bundle with the Android Gradle Plugin](#attempt-2--generating-the-bundle-with-the-android-gradle-plugin)
  - [Attempt 3 : Testing the generated bundle with "Bundletool“](#attempt-3--testing-the-generated-bundle-with-bundletool)
  - [Attempt 4 : Verifying the resource value](#attempt-4--verifying-the-resource-value)
  - [Attempt 5 : Verifying the merged Manifest](#attempt-5--verifying-the-merged-manifest)
- [The — Eurêka ! — moment](#the--eurêka---moment)
- [Final thoughts](#final-thoughts)


# What is Android App Bundle ?

Since the beginning of Android, we had only one format as a result of
assembling Android Apps, called APK (for Android Package). This package
can be installed directly on a device and can be also uploaded on the
Google Play Console to be delivered to the users.

Android App Bundle
(**.aab** file extension) is the new format to be used when uploading apps
to the Play Store. The Play Store will use this Bundle to generate small
signed APKs and dynamically deliver them to the users respecting their
device’s CPU architecture, screen density and even language.

> \[ [Read](https://developer.android.com/guide/app-bundle/) \] The
> official Android documentation
>
> \[ [Watch](https://www.youtube.com/watch?v=QdoEcfibG-s) \] The Android
> Dev Summit presentation


# What was the problem that we have faced ?

Just like in our real app, I
added support for App Bundle in our demo app, and I have restricted the
generation of the small APKs by device architecture and by screen
density by adding this block to our app’s build.gradle file

```groovy
android {
    // ...
    bundle {
        density {
            enableSplit true
        }
        abi {
            enableSplit true
        }
        language {
            enableSplit false
        }
    }
}
```

I’ve generated a signed release App Bundle (.aab file) with Android
Studio and I tried to upload it to the Google Play Console for internal
testing. The uploading of the file passed successfully, and then the
console started generating APKs from the bundle that I have uploaded.

Strangely, the process of generating APKs stops, and the Play Console
shows this error :

> `Upload failed Your APK cannot be analyzed using aapt. Error output:
> Failed to run aapt dump badging: W/ResourceType(163870): No known
> package when getting value for resource number 0x010c00a1
> AndroidManifest.xml:26: error: ERROR getting android:label attribute:
> attribute value reference does not exist`


# How did we drive the investigation ?

## Attempt 1 : Verifying the installation via a normal APK

Hypothesis — Maybe there were some code
added accidentally before generating the bundle.

I generated a normal
signed release APK with Android Gradle Plugin and installed it on a
device, the build passes without warning, and the app is installed and
works perfectly.

— Hypothesis dismissed ❌

## Attempt 2 : Generating the bundle with the Android Gradle Plugin

Hypothesis — Maybe it’s a bug affecting Android Studio.

It’s possible — and recommended — to generate the app bundle using the
Android Gradle Plugin via the dedicated Gradle task by running the
command `./gradlew bundleRelease`. The app build script contains the
signing key configurations. Therefore, the generated bundle will be
signed properly.

>You must have at least version 3.2.0 of AGP to be able to generate App
>Bundles

When I uploaded the new generated bundle, I had the same issue..

— Hypothesis dismissed ❌

## Attempt 3 : Testing the generated bundle with "Bundletool“

Hypothesis — Maybe it’s a bug affecting the Google Play Console !

As
explained before, when Google Play Console receives the bundle file, it
starts generating small signed APKs depending on the split type that we
have activated (in our case it’s screen density and CPU architecture).

Behind the scenes, Google Play Console uses an open source tool called
***Bundletool*** to generate and sign these deployable APKs. So, I have
downloaded the tool
([from here](https://github.com/google/bundletool/releases)), and I
started by generating signed APKs using the command :

{{< figure
src="/img/02_android_app_bundle_debug_story/generate_signed_apk.webp"
position="center" style="border-radius: 8px;"  
caption="Command written in the Terminal"  
captionPosition="center" >}}

The result of this command will be a file having ***.apks*** as extension.
Which is in fact a zip file having multiple signed APKs inside, split by
screen density and architecture.

At this level, I was convinced Bundletool is working properly because it
could generate APKs without having any error. But in order to remove any
doubt, I tried to install the app from this .apks file on a connected
device using this command :

{{< figure
src="/img/02_android_app_bundle_debug_story/install_generated_apk.webp"
position="center" style="border-radius: 8px;"  
caption="Command written in the Terminal"  
captionPosition="center" >}}

When checking the device, I found that the app was successfully
installed, and it works like a charm ! I have repeated the test for both
bundles : the one generated with Android Studio and the one generated
with Android Gradle Plugin. Conclusion : the tool works fine on my
machine.


> \[ [Read](https://developer.android.com/guide/app-bundle/) \] the official doc about Bundletool

— Hypothesis dismissed ❌

## Attempt 4 : Verifying the resource value

Hypothesis — May be the String resource used for the attribute
`android:label` in the application declaration has been deleted
accidentally.

Using Android Studio’s shortcut for *Find in path* `shift + cmd + F`
I’ve searched for all declarations that matches
`android:label=”@string/app_name”`. Knowing that each module can bring
its own application label, I’ve checked the presence of the string in
each module’s `strings.xml` file.

In our demo app, we will find two usages, one for each module (***app***
module and ***mybugsourcelib*** modules), the strings used for the
declaration are properly declared in the two modules (Defined as
`Testing Android` for ***app*** module and `MyBugSourceLib` for the second
module).

— Hypothesis dismissed ❌

## Attempt 5 : Verifying the merged Manifest

Hypothesis — May be there have been an error when merging Manifests
coming from the project libraries and modules.

When building the app, the Gradle build merges all manifest files into a
single Android Manifest file which will be packaged in to the final APK.

{{< figure
src="/img/02_android_app_bundle_debug_story/manifest_merge.webp"
position="center" style="border-radius: 8px;"  
caption=" [The process](https://developer.android.com/studio/build/manifest-merge#merge_priorities) to merge three manifest files, lowest priority (left) into highest priority (right)"  
captionPosition="center" >}}

After finishing the merge, Gradle build generates a report about the
merge operation and put it in a text file named
`manifest-merger-release-report.txt` under `app/build/outputs/logs`. I
opened this file, and I searched for `android:label` to see if I can find
some clues. I have found 02 matches.

* The first match —

{{< figure
src="/img/02_android_app_bundle_debug_story/manifest_merger_report.webp"
position="center" style="border-radius: 8px;"  
caption="Part from the `manifest-merger-release-report.txt` file"  
captionPosition="center" >}}


As we can see, the log says that the manifest merger has added an
android:label value from the main Manifest but has rejected the value
coming from the library’s Manifest. Is this normal ? well, it is.

If we
go back to verify the main `AndroidManifest.xml` we will find that there
is an attribute called `tools:replace` having the value android:label,
what does that mean ?

{{< figure
src="/img/02_android_app_bundle_debug_story/app_module_manifest_file.webp"
position="center" style="border-radius: 8px;"  
caption="The ***app*** module `AndroidManifest.xml` file"  
captionPosition="center" >}}

This is called a ***Merge Rule***, it tells the Manifest merger : when
it comes to the **application**, refuse any application `android:label`
value coming from another Manifest file, and replace it with the main
***app***’s Manifest value which is in this case the String `app_name`.


> \[[Read](https://developer.android.com/studio/build/manifest-merge)\]
> how manifest merge work in Android

So, in our case everything is OK, and there is no clue to be extracted
from this first match unfortunately.

* The second match —

{{< figure
src="/img/02_android_app_bundle_debug_story/manifest_merger_report_2.webp"
position="center" style="border-radius: 8px;"  
caption="Part from the `manifest-merger-release-report.txt` file"  
captionPosition="center" >}}

For this second match, the log says that there is an `android:label` that
has been **added** ! But this time, it’s coming from an activity called
`MyLibActivity`, an activity that has been **added from a library**.

It has
been added because the Merge Rule that we have seen in the first match
applies only on the application node not the activity node.

Well, this
is defiantly a clue ! We should verify the actual value that has been
added to the Manifest.


— Hypothesis gives a clue 🧐

# The — Eurêka ! — moment

The clue leads us to check the Manifest file that will be included in
the generated bundle which os the result of merging all Manifests. In
order to do so, I went to the **build** menu in Android Studio and
selected the option *Analyze APK...* and then selected the **.aab**
file. The good thing is that Analyzing APKs works fine with bundle
files.

{{< figure
src="/img/02_android_app_bundle_debug_story/final_merged_manifest.webp"
position="center" style="border-radius: 8px;"  
caption="APK Analyzer in Android Studio analyzing the app bundle file (.aab)"  
captionPosition="center" >}}

When everything is OK when it comes to the **application label** value, it’s
clear that the **Activity label** value is not good, as it has a *style
resource* as value instead of having a string resource !

The Activity comes from a third party library. Therefore, and in order
to make sure, I have decompiled the library, fixed the declaration and
recompiled the library (for the demo project you’ll need only to fix the
declaration). Then, I have build a new bundle with the new compiled
library, upload it on the Play Store Console.. and it passes the APKs
generation **successfully** !

# Final thoughts

As you can see, it was not trivial to find the cause of
the bug. This is the kind of situations when it’s essential to have a
problem-solving-mindset and avoid getting angry in order to do a proper
investigation.

These are my thoughts that I want to share with you :
* Error messages are not 100% correct. In our case, you may think that I
  should have checked the result `AndroidManifest.xml` since the beginning
  ! I actually did, but the aapt error message was not correct about the
  type of the error nor the line number :/
* When you add a third party
  library or SDK to your app, you are adding its features and also its
  bugs.
* The Android Asset Packaging (*aapt*) must become type-safe,
  otherwise it’s very difficult to spot and avoid such scenarios in big
  apps.
* Reading carefully the Android documentation is not easy, but it’s
  very important, so you can understand the relation between different
  actors of the app when it comes to build, delivery and runtime.
* With Android App Bundle, the Android team is doing a great job trying
  to improve the experience for both developers (No more
  [multiple APKs](https://developer.android.com/studio/build/configure-apk-splits)
  having different app versions) and Android users (downloading only
  what they need) — Thank you, Android team !
* Final thought -


{{< tweet 1128643958614167552 >}}

 *Originally published in
[ProAndroidDev](https://proandroiddev.com/hunting-a-bug-a-true-app-bundle-debugging-story-b8898767f8e3).
Thanks to Simon Percic for th review.*
