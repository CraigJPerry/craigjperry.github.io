title: Hexo with Github Pages
date: 2015-10-15 23:06:31
tags:
---
There are 3 steps to configuring the [hexo](https://hexo.io/) static site generator to use a custom domain while hosting on [Github Pages](https://pages.github.com/):

1.  Setup the git repository
2.  Configure & deploy hexo
3.  Configure DNS

## Create A Git Repo

Create an empty repo named _your-github-username.github.io_ on the Github site.  Next, create a git repository to upload there:

{% codeblock lang:bash %}
[user@host ~]$ git init github-username.github.io
[user@host ~]$ cd github-username.github.io
[user@host ~]$ touch .gitignore
[user@host ~]$ git add .gitignore
[user@host ~]$ git commit 'Initial Commit'
[user@host ~]$ git remote add origin ssh://git@github.com:Your-Github-Username/your-github-username.github.io.git
{% endcodeblock %}

There will be 2 branches in the repo:

*  master: hosts your generated static html / css etc.
*  blog-setup: the name of this branch is unimportant, but it is an orphan branch and exists in parallel to master. The two are never merged. All your commits will be on this branch.

{% codeblock lang:bash %}
[user@host ~]$ git checkout --orphan blog-setup
[user@host ~]$ git rm -rf *
[user@host ~]$ cat - > .gitignore <<EOF
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
EOF
[user@host ~]$ git add .gitignore
[user@host ~]$ git commit -m 'Initial commit of blog setup branch'
[user@host ~]$ git push -u origin blog-setup
{% endcodeblock %}


