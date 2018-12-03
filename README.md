# Javascript Interview Questions

### 1. What does `use strict;` do in JS?

Strict mode allows us to place a program or a function in what's called a strict operating context. In a nutshell -- this makes debugging easier, code error that would otherwise have been ignored or would have failed silently, will now generate errors or throw exceptions. This is extremely helpful as it alerts soon enough that there is a problem in your code and directs you quickly to the probable source.
To use `use strict;`, just place the string on top of the js file or function. You might ask, why is this a string in the first place? When use strict was implemented in the language, they had to take into account the legacy code for older browsers that wouldnt have supported it if new keywords like `use` or `strict` would have been introduced. Using a string allows newer browser to use the context and the older browser to discard / ignore the string on top of the file. If you're working on legacy code, you can very well use strict mode just for your functions. The rest of the program will function as usual in a non-strict mode.
Now that we have some pointers about what `use strict` is, let's see what exactly it does and how it can help us.

* Use of variables before they are declared throw an error. In non-strict mode the engine automatically create a variable age on the global object (`window.age = 18` in browsers or `global.age = 18` in nodejs). When using `use strict` this is not allowed --

  ```javascript
  "use strict;"
  age = 18 // Reference error: age is not defined
  let age; // Variable declarations are hoisted
  console.log(age) // 18
  ```

* Stops us from using keywords that are reserved for future versions of javascript

  ```javascript
  var let = 1; //ok
  function check() {
    "use strict;"
    var let = 2 // SyntaxError: reserved word
  }
  ```

* We cannot delete functions, variables or function arguments

  ```javascript
  var foo = 2
  delete foo // ok

  function bar() {}
  delete bar //ok

  function baz(arg) {
    delete arg // ok
  }

  function fizz(arg) {
    "use strict;"
    var a = 1
    delete a // SyntaxError
    delete args // SyntaxError
  }
  ```

* Makes `eval` safer

  ```javascript
  var x = 2
  eval("var x = 1")
  console.log(x) // 1 - x leaked outside the function and redeclared x
  function check() {
    "use strict;"
    var x = 6
    eval("var x = 3")
    console.log(x) // 6
  }
  ```

### 2. What are the different types in javascript?

Javascript has six types -- five primitive (Boolean, Number, String, Null, Undefined) and one non-primitive (objects). `typeof` function returns the type of the passed-in argument

```javascript
  typeof(1) // "number"
  typeof(true) // "boolean"
  typeof([]) // "object"
  typeof(null) // "object" (actually is null, it's an old js mistake)
```

`undefined` is what javascripts assigns to a declared variable that wasn't yet assigned a value. `null` is used when the programer himself assigns a null value to a variable. Whereas in other languages `null` represents an absence of value, in javascript `null` is its own type and has its own value *null*.

### 3. Passed by value or by reference

In javascript, primitive types (strings, booleans, numbers) are passed by value; objects on the other hand -- by reference. What does this exactly means? Let's look at an example:

```javascript
  var a = 5;
  function foo(a) {
    a = 7
  }
  foo(a)
  console.log(a) // 5
```

As we can see, we can't change the value of an outside variable from the inner scope of a function. The variable passed to `foo` is actually a copy of `a` and changes made in the function body are local to that function scope. This means that `a` as primitive value (number) is passed by value.
Passing by reference means that we dont actually passing the copy of the variable but a reference to it. In javascript objects are passed by reference, so we can change object properties of the passed in object and the changes will be reflected in the outer(original) object. A caveat here is that we can't change what actually `a` points to, we can just change properties on that said object:

```javascript
  var a = {};
  function foo(a) {
    a.baz = false // a is a reference (pointer) to the outer a object
    a = { moo: 3} // reassignment doesn't work
  }
  foo(a);
  console.log(a); // Object {baz: false}
```
