# Custom Errors (Error ko extend karna)

## Kyun?
Apne app ke hisaab se alag-alag errors chahiye hote hain: `HttpError`, `DbError`, `ValidationError`... Inme extra info bhi ho sakti hai (jaise `statusCode`).

`Error` se inherit karo, taaki `err instanceof Error` chale aur `message`, `name`, `stack` mile.

## Basic custom error
```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);                 // (1) zaroori
    this.name = "ValidationError";  // (2) naam sahi karo
  }
}
```
Parent constructor `message` set karta hai aur `name` `"Error"` rakhta hai, isliye `name` khud badalte hain.

Use:
```javascript
function readUser(json) {
  let user = JSON.parse(json);
  if (!user.age)  throw new ValidationError("No field: age");
  if (!user.name) throw new ValidationError("No field: name");
  return user;
}

try {
  readUser('{ "age": 25 }');
} catch (err) {
  if (err instanceof ValidationError) {
    console.log("Invalid data: " + err.message);
  } else if (err instanceof SyntaxError) {
    console.log("JSON Syntax Error: " + err.message);
  } else {
    throw err; // unknown, rethrow
  }
}
```
Type check ke liye `err.name` se behtar `instanceof` hai, kyunki naye subtypes banne par bhi kaam karta rahega.

## Aur specific errors (inheritance)
```javascript
class PropertyRequiredError extends ValidationError {
  constructor(property) {
    super("No property: " + property);
    this.name = "PropertyRequiredError";
    this.property = property;
  }
}
```
Isme extra info (`property`) bhi rakh sakte ho.

### `this.name` baar-baar likhne se bachna
Ek base class bana lo:
```javascript
class MyError extends Error {
  constructor(message) {
    super(message);
    this.name = this.constructor.name;
  }
}

class ValidationError extends MyError {}
class PropertyRequiredError extends ValidationError {
  constructor(property) {
    super("No property: " + property);
    this.property = property;
  }
}
```
Ab `name` apne aap sahi ho jaata hai.

## Wrapping exceptions
Problem: `readUser` kai tarah ke errors de sakta hai (`SyntaxError`, `ValidationError`...). Bulane wale ko har type check nahi karna chahiye.

Solution: ek general error `ReadError` banao aur low-level errors ko usme **wrap** kar do. Original error `cause` me rakho.

```javascript
class ReadError extends Error {
  constructor(message, cause) {
    super(message);
    this.cause = cause;
    this.name = "ReadError";
  }
}
```
`readUser` ke andar `SyntaxError` / `ValidationError` pakad ke `throw new ReadError("...", err)` karo. Bahar wala code sirf `ReadError` check karega, aur details chahiye to `e.cause` dekh lega.

## Task
`FormatError` jo `SyntaxError` se inherit kare:
```javascript
class FormatError extends SyntaxError {
  constructor(message) {
    super(message);
    this.name = this.constructor.name;
  }
}
```

## Yaad rakho
- `Error` extend karo, `super(message)` mat bhoolo, `name` set karo.
- Check ke liye `instanceof`.
- Wrapping se high-level, saaf error handling milti hai.
