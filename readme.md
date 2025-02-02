# okjs 

---

A tiny asynchronous-friendly JS unit test utility.



## About

---

JavaScript and the web form a heavily asynchronous environment, which can pose particular challenges for JS based unit
testing.  In particular, Ajax, click handlers, and other observer models require that exceptions be caught in various
execution blocks and without interfering with object scope.  Additionally, these methods need to execute in a specified
amount of time.

The okjs library directly attends to these hurdles, and is specifically intended for the development process.


## Quick Start

---

        var unit = okjs({
            verbose: true
        });

        unit.test("basic",  function () {
            unit.assert("true-ish", "basic boolean true");
            unit.equal(true, true, "strict comparison true");
            unit.nequal(true, false, "strict comaparison neq");
            unit.nequal(1, true, "1 neq true");
        });

        unit.test("async callback",  function () {
           setTimeout( unit.callback("timeout callback w/ function", function () {
                unit.equal(true,true, "in callback function ok")
            }),  500);
        });

        unit.test("async event",  function () {
            unit.event("click", document, "document was clicked", function (e) {
                unit.equal(e.type, "click", "was a mouse click event");
                unit.equal(e.currentTarget, document, "currentTarget is the document");
            }, 10000);
            unit.log("waiting 10s for user to click page... ");
        });

        unit.test("exception throwing",  function () {
            unit.exception("expected exception", function () {
                throw "catch me"
            });
        });

        window.onload = function () {
            unit.start()
        };


[sample output](http://gkindel.com/okjs/test/quickstart.html)

## Reference

---

### Setup

* `okjs(options)`

    Returns an instance of the testing harness.

    Available options:
    * `verbose` - if set to false, tests that pass are hidden
    * `wait_ms` - default delay for callbacks and events
    * `exceptions` - Disabled the catching of exceptions, which can help debugging with browser tools which
        break on exceptions. Defaults to false.

* `.test(msg, fn)`

    Defines a group of test assertions. Groups are executed sequentially. Exceptions in the function will
    halt the group functions execution.  All outstanding events are resolved before moving on. All assertions
    must be done within a test group. Tests are executed once unit.start() is called.

    * `msg` : Associated test message.
    * `fn` : An anonymous function with no arguments.

* `.start()`

    Begins test execution, to be called sometime after the window onLoad event.

### Assert Statements

* .`assert`(a, msg)

    Basic assert.  Passes if `a` evaluates to true.

    * `a` object to evaluate as a boolean
    * `msg` : Associated test message

* `.equal(a, b, msg)`

    Strict comparison assert.  If objects are passed in, it looks for b.equals(a),
    if defined. Otherwise does a strict (===) compare.

    * `a` , `b` : objects to be compared.
    * `msg` : Associated test message.


* `.nequal(a, b, msg)`

    Basic negative assert. Strict.

    * `a` , `b` : objects to be compared.
    * `msg` : Associated test message.

* `.callback(msg, fn, context, options)`

    Returns a function which is intended to be asynchronously executed, by intervals, observers,
    ajax responses, event listeners, etc. The callback needs to be executed in time or the
    test fails, even if "fn" was left null. Arguments are preserved.

    * `msg` : Associated test message.
    * `fn` : Optional callback which specifies an event listener function.
    * `context` : Scope, to be used for `this` in the callback.
    * `options`:
        * `wait_ms` - custom time allocation for this callback only

* `.event(type, source, msg, fn, delay)`

    Asserts an event will be fired. Event listener with callback is automatically added, waited
     for, and removed once either the event is caught or timeout occurs.  Event arguments are preserved.

    * `type` : Required event type (eg: "click", "resize").
    * `source` : Object which is expected to fire event, supporting DOM event syntax.
    * `msg` : Associated test message.
    * `delay` : Optional delay in msec. Overrides default timeout.
    * `fn` : Optional callback which specifies an event listener function.

* `.exception(msg, fn)`

* `.log(msg)`

    Logs a message with no effect on test results. Useful for notes.

    * `msg` : Associated text message.

## License

---

okjs is open source, made available under the [MIT License](http://www.opensource.org/licenses/mit-license.php).


## More Info

---

Currently, the best place to start is to look at the framework's self-test page, in the [/test/](test/) folder,
 or [online here](http://gkindel.com/okjs/test/test.html)


## Bugs & Contributions

This is an open source project, contributions and bug fixes are welcome.  Forking is encourage, or contact me (see Author section) to get access to the development branch.  Additionally, I encourage the use of GitHub for issue tracking.


## Author

--- 

Twitter: [@gkindel](http://twitter.com/#!/gkindel)
