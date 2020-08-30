+++
date = "2020-08-29"
title = "Lessons in App Development: Start Small"
+++

Four years since [the first one]({{< ref "/blog/evensteven" >}} ""), I'm writing my second app.

This is **Part 1** in (hopefully) a series of blog posts about how I've learned to efficiently and iteratively build this app in my spare time.

As a software engineer, my goal is to increasingly master the art of efficiently and quickly building useful software and when working on projects whose future value is yet to be proven, I've been frequently rewarded for prioritizing quickly delivered B+ solutions over slow but high quality A+ solutions.

Whats the best recipe for building good apps quickly?

## Starting Small

During my time at Square we had four engineering values. In truth, two of these values I've now forgotten, but, the other two have stayed with me are **Show. Don't Tell** and **Start Small**, and I think both are excellent values to work and live by.

When you decide to build an app; you're in a [greenfield situation](https://en.wikipedia.org/wiki/Greenfield_project), and your head immediately starts to race with possibilities, ideas, open questions, and decisions to make. There are so many traps enginers can fall into that causes them to spin their wheels.... feeling busy sure, but not really making externally observable progress with each working session.

You'll find the luxury of getting to choose everything can hinder your ability to actually get the right things done, so and today I want to focus on the art of getting things done by starting small and saying no to temptation!

## Bias towards what you already know

Whats more important; learning new stuff, or shipping something?
For me, I want to prioritize anything that means I get to ship something of value.
I can afford for the insides to be subpar for a _very_ long time.
Only when the project becomes a mild success by definition (e.g. makes \$100 in revenue) will I allow myself to pause on new features and take the time try out replacing old but familiar tech with new tech that might be ultimately better for my app.

This means I want to use my existing skill set as much as possible.

I know very well (amongst other things not related to app development)

- React / Web Development
- Plain old SQL
- CSS Layouts
- VS Code
- NPM

I don't know very well

- Swift
- Kotlin
- XCode
- Cocoapods
- Redux
- User Experience / UI Best Practices

With that in mind, I went down the React Native route, especially because I knew I wasn't going to need to use any hardware APIs on the phones.

## No Backend

An app that [works fine offline](https://alistapart.com/article/offline-first/) and requires [no log-in](https://github.com/aviaryan/awesome-no-login-web-apps) is vastly underated and undervalued in my opinion. You can always move it to be online in the future, but in the meantime, since its just you, enjoy the lightness of not having to spin up, maintain, and build a backend too.

At work you're probably used to lots of great stuff like; continuously run test suites, continuous deployment, finetuned databases, consistent API design, system health monitoring, and, most importantly, someone else footing the bill for the operating costs.

Mobile Apps support SQLite databases. They aren't glamourous, but good enough for to get going for sure!

## No Secondary Features

Know what your critical path is and build that first! Everything else can come later.

Your critical path is the thing that must be up at working no matter what. Its the thing that if it didn't exist, your project wouldn't make sense any more. At a food delivery company I worked for; the critical path was that a driver should be able to sign up, go online, receive delivery requests, deliver the deliveries, sign off and then get paid. Everything else was a nice-to-have frivalous secondary concern, and we tried our best to make sure couldn't disrupt the critical path.

As you focus on building your critical path you're going to have a lot of fun, stimulating ideas for extra ablities and features , but try not to get disctracted by them. Write them down in your Notes app, and get back to the task at hand. Don't build anything else until your critical path is done!

## No Custom UI

Building pretty, consistent UI is not my strength. I don't have artistic talent, and at work I often lean on the skills of designers to build well laid-out spec that I can lean on.

Theres a LOT of freely available collections of UI components available on the internet.

## Summary

When you are just getting started, the possibilities are endless. Make a note of every idea you think of, but leave at that and stay focused!

Start small.
Seriously. Small.
