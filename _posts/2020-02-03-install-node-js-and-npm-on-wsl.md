---
layout: post
categories: Node.js
title: Install Node.js and npm on WSL
sub_heading: Get the package manager you need on WSL!
date: 2020-02-03 15:00:00 -0700

---
![](/uploads/npm-visual.gif)

Node.js is a popular open-sourced platform for executing server-side Javascript code. Great for building back-end server-side applications. With that, we also need npm, the default package manager for Node.js and one of the greatest tools in your chest!

With WSL I installed these from NodeSource because they maintain a repository of the latest version of Node.js.

To install Node.js and npm follow these steps.

Enable the repository by running this curl command as a user with sudo permissions. If you don't run the command with sudo.

    curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -

Once that is enabled you can install Node.js and npm by entering

    sudo apt install nodejs

You can verify if you have them by entering the following.

    node --version

and

    sudo npm --version