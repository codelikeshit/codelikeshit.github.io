---
layout: post
title: Live Reload in Rails
---

###Live Reload is basically updating your stylesheets and views when you are changing your files without reloading. Its a great way to be more productive while you just need to focus on your code and rest is handled by scripts to show you the changes.

####Introduction with Meteor.js:

* This year while working with on a project I came across a new framework i.e. <a href="https://www.meteor.com/" target="_blank">Meteor</a>. Its an awesome JS framework but one thing that I loved was `live reload`.

* After checking out that Meteor thing I thought it would be great if I can do the same with Rails so tried out.

* Live Reload works this way, you just change the code of view part and voila it updates automatically in the browser you don't need to reload your browser tab.

* If you change some `.erb|.haml|.slim` template it will reload automacially.

* Here is the guide how you can remove this hassle of reloading browsers again and again.

####Dependencies:

* I hope most of us know about <a href="https://github.com/guard/guard" target="_blank"> Guard Gem </a>. I use it for running rspec tests on any content changes.

* Along with that we will be using <a href="https://github.com/guard/guard-livereload" target="_blank"> Guard Livereload Gem</a>.

* You need to install the <a href="https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei?hl=en" target="_blank">Live reload chrome plugin.</a>

####Setup Steps:

* Add the following content in your Gemfile

```
  gem 'guard'
  gem 'guard-livereload', '~> 2.4', require: false
```

* Install chrome live reload plugin.

* Then run the Guard installer, it will generate config files for Guard.

```
  bundle exec guard init
```

* It will generate the guardfile and there you need to add the following code.

```
  guard 'livereload', grace_period: 0, apply_css_live: true do
    watch(%r{app/views/.+\.(html.erb)$})
    watch(%r{app/helpers/.+\.rb})
    watch(%r{public/.+\.(css|js|html)})
    watch(%r{config/locales/.+\.yml})
    # Rails Assets Pipeline
    watch(%r{(app|vendor)(/assets/\w+/(.+\.(css|js|html|png|jpg))).*}) { |m| "/assets/#{m[3]}" }
    watch(%r{(app|vendor)(/assets/\w+/(.+)\.(scss))}) { |m| "/assets/#{m[3]}.css" }
  end
```

* Once its done and you have live reload plugin installed in your browser, you need to run Guard, I do it using<a href="https://github.com/ddollar/foreman" target="_blank"> Foreman Gem</a>. Its an awesome background process manager for Ruby.

* It requires a procfile to work. In procfile you can define the tasks you need to run in background. here is the sample procfile:-

```
    guard:            bundle exec guard
```

* Once Guard is running, we need to connect the live reload plugin with the guard by clicking on it.

* Once you click on the plugin on the console it should show `browser is connected`.

* Yeeeey that's all you need to do now hack your code without even bothering about reloading it after every small change.

* Go and change any JS/CSS or *.erb file it will reload the browser page and JS and CSS will be reloaded without even reloading the page.

PS: There is a <a href="http://livereload.com/" target="_blank"> Livereload app</a> for Mac as well, Its paid but yeah its faster than this setup. :)


