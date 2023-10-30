---
title: "Setup jekyll environment and preview locally."
categories:
  - Tech
tags:
  - jekyll
  - ruby
layout: single
---
# Setup jekyll

Github page leverages jekyll to generate static web pages from a github, hence we need to undersand it and setup the environment.
Below instructions tested on Ubuntu 22, other version or Linux kernel may vary.

## Install Ruby

jeklly is built upon Ruby, hence we need to setup a ruby environment.

Note: In addition to below methods, this [article](https://www.ruby-lang.org/en/documentation/installation/) provides several alternative way of installing ruby.

### Install rbenv

Latest jeklly requires Ruby 3.x, but the package manager on my Ubuntu doesn't have it. Hence I have to install one using a Ruby package manager. Fortunately there's existing one on Ubuntu.

```shell
apt-get install rbenv
```

### Install ruby-build

rben itself is not a package manager, the function is provided through a plugin ruby-build.

```shell
apt-get install ruby-build # Installed ruby-build doesn't work.
```

However, ruby-build installed by apt doesn't work. Hence I followed the user guide on [github ruby-build](https://github.com/rbenv/ruby-build) and installed mannually.

```shell
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
git -C "$(rbenv root)"/plugins/ruby-build pull
```

Verify the installation.

```shell
~rbenv commands |grep install
install
uninstall
```

### Install required ruby version

Install and active Ruby.

```shell
rbenv install 3.2.2 # Latest version
rbenv global 3.2.2
ruby --version
# Similar information should be displayed.
# ruby 3.0.0p0 (2020-12-25 revision 95aff21468) [x86_64-linux]
```

## Install Jekyll

[Offical guide](https://jekyllrb.com/docs/installation/ubuntu/)
Now Ruby is ready, let's get started on Jekyll.
First make sure the prerequistes are ready.

```
apt-get install ruby-full build-essential zlib1g-dev
```

Then install Jekyll using gem.

```
gem install jekyll bundler
```

Verify the installation.

```
jekyll --version
# jekyll 4.3.2
```

Done.

# Intialize a github repo with Jekyll

[Github Guide](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll).

## Intialize a jekyll site

```shell
cd /path/to/where/your/site/is/pubshed/
jekyll new --skip-bundle .  # create a jekyll site in current directory
```

A Gemfile as well as other files should be created. Open and edit the Gemfile. Two key changes needs to made to satisfy github.

```shell
# Gemfile
...
# comment below line to disable default jekyll theme.
# gem "jekyll", "~> 4.3.2"
# add below line to enable github page, you can change 228 to any supported github page version, check https://pages.github.com/versions/
gem "github-pages", "~> 228",  group: :jekyll_plugins
```

Save and close.

```shell
bundle install
```

## Test your site locally

It's better to test your site locally before submit it to github to improve efficiency, it takes quite a while before your github *push* taking effect.

```shell
bundle exec jekyll serve
```

And then visit it on http://localhost:4000
