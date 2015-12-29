# Rails Layouts And Templates

## Objectives

1. Identify the default application layout.
2. Yield to view templates from a layout.
3. Specify a custom layout in ActionController on a controller level using the `layout` macro and on the action level using the `render :layout => "custom"` option.
4. Use :layout => false to shut off the layout and only render the view

## Outline

Pretty simple README that illustrates how Rails uses layouts in app/views/layouts/application.html.erb as the default layout applied to all views.

Show that the layout is rendered in a simple static example (like an about page) but that the view is not rendered, only the layout is.

explain how we <%= yield %> within a layout to delegate or yield the rest of the rendering to the view specific action template.

Explain that for every request we only ever render one layout and one view, even if multiple html.erb's compose the entire view using something called partials we'll learn about later, but still, it's one layout, one view, and then lots of partials get mixed in.

layouts correspond to controllers or sections of your application (not weird to have an admin layout, a marketing layout and then an application layout).

show the layout controller macro and the conventions between controller layouts (ProductsController will automatically use layouts/products.html.erb if it exists over application.html.erb even though that's rarely useful).

show the render :layout and render :layout => false for view specific layouts.
