# Skroutz JavaScript Style Guide

- [Rationale](#rationale)
- [Conventions](#conventions)
  - [Code conventions](#code-conventions)
  - [Naming conventions](#naming-conventions)
- [CoffeeScript](#coffeescript)
  - [Literals](#literals)
  - [Strings](#strings)
  - [Objects](#objects)
  - [Functions](#functions)
  - [Variables](#variables)
  - [Whitespace](#whitespace)
  - [Comments](#comments)
  - [Type casting & coercion](#type-casting-&-coercion)
  - [DOM manipulation](#dom-manipulation)
  - [Common pitfalls](#common-pitfalls)
  - [Other](#other)
- [Tools](#tools)
  - [Editors](#editors)
    - [Vim](#vim)
    - [Sublime Text](#sublime-text)
  - [Essential plugins](#essential-plugins)
    - [Vim](#vim-1)
    - [Sublime Text](#sublime-text-1)
  - [Lint](#lint)
  - [Debugging](#debugging)
- [Resources](#resources)
  - [Docs & guides](#docs-&-guides)
  - [Inspiring styleguides](#inspiring-styleguides)
  - [Books](#books)
    - [JavaScript](#javascript)
    - [CoffeeScript](#coffeescript-1)
  - [News](#news)
  - [Podcasts](#podcasts)
  - [Compatibility](#compatibility)
  - [Performance](#performance)

## Rationale

* 80% of the lifetime cost of software goes to maintenance.

* Hardly any software is maintained for its whole life by the original author.

* Code conventions improve readability, allowing engineers to understand new code more quickly and thoroughly.

## Conventions

### Code conventions

* Write new code in CoffeeScript.

* Use soft-tabs with a **2-space** indent (this means pressing the tab key once should produce 2 space characters).

* Try to keep line length **80-90** characters at most.

* Make sure you do not accidentally include trailing whitespace.

### Naming conventions

* Use **CamelCaps** for class names and constructors.

* Use **camelCase** for functions/methods.

* Use **snake_case** for variable names.

* Use a leading underscore (e.g. `_foo`) only for variables and methods that are intended to be "private".

* Use **CAPS** for constants.

* Prefix jQuery object variables with a `$`:

  ```coffeescript
  sidebar = $('.sidebar') # bad

  $sidebar = $('.sidebar') # good
  ```

## CoffeeScript

### Literals

* Prefer literals (`arr = []`) over native constructors (`arr = new Array()`). `Array`, `Object`, etc constructors are ambiguous in how they deal with their paramameters and they can be overriden. Also, literals are shorter and lint tools like them.

### Strings

* Prefer string interpolation over concatenation:

  ```coffeescript
    name = 'Maria'

    console.log 'Ase mas re ' + name + '!' # avoid

    console.log "Ase mas re #{name}!" # good
  ```

* Use single quotes for strings when not interpolating:

  ```coffeescript
    name = "Bob Parr" # bad

    name = 'Bob Parr' # good
  ```

* Use block strings when it improves readability:

  ```coffeescript
    my_msg = 'Password too short!'
    msg_div = """
              <div class="warning_message">
                <p>
                  <span>Warning:</span>
                  #{my_msg}
                </p>
              </div>
              """
  ```

### Objects

* Prefer **dot notation** when accessing properties:

  ```coffeescript
    luke =
      jedi: true
      age: 28
  
    isJedi = luke['jedi'] # bad
  
    isJedi = luke.jedi # good
  ```

* Use subscript notation `[]` when accessing properties dynamically (with a variable) or when that property name is a language keyword:

  ```coffeescript
    options =
      'default':
        bacon: 'nomnom'
    my_key = 'default'
  
    default_food = options['default']
    default_food = options[my_key] # same
  ```

* When defining an object in many lines, do not use commas:

  ```coffeescript
    # good (no commas)
    luke =
      jedi:  true
      age:   28
      human: true
  
    # bad (commas)
    luke =
      jedi:  true,
      age:   28,
      human: true
  
    # bad bad bad (mixed)
    luke =
      jedi:  true,  # comma
      age:   28     # no comma
      human: true
  ```

### Functions

* Do not use parentheses when defining functions that take no arguments:

  ```coffeescript
    # bad
    foo = () ->
      console.log('aha!')

    # good
    foo = ->
      console.log('aha!')
  ```

* Parentheses in function calls may be omitted with respect to readability and clarity. Some examples:

  ```coffeescript
  foo() # you cannot omit () if there are no arguments!

  foo 4

  foo(4).bar(5)

  foo.getSize(5, 6) / foo.getSize(6, 5)
  ```

* Avoid "function grouping":

  ```coffeescript
  (foo 4).bar 8 # bad

  foo(4).bar 8 # good
  ```

* Avoid creating functions inside loops since it's bad for performance:

  ```coffeescript
    # bad
    for i in [1..100]
      doSomething = ->
        console.log(i)

      doSomething()

    # good
    doSomething = (i) ->
      console.log(i)

    for i in [1..100]
      doSomething(i)
  ```

* Avoid explicit `return` statements unless it's for an "early return". For example:

  ```coffeescript
  browser = getBrowser()
  return 'oh no!' if 'ie6'

  # ...cool stuff ie6 doesn't support...
  ```

* Class methods should return `this` to enable method chaining. For example:

  ```coffeescript
    Jedi = ->
      @jumping = false
      @yelling = false

    Jedi::jump = ->
      @jumping = true
      return this

    Jedi::yell = ->
      @yelling = true
      return this

    first = new Jedi
    first.jump().yell()
  ```

### Variables

* Initialize variables at the top of their scope, preferably in a grouped block.

  ```coffeescript
  # TODO add example
  ```

### Whitespace

* Place an empty newline at the end of the file (most modern editors will do this automatically for you).

* Place one space before arrows:

  ```coffeescript
  test =-> print 'test' # bad

  test = -> print 'test' # good

  test2 = (arg)-> print arg # bad

  test2 = (arg) -> print arg # good
  ```

* Insert an extra space before and after operators:

  ```coffeescript
  foo='bar' # bad

  foo = 'bar' # good

  c = a+b # bad

  c = a + b # good
  ```

* Insert an extra space after `,` and `:`:

  ```coffeescript
  foo: [1,2,3] # bad

  foo: [1, 2, 3] # good

  foo:'bar' # bad

  foo: 'bar' # good
  ```

* Avoid spaces around arguments and before a comma `,`:

  ```coffeescript
  foo( options, 'bar' ) # bad
  foo(options , 'bar') # bad

  foo(options, 'bar') # good
  ```

* Avoid spaces around conditional constructs, loops, etc..

  ```coffeescript
  if(true) #bad
  
  if (true) #better
  
  if true #good
  ```

* Assignments should be vertically aligned unless they differ a lot in length or they are less than 3:

  ```coffeescript
  # bad
  a = 'once'
  little = 'little'
  bird = 'bird'
  told = 'told'
  me = 'me'

  # good
  a      = 'once'
  little = 'little'
  bird   = 'bird'
  told   = 'told'
  me     = 'me'

  # bad
  a                        = 'once'
  little                   = 'little'
  bird_told_me_some_things = 'foo'

  # bad (this is too much)
  a      = 'once'
  little = 'little'
  ```

* Break long method chains into many lines, keeping the dot at the beginning of each line:

  ```coffeescript
    # bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount()

    # good
    $('#items')
      .find('.selected')
      .highlight()
      .end()
      .find('.open')
      .updateCount()

    # even better
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount()
  ```

### Comments

* Do not write comments to state the obvious.

* When modifying code, don't forget to update the corresponding comment. Ideally, improve the code to obviate the need for the comment and delete the comment entirely.

* Do not write comments in CAPS (`TODO`, `FIXME`, etc are notable exceptions).

* Short comments should be placed exactly above the code they document, unless they are short enough to be placed on the same line (inline comment).

* You are free to write block comments using either the triple `#` syntax (`###`) or by prefixing each line with a `#`:

  ```coffeescript
  # This
  # is
  # a
  # block
  # comment

  ###
  And another one.
  This style helps the maintainance of documentation.
  Each time I want to add or remove a line, I don't have to mess with any #.
  I also want this comment to be passed to the generated JavaScript.
  ###
  ```

### Type casting & coercion

* Use `parseInt` with a radix for type casting to integers. If, for whatever reason, you are doing something wild and `parseInt` is your bottleneck and need to use bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

  ```coffeescript
  inputValue = '4'

  # bad
  val = parseInt inputValue

  # good
  val = parseInt(inputValue, 10)

  # good
  val = ~~inputValue # comment here
  ```

* Converting to a boolean:

  ```coffeescript
  age = 0

  # bad
  hasAge = Boolean age

  # good
  hasAge = !!age

  # good (ternary operator equivalent)
  hasAge = if age then true else false
  ```

### DOM manipulation

* You should be able to tell a presentational class attribute from a functional one:

  ```coffeescript
  $my_carousel = $('.carousel') # bad

  $my_carousel = $('.js-carousel') # good
  ```

* Assumption is the mother of all fckups. Do not code with assumptions about DOM hierarchy/state.

* Cache DOM lookups:

  ```coffeescript
    # bad
    setSidebar = ->
      $('.sidebar').hide()
      # ...stuff...
      $('.sidebar').css({'background-color': 'pink'})

    # good
    setSidebar = ->
      $sidebar = $('.sidebar')
      $sidebar.hide()
      # ...stuff...
      $sidebar.css({'background-color': 'pink'})
  ```

* Think about performance when writing DOM queries. Try to make the selector engine use the browser's [querySelectorAll method](https://developer.mozilla.org/en-US/docs/Web/API/Document.querySelectorAll). Also a good read: [w3c selectors-api](http://www.w3.org/TR/selectors-api/)

  ```coffeescript
  # slow
  # uses $.sibling internally to find nodes following other nodes in the same tree
  $('.sidebar').children('ul').hide()

  # faster
  $('.sidebar').find('ul').hide()
  ```

### Common pitfalls

* Have a look at the [full list of CoffeeScript aliases](http://coffeescript.org/#operators). Some are notorious for causing confusion to CoffeeScript n00bs.

* Avoid `==` and `!=` as they compile to `===` and `!==` that may be confusing/unwanted.

* Use `@` instead of `this`.

* Use fat arrows `=>` in a function definition when you need to bind to the function the current context (`this`).

* Learn about the [Existential Operator](http://coffeescript.org/#operators) `?` and use it.

* Do not confuse the existential operator `?` with the JavaScript ternary operator. The ternary operator in CoffeeScript is a single-line `if`:

  ```coffeescript
  my_state = if 6 then 'go!' else 'work!'
  ```

* Use the more explicit `if foo?` (will execute unless `foo` is `undefined` or `null`) instead of `if foo`.

### Other

* Do not use `unless` since it's really confusing. Use `if !` or `if not` instead.

* When a `.coffee` file is to be preprocessed by let's say `erb` *avoid* nesting quotes of the 
same type, as it breaks linting and may make it unparseable by other preprocessors.

Example

```
message = '<%= _('ΣΜΣ') %>' # bad

message = '<%= _("ΣΜΣ") %>' # good
```


## Tools

### Editors

#### Vim

Set soft-tabs and rulers according to conventions.

In your `~/.vimrc` add:

    set expandtab
    set tabstop=2
    set shiftwidth=2
    set smarttab
    set colorcolumn=80,90

#### Sublime Text

Set soft-tabs and rulers according to conventions.

Go to "Preferences -> Settings - User" and add:

    "translate_tabs_to_spaces": true,
    "tab_size": 2,
    "rulers": [ 80, 90 ]

### Essential plugins

#### Vim

* [synctastic](https://github.com/scrooloose/syntastic)

#### Sublime Text

* [emmet](https://github.com/sergeche/emmet-sublime)
* [alignment](http://wbond.net/sublime_packages/alignment)
* [jshint](https://github.com/uipoet/sublime-jshint)
* [sublime-better-coffeescript](https://github.com/aponxi/sublime-better-coffeescript)

### Lint

* [jsHint](http://www.jshint.com/)
* [CoffeeLint](http://www.coffeelint.org/)

### Debugging

* [Chrome Dev Tools](https://developers.google.com/chrome-developer-tools/)

* Mobile

  Connect an Android device via USB to the desktop. Enable debug code execution on the device with the Chrome Developer Tools.

  ```
  $> adb forward tcp:9222 localabstract:chrome_devtools_remote
  $> chrome.exe --remote-debugging-port=9222
  ```

* [Code School Course](http://discover-devtools.codeschool.com/)

* Firebug ([download](http://getfirebug.com/))

* Regular Expressions. They are a programmer's best and worst friend. Debug with [debuggex](http://www.debuggex.com/).

* LiveReload. Apply changes in JS, styles without refreshing: [extension](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei) [gem](https://github.com/guard/guard-livereload)

## Resources

### Docs & guides

* [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript?redirectlocale=en-US&redirectslug=JavaScript)
* [WebPlatform](http://webplatform.org)
* [W3C JavaScript APIs](http://www.w3.org/standards/techs/js#w3c_all)
* [Annotated ECMAScript 5.1](http://es5.github.com/)
* [JavaScript Garden](http://bonsaiden.github.io/JavaScript-Garden/)
* [wtfjs](http://wtfjs.com/) (Fun fun)

### Inspiring styleguides

* [Github](https://github.com/styleguide/javascript)
* [Airbnb](https://github.com/airbnb/javascript)
* [Idiomatic](https://github.com/rwldrn/idiomatic.js/)
* [polarmobile CoffeeScript style guide](https://github.com/polarmobile/coffeescript-style-guide)
* [Naming `this` in nested functions](https://gist.github.com/4135065)
* [Conditional callbacks](https://github.com/airbnb/javascript/issues/52)

### Books

#### JavaScript

* [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742)
* [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752)
* [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)
* [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309)
* [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680)
* [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X)
* [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273)
* [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595)

#### CoffeeScript

* [The Little Book on CoffeeScript](http://arcturo.github.io/library/coffeescript/)
* [CoffeeScript Cookbook](http://coffeescriptcookbook.com/)
* [CoffeeScript: Accelerated JavaScript Development](http://pragprog.com/book/tbcoffee/coffeescript)

### News

* [echojs](http://www.echojs.com/)
* [DailyJS](http://dailyjs.com/)
* [html5rocks](http://www.html5rocks.com/en/)
* [JavaScript Weekly](http://javascriptweekly.com/)
* [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
* [Bocoup Weblog](http://weblog.bocoup.com/)
* [Adequately Good](http://www.adequatelygood.com/)
* [NCZOnline](http://www.nczonline.net/)
* [Perfection Kills](http://perfectionkills.com/)
* [Ben Alman](http://benalman.com/)
* [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
* [Dustin Diaz](http://dustindiaz.com/)
* [nettuts](http://net.tutsplus.com/?s=javascript)
* [webplatform](http://blog.webplatform.org/)
* [creativejs](http://creativejs.com/)

### Podcasts

* [shoptalk](http://shoptalkshow.com/)
* [javaScript jabber](http://javascriptjabber.com/)
* [yayQuery](http://yayquery.com/)

### Compatibility

* [quirksmode](http://www.quirksmode.org/)
* [caniuse](http://caniuse.com)
* [html5please](http://html5please.com/)

### Performance

**Remember:** When in doubt, benchmark.

* [Google Devs - Optimizing javaScript](https://developers.google.com/speed/articles/optimizing-javascript)
* [html5rocks - v8](http://www.html5rocks.com/en/tutorials/speed/v8/)
* [jsperf.com](http://jsperf.com)
* [benchmarkjs](http://benchmarkjs.com/)
