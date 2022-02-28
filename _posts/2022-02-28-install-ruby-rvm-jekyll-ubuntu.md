---
title: Install Ruby, RVM, Bundler and Jekyll on Ubuntu 20.04
description: Learn how to install Ruby, RVM, Bundler and Jekyll on Ubuntu 20.04
header: Install Ruby, RVM, Bundler and Jekyll on Ubuntu 20.04
---

Learn how to install Ruby, RVM, Bundler and Jekyll on Ubuntu 20.04

Jekyll is a framework used to generate static websites and is written in Ruby. Users can host webpages using their own github repositories.  
RVM (Ruby Version Manager) is a manager used to switch among different ruby versions. It can switch from one ruby version to another ones inside your project folder without making a mess or breaking packages. It reminds the usage of PHPBrew.  
Bundler is a gem installer. It provides a consistent environment for Ruby projects by tracking and installing the exact gems and versions that are needed.  

Let's start. First of all, remove any package related to ruby:  

~~~ console
$ sudo apt remove ruby bundler jekyll --purge

$ rm -rf ~/.gem ~/.ruby ~/.rvm

$ sudo apt clean && sudo apt autoremove && sudo apt autoclean
~~~

Check for update:  

~~~ console
$ sudo apt update && sudo apt upgrade
~~~

Import the GPG key. Check out the official RVM website to get the correct key.  

~~~ console
$ gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
~~~

[https://rvm.io/rvm/install](https://rvm.io/rvm/install)  

Now install RVM. It can take some minutes. Pay attention to see if it asks for sudo password during the process:  

~~~ console
$ curl -sSL https://get.rvm.io | bash -s stable --ruby
~~~

Add RVM to the PATH of your user:  

~~~ console
$ echo 'source $HOME/.rvm/scripts/rvm' >> ~/.bashrc

$ source ~/.bashrc
~~~

To list all installed rubies:  

~~~ console
$ rvm list rubies
   ruby-2.7.2 [ x86_64 ]
=* ruby-3.0.0 [ x86_64 ]

# => - current
# =* - current && default
#  * - default
~~~

To list all installed rubies, in their ruby string representation only:  

~~~ console 
$ rvm list strings
ruby-2.7.2
ruby-3.0.0
~~~

To list all *known* RVM installable Rubies:  

~~~ console
$ rvm list known
~~~

Now, let's install Bundler and Jekyll:  

~~~ console
$ gem install bundler jekyll
~~~

**ATENTION: AVOID THE USAGE OF SUDO WHEN INSTALLING GEM**  

Now you create a basic website or clone that one from repository:  

~~~ console
$ jekyll new website
~~~

Or cloned:  

~~~ console
$ git clone https://github.com/username/userrepository.github.io.git
~~~

Go to your directory:  

~~~ console
$ cd website

bundle exec jekyll serve --trace
~~~

Check your _config.yml file to see what plugins are beeing used. Open your Gemfile and put all the names there. Then install the plugins with:  

~~~ console
$ bundle install
~~~

Execute the server:  

~~~ console
$ bundle exec jekyll serve --trace
~~~

Your website may appear at http://localhost:4000  













