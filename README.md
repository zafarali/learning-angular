Learning AngularJS
================
So I've been meaning to learn AngularJS for some time now and I've watched a few videos here and there but kept on dropping and never building anything concrete. This repo will help me track my progress through two curriculums I've found on thinkster.io and codeschool.com:  
1. [A Better Way to Learn AngularJS](http://www.thinkster.io/angularjs/GtaQ0oMGIl/a-better-way-to-learn-angularjs)  
2. [Shaping up with AngularJS](http://campus.codeschool.com/courses/shaping-up-with-angular-js/intro)  
3. [AngularJS Tutorial: Learn to build Modern Webapps](http://www.thinkster.io/angularjs/r1gRPYp4kM/angularjs-tutorial-learn-to-build-modern-webapps)  

This repository will also include code from other sources which I will cite in the comments and below when I find them. 

This README will also track some codes, gotchas, comments and other shenanigans to help others like me learning AngularJS for the first/second/third time. Hope this helps!

#####Prerequisites
- [ ] HTML
- [ ] CSS
- [ ] JS (OOPs, Prototyping, functions, events, error handling)
- [ ] Idea of the Model-View-Controller technique
- [ ] The Document Object Model 

#####Requirements
- [ ] Web browser
- [ ] Acccess to the O'Rielly AngularJS Book. If you are a student you can access it [here](http://proquest.safaribooksonline.com/book/programming/javascript/9781449355852/firstchapter) using your university VPN.

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
- **Directives** apply special behaviour to HTML elements. For example the `ng-app` attribute in our [00-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-concepts.html) file is linked to a directive that automatically initializes the application. When we define `ng-model` attributes to our `<input>` element, we create a directive that stores and updates the value from the `<input>` field to the *scope*.
- **filter*s format the value of an expression for display to the user. Filters are very useful and in our [00-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-concepts.html) file we use `currency` to format the output of the expression to resemble a bill or currency.

The image summarizes [00-1-conteps.html](https://github.com/zafarali/learning-angular/blob/master/00-1-concepts.html)  
![Interaction between Controllers-Scope/Model-View](https://docs.angularjs.org/img/guide/concepts-databinding2.png)  *image from [AngularJs Docs](https://docs.angularjs.org/guide/concepts)*