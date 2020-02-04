---
layout: post
categories: SASS
title: Compile and Install SASS with WSL
sub_heading: Compile SASS like a pro using the WSL Terminal
date: 2020-02-03 17:00:00 -0700

---
I love using SASS because it allows me to break up my CSS and use variables when I need to simplify changes. Whether it be stylistic theme components or web page specific design I find this tool to be essential! It saves time and helps me navigate my CSS when it becomes a monster over the course of development. I highly recommend using SASS for your projects.

So let us install SASS on our WSL environment using npm. If you do not have npm on your WSL environment check out this article. I use sudo to make sure access is granted.

    sudo npm install -g sass

Once that is installed we can now use SASS. To give it a shot CD into your directory that has the project. For this example, my directory has index.html and styles.sass in the same folder. The head tag of my index.html page has the following. 

    <head>
      <meta charset="utf-8">
      <title></title>
      <link rel="stylesheet" href="styles.css">
     </head>

Notice I still link a CSS file instead of the SASS one that is already created. That is because the browser can only read CSS so we will need to compile the SASS file. We do this by running the —watch command with 2 variables separated by a colon. The first variable is the SASS file and the second variable is the output CSS file.

    sass --watch styles.sass:styles.css

SASS will output the following.

    >>>Sass is watching for changes. Press Ctrl-C to stop.

You can now navigate to your project folder and will now see the styles.css residing there. Then you should be able to make changes to your SASS file and it will automatically update your CSS file. Pretty neat!