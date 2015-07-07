---
layout: post
title: Attempting to control shuffle of Korg Volca Beats
categories: Korg, Analog, Hardward, Drum Machine, Max
---

After getting the tempo modulation to work, I tried writing a Max patch to introduce shuffle via the same means.

Take a measure with regular eighth notes. Say you want to introduce swing to this measure.  You do this by varying the time between the "on"-notes (the odd-numbered eighth notes in the sequence) and their subsequent "off"-notes (the even-numbered eighth notes in the sequence). An off-note can sound anywhere between the preceding and subsequent on-notes, and this variability in delay is expressed as a percentage of the time between on-notes. Which, if we are shuffling eighth notes, is the tempo itself... Convenient, right?  So first let's find the tempo  in milliseconds:

![](/assets/img/eqn1.png)

Which means our tempo in milliseconds _bpm<sub>ms</sub>_ is:

![](/assets/img/eqn2.png)

And the delay _d_ is determined by the tempo as well as the shuffle percentage _s_:

![](/assets/img/eqn3.png)

So my idea was to generate two bangs from a metronome object in max and delay one of them according to this equation.  Here's the patch:

![](/assets/img/shufflepatch.png)

[Here's a link to the patch.](https://github.com/syntactical/korg_patches/blob/master/KorgShuffle2.maxpat)

I got a slider working for the shuffle amount.  And it does work... but not with the actual drum machine.  There's something wrong with the cadence of the rhythm when you introduce the shuffle... It's hard to explain, but certain beats are entirely skipped over.  But this gave me some ideas about using songs to trigger the tempo of the drum machine, something I haven't been able to explore yet.  Songs that are purely rhythmic and surgically clean, like [Ryoji Ikeda](https://www.youtube.com/watch?v=aKjUo6cegko), so I did this...

<iframe width="100%" height="166" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/213245395&amp;color=ff5500&amp;auto_play=false&amp;hide_related=false&amp;show_comments=true&amp;show_user=true&amp;show_reposts=false"></iframe>

Needs work. But anyway, I did some research and it seems like the SYNC IN takes a 5V signal to trigger the tempo, which apparently is not possible with most sound cards.  But the iPhone app has no problem with it, so I plugged the iPhone app into my computer and recorded the impulses with Audacity.  Here's what they look like:

![](/assets/img/gatetrigger.png)

![](/assets/img/gatetrigger-zoom.png)

A few things I notice... the waveform is clipping, it looks like some sort of electrical signal, and it's only going through the left channel.  But I don't understand much else of what's going on.

I had the idea to record this waveform and play it instead of using the output of ezdac~, but I haven't been able to work on it yet.  I'm going to have to do more research.  