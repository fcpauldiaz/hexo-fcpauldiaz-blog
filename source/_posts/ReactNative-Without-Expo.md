---
title: React Native Without Expo
date: 2018-06-10 15:02:38
tags:
    - android
public: true
---
# Intro

I took an old react native project that I started long ago to finally finish it. I created a fresh start new react native project with `create-react-native-app`. I found out that now that Expo is the default option when creating a default app. I made some changes, integrated [NativeBase](https://docs.nativebase.io) and Redux (using some boilerplate) and I wanted to test the compilation process. I found out that now Expo can compile your code in their cloud and just download the compiled file to test it on the phone. I thought this was great until I saw the file size: 25 mb 😱 and I read that for iOS is around 30 mb. I just could not believe it. After reading some docs, Expo claims that they include multiple libraries so when a new library is needed, it is already in the user's phone and only the JS code is pushed over the cloud. I can't afford it, it is too many space for what I'm doing, so I decided I needed to detach 🤷.

# ExpoKit

There are two detach possibles: ExpoKit and React Native. So, I tested first the ExpoKit. To test it I copied the folder because you can't go back after detaching it. The command created an android and ios folder. I don't know ios, so I only tested with Android, so I created the debug APK and the size was 30mb 😱 even bigger than the Expo cloud compilation. So, after multiple optimizations that included the `minify`, `shrink`, `separateCPUbuilds`, `proGuard` and I deleted most of the dependencies that includes and only got to reduce it at 15 mb 😕. It took me two days to make this, I had a lot of errors, I had to upgrade to latest Gradle because of my java version, added some repositories and I felt disappointed.

# React Native

I didn't give up, so I created another folder and ejected the app to pure React Native. At first, I had the same problems with the Gradle, I had to updated it, clean and build it again. The Expo parts didn't worked anymore. I had a splash screen, app icon and fonts/icons with Expo that were not compatible, so that will be more work. I left the splash screen pending and fixed the fonts/icons installing [react-native-icons](https://github.com/oblador/react-native-vector-icons). After that I compiled the APK and guess what? 7.5 mb with only the `proGuard` option enabled 🔥. It can be reduced even more with `SeparteCPUBuilds`.

# Conclusion

Expo has really cool (but not necessarily) API's integrated, a Mac is not needed to compile to iOS, can update JS code without downloading app from store but the drawback of the size is too big, in my opinion.




