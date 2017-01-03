---
layout: post
title:  "How to post your blog post"
author: Rebecca Barter, Kellie Ottoboni
categories: [introduction]
tags: [publishing, blog posts]
---



So... how does one get their blog post on here? That's an excellent question and I'm so glad that you asked! 

It's quite easy, really (especially if you avoid the advanced steps which will require you to spend some time pulling out your hair trying to understand how jekyll, github pages, and R Markdown interact with one another).

All you need to do is follow the so-simple-and-easy-to-implement steps below! Hooray!




## Step 1: Fork the github repository

First thing's first, you need to **fork the git repo** on which this site is hosted: [https://github.com/rlbarter/Practical-Statistics](https://github.com/rlbarter/Practical-Statistics).
Go to this repository and in the top righthand corner, click the Fork button.
This creates your own copy of the repo in your GitHub account.

Now, you need to copy the repo onto your local machine.
Basically you will write the following code in the **command line** i.e. terminal (assuming that you've installed git at some point in your life):



{% highlight bash %}
git clone https://github.com/YOUR-USERNAME/Practical-Statistics
{% endhighlight %}

Notice that you want to clone **your fork** of the repo, not the original repo.
The clone operation only pulls the master branch of the repo, but the website lives on the gh-pages branch.
To set that up, you would run the following commands:


{% highlight bash %}
git pull origin gh-pages
git checkout -b NAME-OF-NEW-BRANCH
{% endhighlight %}
  
The first line pulls the gh-pages branch from your fork on GitHub to your local machine.
The second line creates a new branch on your local machine (`git checkout` is for switching branches and the `-b` flag says to create a new branch).
You need to **make your changes on a new branch**; the gh-pages branch won't work.
Now you should have all the files necessary.

## Step 2: Write your blog post using R Markdown

Write your blog post in RStudio as an R Markdown file, and save your `.Rmd` file in the `/_source` folder (which lives on the `gh_pages` branch).
Once you're done, add and commit your blog post:


{% highlight bash %}
git add FILENAME
git commit -m "A brief message about what files you're adding"
{% endhighlight %}

## Step 3: Submit a pull request

Now that you're all done adding things, we need to merge it into the website.
We do this in two steps: you'll push it to your GitHub repo and then submit a **pull request**, asking us to pull your changes into the website.
First, push your changes to your repo:


{% highlight bash %}
git push origin NAME-OF-NEW-BRANCH
{% endhighlight %}

Now, navigate back to [the original repo](https://github.com/rlbarter/Practical-Statistics).
At the top of the repo, the new branch will show up with a green Pull Request button - click it and submit the request, **making sure that you set the base branch as gh-pages**.
Now, the maintainers will either merge your changes into the website, or may ask you to make modifications first.
If the latter happens, follow this same process of commits and pushes, and they will automatically get added to your open pull request.

## Advanced steps

If you would like to be able to render the website locally on your machine before submitting your post or any changes to the public version, you need to install [ruby](https://www.ruby-lang.org/en/downloads/) and then [jekyll](https://jekyllrb.com/docs/installation/). 

Having done this (possibly with a huge amount of frustrated hair-pulling), you can simply install the `servr` package in R:


{% highlight r %}
install.packages("servr")
{% endhighlight %}

and then run the `jekyll()` command


{% highlight r %}
servr::jekyll()
{% endhighlight %}

If this doesn't work, it make sure that you have indeed installed ruby and jekyll. Otherwise, your problem will likely be solved by specifying the path to the `jekyll build` command (found using `which jekyll` in the terminal).

For instance, in order to build and serve the website locally, I need to use the following argument:


{% highlight r %}
servr::jekyll(command = "~/.rbenv/shims/jekyll build")
{% endhighlight %}


Note that due to laziness, this site was essentially copied from Yihui Xie's [knitr-jekyll repository](https://github.com/yihui/knitr-jekyll).

## Divergence between the main repository and your fork

As the website gets updated, the main repo will change, but yours will **not** be automatically updated.
To keep yours in sync, you need to set up access to the main repo:


{% highlight bash %}
git remote add upstream https://github.com/rlbarter/Practical-Statistics
{% endhighlight %}

Before, you did pushes and pulls from your fork, which was called `origin`.
`upstream` refers to the original repo.
You could name these whatever you wanted, but `origin` and `upstream` are the conventions.
You can see where else you have push/pull access to by running git remote with the `-v` flag:


{% highlight bash %}
git remote -v
{% endhighlight %}

Now to actually update your local clone and your fork on GitHub, you'd run


{% highlight bash %}
git checkout gh-pages
git pull upstream gh-pages
git push origin gh-pages
{% endhighlight %}

The steps here are pretty straightforward: make sure you're on the right branch, pull from that branch in the main repo, and push the updated branch on your machine to your GitHub fork.
You can do this for the master branch as well.

## Deleting old branches

After you're done making a pull request, you might want to get rid of old branches that have already been merged.
You can see what branches you have by running


{% highlight bash %}
git branch -v
{% endhighlight %}

To delete a branch, first delete it locally and then delete the branch on your GitHub:


{% highlight bash %}
git branch -d OLD-BRANCH
git push origin :OLD-BRANCH
{% endhighlight %}
