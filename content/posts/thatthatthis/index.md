+++
date = "2021-01-01T20:30:00-07:00"
draft = false
title = "How I Built the ThatThatThis App"
summary = "An overview of the things I used to make my recently shipped app"
categories = [ "Coding" ]
+++

[ThatThatThis](<(https://apps.apple.com/us/app/thatthatthis-decision-maker/id1529817354)>) is a small, cheap and useful indie app for methodical decision making. It’s designed to help you wrestle with big questions like “Where shall I live next?”.

I built the app in 25 sit-down sessions outside of work over a 170 day period. The first commit was on July 14th 2020, and the last commit was on December 31st 2020.

I used the [Style Tile Framework](http://styletil.es/) for coming up with the colour scheme, fonts and general look and feel of the app.

I used community-provided [Figma](https://figma.com) templates for creating appropriately sized assets for the App such as [the app icon](https://www.figma.com/resources/assets/ios-app-icon-template/) , the splash screen, and [the App Store Screenshots](https://www.figmaresources.com/resources/bBuBczuTwXf7xR4qY). Fonts were provided by Google Fonts. I used [Lottie](https://lottiefiles.com/) for some small animations.

I used [React Native](https://reactnative.dev/) to make this app, and relied heavily on [React Native Elements](https://reactnativeelements.com/) to build a minimal viable product, before swapping out most of the pre-provided elements for my custom styled ones.

Data is saved locally on the device using SQLite3, and Redux is used as state management layer.

I used [the Expo toolset](https://expo.io/) to make this app and wrote the source code in [Typescript](https://www.typescriptlang.org/). [Prettier](https://prettier.io/) and [ESLint](https://eslint.org/) were used to keep my source code consistently formatted. [Transporter](https://apps.apple.com/us/app/transporter/id1450874784?mt=12) was used to deliver my builds to the Apple App Store.

Special thanks must go to my friend [Ting Chen](https://www.linkedin.com/in/tingc10) for our biweekly makers accountability checkins that kept this side project moving forward.

[Try it out](https://apps.apple.com/us/app/thatthatthis-decision-maker/id1529817354) and let me know what you think!

{{< figure src="screenshot.png" >}}
