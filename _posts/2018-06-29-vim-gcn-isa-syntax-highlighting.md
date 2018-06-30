---
layout: post
title:  "Release: AMD GCN ISA Syntax Highlighting for Vim"
date:   2018-06-29 16:00:00 +0100
categories: vim release gcn
images:
  - vim/gcn-highlighting.png
---

I got into low-level shader optimizations lately, and realised that not many tools
are provided for the job. Since i'm looking at GCN assembly regularly now, i used
the occasion to write a decent syntax highlighting for it inside vim.


That way i got to go through the whole GCN ISA document (props to AMD for having
the only public GPU ISA out there) as well as learning some vim scripting basics!

You can find the plugin on [GitHub](https://github.com/Ryp/vim-gcn-isa).

<img width="100%" src="{{ site.baseurl }}{{ site.images }}/vim/gcn-highlighting.png" />

Writing a simple syntax plugin for vim was overall a pleasant experience, and
i strongly recommend taking a look at [AMD's GCN3 ISA](http://developer.amd.com/wordpress/media/2013/12/AMD_GCN3_Instruction_Set_Architecture_rev1.1.pdf)
document if you care about performance (or just GPU architecture in general).
