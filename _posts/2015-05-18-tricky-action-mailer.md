---
layout: post
title: Tricky Rails mailer
---

###How Rails ActionMailer trick the methods

* ActionMailer in Rails, yoo its awesome it do tons of stuff and makes my life easy while it comes to sending email from Rails app.

* I was writing some code to send emails after user signs up:-

  Mailer class:-

  ```
  class MyAwesomeMailer < ActionMailer::Base

    def send_email(_user, _message)
      @user    = _user
      @message = _message
    end

  end
  ```

  Model class:-

  ```
  class User < ActiveRecord::Base

    after_commit :notify_user, on: :create

    def notify_user
      MyAwesomeMailer.send_email(self, "Welcome to Interakt.co").deliver_now
    end

  end
  ```

* All of sudden a bulb blinked over my head and I realized `send_email` is instance method and I called a class method here MyAwesomeMailer.send_email(self, "Welcome to Interakt.co").deliver_now :p.

* I was confused and asked from couple of my senior but didn't get answer, one tried to guess and said it might be using method_missing hook of Ruby so finally decided to get my hands dirty with source code for first time and trust me it was worth it :).

* Once I cloned the [Rails source](http://github.com/rails/rails) code I jumped directly into https://github.com/rails/rails/blob/master/actionmailer/lib/action_mailer/base.rb file and started browsing it.

* First thing I tried while doing this is calling it like a instance method but it didn't work, go and try it you will get the same exception.

    ```
    MyAwesomeMailer.new.send_email(self, "Welcome to Interakt.co").deliver_now

    NoMethodError: private method `new' called for MyAwesomeMailer:Class

    ```
* Another thing that I analyzed was new is a class method and if I use private or protected it will not work for class methods.

* Its a new learning here, how to create private class methods, came across a new method private_class_method for ruby, usage:-

    ```
    private_class_method :new
    ```

    It makes class methods private and Rails use it all the time.

* Coming back to our point when I call a class method how it calls the instance one:-

* Finally after messing my head around for some time I got the answer and that is its basically using method_missing hook of ruby and doing the stuff in backend.

* When you call a class method e.g:-

  ```
  MyAwesomeMailer.send_email(self, "Welcome to Interakt.co").deliver_now
  ```

  first thing is that(Class Method) method is not available there so method_missing will be called and this is the code inside method_missing hook.

  ```
  class Base < AbstractController::Base

    class << self

      def method_missing(method_name, *args) # :nodoc:
        if action_methods.include?(method_name.to_s)
          MessageDelivery.new(self, method_name, *args)
        else
          super
        end
      end

    end

  end
  ```
  here again that method is not called but created new object of MessageDelivery and yeah you guessed it right that class is doing the whole stuff, have a look on code:-


  ```
  class MessageDelivery < Delegator

    def initialize(mailer, mail_method, *args) #:nodoc:
      @mailer = mailer
      @mail_method = mail_method
      @args = args
    end

    def __getobj__ #:nodoc:
      @obj ||= @mailer.send(:new, @mail_method, *@args).message
    end

    # Returns the Mail::Message object
    def message
      __getobj__
    end

    def deliver_now
      message.deliver
    end
  end

  ```

  Here after initializing the it, when you call deliver_now to deliver message and it calls message method wich call __getobj__ method.


  ```
  def __getobj__ #:nodoc:
    @obj ||= @mailer.send(:new, @mail_method, *@args).message
  end
  ```

  Here @mailer is containing you class i.e MyAwesomeMailer, @mail_method is containing method_name you called.

  This code is basically creating a new instance of your MyAwesomeMailer class and running the method you called on it.

  One thing I am still trying to figure out is why its this way :p




