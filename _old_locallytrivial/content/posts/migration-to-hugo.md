---
title: "Migration to Hugo"
date: 2021-10-23
draft: false
categories:
- Code
tags:
- Hugo
- Blog Stack
---

## Motivation

With the ebb of workload from first year graduate courses, I decided to [rekindle the blog](/posts/reboot) that I had setup a couple years 
prior. As a former professional Python developer, I had chosen Pelican as my static site generator (SSG) in late 2018. The set of plugins
looked sufficient, and I could always write my own if needed. Fast-forward to this week, and the situation is different. The 
Hugo community has grown, broadened, and supported more use cases. In fact, some of the research groups I work with have
switched to Hugo, and more intend to! It seemed opportune to migrate; here we are. 

### My Use Case

It's only fair that I provide some detail on my uses of both Pelican and Hugo. As my content focuses on [mathematical physics](/posts/ftw-20210919)
and [related code](/posts/setup-guide), I often find the need to write $\LaTeX$ equations, include figures, reference sources[^1], and create 
`code blocks`. Occasionally I also reference static files, such as PDF documents. I aspire to better integration with social
media services, but this isn't a primary concern. If your workflow is like mine, hopefully you will find some use herein.

### Why not Pelican?

Pelican was a fine choice a few years ago, and is still not a bad choice today. I was surprised, however, to find that not 
many of the Pelican plugins, themes, or features had been updated since I originally created the blog! Cue the 
decaying-maintenance theme music.

Further, the distant memory of difficulty from setting up the blog was amplified during the reboot process. Pelican had updated some internals,
but my theme, of course, had not. Some plugins I relied on, for example Google Analytics, had dependencies that were no 
longer supported (oath2). The list goes on. I resorted to forking copies of the now-defunt themes and plugins and upgrading 
them myself; however, I didn't have the time to polish the upgrades sufficiently for release. 

For all I've noted about stale Pelican code, I must point out that [Justin Mayer](https://github.com/justinmayer) has done
a tremendous job of keeping Pelican going. He has collected a wayward littany of Pelican themes and plugins into a 
newer, submodule based monorepo, and he has personally helped me update/maintain some plugins over the years 
(e.g. [render-math](https://github.com/pelican-plugins/render-math)). If anyone can keep Pelican moving, he can.

### Why Hugo?

Given the many choices (Jekyll, Next.js, Gatsby, etc.), why Hugo? The most touted advantage, the blazing-fast build speed,
was interesting but honestly not that persuasive for my use case *at first*. I post a couple times a week, so build times aren't 
that critical. After having worked with Hugo during the migration, I have come to deeply appreciate the ability to 
near-instantly rerender pages *automatically* after I change a file; my editing process is now substantially smoother. 

Beyond performance, the community is considerably more active than that of Pelican, with modern themes and plugins that
remain supported. This weighed above all in my decision. As mentioned above, Pelican's ease-of-use had degraded sharply in
comparison. Hugo has a massive selection of [updated themes](https://themes.gohugo.io) as well as [excellent documentation](https://gohugo.io/documentation/) 
for creating new themes.


## Migration

Let's get down to business, how did I migrate? Fortunately, the Pelican paradigm is very similar to that of Hugo. Content
is primarily contained in markdown files, one per post or page. The bulk of my migration, then, was a copy/paste! Its 
worth mentioning that for less trivial cases, Hugo has an impressive set of [migration tools](https://gohugo.io/tools/migrations/);
however, Pelican is sadly absent. 

### Things that were Easy

The migration was simple, for the most part. Since both stacks use markdown files as a primary content format, the bulk
of text remained unchanged. However, the header meta-data in the markdown files is slightly different, where a Pelican 
post would begin:
```markdown
Title: On The General Theory of Relativity
Date: 1915-11-04
Category: Physics
Tags: Gravity,Wheres-my-Nobel
Author: Albert Einstein
```
The same header in Hugo would be written slightly differently, in more conventional YAML syntax
```markdown
---
```markdown
title: On The General Theory of Relativity
date: 1915-11-04
category: 
- Physics
tags: 
- Gravity
- Wheres-my-Nobel
Author: Albert Einstein
---
```

In addition to moving the text over, carrying integrations over was easier than expected. Getting Google-Analytics
and Disqus to work on the site was as simple as putting the right identifiers in the config file. Yes, Pelican has 
similar features, but they didn't ever work seamlessly out-of-the-box for me. Hugo did.


### Things that were Not

Of the many glittering features that were an improvement in Hugo, a few took some adjustment. The first of these is 
the way in which equations are compiled. Hugo uses *KaTeX instead of LaTeX* for rendering equations, which like all
tradeoffs for speed comes with a reduced feature set. In particular, KaTeX has no `split` environment, so I had to 
rewrite a bunch of my equations ([I am not alone](https://github.com/KaTeX/KaTeX/issues/1345)). For instance, was was
```markdown
$$\begin{split} a &= b \\ &= d\end{split}$$
```
Had to now be rewritten in suboptimal latex as
```markdown
$$\begin{aligned} a &= b \\\\ &= d\\\\ \end{aligned}$$
```
I fully anticipate to run into other nuances of KaTeX v. LaTeX; however, I must also note that KaTeX loads remarkably 
faster than MathJax. My site performance on mobile had suffered due to equation rendering before, but now it is blazing
fast!

Another item missing from the Hugo ecosystem is the ability to use BibTex to make citations in posts, analogous to the 
Pelican plugin [pelican-cite](https://github.com/VorpalBlade/pelican-cite). This means that I now must use manual
footnote placements. Fortunately I use Zotero, so I can easily export formatted citations to the clipboard, which minimizes
the time required. As an example, what was 
```markdown
Cite a source. [@bibtexSourceKey]
```
Has now become
```markdown
Cite a source. [^1]
...
[^1]: Fully formatted citation text, 1905.
```
This is admittedly a minor inconvenience, and perhaps a good starting point for a new Hugo plugin, now 
that GO [supports them](https://pkg.go.dev/plugin).


## My Workflow with Hugo

Where did I land after all this migration? I have a stable, (subjectively) clean workflow for adding content with Hugo,
managed entirely through GitHub actions and pages. For a full description of this workflow see [Standup, Hugo!](/posts/standup-hugo)

### GitHub Actions

As mentioned, I use GitHub Actions to build and deploy this blog. Specifically I have a workflow configuration that
relies on a protected `main` branch and Pull Requests, which is [fairly standard](https://guides.github.com/introduction/flow/).
When I make a new post, I make a feature branch in this repo, push it, and make a Pull Request. The workflow builds
the site to confirm I haven't broken anything, but doesn't deploy until after the Pull Request is merged. This was 
near-trivial to setup, thanks to the GitHub action marketplace, specifically the `peaceiris/actions-hugo` action, 
which sets up a Hugo-friendly build environment. I've included the full workflow YAML below:

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

### GitHub Pages

I use [GitHub Pages]() to host this blog, since it is free for public accounts and generally reliable. The Hugo Documentation
has a nice guide for [setting up GitHub Pages with Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/), 
making this part of the process easy.

### Sample PR for this Post

As an explicit example, [here](https://github.com/locallytrivial/locallytrivial.github.io/pull/7) is the Pull Request for this blog post. If this isn't an exercise in meta-programming
it certainly borders on it.


## TL;DR

Hugo is fast, clean, and modern. There is a slightly-higher learning curve than the Pelican+Jinja stack, but the tradeoff
of dramatically improved maintainer and community activity has been worth it for me. I ultimately intend to write Hugo
extensions, and will have more to say then about extensibility. Until then, if [read my explainer](/posts/standup-hugo) if you want to 
standup a blog like this, or leave a comment below.


## References

[^1]: Sample footnote reference


