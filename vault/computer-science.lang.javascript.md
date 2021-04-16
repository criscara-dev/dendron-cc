---
id: 1eeab78a-4075-44c1-8316-d0d35922857a
title: Javascript
desc: ''
updated: 1618134475313
created: 1602345804873
stub: false
---
# Javascript

## The programming language features:

[[NodeJS|computer-science.lang.javascript.nodejs]]
[[Snippets|computer-science.lang.javascript.snippets]]
[[Data Structures|computer-science.lang.javascript.data]]
[[Functions|computer-science.lang.javascript.functions]]
[[Promises|computer-science.lang.javascript.promises]]
[[Debugging|computer-science.lang.javascript.debugging]]
[[React|computer-science.lang.javascript.react]]
[[DOM|computer-science.lang.javascript.dom]]
[[Objects|computer-science.lang.javascript.objects]]
[[HOF|computer-science.lang.javascript.hof]]
[[Spread operator|computer-science.lang.javascript.spread-operator]]
[[rest parameters|computer-science.lang.javascript.rest-parameters]]
[[Destructuring|computer-science.lang.javascript.destructuring]]
[[modules|computer-science.lang.javascript.modules]]
[[genertors|computer-science.lang.javascript.generators]]
[[Maps-weakMaps|computer-science.lang.javascript.Map-WeakMaps]]
[[computer-science.lang.javascript.fetch]]

<a name="arrayfrom">Array From</a>
<a name="closure">Closures</a>


## Scope & Context
1. **Scope** is the biggest source of confusion regarding variables.

`var` uses `function` scope
`let` uses `block` scope

`let` and `const` are **block scoped** (so they're not available outside the block) while `var` it is and as well a `function` since is `hoisted` (lifted up) and it is in the global scope or accessible from the `window` object.
The function walk up the __ladder__ from the inside (current scope) toward the outside until it found the variable is referring to. (see below)
```javascript
let myName = "Cris"
// let myName = 'Cris Junior'
function amazingFunction() {
if (2 + 2 == 4) {
// let myName = "Cris the 3rd"
console.log(`inside the if statement: ${myName}`)
    }
console.log(`inside our function: ${myName}`)
}
amazingFunction()
console.log(`inside thre global scope: ${myName}`)
```


2. **Context** is the biggest source of confusion regarding objects.

What the `this` keyword is pointing forward?
- **The `this` keyword points towards the `object` that is `executing` the current function.**

```javascript
let john = {
firstName :"John" ,
lastName: "Doe" ,
driveCar() {
function imAFunctionNotAMethod() { 
    console.log(this) // this DOESN'T point to the "john" object because is in the context of another function
    }
imAFunctionNotAMethod()
console.log( this.firstName + " " +this.lastName + " is driving a car. ")
    }
}
john.driveCar()
// ...

function breathe(){ console.log(this.firstName + " " + this.lastName + " can inhale!")}

breathe.call(john)
```
From above, last lines, `this` make the code flexible:
`function.call(object)`

## Hoisting

```javascript
greet()
function greet(){ // code block } // this funciton is hosted so it execute with th result: > ...
let greeting = function(){ // code block } // this function is not hoisted and the vaule will be undefined we need instead to call to make it works, id est (i.e.) below:
greeting()
```
---

[Array From](#arrayfrom):
```javaScript
Array.from( ... ) // to convert a DOM list (Nodes) or other type of iterable collection on the front side into array to be able to use Array methods like map, filter, reduce...
}
console.log(workshop, par, chi) // JavaScript cris cara
```

---

[Closure](#closure):
```javaScript

let workshop = 'JavaScript'

function parent() {
    const par = 'cris'
    return function child() {
    const chi = 'cara'
    }
}
console.log(workshop, par, chi) // JavaScript cris cara
```
[1].map( (cb) => cb)

It's a way to amintain a variable private from the outside scope.
The `cb` is a closure because is nested function within a function that has access to its parent scope
---
### Rerefences

- [Can I Use?](https://caniuse.com/)
- [Which features are supported by which engine?](http://kangax.github.io/compat-table)


