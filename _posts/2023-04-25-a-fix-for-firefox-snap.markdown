---
layout: post
title:  "A fix for Firefox Snap on Ubuntu 22.04LTS"
date:   2023-04-25
categories: jekyll update
---

As anyone using `Ubuntu 22.04LTS` `Firefox` must have noticed, the `Snap` install crashes frequently, effectively destroying the `Firefox` installation entirely. The current recommended work-around to the issue is to install `Firefox` from the `Mozilla` repository instead of `Snap`. In this blog I'll tell you how I did it.

Script:
{% highlight bash %}
####    Some way to install a working Firefox on Ubuntu 22.04.2

## Remove and purge firefox-snap
sudo snap disable firefox
sudo snap remove --purge firefox

## Add Mozilla ppa for Ubuntu
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys A6DCF7707EBC211F
sudo apt-add-repository "deb http://ppa.launchpad.net/ubuntu-mozilla-security/ppa/ubuntu jammy main" --yes
sudo add-apt-repository ppa:mozillateam/ppa --yes
sudo add-apt-repository ppa:ubuntu-mozilla-daily/ppa --yes


## Search for the version you want to install from the Ubuntu builds, or grep,
#  replace version info in command and install
#  https://launchpad.net/~ubuntu-mozilla-daily/+archive/ubuntu/ppa
#  sudo apt search firefox |grep build

sudo apt update; sudo apt upgrade -y; sudo apt dist-upgrade -y; sudo apt autoremove -y; sudo apt autoclean -y
sudo apt install firefox-esr=102.10.0esr+build1-0ubuntu0.22.04.1 --yes

## Exit script
echo "Happy hunting!"
return 2> /dev/null; exit

Sum addons
https://addons.mozilla.org/en-US/firefox/addon/privacy-badger17/
https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search
https://addons.mozilla.org/en-US/firefox/addon/darkreader/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=featured
https://addons.mozilla.org/en-US/firefox/addon/dynamic-mountains-theme/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search

{% endhighlight %}

The script should explain itself. It starts with disabling the broken Snap and removing it completely. Then the Mozilla repo is added. From there on it installs a Firefox ESR. The remainder of the script are a fancy exit before notes of addons I like to use. The addons are in notes because Firefox doesn't allow scripting addons. They need to be added manually.

You can download the script here and run it after chmod +x as is. 

[Script on Github](https://github.com/tkafcode/Ubuntu22.04-Firefox-Snap-Work-Around)

If you would like another version of Firefox, just follow the instructions in the script. Likely you'll have to change the version eventually, as the versions in the repo update frequently.
