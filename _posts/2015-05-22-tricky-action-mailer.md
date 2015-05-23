---
layout: post
title: Tricky Rails Mailer
tags: [Rails, Ruby, ActionMailer]
---

###How Rails ActionMailer trick the methods


* <a href="http://guides.rubyonrails.org/action_mailer_basics.html" target="_blank">ActionMailer</a> in Rails, yooo its awesome it do tons of stuff and makes my life easy while it comes to sending email from Rails app.

* If you don't know much about it, you should check <a href="http://guides.rubyonrails.org/action_mailer_basics.html" target="_blank">ActionMailer guide</a> as well, explained pretty well.

* ###A bulb flashed ;) :-


    * I was writing some code to send emails after user signs up:-

    Model class:-

      ```
    class User < ActiveRecord::Base

      after_commit :notify_user, on: :create

      def notify_user
        MyAwesomeMailer.send_welcome_email(self, "Welcome to Interakt.co").deliver_now
      end

    end
      ```
      Mailer class:-

      ```
    class MyAwesomeMailer < ActionMailer::Base

      def send_welcome_email(_user, _message)
        @user    = _user
        @message = _message
      end

    end
      ```

    * All of sudden a bulb flashed over my head and I realized ```send_welcome_email``` is an ```instance method``` and I called a ```class method here```. 
    
    ``` 
      MyAwesomeMailer.send_welcome_email(self, "Welcome to Interakt.co").deliver_now
    ```
    * I was confused and asked from couple of my senior but didn't get exact answer, one tried to guess and said it might be using ```method_missing``` hook of Ruby.

* ### Getting my Hands Dirty:-

    * Finally decided to get my hands dirty with source code for first time and trust me it was worth it :).

    * Once I cloned the <a href="http://github.com/rails/rails" target="_blank">Rails source</a> code I jumped directly into <a href="https://github.com/rails/rails/blob/master/actionmailer/lib/action_mailer/base.rb " target="_blank">ActionMailer::Base</a> module and started browsing it.

    * First thing I tried with it was calling ```send_welcome_email``` like a instance method but it didn't work, go and try it you will get the same exception.

        ```
        MyAwesomeMailer.new.send_welcome_email(self, "Welcome to Interakt.co").deliver_now

        NoMethodError: private method `new' called for MyAwesomeMailer:Class

        ```

* ### Make class method private:-

    * Rails mark new method of mailer as ```private method``` so you can't call it using dot(.) operator.
    
    * Another thing that I analyzed was new is a ```class method``` and if I use ```private``` or ```protected``` it will not work for class methods.

    e.g:-

      ```

      class MyAwesomeClass

        private

        # Instance method
        def my_instance_method
          puts "I am not public any more"
        end

        # Class method
        def self.my_class_method
          puts "I am still public method"
        end

      end

      MyAwesomeClass.my_class_method
      ==> I am still public method
      ```

    * Callling ```MyAwesomeClass.my_class_method``` and it will work and its not a private method, it cleary indicates its not set as private method. Now the question is how to make class level methods private.


    * I came across a new method ```private_class_method``` for ruby, usage:-

      ```
      private_class_method :new
      ```

    It makes class methods private and Rails use it all the time.

* ###Getting the idea:-

    * Coming back to our point when I call a class method how it calls the instance method with the same name.

    * Finally after messing my head around for some time I got the answer and that is its basically using ```method_missing``` hook of ruby and doing the stuff in backend.

* ###Flow(How it works internally):-

    * When you call a class method e.g:-

    ```
    MyAwesomeMailer.send_welcome_email(self, "Welcome to Interakt.co").deliver_now
    ```

    *  first thing is that(Class Method) method is not available there so ```method_missing``` will be called and this is the code inside ```method_missing``` hook method.

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
    
    * In code about that method is not called but created new object of MessageDelivery and yeah you guessed it right that class is doing the whole stuff, have a look on code:-

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

    * Here after initializing the object, when you call ```deliver_now``` to deliver your email and it calls message method wich call ```__getobj__``` method.


    ```  
    def __getobj__ #:nodoc:
      @obj ||= @mailer.send(:new, @mail_method, *@args).message
    end
    ```

    * Here ```@mailer``` is containing you class i.e MyAwesomeMailer, ```@mail_method``` is containing method_name you called.
    * This code is basically creating a new instance of your ```MyAwesomeMailer``` class and running the method you called on it.


One conclusion of all that is Rails is doing a lot of magic behind the scene and helping developers to focus on business logic. Its very easy to begin with but it take some efforts to master in & out of it :).


