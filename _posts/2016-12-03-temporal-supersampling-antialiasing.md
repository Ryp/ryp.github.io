---
layout: post
title:  "Temporal Supersampling Antialiasing"
date:   2016-12-03 16:26:56 +0100
categories: aliasing
images:
  - colombelles-no-aa.png
  - colombelles-tssaa.png
  - colombelles-magnified1-no-aa.png
  - colombelles-magnified1-tssaa.png
---

Temporal Supersampling Antialiasing is one of the most recent development in modern AA techniques.
The basic idea is quite simple and can be expressed as follow: Take your usual FSAA for example (let's say 4X).
Now, instead of rendering all of the subpixel samples at once, iterate over them over 4 frames and render the scene with the selected offset.
Store these pixels in a history buffer and a few frames later, your history should contain the equivalent of the 4X FSAA we talked about earlier.

Great, we just spread the usually prohibitive cost of FSAA over a few frames, getting good quality *almost* for free!

Now *what if* your camera moved a tiny bit while accumulating pixel value in the history ? Well, that's the part where all hell breaks loose. This article aims to document the stupid tricks i came up with for resolving this problem, why they sucked and how we fixed the dam thing!

{% highlight cpp %}
void main()
{
    return 0;
}
{% endhighlight %}

<div class="twentytwenty-container">
  <img width="100%" src="{{ site.baseurl }}{{ site.images }}/colombelles-magnified1-no-aa.png" />
  <img width="100%" src="{{ site.baseurl }}{{ site.images }}/colombelles-magnified1-tssaa.png" />
</div>

