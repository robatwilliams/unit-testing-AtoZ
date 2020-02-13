# unit-testing-AtoZ

> A collection of short and straightforward tips for better unit tests

Much of it is fairly general, but reflects my focus on web frontend applications.

## D

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
