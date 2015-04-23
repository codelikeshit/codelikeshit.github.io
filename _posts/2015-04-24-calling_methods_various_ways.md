---
layout: post
title: Calling Ruby methods in various ways.
---

###Calling methods for a object other than `dot operator`.

Sample class:

```
class User

  def awesome_method1
    puts "##############"
    puts "Inside method 1"
    puts "##############"
  end

end

```

* Using `dot operator`(Normally used).

```
_user = User.new
_user.awesome_method1

=====Output====
###############
Inside method 1
###############

```

* Using `send` method.

```
_user = User.new
_user.send :awesome_method1

=====Output====
###############
Inside method 1
###############
```

* Using `method(:method_name).call`.

```
_user = User.new
_used.method(:awesome_method1).call

=====Output====
###############
Inside method 1
###############
```


* The major difference between `dot`, `send` and `method(:name).call` is last two will call the private, protected methods as well.
