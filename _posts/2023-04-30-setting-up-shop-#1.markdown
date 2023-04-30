---
layout: post
title:  "Setting up shop #1"
date:   2023-04-30
categories: shop
---

I know you've been anxiously waiting since my first post what's he going to do with the `docker-compose` he installed. Well, now's the time to take it into use. This post is one of a series where I'm going to blog about setting up a web shop. 

For a web shop there are many `services` as they are called in `Docker`, and that's where `docker-compose` comes in. It's going to run the web server, encryption, php, database, and the web shop app itself from one configuration file. What the app will be, will come later. Much later. This blog is humbly about `Docker`and the web server `Nginx`. Logically the next post would be about `PHP` and the app, but we'll see. For what I know, it's never that straight forward with security critical applications. I bet 'ya there's going to be trouble with configuring the ports right and looking back at this post I'm going to think I must have been stupid to think it was going to be fine.

Let's get to business. 

Firstly `Docker Compose` needs the configuration file called `docker-compose.yml` (or.yaml). It looks something like this:

{% highlight yaml %}
version: "3.9"                          # Docker Compose File version
services:
    nginx:
        container_name: nginx
        restart: unless-stopped
        image: nginx
        ports:
            - 80:80
            - 443:443
        volumes:
            - ./nginx/nginx.conf:/etc/nginx/nginx.conf
#    php:
#        build: .                       # Needs a build for pdo etc installation
#    database:
#    volumes:
#    tkaf_app:                          # Needs a build scalable for hosting
#        build: .

{% endhighlight %}

The first line is what `Docker Compose` version is going to be used. At the end of the version you see an `yml` style `inline comment`. `Docker Compose` and `docker-compose` are the same thing. `Docker Compose` being the tool that actually runs the multi-container application, and `docker-compose` the name of the install and the command. You can refer to the links below on what the versions are:

https://docs.docker.com/compose/
https://docs.docker.com/compose/compose-file/compose-file-v3/

From the second line start the `services`. The idea is that all parts of the application run in their own isolated container. Starting with the web server. The commented lines are what's planned next; all `services`. 

In the `services` block the first line is `nginx`. As you see, the `yml` syntax adheres to basic `xml` syntax with indentation. 

In the `nginx` block the first line, in this case,  is the name for the container. It's unneccessary, but good for clarity. With a declared name you'll see the containers name with `docker ps`, not just the containers UUID. The next line tells the container to keep running until stopped so it doesn't need to be restarted manually if something goes wrong with later deployment. Also unneccessary, but good.

Then the third line declares the `Docker` `image` to be used. In this case it's named just `nginx`, but a version could be added like `nginx:latest` where it would use the most recent version. 

Then the ports. This should be very, very simple, but seems not. Port 80 is where `nginx` has basic `http` access to the world, and port 443 where it gets encypted `https` access to the world. The first declaration in `80:80` is the system port, and the latter the container port. Same with `443:443`. And would they work as they are? No. You have to configure them like, say, 10080:80, 10443:443, because otherwise they conflict. More on that later once building accreditation with SSL.

The `volumes` line declares what `Docker` mounts. In this case it only mounts a configuration file. For later the project `docker-compose.yml` is in a folder called `build`. There `nginx` has it's own folder called `nginx` and there is firstly the configuration file for `nginx` called `nginx.conf`. The configuration file is then mounted in the container as `/etc/nginx/nginx.conf`. The syntax for the mount is `./nginx/nginx.conf:/etc/nginx/nginx.conf` where `./nginx/nginx.conf` is where from on the system and `/etc/nginx/nginx.conf` where to in the container. More on the mount syntax later.

That's should be all for now. With the above you get a running containerized and secure web server ready for encyption accreditation. 

But why not Vagrant, if later PHP and what not? That too, later.