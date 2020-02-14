# unit-testing-AtoZ

> A collection of short and straightforward tips for better unit tests

Much of it is fairly general, but reflects my focus on web frontend applications.

## A

### Arrange, act, assert

The three phases of a test: prepare the participants, act on some of them, then assert your expectations.

Readability is improved when these have some clear separation, but for simpler tests it can make sense to combine some or all of them:

```javascript
test('add', () => {
  const calculator = new Calculator();

  const result = calculator.add(1, 2);

  expect(result).toBe(3);
});

test('isPrime', () => {
  expect(isPrime(3)).toBe(true);
});
```

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

### Code review

Tests are code, too. They need reviewing just like the code under test.

The usual benefits of code review apply, including increased quality, knowledge sharing, and learning about good testing practices. It also results in a better review of the code under test, through strengthening the understanding of it.

Poor tests have many problems, and can later make it difficult to change or refactor the code under test.

### Coverage

A measure of "how much" and "what" is being tested.

Usually refers to "code coverage", meaning which lines/paths through the code were executed while executing the tests. It's a useful indicator of untested code, but reveals nothing about good test cases and appropriate assertions.

The other, stronger, type of coverage is use case coverage. This measures which of the possible scenarios that the code could encounter are covered by tests. Full coverage means that any change to the behaviour of the code will cause at least one test to fail. Usually analysed manually, but mutation testing can also help.

Although tools can enforce a specified level of coverage, it is quite a nuanced topic that requires judgement. Enforcement by tool may not be the most effective way of improving meaningful test coverage, and it may encourage/force the addition of useless tests.

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

Helps keep the test file organised. Allows code folding in IDEs to be used to gain an overview of the tests. Produces neat and easily readable test run output. Avoids the need for repetitive test names. Allows running of only the related tests using a focus marker.

May be nested, but it's better not to go too deep. Optional; use only when they help. Not necessary to wrap the entire test file in one when using a modern test runner.

## E

🏜️

## F

### Failure message

The message printed by the test runner when the test fails.

There are many ways of asserting the same expectations, but using the most suitable matcher tends to produce a more informative message:

```javascript
expect(array.length).toBe(3); // expected 3, received 1
expect(array).toHaveLength(3); // expected length 3, received length 1, array: ['apple']
```

### File

Contains tests. A pattern that works well is to put it alongside the tested file, and give it the same name with a well-known suffix to mark it as a test.

```
folder/
├── calculator.js
├── calculator.spec.js
```

Reminds and encourages us to update the tests when we change the code. Makes it easy to find the tests for a given file, or highlights their absence. Allows filtering in/out within IDE search and build tools. Promotes testing as an integral part of development, and tests as equal-class citizens of the codebase.

There are many approaches, but top-level separation into `src` and `test` folders with mirrored subfolders should be avoided. Use a consistent approach, possibly tightening the test runner's default configuration to enforce that.

### Focus

Marking a test (or a group of tests) as the only ones to be run.

Useful for reducing execution time and test runner output clutter when working on a specific area of functionality. Necessary for debugging a single test.

A lint rule can be used to avoid forgetting to remove a focus marker.

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

### Loose

A test that isn't too fussy about what it expects, and may continue passing even when a change has broken the code under test.

Caused by inadequate or imprecise assertions, which can be fixed by increasing their scope and/or using a stricter matcher:

```javascript
expect(foo).toBeTruthy();
expect(foo).toBe(true);

expect(() => returnBook()).toThrow();
expect(() => returnBook()).toThrow('Unknown book');
```

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

### Skip

Marking a test (or a group of tests) to not be run.

A lint rule can be used to avoid forgetting to remove a skip marker. When a test needs to be checked in despite being skipped, a lint-ignore comment can be used - preferably preceded with a comment explaining why the test is skipped.

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
