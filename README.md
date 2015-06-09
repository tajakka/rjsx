# 0. Introduction

RJSX is a web application framework built with productivity, maintainibility, code clarity
and ease of use in mind. It works best with [robo.ceo](http://robo.ceo) functions running in
the backend but it can be easily used in a standalone fashion too. It is essentially an 
abstraction layer on top of [Facebook's React](https://facebook.github.io/react/) 
and a few other technologies topped up with the following goodies:

* A class system (that unlike raw React works with multiple inheritance and mixins, and does
not require any ES6 magic)
* Automatic import for React components
* ~~Smart~~ module system (smart part being still work in progress)
* Enough syntactic sugar to cause you Type 2 syntactic diabetes
* Asynchronous modules with all the AMD nonsense abstracted away
* Tight ~~but smart~~ Semantic UI integration
* Other built in UI components that SemanticUI is missing
* Layout engine
* No boilerware - all you need for a project is a MyInternet server instance running
and the code files that actually define your project
* Automatic building ~~and minifying~ 
* Core components for web app development, ranging from web socket managers to gesture 
recognition and [Flux](https://facebook.github.io/react/docs/flux-overview.html) like Stores 
to a super easy but full featured login system
* No need to write a single line of raw JS/HTML/CSS
* Easy configuration with myinternet.json
* Compability with vanilla JS. Modules compile (automatically)into vanilla JS that can be used 
in non-rjsx
projects
* Built in localization system
* Error handling
* Logging facade + default log viewer
* Testing framework + test runner
* Probably other noteworthy features as well that wont just pop in to my head right now

# 1. Motivation

RJSX got started back in late December 2014 after seeing the Light of React and deciding to
abandon all the other bullshit frameworks in favor of it. Before that the experience of doing
frontend app development was really close to eating AIDS infested bloody diarrhea. React kind of
took the blood and wetness away, but there was still AIDS poo left in the form of
javascript, a language built for emptying forms in 1990's web browsers.

This, and getting a little bit too ~~high~~ carried away while thinking how to integrate
React efficiently led us into making not only our own abstraction layer, but our own JSX based 
language too.

Very soon after the first version was built, Facebook released a version supporting ES6 classes,
making it more like actual programming languages. However, that came with the cost of losing 
mixin support and our fresh but not-yet-so-shiny framework still had a upper hand in a few other
ways, so development was continued. And as we had our own framework + language combo, we had to
make it cool.

Okay, now enough for the humanist essays, let's get into the real stuff.

# 2. Hello World! example

<Note>This may seem a little verbose, but it's written so that even a mentally handicapped 
person could get started. If you're not a retard, skip to 2.4</Note>

## 2.0 Prequisites
To run the apps you need 2 things installed

1. [Java 8 Runtime Environment
](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)
2. [MyInternet](http://example.org)

<Note>This shouldn't be black magic, but we're planning to make things even easier and make a
bundled installer and a launcher</Note>

## 2.1 Running the server

1. open command line/terminal
2. navigate to myinternet folder
3. enter the following command

Linux/OSX users:

    sudo java Launcher

Windows users:

    java Launcher

This launches a new server instance that listens port 80 for HTTP requests and port 443 for 
HTTPS.


<Note> A GUI version of this is on the way if this seems too difficult (and if it does, you
probably shouldn't be even trying to code anyways)</Note>

## 2.2 Configuring the web app

MyInternet needs to know that a certain folder under it's root should be served as a web app,
so we need to make a configuration file.

1. open folder `path-to-myinternet/myinternet/root`
2. create folder hello
3. navigate to hello folder
4. create new file called `myinternet.json`
5. open the file and enter the following
```json
{
	title:"Hello, World!"
}
```
The title parameter is optional and you could have omitted it leaving only `{}`


## 2.3 Creating the main app module

1. open folder `path-to-myinternet/myinternet/root/hello`
2. create folder js
3. create file *app.rjsx*
4. open it and enter the stuff in the following section

## 2.4 Actual Hello, World! file

```jsx
rclass HelloWorld{
	render(){
		return <div>Hello, World!</div>;
	}
}
```

## 2.5 Analysis

* *rclass* keyword followed by element name defines a React element class. This is somewhat
analoguous to React ES6 versions of class HelloWorld extends React.Component (although the
implementations differ). By using *class* instead you would have created a normal class instead
* *render* is the only method that react classes need to define. It should return exactly one
React component.
* the weird looking return statement is a div element expressed in JSX syntax. You can think of
it as being able to inject HTML code inside JS (but there's some more magic to it). Note that
unlike in vanilla JSX, you don't need to wrap React elements with parenthesis. 

## 2.6 Running the example
1. open your favourite web browser (preferably from this millennium)
2. navigate to http://localhost/hello
3. You should see a loading icon (should happen only on the first time) and shortly the text
`Hello, World!`


# 3. Simple Todo List example

As a Hello World example does not really do our framework justice, let's take a little more
complex example.

## 3.0 Specification

Just a simple non-peristent task list where user can enter tasks and mark them as done.

## 3.1 Design

We basically want an input thingy at the top, and then some task list below it.

### 3.1.0 Drawing a shitty wireframe

image:img/shittywireframe.jpg

### 3.1.1 Reducing the wireframe to components

image:img/shittywireframe_components.jpg

We seem to have the following component hierarchy:

* main app (App)
  * input thingy (TaskInput)
    * title field (Input)
    * buttons (Buttons)
      * submit button (Button)
      * clear button  (Button)
  * task list (TaskList)
    * bunch of tasks (Task)
      *status indicator (Icon)
      *title (span)


## 3.2 Implementation

### 3.2.0 Boilerplate

As with the hello world example we need to create folder for the project and make a
myinternet.json configuration file in it.

For easier maintainibility, let's also make our project folder as a sibling to myinternet
folder, instead of placing it under myinternet/root. Then we need to create a *.link* file so
that MyInternet knows where to get our app resource. So under myinternet/root/ we create a file
called tasklist.link with the following contents (assuming that the project folder is a
sibling to myinternet and is named tasklist):

    ../tasklist

<Ref href="http://example.org">see MyInternet documentation for details</Ref>

Or, if you want to get rid of this step either get a ~~graphical util~~ (not yet implemented)
or [a script](http://example.org), which would reduce project creation to this:

    createproject tasklist

### 3.2.1 app.rjsx

```jsx
css tasklist;
rclass App{
	render(){
		return 
			<div @class>
				<TaskInput onNewTask=this.handleNewTask/>
				<TaskList tasks=P.tasks/>
			</>;
	}
	
	handleNewTask(task){
		P.pushTask(task);
	}

}
```


#### 3.2.1.0 Analysis   

* *css* keyword includes css files, css/tasklist.css in this case. As stated, you don't **need**
to use any raw CSS, but it might be still a good idea to separate the style stuff so that some
UI clown can make things pretty without molesting the code.
* Note that since we're returning two items, we need to wrap it with something since render
needs to return exactly one core item.
* *@class* macro gives the element a css class name from the surrounding elements getCSSClass()
method. If this method has not been defined, the css class will be the same as element name in
lowercase. So in this example, the wrapping div would get app as a class.
* TaskInput and TaskList tags are autoclosing i.e. dont need a body or a closing tag
* TaskInput gets a onNewTask listener handleNewTask as a property. Note that unlike vanilla JSX
you don't need to wrap the attribute value with {} if the statement is simple enough not to
cause ambivalence
* P.tasks is basically a shorthand for this.props.tasks. If you're unfamiliar with the state and
props concepts, check out React documentation. TL;DR: props contains all the data to describe
the element and state has all the info on what state it is in currently. Or simply put: shove
all your data in props, and use state for storing temporary info, such as if a menu is opened
* In handleNewTask the received task is pushed to P.tasks array. See section ??? for details on
how P.push*() works
* Here it doesn't make much sense, but especially with longer class names it's often convenient
to close the tags with ```</>``` instead of writing out the full element name.
* Note that TaskInput and TaskList are not explicitly imported anywhere. This is because rjsx 
will automatically try to import them from js/taskinput and js/tasklist, unless they are
explicitly imported using the *import* keyword at the beginning.


### 3.2.2 taskinput.rjsx
```jsx
rclass TaskInput{
	initialState={
		title:""	
	};
	render(){
		return <div @class>
				<Input placeholder="@string(title)" @linkTitle/>
				<Buttons>
					<Button icon="send" red @handleSendClick>
						@string(send)
					</>
					<Button icon="erase" blue @handleClearClick>
						@string(clear)
					</>
				</Buttons>
			</div>
	}

	handleSendClick(){
		var newTask={
			title:S.title,
			completed:false		
		};
		P.onNewTask?(newTask);
	}
	handleClearClick(){
		S.reset();
	}

	 		
}
```
#### 3.2.2.0 Analysis

* *Input* is a fancy [SemanticUI](http://semanticui.com) element. The same goes for *Buttons*
and *Button*
* You **don't need** to wrap strings with *@string()*, but if you do, MyInternet checks
automatically if a translation exists under `strings/[lang_name].json` and if not, falls back
to the argument. See 4.5 for details. 
* *@linkTitle* binds the value of input field to S.title. I.e. if you change S.title, Input will
be updated immediately and vice versa. See the section on link macros for more details
* red and blue are passed to the *Button*s as css classes. You can do this with all elements.
* *@handleSendClick* and *@handleClearClick* macros bind onClick handlers to handleSendClick and
handleClearClick respectively. This equivalent, but slightly prettier than vanilla React version
of onClick={this.handleSendClick}
* In handleSendClick we're passing S.title to P.onNewTask, but only when the onNewTask property
has been set. So that's what the weird question mark thing does. See the section on Elvis
operators for more details.
* S.reset() resets the state to the one specified in initialState field. Since there's nothing 
interesting in initialState we could have just omitted it.


### 3.2.3 tasklist.rjsx

```jsx
rclass TaskList{
	render(){
		return <div @class>
			{renderTasks()}
			</div>
	}
	renderTasks(){
		return P.tasks?.map(t->renderTask(t));
	}
	
	renderTask(task){
		return <Task ...task/>;
	}
}
```

#### 3.2.3.0 Analysis
* In render we get the div contents dynamically from *renderTasks()* method. Such statements 
need to be wrapped inside {} to separate them from plaintext.
* Thanks to some elaborate hacks, we don't need to write this.renderTasks() here or pretty much 
anywhere else where this can be omitted logically without creating ambivalence.
* *renderTasks* has a lot of crazy looking shit going on
  * The *?* does the same job as above, and stops evaluating the expression if P.tasks is
    undefined instead of throwing an exception
  * *map* is just run-of-the-mill javascript array mapping function. Basically it turns the 
     array into another one by replacing each element using the provided function.
  * t->renderTask(t) is a *lambda expression*, basically a handier syntax for functions with
    some added benefits. See the section lambda expressions for more details
  * As intimidating as this may look to an untrained eye, it really makes this frequently
    repeating pattern so much more pleasant to write (not to mention less bug prone). 
    In vanilla React this would be:

```js
if(!this.props.tasks)
	return undefined;
var self=this;
return this.props.tasks.map(
	function(task){
		return self.renderTask(task);
	});
//148 chars vs 39

```

* *renderTask* creates a *Task* element and passes all the task attributes as properties to Task
  with that weird looking *...task* syntax. So for example if `task ={title:"foo"}` then task 
  would get `P.title === "foo"`
 
### 3.2.4 task.rjsx

```jsx
rclass Task{
	render(){
		return <div @class>
				{getStatusIcon()} 
				<span tasktitle>
					{P.title}
				</span>
			</div>;

	}		
		
	style=css{
		backgound-color:rgba(0,0,0,0);
		transition:0.2s all ease-in;
		cursor:pointer;				
	};
	hoverStyle=css{
		backgound-color:rgba(255,0,0,0.3);
		transition:0.5s all ease-in;		
	};	
	
	getStatusIcon(){
		if(P.completed)
			return <Icon green check circle @handleIconClick @hoverStyle/>;
		else
			return <Icon blue circle @handleIconClick @hoverStyle/>;
	}
	handleIconClick(){
		P.toggleCompleted();			
	}
}
```

#### 3.2.4.0 Analysis

* When *@hoverStyle* macro is used a lot of magic happens in the background and the element
  gets hoverStyle argument passed as inline css. By using just @style, you'd get the style field
  inlined. In effect, the icon gets a nice little red tint with animated onset when user hovers
  mouse over it.
* Inside *css{}* blocks you can enter raw CSS, which is slightly more convenient than doing the
  same with plain javascript object, as vanilla React does. Obviously, you can still use js
  objects for this if you want.
* *P.toggleCompleted* toggles the value of P.completed and is equivalent to 
   `this.setProps({completed:!this.props.completed});`  


<Note> 
In this implementation changes to task states do not get passed back to App (that stores
all the tasks). This is for two reasons.

1. You should really use Stores for handling data flow like this but this example is supposed
   to use just the language level features, and none of the sweet bundled components that we
   have. See section Stores for more info.
2. I'm lazy and passing the change back to App without using Stores is a pain in the ass.
</Note>




## 3.3 Result

Here's how our app looks like:

<TodoAppExample/>

## 3.4 Further development

If we wanted, we could make this into a real app with ease. For example adding login
functionality is easy as letting App extend RoboUI.RoboApp, letting the super class handle 
render and changing our render method to renderMain():

```jsx
import RoboUI;
css tasklist;
rclass App extends RoboUI.RoboApp{
	renderMain(){
		return 
			<div @class>
				<TaskInput onNewTask=this.handleNewTask/>
				<TaskList tasks=P.tasks/>
			</>;
	}
	
	handleNewTask(task){
		P.pushTask(task);
	}

}
```

Bang. We have full flexed login & register systems in place. However, our login things depend on
the proprietary parts of our backend, so you're going to have to write your own base app 
implementation (which is pretty easy) if you're reading this as a random outsider and not 
someone working directly with us.

There are also tons of other features which make seemingly complicated, but generic and
essential things a breeze. They are still way out of scope for this example, so see section 6 
for details.





# 4. Core specs

This section is extremely boring, sorry.

## 4.0 File structure

A minimal file structure for a project would be

* project_root/
  * js/
    * app.rjsx
  * myinternet.json

All rjsx files should be placed under *js/foo.rjsx* or 
*js/[module_name]-components.js/Foo.rjsx*.

Filenames should be **lowercase**, except for when they are bundled inside a module,
in which case they are case insensitive. 

Each rjsx file should represent exactly one class or React component. To bundle several files
into one module, see using modules.

## 4.1 Configuration file

The configuration file is used to set some basic features of the project, and to signal
*MyInternet* that the resource should be served as an app.

example:
    myinternet.json

```json
{
	title:"my fancy app",
	debug:"auto",
	logging:"true",
	errorHandling:"true",
	favicon:"img/myfancyapp.png",
	logger:"console"  
}

```


### 4.1.0 Syntax

*myinternet.json* accepts a superset of JSON, where attribute keys do not need to (but can) be 
wrapped with quotes. There are also plans to introduce verbatim strings, i.e. wrapping the value
with `@"` and `"@"`, and not having to escape anything between them, even newlines.


 If for some reason you were not familiar with JSON syntax and an acute
brain stroke is preventing you from parsing the above example in your head, you 

1. wrap values with "
2. separate keys from values with :
3. separate key/value pairs with ,
4. wrap the whole thing with {}
4. do whatever you want with whitespace

### 4.1.1 Parameters

There are no required parameters. An empty JSON object `{}` is a perfectly fine configuration
file. There are also no "forbidden" parameters, but at the moment only the following ones have
implemented or explicitly planned function.

#### 4.1.1.0 title

Title for your app. Shows up in browser title bar and might end up having some other function 
later on.

default: app

#### 4.1.1.1 favicon

Path to the fav icon for your app.

default: nothing


#### 4.1.1.2 debug

Debug mode. If true, code will not be minimized and logging & error handling will be enabled. 
Some libraries, such as React, Underscore, jQuery and RequireJS will also be loaded from a CDN 
instead of locally.

values: true/false/~~auto~~ (not implemented yet)

If auto, debug will be true when host is either localhost,127.0.0.1 or 0.0.0.0 and false
otherwise

default: ~~auto~~ true

#### 4.1.1.3 logging


Enable logging. If true, log events (see logging) will go to whatever logging is specified,
otherwise they are cleaned up from the code entirely. 

values: true/false/auto

If auto, logging will have the same value as debug

default: auto


<Note>not implemented yet, behaves like auto now</Note>

#### 4.1.1.4 errorHandling

Enable error handling. If true, try-catch loops are generated automatically around every method
and a fancy error window with debug tools is opened.

values: true/false/auto

If auto, errorHandling will have the same value as debug

default: auto

<Note>not implemented yet, behaves like auto now</Note>

#### 4.1.1.5 logger

Logger object that the log events generated by L logging facade are delegated to. 

default: FancyLogger

<Note>You can open FancyLogger by focusing body and pressing ctrl+shift+l</Note>

## 4.2 Classes

Rjsx files can represent either normal classes or React components. One file should contain
exactly one class/component.

### 4.2.0 Woah dude

What if like, there were no classes man. All the objects would realize they're just the same, 
and then they'd be dancing and singing around the campfire, and then there would be no wars and
no complex software.

### 4.2.1 Class definition

Classes are defined using either the rclass or class keyword for React components and normal
classes respectively, followed by whitespace, followed by the class name, possible extend
operator and finally the class body.

Minimal normal class:

```jsx
class Foo{
}
```

Minimal React component:
```jsx
rclass Foo{
	render(){
		return <span/>;	
	}
}
```

### 4.2.2 Inheritance

Classes can inherit other classes (or React components) using the extend keyword.

For example if class Foo needs to extend Bar and Baz we write

```jsx
rclass Foo extends Bar,Baz{
}
```

<Note>
When inheriting multiple classes which have the same implementation for some field/method,
the latter implementation is used. So for example if Bar and Baz both implemented foobar(),
Foo would inherit foobar() from Baz
</Note>

### 4.2.3 import keyword

Other components can imported using the import keyword before the class declaration. You can
either split the imports to several lines like this:

```jsx
import Foo;
import Bar;
import Baz;
```

Or do several imports for one line like this:
```jsx
import Foo,Bar,Baz;
```

Note that this keyword is not needed too often, because rjsx is able to import React components
automatically. So for example if you have a component &lt;Foo/&gt; somewhere, Foo is
added to imports.



#### 4.2.3.0 Anonymous imports

If you need to load some script without using it as a module, you can preface the statement
with ?.

For example if you need to load some jQuery plugin:
```jsx
import ?somejqueryplugin
```
#### 4.2.3.1 Custom paths

The statement `import Foo` would try to import Foo from js/foo.js (see resource resolving). 
If you need to fetch it from some other path, you can use the following syntax:

    import module_name?path_to_module

example
	
    import MyStolenModule?http://someotherdudessite.com/js/awesomemodule.js

<Note>
Not sure if this really works, haven't bothered to test and there hasn't been any need
for it so far
</Note>

### 4.2.4 css keyword

Using the css keyword before a class loads the corresponding css file(s) from css/.

For example 

    css foo,bar,baz;

Would load the css files

    css/foo.css
    css/bar.css
    css/baz.css

If the name contains a dot, it is interpreteted as a path.

    css foo/bar.css

would load

    foo/bar.css

### 4.2.5 Comments

Lines can be commented out separately with `//` or as blocks using `/*` to open a block and
`*/` to close it.

example:
```jsx
someStatement(); //this is a comment
/*
this here is also a comment
but it can span
over several
lines
*/

```

Unlike in vanilla JSX, the comment syntax works inside JSX tags too without having to wrap 
the comments with `{}`


### 4.2.6 Class body

Class body consist of field and method definitions separated by whitespace and wrapped with 
`{}`.

Example:
```jsx
class Foo{//class body starts here
	
	someMethod(someArg,anotherArg){
		//method body	
	}
	
	someStringField="foo";
	someObjectField={
		foo:123			
	};
}//and ends here

```
### 4.2.7 Methods

Methods are defined with the following syntax:

* annotations (optional)
* static modifier (optional)
* return type (optional)
* methodName
* `(`
  * arguments, separated by `,`
* `)`
* `{`
  * method body. This is pretty much just vanilla JSX with added optional syntax sugar 
   (i.e. all valid JSX code is valid inside methods too)
* `}`


For example

```jsx 
@Deprecated
static string foo(){
	return "foobar";
}

```

is a static method named foo, with string as return type that has been annotated as Depracated

### 4.2.8 Fields

Methods are defined with the following syntax:

* annotations (optional)
* static modifier (optional)
* type (optional)
* fieldName
* `=` or `:`, both work but first one is preferred.
* one of the following
  * simple assignment such as "foo" or 420 followed by `;`
  * a javascript object i.e. `{}` and stuff inside it.
  * a `css{}` block, see syntax sugar

For example

```jsx 
foo = {
	bar:123,
	baz:420
}

```

is a field named foo with object {bar:123,baz:420} as it's value.

### 4.2.9 Modifiers

#### 4.2.9.0 static

`static` makes the methods/variables static, i.e. you can invoke them without having an instance
of a class.

For example if we have
```jsx
class Foo{
	static getInstance(){
		return new Foo();	
	}

}
```

we can call `Foo.getInstance()`.

#### 4.2.9.1 Types

At the moment return types are mostly for decoration, but in the future we might just implement
stuff like Flow type checking

#### 4.2.9.2 Annotations

Annotations begin with `@` and don't have much function implemented yet. So far the only
functional one is `@Test` which marks a test case. (see testrunner)


## 4.3 Modules


You can bundle both .rjsx/.jsx/.js and .mcss/.css files in to modules by placing them in 
[module filename]-components/ folder. The bundling process is completely automatic and you 
don't have to worry about it. For example if the file structure is:


    foo.js-components/foo.rjsx
    foo.js-components/bar.rjsx 
    foo.js-components/baz.rjsx

If you then request foo.js you will get a package that contains Foo.Foo, Foo.Bar and Foo.Baz

You can also load the modules partially so that you only get the submodules you need and 
don't already have. The spec for importing partial modules lives but the syntax will probably 
be something like

    import Foo[foo bar];


<Note>Partial modules are mostly implemented, but not yet quite ready for production</Note>

<Note>Smart partial module loading is planned, but not yet implemented</Note>


## 4.4 frontendcommons

All projects share the contents of this folder, and if a resource is missing from from the
projects folder, it is resolved to frontendcommons if it exists there.

For example if we try to load module Layout, but we don't have such a module, it resolves to

    frontendcommons/js/layout.js

Which again resolves to the module defined in

    frontendcommons/js/layout.js-components/

## 4.5 Localizations

RJSX provides an easy way to to translate strings. Just wrap any piece of text with `@string()`
and then provide the translations in

    project_name/strings/[lang_id].json

where lang_id is en for english etc. That is all. MyInternet checks users `accept-language`
header, resolves lang_id from there, and checks if a translation exists. If it doesn't, string
will fall back to english translation, or if that doesn't exist, to the original key.

For example, if users language is finnish and we have the following render method.

```jsx
return <div>@string(Hello, World!)</div>;
```

and the following strings/fi.json

```json
{
	"Hello, World!":"Eihän ne amerikan neekerit tätä ymmärrä niin ihan sama mitä laitan!"
}
```

it will compile to:

```jsx
return <div>Eihän ne amerikan neekerit tätä ymmärrä niin ihan sama mitä laitan!</div>;
```

If however the translation was missing it would fall back to


```jsx
return <div>Hello, World!</div>;
```

As there is really no harm in using `@string()` when translations are missing, it is probably
good practice to surround all strings with it, especially for reusable components.

Also note that translations fall back to `frontendcommons/strings/` as well, so you dont have to
make or copy/paste basic translations all over again for different project.



<Note>You can use @string() anywhere in rjsx code, not just inside JSX tags</Note>


## 4.6 Core libraries
The following libraries are loaded and available for use without having to import anything

handle|description
------|-----------
$     |jQuery
_     |underscore
React |React core
RoboReact|our layer on top of react
Util  |some random utility methods
require|requirejs




# 5 Syntax sugar

RJSX has a bunch of different syntactic improvements. There is/was really no other guideline,
than that the improvements should have some actual value, shouldn't be too esoteric and most of
all *should not make whitespace functional* as that is a heresy and work of the Antichrist. So,
curly braces stay in.

## 5.0 Scope hack

Face it, JavaScript scopes suck ass and they do it big time. Not only is this annoying to write:

```js
var self = this;
function doSomething(){
	self.doSomethingElse();
};
```

but it's quite bug prone, as you often end up getting absend minded and doing the logical thing

```js
function doSomething(){
	this.doSomethingElse();
};
```

Also, when doing object orientated stuff (as we are), you end up up prefacing pretty much every
variable and method call with `this.`, while most of the time there would be no ambivalence if 
you omitted the `this.` part.

RJSX fixes all of this by resolving each variable and then adding `.this` automatically where 
needed. So you'd use the this statement pretty much the same way you would in real programming
languages like Java.

## 5.1 Lambda expressions

Creating callbacks the traditional way is ugly and bug prone, and you end up with scope issues. 
RJSX fixes this by using Java like lambda expressions.They behave roughly the same way as fat 
arrow statements `=>` in some other compile-to-javascript languages, i.e. scope is bound
automatically.

So for example this

```jsx
foo.map(item->doSomethingToThatItem(item));
```

compiles to this
```js
foo.map(
	function(item){
		return this.doSomethingToThatItem(item);
	}.bind(this)
	);
```


You can also take in either 0 or more than 1 argument by wrapping the arguments with `()`.

For example
```jsx

()->alert("no args");

(foo,bar)->alert(foo+" "+bar);

```

compiles to this:
```js

function(){
	return alert("no args");
}.bind(this);

function(foo,bar){
	return alert(foo+" "+bar);
}.bind(this);

```

You can also make multiline arguments with lambdas. Note that the syntax is *Java* style and 
**not** *CoffeeScript* style. I.e. last line is **not** automatically returned and you need to
end it with a return statement if you want to return something.

E.g.

```jsx
()->{
	alert("foo");
	return "foo";
}

```

compiles to this:
```js
function(){
	alert("foo");
	return "foo";
}.bind(this);
```

## 5.2 Java style function references

You can already make function references in js, but in some cases the following java-like
function reference makes sense.

```jsx
socketManager.setListener(this::socketListener);
```

compiling to

```js
socketManager.setListener(function(args){
	this.socketListener(args);
}.bind(this));
```

The difference to
```jsx
socketManager.setListener(this.socketListener);
```

being that if `this.socketListener` changes, the latter one wont get updated.

## 5.3 Shortcuts for this.state and this.props

### 5.3.0 P and S

In general P and S are shortcuts for this.props and this.state, but there is also some handy
added functionality. All of these work both on P and S unless otherwise stated.

### 5.3.1 setX

Set the value of foo property to bar:

```jsx
P.setFoo(bar)
```

is compiled to:

```js
this.setProps({foo:bar})
```

### 5.3.2 getX

Get item bar from object/array foos

```jsx
P.getFoo(bar)
```

is compiled to:

```js
this.props.foos[bar]
```

<Note>X is expected to be a singular name, and the containing array a plural. If X ends with
s, then the array should be Xes. Otherwise, Xs</Note>

### 5.3.3 pushX

Push item bar from to array foos

```jsx
P.pushFoo(bar)
```

is compiled to:

```js
(
function(){
	var foos = this.props.foos||[];
	foos.push(bar);
	this.setProps(
		{foos:foos}
	);
	return foos;
}.bind(this))()

```

<Note>X is expected to be a singular name, and the containing array a plural. If X ends with
s, then the array should be Xes. Otherwise, Xs</Note>

### 5.3.4 toggleX

Toggles the value of foo property

```jsx
P.toggleFoo()
```

is compiled to:

```js
this.setProps({foo:!this.props.foo})
```

### 5.3.5 removeX

Remove item bar from object/array foos

```jsx
P.removeFoo(bar)
```

is compiled to:

```js
(
function(){
	var foos = this.props.foos||[];
	foos.remove(bar);
	this.setProps(
		{foos:foos}
	);
	return foos;
}.bind(this))()

```

<Note>X is expected to be a singular name, and the containing array a plural. If X ends with
s, then the array should be Xes. Otherwise, Xs</Note>


### 5.3.6 putX

Put value baz with key bar to array/object foo

```jsx
P.putFoo(bar,baz)
```

is compiled to:

```js
(
function(){
	var foos = this.props.foos||[];
	foos[bar]=baz;
	this.setProps(
		{foos:foos}
	);
	return foos;
}.bind(this))()

```

### 5.3.7 simple arithmetics

Some of these might not have much value in them, but thenagain implementation was mostly just
copy/pasting so why not have them all.

#### 5.3.7.0 sumX

Add 420 to foo

```jsx
P.sumFoo(420)
```

is compiled to:

```js
this.setProps({foo:this.props.foo+420})
```

#### 5.3.7.1 substractX

Substract 420 to foo

```jsx
P.substractFoo(420)
```

is compiled to:

```js
this.setProps({foo:this.props.foo-420})
```

#### 5.3.7.2 increaseX

Increase value of foo by 1

```jsx
P.increaseFoo();
```

is compiled to:

```js
this.setProps({foo:this.props.foo+1})
```


#### 5.3.7.3 decreaseX

Decrease value of foo by 1

```jsx
P.decreaseFoo();
```

is compiled to:

```js
this.setProps({foo:this.props.foo-1})
```
#### 5.3.7.4 multiplyX

Multiply foo by 420

```jsx
P.multiplyFoo(420)
```

is compiled to:

```js
this.setProps({foo:this.props.foo*420})
```

#### 5.3.7.5 divideX

Divide foo by 420

```jsx
P.divideFoo(420)
```

is compiled to:

```js
this.setProps({foo:this.props.foo/420})
```

### 5.3.7.6 modulusX

Modulus 420 of foo

```jsx
P.modulusFoo(420)
```

is compiled to:

```js
this.setProps({foo:this.props.foo%420})
```

### 5.3.8 S.reset()

reset state to initial value

```jsx
S.reset()
```

is compiled to:

```js
this.setState(this.getInitialState())
```


<Note>This works only for state</Note>

### 5.3.8 S.reset()

reset state to initial value

```jsx
S.reset()
```

is compiled to:

```js
this.setState(this.getInitialState())
```


<Note>This works only for state</Note>

### 5.3.9 forEachX

Iterate through array Foo and log all items
```jsx
P.forEachFoo(i->console.log(i))
```

is compiled to:

```js
this.props.foos.forEach(function(i){console.log(i)});

```

<Note>X is expected to be a singular name, and the containing array a plural. If X ends with
s, then the array should be Xes. Otherwise, Xs</Note>

### 5.3.10 filterX

Iterate through array Foo and remove items that don't match the filter
```jsx
P.filterFoos(foo->foo.isAlive());
```

is compiled to:

```js
(
function(){
	var foos = this.props.foos||[];
	foos=foos.filter(function(foo){return foo.isAlive()}.bind(this));
	this.setProps(
		{foos:foos}
	);
	return foos;
}.bind(this))()

```

### 5.3.11 mapX

Iterate through array Foo and replace the items
```jsx
P.mapFoos(foo->convertToBar(foo));
```

is compiled to:

```js
(
function(){
	var foos = this.props.foos||[];
	foos=foos.map(
		function(foo){
			return this.convertToBar(foo);
		}.bind(this)
	);
	this.setProps(
		{foos:foos}
	);
	return foos;
}.bind(this))()

```

## 5.4 Elvis (or existence/null check) operators

These exist to simplify null checks. You can chain these expressions, and think that the 
execution stops when first null is reached. So for example

```jsx
foo?.bar.baz
```

will result in undefined when foo is null (and not a NullPointerException)


### 5.4.0 Assignment ?= 

```jsx
foo?=bar
```

compiles to
```js
if(foo===null||foo===undefined)
	foo=bar;
```
### 5.4.1 Pointer ?.

```jsx
foo?.bar
```

compiles to
```js
(foo?foo.bar:undefined)
```
### 5.4.2 Method call ?(

```jsx
foo?()
```

compiles to
```js
(foo?foo():undefined)
```

### 5.4.3 Array reference ?[

```jsx
foo?[420]
```

compiles to
```js
(foo?foo[420]:undefined)
```

## 5.5 Tag matching shortcuts

Sometimes having to match long tag names can be a pain in the ass, so you can replace any
closing tag with `</>` and it will match the correct starting tag.

For example this works just fine:

```jsx
<div>
	<AReallyFuckingLongTagName.AndLookTheresEvenASubModule>
		
		<Button>foo</Button>
	</>
</div>
```

## 5.6 CSS blocks

You can inline raw CSS which should be slightly more convenient and less bug prone than doing 
the same with js objects.

```jsx
var style = css{
	position:absolute;
	background-color:red;
	font-weight:bold;	
}
```
compiles to

```js
var style = {
	position:"absolute",
	backgroundColor:"red",
	fontWeight:"bold"	
}
```

## 5.7 Improved foreach syntax

Vanilla JS provides a way to iterate over keys of an array/object, but the more common scenario
is obviously iterating over values of it. RJSX provides a Java-esque solution to this


```jsx
for(item:items)
	alert(item);

```

compiles to

```js
for(var index in items){
	var item=items[index];
	alert(item);
}
```

If you need to specify the index key name you can do it like this:

```jsx
for(item,key:items)
	alert(key+" is "+item);

```

compiles to

```js
for(var key in items){
	var item=items[key];
	alert(key+" is "+item);
}
```

## 5.8 Whatchamacallit for Objects

This feature actually exist in new JSX versions to, but it is still probably worthy of a mention
here. The following syntax is usefull for *consuming* keys from an Object. Such pattern might
arise for example when a React component has use for some of it's props and passes the rest to
it's child.

```jsx
{foo,bar,...others}=baz;
```

```js
var foo=baz.foo;
var bar=baz.bar;
var others= _(baz).omit('foo','bar');
```

## 5.9 Whatchamacallit for Arrays

The same as above, but for arrays. Not very useful, but it was an easy copy+paste and
CoffeeScript had it, so...

```jsx
[foo,bar,...others]=baz;
```

```js
var foo=baz[0];
var bar=baz[1];
var others= baz.slice(2);
```

## 5.10 Logging facade

`L.X(` is a shortcut for `window.logger(this.getElementName()`, which should contain alogger 
object. The following methods are available:

* L.trace(msg[,data]);
* L.debug(msg[,data]);
* L.info(msg[,data]);
* L.warn(msg[,data]);
* L.error(msg[,data]); 

## 5.11 JSX tag syntax improvements

### 5.11.0 Redudant {} can be omitted from attributes

E.g.

```jsx
<Foobar foo=123 bar=this.foo baz={a:123,b:420} />
```

can be used instead of 

```jsx
<Foobar foo={123} bar={this.foo} baz={{a:123,b:420}} />
```

### 5.11.1 Comments can be used inside JSX syntax normally

E.g.

```jsx
<div>
//nah fuck this
/*<div>hurrdurr</div>*/
</div>
```

can be used instead of 

```jsx
<div>
{/*nah fuck this*/}
{/*<div>hurrdurr</div>*/}
</div>

```

### 5.11.2 Parenthesis can be omitted from return statements

E.g.

```jsx
return <Foo/>;

```

can be used instead of 

```jsx
return (<Foo/>);

```

### 5.11.3 Lone strings work as class names

E.g.

```jsx
<div foo bar baz/>

```

compiles to 

```jsx
<div className="foo bar baz"/>

```


## 5.12 Tag macros

Tag macros make some common patterns a little more convenient

For example

```jsx
<Foo @click/>;

```

compiles to

```jsx
<Foo onClick={this.handleClick};

```

### 5.12.0 @data

Pass data property

```jsx
<Foo @data/>
```

compiles to:
```jsx
<Foo data={this.props.data}/>
```
### 5.12.1 @style

Set element style to this.style

```jsx
<Foo @style/>
```

compiles to:
```jsx
<Foo style={this.getStyle()}/>
```

### 5.12.2 @hoverStyle

Set element style to this.style when cursor is outside the component and to this.hoverStyle
when inside;

```jsx
<Foo @hoverStyle/>
```

compiles to:
```jsx
<Foo style={this.getStyle()} onMouseOver={this.onMouseOver} onMouseOut={this.onMouseOut}/>
```

### 5.12.3 @activeStyle

Set element style to this.style when cursor component is not active and to this.activeStyle
when it's not.

```jsx
<Foo @activeStyle/>
```

compiles to:
```jsx
<Foo style={this.getStyle()} onFocus={this.onFocus} onBlur={this.onBlur}/>
```

### 5.12.4 @click

Set onClick handler to this.handleClick

```jsx
<Foo @click/>
```

compiles to:
```jsx
<Foo onClick={this.handleClick}/>
```

### 5.12.5 @mouse

Set mouse event listeners to this.handleX

```jsx
<Foo @mouse/>
```

compiles to:
```jsx
<Foo onMouseOver={this.onMouseOver} onMouseOut={this.onMouseOut} onMouseMove={this.onMouseMove}
	onContextMenu={this.onContextMenu} onDoubleClick={this.onDoubleClick}	
/>
```

### 5.12.6 @drag

Set drag listeners to this.handleX

```jsx
<Foo @drag/>
```

compiles to:
```jsx
<Foo draggable={true} onDrag={this.handleDrag} onDragEnd={this.handleDragEnd} 
	onDragStart={this.handleDragStart}
/>
```

### 5.12.7 @drop

Set drop listeners to this.handleX

```jsx
<Foo @drop/>
```

compiles to:
```jsx
<Foo droppable={true} onDragEnter={this.handleDragEnter} onDragExit={this.handleDragExit} 
	onDragLeave={this.handleDragLeave} onDragOver={this.handleDragOver}
	onDrop={this.handleDrop} />
/>
```

### 5.12.8 @touch

Set touch listeners to this.handleX

```jsx
<Foo @touch/>
```

compiles to:
```jsx
<Foo onTouchCancel={this.handleTouchCancel} 
	onTouchEnd={this.handleTouchEnd} onTouchMove={this.handleTouchMove}
	onTouchStart={this.handleTouchStart} />
/>
```


### 5.12.9 @gesture

Listen for touch events and when a gesture is recognized, this.handleGesture(g) is triggered

example:

```jsx
render(){
	<Foo @gesture/>
}
onGesture(g){
	if(g.name=="PentagramUpsideDown")
		summonSatan();
}
```


At the moment of writing, only the following gestures are recognized:

* Tap
* LongTap
* DoubleTap
* SwipeLeft
* SwipeRight
* SwipeUp
* SwipeDown

### 5.12.10 @focus

Set focus listeners to this.handleX

```jsx
<Foo @focus/>
```

compiles to:
```jsx
<Foo onFocus={this.onFocus} onBlur={this.onBlur}/>
/>
```

### 5.12.11 @scroll

Set scroll listeners to this.handleX

```jsx
<Foo @scroll/>
```

compiles to:
```jsx
<Foo onScroll={this.handleScroll} onWheel={this.handleWheel}/>
/>
```

### 5.12.12 @clipboard

Set clipboard listeners to this.handleX

```jsx
<Foo @clipboard/>
```

compiles to:
```jsx
<Foo onCopy={this.handleCopy} onPaste={this.handlePaste} onCut={this.handleCut}/>
```

### 5.12.13 @keyboard

Set keyboard listeners to this.handleX

```jsx
<Foo @keyboard/>
```

compiles to:
```jsx
<Foo onKeyDown={this.handleKeyDown} onKeyUp={this.handleKeyUp} onPress={this.handleKeyPress}/>
```

### 5.12.14 @change

Set change listener to this.handleChange

```jsx
<Foo @change/>
```

compiles to:
```jsx
<Foo onChange={this.handleChange}/>

```

### 5.12.15 @class

Pass this.getCssClass() to component

```jsx
<Foo @class/>
```

compiles to:
```jsx
<Foo className={this.getCssClass()}/>

```

### 5.12.16 @href

Pass this.props.href to component

```jsx
<Foo @href/>
```

compiles to:
```jsx
<Foo href={this.props.href}/>

```

### 5.12.17 @route

Pass router attributes to component. See ASyncRoute for details

```jsx
<Foo @route/>
```

compiles to:
```jsx
<Foo args={this.getRoute()} routeBase={this.routeBase}/>

```

### 5.12.18 @linkX

Link value to this.state.foo so that component value changes when this.state.foo changes and 
vice versa.

```jsx
<Bar @linkFoo/>
```

compiles to:
```jsx
<Bar valueLink={this.linkState('foo')}/>

```

### 5.12.19 @handleXClick

Binds onClick listener to this.handleXClick

```jsx
<Foo @handleFooClick/>
<Bar @handleBarClick/>
```

compiles to:
```jsx
<Foo onClick={this.handleFooClick}/>
<Bar onClick={this.handleBarClick}/>

```

## 5.13 Custom Keywords
### 5.13.0 join

    join(a,b...);

joins two or more objects

### 5.13.1 $this

jQuery wrapper for the root node

```jsx
$this
```

compiles to:
```jsx
$(this.getDOMNode())

```

### 5.13.2 AJAX calls

Basically just defining a default error handler for AJAX calls + fixing some jQuery hickups + 
making the syntax a little prettier.

The following commands are available:
* get(url,data,callback[,error]);
* getJSON(url,data,callback[,error]);
* put(url,data,callback[,error]);
* post(url,data,callback[,error]);
* patch(url,data,callback[,error]);
* del(url,data,callback[,error]);

If you pass a function to data it will be treated as a callback, so for example

    get("api/foo",data->alert(data));

will work. You can also pass an object as the first parameter if you prefer doing things that
way.


# 6. Core components

Unless stated otherwise, all components are located directly under `frontendcommons/` and they
are **not** automatically imported. So for example to use an ItemStore you need to add the 
following import statement.

```jsx
import ItemStore;
```

## 6.0 Stores

RJSX Stores kind of sort of implement 
[the flux pattern](https://facebook.github.io/react/docs/flux-overview.html)
or at least take care of the same need, passing states between components without callback hell.

<Note> This is still in the alpha stage, so everything is subject to change, and througough
documentation at this phase is pointless</Note>

### 6.0.0 Store.rjsx

Store is the abstract base class for all stores. A store object represents data access for
exactly one item. When data in the Store changes (either by user actions or server events), the
change gets broadcasted to all the items that have registered to the store.


When making new Stores, extend the Store class and implement the following 
methods:

* constructor that takes in parameter *key* and should set *this.key=key*
* getData(), returns an object with the current state of the item for the key provided in the
constructor
* storeData(data) should handle the actual data storage process 

For example if we want to make a store that reads JSON formatted items from *localstorage*:

```jsx
import Store;
class LocalStorageStore extends Store{
	LocalStorageStore(key){
		this.key=key;
	}
	getData(){
		return JSON.parse(localStorage[key]);
	}
	storeData(data){
		localStorage[key]=JSON.stringify(data);
	}


}

```

If you need to alter the default functionality in some way you can also override the following
methods:

* void setData(data)
* void replaceData(data)
* void broadcastChange(data)
* static Store getInstance(key)
* static void setState(key,data)
* static void replaceState(key,data)
* static void register(key,listener,initVal)
* static void unregister(key,listener)
* static object getState(key)

### 6.0.1 Usage

To use the Stores components need to jump through the following hoops

* import the store implementation that's being used
* implement method getStore() that should return the **class** of the store being used
* implement an object field storedStates that specifies how `S` is bound with stored
values. The keys of the object correspond to state keys and values to the store keys. So for 
example `storedStates={item:"420"}` binds `S.item` to stored item with key `"420"`


When updating the store, you should use method `storeState(key,data)`.

Example:

```jsx
import ItemStore;
rclass Foo{
	render(){
		return <div>
				<RoboUI.Avatar user={S.item.author}/>
				{S.item.title}
				<Button @click>change title</Button>
			</div>;	
	}
	
	handleClick(){
		storeState(
			P.key,
			{
				title:prompt("new title?")
			}
		);			
	}	
	getStore(){
		return ItemStore;
	}
		
	storedStates={
		item:P.key
	}


}
```

Now if we render the component with

```jsx
<Foo key="420"/>
```

We get an item that stays in sync with the server all the time without having to do anything 
else.


### 6.0.2 ItemStore.rjsx

ItemStore depends on proprietary *robo.ceo* backend components, so skip this if you're reading
this as an outsider.

ItemStore has pretty much the same API as Store and you don't have to worry about it's
internals. 

It keeps the stored data in sync with the server using Restful calls and listening to websocket
for change events.


## 6.1 WindowManager

WindowManager is only partially implemented, and not even the implemented parts work properly
yet so fuck the documentation. When it's done, it should handle the following:

* creating and handling 'windows' (not actual windows but elements that behave as such) inside 
the apps, with the ability to move and resize them
* 'task bar'
* layout mechanisms, tiling etc + something sane for mobile interfaces
* popups
* dialogs

## 6.2 Basic UI shit

Random UI related stuff that SemanticUI misses. Way more to come. 

### 6.2.0 CloseButton

A button that spawns in the top right corner of it's parent. Listen for events by adding an 
onClose property to it

### 6.2.1 ComboBox

A hybrid of Input and Select elements, i.e. a dropdown selection that also supports freeform
text. Accepts select options as children and can also be used with valueLinks (i.e. 
`<ComboBox @linkFoo/>` will work).

### 6.2.2 LoadingIcon

A simple loading icon. Defaults to 32x32 but you can specify the size with size property.

### 6.2.3 PopupDispatcher

Basically a thing that you could extend to make showing popup components easier. Internal
documentation sucks, and the API prob needs a strong re-evaluation so fuck this, just don't use
it yet.

### 6.2.4 Select

A pretty version of the html counterpart. //TODO document features

## 6.3 ASyncRoute

A router thingy. Basically if you go to `myapp/#/foo/bar/baz` it will try to render component
Foo (unless you have specified a different component for it) and give foo/bar/baz as arguments
to it. That History API thingy is not supported (yet) so you need to have that `#` before the
actual route.

As the name suggest, routes are being loaded asynchronously, so the components
responsible for rendering the route are loaded only when needed. This is pretty desirable in 
complex apps, but the mainstream routers don't seem to support it for some odds reason.

### 6.3.0 Usage

You need to do 3 things.

1. import ASyncRoute
2. provide a defaultRoute where the user is directed if there is no route
3. render `<ASyncRoute @route/>` somewhere 

You can also specify the components either by

1. putting them in a map eg.

```jsx
routes={
	foo:"TheComponentThatShouldHandleFoo",
	bar:"AndThisOneDoesBar"
}
```

or

2. implementing getRoute(key) method to do something fancier. The default implementation just
returns `this.routes?[key]||key`

```jsx
getRoute(key){
	return "StupidEnterpriseyNamingPractise"+key
}
```
	

The rendered component will then get the stuff after `#` separated by `/` as an array for it's 
args property.


## 6.4 ASyncComponent

If you need to render a component by it's name only this one is really handy. Just give the
component name as component property, and the other props you give it will be passed to the 
actual component.

example:

```jsx
<ASyncComponent component="div"> this sure is a silly way to render a div</>
```

## 6.5 SocketManager

SocketManager listens to websocket for JSON formatted event, responds to (and sends) pings and
keeps the connection alive.

Usage is reasonably straight forward.

1. import SocketManager
2. create a SocketManager instance with `ǹew SocketManager.Manager("[socket address]")`;
3. start listening to the socket, 
`mySocketManager.on([eventType("any" to get all events")],[callback])`. componendDidMount() is
usually the right place for this.

E.g.

```jsx
import SocketManager;
rclass SocketExample{
	socketManager=new SocketManager.manager("somesocket");
	componentDidMount(){
		socketManager.on("msg",evt->P.messages.push(evt));
	}	
	render(){
		return <div>{renderMessages()}</div>
	}
	renderMessages(){
		return P.messages?.map(this::renderMessage);	
	}
	renderMessage(m){
		return <div>
			<RoboUI.Avatar user={m.author}/>
			{m.msg}
			</div>
	}
}
```
 
 



# 7. SemanticUI components

RJSX comes with compiler level integration to SemanticUI library. The idea is to provide all the
basic UI components with an intuitive API, so that you can just put things together and get
pretty looking results without having to give two shits about it.

As the RJSX semantic ui should behave quite intuitively compared to their plain html
counterparts, we're not going to document all of the features. Just note a couple of things

1. You can add classnames easily (see 5.11.3)
2. The components usually accept *icon* and *label* as parameters where it makes sense
3. Components are translated server side to UIUnit components, so we don't need to load
a gazillon different React components to use SemanticUI as with certain other
React+SemanticUI solutions


For example this gets you a large red button with a bomb icon

```jsx
<Button large blue icon="bomb"/>
```

Here's a list of all the supported components. Overlined ones are known not to working properly 
at the moment. 

* ~~Accordion~~
* Actions
* Ad
* Author
* Avatar
* Bar
* Button
* Buttons
* Breadcrumb
* Card
* Comments
* Comment
* CheckBox
* Content
* Column
* Date
* Description
* Detail
* Dimmer
* Divider
* Dropdown
* Feed
* Fields
* Field
* Flag
* Form
* Grid
* Header
* Icon
* Image
* Input
* Item
* Label
* List
* LinkItem
* Loader
* Menu
* MetaData
* Message
* Modal
* Nag
* Or
* Progress
* Segment
* Rail
* Rating
* Reveal
* Sidebar
* Shape
* Search
* Step
* Steps
* Summary
* Statistic
* Tab
* Title
* Table
* User 

# 8. Layout engine
There is also a layout library + engine, but it's still largely a work in progress, so
documenting it is kinda pointless at this stage.

The layouts work either with flexbox (simpler stuff) or the layout engine 
(seems to have a few glitches left).

You'd use the layouts as RJSX elements, like this

```jsx
<Layout.BorderLayout>
	<div layout="top">top</div>
	<div layout="center">center</div>
	<div layout="right">right</div>
	<div layout="bottom">bottom</div>
	<div layout="left">left</div>

</Layout>

```

Few actually working layouts include:

* Layout.Wrapper, wraps the content+ has position:relative
* Layout.Centered, centers the contents both vertically and horizontally
* Layout.Spread, spreads the contents evenly horizontally




# 9. RoboUI components

<Note>Most of these are either proprietary or depend on proprietary parts of the backend, so if
you are reading this as and outsider, tough luck</Note>

As *robo.ceo* related tend to share a lot of the basic UI components, the basic stuff should be
bundled into a single library so that the components made can be easily 'remixed' into new
products. Since we havent gotten too deep yet in building the new UI, a lot of these are just
placeholder implementations at the moment.

The essentials are:

# 9.0 RoboUI.RoboApp

Make your App extend this class and you get all the basic app functionality. At the moment this
means mainly just login & register functionality.

You can customize the login window by setting the following fields in your main app:

* loginImg
* loginTitle
* loginDescription

Register customization is on it's way.

## 9.1 RoboUI.AbstractItem

Basic store interaction etc, slightly more convenient than doing it by hand every time.

You just need to have a data.id property for your component and it stays in sync with the
server.

## 9.2 RoboUI.AbstractList

Specification still lives, but the idea is that you extend this component, have a data.id
property and implement a renderItem method. Then you will have a well performing list that
stays in sync all the time etc.

## 9.3 RoboUI.Avatar

Not yet fully implemented, but the idea is that you give it an user id, and then you get the
avatar + some basic features such as a popup menu with the option to send pm's, open profile and
whatnot.

## 9.4 RoboUI.RoboIcon

Abstract icon with certain popup functions. Core class for TypeIcon and StatusIcon.

## 9.4 RoboUI.TypeIcon

Icon representing the items type + some functionality.

## 9.5 RoboUI.StatusIcon

Icon representing the items status + ability to change the status with a popup menu.

## 9.6 RoboUI.Search

Search widget.

## 9.7 RoboGraph

Graph visualization toolkit.

//TODO better specs



# 10. Testing

RJSX comes with it's own easy to use JUnit style testing framework. Unlike typical javascript
test frameworks it is about getting shit to work reliably and not so much about making your
code look like humanist essays on how the components make you feel deep down inside.

## 10.0 API

All test methods should be annotated with `@Test` so that *TestRunner* knows which ones to run.

TestRunner tries to feed the following arguments to test methods:

1. a reference to the TestRunner object
2. a callback method for asynchronous tests
3. a jQuery object reference to the testArea component

Test methods should do one of the following:

1. return true if the test should pass
2. return false if it should fail
3. throw an exception
4. return nothing **and** use the provided callback method




Here are a few examples of the api:

```jsx
@Test
simpleTest(){
	Assert.eq("bar",foo());
	return true;	
}

foo(){
	return "bar";
}
	
```

```jsx
@Test
moreComplexTest(t,callback,testArea){
	testArea(
		<MyComponent/>	
	);	
	$(".testarea .mycomponent").trigger("click");
	//this is all kinda ugly and repetitive, so a better solution is on its way	
	setTimeout(()->{
		try{
			Assert.style({
				backgroundColor:"red"
			},$(".testarea .mycomponent"));
			callback(true);
		}catch(e){
			callback(false);		
		}			
	},420);	

}
```

## 10.1 Assert

Assert is basically just an RJSX module located under frontendcommons/ that has all sorts of
methods that throw an exception if some condition is not met.

Currently only the following ones are implemented, but way more are to come:

* Assert.eq(a,b);
* Assert.isTrue(a);
* Assert.isFalse(a);
* Assert.isNull(a);
* Assert.isNotNull(a);
* Assert.fail(msg);

## 10.2 TestRunner

Running the tests is easy, just open `[url-to-your-project-root]/testrunner` in your browser and
you will see

1. a list of all the tests in your project
2. test area, where test can render things, and you can visually inspect if they are all right
 (obviously such inspectations are not a white mans job, and you should/could automatize it)

Then just click run all.

Here's a screenshot:

image:img/testrunner.jpg

<Note> Using testrunner in other projects than frontendcommons does not work yet unfortunately
</Note> 



# 11. Logging and error handling

## 11.0 Logging
RJSX has a SLF4J like logging facade system. The following functions are available and you don't
need to import anything to use them:

* L.trace(msg[,data]);
* L.debug(msg[,data]);
* L.info(msg[,data]);
* L.warn(msg[,data]);
* L.error(msg[,data]);

You configure the logger with `myinternet.json` (See section 4.1), but the default behaviour
should be just fine. By default log events go to FancyLogger when App is on debug mode 
(debug mode is on by default if you're running your app locally, and off when not).



### 11.0.0 FancyLogger

You can open the logger by focusing body (i.e. going out of text fields etc) and pressing 
`ctrl+shift+L`. You should then see a pretty window that looks like this:

image:img/logviewer.jpg

You can then filter the log events by:
* level
* calling RJSX component
* log message
* log data

By clicking the caller, you can see the stack trace of the event. When you click the stack trace
elements, a source viewer opens with the correct line hilighted. The source is for compiled js 
but as it only gets minified in production, it should still be readable.


<Note>
When in production, you should probably keep the logging off or use a different 
implementation. This is because stack traces are being recorded and that probably comes with 
a performance cost.
</Note>

## 11.1

By default, RJSX adds a try-catch loop around every method and delegates it to the default error
handler. You can change this with `myinternet.json`, see 4.1.

The default error handler shows a fancy error window with stack trace equipped with similar 
source viewer as in 11.0.0. At the moment there is no error reporting mechanism implemented, but
such features are planned.


 



# 12. RoboReact API

RJSX components have naturally the same API as their React counterparts, but with a few 
extensions. Most of those are internal, but there are a few public ones, and more may spawn
later on.


## 12.0 getElementName()
returns the element name

## 12.1 getCssClass()
returns this.cssClass or getElementName() if that's not present








# 13. Behind the scenes

Sorcery and black magic.


# 14. Contributing

We've not opensourced this yet, but let us know if you're interested in development or have
feedback/ideas.



