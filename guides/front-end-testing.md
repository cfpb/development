# Front-End Testing

Extol the [testing pyramid](https://docs.google.com/presentation/d/1NFjgq5SbQ4crNi06SplyqZQ-K9o5RX966AoRVEB38Yg/pub?start=false&loop=false&delayms=3000&slide=id.g18ef6d797_047).

[![Testing pyramid](http://i.imgur.com/1hQdxod.png)](https://docs.google.com/presentation/d/1NFjgq5SbQ4crNi06SplyqZQ-K9o5RX966AoRVEB38Yg/pub?start=false&loop=false&delayms=3000&slide=id.g18ef6d797_047)

## Manual testing & browser support QA

Refer to the [Sauce Labs Guide](browser-testing-with-sauce-labs.md) for instructions on using Sauce Labs to manually test your application in a range of browsers and virtual machines.

## Unit tests

If your code has logic, it should have unit tests. As seen in the pyramid, unit tests should comprise the majority of your tests. Why? Unit tests can be executed quickly, they can be easily automated, they're maintainable and they promote good practices by requiring your code to be written in concise, modular chunks.

A unit test verifies that a single discrete piece of your codebase works as expected. That "piece" could be a JavaScript function, a CommonJS/AMD module or some other unit of functionality. It's vaguely defined but don't let that scare you. Start by choosing one of the test frameworks below and writing tests for individual JavaScript methods. There are many guides about writing [JavaScript](http://alistapart.com/article/writing-testable-javascript) [unit](http://www.smashingmagazine.com/2012/06/27/introduction-to-javascript-unit-testing/) [tests](http://www.htmlgoodies.com/beyond/javascript/testing-javascript-using-the-jasmine-framework.html) that serve as great introductions. At CFPB we like to separate our JavaScript into discrete [npm modules](https://github.com/cfpb/development/blob/main/guides/authoring-npm-modules.md). This makes testing easier because each module includes self-contained tests.

### Test frameworks
- [Jest](https://jestjs.io) is a comprehensive testing framework for Node and browser JavaScript.
- [Jasmine](https://github.com/jasmine/jasmine) shares basic semantics with [Mocha](http://mochajs.org), but also includes lots of nice assertion helpers, object [spies](http://jasmine.github.io/2.2/introduction.html#section-Spies), and async support.
- [Tape](https://www.npmjs.com/package/tape) is a [TAP](http://en.wikipedia.org/wiki/Test_Anything_Protocol)-producing test harness for Node with great asynchronous support. Check out the [Testling tape guide](https://ci.testling.com/guide/tape).

Check out 18f's excellent [testing cookbook](https://pages.18f.gov/testing-cookbook/) for more best practices.

## Code coverage

The goal is to have all of your code covered by unit tests. Tools known as code coverage reporters will tell you what percentage of your codebase is covered by tests. The consumerfinance.gov (consumerfinance.gov) project uses [Jest](https://github.com/facebook/jest/). When consumerfinance.gov unit tests are run using `gulp test:unit`, a coverage report with coverages percentages is printed to the command line. It also generates an interactive HTML code coverage report in `test/unit_test_coverage`.

![lcov report](http://i.imgur.com/oJYfZCN.png)

## Integration tests

Integration tests test the points at which your front-end JavaScript communicates with your project's back-end. These tests run slower than unit tests because they interact with an external environment. When building single page applications, integration tests usually come in the form of API tests.

## Browser tests

When building browser-based applications, systems tests are browser (otherwise known as GUI) tests. They verify that the front-end code operates as expected in environments similar to the end-users'. They are slow and expensive to write and maintain. They break easily when the GUI is modified. But they are an incredibly important method of ensuring your application is operating correctly.

They are often written in languages that can be understood by non-developers, traditionally business analysts. The consumerfinance.gov (consumerfinance.gov) project uses [Protractor](https://www.protractortest.org) and [Cucumber](https://github.com/cucumber/cucumber-js) to execute [feature files](https://github.com/cfpb/consumerfinance.gov/tree/main/test/browser_tests/cucumber/features/suites/default) that verify pages are working as expected.
