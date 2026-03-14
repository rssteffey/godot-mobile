---
layout: post
title:  "Hello iOS World"
date:   2026-03-11 9:45:29 -0500
categories: godot
---

## Let's make this thing port to iOS.

Today's goal: make a Godot app show up on my iPhone.

I read through a "Switching from Unity to Godot" guide a few months ago and remember the gist of things (Nodes are king here. Node to node, dust to dust, this is the way)

That said - none of that actual game logic matters right now; We just need a grey screen and that beautiful Godot robot.
I'm blindly going full steam ahead with these iOS build docs, so we're gonna be throwing a lot at the wall and seeing what sticks.

#### First spaghetti to be flung: Project Settings:
- Display > Window > `480` x `720` viewport
- stretch mode = `canvas_items` (based on the advice of a random blog post)
- aspect `keep` 
- orientation `portrait` (because that sounds right)

Easy peasy, just want something visible now

#### Second Noodle: Visuals
I threw a RichTextLabel and Sprite in the viewport and confirmed in the editor player that I can see output. Good enough for me.

![Image](/assets/img/hello_world/game_output.png){: .smaller}

This is a breeze! Nothing could possibly slow us down now that we're on the last step!

#### The Last Step: Export the Rest of the Owl 

I'll be using the official Godot docs for [Exporting for iOS](https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_ios.html)

Setup is an instant reminder of why my last laptop's drive was never big enough. Godot wants to install export templates, iOS of course requires XCode, XCode requires the iOS SDK, etc etc.  
And to think I was salty about adding a Jekyll gem this morning to set up this blog.

I get everything installed, and click 'export' within Godot.

`Failed to run xcodebuild with code 0`

The Godot logs are helpful, and report that I've failed to set up my account in XCode properly.  My fault for trying to speedrun the Godot side without ensuring I can do a standard iPhone app on this laptop yet.  
Let's go look in XCode for a bit

## Apple Developer Accounts

I don't have this blog set up for footnotes and things yet, but if I did there would be a massive saga down there about how annoying Apple makes the experience for a non-paid dev. The docs seem purposely obtuse about terms, constantly try to redirect you back to registering for the $99 yearly developer fee, and the UIs and links are frequently broken or 500's if you aren't signed into to a "real" dev organization.

Anyways I get my account added to XCode, and aside from some nonsense where my name shows up as "bob" from when I first signed up 15 years ago in high school - it seems to be working.

#### Team ID

Unfortunately - in keeping with the side rant above - I'm on a personal developer account, which means finding the team ID to put in Godot's exporter is a PITA.

Thanks to [this blog post](https://community.gamedev.tv/t/getting-a-team-id-without-a-paid-membership/239118) for the solution.

`XCode Accounts` > `Personal Team` > `Manage Certificates` > `Add`

Once you've added a new certificate, open up Keychain Access and find the developer certificate you just made.  Right click and get info. The alphanumeric string in `Organizational Unit` is what we need.  Copy that to Godot's export `App Store Team ID` and we're in business!

#### Simulator Bad; Hardware Good

And by "in business" I of course mean another error.  But it's a _new_ error, and that is forward progress in software!

`Your team has no devices from which to generate a provisioning profile`

k k k k k, more Apple developer setup.  Got it.

Here is the big one. This is the roadblock that ensured I will publish this blog even if I don't do another day of Godot work.

*If you have a personal free-tier Apple Developer account, you must link your real physical iPhone to XCode*

It does not matter that the exported Godot app can run on the native iOS simulator.

I bounced off this for a few hours before finally plugging my iPhone in on a whim and letting XCode set it up as a device.
Maybe this is obvious to other iOS developers, but all the Apple documentation cyclically points back to device management in the online portal which is a paid-only feature and _extremely_ frustrating to try and solve without clear documentation.


## Success
This time, Godot exports successfully.
Hop back into XCode, build to the device, and BINGPOT, we're in.

I admittedly still can't compile to the simulator, but my iphone is a beautiful grey piece of art

![Image](/assets/img/hello_world/ios_app.png){: .smaller}