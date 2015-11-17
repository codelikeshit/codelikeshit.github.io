---
layout: post
title: Live Reload in Rails
---
###Live Reload is reloading your CSS and Views when you are changing your views or assets file(JS/CSS) files. 

* This year while working with on a project I came acroos a new framework i.e. [Meteor](https://www.meteor.com/) Its an awesome JS framework but one thing that I loved was Live Reload. 

* Live Reload works this way, you just change the code of view part and voila it updates automatically in the browser you don't need to reload your browser tab, and I love when I can reduce manual efforts for anything.

* I hope most of us know about Guard Gem. I use it for running my R-spec tests on any changes. Now it will help you out with the reloading the content as well.

* We do have three requirements:-

* gem 'guard'
* gem 'guard-livereload', '~> 2.4', require: false
* Live reload chrome plugin(Its free :p)

* Basically you need to install chrome live reload plugin in your browser.

* Add Guard and guard-livereload gem in your Gemfile.

* Then run the Guard installer.

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

* Once its done and you have live reload plugin installed in your browser, you need to run Guard, I do it using foreman

        ```
            guard:            bundle exec guard
        ```

* Once Guard is running, we need to connect the live reload plugin by clicking on it and Guard will show `browser is connected`.

* Now go and change any JS/CSS or *.erb file it will reload the browser page and JS and CSS will be reloaded without even reloading the page.

* I found it very productive :)

