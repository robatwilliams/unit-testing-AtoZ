# unit-testing-AtoZ

> A collection of short and practical tips for better unit testing

Much of it is fairly general, but reflects my focus on web frontend applications.

## A

### Abstraction

ð§

### Arrange, act, assert

The three phases of a test: create/prepare the participants, act on some of them, then assert your expectations.

Readability is improved by having clear separation between them, but some or all of them can be combined in simpler tests:

```javascript
// Imagine that this test is more complex
test('add', () => {
  const calculator = new Calculator();

  const result = calculator.add(1, 2);

  expect(result).toBe(3);
});

test('isPrime', () => {
  expect(isPrime(3)).toBe(true);
});
```

### Asynchronous

🚧

## B

### Brittle

A test that is easily broken when it doesn't need to - by a change that doesn't break the code under test.

Often caused by asserting on more than what the test really cares about, such as too-wide a scope, or incidental behaviour. Fixable by refining/reducing the scope of assertions, and/or using a less-strict matcher:

```javascript
expect(invoice).toEqual({ id: 123, items: 10, amountPayable: 123, etc... });
expect(invoice).toMatchObject({ items: 10, amountPayable: 123 });

expect(classes).toBe('btn btn-primary btn-disabled');
expect(classes).toContain('btn-disabled');
```

➡ See: [loose](#loose)

## C

### Cleanup

Restoring the test environment and participants to the state they were in before the test began.

This helps ensure that subsequently run tests aren't affected by anything the current test did. Practically this means restoring any shared/static participants (if they must be used at all), restoring implementations that were mocked, and clearing out pending asynchronous work.

Test frameworks can be configured to do some of the cleanup automatically after each test:

```jsonc
// jest.config.json
{
  "restoreMocks": true,
  "setupFilesAfterEnv": ["./jest.setup.js"]
}
```

```javascript
// jest.setup.js
afterEach(() => {
  jest.clearAllTimers();

  // It's not so easy for Promises: https://github.com/facebook/jest/issues/2157
});
```

➡ See: [isolation](#isolation)

### Code completion

IDE/editor feature that provides context-aware code completion with documentation.

Usual completion benefits apply when this feature is aware of the test framework. Since frameworks are usually used via the globals they provide, some configuration may be needed to make the completion feature aware of these. The framework can also be made aware of any custom assertions in use, which makes them more easily discoverable for developers.

### Code review

Peer review of code to improve quality and identify problems.

Tests are code too, so they need reviewing. Usual benefits apply, including learning about good testing practices. Poor tests have many problems, and can later make it difficult to change or refactor the code under test.

### Coverage

A measure of what is being tested.

Code coverage refers to which lines/paths through the code were executed while running the tests. It's an useful indicator of untested code, but reveals nothing about test quality.

Use case coverage refers to which of the possible scenarios that the code could encounter are covered by tests. Full coverage means that any change to the behaviour of the code will cause a test failure, and indicates good choice of test cases and appropriate assertions. Usually analysed manually, but can be aided by mutation testing.

Although tools can enforce a specified level of coverage, it's quite a nuanced topic that requires judgement. Enforcement by tool may not be the most effective way of improving meaningful test coverage, and it may encourage/force the addition of useless tests.

➡ See: [mutation testing](#mutation-testing)

### Custom assertion

An additional assertion that's added to the test framework's default offering.

Allows clearer and less repetitive asserting within tests. May be self-written, or from an extension library such as [jest-extended](https://github.com/jest-community/jest-extended).

➡ See: [code completion](#code-completion)

## D

### Disable

Marking a test (or a group of tests) to not be run:

```javascript
test.skip('', () => {});
```

A [lint rule](<(https://github.com/jest-community/eslint-plugin-jest/blob/master/docs/rules/no-disabled-tests.md)>) can be used to avoid forgetting to remove it.

## F

### Failure message

The message printed by the test runner when the test fails.

There are many ways of asserting the same expectations, but using the most suitable matcher tends to produce a more informative message:

```javascript
expect(array.length).toBe(3); // expected 3, received 1
expect(array).toHaveLength(3); // expected length 3, received length 1, array: ['apple']

expect(haystack.includes(needle)).toBe(true);
expect(haystack).toContain(needle);
```

🧐 ESLint: [jest/prefer-to-contain](https://github.com/jest-community/eslint-plugin-jest/blob/master/docs/rules/prefer-to-contain.md), [jest/prefer-to-have-length](https://github.com/jest-community/eslint-plugin-jest/blob/master/docs/rules/prefer-to-have-length.md)

### File

Contains tests. A pattern that works well is to put it alongside the tested file, and give it the same name with a well-known suffix to mark it as a test.

```
folder/
├── calculator.js
├── calculator.spec.js
```

Reminds and encourages us to update the tests when we change the code. Makes it easy to find the tests for a given file, or highlights their absence. Allows filtering in/out within IDE search and build tools. Promotes testing as an integral part of development, and tests as equal-class citizens of the codebase.

There are many approaches, but top-level separation into `src` and `test` folders with mirrored subfolders should be avoided. Use a consistent approach, possibly tightening the test runner's default configuration to enforce that.

➡ See: [long file](#long-file)

### Focus

Marking a test (or a group of tests) as the only one to be run:

```javascript
test.only('', () => {});
```

Useful for reducing execution time and test runner output clutter when working on a specific area of functionality. Also for debugging a single test. A [lint rule](<(https://github.com/jest-community/eslint-plugin-jest/blob/master/docs/rules/no-focused-tests.md)>) can be used to avoid forgetting to remove it.

## G

### Grouping block

Allows grouping together of related tests:

```javascript
describe('add', () => {
  test('two positive numbers', () => {});
  test('zero and zero', () => {});
});
```

Helps keep the test file organised. Allows code folding in IDEs to be used to gain an overview of the tests. Produces neat and easily readable test run output. Avoids the need for repetitive test names. Allows running of only the related tests using a focus marker.

May be nested, but it's better not to go too deep. Optional; use only when they help. Not necessary to wrap the entire test file in one when using a modern test runner.

## H

### Hook

🚧

## I

### Ineffective test

A test that doesn't test what its name says it tests - continues to pass when the code is broken.

Validate suspect tests by modifying the code under test to see if it makes the test fail.

### Isolation

🚧

➡ See: [cleanup](#cleanup)

## L

### Linting

Static analysis to detect problems, enforce best practices, and maintain consistency.

Some test-specific adaptations to linter configuration is normally necessary; this can be targeted at only the test files:

```jsonc
// .eslintrc.json
{
  "overrides": [
    {
      "files": ["*.spec.js"],
      "env": {
        "jest": true // Predefined globals for no-undef
      },
      "rules": {
        // For describe() blocks
        "max-lines-per-function": "off",
        "max-statements": "off"
      }
    }
  ]
}
```

Many test frameworks/libraries have companion plugins for ESLint, for example [eslint-plugin-jest](https://github.com/jest-community/eslint-plugin-jest).

### Long file

A file of tests that is so long as to be difficult to comprehend and navigate.

With the approach of a single test file per implementation file, the test file becomes the longer of the pair. IDE features such as code folding become invaluable for navigation. When a file becomes unreasonably long, it may be a sign that the file under test is in need of splitting - rather than the test file.

### Loose

A test that isn't too fussy about what it expects - and may continue passing when a change has broken the code under test.

Often caused by inadequate or imprecise assertions. Fixable by increasing their scope and/or making them stricter:

```javascript
expect(foo).toBeTruthy();
expect(foo).toBe(true);

expect(() => returnBook()).toThrow();
expect(() => returnBook()).toThrow('Unknown book');
```

🧐 ESLint: [jest/no-truthy-falsy](https://github.com/jest-community/eslint-plugin-jest/blob/master/docs/rules/no-truthy-falsy.md), [jest/require-to-throw-message](https://github.com/jest-community/eslint-plugin-jest/blob/master/docs/rules/require-to-throw-message.md)

➡ See: [brittle](#brittle)

## M

### Mutation testing

Automatically modifying the code under test to introduce bugs, then running the tests against it to see if they fail.

This tells us about the effectiveness of the tests at catching problems; the use case coverage.

🔗 [Stryker Mutator](https://stryker-mutator.io/)

➡ See: [coverage](#coverage)

## N

### Name

🚧

## P

### Partial mock data

Mock data that is minimally tailored for the test, missing parts of the real data:

```javascript
expect(isValidTrade({ quantity: -1 }).toBe(false);
```

Not a realistic test of the code. The code under test may produce the expected result for the wrong reason. Typed languages can help avoid this (when used well), especially when it comes to maintaining existing tests as the data model grows.

## R

### Redundant assertion

An assertion whose expectation is already covered by another assertion present:

```javascript
expect(book).not.toBeNull();
expect(book.title).toBe('The Art of Unit Testing');
```

Can simply be removed to reduce noise.

### Runner scripts

Shortcut commands for frequently-used test runner invocations.

For a task runner or package manager. Saves some typing, and shares useful commands among the team:

```jsonc
// package.json
{
  "scripts": {
    "test": "jest",
    "test:changed": "jest --onlyChanged",
    "test:coverage": "jest --coverage",
    "test:debug": "node --inspect-brk ./node_modules/jest/bin/jest.js --runInBand",
    "test:since-master": "jest --changedSince origin/master",
    "test:watch": "jest --watch"
  }
}
```

## T

### Template (snippet)

Frequently-typed piece of code that can be quickly by an editor/IDE.

Saves on repetitive typing and maintains your flow. The following test skeleton can be inserted in ~3 keystrokes, whereas typing it would require 10-20:

<!-- prettier-ignore -->
```javascript
it('', () => {
});
```

It's possible to define [your own](https://code.visualstudio.com/docs/editor/userdefinedsnippets), or install a [prebuilt pack tailored for your test framework](https://marketplace.visualstudio.com/items?itemName=andys8.jest-snippets).

### Time

🚧

## U

### Utility function

🚧

## W

### Watch mode

Test runner feature that automatically re-runs tests when the test or implementation files are changed.
