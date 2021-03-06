# Unit testing A-Z

> A collection of short and practical tips for better unit testing

Much of it is fairly general, but reflects my focus on web frontend applications.

## A

### Abstraction

The abstraction away of commonality from similar tests, usually setup code and mock data.

Makes it clear what the differences are between tests. Follows the DRY principle; less repetition, fewer places to change when something changes. Too much abstraction however can make tests difficult to understand, and make it difficult to tailor setup and mock data to suit individual tests.

➡ See: [hook](#hook)

➡ See: [utility function](#utility-function)

### Arrange, act, assert

The three phases of a test: create/prepare the participants, act on some of them, then assert your expectations.

Readability is improved by having clear separation (e.g. a blank line) between them, but some or all of them can be combined in very simple tests.

### Asynchronous

Asynchronous code is trickier to test correctly, so care is needed.

Make sure that assertions are actually run, and that they run at the right moment. Use the test framework's utility functions to flush pending async tasks, and to assert that assertions were run.

➡ See: [cleanup](#cleanup)

## B

### Brittle

A test that is easily broken when it doesn't need to - by a change that doesn't break the code under test.

To avoid, assert on only what the test really cares about. Refine the scope of assertions, ignore incidental behaviour, and/or use less-strict matchers:

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

This ensures that subsequently run tests aren't affected by anything the current test did. Practically this means restoring any shared/static participants (if they must be used at all), restoring implementations that were mocked, and clearing out pending asynchronous work.

Test frameworks can be configured to do some of this automatically after each test:

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

Usual completion benefits apply when this feature is aware of the test framework. Since frameworks are usually used via the globals they provide, some configuration may be needed to make the completion feature aware of these. It can also be made aware of any custom assertions, making them more easily discoverable for developers.

### Code review

Peer review of code to improve quality and identify problems.

Tests are code too, so they need reviewing. Usual benefits apply, including learning about good testing practices. Poor tests have many problems, and can later make it difficult to change or refactor the code under test.

### Coverage

A measure of what is being tested.

Code coverage refers to which lines/paths through the code were executed while running the tests. It's an useful indicator of untested code, but reveals nothing about test quality.

Use case coverage refers to which of the possible scenarios that the code could encounter are covered by tests. Full coverage means that any change to the behaviour of the code will cause a test failure, indicating that the tests are effective at catching bugs. Usually analysed manually, but can be aided by mutation testing.

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

## E

## F

### Failure message

The message printed by the test runner when the test fails.

There are many ways of asserting the same expectations, but using the most suitable matcher tends to produce a more informative message:

```javascript
expect(array.length).toBe(3); // expected 3, received 1
expect(array).toHaveLength(3); // expected length 3, received length 1, array: ['apple']

expect(haystack.includes(needle)).toBe(true); // expected true, received false
expect(haystack).toContain(needle); // expected value <needle>, received array <haystack>
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

Means for a function to be registered with the test runner to be run at a specific point in the test execution lifecycle.

```javascript
let participant;

beforeEach(() => {
  // e.g. do some common setup before each test
  participant = ...;
});

afterEach(() => {
  // e.g. cleanup so as not to affect the next test
});
```

They are often used to abstract common setup between tests. However, they have many problems and encourage poor practices:

- Encourages the use of shared state, compromising isolation
- No way of tailoring setup for individual tests, so the "common" setup becomes bloated with test-specifics - a poor abstraction
- Makes it difficult to modify, move, refactor, and add tests
- Especially problematic with nested tests in grouping blocks, with hooks at each level

Prefer to use utility functions instead, and use a [lint rule](https://github.com/jest-community/eslint-plugin-jest/blob/master/docs/rules/no-hooks.md) to discourage using hooks.

➡ See: [abstraction](#abstraction)

➡ See: [isolation](#isolation)

➡ See: [utility function](#utility-function)

## I

### Ineffective test

A test that doesn't test what its name says it tests - continues to pass when the code is broken.

Validate suspect tests by modifying the code under test to see if it makes the test fail.

### Isolation

The quality of being independent of and unaffected by other tests.

Runtime isolation means tests don't affect each other at runtime - they can be run together, alone, or in any order, and the result will be the same. To achieve that, avoid shared state/variables, and clean up anything whose sharing cannot be avoided. Deep-clone mock data before using it.

Subject isolation means a test only tests the code under test - changes to other code won't break it. To achieve that, mock things that the code under test would interact with.

Test code isolation means a test can be changed or moved without dragging up roots that ties it to other tests. To achieve that, avoid excessive abstraction of test setup.

➡ See: [abstraction](#abstraction)

➡ See: [cleanup](#cleanup)

## J

## K

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

Many test frameworks/libraries have companion plugins for ESLint, for example [eslint-plugin-jest](https://github.com/jest-community/eslint-plugin-jest). Some of the items covered in this collection can be enforced by such rules.

### Long file

A file of tests that is so long as to be difficult to comprehend and navigate.

With the approach of a single test file per implementation file, the test file becomes the longer of the pair. IDE features such as code folding become invaluable for navigation. When a file becomes unreasonably long, it may be a sign that the file under test is in need of splitting - rather than the test file.

### Loose

A test that isn't too fussy about what it expects - and may continue passing when a change has broken the code under test.

To avoid, make adequate and precise/strict assertions:

```javascript
expect(foo).toBeTruthy();
expect(foo).toBe(true);

expect(() => returnBook()).toThrow();
expect(() => returnBook()).toThrow('Unknown book');
```

➡ See: [brittle](#brittle)

## M

### Mutation testing

Automatically modifying the code under test to introduce bugs, then running the tests against it to see if they fail.

This tells us about the effectiveness of the tests at catching problems; the use case coverage.

🔗 [Stryker Mutator](https://stryker-mutator.io/)

➡ See: [coverage](#coverage)

## N

### Name

Describes what is being tested.

Names should be clear, concise, and unambiguous. Good naming encourages other good testing practices. Helps spot missing tests. Adds documentation and provides context (possibly replacing a code comment). Aids readability and understanding of test runner output.

There are numerous styles of test naming, and test frameworks offer a few test definition functions such as `test()` and `it()` to support these. The former is flexible, while the latter is for names of the form "it( _expectation_ when _scenario/input_". Use a consistent approach (a [lint rule](https://github.com/jest-community/eslint-plugin-jest/blob/master/docs/rules/consistent-test-it.md) can enforce this), and write test names that follow that approach.

When the _why_ of the tested behaviour isn't obvious, consider extending the test name to include it: it - when - because. Try to avoid ambiguous words/phrases such as "correctly" or "as it should", and unnecessary words such as "should".

➡ See: [grouping block](#grouping-block)

### Negative testing

Testing that the code doesn't do something, or doesn't do anything other than what it should.

The former is for known unwanted behaviours, and so can be specifically asserted:

```javascript
expect(showErrors).not.toHaveBeenCalled();
expect(submit).toHaveBeenCalledTimes(1);
expect(order).not.toHaveProperty('expiry');
```

The latter is for unknown unwanted behaviours, and so requires a strict assertion on the complete area of interest:

```javascript
expect(order).toEqual({ price: 1, quantity: 2 });
```

➡ See: [brittle](#brittle)

## O

## P

### Partial mock data

Mock data that is minimally tailored for the test, missing parts of the real data:

```javascript
expect(isValidTrade({ quantity: -1 }).toBe(false);
```

Not a realistic test of the code. The code under test may produce the expected result for the wrong reason. Typed languages can help avoid this (when used well), especially when it comes to maintaining existing tests as the data model grows.

## Q

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

## S

## T

### Template (snippet)

Frequently-typed piece of code that can be quickly inserted by an editor/IDE.

Saves on repetitive typing and maintains your flow. The following test skeleton can be inserted in ~3 keystrokes, whereas typing it would require 10-20:

<!-- prettier-ignore -->
```javascript
it('', () => {
});
```

Most IDEs/editors allow defining [your own](https://code.visualstudio.com/docs/editor/userdefinedsnippets), or installing a [prebuilt pack tailored for your test framework](https://marketplace.visualstudio.com/items?itemName=andys8.jest-snippets).

### Time: clock

An external, continuously-varying factor that makes every test run different.

For consistent tests all-year in all time zones, inject a fixed time/date into the code under test, or mock the runtime's current time and date.

### Time: timers

Timers set up by code to run some code after a specified duration.

To avoid long-running tests due to waiting in actual-time for that duration to pass, use the test framework's time-mocking capability.

### Type safety

Static type checking of the code, providing lower-effort problem detection and faster feedback than unit tests.

When the code that uses the code under test is type-checked against the latter, unit tests for problems caught by the type system are unnecessary.

## U

### Utility function

A function which abstracts away some common work away from individual tests.

Typical uses are setup logic, or to return mock data. The function can be parameterised to allow tailoring the setup/data for individual tests, this also aids test comprehension by making clear the differences between tests. Multiple participants can be returned on an object for convenient destructuring.

The preferred alternative to hooks.

```javascript
test('order with zero quantity is not valid', () => {
  const { validator, account } = setup();
  const order = createOrder({ quantity: 0 });

  // ...
});

function setup() {
  // ...

  return {
    validator,
    account: defaultAccount,
  };
}

function createOrder({ quantity = 100 } = {}) {
  return {
    symbol: 'EURGBP',
    quantity,
    // ...
  };
}
```

➡ See: [abstraction](#abstraction)

➡ See: [hook](#hook)

## V

## W

### Watch mode

Test runner feature that automatically re-runs tests when the test or implementation files are changed.

## X

## Y

## Z
