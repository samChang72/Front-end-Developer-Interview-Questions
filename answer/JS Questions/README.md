#### JS Questions:

* Explain event delegation
  * Ans (Short): Bind event handler to a single parent to listen and handle event from children (through event bubbling).
  * Ans (Long):
      * DOM event delegation is a mechanism of responding to ui-events via a single common parent rather than each child, through the magic of event "bubbling" (aka event propagation). 
      * With event delegation the number of event bindings can be drastically decreased by moving them to a common parent element, and code that dynamically creates new elements on the fly can be decoupled from the logic of binding their event handlers.
      * Another benefit to event delegation is that the total memory footprint used by event listeners goes down (since the number of event bindings go down). It may not make much of a difference to small pages that unload often (i.e. user's navigate to different pages often). But for long-lived applications it can be significant.
  * Ref: [What is DOM Event delegation?](http://stackoverflow.com/questions/1687296/what-is-dom-event-delegation), the answer with highest vote up really good and should be accepted, btw.
* Explain how `this` works in JavaScript
  * Ans (Real): Well I know how to use it but Explain...
  * Ans (Cheated):
      * The ECMAScript Standard defines this as a keyword that "evaluates to the value of the ThisBinding of the current execution context"
      * When evaluating code in the initial global execution context (global scope), ThisBinding is set to the global object (if in browser, window)
      * If a function is called on an object, such as in `obj.myMethod()` or the equivalent `obj["myMethod"]()`, then ThisBinding is set to the object `obj`.
      * by a direct call to eval(), ThisBinding is left unchanged; it is the same value as the ThisBinding of the calling execution context
      * if not by a direct call to eval(), ThisBinding is set to the global object as if executing in the initial global execution context 
      * If call a function directly e.g. `myFun()`, then ThisBinding is set to global object.
      * There are some functions that allow you to specify ThisBinding
      ```
      Function.prototype.apply( thisArg, argArray )
      Function.prototype.call( thisArg [ , arg1 [ , arg2, ... ] ] )
      Function.prototype.bind( thisArg [ , arg1 [ , arg2, ... ] ] )
      Array.prototype.every( callbackfn [ , thisArg ] )
      Array.prototype.some( callbackfn [ , thisArg ] )
      Array.prototype.forEach( callbackfn [ , thisArg ] )
      Array.prototype.map( callbackfn [ , thisArg ] )
      Array.prototype.filter( callbackfn [ , thisArg ] )
      ```
      * When a function is called in a constructor context, the value of this is the new object that the interpreter created
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/the_this_keyword.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/the_this_keyword.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/the_this_keyword.js)
  * Ref: [How does the “this” keyword work?](http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work), [Scope in JavaScript](http://web.archive.org/web/20110725013125/http://www.digital-web.com/articles/scope_in_javascript/)
* Explain how prototypal inheritance works
  * Ans:
    * Explain what prototype first:
      * JavaScript only has one construct: objects
      * Each object has an internal link to another object called its prototype
      * That prototype object (of another object) has a prototype of its own, and so on until an object is reached with null as its prototype. null, by definition, has no prototype, and acts as the final link in this prototype chain.
    * how prototypal inheritance works
      * JavaScript objects are dynamic "bags" of properties (referred to as own properties). JavaScript objects have a link to a prototype object.
      * When trying to access a property of an object, the property will not only be sought on the object but on the prototype of the object, the prototype of the prototype, and so on until either a property with a matching name is found or the end of the prototype chain is reached.
  * Ref: [Inheritance and the prototype chain](https://developer.mozilla.org/en/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
* What do you think of AMD vs CommonJS?
  * Ans (Real): Actually I do not know both of them (:->).
  * Ans (Cheating): 
    * The difference:
      * AMD takes a browser-first approach, CommonJS on the other hand takes a server-first approach.
      * AMD using asynchronous behavior, CommonJS assuming synchronous behavior.
    * In my opinion the asynchronous behavior in AMD will cause more round trip, make longer loading/rendering time and also harder to read. On the other hand, CJS block browser more and seems less flexibility (for dynamic/lazy/defer loading).
  * Ref: [AMD vs Common JS & UMD](https://www.linkedin.com/pulse/amd-vs-common-js-umd-damodaran-sathyakumar).
* Explain why the following doesn't work as an IIFE: `function foo(){ }();`.
  * Ans (short): The `function foo(){ }` is not wrapped by `()`
  * Ans (long): When the parser encounters the function keyword it treats it as a function declaration (statement) by default, so it will parse it as
  ```javascript
  // the function declaration
  function foo(){ }
  // ??? what's this?
  ();
  ```
  Because JS is executed under two phase, compilation and execution, function declaration (`function foo(){ }` here) will be parsed and execued in compilation phase, then only `();` executed in execution phase which causes the SyntaxError.
  * What needs to be changed to properly make it an IIFE?
    * Ans: So what we need to do is turn it into function expression by changing `function foo` to `var foo = function` or wrapping `function foo () {}` in parens.
      * change to function expression
      ```javascript
      var foo = function(){
        // code to run
      }();
      ```
      * wrap with parens
      ```javascript
      (function foo() {
        // code to run
      })();
      ```
      * you can even remove its name
      ```javascript
      (function () {
        // code to run
      })();
      ```
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/IIFE_test.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/IIFE_test.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/IIFE_test.js)
  * Ref: [Immediately-Invoked Function Expression (IIFE)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/), [JavaScript function declaration and evaluation order](http://stackoverflow.com/questions/3887408/javascript-function-declaration-and-evaluation-order)
* What's the difference between a variable that is: `null`, `undefined` or undeclared?
  * Ans: 
    * `undeclared` variables don’t even exist
    * `undefined` variables exist, but don’t have anything assigned to them
    * `null` variables exist and have null assigned to them
  * How would you go about checking for any of these states?
    * Ans: As the code below, it can detect the status of a specific variable
    ```javascript
    function testStatus () {
        try { // here it is undeclared so will throw an exception
            if (val) {};
        } catch (e) { // just return the exception or some msg
            return e;
        }
        if ((typeof val) === 'undefined') // here it is undefined
            return 'val is undefined';
        if (val == null) // it is null
            return 'val is null';
    }
    ```
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/null_undefine_undeclare.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/null_undefine_undeclare.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/null_undefine_undeclare.js)
  * Ref: [JS: null, undefined, and undeclared](http://lucybain.com/blog/2014/null-undefined-undeclared/), [What is the difference in Javascript between 'undefined' and 'not defined'?](http://stackoverflow.com/questions/833661/what-is-the-difference-in-javascript-between-undefined-and-not-defined)
* What is a closure, and how/why would you use one?
  * Ans:
    * Closures are functions that refer to independent (free) variables. In other words, the function defined in the closure 'remembers' the environment in which it was created.
    * How/Why use it: For me the reason is I want something private, make global scope clean, and prevent any unexpected interaction/side-effect. e.g., assume making a game with JavaScript and you want keep the whole environment of the game private and safe.
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/closure.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/closure.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/closure.js)
  * Ref: [Closures - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
* What's a typical use case for anonymous functions?
  * Ans: The inline function, e.g., jQuery Event Handler
  ```javascript
  $('.selector').on('click', function (e) {
    // this is anonymous functions
  });
  ```
  or IIFE
  ```javascript
  (function () {
    //  IIFE with anonymous functions
  })();
  ```
* How do you organize your code? (module pattern, classical inheritance?)
  * Ans:
    * I use object literal to modulize/componentlize my code and handle prototype chain to simulate classical inheritance if needed.
    * I will write some APIs to wrap the common process of define/extend a `class` create instance and call super method, after that I can work with almost pure object literal.
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/code_organization.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/code_organization.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/code_organization.js)
  * Note:
    * The sample above just simple demo case, in real use you may separate js code to several different files and wrap them in different IIFE so they will have different scope.
    * This probably a little old fashion, today probably use some libraries (e.g., AMD/CommonJS) to help you organize your code.
  * Ref: The way I organize my code is learned from [ZK](https://www.zkoss.org/), See [zk.js](https://github.com/zkoss/zk/blob/v7.0.3/zk/src/archive/web/js/zk/zk.js#L211)
* What's the difference between host objects and native objects?
  * Ans:
    * host objects: object supplied by the host environment to complete the execution environment of ECMAScript. e.g., (assuming browser environment): `window`, `document`, `location`, `history`, `XMLHttpRequest`, `setTimeout`, ...
    * native objects: object in an ECMAScript implementation whose semantics are fully defined by this specification rather than by the host environment. e.g., `Object` (constructor), `Date`, `Math`, `parseInt`, `eval`, string methods like `indexOf` and `replace`, ...
  * Ref: [What is the difference between native objects and host objects?](http://stackoverflow.com/questions/7614317/what-is-the-difference-between-native-objects-and-host-objects)
* Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?
  * Ans:
    * `function Person(){}` declare a function named Person.
    * `var person = Person()` declare a variable named person, call Person function, and assign the value returned by Person function to variable person.
    * `var person = new Person()` declare a variable named person, create an object with the constructor function Person, and assign the created object to person variable.
    * (Doubt) is this the answer? Not sure what the question asking.
* What's the difference between `.call` and `.apply`?
* Explain `Function.prototype.bind`.
* When would you use `document.write()`?
* What's the difference between feature detection, feature inference, and using the UA string?
* Explain AJAX in as much detail as possible.
* Explain how JSONP works (and how it's not really AJAX).
* Have you ever used JavaScript templating?
  * If so, what libraries have you used?
* Explain "hoisting".
* Describe event bubbling.
* What's the difference between an "attribute" and a "property"?
* Why is extending built-in JavaScript objects not a good idea?
* Difference between document load event and document ready event?
* What is the difference between `==` and `===`?
* Explain the same-origin policy with regards to JavaScript.
* Make this work:
```javascript
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
```
* Why is it called a Ternary expression, what does the word "Ternary" indicate?
* What is `"use strict";`? what are the advantages and disadvantages to using it?
* Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, **"buzz"** at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`
* Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?
* Why would you use something like the `load` event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?
* Explain what a single page app is and how to make one SEO-friendly.
* What is the extent of your experience with Promises and/or their polyfills?
* What are the pros and cons of using Promises instead of callbacks?
* What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?
* What tools and techniques do you use debugging JavaScript code?
* What language constructions do you use for iterating over object properties and array items?
* Explain the difference between mutable and immutable objects.
  * What is an example of an immutable object in JavaScript?
  * What are the pros and cons of immutability?
  * How can you achieve immutability in your own code?
* Explain the difference between synchronous and asynchronous functions.
* What is event loop?
  * What is the difference between call stack and task queue?
