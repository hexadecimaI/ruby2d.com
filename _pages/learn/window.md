---
title: The window
description: Learn how to create and configure a window
next_topic: 2d-basics
layout: learn
---

# Creating a window

As you saw in the ["get started" tutorial](/learn/get-started), the simplest thing you can do in Ruby 2D is require the gem and `show` the window:

```ruby
require 'ruby2d'

show
```

If you'd like to follow along, save this code to a file named `window.rb` and run it using the standard Ruby interpreter on the command line, like so:

```bash
ruby window.rb
```

You should see a black, empty window with a size of 640-by-480 pixels and a title bar with the text "Ruby 2D".

# Setting attributes

When you don't set any window attributes, the default values will be used. You can change these attributes by using the `set` method. Let's try changing the window title before we `show` it:

```ruby
require 'ruby2d'

set title: "Hello World!"

show
```

Notice the title bar of the window is now set to the new text we provided. The Ruby 2D domain-specific language (DSL) makes it easy to change things in a natural and intuitive way. Here, we're calling a method called `set` and passing it a `Hash` with a`Symbol` called `:title` as the key, and a `String` with the text `"Hello World!` as the value. Alternatively, we could write it this way to make these details more explicit:

```ruby
set( { :title => "Hello World!" } )
```

This is a bit verbose, especially for Ruby, so we'll stick to the style in the original example.

Let's play with some other attributes. The black background is a little boring, so let's change it! We can `set` it to something more interesting, like the color blue:

```ruby
set background: 'blue'
```

Try some other colors, like `red`, `orange`, `lime`, `fuchsia`, or roll the dice with `random`. When there are multiple attributes we want to set, we can chain them together for convenience:

```ruby
set title: 'Howdy', background: 'navy'
```

Great! You've got the basics of setting attributes down. Here are all the attributes you can set:

| Attribute          | Description |
|--------------------|-------------|
| `:title`           | The title of the window. Default: `"Ruby 2D"` |
| `:background`      | The background color of the window. Any valid Ruby 2D `Color` is acceptable. Default: `"black"` |
| `:width`           | The width of the window in pixels. Default: `640` |
| `:height`          | The height of the window in pixels. Default: `480` |
| `:viewport_width`  | The width of the viewport in pixels, the visible part of the window. Default is the width of the window at the time `show` is called. |
| `:viewport_height` | Same as `:viewport_width` above, but for setting the height. |
| `:resizable`       | Determines if the window can be resized, either `true` or `false`. Default: `false` |
| `:borderless`      | Determines if the window has a border, either `true` or `false`. Default: `false` |
| `:fullscreen`      | Determines if the window will be shown in fullscreen mode, either `true` or `false`. Default: `false` |
| `:diagnostics`     | Prints debugging information to the console, either `true` or `false`. This is mostly useful when working on Ruby 2D itself. Default: `false` |

# Getting attributes

Sometimes it's also helpful to get the value of a window attribute, so there's a method for that too called `get`:

```ruby
get :width  # returns `640`, for example
```

For every attribute you can `set`, you can also `get` its value by providing the `Symbol`. Also, unlike `set`, you can only `get` one attribute at a time.

There are also a few extra attributes that you can get:

| Attribute  | Description |
|------------|-------------|
| `:window`  | The window object itself, just in case you want to `inspect` it. |
| `:frames`  | The number of frames that have been rendered since the start. |
| `:fps`     | The current frame rate expressed in frames per second (a `Float`). |
| `:mouse_x` | The x-coordinate position of the mouse, relative to the window. |
| `:mouse_y` | The y-coordinate position of the mouse, relative to the window. |

# The Window class

When you `require 'ruby2d'`, a new window is instantiated for you by calling `Ruby2D::Window.new`. Sometimes it might be convenient to reference the `Window` class directly, for example when retrieving attributes:

```ruby
Window.title   # returns "Ruby 2D"
Window.width   # returns 640
Window.height  # returns 480
```

# The update loop

The window also manages the update loop, one of the few infinite loops in programming you'll encounter that isn't a mistake. Every window has a heartbeat, a loop that runs 60 times per second, or as close to it as the computer's performance will allow. Using the `update` method, we can enter this loop and make the window come to life!

Say we're bored with the static background we currently have. Let's try changing it to a random color every second. Here's a complete Ruby program to do just that:

```ruby
require 'ruby2d'

tick = 0

update do
  if tick % 60 == 0
    set background: 'random'
  end
  tick += 1
end

show
```

Ah, much better! How does this work? First, we set a variable called `tick` to `0`. Then, we enter the `update` loop and `do` something interesting, like dividing `tick` by 60 and checking if its remainder equals 0. Each cycle of the loop, we increment `tick` by one. When the remainder _does_ equal 0, we know a second has passed (assuming the window is running at about 60 frames per second), and we `set` the background color to `'random'`. Pretty cool, huh?

# Closing the window

When you're done with a window, there's nothing left to do but close it. In the examples above, you probably closed the window by clicking the "close" button on the title bar itself, or perhaps using a keyboard shortcut (like "Command-Q" on a Mac or "ALT+F4" on Windows), or using a menu bar, or right-clicking its icon, or whatever. The point is, you had to do it through the user interface in some way. But, what if instead you wanted to close the window with code? Well, there's a method for that too!

Using `close`, you can programmatically close the window after calling `show`. This can be a bit tricky because once you show the window, you enter the window's infinite loop and the code that follows won't be reached until _after_ the window is closed and the loop is exited. Don't worry though, this isn't some catch-22 — we just have to make sure to call `close` from _within_ the loop. Here's an example of a complete Ruby program where `close` is called after the window is shown and while the loop is running, taking advantage of the `update` method you learned about earlier.

```ruby
require 'ruby2d'

t = Time.now

update do
  # Close the window after 5 seconds
  if Time.now - t > 5 then close end
end

show
```

Each turn of the window's loop, it checks if the time we saved in variable `t` is greater than five seconds earlier than the current time. When it is, `close` is called and the window goes away.

# That's it!

You've learned all there is to know about the window in Ruby 2D. Continue to the [next topic ▸](/learn/{{ page.next_topic }})
