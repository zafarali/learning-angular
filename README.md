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
Directives can communicate with each other as shown in the Reading List Example. Here we create a directive called `<book>` and a couple of attributes that are dependant on the `book` directive.Each `<book>` has a local scope as defined. `<book>` also has an API which can control the local scope. Each attribute we define accesses the `<book>` API to manipulate its properties. The properties can be viewed when we roll over it. A streamlined way (albeit without the romance,thriller etc. properties) is shown in [03-6-elements.html](https://github.com/zafarali/learning-angular/blob/master/03-6-elements.html)
We also have a `transclude` property which is demonstrated in [03-5-transclusion.html](https://github.com/zafarali/learning-angular/blob/master/03-5-transclusion.html) that allows whatever is inside our custom element to be placed inside it and not overwritten by the template.