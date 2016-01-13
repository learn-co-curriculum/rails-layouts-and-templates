# Rails Layouts And Templates

This lesson will show you how to use layouts to achieve a common look and feel between different views in your app.

## Objectives

After this lesson, you'll be able to...

1. Identify the default application layout.
2. Yield to view templates from a layout.
3. Specify a custom layout in ActionController on a controller level using the `layout` macro and on the action level using the `render :layout => "custom"` option.
4. Use :layout => false to shut off the layout and only render the view

## Life Without Layouts

Imagine that you're tasked to build an online store app with Rails.

You would probably have a few different views in this app, for example:

1. A list of products
2. A detail view that shows more info for a selected product
3. A shopping cart

Across all these views you would want a consistent look, which is called a layout. This layout perhaps contains something like a logo, navigation links, a search bar, and a footer at the bottom that contains some info about the shop.

Without layouts you would need to copy and paste your logo, navigation, search, and footer code into each action's template that you want in your application.

And when you want to make a change to one of the common components, for example the navigation, you would have to go and make that change in all the template files that you created in your application. This will obviously slow you down a lot.

## Layouts To The Rescue

Luckily you don't need to copy content from one template file to the next, because layouts in Rails are enabled by default, and when you generate a new Rails app, it generates a layout for you.

To find the generated layout, go and have a look in your Rails app at the following path. When you render a template for an action without specifying a different layout to use, Rails will use the layout found at this location: **app/views/layouts/application.html.erb**.

When you first generate a Rails app, this file will generate something very similar to this, depending on your version of Rails. The application.html.erb file is a very good place to start adding common components, like the navigation, search, and footer from the example above.

```erb
<!-- app/views/layouts/application.html.erb -->

<!DOCTYPE html>
<html>
<head>
  <title>ACME Store</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>
</head>
<body>

<%= yield %>

</body>
</html>
```

## Yield

Let's say you code up a new layout from scratch, and you end up with something like this:

```erb
<!-- app/views/layouts/application.html.erb -->

<!DOCTYPE html>
<html>
<head>
  <title>ACME Store</title>
</head>
<body>
    <h1>Welcome To The ACME Store!</h1>
</body>
</html>
```

**Please note:** usually you should include links to assets like style sheets and JavaScript files in your layouts, which was omitted in the code above to keep it simpler.

Other than the missing links to common assets, this layout is missing something **terribly important**. To see what it is, have a look at the following example that uses this layout.

This is an example of an action that we expect to result in our action's template rendered within the layout defined above.


```ruby
#/app/controllers/static_controller.rb

class StaticController < ApplicationController
  def about
  end
end
```

There should also be an associated route in the /config/routes.rb file to route a request to **/about** to the about action in the static controller above.

```ruby
#/config/routes.rb

Rails.application.routes.draw do
  get 'about', to: 'static#about'
end
```

And this is the template for the about action, with a simple message in it, which we would want to display in our layout from above.

```erb
<!-- /app/views/static/about.html.erb -->

<p>Hello!</p>
```

When you load up this route in your browser, you will be greeted by a very bold message saying: **Welcome To The ACME Store!**, but you won't see the **Hello!** from the about action's template.

This is happening because the layout file at *app/views/layouts/application.html.erb* does not have a yield statement in it.
The yield keyword is what Rails uses to decide where in the layout to render the content for the action. If you don't put Yield in your layout, only your layout will render for each action, instead of your layout with the contents of the action template in the correct place in the layout.

To fix this issue, just change the layout file to include yield:

```erb
<!-- app/views/layouts/application.html.erb -->

<!DOCTYPE html>
<html>
<head>
  <title>ACME Store</title>
</head>
<body>
    <h1>Welcome To The ACME Store!</h1>
    <%= yield %>    
</body>
</html>
```

Now when you hit up this route in your browser, you will see **Welcome To The ACME Store!**, followed by **Hello!**. This means that when the layout rendered, it pulled the action specific template into the correct place, where we added the yield statement.

## How Layouts and Templates Are Stitched Together

At it's simplest level, this is what happens when a request is made to your rails application:

1. Rails finds the template for your action based on either convention, or any other options you passed to the render method in your controller action.
2. It finds the correct layout to use, in a very similar fashion as finding the action's template, through convention, or specific options that you provided.
3. Rails uses the action template that you provided to generate the content specific to the action (this template might be composed of partial views, which you'll learn about a bit later)
4. It then looks for the **yield** statement in the layout, and inserts the result of the action template in the layout at the correct spot

So this means that for every request handled by rails, at most one layout will be used, and only one action template. The action template can call out to other templates, called partials, to render itself. Partials are covered in upcoming lessons, so don't worry too much about it for now.

## How Rails Decides Which Layout To Use

Think about the example from earlier in this lesson, the ACME store app. As mentioned before, it would make sense to have the same layout for the product list, product detail pages, and cart, because you would want some common elements in the same places between those different views.

But when you add administration functionality to this online store, in order to allow someone to add new products to the site, update prices, and perhaps draw reports, you probably want to use a different layout, which is quite easy to do with Rails.

### Deciding On A Layout Through Convention

Rails uses a simple convention to find the correct layout for your request. If you have a controller called **ProductsController**, it'll see whether there is a layout for that controller at **layouts/products.html.erb**. Similarly, if you have a controller called **AdminController**, it'll look for a layout at **layouts/admin.html.erb**. If it can't find a layout specific to your controller, it'll use the default layout at **app/views/layouts/application.html.erb**.

Most applications use the default layout for everything though, so try not to have a layout for each controller. You need to have a consistent look and feel throughout your site, and use a different layout only if the situation really warrants it.

### Overriding Conventions

If you need to override the conventions explained above, you can easily do so. For example, if you have a controller called **ShoppingCartController**, and you want to use the layout at **layouts/products.html.erb**, you have two options:

1. If you want to use the products layout for every action, simply specify that you want to use the products layout using the `layout` method in your controller, passing it a string that it can use to find the layout:

```ruby
class ShoppingCartController < ApplicationController
  layout "products"
end
```

2. If you want to use the products only for a particular action, simply use the render method in the controller action, and specify the layout you want it to use like this:

```ruby
class ShoppingCartController < ApplicationController
  def list
    render :layout => "static"
  end

  # the rest  of the actions will use standard conventions
end
```

If you want to render your action template without a template, you can do the following:

```ruby
class ShoppingCartController < ApplicationController
  def list
    render :layout => false
  end

  # the rest  of the actions will use standard conventions
end
```


## Recap

Use Layouts to provide a common look and feel between different views of your app. As much as possible use and adapt only the default layout, until you have a good reason to use a different layout for a different section or action.

Next, we'll take what we've learned here and see it working in a lab.
