# Flexible Path Segments

While being able to build dynamic and smart `/rock`, `/paper`, and `/scissors` URLs is nice, the dynamic nature of an interactive application rarely comes from the random value generator.

Rather, the interactivity comes from being able to accept **user input**. The first way we'll see how to do this is by making certain segments of the URL path **flexible**, such that the visitor can type anything they want into that spot; yet, they won't get the usual "No route matches" error. Instead, we'll catch what they typed in a special hash, the **params hash**, and route them along to the action.

Here's how it works:

If I, for example, wanted URLs such as:


```
/users/42/feed
/users/89/feed
/users/12/feed
...
```

And I wanted to handle _all_ URLs of this pattern with the same action (instead of defining an infinite number of actions, one for every possible ID), then I could write a **flexible** segment by starting it off with a colon (`:`) like this:

```ruby
match("/users/:route_user_id/feed", { :controller => "application", :action => "user_feed", :via => "get" })
```

Now, no matter what the visitor enters in that spot, e.g. `/users/42/feed`, they will always be routed to the `profile` action. Crucially, Rails will create a hash that looks like this:

```
params = { "route_user_id" => 42 }
```

The key `route_user_id` comes from whatever we made up when we were naming the flexible segment in the path — it could have been `zebra` if we wrote the route like this:

```ruby
match("/users/:zebra/feed", { :controller => "application", :action => "user_feed", :via => "get" })
```

The value associated with the key, in this case `42`, is whatever the user typed in that spot — it could have been `giraffe` if the user visited `/users/giraffe/feed`.

In any case, once we're in the action, we can use the `params` hash just like any other `Hash`. It has the same old methods — `.fetch`, `.keys`, etc:

```
class ApplicationController < ActionController::Base
  def user_feed
    the_id = params.fetch("route_user_id")

    user = User.where({ :id => the_id }).at(0)

    render({ :json => user.feed.as_json }) # Assuming we've already defined User#feed
  end
end
```

This simple mechanism is how most of the links you click on (i.e. URLs that you visit) work. `https://twitter.com/ChicagoBooth`, `https://github.com/raghubetina`, and `https://www.airbnb.com/rooms/1875533` all follow the same pattern.
