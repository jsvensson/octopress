---
layout: post
title: "Nested Resource Controllers in Laravel"
date: 2013-06-19 12:48
comments: true
external-url:
categories:
 - Laravel
 - PHP
 - programming
---

[Laravel 4][laravel] is out, and it's pretty good for something PHP-based.

Yes, I have _opinions_ about PHP, as do [others][fractal].

<!-- more -->

Anyway. Laravel adds some much-needed sanity to PHP-based frameworks. I've previously messed around a bit with [CodeIgniter][codeigniter], but I was not impressed at all. Things felt sloppy and messy and didn't use any modern PHP features at all, nor is anyone interested in the massive undertaking it would require to haul CodeIgniter up to modern standards and still maintain backwards compatibility.


## RESTful controllers

Laravel has a couple of nifty features. It uses ye olde MVC structure, but the controllers come in several flavors. The standard version is a [RESTful controller][restful-controller]. It responds to standard HTTP verbs, making it easy to split your logic for a given page up into input and output.

First, define the route to the controller in your `routes.php`:

``` php routes.php
  Route::controller('list', 'ListController');
```

Now you can start writing your RESTful controller in `app/controllers/ListController.php`.

``` php ListController.php
<?php

class ListController extends BaseController
{

  public function getNewList()
  {
    // Return view for creating a new list
  }

  public function postNewList()
  {
    // Target for POST form, run validation etc then create list
  }

} ?>
```

This is a very handy way to avoid having to check if you're getting any POST input data -- `getNewList()` is always what creates the view (accessed via `/list/new-list`), `postNewList()` is always where the actual form validation, logic handling and object creation takes place.


## Resource controllers

A [resource controller][resource-controller] is the second flavor of controllers. This uses [CRUD verbiage][crud] to set up the standard actions to create, read, update and destroy objects. Define it in your `routes.php`:

``` php routes.php
  Route::resource('list', 'ListController');
```

You can use Artisan to create a scaffold of empty stub methods corresponding to the CRUD verbs:

    php artisan controller:make ListController


## Let's nest!

The documentation covers what the verbs actually do, so I won't go into that. Now we have our List controller -- but what if we want a List that contains Tasks?

We could simply leave it like this and just add a Task controller for dealing with the task objects and have the List show all tasks for a given list, but we want a bit more. This is the structure we want:

 * `/list` shows all your lists.
 * `/list/{list_id}` shows the details for a list; all your tasks could be shown here.
 * `/list/{list_id}/tasks` is a bit redundant, but tasks could also be shown here.
 * `/list/{list_id}/tasks/create` lets you create a new task for that list.
 * `/list/{list_id}/tasks/{task_id}` shows the details for a specific task.

In other words, what we want is another set of CRUD verbs under the first set.

We start by setting up our route:

``` php routes.php
  Route::resource('list', 'ListController');
  Route::resource('list.tasks', 'ListTaskController');
```

My personal preference is to name it `ListTaskController` to show that tasks belong to a list.

That's all there is to the route -- `list.tasks` defines the nested namespace, and Laravel is smart enough to deal with the extra routing for us.

The List controller can be written as usual. The Task controller requires an extra consideration, though.

The route created for us looks like `/list/{list_id}/tasks/{task_id}` -- it contains two parameters, so any Task method that needs the task id will require them both; otherwise it will just get the first parameter containing the list id.

``` php ListTaskController.php
<?php

class ListTaskController extends BaseController
{
  public function index($list_id)
  {
    return "Showing all tasks for list $list_id";
  }

  public function show($list_id, $task_id)
  {
    return "Showing task $task_id for list $list_id";
  }

} ?>
```

Now you can try things out:

 * `/list/1/tasks` should say "Showing tasks for list 1"
 * `list/1/tasks/3` should say "Showing task 3 for list 1"


[restful-controller]: http://laravel.com/docs/controllers#restful-controllers
[resource-controller]: http://laravel.com/docs/controllers#resource-controllers
[crud]: http://en.wikipedia.org/wiki/Create,_read,_update_and_delete

[laravel]: http://laravel.com/
[codeigniter]: http://codeigniter.com/
[fractal]: http://me.veekun.com/blog/2012/04/09/php-a-fractal-of-bad-design/
