---
layout: post
title:  "Welcome to Jekyll!"
date:   2023-04-20 05:00:10 -0500
categories: jekyll update
---

This blog has been created for `Github Pages` using `Docker` and `Jekyll`. Mostly just to show off, but also because I try not to ever install anything on my system. It made the process a bit more complicated but I thought sharing the learning experience would make a good first post.

The system in this project is an Ubuntu 22.04.1. First lets install Docker.

Script:
{% highlight bash %}
####    Install, configure Docker, Docker-Compose on Ubuntu 22.04.1

##  Installs
sudo apt install docker docker.io docker-doc docker-compose --yes

##  Start, On-Demand
# sudo systemctl start docker.service
# sudo systemctl enable --now docker.service
# sudo systemctl stop docker.service
# sudo systemctl disable docker.service

##  Run as user
# sudo usermod -aG docker $USER
{% endhighlight %}

The script should explain itself. There's a command that installs `docker` from Ubuntu apt with documentation `docker-doc` and with `docker-compose` for easier local deployment later, if needed. The commented parts make a note on how to start `Docker` on demand, instead of having it run all the time, and how to use `Docker` without `sudo`.

Next with `Docker` installed it was time to deploy this blog.

Script:
{% highlight bash %}
####    Jekyll-Docker by https://github.com/envygeeks/jekyll-docker/blob/master/README.md

##  Create site
export site_name="tkafcode.github.io"
docker run --rm \
  --volume="$PWD:/srv/jekyll" \
  -it jekyll/jekyll:latest \
  sh -c "chown -R jekyll /usr/gem/ && jekyll new $site_name" \
  && cd $site_name \
  && docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll:latest bundle add webrick \
  && docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll:latest bundle update

##  Serve
# cd $site_name && docker run --volume="$PWD:/srv/jekyll" -p 4000:4000 -it jekyll/jekyll:latest jekyll serve --watch --drafts
{% endhighlight %}

I try to use naming and comments in my code that make the code easy to read, preferably so, that if I someday forget what it was about, the code explains itself. Anyway, here I first created a `Github Pages` site on `Github`. Then I cloned it with `git clone` and copied the `.git` folder in it. Next I ran the above script and pasted the `.git` folder in to the created site folder. The first line of commands names the created Jekyll-site with the name of my `Github Pages` site, second long command creates the site.

In the second command a lot of `Docker` options are given, like usual. Later those can be simplified with `docker-compose`. In the command a volume is created with `--volume="$PWD:/srv/jekyll" \`, an image is pulled with `-it jekyll/jekyll:latest \` and then the site is created with `sh -c "chown -R jekyll /usr/gem/ && jekyll new $site_name" \`. The remaining commands add configurations to the site.

With `&& docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll:latest bundle add webrick \` the missing dependency `webrick` is added and with `&& docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll:latest bundle update` the site dependencies are updated just in case. The dependencies are updated pretty much after every operation that `bundle` does, and the update is unnecessary, yet doesn't do any harm either.

The commented part in the script is about serving the  site locally. Running the command will set up the site at localhost:4000.

From there on it was time to write and then push to `Github`.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. The first post template was created with the site and all I had to do was edit it, this.

Jekyll offers support for code snippets, as so:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

And also `bash` from the shell scripts has formating options.

But now to publishing the site on Github.

After a short check on how the site looks in the browser and correcting a lot of typos it was time to get pushing to `Github`. 

{% highlight bash %}
git add --all
git commit "Site first commit."
git push -u origin main
{% endhighlight %}

There. This is it then.

All that's left, says Jekyll, is to include the necessary front matter. Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
