# Backbone Routefilter

Before and after filters for Backbone.Router. Useful for doing things like client side content not found pages– any time you want to do something to every route before or after it's routed, this is a good way to do it.

## Getting Started
Download the [production version][min] or the [development version][max].

[min]: https://raw.github.com/boazsender/backbone.routefilter/master/dist/backbone.routefilter.min.js
[max]: https://raw.github.com/boazsender/backbone.routefilter/master/dist/backbone.routefilter.js

In your web page:

```html
<!doctype>
<html>
<head>
<script src="http://code.jquery.com/jquery.js"></script>
<script src="http://underscorejs.org/underscore.js"></script>
<script src="http://backbonejs.org/backbone.js"></script>
<script src="backbone.routefilter.js"></script>
<script>
jQuery(function($) {
  // Set up a a Router.
  var Router = Backbone.Router.extend({
    routes: {
      "": "index",
      "page/:id": "page"
    },
    before: function( route ) {
      // Do something with every route before it's routed.
      // "route" is a string containing the url route just like regular Backbone
      // route callbacks. If the url has more fragments, the before callback will
      // aslo get them, eg: before: function( frag1, frag2, frag3 ).

      if( route === 'foo') {
        console.log('The before filter ran and the route was foo!');
      }
      // Returning false from inside of the before filter will prevent the
      // current route's callback from running, and will prevent the after
      // filter's callback from running.
    },
    after: function( route ) {
      // Do something with route information right after a route callback has occured.
      // This will not run if you return false from within the before callback.
      console.log('The after filter ran and the route was ' + route + '!');
    },
    index: function(){
      console.log('navigated to index.')
      // Instantiate your index view.
    },
    page: function( route ){
      console.log('navigated to page and the route was: ' + route + '.')
      // Instantiate your page view.
    }        
  });

  // Instantiate the Router.
  var router = new Router();

  // Start the history.
  Backbone.history.start();

  // Navigate to page two.
  router.navigate('page/foo', true);

  // Override the before filter on the fly
  router.before = function( route ) {
    if( route === 'bar' ){
      // return false to stop ecexution of the callback for this route,
      // and the after callback if we navigate to page/bar.
      console.log('We navigated to another page, but the page callback and after filter did not run because we returned false from inside the before filter');
      return false;
    }
  }
  
  // Navigate to the place our before filter is handling.
  router.navigate('page/bar', true);
});

</script>
</head>
<body>
</body>
</html>
```

## Release History
* v0.1.0 - 08/29/2012 - backbone.routefilter first release (unit test coverage, stable api, and stable plugin approach).
* v0.1.0-pre - 08/28/2012 - backbone.routefilter is pre release

## License
Copyright (c) 2012 Boaz Sender  
Licensed under the MIT, GPL licenses.

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [grunt](https://github.com/cowboy/grunt).

### Important notes
Please don't edit files in the `dist` subdirectory as they are generated via grunt. You'll find source code in the `src` subdirectory!

While grunt can run the included unit tests via PhantomJS, this shouldn't be considered a substitute for the real thing. Please be sure to test the `test/*.html` unit test file(s) in _actual_ browsers.

### Installing grunt
_This assumes you have [node.js](http://nodejs.org/) and [npm](http://npmjs.org/) installed already._

1. Test that grunt is installed globally by running `grunt --version` at the command-line.
1. If grunt isn't installed globally, run `npm install -g grunt` to install the latest version. _You may need to run `sudo npm install -g grunt`._
1. From the root directory of this project, run `npm install` to install the project's dependencies.

### Installing PhantomJS

In order for the qunit task to work properly, [PhantomJS](http://www.phantomjs.org/) must be installed and in the system PATH (if you can run "phantomjs" at the command line, this task should work).

Unfortunately, PhantomJS cannot be installed automatically via npm or grunt, so you need to install it yourself. There are a number of ways to install PhantomJS.

* [PhantomJS and Mac OS X](http://ariya.ofilabs.com/2012/02/phantomjs-and-mac-os-x.html)
* [PhantomJS Installation](http://code.google.com/p/phantomjs/wiki/Installation) (PhantomJS wiki)

Note that the `phantomjs` executable needs to be in the system `PATH` for grunt to see it.

* [How to set the path and environment variables in Windows](http://www.computerhope.com/issues/ch000549.htm)
* [Where does $PATH get set in OS X 10.6 Snow Leopard?](http://superuser.com/questions/69130/where-does-path-get-set-in-os-x-10-6-snow-leopard)
* [How do I change the PATH variable in Linux](https://www.google.com/search?q=How+do+I+change+the+PATH+variable+in+Linux)
