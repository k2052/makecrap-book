Build the pieces not the whole.
There is tendency to try to try to understand your app, to maintain the picture in your head.
The best development methodologies require you to understand the least to get something done.
Results should start happening in as little code as possible.

You shouldn't pollute your code orthogonally with a whole bunch of classes. You should have a little bit of functionality.
Ask what is one thing my app needs to do? And then build that into the app.

An app should only have on entry point. It should be a value.

App app sould mirror the REST model of the server side and be model centric.
A controller for each. Routes to show that model, list the model etc. This also makes it easy to prerender parts of the app server side

App class sets up the router.

Using the router model means you keep the view from fucking with the controller. it just goes router.navigate().

For example, to display a Post. we just call @app.router.navigate("posts/show/#{post.id}")

For sharing data (e.g sessions and global) we will use a State class. For example, accessing the currentUser.

    @user = State.get('user')

Or stuff like

    State.observeKey 'post', =>
      @render(State.get('post'))

Unlike Spine.js we will keep our controllers entirely route oriented.
A route method should just init the view. That's it. The view should handle it's dependency chain.

Any payloads we should push onto the State class. Just like params or body when server side.

Say we have a Games controller

class Games
  create: ->
    # Need JSON
    State.get("body") or State.get("game")

App development should a mirror API first dev. Start with the controllers and models and get them returning data. Controller routes
should return views.

Exactly how will that work? router.navigate() changes the body. So we should look to the app class to test whatever is returned by
a route?

If a view handles it's dependency chain. We can just call a render() method on a route that essentially sets the active view

We can think of the App class as a bit like the browser. We set the active page/view onto the App class (the browser). Then
to test we just ask the app for the active view and test it's content against the expected content.

The State class is our session data.

Each route can call a render function which adds the view to the App class but only if it doesn't exist.
If it already exists it justs sets it as active and then calls render on it. The constructor on view classes should
be mostly empty and call render() to setup content.

Always start with the data. Models first

