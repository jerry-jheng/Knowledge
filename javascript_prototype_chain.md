# Javascript Prototype Chain
ref: [Javascript 面向對象編程（一）：封裝](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_encapsulation.html)

ref: [Javascript 面向對象編程（二）：構造函數的繼承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)

ref: [Javascript 繼承機制的設計思想](http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html)

ref: [Javascripter 必須知道的繼承 prototype, [[prototype]], \_\_proto__](https://medium.com/@peterchang_82818/javascripter-%E5%BF%85%E9%A0%88%E7%9F%A5%E9%81%93%E7%9A%84%E7%B9%BC%E6%89%BF%E5%9B%A0%E5%AD%90-prototype-prototype-proto-object-class-inheritace-nodejs-%E7%89%A9%E4%BB%B6-%E7%B9%BC%E6%89%BF-54102240a8b4)

ref: [從\_\_proto__和prototype來深入理解JS物件和原型鍊](https://github.com/creeperyang/blog/issues/9)

ref: [\_\_proto__ VS. prototype in JavaScript](https://stackoverflow.com/questions/9959727/proto-vs-prototype-in-javascript)


```javascript
// 1. Object
// Premitive Data Type
var a = 1
var s = "foo"
var b = true
var obj = { a: 1 }
NaN
undefined
null

// 2. Function
// Functions are first-class citizen
function foo1 () {}

var foo2 = function () {
    console.log("foo")
}

// Ananymous function
console.log(foo1 instanceof Object) // ture
console.log(typeof(foo2.prototype)) // object

// 3. Prototype
var A = { value: "" }

var a1 = {}
a1.value = 1
var a2 = {}
a2.value = 2

/// Improve
var makeA1 = function(value) {
    return {
        value: value
    }
}
var a3 = makeA1(3)

// Constructor
var makeA2 = function(value) {
    this.value = value
}

var a4 = new makeA2(4)

console.log(a4)
console.log(a4.constructor == makeA2.prototype.constructor) // true
console.log(a4.__proto__.constructor == makeA2.prototype.constructor) // true

// Prototype Chain
(new Foo).__proto__ === Foo.prototype
(new Foo).prototype === undefined

console.log("###")
console.log(a4)
console.log(a4.__proto__)
console.log(a4.__proto__.__proto__)
console.log(a4.__proto__.__proto__.__proto__)
console.log("###")

// Example 1

var Cat = function(color) {
    this.color = color
}

Cat.prototype.size = 100

var cat1 = new Cat("white")
console.log(cat1)
console.log(cat1.size)

var cat2 = new Cat("red")
Cat.prototype.size = 200
console.log(cat2)
console.log(cat2.size)
console.log(cat1.size)

// Example 2

var Dog = function(age) {
    this.age = age
}

var Animal = function() {
    this.leg = 4
}

var Creature = function() {
   this.temp = 37
}

Animal.prototype = new Creature()
Animal.prototype.constructor = Animal

Dog.prototype = new Animal()
Dog.prototype.constructor = Dog

var dog1 = new Dog(1)
console.log(dog1)
console.log(dog1.constructor)
console.log(dog1.temp)
```