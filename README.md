# pattern-note

## getter-setter pattern

The pattern is known as the getter-setter pattern, or accessor properties. This pattern allows you to define special methods in an object which will get called when a property is accessed (`get`) or modified (`set`). This can be useful in various scenarios, for instance, when you want to run some code every time a property is changed or when you want to compute the returned value dynamically.

Here is an example of how this might be used:

```javascript
let person = {
  firstName: 'John',
  lastName: 'Doe',
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  set fullName(name) {
    let parts = name.split(' ');
    this.firstName = parts[0];
    this.lastName = parts[1];
  }
}

console.log(person.fullName); // John Doe
person.fullName = 'Jane Doe';
console.log(person.firstName); // Jane
console.log(person.lastName); // Doe
```

In this example, we define a `fullName` property with a getter and a setter. The getter concatenates `firstName` and `lastName` to create a full name. The setter splits a full name into `firstName` and `lastName`.

This pattern is also often used in classes:

```javascript
class Circle {
  constructor(radius) {
    this._radius = radius; // the underscore is a common convention when you want to avoid name clashes
  }

  get radius() {
    return this._radius;
  }

  set radius(radius) {
    if (radius <= 0) {
      throw new Error('Radius should be greater than zero');
    }
    this._radius = radius;
  }

  get area() {
    return Math.PI * this.radius * this.radius;
  }
}

let circle = new Circle(5);
console.log(circle.radius); // 5
console.log(circle.area); // 78.53981633974483
circle.radius = 10;
console.log(circle.radius); // 10
console.log(circle.area); // 314.1592653589793
```

In this example, a `Circle` class is defined. The `radius` property is controlled with getter and setter. The getter simply returns the `_radius` value, while the setter validates that the radius is not set to a negative or zero value. There's also an `area` getter, which computes the area of the circle using its radius.

The MDN Web Docs is a great resource to learn about getters and setters in JavaScript: [MDN Web Docs: Getter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get) and [MDN Web Docs: Setter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set).

## adder-remover pattern

```javascript
const add = (store: any, key: string, id: string) => {
  const ids = store.get(key);
  if (!_.includes(ids, id)) {
    store.set(key, _.concat(ids, id));
  }
};

const remove = (store: any, key: string, id: string) => {
  const ids = store.get(key);
  if (_.includes(ids, id)) {
    store.set(key, _.without(ids, id));
  }
};
```

```javascript
const addVal = (key: string, val: any) => !_.includes(store.get(key), val) && store.set(key, _.concat(store.get(key), val));

const removeVal = (key: string, val: any) => _.includes(store.get(key), val) && store.set(key, _.without(store.get(key), val));
```

above two blocks are equivalent, it includes the condition check in the function body, but that is not necessary:

```javascript
const addVal = (key: string, val: any) => store.set(key, _.union(store.get(key), [val]));

const removeVal = (key: string, val: any) => store.set(key, _.without(store.get(key), val));
```
