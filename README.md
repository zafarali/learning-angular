Learning AngularJS
================
So I've been meaning to learn AngularJS for some time now and I've watched a few videos here and there but kept on dropping and never building anything concrete. This repo will help me track my progress through two curriculums I've found on thinkster.io and codeschool.com:  

1. [A Better Way to Learn AngularJS](http://www.thinkster.io/angularjs/GtaQ0oMGIl/a-better-way-to-learn-angularjs)  
2. [Shaping up with AngularJS](http://campus.codeschool.com/courses/shaping-up-with-angular-js/intro)  
3. [AngularJS Tutorial: Learn to build Modern Webapps](http://www.thinkster.io/angularjs/r1gRPYp4kM/angularjs-tutorial-learn-to-build-modern-webapps)  

This repository will also include code from other sources which I will cite in the comments and below when I find them. There are also basic code example as alternative to demos shown in the videos so that we can see more than one example to be able to accurately wrap ones head around.

This README will also track some codes, gotchas, comments and other shenanigans to help others like me learning AngularJS for the first/second/third time. Hope this helps!

#####Prerequisites
- [ ] HTML
- [ ] CSS
- [ ] JS (OOPs, Prototyping, functions, events, error handling)
- [ ] Idea of the Model-View-Controller technique
- [ ] The Document Object Model 

#####Requirements
- [ ] Web browser
- [ ] Acccess to the O'Rielly AngularJS Book. If you are a student you can access it [here](http://proquest.safaribooksonline.com/book/programming/javascript/9781449355852/firstchapter) using your university VPN. [*AngularJS by Brad Green and Shyam Seshadri (O’Reilly). Copyright 2013 Brad Green and Shyam Seshadri, 978-1-449-34485-6.*]

###00-Concepts
AngularJS relies heavily on the MVC approach:
- a **Model** which contains the data shown to the user in the view and with which the user interacts. (think of it as the place where we store the data)
- a **View** this is what the user sees. (the user interface or the DOM)
- the **controller** which contains all the logic.

There are some *buzzwords* used in AngularJS:
- **Data Binding** is the sync of data between the model and the view 
- **Two way Data Binding** is a feature of angular that says that everything in the document is live. Whenever the input changes the expression is automatically recacluated and the DOM is updated as well.
- **Scope** this is where the variables and data (known as model) are stored, think of it as the traditional *scope* that allows all other components to access the data in the model.
- **Templates** all HTML files with angular code are known as templates because angular must fill in expressions and other gadgematics.
- **Directives** apply special behaviour to HTML elements. For example the `ng-app` attribute in our [00-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-0-concepts.html) file is linked to a directive that automatically initializes the application. When we define `ng-model` attributes to our `<input>` element, we create a directive that stores and updates the value from the `<input>` field to the *scope*.
- **filters** format the value of an expression for display to the user. Filters are very useful and in our [00-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-0-concepts.html) file we use `currency` to format the output of the expression to resemble a bill or currency.

The image summarizes [00-1-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-1-concepts.html)  
![Interaction between Controllers-Scope/Model-View](https://docs.angularjs.org/img/guide/concepts-databinding2.png)  *image from [AngularJs Docs](https://docs.angularjs.org/guide/concepts)*

- **Services** contain all the 'independant logic', which is other words is a way to supply and manipulate data to our application. We use services to allow us to resue it in other parts of the application. For example in [00-1-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-1-concepts.html) all the logic was stored within `InvoiceController`. In [00-2-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-2-concepts.html) we will refactor the code to move the conversion logic into a service in another module.

- **Dependency Injection** In [00-2-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-2-concepts.html) we see that `ng-app` defines `invoice-srv-demo` as the main module to use in application. In the defintiion of this module we state that `finance` is a dependancy to the main module. We also define the constructor for the controller after passing in the dependancy `currencyConverter` from the finance module. This is known as *dependency injection*

###00-Spin
####The Dot.
Found this gotcha thanks to [this video](http://www.thinkster.io/angularjs/axAQatdKIq/angularjs-the-dot).
```html
	<script>
		function FirstCtrl($scope) {}
		function SecondCtrl($scope) {} 
	</script>
	<div ng-app="">
		<div ng-controller="myCtrl">
			<input type="text" ng-model="message">
			<br /><h1><b>{{message}}</b></h1>
		</div>
		<div ng-controller="yourCtrl">
			<input type="text" ng-model="message">
			<br /><h1><b>{{message}}</b></h1>
		</div>
	</div>
```
If we set up our code as above, *scope inheritence* is broken Each new `ng-model` that we set up for the `message` is creating a new instance of message which means there is no longer communication between the two `message`s. To rectify this we place a `data` model on the `$scope` which has the `message` property. I like to think of this as each controller getting its own scope because recitifying the code as shown below also doesn't fix the issue and no data is shared between the two controllers.
```html
	<!--Script remains the same-->
	<div ng-app="">
		<div ng-controller="myCtrl">
			<input type="text" ng-model="data.message">
			<br /><h1><b>{{data.message}}</b></h1>
		</div>
		<div ng-controller="yourCtrl">
			<input type="text" ng-model="data.message">
			<br /><h1><b>{{data.message}}</b></h1>
		</div>
	</div>
```
However editing the code in a way (below) such that there is a scope set up for the entire application will ensure that the scope can sync data between the controllers.
```html
<!--Script remains the same, we initialize data.message with 'hi'-->
	<div ng-app="" ng-init="data.message='hi'">
		{{data.message}}
		<div ng-controller="myCtrl">
			<h1>{{2+2}}</h1>
			Type in a message: <input type="text" ng-model="data.message">
			<br /><h1><b>{{data.message}}</b></h1>
		</div>
		<div ng-controller="yourCtrl">
			Type in a message: <input type="text" ng-model="data.message">
			<br /><h1><b>{{data.message}}</b></h1>
		</div>
```
*(this observation is indeed true, we need to have a parent to allow this kind of inheritence)*. We now look at a different way to do this.
####Data sharing between controllers
This can be done by creating a *factory* which will supply the data. We then bind the local `$scope`s to the `Data` factory where we can then sync data between the controllers. See [00-2-spin.html](https://github.com/zafarali/learning-angular/blob/master/00-2-spin.html).
```javascript
var myApp = angular.module('myApp', []);
myApp.factory('Data', function(){return{message:"this is important"};});
function myCtrl($scope,Data){$scope.data = Data;}
function yourCtrl($scope,Data){$scope.data = Data;}
```
###Shopping Cart (01-1-shopping and 01-1-forms)
The [shopping cart](https://github.com/zafarali/learning-angular/blob/master/01-1-shopping.html) example demonstrates some of the other functionality of the AngularJS framework. You may have noticed that we have seen two ways to creating `controller`s:
```javascript
//method 1
function Ctrl($scope) {};
//method 2
var myApp = angular.module('myApp', []);
myApp.controller('Ctrl', function($scope){});
```
We can compare the two using our shopping cart app in [this commit](https://github.com/zafarali/learning-angular/commit/ce54e0d417f80a0087035b62e92a6f8030ccd4c0#diff-1d019a5313eee180347d30c7801153a5). Method 2 ensures that we keep our controllers out of the global namespace using the module we defined.  
The docs page regarding [forms](https://docs.angularjs.org/guide/forms) we find a whole load of neat tricks and tips that are demonstrated in [01-01-forms.html](https://github.com/zafarali/learning-angular/blob/master/01-1-forms.html). The page also demonstrates the abilities of `ng-show`, `ng-hide`, and validation techniques that AngularJS provides.
###02-Filters
####Custom filters
There's only syntax to learn here, watched this [video] and see the (http://www.thinkster.io/angularjs/EnA7rkqH82/angularjs-filters) dummy code I [typed](https://github.com/zafarali/learning-angular/blob/master/02-1-filters.html) up. Another example is shown below using extra aruments that can be used as `text|decimal2binary:true`:
```javascript
//converts decimals to binary form (i don't think this really works)
app.filter('decimal2binary', function(){
	function convert(num,bool){
		if(bool){ console.log("you said true as argument!");}
		num = parseInt(num);
		if(num === 0){
			return 1;
		}else{
			remainder = num % 2;
			return convert(num/2)+""+remainder;
		}
	}
	return function(input, bool){
		return convert(input, bool);
	}
});
```
####Searching and filters
We can see an implementation of the search technique in [02-2-filters.html](https://github.com/zafarali/learning-angular/blob/master/02-2-filters.html). Some other in-built AngularJS filters are summarized below:
- `limitTo` will limit the number of results shown
- `orderBy` takes a string and will order the results using the property it maps to
- `lowercase` and `uppercase` turn the results to lower and upper case (these are best applied to the data binding as shown below)
- `date`, `currency` format our data and are self explanatory
- `number` takes a integer and rounds off the data to that decimal place. (eg: 0 would round off to 0dp)
```html
<span ng-repeat="element in list | filter: search | orderBy: 'lastname' | limitTo: 4">
{{element.lastname | uppercase}}, {{element.firstname | lowercase}}
</span>
```
###03-Directives
####Custom elements
Directives are one of the  most impressive features AngularJS has to offer. In our first example we create a directive which is of `restrict`ion `'E'` which means that it is an element. Our directive is simple in that it just shows an image of a calculator created by the `<caclulator>` element. The [commit](https://github.com/zafarali/learning-angular/commit/fa0b6c1864501592e73ef3cea3e47e34c9763942) shows the code in detail.
####Custom Attributes and Classes
This [commit](https://github.com/zafarali/learning-angular/commit/403aad17fce1eb09e58b759dad63025db5166cf8) demonstrates a custom attribute we build. Here instead of setting `restrict:"E"` we set `restrict:"A"` and provide a `link` function which is executed whenever the attribute is attached. If we set `restrict:"C"` then the directive is executed whenever we have the class with that name [(see commit)](https://github.com/zafarali/learning-angular/commit/552ae91250175137f9e2b742883c7973c463dd48). We can also have directives in comments using `restrict:"M"` and using the following:
```html
<!--directive:myDirective-->
```
####Useful Directives
Directives default to `restrict:"A"`. The directive will also pass in the `scope` and `element` which we can use. As shown in [03-2-directives.html](https://github.com/zafarali/learning-angular/commit/d9e6515d2c8dd7b816907f98b9fa8f75472d4956). The directive also passes the `attrs` which is an object with all the attributes of the element. We can exploit this to make our code abstract when we want to add/remove classes. This is demonstrated in [this commit](https://github.com/zafarali/learning-angular/commit/a0636adf5790aeeb7d20489133bdc23d7d338578). We also see the use of `$apply()` to evaluate functions in [this example](https://github.com/zafarali/learning-angular/blob/master/03-3-directives.html).
####Directive Communication ([03-4-readinglist.html](https://github.com/zafarali/learning-angular/blob/master/03-4-readinglist.html))
Directives can communicate with each other as shown in the Reading List Example. Here we create a directive called `<book>` and a couple of attributes that are dependant on the `book` directive.Each `<book>` has a local scope as defined. `<book>` also has an API which can control the local scope. Each attribute we define accesses the `<book>` API to manipulate its properties. The properties can be viewed when we roll over it. A streamlined way (albeit without the romance,thriller etc. properties) is shown in [03-6-elements.html](https://github.com/zafarali/learning-angular/blob/master/03-6-elements.html).  
- **Transclusion** We also have a `transclude` property which is demonstrated in [03-5-transclusion.html](https://github.com/zafarali/learning-angular/blob/master/03-5-transclusion.html) that allows whatever is inside our custom element to be placed inside it and not overwritten by the template. 
- *Nested elements* and how they communicate with each other are demonstrated in [03-7-nested.html](https://github.com/zafarali/learning-angular/blob/master/03-5-transclusion.html)

####Directive properties
Here is a summary of the properties I see are most useful in directives:
- `restrict:"E"||"A"||"C"||"M"` this declares how the directive behaves, as an element, attribute, class or comment respectively. We can use any combination of these.
- `template:"string"` allows your directives to have a template.
- `templateUrl:"string"` allows your directive to load a URL as a template.
- `replace:bool` if true it will replace the current element and by default it is false and will append to the element.
- `transclude` Lets you move the children of the directive into a place inside the template. We do this by specifiying `ng-transclude` attribute of the target location. example (here it targets the `<span>` and not the other `<div>`s:
```javascript
//...
transclude:true,
template:"<div><div><span ng-transclude></span></div></div>",
//...
```
- `link:function()` executes when the directive sets up. Can be useful in setting `timeout`s, or binding `event`s to the element
- `controller:function()` allows you to set up scope properties and create an interface for other directives to interact with.
-`scope:{}` allows you to create a new scope for the element rather than inherit from the parent
- `require:['^directives']` makes it mandatory for the current directive to be nested in the parent to ensure it functions correctly.  
A quick gotcha to note, when you name your directive `datePicker` when declaring it, you can refer to it as `date-picker` in your HTML.

####Pre-link, Post-link and Compile
From the readings it seems there are two important phases when an angular application is being created. 
1. The Loading of the Script (not so important for us right now)
2. The **compile phase** is when the directives are registered, the templates are applied and the *compile* function is executed.
3. The **link phase** occurs once the templates have been applied and the compile functions have been executed. In this phase the *link* functions are executed whenever the data in the view is updated.  
The main difference I see in them is that the *compile* function is executed once only during the compilation stage while the `link` function is executed once for each instance of the directive. At the moment I do not see an immediate use case for the compile function, however the book suggests that it is useful when you need to modify something in relation to all cases. Note the difference between the function defintions below, the `compile` function doesn't have access to the scope because it is not created yet.
```javascript
return {
	restrict:"",
	require:"",
	controller:function(scope,element,attrs){},
	compile: function(templateElement, templateAttrs, transclude){},
	link: function(scope, instanceElement, instanceAttrs, controller){}
}
```
If we take a look at [03-8-compilevslink.html](https://github.com/zafarali/learning-angular/blob/master/03-8-compilevslink.html), we see the log executes the `controller` and the `compile`'s `pre` and `post` but not the link. Trying different combinations it seems that if you have a `compile`, you cannot have a `link` but you can get the effect of having a `compile` and `link` by setting a `pre` and `post` function to the `compile` function.  
*UPDATE: I have just seen a [video](http://www.thinkster.io/angularjs/h02stsC3bY/angularjs-angular-element), it is indeed true that you cannot have a `link` function and a `compile` function. However, if we do choose to have a `compile` function, we can return (from the `compile` function) another function that will become the `link` function. Go see the website for a nice example on how this works.

###04-Scopes
Scopes can be nested. These nested scopes can either be *child scopes* (which inherits properties from its parents) or *isolate scopes* (which doesn't inherit anything). Everything that we try to evaluate for example `{{name}}` which is input via `ng-model='name'` will be held on the *scope*. Thus we can think of scopes as the link between the controllers/directives and the view.
####Isolate Scope
We can demonstrate a simple way to create an isolate scope for each directive we create as shown [04-0-scope.html](https://github.com/zafarali/learning-angular/blob/master/04-0-scope.html). When we define a directive, a property we can return is `scope`. This can be set to be `true`, `false`, or `{/*stuff inside*/}`. When we set to `false` (by default), it will use the existing scope in the directive. `true` will inherit the scope from the parent. `{}` will create an isolate scope for each directive. When you define an isolate scope there are *binding strategies* or methods of getting data into that scope. They are discussed below.
##### @
We demonstrate two methods of using attributes to store data into the directives isolate scope in [04-1-scope.html](https://github.com/zafarali/learning-angular/blob/master/04-1-scope.html) 
##### =
The `@` operator will expect a string to bind the information we want to pass, however, the `=` will expect an object, this allows us to set up a two way binding between the parent scope and our directive scope. [04-2-scope.html](https://github.com/zafarali/learning-angular/blob/master/04-2-scope.html) demonstrates how updating the directive scope updates the parent. Note that in our attribute we are now passing objects and not strings.
##### &
The `&` operator will evaluate an expression that you pass to it. So it seems that if you are passing values between the controller and the directive you need it to be in an object, and the object must map its properties to the function. This is demonstrated in [04-3-scope.html](https://github.com/zafarali/learning-angular/blob/master/04-3-scope.html).
##### All together now!
[04-4-review.html](https://github.com/zafarali/learning-angular/blob/master/04-4-review.html) encapsulates all the concepts we have discussed so far. Note that when we use `=`, we assume that the user is going to order from the same bookstore and thus we would like reflect the change in all other directives.
#### Some Comments
* We see that there is * **one** root scope*, and Angular will first look in the current scope before going up to the parent until it reaches the root scope. A demonstration follows:
```html
<script>
function MyParentCtrl($scope){
	$scope.name='John Doe'; 
	$scope.home='Canada';
}
function MyChildCtrl($scope){
	$scope.name='Jane Doe';
}
</script>
<div ng-app>
<dig ng-controller="MyParentCtrl">
Hello {{name}}! You are from {{home}}
<div ng-controller="MyChildCtrl">
Hello {{name}}! You are from {{home}}
</div></div></div>
```
Which will print out `Hello John Doe! You are from Canada
Hello Jane Doe! You are from Canada`. This demonstrates the point.
* Broadcasting and Emmiting
`$emit(name, args)` will allow an event to be executed in current and parent scopes. `$broadcast(name,args)` will allow an event to be executed in current and child scopes.
This will be demonstrated in future pages but can be seen very nicely on this page [Scope Events Propagation](https://docs.angularjs.org/guide/scope#scope-events-propagation).
* `Controller as` Syntax allows us to refer to the Controller by an *alias*:
```html
<script> function MyLongCtrlName(){
	this.clickMe = function(){
		alert("You clicked me!!");
	}

	this.name = "Type your name here...";
}</script>
<div ng-app ng-controller="MyLongCtrlName as ctrl1">
<input type="text" ng-model="ctrl1.name">
{{ctrl1.name}} 
<button ng-click="ctrl1.clickMe()">Click this button</button>
</div>
```
It seems to add clarity to the code and in larger applications I think I will definietly be going to use it more often! 

### 05-Other Paradigms and tiny shenanigans
Some alternative ways of thinking of Controllers and different ways of organizing angular applications is also provided on the thinkster.io page. I summarize them here very briefly:
* Think of setting up a controller like this:
```javascript
app.controller("Ctrl", function($scope){
	this.hi = 'Hello';
	this.sayHi = function(){
		alert(this.hi);
	}
	return $scope.Ctrl = this;
});
```
We can acess this now using the following html:`<div ng-click="Ctrl.sayHi()">Click</div>`. This serves to make the controller explicit and closely mimics the `Controller as` syntax above.
* Organization. We can organize and initialize our controllers and directives like this. (however this doesn't work for filters)
```javascript
var app = angular.module('myApp', []);
var myAppComponents = {};

myAppComponents.controllers = {};
myAppComponents.controllers.AppCtrl1 = function($scope){/*stuff here*/};
myAppComponents.controllers.AppCtrl2 = function($scope){/*stuff here*/};

myAppComponents.directives = {};
myAppComponents.directives.mydirective = function(){return {/*stuff here*/}};

app.directive(myAppComponents.directives);
app.controller(myAppComponents.controllers);
```
* When code bases become very large, we need modules. We have already seen that we have our main app in a module but we can actually create modules seperately. We have already shown this in [00-2-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-2-concepts.html) but just to drive home the point we reiterate using a directive and a factory in [05-0-modules.html](https://github.com/zafarali/learning-angular/blob/master/05-0-modules.html).

* `$index`: We have already seen in past examples `$index` which holds the index of the current item in an `ng-repeat`. 

#### Mouse Events
We can use AngularJS listeners along with the `$event` object to do some interesting things. The directives are named in the following convention `ng-listener` where listener can be `click`, `dbl-click`, `mouseenter` etc...  Demonstrated in [05-1-event.html](https://github.com/zafarali/learning-angular/blob/master/05-1-event.html) is how we use `$event` to log the event. We can, by the same logic, pass the `$event` into a function that deals with the event in question.

#### Console logging
Angular has a built in method to log things to the console using `$log`. The [documentation](https://docs.angularjs.org/api/ng/service/$log) summarizes everything very simply and has a nice example that demonstrates it.

#### Observing the model
`$watch(toWatch, toDo, deepWatch)` is a function that allows you to observe changes in your model. We must note that executing this operation returns another function that if executed stops the watching. For example:
```javascript
$scope.$watch('username', function(){
	if($scope.username=='secret'){
		console.log('You typed in a secret name');
	}
});
```
Now assume we have a `ng-model` input to allow the user to type in the `username`. When the value updates and evaluates to `secret` the message will be logged to the console.
`$watch` can also watch functions to monitor their return function. The `deepWatch` parameter is a boolean. If set to true and an array of objects is passed into the `toWatch` parameter, each property of the object will be checked for changes.

#### Angular Classes
Angular automatically adds classes to some elements depending on how its being used. Here are some use cases:
* `ng-invalid`/`ng-valid` will be on an input element depending on if the text inside has passed validation or not
* `ng-pristine`/`ng-dirty` - if the user hasn't interacted with an input element it will have the class of `ng-pristine` and if it has been interacted with it will be `ng-dirty`.
* `ng-binding` is given to any element that has data binding on it.
* `ng-scope` is given to any element with a new scope.
* `ng-class` is an attribute of an element. We pass in an object with keys that are CSS class names and values are conditions. An example follows:
```javascript
<div ng-class="{ showClass: whenThisIsTrue, showErrorClass: whenErrorIsTrue, neutralClass: !whenThisisTrue && !whenErrorIsTrue }">...</div>
```
More information found [here](https://docs.angularjs.org/guide/css-styling)

#### element()
AngularJS implements jqLite which is a light version of jQuery. Thus if you are using jQuery on your page you can treat it as a jQuery element, if not we can just use the jqLite API to interact with it. We can see which jQuery methods that jqLite supports over at [the Angular Documentation](https://docs.angularjs.org/api/ng/function/angular.element#angular-s-jqlite).  
A few quick tip:
* Never use `element` in controllers as controllers only have logic for views.
* Try to avoid using jQuery. If we must there is a [video here](https://www.youtube.com/watch?v=bk-CC61zMZk) which suggests that we keep jQuery manipulation within link functions of directives. *I haven't tried this myself yet*

### 06-Template
Templates are an easy way of making your code very organized. [06-0-templateurl.html](https://github.com/zafarali/learning-angular/blob/master/06-0-templateurl.html) demonstrates how to link a partial file into the main view using the `templateUrl` property of a directive. Note that to use `templateUrl` we must have our files on a server. We can use `$templateCache` to load strings of html into the cache to mimic this effect if we do not have a seperate .html file. We then execute it using `app.run()` function.
We can retrieve whats on `$templateCache` using the `get` function and we can insert things into it using the `put` function as seen in the example at [06-1-templatecache.html](https://github.com/zafarali/learning-angular/blob/master/06-1-templatecache.html)
