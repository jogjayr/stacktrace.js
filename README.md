stacktrace.js
===============
[![Build Status](https://travis-ci.org/stacktracejs/stacktrace.js.svg?branch=master)](https://travis-ci.org/stacktracejs/stacktrace.js) [![Coverage Status](https://img.shields.io/coveralls/stacktracejs/stacktrace.js.svg)](https://coveralls.io/r/stacktracejs/stacktrace.js?branch=master) [![Code Climate](https://codeclimate.com/github/stacktracejs/stacktrace.js/badges/gpa.svg)](https://codeclimate.com/github/stacktracejs/stacktrace.js)

A JavaScript tool that allows you to debug your JavaScript by giving you a [stack trace](http://en.wikipedia.org/wiki/Stack_trace) of function calls leading to an error (or any condition you specify)

## Usage
```js
var callback = function(stackframes) {
    var stringifiedStack = stackframes.map(function(sf) { 
        sf.toString(); 
    }).join('\n'); 
    console.log(stringifiedStack); 
};

var errback = function(err) { console.log(err.message); };

StackTrace.get().then(callback, errback)
=> Promise(Array[StackFrame](https://github.com/stacktracejs/stackframe), Error)
=> callback([StackFrame('func1', [], 'file.js', 203, 9), StackFrame('func2', [], 'http://localhost:3000/file.min.js', 1, 3284)])

// Somewhere else...
var error = new Error('BOOM!'); 

StackTrace.fromError(error).then(callback, errback)
=> Promise(Array[StackFrame](https://github.com/stacktracejs/stackframe), Error)

StackTrace.generateArtificially().then(callback, errback)
=> Promise(Array[StackFrame](https://github.com/stacktracejs/stackframe), Error)

StackTrace.instrument(interestingFn, callback, errback)
=> Instrumented Function

StackTrace.deinstrument(interestingFn)
=> De-instrumented Function
```

## Get stacktrace.js
```
npm install stacktrace-js
bower install stacktrace-js
https://rawgithub.com/stacktracejs/stacktrace.js/master/dist/stacktrace.min.js
```

### stacktrace.js is available on cdnjs.com
https://cdnjs.com/libraries/stacktrace.js

`//cdnjs.cloudflare.com/ajax/libs/stacktrace.js/0.6.4/stacktrace.min.js`

## API

#### `StackTrace.get(/*optional*/ options)` => Promise(Array[StackFrame])
Generate a backtrace from invocation point, then parse and enhance it.
options: Object
* **sourceCache: Object (String URL => String Source)** - Pre-populate source cache to avoid network requests
* **offline: Boolean (default: false)** - Set to `true` to prevent all network requests
 
#### `StackTrace.fromError(error, /*optional*/ options)` => Promise(Array[StackFrame])
Given an Error object, use [error-stack-parser](https://github.com/stacktracejs/error-stack-parser)
 to parse it and enhance location information with [stacktrace-gps](https://github.com/stacktracejs/stacktrace-gps).
* **sourceCache: Object (String URL => String Source)** - Pre-populate source cache to avoid network requests
* **offline: Boolean (default: false)** - Set to `true` to prevent all network requests
 
#### `StackTrace.generateArtificially(/*optional*/ options)` => Promise(Array[StackFrame])
Use [stack-generator](https://github.com/stacktracejs/stack-generator) to generate a backtrace by walking the `arguments.callee.caller` chain.
* **sourceCache: Object (String URL => String Source)** - Pre-populate source cache to avoid network requests
* **offline: Boolean (default: false)** - Set to `true` to prevent all network requests
 
#### `StackTrace.instrument(fn, callback, /*optional*/ errback)` => Boolean
Call callback with a _stack trace_ anytime `interestingFn` is called. Returns `true` if given Function is successfully instrumented
* **fn** - Function to wrap, call callback on invocation and call-through
* **callback** - Function to call with stack trace (generated by `StackTrace.get()`) when fn is called
* **errback** - (Optional) Function to call with Error object if there was a problem getting a stack trace. 
Fails silently (though `fn` is still called) if a stack trace couldn't be generated.
 
#### `StackTrace.deinstrument(fn)` => Boolean
Remove StackTrace instrumentation on `fn`. Returns `true` if deinstrumentation succeeds.
* **fn** - Previously wrapped Function

## Browser Support
* Chrome 1+
* Firefox 3+
* Safari 5+
* Opera 9+
* IE 6+
* iOS 7+
* Android 4.0+

_NOTE: You won't get the benefit of [source maps](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/)
in IE9- or other very old browsers._

## Using node.js/io.js only? 
I recommend the [stack-trace node package](https://www.npmjs.com/package/stack-trace). 
It has a very similar API and also supports source maps.

## Contributing
Want to be listed as a *Contributor*? Start with the [Contributing Guide](CONTRIBUTING.md)!

This project is made possible due to the efforts of these fine people:

* [Eric Wendelin](http://www.eriwen.com)
* [Victor Homyakov](https://github.com/victor-homyakov)
* [Many others](https://github.com/stacktracejs/stacktrace.js/graphs/contributors)

## License
This project is licensed to the [Public Domain](http://unlicense.org)
