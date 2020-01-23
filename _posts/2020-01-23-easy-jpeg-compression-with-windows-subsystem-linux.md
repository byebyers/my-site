---
layout: post
categories: WSL
title: Easy JPEG compression with Windows Subsystem Linux
sub_heading: Fastest way to compress a ton of image files!
date: 2020-01-23 13:00:00 -0700

---
![](/uploads/compress-gif.gif)

File compression of images is just a reality in digital life. But when you have over a hundred pictures this can become a time-consuming task. If you have Windows Subsystem Linux on Windows or a Linux kind of terminal here is a quick way to relieve that stress!

First, pull up your terminal and let's download a time-saver called jpegoptim!

Let's get that software with sudo

    sudo apt-get install jpegoptim

Let us move to the folder with all of pics.

    cd path-to-your-file

Then let us compress the batch!

    jpegoptim *.jpg

And you are done! Note if your files end in .jpeg then use that file extension instead.