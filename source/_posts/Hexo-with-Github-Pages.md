title: Hexo with Github Pages
date: 2015-10-15 23:06:31
tags:
---
There are 3 steps to configuring the [hexo](https://hexo.io/) static site generator to use a custom domain while hosting on [Github Pages](https://pages.github.com/):

1.  Create A Git Repo
2.  Configure Hexo
3.  Configure DNS

Lastly there's some notes on how to use this setup.

## Create A Git Repo

Create an empty repo named _your-github-username.github.io_ on the Github site.  Next, create a git repository to upload there:

{% codeblock lang:bash %}
[user@host ~]$ git init github-username.github.io
[user@host ~]$ cd github-username.github.io
[user@host github-username.github.io]$ touch .gitignore
[user@host github-username.github.io]$ git add .gitignore
[user@host github-username.github.io]$ git commit 'Initial Commit'
[user@host github-username.github.io]$ git remote add origin ssh://git@github.com:Your-Github-Username/your-github-username.github.io.git
{% endcodeblock %}

There will be 2 branches in the repo:

*  master: hosts your generated static html / css etc.
*  blog-setup: the name of this branch is unimportant, but it is an orphan branch and exists in parallel to master. The two are never merged. All your commits will be on this branch.

{% codeblock lang:bash %}
[user@host github-username.github.io]$ git checkout --orphan blog-setup
[user@host github-username.github.io]$ git rm -rf *
[user@host github-username.github.io]$ cat - > .gitignore <<EOF
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
EOF
[user@host github-username.github.io]$ git add .gitignore
[user@host github-username.github.io]$ git commit -m 'Initial commit of blog setup branch'
[user@host github-username.github.io]$ git push -u origin blog-setup
{% endcodeblock %}

## Configure Hexo

First i installed hexo and created a site:

{% codeblock lang:bash %}
[user@host github-username.github.io]$ npm install hexo -g
[user@host github-username.github.io]$ hexo init
[user@host github-username.github.io]$ npm install
[user@host github-username.github.io]$ hexo server
INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
{% endcodeblock %}

Visiting the URL in your browser will show the default page. Next i installed a theme i liked from [the wiki themes list](https://github.com/hexojs/hexo/wiki/Themes). I merged the theme repository as a subtree in git since in practice git submodules (the suggested method) can be quite clunky. The [Github Page](https://help.github.com/articles/about-git-subtree-merges/) on this topic is a good reference.

{% codeblock lang:bash %}
[user@host github-username.github.io]$ git remote add -f alex https://github.com/ppoffice/hexo-theme-alex.git
[user@host github-username.github.io]$ git merge -s ours --no-commit alex/master
[user@host github-username.github.io]$ git read-tree --prefix=themes/alex -u alex/master
[user@host github-username.github.io]$ cat - >> _config.yml <<EOF
theme: alex

# Overridable Alex theme setting defaults from themes/alex/_config.yml
theme_config:

  # Header
  menu:
    Home: /
    Archives: /archives
    About: /about/index.html
  rss: 
  github: https://github.com/ppoffice/hexo-theme-alex

  # Content
  excerpt_link: Read More
  fancybox: true

  # Sidebar
  sidebar: left
  widgets:
  - category
  - archive
  - recent_posts
  - tag
  - tagcloud
  - links

  # Links
  links: 
    Hexo: http://hexo.io

  # Miscellaneous
  google_analytics:
  favicon: /favicon.png
  twitter:
  google_plus:
  fb_admins:
  fb_app_id:
EOF
[user@host github-username.github.io]$ git commit -m 'Import alex theme'
{% endcodeblock %}

Next i made the site deployable to github pages. I already have an SSH key trusted by github so i will deploy my git repo by SSH.

{% codeblock lang:bash %}
[user@host github-username.github.io]$ npm install hexo-deployer-git --save
[user@host github-username.github.io]$ cat - >> _config.yml <<EOF
deploy:
  type: git
  repo: ssh://git@github.com/Your-Github-Username/your-github-username.github.io
  branch: master
EOF
{% endcodeblock %}

## Configure DNS

Firstly, Github Pages requires that we publish a file called CNAME to the root of our repository (master branch). Since hexo performs a force push to that location, if we simply added the CNAME file it would be overwritten at deployment time. There is a simple plugin available to perform this for us though:

{% codeblock lang:bash %}
[user@host github-username.github.io]$ npm install hexo-generator-cname --save
[user@host github-username.github.io]$ cat - >> _config.yml <<EOF
plugins:
- hexo-generator-cname
EOF
{% endcodeblock %}

Now when `hexo generate` is run, a CNAME file containing your domain name (configured from the url variable in _config.yml) will be created.

Lastly, you need to login to your DNS provider and create a DNS CNAME record pointing hostname *www* to *your-github-username.github.io*. This can take a few hours to replicate. 

## Using This Setup

Now that the configuration is complete, we can actually publish some posts!

### Leverging Git: A Branching Strategy

We have 2 unrelated branches in our repo: master; blog-setup. We never directly edit anything in master, that's updated for us when we run `hexo deploy`. The *blog-setup* branch contains the hexo site and i create branches off that for each of my posts:

{% codeblock lang:bash %}
[user@host github-username.github.io]$ git branch
* blog-setup
  master
[user@host github-username.github.io]$ git checkout -b my-next-post-topic
[user@host github-username.github.io]$ hexo new post "My Next Post Topic"
[user@host github-username.github.io]$ git add .
[user@host github-username.github.io]$ git commit -m "Started topic"
[user@host github-username.github.io]$ git push -u origin my-next-post-topic
{% endcodeblock %}

When i've made my last commit and i'm ready to publish, i merge the branch back to blog-setup before running `hexo generate` and `hexo deploy`. The neat thing about pushing the branch upstream is that i can work on it over a few days from different machines, using Github as both an offsite backup and a synchronisation mechanism.
