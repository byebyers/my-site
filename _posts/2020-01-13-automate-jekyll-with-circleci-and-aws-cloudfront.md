---
layout: post
categories: Jekyll
title: Automate Jekyll with CircleCi and AWS Cloudfront
sub_heading: 'Using CircleCi to take repository updates to build new static pages. '
date: 2020-01-13 14:00:00 -0700

---
Getting Jekyll and or any Static Site Generator to build and send new pages to your host of choice may leave you scratching your head. I mean you can do it yourself but do you want to? This guide utilizes CircleCi to help automate this process for you and it is pretty easy to set up. I am assuming you already have your static files hosted on AWS S3 and are using Cloudfront.

### What is CircleCi?

They are a service that spins up a virtual machine in the cloud for whatever your tech needs are. Pretty neat and even though they charge for the build time you use, they have a nice helping of free minutes. Which is more than enough for your portfolio site or hobby projects.

When it comes to Statice Site Generators, CircleCi has a neat trick that I love. It's that their service automatically scans my repository for any changes. So when I add .md files or any kind of content to my blog then CircleCi spins up an environment to build my site to be sent over to AWS. On top of that, they do all the necessary tasks to invalidate AWS Cloudfront as specified on my config file.

To get this service running for your project we need the following.

* Administrative rights to the repository.
* Knowledge of the correct virtual environment.
* A CircleCi Config file.
* Public and Private Access Keys for AWS.
* Cloudfront Key for AWS.

### Getting started with CircleCi

First, we need to go to [circleci.com](http://circleci.com/) and then sign up (or "go to app" if you already have your account). Then log in with Github or Bitbucket. The reason for this is because you need to link a repository with CircleCi to use their services.

![](/uploads/snap1-min.jpg)

From there you will be prompted to set up your first project. If not you can select "Add Projects" on the right menu pane. There you will be shown a list of your repositories from your provider. Click "Set Up Project" for the project that has the Static Site code.

![](/uploads/snap2-min.jpg)

Here is where you set up your environment. For Jekyll, I chose Linux as my operating system (most of the time you would use this) and Ruby as the language. Note you should see the warning that says you need to add a config file before building. We will do that next before we click "Start Building". If you did that already by accident it is ok. The build will not work until the config file is added.

Let's take a look at my sample config file for Jekyll. Think of this as a set of instructions for the virtual machine to compute so you don't have to do this manually every single time.

    defaults: &defaults
      working_directory: ~/repo
    version: 2
    jobs:
      build:
        <<: *defaults
        docker:
          - image: circleci/ruby:2.5
        environment:
          BUNDLE_PATH: ~/repo/vendor/bundle
        steps:
          - checkout
          - restore_cache:
              keys:
                - rubygems-v1-{{ checksum "Gemfile.lock" }}
                - rubygems-v1-fallback
          - run:
              name: Bundle Install
              command: bundle check || bundle install
          - save_cache:
              key: rubygems-v1-{{ checksum "Gemfile.lock" }}
              paths:
                - vendor/bundle
          - run:
              name: Jekyll build
              command: bundle exec jekyll build
          - run:
              name: HTMLProofer tests
              command: |
                bundle exec htmlproofer ./_site \
                  --allow-hash-href \
                  --check-html \
                  --disable-external
          - persist_to_workspace:
              root: ./
              paths:
                - _site
      deploy:
        <<: *defaults
        docker:
          - image: circleci/python:3.6.5
        environment:
          S3_BUCKET_NAME: //YOUR S3 BUCKET NAME
        steps:
          - attach_workspace:
              at: ./
          - run:
              name: Make Sure PIP is Updated
              command: sudo pip install --upgrade pip
          - run:
              name: Install AWS CLI
              command: pip install awscli --upgrade --user
          - run:
              name: Configure aws
              command: |
                      export aws_access_key_id=$AWS_ACCESS_KEY_ID
                      export aws_secret_access_key=$AWS_SECRET_ACCESS_KEY
          - run:
              name: Upload to s3
              command: ~/.local/bin/aws s3 sync ./_site s3://{YOUR S3 BUCKET NAME}/ --delete --acl public-read
          - run:
              name: Checking aws
              command: ~/.local/bin/aws --version
          # ACTIVATE CLOUD FRONT CLI
          - run:
              name: activate cloud front
              command: |
                      ~/.local/bin/aws configure set preview.cloudfront true
                      ~/.local/bin/aws configure set preview.create-invalidation true
          # Invalidate Cloudfront
          - run:
              name: Invalidate Cloudfront
              command: ~/.local/bin/aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DISTRIBUTION_ID --paths /\*
    workflows:
      version: 2
      test-deploy:
        jobs:
          - build
          - deploy:
              requires:
                - build
              filters:
                branches:
                  only: master

As you can see there are 2 parts to this process. There is the "build" section and the "deploy" section but we will go over these in detail. If you can understand what is going on here then you should be able to change this how you like to work with other Static Site Generators.

### Side Note for Jekyll:

If you are using CircleCi with Jekyll then you will need the following dependencies in your Gemfile.

    gem "jekyll", "~> 3.6.0" //or the version for your Jekyll project
    gem "html-proofer"
    gem 'nokogiri', '~> 1.10', '>= 1.10.1' //or the version for your Jekyll project

### CircleCi Defaults

    defaults: &defaults
      working_directory: ~/repo
    version: 2

All this really means is that they are pulling from a repository and you are using CircleCi version 2.

### Build Instructions

    build:
        <<: *defaults
        docker:
          - image: circleci/ruby:2.5
        environment:
          BUNDLE_PATH: ~/repo/vendor/bundle

By default, Jekyll is run in a Ruby environment. We pull this from a Docker Image as well as the environment where Jekyll installs dependencies.

### Build Steps

    steps:
          - checkout
          - restore_cache:
              keys:
                - rubygems-v1-{{ checksum "Gemfile.lock" }}
                - rubygems-v1-fallback
          - run:
              name: Bundle Install
              command: bundle check || bundle install
          - save_cache:
              key: rubygems-v1-{{ checksum "Gemfile.lock" }}
              paths:
                - vendor/bundle
          - run:
              name: Jekyll build
              command: bundle exec jekyll build
          - run:
              name: HTMLProofer tests
              command: |
                bundle exec htmlproofer ./_site \
                  --allow-hash-href \
                  --check-html \
                  --disable-external
          - persist_to_workspace:
              root: ./
              paths:
                - _site

Part of this should look familiar to you if you already build your Jekyll sites. The first steps restore the cache before installing Bundle then saves it. This has to be done every time because CircleCi spins up a brand new virtual machine when changes are made to the repository. The next steps are simple. The site is built and the HTML is checked. This part is important to make sure clean code is sent to the hosting provider. A lot of mistakes in templating can be caught in this part. The last step makes sure the static files are put in the _site folder per Jekyll's requirements.

### Deploy Instructions

    deploy:
        <<: *defaults
        docker:
          - image: circleci/python:3.6.5
        environment:
          S3_BUCKET_NAME: //YOUR S3 BUCKET NAME

In order to deploy to S3 we need to access it through this cloud terminal. That is why we are loading a python image from Docker. So that we can use PIP to install AWS CLI. The next part is the environment in which you will place the name of your AWS S3 Bucket.

### Deploy Steps

    steps:
          - attach_workspace:
              at: ./
          - run:
              name: Make Sure PIP is Updated
              command: sudo pip install --upgrade pip
          - run:
              name: Install AWS CLI
              command: pip install awscli --upgrade --user
          - run:
              name: Configure aws
              command: |
                      export aws_access_key_id=$AWS_ACCESS_KEY_ID
                      export aws_secret_access_key=$AWS_SECRET_ACCESS_KEY
          - run:
              name: Upload to s3
              command: ~/.local/bin/aws s3 sync ./_site s3://{YOUR S3 BUCKET NAME}/ --delete --acl public-read
          - run:
              name: Checking aws
              command: ~/.local/bin/aws --version
          # ACTIVATE CLOUD FRONT CLI
          - run:
              name: activate cloud front
              command: |
                      ~/.local/bin/aws configure set preview.cloudfront true
                      ~/.local/bin/aws configure set preview.create-invalidation true
          # Invalidate Cloudfront
          - run:
              name: Invalidate Cloudfront
              command: ~/.local/bin/aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DISTRIBUTION_ID --paths /\*

You should be able to understand what is happening here. Essentially we are installing PIP then AWS CLI. Then configuring AWS.

Here you should see the following:

    export aws_access_key_id=$AWS_ACCESS_KEY_ID
    export aws_secret_access_key=$AWS_SECRET_ACCESS_KEY

CircleCi has a way to add these keys in project settings so we don't need to worry about that now. What you need to know is that we are using variables that start with $ and are completely capitalized.

The next part uploads to S3. Replace {YOUR S3 BUCKET NAME} with the bucket name. No brackets.

    # ACTIVATE CLOUD FRONT CLI
          - run:
              name: activate cloud front
              command: |
                      ~/.local/bin/aws configure set preview.cloudfront true
                      ~/.local/bin/aws configure set preview.create-invalidation true
          # Invalidate Cloudfront
          - run:
              name: Invalidate Cloudfront
              command: ~/.local/bin/aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DISTRIBUTION_ID --paths /\*

The last parts checks AWS then goes on to activate and invalidate Cloudfront. All this does is clears Cloudfront's cache so it serves the most recent version of your static generated site. Really important for changes otherwise you have to do it yourself. We also have another variable called $CLOUDFRONT_DISTRIBUTION_ID that we need to add to settings.

### Workflows

    workflows:
      version: 2
      test-deploy:
        jobs:
          - build
          - deploy:
              requires:
                - build
              filters:
                branches:
                  only: master

As the name implies, this is a snapshot of what goes on. However, this does tell CircleCi what branch to pull from so if you don't want it to be master then you will change it here.

Once you are finished tweaking this config file you will add it to your project. For Jekyll you would make a .circleci folder in your root directory and add it as config.yml

### Adding the keys!

After you updated and added your config file you will go back to your CircleCi "Set Up Project" page and click "Start Building." The process will get started but not work right away. That is because we need to add our keys to those variables I talked about above. For this Jekyll config file, we have 3 variables.

    $AWS_ACCESS_KEY_ID
    $AWS_SECRET_ACCESS_KEY
    $CLOUDFRONT_DISTRIBUTION_ID

So to add these variables we need to go to jobs in the right menu pane (if you are not already there) and click the gear icon for the new project we just created.

![](/uploads/snap3-min.jpg)

From there we go to Build Settings on the right side and click "Environment Variables"

![](/uploads/snap4-min-1.jpg)

![](/uploads/snap5-min.jpg)

There is where you add your variables. However, you will need to add the name of the variable in all caps without the $ in front as shown above. The value will be your AWS key. Once these are added then your project should be able to build correctly when changes are made. If you want to rerun a failed build you would click the build number then "Rerun workflow".