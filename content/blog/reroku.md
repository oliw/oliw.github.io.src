+++
date = "2016-03-01T08:48:57-07:00"
title = "Reroku (Heroku on a Raspberry Pi)"
+++

My team recently switched our applications from Heroku to barebones AWS. The experience made me truely appreciate just how effective Heroku is and how it has made deploying apps as painfree as could be. After all, what good is making an app if nobody can see it!

I've also been a huge fan of my Raspberry Pi. Every time I look at it, I feel challenged to think of a cool use for it.

So with that in mind, I decided to take a shot of making a proof-of-concept of a Heroku-like service that can run on a raspberry pi. It was a great exercise in Unix scripting and gluing a few things together. Plus I got to learn more about the innards of `git` and what happens when you do a `git push`!

Kudos to the Dokku project which heavily inspired me.

Its a proof-of-concept but I'm reluctant to invest time making a stable version since I think something like docker+kubernetes might be just around the corner for the Pi and I'd rather invest time figuring out how to make an accessible interface for that!

Check it out on [github](https://github.com/oliw/reroku-poc)!
