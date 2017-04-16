---
layout: post
title:  "Reinstalling... again"
date:   2017-04-16 00:00:00 +0100
tags:
---

And after so long, here I'm...

I got this bank holidays to start learning about Machine Learning (but this will be another topic).



This post is mainly to show my massive failure of installing [cuda](https://developer.nvidia.com/cuda-downloads) drivers and [cudnn](https://developer.nvidia.com/cudnn) libraries in my Linux Mint. 
I mainly failed and broke all my video configurations with that :( 
Meanwhile, looking on the internet I saw that Ubuntu was recommended for this, as it has many drivers available. So after I screwed up everything, I decided to install Ubuntu, and here we go formatting the system and installing everything again. When trying to install the NVIDIA Driver 375 `apt show nvidia-375`, it failed miserably, screwing up my windows manager. After fighting with `lightdm` and trying `gdm` I gave up and decided to come back to Mint, and take the opportunity to install the newest version, [18.01 Cinnamon](https://www.linuxmint.com/download.php).

<!--more-->
And here we go again, installing all the programs. So I got tired of it and I decided to create a script to do it, just in case I screw it up again, what I might do next time I try to install those drivers again.
Obviously it's still a simple script, but the idea is to add everything I still in my computer, so I will try to keep it as up to date as possible.
You can find the repo [here](https://github.com/diegoluiz/mint-setup). 
To install everything you just need to run the following line in your terminal (it will ask the root password)
```
wget https://raw.githubusercontent.com/diegoluiz/mint-setup/master/script.sh | bash -E -
```

So, I hope soon I will be posting about my machine learning thoughts... 
