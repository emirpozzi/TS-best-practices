# Typescript Best Practices

- ## Make types/interface for any object

```typescript
interface Person {
  name: string;
  age: number;
}
type Country = 'Sweden' | 'Norway' | 'Finland' | 'Denmark';
```

- ## Type every function input and output. Applies to arrow functions too

```typescript
(x: number): number => x * 2;
```

- ## Use types over enums

```typescript
// DON'T
enum Countries {
  Sweden = 'Sweden',
  Norway = 'Norway',
}
// DO
type Countries = 'Sweden' | 'Norway';
```

- ## Use immutable types when an object doesn't need to change after creation

```typescript
const person : Readonly<Person> = { ... }

const listNumbers : ReadonlyArray<number> = [1,2,3,4]

const map : Readonly<Record<string, number>> = {"john" : 34 }
// TRY TO DEFINE CUSTOME READONLY VERSIONS OF TYPES
type ReadonlyRecord<K extends string|number|symbol,V> = Readonly<Record<K,V>>
const foo : ReadonlyRecord<string,number> = {
  "hello" : 1
}
// THIS CODE WILL NOW GENERATE COMPILE WARNINGS
foo["hello"] = 2
foo["newKey"] = 0
```

- ## Apply functional programming: do not mutate inputs but create new ones

```typescript
// DON'T
function toUpperCase(input: string): void {
  input.toUpperCase();
}
// DO
function toUpperCase(input: string): string {
  return input.toUpperCase();
}
```

- ## Name complex conditions

```typescript
// DON'T
if (list && list[0] === "foo" && MYLIST.includes("bar"))
// DO
const isMyListValid = list && list[0] === "foo" && MYLIST.includes("bar");
if (isMyListValid) ...
```

- ## Define types for valid states

```typescript
// DON'T
interface Message{
  error? : Error,
  content : string
}
const message : Message = ...
if (!message.error) // ...
// DO
interface SuccessfullMessage{
  content : string
}
interface InvalidMessage{
  error : Error,
  content : string
}
function printContent(message : SuccessfullMessage) // ...
function handleError(message : InvalidMessage) // ...
```

- ## Use records to map values

```typescript
// DON'T
if (country === 'Sweden') return 'SE';
if (country === 'Norway') return 'NO';
if (country === 'Finland') return 'FL';
// DON'T
switch (country) {
  case 'Sweden':
    id = 'SE';
    break;
  default:
    break;
}
// DO
const countries: Readonly<Record<string, string>> = {
  Sweden: 'SE',
  Norway: 'NO',
  Finland: 'FL',
};
const country = 'Sweden';
const id = countries[country];
```

- ## Let the compiler infer types

```typescript
// DON'T
const isValid: boolean = true;
// DO
const isValid = true;
```

- ## Use JSdocs to add necessary documentation on functions, constants and types. Use over comments when possible too.

```typescript
// DON'T
// When the parameter is valid we can do something with it
const isValid = true;

// DO
/** When the parameter is valid we can do something with it */
const isValid: boolean = true;
```

<br></br>

# Unit tests

- ## Make data driven tests. Include edge cases in the inputs _(0, null, undefined, extremes of a range, maximum value)_

```typescript
// KARMA
[1, 2, 5, 11, 111.3141, 50105].forEach((input) =>
  it('should ... ', () => {
    const sut = isNumberOdd(input);
    expect(sut).toBe(true);
  }),
);

// JEST
  it.each([1, 2, 5, 11, 111.3141, 50105])('should ... ', (input) => {
    const sut = isNumberOdd(input);
    expect(sut).toBe(true);
  });
```

- ## Divide tests in 3 blocks whenever possible : Arrange, Act, Assert (AAA).

```typescript
    it('should ... ', () => {
      // Arrange
      const mock = ...
      // Act
      const sut = isNumberOdd(input);
      // Assert
      expect(sut).toBe(true);
    }),
```

> NOTE: Don't write the comments in the example, this is just for clarification
