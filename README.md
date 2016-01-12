# Rails Layouts And Templates

## Objectives

After this lesson, you'll be able to...

1. Identify the default application layout.
2. Yield to view templates from a layout.
3. Specify a custom layout in ActionController on a controller level using the `layout` macro and on the action level using the `render :layout => "custom"` option.
4. Use :layout => false to shut off the layout and only render the view

## Life Without Layouts

Imagine that you are tasked to build an online shop app with Rails.
You would probably have a few different views in this app, for example a list of products, a detail view that shows more info for a selected product, and a shopping cart.
Across all these views you would definitely want a consistent navigation, perhaps containing a search bar, and a footer at the bottom that contains some info about the shop.
Without layouts you would need to copy and paste your navigation, search, and footer code into each action's template that you want in your application.
And when you want to make a change to one of the common components, for example the navigation, you would have to go and make that change in all the template files that you created in your application.

### An Example Of Duplicating Content In Templates

Pretty simple README that illustrates how Rails uses layouts in app/views/layouts/application.html.erb as the default layout applied to all views.

Start with why layouts are nice. Notice that every page needs certain things. It would be silly to copy paste every time

Show that the layout is rendered in a simple static example (like an about page) but that the view is not rendered, only the layout is.

explain how we <%= yield %> within a layout to delegate or yield the rest of the rendering to the view specific action template.

Explain that for every request we only ever render one layout and one view, even if multiple html.erb's compose the entire view using something called partials we'll learn about later, but still, it's one layout, one view, and then lots of partials get mixed in.

layouts correspond to controllers or sections of your application (not weird to have an admin layout, a marketing layout and then an application layout).

show the layout controller macro and the conventions between controller layouts (ProductsController will automatically use layouts/products.html.erb if it exists over application.html.erb even though that's rarely useful).

show the render :layout and render :layout => false for view specific layouts.
