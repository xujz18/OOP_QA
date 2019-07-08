# Functional programming
计81 胥嘉政 2018011300

## Brief introduction
* Structured programming
* Think of computer operations as mathematical function calculations
* Function can serve as input&output as parameter

## Functions are first-class citizens
* Characters similar with numbers (take JavaScript as example)

```
//1.can be stored as variables
var fortytwo = function () {return42; };

//2.can be stored as an element in an array
var fortytwos = [42, function () {return42; }];

//3.can be member variables of an object
var fortytwos = [number: 42, fun: function() {return 42;}];

//4.can be created directly when being used
42+(function() {return 42; }) ();
	//=84
```

## Higher order function
```
//function can be passed to another function as a parameter
function weirdAdd (n,f) {x=n+f(); return x;}
weirdAdd (42, function() {return 42;} );
	//=84

//function can be returned by another function
function weirdAdd () {return function() {return 42;} }
```
### Further example application
```
_.max ([1,2,3,4,5]);
	//=5
```
```
_.max ([ {name: "Fred", age: 65}, {name: "Lucy", age: 36} ]);
	//can not compare what is bigger
```
```
var people = [ {name: "Fred", age: 65}, {name:"Lucy", age:36} ];
_.max (people, function(p) {return p.age}];
	//={name: "Fred", age:65}
	//still need system > to compare
```
```
finder (plucker('age'), Math.max, people);
	//={name: "Fred", age:65}
finder (plucker('name'), function(x, y) {return (x.charAt=="L" ? x:y}, people);
	//={name:"Lucy", age: 36}
```
Different fucntions as different  parameters to achieve different goals.

## Create fucntion with function
### Currying(柯里化）
```
function curry2 (fun) {
	return function (secondArg) {
		return function (firstArg) {
			return fun (firstArg, secondArg);
		}
	}
}
```
```
function div(n,d) {return n/d;}
var div10 = curry2 (fun) (10)
div10 (50)
	//=5
```

### The conduit（管道）
```
rev(_.rest(_.initial(_.compact([2,3,null,1,42,false]))));

pipeline ( [2,3,null,1,42,false],
			_.compact,
			_.initial,
			_.rest,
			rev);
```
```
function firstEditions (table) {
	return pipeline (table,
						function (t) {return as(t,{ed: 'edition'})},
						function (t) {return project(t, ['title', 'edition', 'isbn'])},
						function (t) {return restrict(t, function(book) {return book.edition==1; })})
}
firstEdition (library);
```
```
var RQL = {
	select: curry2(project),
	as: curry2(as),
	where: curry(restrict)
};
function allfirstEditions (table) {
	return pipeline (tabel,
						RQL.as({ed:'edition'}),
						RQL.select(['title', 'edition', 'isbd']),
						RQL.where(function(book) {return book.edition == 1;}))
}
allfirstEdition (table);
```

## Summary
* Functions are First-class citizens
* Higher order function
* Create function with function using Currying
* The Conduit