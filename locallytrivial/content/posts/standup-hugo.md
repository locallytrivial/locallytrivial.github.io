---
title: "Standup, Hugo! Creating a Blog with Hugo"
date: 2021-10-23
draft: false
categories:
- Code
tags:
- Hugo
- Blog Stack
---

## Preliminaries

Let's quickly review what you'll need to build a Hugo blog, and what it will mean to *have* a Hugo blog at the end of the process.

### What You'll Need
Before we begin, you'll need a few things. I'll list the essentials below (as well as my recommendations where relevant)

- [Git](https://git-scm.com), the ubiquitous VCS of the era
- [GitHub](https://github.com) account, for storing your blog content (and hosting for free!)
- Text Editor, for writing content and editing configuration files (I recommend either [Sublime Text](https://www.sublimetext.com) for simple uses or [GoLand](https://www.jetbrains.com/go/) for those who also need to edit GO code for plugins)

### What You'll Have

By the end of this post, the hope is that you will have:
- A fully-functional, clean, and simple blog 
- Hosted for free on GitHub Pages
- A workflow for adding new content and automatically building your website 

Let's (hu)go!


## Getting Started with Hugo

The good news: Hugo has excellent [getting started documentation](https://gohugo.io/getting-started/quick-start/). My goal
is to guide you through the process quickly, filling in any gaps.

### Installing Hugo

A convenient detail about using Hugo is that is comes as a single binary, there is no complex *environment* to maintain! 
Nonetheless, we must install Hugo. On Macos, this is done by
```bash
brew install hugo
```
See the [installation docs](https://gohugo.io/getting-started/installing) for other operating systems.

### Creating the Repository

The blog needs a place to live - [create a GitHub Repo](https://docs.github.com/en/get-started/quickstart/create-a-repo) to 
store the code. **Note**: if you intend to host with GitHub Pages as is done here, the name of your repository must be
`<username>.github.io` for users or `<organization>.github.io` for organizations. Clone your repo! We use ssh below, 
see [GitHub SSH Key Guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) 
for more detail if you need to setup SSH keys first.
```bash
git clone git@github.com:<username>/<username>.github.io.git
```
Once the repo is cloned locally, change directory to it
```bash
cd /path/to/my/repo
```
Now run the Hugo command for creating a new site, with a specific name `<sitename>` for the output folder to be created
in the repo
```bash
hugo new site <sitename>
```
This is a good place to commit and push all changes!

### Preliminary Content

We need to add a post before rendering the site! We'll create a post called `my-first-post` using the Hugo command `new`.
First make sure you're in the site directory
```bash
cd /path/to/my/repo/<sitename>
```

Now create the post markdown file
```bash
hugo new posts/my-first-post.md
```

Or, equivalently, create a markdown file at `/path/to/my/repo/<sitename>/content/posts/my-first-post.md` that looks like:
```markdown
---
title: "Hello, World!"
date: 2021-10-23
draft: false
---

Sample post content
```


### Theme

Before we can build the site, we'll need to choose a theme. This choice is important since not all themes have the same 
set of configuration options. Choose your theme from the [vast selection](https://themes.gohugo.io) and then add it as a
[git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules). For this demo we'll use the [Coder Theme](https://github.com/luizdepra/hugo-coder),
on which this site's theme is based.
```bash
cd /path/to/my/repo/<sitename>
git submodule add https://github.com/luizdepra/hugo-coder.git themes/coder
```

Now we need to specify the theme in the Hugo config file, which is usually located `/path/to/my/repo/<sitename>/config.toml`.
Use your text editor of choice to add the following:
```toml
theme = "coder"
```
Where you can replace `"coder"` with the name of the theme you chose, if you chose differently. Now the theme should be ready
for build! Note that you will need to commit the submodule just like other changes to the repo.

### Building

Let's build the site and serve it:
```bash
hugo server -D
```
The output will tell you where it is being served, likely `localhost:1313`. Now you've built the basics!

### Adding a Non-post Page

The documentation is a little thin on this, but many of us would like to have an "About" page or some other page
that has non-post content. The process for creating these is very easy! Let's use the "About" page as an example. Create
a file for it at location `/path/to/my/repo/<sitename>/content/about.md` with header and content:

```markdown
---
title: "About"
date: 2021-10-23
draft: false
---

I am a Hugo-powered blogger!
```

Once you save the file, you should notice that Hugo has detected a change, and automatically rebuilt the site! We should
add the page to the menu to make it easy to find, which is done in the config file. **Note**: menu items should be standard across themes; 
however, check with the documentation of your specific theme if you encounter any rendering oddities. In the config file we'll
add (or modify if exists):

```toml
[menu]
  [[menu.main]]
    identifier = "about"
    name = "About"
    url = "/about/"
```

If you want to add other pages to the menu, this is accomplished by adding another `[[menu.main]]` item. Once Hugo detects
this change and reruns, you should be able to see the "About" menu item and the corresponding page by clicking it.

## Workflow and Deployment

Now you have a functioning Hugo project, let's setup some bells and whistles to get this content online. We will first
setup GitHub actions that will build and deploy the site for you, then we will configure GitHub pages. See the 
[Hugo docs](https://gohugo.io/hosting-and-deployment/hosting-on-github/) for more details.

### GitHub Action

[GitHub Actions](https://docs.github.com/en/actions) offer a wide array of automation and workflow benefits. Today, we'll
setup a workflow that checks for new commits on main and from pull requests, sets up a Hugo environment, and builds your site.
The configuration below is also set such that only after you *merge* the pull request will the site be deployed. A full example 
is included at the end of this post.

Next we need to create a YAML file for this workflow at the following location `/path/to/my/repo/.github/workflows/gh-pages.yml`, 
and configure it to match the below:
```yaml
name: GitHub Pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.88.1'
          extended: true
      - name: Build
        run: hugo --minify --source=locallytrivial
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./locallytrivial/public
          cname: locallytrivial.com
```

The above workflow uses the `actions/checkout` action to clone your repo content, the `peaceiris/actions-hugo` action
to setup the necessary Hugo dependencies, and the `peaceiris/actions-gh-pages` action to deploy the site (only on the 
`main` branch, which is where commits go after Pull Request is merged).

### GitHub Pages

To setup GitHub pages, you'll go to the GitHub Pages section of your repo settings 
`https://github.com/<username>/<username>.github.io/settings/pages`, and make sure the "Source" branch is set to 
`gh-pages`. This will only be available after you have pushed to master (or merged your first pulln request). You can 
optionally set a custom domain name here as well! (That step might require editing DNS configurations from your domain
host, e.g. [domains.google.com](domains.google.com))

## Example Usage: Making a Post

Now you should be all-ready to make a post and deploy the site. This example gives each workflow step to make a sample
post titled, "Standup, Hugo!".

### Create a Feature Branch

The first step in a Pull Request workflow is to create a feature branch. See [GitHub docs](https://guides.github.com/introduction/flow/) 
for more on git flow. Make sure you're on latest main branch

```bash
git checkout main
git pull
```

Now create a branch, with a somewhat descriptive name

```bash
git checkout -b post-standup-hugo
```

### Create the Post

Create a new Markdown document for the post `/path/to/my/repo/<sitename>/content/posts/standup-hugo.md` with content:

```markdown
---
title: "Standup, Hugo!"
date: 2021-10-23
draft: false
categories:
- Code
tags:
- Hugo
- Blog Stack
---

This is a post I created using the GitHub workflow!
```

### Commit, Push, Create Pull Request

Commit and push these changes
```bash
git add /path/to/my/repo/<sitename>/content/posts/standup-hugo.md
git commit . -m "commiting new post"
git push
```

Now navigate to the GitHub page for your repo and find the branches page: `https://github.com/<username>/<username>.github.io/branches`. 
Find the `post-standup-hugp` branch and click "New Pull Request". Enter a somewhat descriptive title and 
description, then click "Create Pull Request". 

### Merge and Deploy (with a button!)

Wait for the build suite to pass, then click "Merge Pull Request". Once it is merged, the build suite on the `main` branch
will deploy your code to the `gh-pages` branch! If you do not see your website at `https://<username>.github.io` make
sure to double check that the Source branch is set to `gh-pages` in the repository settings > Pages.

Sit back, and enjoy. You've stood up a Hugo blog!




