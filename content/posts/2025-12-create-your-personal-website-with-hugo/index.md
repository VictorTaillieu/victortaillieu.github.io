---
title: "Create your personal website with Hugo"
summary: "Quickly and easily generate a static website deployed on GitHub Pages"
date: 2025-12-02
tags:
  - website
  - hugo
  - github pages
---

## Introduction

Creating a personal website is a great way to showcase your projects, share something you learned, and introduce yourself to potential recruiters. These are exactly the reasons that motivated me to do it. In this guide, we will walk you through the steps to create your own website using [Hugo](https://gohugo.io/), a popular static site generator, and deploy it on [GitHub Pages](https://docs.github.com/en/pages) for free hosting.

## Why Hugo?

When I was looking for a framework to generate my personal website, I initially tried [Jekyll](https://jekyllrb.com/), written in Ruby. It is considered the most popular static site generator and is natively supported by GitHub Pages. However, I wanted something faster and more flexible. After some research, I came across Hugo, written in Go.

Here are the reasons that convinced me to choose Hugo:
- **Markdown Support**: Hugo uses [Markdown](https://www.markdownguide.org/) files for content, making it easy and convenient to add content to your site.
- **Speed**: Hugo is known for its incredible speed, making it fast to build and very responsive.
- **Flexibility**: Hugo offers a wide range of [themes](https://themes.gohugo.io/) and customization options, allowing you to create a unique website that reflects your personal style. Especially, it includes the [Blowfish theme](https://blowfish.page/) that we will use in this guide, which provides a modern and clean design.
- **Built-in Features**: Hugo comes with rich built-in features like multilingual support, site search, light/dark mode, table of contents, and more, which can enhance the user experience of your website.

## Prerequisites

Before we start, make sure you have **Hugo** and **Git** installed on your machine. If not, you can check the instructions below:

- [Install Hugo](https://gohugo.io/installation/)
- [Install Git](https://git-scm.com/install/)

You should be familiar with the use of the command line and basic Git commands.

You will also need a **GitHub account** to host your website. If you don't have one, you can [sign up](https://github.com/signup) for free.

## Setup

### Create the site

Initializing your Hugo site with the Blowfish theme only requires a few commands:

```bash {noClasses=false linenos=false}
hugo new site mywebsite # replace 'mywebsite' with your desired site name
cd mywebsite
git init
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
rm hugo.toml
cp -r themes/blowfish/config .
sed -i '/^# theme/s/^# //' config/_default/hugo.toml
```

You can now view your site (empty for now) at [localhost:1313](http://localhost:1313/) after running:

```bash {noClasses=false linenos=false}
hugo server
```

![](empty-blowfish.png "Blowfish empty site")

### Upload to GitHub

First, create a `.gitignore` file to avoid pushing generated files by Hugo:

```bash {noClasses=false linenos=false}
# .gitignore

/public/
/resources/_gen/
.hugo_build.lock
```

Next, create a new repository on GitHub named `myusername.github.io`, replacing `myusername` with your actual GitHub username. Follow the instructions provided by GitHub to push your local Hugo site to the newly created repository:

```bash {noClasses=false linenos=false}
git remote add origin https://github.com/myusername/myusername.github.io.git # or git@github.com:myusername/myusername.github.io.git if you use SSH
git branch -M main
git add .
git commit -m "Initial commit"
git push -u origin main
```

The repository is now ready! We will come back later to it to [deploy the site](#deploying-the-website).

## Configuration

### Basic settings

Before adding content, you should set a few configurations to personalize your site. Here are the main files and settings you may want to modify:

```toml {noClasses=false linenos=false}
# config/_default/hugo.toml

baseURL = "https://myusername.github.io/"
defaultContentLanguage = "en"
```

The `baseURL` points to the URL where your site will be hosted. Again, replace `myusername` with your actual GitHub username. This domain is the one provided by GitHub for free. The `defaultContentLanguage` sets the default language of your site. We will see later how to [add other languages](#supporting-multiple-languages) if needed.

```toml {noClasses=false linenos=false}
# config/_default/languages.en.toml

title = "My Personal Website"

[params.author]
  name = "My Name"
  image = "img/author.jpg"
  headline = "Software Developer"
  links = [
    { email = "mailto:myemail@gmail.com" },
    { github = "https://github.com/myusername" },
    { linkedin = "https://www.linkedin.com/in/myusername/" }
  ]
```

Here, we set the title of the website and provide some information about the author. Make sure to replace the values with your own information. The image should be placed in the `assets/` folder.

```toml {noClasses=false linenos=false}
# config/_default/params.toml

colorScheme = "blowfish"
```

Blowfish proposes multiple color schemes out of the box. Possible values are: `blowfish` (default), `avocado`, `fire`, `ocean`, `forest`, `princess`, `neon`, `bloody`, `terminal`, `marvel`, `noir`, `autumn`, `congo`, `slate`.

```toml {noClasses=false linenos=false}
# config/_default/menus.toml

[[main]]
  name = "Projects"
  pageRef = "projects"
  weight = 10

[[main]]
  name = "Blog"
  pageRef = "posts"
  weight = 20

[[main]]
  name = "CV"
  pageRef = "cv"
  weight = 30
```

This file defines the different menu sections of your website. The `pageRef` attribute corresponds to the URL path of the section. The `weight` attribute determines the order of the menu items in the navigation bar.

### Personalization options

Blowfish offers many options to customize your website further. You can for example enable the table of contents and the zen mode for articles with the following settings:

```toml {noClasses=false linenos=false}
# config/_default/params.toml

[article]
  showTableOfContents = true
  showZenMode = true
```

Feel free to explore all the available options in the [Blowfish documentation](https://blowfish.page/docs/configuration/).

## Managing content

### Adding pages

Now that the structure of your website is in place, you can start adding content. Hugo uses Markdown files to edit pages which makes it easy and fast to write.

To add some text on your homepage, simply create a `content/_index.md` file:

```markdown {noClasses=false linenos=false}
# content/_index.md

---
title: "Homepage"
description: "About me"
---
Hello! This is my personal website where I share my projects and blog posts.
Feel free to explore and contact me if you have any question!
```

Now, let's say that you want to add a blog article. It is as simple as creating a new folder containing an `index.md` file inside the `content/posts/` directory:

```markdown {noClasses=false linenos=false}
# content/posts/post1/index.md

---
title: "Create your personal website with Hugo"
summary: "Quickly and easily generate a static website deployed on GitHub Pages"
date: 2025-12-02
---

In this article, we will walk you through the steps to create your own website using [Hugo](https://gohugo.io/), a popular static site generator, and deploy it on [GitHub Pages](https://docs.github.com/en/pages).

![](illustration.png "Hugo Website")
```

Note that if you want to add an image to your post, you can place it under the post folder and reference it in the Markdown file.

You can follow the same approach to add other sections like a projects page or a CV page by creating the corresponding Markdown files in the `content/` directory.

### Supporting multiple languages

Hugo makes it easy to create a multilingual website. To add support for another language, you need to create new configuration files under the `config/_default/` directory. For example, to add French support, create `languages.fr.toml` and `menus.fr.toml` files.

Then, for each content file, you need to create a corresponding file for the new language. For example, to translate the homepage, create a `content/_index.fr.md` file.

### Files organization

Here is what the structure of your Hugo website might look like:

```
mywebsite/
├── assets/
│   └── img/
│       └── author.jpg
├── config/
│   └── _default/
├── content/
│   ├── _index.md
│   ├── _index.fr.md
│   └── posts/
│       └── post1/
│           ├── illustration.png
│           ├── index.md
│           └── index.fr.md
├── themes/
│   └── blowfish/
└── .gitignore
```

## Deploying the website

The final step is to deploy your website so that it is accessible online for everyone! To do this, we will use GitHub Actions, a continuous integration and deployment service provided by GitHub.

First, go to the settings of your repository on GitHub, navigate to the `Pages` section, and select `Github Actions` as the source.

Create a `.github/workflows/gh-pages.yml` file in your repository with the following content:

```yaml {noClasses=false linenos=false}
# .github/workflows/gh-pages.yml

name: Deploy Hugo to GitHub Pages

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-24.04
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"

      - name: Build site
        run: hugo --minify

      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-24.04
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Commit and push your local changes to your GitHub repository. The GitHub Actions workflow will automatically build your site and deploy it to GitHub Pages whenever you push changes to the `main` branch.

After a few moments, your website should be live at `https://myusername.github.io/`, replacing `myusername` with your actual GitHub username.

## Conclusion

Congratulations! You have successfully created and deployed your personal website using Hugo and GitHub Pages. You can now continue adding more content, customizing the design, and sharing your website over your social networks.

## References

- [Hugo documentation](https://gohugo.io/documentation/)
- [Blowfish documentation](https://blowfish.page/)
- [Blowfish tutorial from the creator of the theme](https://n9o.xyz/posts/202310-blowfish-tutorial/)
- [List of static site generators](https://jamstack.org/generators/)
