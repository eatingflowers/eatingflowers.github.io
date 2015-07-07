---
layout: post
title: Using Max patch to control tempo of Korg volca beats
categories: [Korg, Analog, Drum Machine, Max, Hardware]
---

I just got a Korg Volca Beats analog drum machine, and it's great.  It's all analog and cost less than $200.  (I also bought an Akai Rhythm Wolf, another analog drum machine in the same price range, but I sent it back because it didn't do nearly as much as the Korg did and was more expensive.)  It's really fun to play with and you can get totally absorbed in it for hours.  Here it is:

![Korg volca beats](/assets/img/volcabeats.jpg)

One of the features that I notice this is missing, though, is a shuffle feature.  ("Shuffle" is a term used to describe rhythmic cadence that gives music a "swing" feeling, as in Jazz.  [Here are some examples if this doesn't make sense.](https://en.wikipedia.org/wiki/Swing_%28jazz_performance_style%29#Swing_note))  I can't believe it doesn't have a shuffle feature!!

It turns out Korg has an iPhone app that will let you control the shuffle parameter as well as the tempo (which already exists on the device, of course.)  However, the shuffle parameter on the app is limited to 50-75%.  But what if I want more than that 25% range?  (I will explain the shuffle parameter in another post detailing my patch to control this shuffle.  Briefly, though, it refers to the position of the off-beat notes in between the on-beat notes.)  I also don't have an iPhone.

The drum machine has a feature called "SYNC IN", which can accept a signal denoting a rhythmic pattern via a 1/8" audio cord.  This is also how the iPhone app communicates with the drum machine. I didn't know anything about what the signal was supposed to be, but I started out by trying to control the tempo of the device from a Max patch...

This was my first Max patch, and [Andy McWilliams](http://jahya.net/) helped me get started.  Here's what it looks like:

![Tempo patch](/assets/img/tempopatch.png)

[You can see the code for the patch here.](https://github.com/syntactical/korg_patches/blob/master/TempoKorg.maxpat)

Veeeeery simple.  That's 120 beats per minute, 1 for the beat multiplier (from the Max tempo reference: "can lengthen the amount of time taken for one beat. It slows the tempo down by a factor. For example, a multiplier of 2 will make tempo send out its output half as fast."), and 8 for the "initial rhyhtmic output" (is this a common phrase?), which refers to the number of subdivisions each measure has.  So with 8 we're outputting 8 eighth notes per measure.  All this connected to a change object, which is necessary since the tempo object doesn't emit bangs.  It emits a number corresponding to whichever subdivision of the measure is currently played.  So in our example it'd go 0, 1, 2, 3, and on up to 7.  (Remember we are counting from zero.)  The change object registers the numeric message emitted from the tempo object, and effectively emits a bang with each changing number from the tempo object.  This is connected to a click~ object which in turn is connected to an ezdac~ object, which convert the digital bang signal to an analog audio signal.

And it worked!  I got an idea of what the rhythm of the signal was by plugging my headphones into the drum machine's SYNC OUT jack and found that the signal was composed of regularly spaced eighth notes.  I can now control the tempo of my drum machine with the Max patch.

I'll be writing about trying to get the shuffle feature in a later post.