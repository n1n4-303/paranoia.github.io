---
layout: default
title: "Understanding granular synthesis with pure data"
permalink: /tutorials/granular
---

# Understanding granular synthesis with pure data
  
<br />Granulation is a technique in which you manipulate a short-duration microacoustic event (typically between 1 to 100ms) known as grain, that can be used on sound design to achieve multiple results, like creating unpredictability, for instance. It may be useful if you're thinking sound as a process and you can take it to many interesting directions. If you are interested in learning more deeply about granular synthesis and its possibilities, refer to ROADS, 2004.
<br />Granular synthesis is behind many fancy plugins or built-in fx you are used to see in your DAW of choice, such as Ableton's Grain Delay. A good way to understand how they're used might be to just try to build a simple granular synthesizer yourself.


## Let's do it!

<br />**The recorder**
<br />So we need a way to record an input. We do that by using an analog to digital converter [adc~]. For the sake of organization, we're going to put it into a subpatch, so our patch will look more clear further on.
<br />
![subpatch-recorder](https://user-images.githubusercontent.com/64982634/83361737-46ace280-a383-11ea-8692-bd982ff0bf39.JPG)
<br />
The signal coming from your microphone is an analog signal which represents in voltage the changes that happen in air pressure. What the adc does is to translate these into digital domain by capturing periodic snapshots of that voltage and representing these snapshots as discrete numbers.
<br />Later in the patch you'll see that we need a [dac~], a digital-to-analog-converter in order to listen to these numbers back through our speakers.
<br />
![recorder](https://user-images.githubusercontent.com/64982634/83361865-790b0f80-a384-11ea-864a-ae847d533a62.JPG)
<br />
The bang is your record button. Note that pd records at whatever sample rate ADC and DAC are running at, this is defined in your audio settings, so be aware of that.
<br />[tabwrite~] will allow to us to visualize our waveform and store it. For that to happen, you need to create an array with a matching name.
<br />Here our array is going to be one second long.
<br />
![array-properties](https://user-images.githubusercontent.com/64982634/83362038-17e43b80-a386-11ea-8364-cf0d2047492f.JPG)
<br />
I added 3 points to the size so that I could use a 44.1 value in my patch. You might want to look up how our [tabwrite4~] object operates to understand interpolation. We're going to use it here to read our array continuously.
<br />I'm also going to create a way for us to write and read our sample file. Here's how:
<br />
![soundfiler](https://user-images.githubusercontent.com/64982634/83362167-1109f880-a387-11ea-8809-508a4ff3cc96.JPG)
<br />
→ Why can't I read my file?
<br />A reason you might not be able to read a file from soundfiler is that you must save the patch first so that pd knows what directory to read from. Pd will save your sample to the same directory as the patch. I'm also going to create a way for us to write and read our sample file. Here's how:
<br />Lets put a [tabread4~] as I mentioned before to read from our "sampling" array. And we should also add a [dac~] in order to listen to the sample and give it a on and off button.
<br />
![dac-tab](https://user-images.githubusercontent.com/64982634/83399431-1dc73480-a3f9-11ea-8d2d-a6571ccb4115.JPG)
<br />
<br />**The sampler**
<br />Now we're going to have a [phasor~] reading the sample and then a multiplier [*~] and [+~] initially set to read the entire sample. You'll see where this is going now.
<br />
![phasor-1](https://user-images.githubusercontent.com/64982634/83399821-c4abd080-a3f9-11ea-9ef2-2ab1312f6763.jpg)
<br />
The [*~] object will allow us to manipulate the amount of the sample we want to playback and the [+~] will set the starting point.
<br />
