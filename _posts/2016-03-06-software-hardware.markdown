---
layout: post
title:  "- Software + Hardware"
date:   2016-03-06 00:00:00 +0100
categories: hardware
---
AS I said in my previous post, I bought some stuff to improve my laptop's performance, specially this [part](http://uk.crucial.com/gbr/en/ct250mx200ssd3), which I was amazed how a laptop could have an extra small SSD. I knew it would be tough to disassembly my laptop, but in my first attempt, after some effort, I managed to install the RAM memory. I have a [Asus G55VW](https://www.asus.com/ROG-Republic-Of-Gamers/ROG-G55VW/), it's a pretty old, but powerful, laptop.
<!--more-->
And it turned out to be harder than I expected to install the mSata. Yesterday I decided to install it once for all, and this is the result ![IMG_20160305_213914]({{ site.url }}/assets/2016-03-06-software-hardware/img_20160305_213914.jpg) I'm happy to be writing this post from my laptop, because I didn't know if I would be able to assembly it again hahaha After everything was in place, I started install Linux Mint again, and after it, in my first boot, nothing happened. The drive couldn't be found to boot, but once I started my old Linux system I could see it. So when I looked further on the internet for the problem I found out how out to date I'm. When I was younger, I learnt about one thing called [MBR](http://www.wikiwand.com/en/Master_boot_record), which is where [GRUB](http://www.wikiwand.com/en/GNU_GRUB) writes the code to do the boot magic. But MBR was superseded by [GTP](http://www.wikiwand.com/en/GUID_Partition_Table) and it some [BIOS](http://www.wikiwand.com/en/BIOS) can't map it, because now, we have a new thing instead of BIOS, it's called [UEFI](http://www.wikiwand.com/en/Unified_Extensible_Firmware_Interface). I read some pages on the internet explaining how to boot GTP with BIOS, but none of them worked :( Summarising, I could install the device, but I couldn't make my OS being booted by it, so I mapped only my /home folder to be there. Lessons learnt:

* Even though I'm more into other things (software), it's good to keep up to date with hardware stuff
* I can assembly and disassembly my laptop completely :)
* Some new stuff as GTP, UEFI and so on (which I still have to study deeper)

  See you :)
