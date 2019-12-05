---
layout: post
categories: Services
title: How I save my keys on Windows.
sub_heading: Saving public and private keys locally.
date: 2019-12-04 16:00:00 -0700

---
Saving public and secret keys can be a hassle when you are starting out. Especially if you are new to doing any kind of security for servers. I hope to give a couple of things to think about when utilizing your local machine to save keys. As well as securing them in a way to store online.

First I have to say the following. I am not a security expert, nor do I guarantee that my way is the best. Since you are reading this article, I assume you are already doing your research. So please compare my article with other resources online. You will find something that works for you.

Also, I am assuming that you and your client are using Windows. If you need a Win & Mac solution you can take what you learn here and find alternatives.

Ok, with that out of the way I am going to talk about the following:

\-Understanding what apps are local to your machine compared to cloud-based ones.

\-Using free, open-sourced tools to protect your keys so that your client can use them as well.

\-Creating good habits online

So here it goes. What runs on the cloud and what runs on your machine? As a Windows user, you may be familiar with how Microsoft pushes its customers to use online accounts to log in. The days of the local machine will live on, but it is becoming more difficult. You are asked to sign in to Cloud services to get the full "Windows" experience. This is how Microsoft can get important details like crash reports. While also providing information to third-party services to better reach out to you. It's dumb, and if you are interested in preventing third party access you can check out this article by [Wired](https://www.wired.com/story/windows-10-privacy-settings/).

Beyond that, we can look at a more of a common-sense approach to your Windows 10 computer. When it comes to saving sensitive data, do not use services like Sticky Notes or Office 365. Why? because you can access these resources online from any device. That is where a breach can occur. On my machine, I keep in mind what platforms I am using and if I can access it via phone. If the answer is yes then it's not a great idea despite how useful they are.

I know that is obvious but even if this reminder helps only one person than it is worth it. It's funny how many tools we use every day that are cloud-based that we are not aware of. Anyways let us get on with it.

With Cloud-based apps out of the picture then what do we use? For keys and sensitive data, I recommend using open-source options locally. What I use is Libre Office, a free alternative to Office 365. This is because LibreOffice Writer (Microsoft Word) can password-protect documents. I make a separate doc for each set of keys I have or per client. I never save my keys in one place. With that, I save these documents on my machine in a folder that is not labeled Desktop.

How to save a LibreOffice Doc with a password

1) Save the doc as you would normally do.

![](/uploads/shot1.jpg)

2) Check the box "Save with password"

![](/uploads/shot2.jpg)

3) Add password and click "ok"

![](/uploads/shot3.jpg)

Yes, you can do this with Word and if your client prefers that program then so be it. But that means they need to have an office 365 subscription. So having a free option is a better alternative. AND ALWAYS USE A UNIQUE PASSPHRASE FOR GODS SAKE. Here is a doc on why [passphrases are better than passwords](https://www.passworddragon.com/password-vs-passphrase). Can't stress this enough in life. Using old passwords for things increases the chance you use them in a non-secure way. As of the time of this writing, Disney Plus is getting heat from its users who are getting hacked. All by hackers using their victim's old passwords. With that in mind, unique passphrases are better. Please be aware that there is no password resets with these documents. So if you forget your password then you lost the document for good.

That is when good habits come in. When I save these docs on my machine, I put them in non-obvious folders, and I password protect my Windows device. That way there are at least 2 password walls to get to those keys. So what about remembering these passphrases? If I am using unique passphrases for each doc containing keys then how the heck do I remember these? Great question and what I currently use is [Mozilla's Lockwise password protector](https://www.mozilla.org/en-US/firefox/lockwise/). Again, it is a free open-sourced alternative to Paid Password wallets. The way I save my keys is by doing the following.

\-Use the website main address as the URL

\-Make the document title as the name

\-Add the password to the password field

Simple

If you do this you must again use a unique passphrase for your Lockwise account. NOT YOUR NETFLIX PASSWORD. This way you only have to remember 1 passphrase instead of many. Problem solved. I find this to be the easiest solution for me to use.

Before we finish up I have another solution for you. In case you need to store these password-protected docs online or share them with your client. You also may need a way to share other sensitive documents. Which is why I recommend using a file compressor like [7-zip](https://www.7-zip.org/). It is another open-sourced solution for Windows in which you can compress a folder to a .zip file. It saves space AND you can password protect them.

Here is how to use 7-Zip

1) Right-click the folder you want to zip. Then hover over 7-zip then click "Add to archive..."

![](/uploads/7zip1.jpg)

2) In the menu, click "Archive Format" and select "zip"

![](/uploads/7zip2.jpg)

3) After that enter your passphrase in the Encryption section. Then make sure the Encryption method is set to "AES-256"

![](/uploads/7zip3.jpg)

4) Once you hit "OK" you will have a zipped folder in the same directory as your original.

Like the word docs, there are no redos with these passwords and you should use a unique passphrase for safety. Just like we did for creating those password-protected docs above. With those zip folders protected you could store them in a private folder online (at your discretion) or on a hard drive at home. That way you are covered if your machine goes down.

And that's it. A super simple way to store your keys and little things to look out for on your Windows 10 operating system. As long as you are looking out online you should have most of your bases covered. Good luck and have fun!