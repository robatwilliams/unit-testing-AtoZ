# unit-testing-AtoZ

> A collection of short and straightforward tips for better unit tests

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
