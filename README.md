An ES6 cheet sheet made after following this excellent [ES6 tour](https://ponyfoo.com/articles/tagged/es6-in-depth). Written to be useful especially for Scala/Java/C# dev doing a bit of JS on the side (by choice... or more likely because of projects constraints).

## Let / Const
Block-scoped declarations. Avoid ```var``` and use the newer keywords ```const``` if possible, ```let``` if not.
```js
const immutableRef = 3
let mutableRef = 4
```
Note : Be mindful of the [Temporal Dead Zone](http://jsrocks.org/2015/01/temporal-dead-zone-tdz-demystified/) and [hoisting](https://ponyfoo.com/articles/javascript-variable-hoisting)

## Classes
```js
class Car {
  constructor (startDistance) {
    this.distance = startDistance
  }
  move () {
    this.distance += 2	
  }
  static isFarther (left, right) {
    return left.distance > right.distance
  }
}

//// Inheritance
class FastCar extends Car {
  constructor () {
    super(0)
  }
	move () {
    super.move()
    this.distance += 4
  }
}
```
## Template literals
```js
var {count, today} = { count: 3, names: ['john', 'joe', 'josh'], today: new Date()}
var html = `<p>
  	I'm "happy" to have ${count} brothers, today ${today.toLocaleString()}
  	They are called
	</p>
	<ul>
  	${names.map(name => `<li>${name}</li>`).join('\n')}
	</ul>
`

//// Raw template
var singleLine = String.raw`The "\n" newline won't result in a new line.`
```
NOTE : you can create your own template functions, usable in the same fashion as String.raw , see [here](https://ponyfoo.com/articles/es6-template-strings-in-depth#demystifying-tagged-templates)

## Destructuring 
Good for : 
- read object returned by function
- default function parameter value ```function random ({ min=1, max=300 } = {}) {}```
- [regexp extraction](https://ponyfoo.com/articles/es6-destructuring-in-depth#use-cases-for-destructuring)
```js
var [a] = [10]
var [,,a,b] = [1,2,3,4,5]
var {foo: a, bar: b} = {foo: 1, bar: 2}

var foo = { bar: { deep: 'pony', dangerouslySetInnerHTML: 'lol' } }
var {bar: { deep, dangerouslySetInnerHTML: sure }} = foo

var key = 'dynamic'
var { [key]: foo } = { key1: 'foo' }  // foo == 'foo'

//// Swapping
var left = 10; var right = 20;
if (right > left) {
  [left, right] = [right, left];
}

//// Default values
var {foo=3} = { bar: 2 }

//// Function parameters
function greet ({ age, name:greeting='she' }) {
  console.log(`${greeting} is ${age} years old.`)
}
greet({ name: 'nico', age: 27 })
```

## Arrow Functions
```js
[1, 2, 3, 4].map((num, index) => num * 2 + index)

[1, 2, 3, 4].map(num => {
  var multiplier = 2 + num
  return num * multiplier
})
```
Note: Careful with their [Lexical Scope](https://derickbailey.com/2015/09/28/do-es6-arrow-functions-really-solve-this-in-javascript/) !

## Object literals
```js
//// Property value shorthand
function clear () { elt = {} }
module.exports = { clear }

function getCar(make, model) {
	return {
		make,  // same as make: make
		model, // same as model: model
		['make' + make]: true,

    // omits `function` keyword & colon
		depreciate() {
			this.value -= 2500;
		}
}
```

## Rest parameters
```js
function sum (multiplier, base, ...numbers) {
  var sum = numbers.reduce((accumulator, num) => accumulator + num, base)
  return multiplier * sum
}
```

## Spread operator
| Use Case       | ES6                       | ES5 |
| -------------  | -------------             | ------------- |
| Destructuring  | ```[a, ...rest] = list``` | ```a = list[0], rest = list.slice(1)```  |
| Concatenation  | ```[1, 2, ...more]```     |```[1, 2].concat(more)```  | 
| Push onto list | ```list.push(...[3, 4])```| ```list.push.apply(list, [3, 4])```  |
