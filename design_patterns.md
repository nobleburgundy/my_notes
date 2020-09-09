# Design Pattern Notes

## Null Object pattern

One of the easier patterns to understand and start with. Basically, create a NullObject class so you don't have to check for if the object is null elsewhere in your code. For example, if you have a User class and for auth reasons, you're checking if that user exists. If you create a `NullUser` class, you can create the check once, then return NullUser if that user isn't found.

```js
class User {
  constructor(id, name) {
    this.id = id;
    this.name = name;
  }

  hasAccess() {
    return this.name === "James";
  }
}

class NullUser {
  constructor() {
    this.id = -1;
    this.name = "Guest";
  }
}

function getUser(id) {
  const user = users.find((user) => users.id === id);
  if (user == null) {
    return new NullUser();
  } else {
    return user;
  }
}

const users = [new User(1, "James"), new User(2, "Laura")];

console.log(getUser(3));
// returns:
/** 
{
  id: -1,
  name: "Guest"
}
**/
```

## Builder Pattern

Useful for creating objects that have many different parts that need to come together to create a single object.

### Classic Example...

```js
class Address {
  constructor(zip, street) {
    this.zip = zip;
    this.street = street;
  }
}

class User {
  constructor(name) {
    this.name = name;
  }
}

class UserBuilder {
  constructor(name) {
    this.user = new User(name);
  }

  setAge(age) {
    this.user.age = age;
    return this;
  }

  setPhone(phone) {
    this.user.phone = phone;
    return this;
  }

  setAddress(address) {
    this.user.address = address;
    return this;
  }

  build() {
    return this.user;
  }
}

let user = new UserBuilder("Bob").setAge(34).setPhone("123123").build();
console.log(user);
```

### Modern JS example (use javascript object {param1, param2 etc} as input)...

Use JavaScript object {} for paramenters and use defaults if needed...

```js
class Address {
  constructor(zip, street) {
    this.zip = zip;
    this.street = street;
  }
}

class User {
  constructor(name, { age, phone = "1234560", address } = {}) {
    this.name = name;
    this.age = age;
    this.phone = phone;
    this.addres = address;
  }
}

let user = new User("Bob", {
  age: 23,
  phone: "234-909-9456",
  address: new Address("1", "Main Stret"),
});
```

## Singleton Pattern

A way of creating a single object that is shared amongst other resources and objects. Only a single type of othis object is created. This can lead to some problems creating global variables and can be very difficult to test since the whole application is using this single object. Can also lead to race conditions. Some think you should never use this pattern, however there might be some small niche uses for this pattern. For example, if there is a single amount of small information that is shared throughout the application.

### Example (a logger)...

```js
// in fancyLogger.js
class FancyLogger {
  constructor() {
    if (FancyLogger.instance == null) {
      this.logs = [];
      FancyLogger.instance = this;
    }
    return FancyLogger.instance;
  }

  log(message) {
    this.logs.push(message);
    console.log(`FANCY: ${message} logs`);
  }

  printLogCount() {
    console.log(`${this.logs.length} Logs`);
  }
}

const logger = new FancyLogger();
Object.freeze(logger); // prevents object from being changed
export default logger;

// firstuse.js
import logger from "./fancylogger.js";

export default function logFirstImplementation() {
  logger.printLogCount();
  logger.log("First file");
  logger.printLogCount();
}

// seconduse.js
import logger from "./fancylogger.js";

export default function logSecondImplementation() {
  logger.printLogCount();
  logger.log("Second file");
  logger.printLogCount();
}

// index.js
import logFirstImplementation from "./firstuse.js";
import logSecondImplementation from "./seconduse.js";

logFirstImplementation();
logSecondImplementation();
```

## Facade Pattern

A good pattern for clean code that is easy to refactor in the future. It's creating a 'facade' between complex code and a simpler implimentation.

### Example (fetch users via api)

```js
// original fetch implimentation w/o facade pattern...
function getUsers() {
  return fetch("https://jsonplaceholder.typicode.com/users", {
    method: "GET",
    headers: { "Content-Type": "application/json" },
  }).then((res) => res.json());
}

function getUserPosts(userId) {
  return fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`, {
    method: "GET",
    headers: { "Content-Type": "application/json" },
  }).then((res) => res.json());
}

getUsers().then((users) => {
  users.forEach((user) => {
    getUserPosts(user.id).then((posts) => {
      console.log(user.name);
      console.log(user.id);
    });
  });
});

// function w facade
function getFetch(url, params = {}) {
  const queryString = Object.entries(params)
    .map((param) => {
      return `${param[0]}=${param[1]}`;
    })
    .join("&");
  return fetch(`"https://jsonplaceholder.typicode.com/users"`, {
    method: "GET",
    headers: { "Content-Type": "application/json" },
  }).then((res) => res.json());
}

// which makes it really easy to implement something like Axios...
function getFetch(url, params = {}) {
  return axios({
    url: url,
    method: "GET",
    params: params,
  }).then((res) => res.data);
}

// then above get functions above can be simplified after getUsers function created...
function getUsers() {
  return getFetch("https://jsonplaceholder.typicode.com/users");
}

function getUserPosts(userId) {
  return getFetch(`https://jsonplaceholder.typicode.com/posts`, {
    userId: userId,
  });
}
```

## Command Pattern

The command pattern takes the different operations and encapselate into individual operations that have a 'perform' and an 'undo' method. Useful applications of this pattern are usually complex actions such as adding users to a database or similar where the undo action can be pretty complex.

### Example

```js
// example calculator
class Calculator {
  constructor() {
    this.value = 0;
    this.history = [];
  }

  add(valueToAdd) {
    this.value = this.value + valueToAdd;
  }

  //with command pattern
  executeCommand(command) {
    this.value = command.execute(this.value);
    this.history.push(command);
  }

  undo() {
    const command = this.history.pop();
    this.value = command.undo(this.value);
  }

//   subtract(valueToSubtract) {
//     this.value = this.value - valueToSubtact;
//   }

//   multiply(valueToMultiply) {
//     this.value = this.value * valueToMultiply;
//   }

//   divide(valueToDivide) {
//     this.value = this.value / valueToDivide;
  }
}

class AddCommand {
  constructor(valueToAdd) {
    this.valueToAdd = valueToAdd;
  }

  execute(currentValue) {
    return currentValue + valueToAdd;
  }

  undo(currentValue) {
    return currentValue - valueToAdd;
  }
}

// above subtract, multiply, divide functions made into command pattern functions...
class SubtractCommand {
  constructor(valueToSubtract) {
    this.valueToSubtract = valueToSubtract;
  }

  execute(currentValue) {
    return currentValue - valueToSubtract;
  }

  undo(currentValue) {
    return currentValue + valueToSubtract;
  }
}

class MultiplyCommand {
  constructor(valueToMultiply) {
    this.valueToMultiply = valueToMultiply;
  }

  execute(currentValue) {
    return currentValue * valueToMultiply;
  }

  undo(currentValue) {
    return currentValue / valueToMultiply;
  }
}

class DivideCommand {
  constructor(valueToDivide) {
    this.valueToDivide = valueToDivide;
  }

  execute(currentValue) {
    return currentValue / valueToDivide;
  }

  undo(currentValue) {
    return currentValue * valueToDivide;
  }
}

const calculator = new Calculator();
calculator.executeCommand(new AddCommand(10));
console.log(calculator.value); // outputs 10
calculator.undo();
console.log(calculator.value); // outputs 0

class AddThenMultiply {
	constructor (valueToAdd, valueToMultiply) {
		this.addCommand = new AddCommand(valueToAdd)
		this.multiplyCommand = new MultiplyCommand(valueToMultiply)
	}

	execute() {
		const newValue = this.addCommand.execute(currentValue)
		return this.multiplyCommand.execute(newValue)
	}

	undo() {
		const newValue = this.multiplyCommand.undo(currentValue)
		return this.addCommand.undo(newValue);
	}
}
```

This example results in more code to implement the command pattern, so you wouldn't use if for something as simple as this. For more complex actions that might require an 'execute' and 'undo', the command pattern is a good choice. This pattern is really popular with text editors (ie bold shortcut and bold button both perform the same action and 'undo' unbolds it).
