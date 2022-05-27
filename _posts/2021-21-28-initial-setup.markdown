---
layout: post
title:  "Initial Setup"
date:   2021-12-28 15:40:21 -0500
author: "Matt Nicotra"
# permalink: /2021-21-28-initial-setup/
---

We want to transition our website from a WYSIWYG hosting service to [GitHub Pages](https://pages.github.com/) because it will be cheaper (free). We also hope that it will enable us to share web maintenance duties more easily.

To set up the site I first learned about GitHub pages and Jekyll from [this excellent set of youtube videos by Bill Raymond](https://www.youtube.com/watch?v=EvYs1idcGnM&list=PLWzwUIYZpnJuT0sH4BN56P5oWTdHJiTNq). 

Then I created the initial site. 

### Set up repo on GitHub

1. Created a repo called "[cnidofest-website](https://github.com/matthewnicotra/cnidofest-website)" in my GitHub account. The repo is empty, but that is okay because it will be filled with files from Jekyll soon.

2. Created a README.md file for the repo. Note: this establishes the "main" branch of the repo, which is what I will select in the next step.

3. Enabled the repo as be a GitHub Pages website by going to Settings > Pages, then selecting "main" from the dropdown under **source**. I kept the folder at "/root", then clicked "save".

4. I also edited the "About" box (circled at top of screenshot below) to include the path to the site for ease of access later on.

    ![Screenshot of the repo page ](/cnidofest-website/assets/images/repo-after-first-build.png)

5. After a few minutes, an "Environment" appeared at the bottom of the **\<code\>** tab for the repo, which meant that GitHub had build the page (using Jekyll). Right now, Jekyll just uses the README.md in place of an index.html page, so the site is pretty bland:

    ![Screenshot of the site](/cnidofest-website/assets/images/site-from-readme.png)

### Clone repo and set up site with Jekyll

Now, I am going to clone the repo and use Jekyll to create the site. This will let me develop the site on my local computer before pushing it back to GitHub, where Jekyll will build the site for GitHub pages. 

I have already installed Jekyll on my computer.

1. On my laptop, I created a folder called `Sites`. I then moved into that folder and cloned the `cnidofest-website` directory.

    ```bash
    cd Sites
    git clone https://github.com/matthewnicotra/cnidofest-website.git
    ```

2. I then changed into this directory and used Jekyll to create a new site. The `bundle exec jekyll new` creates the new site, the `.` tells Jekyll to create the site in this directory, and the `--force` is used because Jekyll will balk at created a new site if there are files in the existing directory.

    ```bash
    bundle exec jekyll new . --force
    ```

3. _This step was only required for me because, for some reason, I need to manually add webrick to jekyll._ 

    ```bash
    bundle add webrick
    ```

4. Now I can test the site by running the command below and heading to http://127.0.0.1:4000/

    ```bash
    bundle exec jekyll serve
    ```

5. In `_config.yml` there are a few things to update. Under site headings, changed the site info from the defaults:

    ```yml
    title: Cnidofest
    email: cnidofest@gmail.com
    description: >- # this means to ignore newlines until "baseurl:"
        The Cnidarian Model Systems Meeting 
    baseurl: "/cnidofest-website" # the subpath of your site, e.g. /blog
    url: "http://matthewnicotra.github.io" # the base hostname & protocol for your site, e.g. http://example.com
    twitter_username: cnidofest
    github_username:  matthewnicotra
    ```

6. To make these changes take effect, we have to kill and restart Jekyll. 

    ```bash
    ^C
    bundle exec jekyll serve
    ```

### Set up the theme

The default theme for Jekyll sites is `minima`. But for this site, I am going to use the theme [Hydejack](https://www.hydejack.com). I want to use a static version of the hydejack theme (so that I can control when the theme updates, rather than having the theme change without me knowing). Therefore, I did the following:

1. Fork the `hydecorp/hydejack` repo, which I renamed `matthewnicotra/hydejack-matthewnicotra` (to make clear I am using the local version)

2. Edit the `_config.yml` file as follows. The plugin `jekyll-include-cache` is required by the Hydejack theme.

    ```yml
    # Build settings
    remote_theme: matthewnicotra/hydejack-matthewnicotra
    plugins:
    - jekyll-feed
    - jekyll-include-cache
    ```


3. Edit the `Gemfile` so it looks like this:

    ```ruby
    source "https://rubygems.org"

    gem "github-pages", group: :jekyll_plugins

    # If you have any plugins, put them here!
    group :jekyll_plugins do
    gem "jekyll-feed"
    gem "jekyll-include-cache"
    end

    # Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
    # and associated library.
    platforms :mingw, :x64_mingw, :mswin, :jruby do
    gem "tzinfo", "~> 1.2"
    gem "tzinfo-data"
    end

    # Performance-booster for watching directories on Windows
    gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]


    gem "webrick", "~> 1.7"
    ```

4. Delete the `Gemfile.lock` file. This is necessary because this file freezes all gems at versions that are incompatible. 

5. Update bundler, which creates a new `Gemfile.lock`.

    ```bash
    bundle update
    ```

6. Now start the local server

    ```bash
    bundle exec jekyll serve
    ```
    
