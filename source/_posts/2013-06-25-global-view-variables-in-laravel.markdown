---
layout: post
title: "Global view variables in Laravel"
date: 2013-06-25 14:34
comments: true
categories:
 - PHP
 - programming
 - Laravel
---

I got tired of all those `View::make()->with('foo', $bar);` I had in the controllers that I swore I would fix "later". Well, now it's later and I finally discovered `View::share()`.

<!-- more -->

The problem: almost every single view I have wants to know stuff about the currently logged-in user to populate menus and other stuff. My temporary solution was to use `with()` and pass the User object every single time I created a view:

``` html+php FooController.php
function getFoo()
{
  return View::make('foo.bar')
    ->with('user', $user);
}
```

## A doom with a view

That got tedious, not to mention redundant. I finally took the time to do a proper read of the documentation, and found `View::share()` mentioned in a single sentence. It's dead simple to use:

``` html+php
View::share('name', 'Steve');
```

The way I like to set things up, I make a `BaseController` class that extends Laravel's own `Controller`, and set up various global things there. All other controllers then extend from `BaseController` rather than Laravel's `Controller`. (In this case I'm using [Sentry][sentry] to manage user registration/authentication -- I can highly recommend it!)

``` html+php app/controllers/BaseController.php
class BaseController extends Controller
{
  public function __construct()
  {
    // Fetch the User object
    $user = Sentry::getUser();

    // Sharing is caring
    View::share('user', $user);
  }
}
```

Now I have access to the currently logged in user as `$user` in all views, and can access anything from it as normal, such as `$user->email` et cetera. Just make sure to check the object first; Sentry returns `null` if there's no user for the session.

## Using Filters

If you know for a fact that you want something set up for views on every request throughout the entire application, you can also do it via a filter that runs before the request -- this is how I deal with the User object in Laravel.

``` html+php app/filters.php
App::before(function($request)
{
  // Set up global user object for views
  View::share('user', Sentry::getUser());
});
```

[sentry]:https://cartalyst.com/manual/sentry
