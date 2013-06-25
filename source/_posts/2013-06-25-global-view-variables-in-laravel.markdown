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

The way I like to set things up, I make a `BaseController` class that extends Laravel's own `Controller`, and set up various global things there. All other controllers then extend from `BaseController` rather than Laravel's `Controller`.

``` html+php app/controllers/BaseController.php
class BaseController extends Controller
{
  public function __construct()
  {
    // Fetch the User object, or set it to false if not logged in
    if (Sentry::check()) {
      $user = User::find(Sentry::getUser()->id);
    }
    else {
      $user = false;
    }

    // Sharing is caring
    View::share('user', $user);
  }
}
```

Now I have access to the currently logged in user as `$user` in all views, and can access anything from it as normal, such as `$user->email` et cetera.
