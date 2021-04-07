---
layout: post
categories: CSS
title: Use CSS to easily make a webpage full height
sub_heading: 'By using min-height calc we can guarantee the footer will stay at the
  bottom. '
date: 2019-12-10T20:00:00.000+00:00

---
It's easy to find yourself wondering how to get your footer to stick to the bottom of the page. When starting out, you may overlook this component. This is because there may be enough content that the footer will wind up in its rightful place at the bottom. But when you have or want to use white space you may find that the footer is not behaving the way you want.

When I fixed this issue for myself I began to think of my website's in 3 parts. The header, content, and the footer. The part I want to focus on for this exercise is the content section of your site. I found this made the most difference to guaranteeing a full-height page when there was little content available.

![Visual showing how to get the footer to the bottom of the page. ](/uploads/height-visual.gif "CSS full height visual")

This is how I solved it with CSS:

    .main-content { 
    min-height: calc(100vh - 140px); 
    min-height: calc((var(--vh, 1vh) * 100) - 140px); 
    }

This is because applying a minimum height of 100vh (view height) may push elements outside of the page which is not what we are looking for. So what I have done here is use CSS's calc feature. The 140px represents the total number of pixels taken up by both the footer and the header. So what you are seeing is the full 100 view height minus the header and the footer. That way when the web page loads you don't have unnecessary scrolling on these pages.

The line _min-height: calc((var(--vh, 1vh) * 100) - 140px);_ is meant to define the vh for the browser in case it is not done so already. By providing both lines we are providing the right backups for older browsers.

So why vh instead of percentage? This is because some browsers will include the search bar in the browser as apart of percentage and will give that unnecessary scroll we don't want. By using vh we guarantee we are using the full view window that our website resides.

Easy stuff! Hope this helps you!