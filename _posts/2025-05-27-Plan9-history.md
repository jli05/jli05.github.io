---
title: Plan 9 History and Resources
layout: post
---

[Plan 9](https://en.wikipedia.org/wiki/Plan_9_from_Bell_Labs) is a computer operating system intended as successor to Unix. The last edition the 4th was released in 2002, after which various forks continued.

![Plan 9 chronology](/assets/2025-05-Plan9/Plan9-chrono.pdf)

At this moment, resource about Plan 9 is sometimes scattered, sometimes scarce. Below is a summary:

4th Edition only offers 32-bit support.
* download [http://9p.io/plan9/download.html](http://9p.io/plan9/download.html)
* A copy of code on GitHub [https://github.com/0intro/plan9](https://github.com/0intro/plan9)

9Legacy incorporated patches following the 4th Release and 64-bit support for Intel, AMD, ARM, RISC-V etc processors.
* download [http://9legacy.org/download.html](http://9legacy.org/download.html)
* A copy of code on GitHub [https://github.com/0intro/9legacy](https://github.com/0intro/9legacy)

From the download pages one may download live system image, then run a simulator as `qemu` to boot with them.

Plan 9 command manual pages can be found at [https://plan9.io/sys/man/1/INDEX.html](https://plan9.io/sys/man/1/INDEX.html). Some commands are recognisable as under Unix/Linux. Some are specific to Plan 9: `rc` is the name of the shell instead of `dash` or `bash`. `cpu` for connecting to remote machine.

[http://shithub.us](http://shithub.us) holds many Plan 9-related projects. For example [lola](http://shithub.us/aap/lola/HEAD/info.html) a windowing system to supplant the built-in `rio`.

![lola nature theme](http://aap.papnet.eu/pub/plan9/themes/nature.png)
