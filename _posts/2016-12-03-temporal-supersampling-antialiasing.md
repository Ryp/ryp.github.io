---
layout: post
title:  "Temporal Supersampling Antialiasing"
hidden: true
categories: temporal aliasing
images:
#  - tssaa/colombelles.png
#  - tssaa/colombelles-tssaa.png
  - tssaa/colombelles-magnified1.png
  - tssaa/colombelles-magnified1-tssaa.png
  - tssaa/sme-smear.png
date: 2016-12-03 16:26 +0100
---

Temporal Supersampling Antialiasing is one of the most recent development in modern AA techniques.
The basic idea is quite simple and can be expressed as follow:

Increase the number of samples you render for a single output pixel (let's call this number **n**),
but instead of rendering all of the subpixels at once like you would when doing supersampling,
iterate over them over **n** frames and render the scene with the selected screen-space offset while
storing you last results.

When you have enough frames, merge the history of the last **n** frames together and you should obtain
a antialiased image based on these jittered frames.

What we did was just spreading the usually prohibitive cost of supersampling over a few frames,
getting the equivalent quality for *almost* free!

This is the result on a static scene:

<div class="twentytwenty-container">
  <img width="100%" src="{{ site.baseurl }}{{ site.images }}/tssaa/colombelles-magnified1.png" />
  <img width="100%" src="{{ site.baseurl }}{{ site.images }}/tssaa/colombelles-magnified1-tssaa.png" />
</div>

*This screenshot was taken using 8 samples of a 2-3 Halton sequence.*

While this technique might sound impressive given the fact that performance was not sacrified,
it is crucial to preserve temporal coherency when objects move on the screen.
Don't do this and the history will not correspond to the current object we are shading anymore!

Smearing is barely noticeable as you can see:
<img width="100%" src="{{ site.baseurl }}{{ site.images }}/tssaa/sme-smear.png" />

# It's all downhill from here

This article aims to document some of the stupid tricks i came up with for solving these problems,
why most of them sucked and how we fixed the dam thing!

<!--
{% highlight cpp linenos %}
void main()
{
    return 0;
}
{% endhighlight %}
-->

