## <a name='TOC'>Table of Contents</a>
1. [Rationale](#rationale)
1. [Conventions](#conventions)
  1. [Code Conventions](#code-conventions)
  1. [Naming Conventions](#name-conventions)

1. [CoffeeScript Style](#coffeescript)
  1. [Literals](#js-literals)
  1. [Strings](#js-strings)
  1. [Objects](#js-objects)
  1. [Functions](#js-functions)
  1. [Variables](#js-variables)
  1. [Whitespace](#js-whitespace)
  1. [Comments](#js-comments)
    1. [Inline Comments](#js-inline)
    1. [Block Comments](#js-block)
  1. [Type Coercion](#js-type-coercion)
  1. [DOM Manipulation](#js-jquery)
  1. [Common pitfalls](#js-pitfalls)

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

## <a name='rationale'>Rationale</a>

  * 80% of the lifetime cost of software goes to maintenance.

  * Hardly any software is maintained for its whole life by the original author.

  * Code conventions improve readability, allowing engineers to understand new code more quickly and thoroughly.

## <a name='conventions'>Conventions</a>

### <a name='code-conventions'>Code Conventions</a>

  * Write new code in CoffeeScript.

  * Use soft-tabs with a **2-space** indent (this means pressing the tab key once should produce 2 space characters).

  * Try to keep line length **80-90** characters at most.

  * Make sure you do not accidentally include trailing whitespace.

### <a name='name-conventions'>Naming Conventions</a>

  * Use **CamelCaps** for class names and constructors.

  * Use **camelCase** for functions/methods.

  * Use **snake_case** for variable names.

  * Use a leading underscore (e.g. `_foo`) only for variables and methods that are intended to be "private".

  * Use **CAPS** for constants.

  * Prefix jQuery object variables with a `$`:

  ```coffeescript
  sidebar = $('.sidebar') # bad

  $sidebar = $('.sidebar') #good
  ```

**[[⬆]](#TOC)**

## <a name='coffescript'>CoffeeScript</a>

### <a name='js-literals'>Literals</a>

  - Prefer literals (`arr = []`) over native constructors (`arr = new Array()`). `Array`, `Object`, etc constructors are ambiguous in how they deal with their paramameters and they can be overriden. Also, literals are shorter and lint tools like them.

### <a name='js-strings'>Strings</a>

  - Use string interpolation instead of concatenation:

  ```coffescript
    name = 'Maria'

    console.log 'Ase mas re ' + name + '!' # bad

    console.log "Ase mas re #{name}!" # good
  ```

  - Use single quotes for strings when not interpolating:

  ```coffescript
    name = "Bob Parr" # bad

    name = 'Bob Parr' # good
  ```

  - Use block strings when it improves readability:

  ```coffescript
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

  [Read more](http://coffeescript.org/#strings) about strings.

**[[⬆]](#TOC)**

### <a name='js-objects'>Objects</a>

  - Prefer **dot notation** when accessing properties:

    ```coffescript
    luke =
      jedi: true
      age: 28

    isJedi = luke['jedi'] # bad

    isJedi = luke.jedi # good
    ```

  - Use subscript notation `[]` when accessing properties dynamically (with a variable) or when that property name is a language keyword:

    ```coffescript
    options =
      'default':
        bacon: 'nomnom'
    my_key = 'default'

    default_food = options['default']
    default_food = options[my_key] # same
    ```

  - When defining an object, either add commas in the whole block, or don't. Please do not mix both styles:

  ```coffeescript
  # bad bad bad
  luke =
    jedi:  true,  # comma
    age:   28     # no comma
    human: true
  ```

**[[⬆]](#TOC)**

### <a name='js-functions'>Functions</a>

  - Do not use parentheses when declaring functions that take no arguments.

  ```coffeescript
  # bad
  foo = ()->
    console.log('aha!')

  # good
  foo = ->
    console.log('aha!')
  ```

  - Parentheses in function/method calls should be omitted or included with respect to readability and clarity. Some good examples:

  ```coffeescript
  foo 4

  foo(4).bar(5)

  foo.getSize(5, 6) / foo.getSize(6, 5)

  ```

  - Avoid the "function grouping" style:

  ```coffeescript
  (foo 4).bar 8 # bad

  foo(4).bar 8 # good
  ```

  - Avoid creating functions inside loops since it's bad for performance:

  ```coffeescript
  # bad
  for i in [1..100]
    doSomething = ->
      # code that does something here..

    doSomething()

  # good
  doSomething = ->
    # code that does something here..

  for i in [1..100]
    doSomething()
  ```

  - Avoid explicit "return" statements unless it's for an "early return". For example:

  ```coffeescript
  browser = getBrowser()
  return 'oh no!' if 'ie6'

  # ...cool stuff ie6 doesn't support...
  ```
  - Class methods should return `this` to enable method chaining. For example:

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

**[[⬆]](#TOC)**

### <a name='js-variables'>Variables</a>

- Initialize variables at the top of their scope, preferably in a grouped block.

**[[⬆]](#TOC)**


### <a name='js-whitespace'>Whitespace</a>
- Place an empty newline at the end of the file.

- Place 1 space before arrows

  ```coffeescript
    test =-> print 'test'  # bad

    test = -> print 'test'  #good
  ```


- Insert one extra space before and after operators

  ```coffeescript
  foo='bar'  # bad

  foo = 'bar'  # good

  c = a+b  # bad

  c = a + b  #good
  ```

- Insert one extra space after `,` and `:`

  ```coffeescript
  foo: [1,2,3] # bad

  foo: [1, 2, 3] # good

  foo:'bar'  # bad

  foo: 'bar'  # good

  ```


- Avoid spaces around arguments and before a `,`

  ```coffeescript
  foo( options, 'bar' )  # bad
  foo(options , 'bar')  # bad

  foo(options, 'bar')  # good
  ```

- Assignments should be vertically aligned

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
  ```

- Use indentation when making long method chains.

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

  # also good
  $('#items')
    .find('.selected')
      .highlight()
      .end()
    .find('.open')
      .updateCount()
  ```

  **[[⬆]](#TOC)**


### <a name='js-comments'>Comments</a>
- When modifying code, update also the corresponding comment. (Ideally, improve the code to obviate the need for the comment, and delete the comment entirely.)

- Avoid using CAPS when writing comments unless writing annotations such as `TODO`, `FIXME`, etc.

### <a name='js-inline'>Inline comments</a>
- Do not write comments to state the obvious.

- Inline comments should be placed exactly above the code describing, unless they are suffeciently short and can be placed on the same line.

- Inline comments should start with a capitalized word (unless first word is variable, method, etc name)

### <a name='js-block'>Block comments</a>
 - Block comments should also be placed above above the code describing.

 - Block comments should be written using in each line a `#` following a `space`. Example:

  ```coffeescript
  # This is a block comment. This comment is describing the code below.
  # One line was not enough to state the case so i decided that two lines are ok.
  ```

- Block comments' style using triple `#` is also accepted and
  prefered when writing documentation. Example:

  ```coffeescript
  ###
  This is a block comment. This comment is documenting the module in this file.
  This style helps the maintainance of documentation.
  I also want this comment to be passed to the generated javascript.
  ###
  ```

**[[⬆]](#TOC)**

### <a name='js-type-coercion'>Type Casting & Coercion</a>

- Use `parseInt` for integers and always with a radix for type casting.
  If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

  ```coffeescript
  inputValue = '4'


  # bad
  val = parseInt inputValue

  # good
  val = parseInt(inputValue, 10)

  # good
  val = ~~inputValue
  ```

- Booleans:

  ```coffeescript
  age = 0

  # bad
  hasAge = Boolean age

  # good
  hasAge = !!age

  # also good
  # this is coffee's ternary operator equivalent
  hasAge = if age then true else false
  ```

  **[[⬆]](#TOC)**

### <a name='js-jquery'>DOM Manipulation</a>
- You should be able to tell a presentational class from a functional class

```coffeescript
  $my_carousel = $('.carousel')  # bad

  $my_carousel = $('.js-carousel')  # good

```

- Do not base your code on assumptions about DOM hierarchy/state.


- Cache DOM lookups.

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

- Care for performance when writing your DOM queries.
  Try make the selector engine use [querySelectorAll](https://developer.mozilla.org/en-US/docs/Web/API/Document.querySelectorAll)

  also good read: [w3c selectors-api](http://www.w3.org/TR/selectors-api/)

  ```coffeescript
  # slow
  # internally uses $.sibling to find nodes following
  # other nodes in the same tree.
  $('.sidebar').children('ul').hide()

  # faster
  $('.sidebar').find('ul').hide()
  ```

  **[[⬆]](#TOC)**

### <a name='js-jquery'>Common Pitfalls</a>
- Aliases

Have a look at the [full list of coffeescript aliases](http://coffeescript.org/#operators)

Some of the above are notorious for causing comfusion to coffeeScript newcomers.
We advise the following:

 - Avoid `==` and `!=` as they compile to `===` and `!==` that may be confusing/unwanted.

 - Prefer `@` to `this`

 - Use fat arrows `=>` in a function definition when you need to bind to the function the current context `this`.

 - Do not confuse `?` syntax with JS ternary assignments. Ternary statement in coffee is:

 ```coffeescript
  my_state = if 6 then "go!" else "work!"
 ```

 - Use coffeescript existantial operator `?` for existance checking. It returns `true` unless a variable is `null` or `undefined`.
 Example use cases:

 ```coffeescript
 state = job_title ? "Unemployed"
 ```


 ```coffeescript
  zip = location?.address?.zipcode  # Avoid throwing typeError in chaining
 ```
 [Read more about this](http://coffeescript.org/#operators)


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
- [coffeeLint](http://www.coffeelint.org/)

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

- Regular Expressions
They are a programmers best friend. Use them at will.
Debug them with [debuggex](http://www.debuggex.com/)

- LiveReload
Apply changes in js, styles without refreshing
[extension](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei)
[gem](https://github.com/guard/guard-livereload)


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
* [airbnb](https://github.com/airbnb/javascript)
* [idiomatic](https://github.com/rwldrn/idiomatic.js/)
* [polarmobile CoffeeScript Style Guide](https://github.com/polarmobile/coffeescript-style-guide)

- [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
- [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52)

### <a name='books'>Books</a>
  - JavaScript
    - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
    - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
    - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
    - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
    - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
    - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
    - [The Little Book on CoffeeScript](http://arcturo.github.io/library/coffeescript/) - Alex MacCaw
    - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
    - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - CoffeeScript
    - [The little book of CoffeeScript](http://arcturo.github.io/library/coffeescript/)
    - [CoffeeScript cookbook](http://coffeescriptcookbook.com/)

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
