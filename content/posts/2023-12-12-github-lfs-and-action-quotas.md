---
title: "Git LFS and Github Free Tier"
date: 2023-12-27T19:29:31+03:00
tags: [git, lfs, github]
---

> TL;DR: Just don't!
# Git, binary files, and LFS

Storing binary files directly in git is something you might be interested in doing, but it wasn't really designed for that purpose to begin with. Git is good at versioning text files, not binaries!

**In short:** Git by default doesn't generate diffs for binaries and versions the complete history for each file, which will cause your `.git` folder to explode in size over time.

I'm working on [a game](https://github.com/Ryp/reaper) and naturally this need came up. I chose Git LFS to address this issue because I worked with it before, and it seems like the most mature option for my use case so far.

**TIP:** If you're migrating over to Git LFS but you already have some binary files in git, you can clean your repo history beforehand with [git-filter-branch](https://git-scm.com/docs/git-filter-branch).

# Github Free Tier and Git LFS

Github is probably where you're already hosting your code — and they just so happen to have LFS support in the free tier, so that will be today's focus.

According to Github's [pricing page](https://docs.github.com/en/billing/managing-billing-for-git-large-file-storage/about-billing-for-git-large-file-storage):
> Every account using Git Large File Storage receives **1 GiB of free storage** and **1 GiB a month of free bandwidth**. If the bandwidth and storage quotas are not enough, you can choose to purchase an additional quota for Git LFS. Unused bandwidth doesn't roll over month-to-month.

The storage limit sounds reasonable for a lot of projects, including mine — at least for the foreseeable future. Bandwidth is where it gets spicy!

# Clone Clone Clone

Aside from pulling and pushing, cloning should be the operation that eats up most of that bandwidth quota. To be specific:
- Your clones. This should be reasonably low — we're good.
- **Github Action clones**; oh.
- **Other people's clones**; WHAT!

It sounds overwhelming, so let's break it down.

# Taming Github Actions

At the time of writing, my CI scripts are spawning 12 jobs that need to clone the repo to work. If you were hosting 100MB of data there, one commit and *-POOF-* your bandwidth quota is gone!

A neat idea here is to cache all LFS files using the cache of Github Actions. It has its own quota, of course, but none that we should be worrying about yet.

With this idea, a runner would try to get the LFS files from the cache first, instead of getting them from the checkout step. The first jobs will have to warm up the cache and actually eat some LFS quota, but successive runs should just grab that data for free!

I'm not the first to come up with the idea; there's even a clever drop-in solution that does all that and more! It inserts itself at the checkout step and is mostly transparent.

You can find it at [nschloe/action-cached-lfs-checkout](https://github.com/nschloe/action-cached-lfs-checkout).

# Thou shall not cache!

I grabbed it, added it, and pushed a few commits before going to sleep and letting the CI work for me overnight. The next day, I woke up to an email from Github telling me that my quota had been spent. Urgh.

It turns out there's some kind of [issue](https://github.com/nschloe/action-cached-lfs-checkout/issues/2) that shows up. Even if the runner logs show that no Git LFS files were pulled (except from the cache, of course), the quota goes down...

I didn't investigate more; my current theory is that LFS-enabled checkouts are deducted from the quota even if the runner didn't pull any data — and I can imagine why it's not by accident. I'll let someone braver than me go to the bottom of the issue. In the meantime, that means that we're getting home empty-handed. Oh well.

# How to DoS on Github

The limit I personally find obnoxious is the fact that random people on Github can just clone my project ten times and run down my free tier quota.

I don't have a solution for this, except setting my project to private - a deal-breaker for me.

# Conclusion

If anyone has a workaround for both previous issues, please let me know. In the meantime, my conclusion is that Github's Free Tier LFS offer is not usable for most projects.

I ended up creating a separate repo with all the asset files. I link it back to the main repo with a Git submodule and that's good enough for me. The history of the asset repo will become big over time but won't pollute the rest. When it stops being manageable, I hope there'll be a better solution on the horizon, and I'll be able to seamlessly switch.
