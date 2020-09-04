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
