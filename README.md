## <a name='TOC'>Table of Contents</a>

1. [Conventions](#conventions)

1. [javaScript Style](#javascript)
  1. [Literals](#js-literals)
  1. [Strings](#js-strings)
  1. [Objects](#js-objects)
  1. [Functions](#js-functions)
  1. [Variables](#js-variables)
  1. [Hoisting](#js-hoisting)
  1. [Whitespace](#js-whitespace)
  1. [Semicolons](#js-semicolons)
  1. [Type Coercion](#js-type-coercion)
  1. [JQuery](#js-jquery)
  1. [Accessors](#js-accessors)
1. [coffeeScript Style](#coffeescript.md)

1. [Tools](#tools)
  1. [linting](#linting)
  1. [editors](#editors)

    1. [sublime](#sublime)
    1. [vim](#vim)

  1. [debugging](#debugging)

1. [Resources](#resources)
  1. [Docs](#docs)
  1. [Books](#books)
  1. [Podcasts](#podcasts)
  1. [News](#news)
  1. [Compatibility](#compatibility)
  1. [Performance](#performance)


## <a name='conventions'>Conventions</a>

  * Write new code in coffeescript.
  * Use soft-tabs with a two space indent.
  * CamelCase for functions and classes, underscores for variables
  * prefix jQuery/ender-qwery elements with ``$`` like ``$taco = $('.js-taco')``

**[[⬆]](#TOC)**

## <a name='javascript'>JavaScript</a>

### <a name='js-literals'>Literals</a>

  - Use literals whenever possible

  ** Why? **

  * The Array, Object, etc constructors are ambiguous in how they deal with their
  paramameters and they can be overriden.

  Example:

  ```javascript
    Array = 5;
    arr   = new Array();

    //TypeError: number is not a function
  ```

  Also:

  * They are shorter.
  * Linters like them.

### <a name='js-strings'>Strings</a>

  - Use single quotes for strings

  ```javascript
  // bad
  var name = "Bob Parr";

  // good
  var name = 'Bob Parr';
  ```

  **Why?**

  Double quoted strings imply interpolation.

  Although there is no native string interpolation in javaScript, there is in
  CoffeeScript.

  [Read more about this](http://coffeescript.org/#strings).

**[[⬆]](#TOC)**

### <a name='js-objects'>Objects</a>

- Use dot notation when accessing properties.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - Use subscript notation `[]` when accessing properties with a variable or when
   that property name is a reserved word.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

  **Why?**

  The dot notation is less verbose and very common in many programming languages.


- Avoid using [reserved words](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Reserved_Words?redirectlocale=en-US&redirectslug=JavaScript%2FReference%2FReserved_Words) for properties

  ```javascript
  //bad
  //Syntax error prior to ECMAScript 5
  var options = {
      default: {
        bacon: 'nomnom'
      }
  };

  //better
  var options = {
      'default': {
        bacon: 'nomnom'
      }
  };
  ```


**[[⬆]](#TOC)**


### <a name='js-functions'>Functions</a>

- Prefer function expressions to declarations

```javascript
//function declaration
function foo() {}
```


```javascript
//function expression
var foo = function() {};
```

**Why?**

* Function declarations get [hoisted](http://bonsaiden.github.io/JavaScript-Garden/#function.scopes) before the execution of the program starts.

- Avoid declaring functions inside loops

The reasons have to do with performance

```javascript
//bad
for (i = 0; i < 10000; i++) {
  doSomething = function() {
    //code that does something here..
  };

  doSomething();
}
```


```javascript
//good
doSomething = function() {
  //code that does something here..
};

for (i = 0; i < 10000; i++) {f
  doSomething();
}
```

**[[⬆]](#TOC)**


### <a name='js-variables'>Variables</a>

- Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

  ```javascript
  // bad
  superPower = new SuperPower();

  // good
  var superPower = new SuperPower();
  ```

- Use one `var` declaration for multiple variables and declare each variable on a newline.

  ```javascript
  // bad
  var items = getItems();
  var goSportsTeam = true;
  var dragonball = 'z';

  // good
  var items = getItems(),
      goSportsTeam = true,
      dragonball = 'z';
  ```

- Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

  ```javascript
  // bad
  var i, len, dragonball,
      items = getItems(),
      goSportsTeam = true;

  // bad
  var i, items = getItems(),
      dragonball,
      goSportsTeam = true,
      len;

  // good
  var items = getItems(),
      goSportsTeam = true,
      dragonball,
      length,
      i;
  ```

- Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment [hoisting](#hoisting) related issues.


  ```javascript
  // bad
  function() {
    test();
    console.log('doing stuff..');

    //..other stuff..

    var name = getName();

    if (name === 'test') {
      return false;
    }

    return name;
  }

  // good
  function() {
    var name = getName();

    test();
    console.log('doing stuff..');

    //..other stuff..

    if (name === 'test') {
      return false;
    }

    return name;
  }

  // bad
  function() {
    var name = getName();

    if (!arguments.length) {
      return false;
    }

    return true;
  }

  // good
  function() {
    if (!arguments.length) {
      return false;
    }

    var name = getName();

    return true;
  }
  ```

**[[⬆]](#TOC)**


## <a name='js-hoisting'>Hoisting</a>

  - Variable declarations get hoisted to the top of their scope, their assignment does not.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope.
    // Which means our example could be rewritten as:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

- Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

- Named function expressions hoist the variable name, not the function name or the function body.

  ```javascript
  function example() {
    console.log(named); // => undefined

    named(); // => TypeError named is not a function

    superPower(); // => ReferenceError superPower is not defined

    var named = function superPower() {
      console.log('Flying');
    };


    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      };
    }
  }
  ```

- Function declarations hoist their name and the function body.

  ```javascript
  function example() {
    superPower(); // => Flying

    function superPower() {
      console.log('Flying');
    }
  }
  ```

- For more information refer to one of the following:
  - [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)
  - [JavaScript Garden - Scopes](http://bonsaiden.github.io/JavaScript-Garden/#function.scopes)

    **[[⬆]](#TOC)**


### <a name='js-whitespace'>Whitespace</a>

- Place 1 space before the leading brace.


  ```javascript
  // bad
  function test(){
    console.log('test');
  }

  // good
  function test() {
    console.log('test');
  }

  // bad
  dog.set('attr',{
    age: '1 year',
    breed: 'Bernese Mountain Dog'
  });

  // good
  dog.set('attr', {
    age: '1 year',
    breed: 'Bernese Mountain Dog'
  });
  ```


- Place an empty newline at the end of the file.


  ```javascript
  // bad
  (function(global) {
    // ...stuff...
  })(this);
  ```


  ```javascript
  // good
  (function(global) {
    // ...stuff...
  })(this);
  ```

- Use indentation when making long method chains.

  ```javascript
  // bad
  $('#items').find('.selected').highlight().end().find('.open').updateCount();

  // good
  $('#items')
    .find('.selected')
      .highlight()
      .end()
    .find('.open')
      .updateCount();


  // bad
  var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
      .attr('width',  (radius + margin) * 2).append('svg:g')
      .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
      .call(tron.led);

  // good
  var leds = stage.selectAll('.led')
      .data(data)
    .enter().append('svg:svg')
      .class('led', true)
      .attr('width',  (radius + margin) * 2)
    .append('svg:g')
      .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
      .call(tron.led);
  ```

- Insert one extra space before and after operators

  ```javascript
  //bad
  foo = 'bar';

  //bad
  c = a+b;

  //good
  c = a + b;
  ```

- Do not include extra spaces around arguments

  ```javascript
  //bad
  foo( options );

  //good
  foo(options)
  ```

- Assignments should be vertically aligned

  ```javascript
  //bad
  var a = 'once',
      little = 'little',
      bird = 'bird',
      told = 'told',
      me = 'me';

  //good
  var a      = 'once',
      little = 'little',
      bird   = 'bird',
      told   = 'told',
      me     = 'me';

  //you may also leave an empty line after var
  // like (easier to use with editor alignment plugins)
  var
      a      = 'once',
      little = 'little',
      bird   = 'bird',
      told   = 'told',
      me     = 'me';
    ```

  **[[⬆]](#TOC)**

### <a name='js-semicolons'>Semicolons</a>

- **Yup.**

  No reason to make assumptions about [ASI](http://es5.github.io/#x7.9)


  [read more](http://benalman.com/news/2013/01/advice-javascript-semicolon-haters/)

  ```javascript
  // bad
  (function() {
    var name = 'Skywalker'
    return name
  })()

  // good
  (function() {
    var name = 'Skywalker';
    return name;
  })();

  // good
  ;(function() {
    var name = 'Skywalker';
    return name;
  })();
  ```

  **[[⬆]](#TOC)**




### <a name='js-type-coercion'>Type Casting & Coercion</a>


- Perform type coercion at the beginning of the statement.


* Strings:

  ```javascript
  //  => this.reviewScore = 9;

  // bad
  var totalScore = this.reviewScore + '';

  // good
  var totalScore = '' + this.reviewScore;

  // bad
  var totalScore = '' + this.reviewScore + ' total score';

  // good
  var totalScore = this.reviewScore + ' total score';
  ```

- Use `parseInt` for Numbers and always with a radix for type casting.
  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

  ```javascript
  var inputValue = '4';

  // bad
  var val = new Number(inputValue);

  // bad
  var val = +inputValue;

  // bad
  var val = inputValue >> 0;

  // bad
  var val = parseInt(inputValue);

  // good
  var val = Number(inputValue);

  // good
  var val = parseInt(inputValue, 10);

  // good
  /**
   * parseInt was the reason my code was slow.
   * Bitshifting the String to coerce it to a
   * Number made it a lot faster.
   */
  var val = inputValue >> 0;

  //also good
  var val = ~~inputValue;
  ```

- Booleans:

  ```javascript
  var age = 0;

  // bad
  var hasAge = new Boolean(age);

  // good
  var hasAge = Boolean(age);

  // good
  var hasAge = !!age;

  //also good
  var hasAge = age ? true : false;
    ```

  **[[⬆]](#TOC)**

### <a name='js-jquery'>jQuery</a>

- Prefix jQuery object variables with a `$`.

  ```javascript
  // bad
  var sidebar = $('.sidebar');

  // good
  var $sidebar = $('.sidebar');
  ```

- Cache jQuery lookups.

  ```javascript
  // bad
  function setSidebar() {
    $('.sidebar').hide();

    // ...stuff...

    $('.sidebar').css({
      'background-color': 'pink'
    });
  }

  // good
  function setSidebar() {
    var $sidebar = $('.sidebar');
    $sidebar.hide();

    // ...stuff...

    $sidebar.css({
      'background-color': 'pink'
    });
  }
  ```

- Scope your DOM queries properly
  Try make the selector engine use [querySelectorAll](https://developer.mozilla.org/en-US/docs/Web/API/Document.querySelectorAll)

  also good read: [w3c selectors-api](http://www.w3.org/TR/selectors-api/)

  ```javascript
  // bad
  // it translates to $()
  $('.sidebar', 'ul').hide();

  //bad
  // internally uses $.sibling to find nodes following
  // other nodes in the same tree.
  $('.sidebar').children('ul').hide();

  // good
  $('.sidebar').find('ul').hide();


  // good
  // uses
  $('.sidebar ul').hide();

  // good
  $('.sidebar > ul').hide();
  ```

  **[[⬆]](#TOC)**


### <a name='accessors'>Accessors</a>

- Accessor functions for properties are not required
  - If you do make accessor functions use getVal() and setVal('hello')

  ```javascript
  // bad
  dragon.age();

  // good
  dragon.getAge();

  // bad
  dragon.age(25);

  // good
  dragon.setAge(25);
  ```

- If the property is a boolean, use isVal() or hasVal()

  ```javascript
  // bad
  if (!dragon.age()) {
    return false;
  }

  // good
  if (!dragon.hasAge()) {
    return false;
  }
  ```

- It's advised to create get() and set() functions, but be consistent.

  ```javascript
  function Jedi(options) {
    options || (options = {});
    var lightsaber = options.lightsaber || 'blue';
    this.set('lightsaber', lightsaber);
  }

  Jedi.prototype.set = function(key, val) {
    this[key] = val;
  };

  Jedi.prototype.get = function(key) {
    return this[key];
  };
  ```


- Methods can return `this` to help with method chaining.

  ```javascript
  // bad
  Jedi.prototype.jump = function() {
    this.jumping = true;
    return true;
  };

  Jedi.prototype.setHeight = function(height) {
    this.height = height;
  };

  var luke = new Jedi();
  luke.jump(); // => true
  luke.setHeight(20) // => undefined

  // good
  Jedi.prototype.jump = function() {
    this.jumping = true;
    return this;
  };

  Jedi.prototype.setHeight = function(height) {
    this.height = height;
    return this;
  };

  var luke = new Jedi();

  luke.jump()
    .setHeight(20);
  ```


**[[⬆]](#TOC)**


## <a name='tools'>Tools</a>
### Editors
#### <a name='sublime'>Sublime Text</a>
Set soft-tabs, rulers according to [Conventions](#conventions)

Go to `Preferences -> Settings - User` and insert:

    "translate_tabs_to_spaces": true,
    "tab_size": 2,
    "rulers": [ 80, 90 ]

##### Essential plugins
- [emmet](https://github.com/sergeche/emmet-sublime)
- [alignment](http://wbond.net/sublime_packages/alignment)
- [jshint](https://github.com/uipoet/sublime-jshint)
- [sublime-better-coffeescript](https://github.com/aponxi/sublime-better-coffeescript)

#### <a name='vim'>vim</a>
Set soft-tabs, rulers according to [Conventions](#conventions)

In your `.vimrc` insert:

    set expandtab
    set tabstop=2
    set shiftwidth=2
    set smarttab
    set colorcolumn=80,90

##### Essential Plugins
- [synctastic](https://github.com/scrooloose/syntastic)

#### Linting
- [jsHint](http://www.jshint.com/)
- [coffeLint](http://www.coffeelint.org/)

#### <a name='debugging'>Debugging</a>
- Chrome Dev Tools

  - Mobile

    Connect an Android device via USB to the desktop & Debug code executing on the device with the Chrome Developer Tools

  ```
  $> adb forward tcp:9222 localabstract:chrome_devtools_remote
  $> chrome.exe --remote-debugging-port=9222
  ```

  **Resources**
  - [official guide](https://developers.google.com/chrome-developer-tools/)
  - [codeschool course](http://discover-devtools.codeschool.com/)


- Firebug ([download](http://getfirebug.com/))

**[[⬆]](#TOC)**

## <a name='resources'>Resources</a>

### <a name='docs'>Docs & Guides</a>

- [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript?redirectlocale=en-US&redirectslug=JavaScript)
- [webplatform](http://webplatform.org)
- [w3c javaScript APIs](http://www.w3.org/standards/techs/js#w3c_all)
- [Annotated ECMAScript 5.1](http://es5.github.com/)
- [javaScript Garden](http://bonsaiden.github.io/JavaScript-Garden/)

### <a name='styleguides'>Inspiring Styleguides</a>

* [github](https://github.com/styleguide/javascript)
* [jQuery](http://contribute.jquery.org/style-guide/js/?rdfrom=http%3A%2F%2Fdocs.jquery.com%2Fmw%2Findex.php%3Ftitle%3DJQuery_Core_Style_Guidelines%26redirect%3Dno)
* [airbnb](https://github.com/airbnb/javascript)
* [idiomatic](https://github.com/rwldrn/idiomatic.js/)

- [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
- [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52)

### <a name='books'>Books</a>

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [The Little Book on CoffeeScript](http://arcturo.github.io/library/coffeescript/) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch

### <a name='news'>News</a>

  - [echojs](http://www.echojs.com/)
  - [DailyJS](http://dailyjs.com/)
  - [html5rocks](http://www.html5rocks.com/en/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)
  - [webplatform](http://blog.webplatform.org/)
  - [creativejs](http://creativejs.com/)

### <a name='podcasts'>Podcasts</a>

- [shoptalk](http://shoptalkshow.com/)
- [javaScript jabber](http://javascriptjabber.com/)
- [yayQuery](http://yayquery.com/)

### <a name='compatibility'>Compatibility</a>

- [quirksmode](http://www.quirksmode.org/)
- [caniuse](http://caniuse.com)
- [html5please](http://html5please.com/)


### <a name='performance'>Performance</a>

- [Google Devs - Optimizing javaScript](https://developers.google.com/speed/articles/optimizing-javascript)

- [html5rocks - v8](http://www.html5rocks.com/en/tutorials/speed/v8/)

**Remember:** When in doubt, benchmark.

 - [jsperf.com](http://jsperf.com)

 - [benchmarkjs](http://benchmarkjs.com/)


**[[⬆]](#TOC)**