---
layout: post
title: Calling Ruby methods various ways.
---

###Calling methods for a object other than `dot operator`.

Sample class:

{% highlight ruby %}
class User

  def awesome_method1
    puts "##############"
    puts "Inside method 1"
    puts "##############"
  end

end

{% endhighlight %}

* Using `dot operator`(Normally used).

{% highlight ruby %}

_user = User.new
_user.awesome_method1

=====Output====
###############
Inside method 1
###############

{% endhighlight %}

* Using `send` method.

{% highlight ruby %}

_user = User.new
_user.send :awesome_method1

=====Output====
###############
Inside method 1
###############
{% endhighlight %}

* Using `method(:method_name).call`.

{% highlight ruby %}

_user = User.new
_used.method(:awesome_method1).call

=====Output====
###############
Inside method 1
###############

{% endhighlight %}

* The major difference between `dot`, `send` and `method(:name).call` is last two will call the private, protected methods as well.


