---
layout: post
categories: Netlify
title: Configuring Netlify Large Media
sub_heading: Save large files on Netlify and not your repo...
date: 2020-07-24 13:00:00 -0700

---
### Things to understand before getting started. 

If you are using Gatsby, you cannot use the following plugins. Which are included in the Netlify Starter Template. 

    gatsby-plugin-sharp
    gatsby-transformer-sharp
    gatsby-remark-relative-images

Nor can you use Gatsby image. If you have those they will throw errors in deployment and prevent Netlfy LM from working. 

Another thing to understand is that Netlify LM transform images do not work on a development server. Only in production. So you will need to deploy and publish to see if your transformations work. 

### Getting Started

First you will need git LFS

    git lfs install

Make sure Identity and Git Gateway are enabled and added to your Netlify config file. You need this to create a connection between your repo and Netlify LM. 

Then you can install Netlify CLI if you haven't already.

    npm install netlify-cli -g

Add the Netlify Large Media addon. 

    netlify plugins:install netlify-lm-plugin
    netlify lm:install

Next log into Netlify then link your repository.

     netlify link

This will create a .netlify/state/.json file in your project with the relevant site id.

    {
    	"siteId": "xxx"
    }

After, I added this folder to my .gitignore file.

    # Local Netlify folder
    .netlify

Then run the setup

    netlify lm:setup

Which will create another file called .lfsconfig

    [lfs]
    	url = https://xxx.netlify.app/.netlify/large-media

Commit .lfsconfig. Next, we can start tracking changes.

You can track specific files by there name.

    git lfs track "lake.jpg"

Or you can track an entire folder.

    git lfs track "static/image/**"

This will create a .gitattributes file with the following code.

    static/img/** filter=lfs diff=lfs merge=lfs -text

Commit the file. We can now set up [netlify credential helper](https://github.com/netlify/netlify-credential-helper "netlify credential helper"). This helps with authentication. If you are using WSL with Windows then be sure to follow the "Windows with Powershell" instructions.

If it asks to download a file then look for it under "Releases" on the right-hand side of the repo. 

With that finished, you should now have the binary file for the credentials. I had to add a few lines of code to my gitconfig file in my .netlify folder as well. You might not need to do this. My file was located below however your path may be different. 

    home/username/.netlify/helper/gitconfig

To access this file I ran the following command. 

    nano gitconfig

And added this code.

    [credential]
    	helper = netlify
    [lfs]
    	contenttype = true

Hit ctrl + o to write out files then ctrl + x to exit. 

Once this is finished then you push your lfs files.

    git lfs push --all

If it asks for a branch than it may be origin for you. 

    git lfs push --all origin

Then go to your Netlify Site account and check Large Media. Your files should be available there. 

![Nelify's large media tab showing files you uploaded.](/uploads/lage-media-page.jpg "Large Media Page")

And images in your git repo should look like this.

![](/uploads/git-lfs-image.jpg)

### Transforming Images

You can add transformations like this.

    <img
      src={`${image}?nf_resize=fit&w=300`}
      alt="About page"
    />

Or like this.

    <img srcset="img.jpg?nf_resize=fit&w=320 320w,
                 img.jpg?nf_resize=fit&w=480 480w,
                 img.jpg?nf_resize=fit&w=800 800w"
          sizes="(max-width: 320px) 280px,
                 (max-width: 480px) 440px,
                 800px"
            src="img.jpg?nf_resize=fit&w=800" alt="Albus using spells to save Harry">

### Troubleshooting

I recommend building your site and see if there are any errors there. Fixing those helped me get the files to upload.

Another thing you can do try lm:info

    netlify lm:info

    netlify lm:info
      ✔ Checking Git version [2.20.1]
      ✔ Checking Git LFS version [2.7.0]
      ✔ Checking Git LFS filters
      ✔ Checking Netlify's Git Credentials version [0.1.8]

If you are missing any of these then connect with the Netlify Community for a fix. If not the issue could be in the deploy logs.

### References

[https://community.netlify.com/t/support-guide-troubleshooting-your-netlify-large-media-configuration/188](https://community.netlify.com/t/support-guide-troubleshooting-your-netlify-large-media-configuration/188 "Support guide troubleshooting your netlify large media")

[https://git-lfs.github.com/](https://git-lfs.github.com/ "https://git-lfs.github.com/")

[https://css-tricks.com/getting-netlify-large-media-going/](https://css-tricks.com/getting-netlify-large-media-going/ "https://css-tricks.com/getting-netlify-large-media-going/")

[https://docs.netlify.com/large-media/overview/#large-media-docs](https://docs.netlify.com/large-media/overview/#large-media-docs "https://docs.netlify.com/large-media/overview/#large-media-docs")

[https://github.com/netlify/netlify-credential-helper](https://github.com/netlify/netlify-credential-helper "https://github.com/netlify/netlify-credential-helper")

[https://netlify-photo-gallery.netlify.app/](https://netlify-photo-gallery.netlify.app/ "https://netlify-photo-gallery.netlify.app/")