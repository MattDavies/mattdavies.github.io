---
layout: post
title: Express install Jekyll on Windows and import a Wordpress blog
---

There's a huge array of great posts covering installing Jekyll and migrating from Wordpress to Jekyll. However while reviewing these myself I didn't find anything which covered everything I needed to do, so I've put together a quick shortcut guide below. If you're here, I'll assume you've already made the decision to jump ship and just need a set of commands to run.

Pull up a powershell terminal elevated to Administrator and lets get going.

Install Jekyll locally on [Windows](http://jekyll-windows.juthilo.com/) for debugging (optional)
---
{% highlight powershell %}
New-Item -ItemType Directory -Force -Path "C:\temp\wpjekyll"
Invoke-WebRequest "http://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-2.1.5-x64.exe?direct" -OutFile "C:\temp\wpjekyll\rubyinstaller.exe"
C:\temp\wpjekyll\rubyinstaller.exe" /verysilent /tasks="modpath"
Invoke-WebRequest "http://cdn.rubyinstaller.org/archives/devkits/DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe" -OutFile "C:\temp\wpjekyll\rubydevkitinstaller.exe"
C:\temp\wpjekyll\rubydevkitinstaller.exe -o "C:\RubyDevKit" -y
ruby C:\RubyDevKit\dk.rb init
ruby C:\RubyDevKit\dk.rb install
gem install jekyll
{% endhighlight %}

Grab an [awesome template](https://github.com/poole/poole) of your choice
---
{% highlight powershell %}
Invoke-WebRequest "https://github.com/poole/poole/archive/master.zip" -OutFile "C:\temp\wpjekyll\jekyll-template.zip"
$shell = new-object -com shell.application
$template = $shell.NameSpace("C:\temp\wpjekyll\jekyll-template.zip")
New-Item -ItemType Directory -Force -Path "C:\temp\wpjekyll\jekyll-template"
foreach($item in $template.items()) { $shell.Namespace("C:\temp\wpjekyll\jekyll-template").copyhere($item) }
{% endhighlight %}

Give our new blog a test run
---
{% highlight powershell %}
gem install bundler
cd C:\temp\wpjekyll\jekyll-template\poole-master
Add-Content _config.yml "`r`nmarkdown: redcarpet`r`nhighlighter:      rouge"
"source 'https://rubygems.org'`r`ngem 'github-pages'`r`ngem 'rouge'`r`ngem 'wdm', '>= 0.1.0' if Gem.win_platform?"|sc "Gemfile"
bundle install
jekyll serve
{% endhighlight %}

Check it out at [http://localhost:4000](http://localhost:4000)!

Host your blog
---

There's lots of hosting options - after all, Jekyll ends up generating plain old HTML! [GitHub Pages](https://help.github.com/articles/using-jekyll-with-pages/) is a common choice - they'll both build the content and host the result for you and it's free including using a custom domain name.

1. Create a repository on [GitHub Pages](https://github.com/new) named yourgithubusername.github.io.
2. Create a git repository using the template folder from the previous section and push it up to your new GitHub repository using your tool of choice. You'll want to make sure you've pushed to the master branch of the repository.
3. Hang tight! It might take up to 10 minutes, but your blog should become available at http://yourgithubusername.github.io.