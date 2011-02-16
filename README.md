Light Traits
============

Very light-weight trait composition library for Javascript based on the new object
manipulation API defined in [Ecmascript-262, Edition 5](ES5)

Traits
------

Traits are a flexible language feature to factor out and (re)combine reusable
pieces of code. They are a more robust alternative to multiple inheritance or
mixins. They are more robust because name clashes must be resolved explicitly
by composers, and because trait composition is order-independent (hence more
declarative). To put it more bluntly: if you combine two traits that define a 
method with the same name, your program will fail. Traits won't automatically 
give precedence to either one.

Light Traits
------------

This library is inspired by an awesome [traitsjs] and pretty much follows it's
design with a few slight differences:

###Lighter###

This implementation is more optimal in terms of object instantiation speed and
memory footprint. This is achieved by not binding the `this` pseudo variable in 
all accessors and methods of the composed object to an instance. While this allows
shared functions across objects, it also matches better overall behavior of the
language. 
Note: a similar thing can be achieved in [traitsjs] as well in which case the
claim of being more optimal will not longer be true. It's just not a default
behavior, at least not yet :)_

###Better ergonomics###

This is a bold statement and maybe not entirely correct but, few API differences
makes working with traits less verbose and therefore more pleasant. Take a look
at the examples section to see the difference.

###Ecmascript 5 shims are not included###

If your JavaScript engine does not comes with all the shiny [Ecmascript 5][ES5]
features this library will not work for you. Luckily, there are other libraries
that shim JavaScript engines, so that might be an option for you.

Examples
--------

      function ColorTrait(color) {
          return Trait({ color: function() { return color; } });
      }

      var pointTrait = Trait(
          ColorTrait('red'), // a color trait
          Trait({
              _x: Trait.required,
              _y: Trait.required,
              get x() { return this._x },
              get y() { return this._y },
              toString: function toString() { return "" + this.x + " by " + this.y }
          })
      );

      function Point(x, y) {
          // result inherits from object being passed to create
          return pointTrait.create({ _x: x, _y: y });
      }

      var point = Point(1, 7);
      point.color();    // -> 'red'
      point.toString(); // -> '1 by 7'


[traitsjs]:http://www.traitsjs.org/
[ES5]:http://www.ecma-international.org/publications/standards/Ecma-262.htm
