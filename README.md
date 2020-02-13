# unit-testing-AtoZ

> A collection of short and straightforward tips for better unit tests

Much of it is fairly general, but reflects my focus on web frontend applications.

## A

🏜️

## B

### Brittle

A test that is easily broken when it doesn't need to - by a change that doesn't break the code under test.

Often caused by asserting on more than what the test really cares about, which can be fixed by reducing their scope and/or using a less-strict matcher:

```javascript
expect(invoice).toEqual({ id: 123, items: 10, amountPayable: 123, ... });
expect(invoice).toMatchObject({ items: 10, amountPayable: 123 });

expect(classes).toBe('btn btn-primary btn-disabled');
expect(classes).toContain('btn-disabled');
```

## C

🏜️

## D

🏜️

### `describe()` block

Allows grouping together of related tests.

```javascript
describe('add', () => {
  test('two positive numbers', () => {});
  test('zero and zero', () => {});
});
```

Helps keep the test file organised. Allows code folding in IDEs to be used to gain an overview of the tests. Produces neat and easily readable test run output. Avoids the need for repetitive test names. Allows running of only the related tests using the `describe.only()` modifier.

May be nested, but it's better not to go too deep. Optional; use only when they help. Not necessary to wrap the entire test file in one when using a modern test runner.

## E

🏜️

## F

### File

Contains tests. A pattern that works well is to put it alongside the tested file, and give it the same name with a well-known suffix to mark it as a test.

```
folder/
├── calculator.js
├── calculator.spec.js
```

Reminds and encourages us to update the tests when we change the code. Makes it easy to find the tests for a given file, or highlights their absence. Allows filtering in/out within IDE search and build tools. Promotes testing as an integral part of development, and tests as equal-class citizens of the codebase.

There are many approaches, but top-level separation into `src` and `test` folders with mirrored subfolders should be avoided. Use a consistent approach, possibly tightening the test runner's default configuration to enforce that.

## G

🏜️

## H

🏜️

## I

🏜️

## J

🏜️

## K

🏜️

## L

🏜️

## M

🏜️

## N

🏜️

## O

🏜️

## P

🏜️

## Q

🏜️

## R

🏜️

## S

🏜️

## T

🏜️

## U

🏜️

## V

🏜️

## W

🏜️

## X

🏜️

## Y

🏜️

## Z

🏜️
