---
layout: post
title: GOD mode with Active admin gem.
---

### Implementing *GOD mode* in <a href="https://github.com/activeadmin/activeadmin" target="_blank">Active Admin</a>

####Number of clients asked me if it is possible to add GOD mode in application, where admin user will be able to login as any of other user and perform the operations as that user.

Requirements:

* Your app is a **Rails application**.
* Your application is using <a href="https://github.com/plataformatec/devise" target="_blank">Devise</a> for authentication.
* <a href="https://github.com/activeadmin/activeadmin" target="_blank">Active Admin</a> implementation.

Let's do some code:

* I am assuming you are having one model named **User**.
* First open up file **myawesomeproject/admin/user.rb** and add the following code there.

{% highlight ruby %}
action_item :view, only: :show do
  link_to 'Login as User', sign_in_as_user_path(user_id: resource.id), :target => "_blank"
end
{% endhighlight %}

* Add path in config/routes.rb
{% highlight ruby %}
 get "/sign_in_as_user" => "sessions#sign_in_as_user"
{% endhighlight %}

* You might have noticed I am pointing it to a normal URL outside the active admin actions, the reason is I want to use **sign_in** method that devise provides, it is available in the **Devise::SessionsController**.

* Now override the sessions controller by inheriting from Devise::SessionsController and put this code inside.

{% highlight ruby %}

  before_filter :check_admin_signed_in, only: :sign_in_as_user

  def sign_in_as_user
  _user = User.find_by_id params[:user_id]
  if _user.present? and _user.confirmed?
    sign_in("user", _user)
    redirect_to root_path, notice: "Signed in as user"
  else
    redirect_to admin_users_path, notice: "Record not found"
  end

  def check_admin_signed_in
    redirect_to root_path, notice: 'Unauthorized Access' unless current_admin_user.present?
  end

{% endhighlight %}


