
Disclaimer: Please note that this page is under intensive "draft" mode, so it is being actively updated.

If you want to add anything or make corrections, please see instructions [contributing](CONTRIBUTING.md). You can also join our [Discord discussions](https://discord.gg/9b4dWYdHqS).

# History

TODO: Add the history of TDD, and the key people involved in development

# Different types of test

## Unit tests

- Unit tests help to refactor,
- Unit tests remove *fear of change*
- Unit tests protect us to break something

TODO: Add Fowler's article where he remarks that there were so many different definitions of unit test evne when XP began, and the classification sociable vs solitary unit test. Explain difference interaction-based vs state-based vs output-based verifification.

## Integration tests

TODO: Definition of integration test - different variants. For example, with sociable unit test, tests spanning multiple classes are NOT integration tests per se, they are still unit tests.... But with solitary unit test, tests spanning multiple classes are integration tests... Do intgeration tests include or not include business logic? (the two interpretations)

## Acceptance tests

TODO: Interpretation of acceptance tests from e2e perspective (that's what many people see this as) - testing from the end user.... vs unit tests being acceptance tests (quoting Ian Cooper's video TDD Revisited)

## End-to-end tests

TODO: Problems of definition of e2e tests, spanning vs not spanning third-party systems

# Test driven development

## TDD rules

* You're not allowed to write any production code until you've got a unit test failing first

> Warning: **not compiling is failing !!!**

If a test fails to compile, stop writing the test and write production code to make the test compile.

## TDD loop

### Red - Failing test

Write a failing test, well [named](#tests-name), that test a behavior, **not a method**.

We should test behaviors not classes !

TODO: Here wording needs to be clarified. Our unit tests are calling classes/methods inside the test itself, but the emphasis is on behavior... Also the difference of interpretatation of behavior as outcomes (testing behavior through state), versus interpretation of behavior as interactions.

### Green - Make it pass ! Fast !

Make it pass with the minimum required code, this step should be fast, it *must* take less than 1 minute,

if not, it means your step is too big ! [see baby steps for more](#baby-steps)

### Blue - Refactor !

This is a step that is crucial and most of the time skipped by developers.

In this step, you'll can remove duplicate, introduce some designs patterns, improve code readability.

> **Never** forget to run tests after !!!   
> This will validate your modifications !

TODO: We also need to point out difference between localized vs more global refactoring... and avoiding premature design patterns.

## How to choose tests

Tests must be chosen well in order to accomplish the next minimum line of code you want to add.

Like a chess player, each step you take must be calculated
and planned in order to complete the whole algorithm step by step.

This means, you don't write a random unit test that won't help you complete the algorithm,
but a precise test, which targets a precise addition.

## How much tests is enough

As many tests as necessary to validate business behavior.

This means that a method for operating a pacemaker will contain many more tests than a method for dropping candy from a
candy machine.

## One assertion per test

> The single assert rule means,     
> you should have a single logical assertion.   
> However, you can have multiple assert statements
> that can be composed.     
> -- <cite>Uncle bob</cite>

The Single Assert Rule maintains that a single logical assertion should follow a single logical action.

## Tests name

Uncle bob propose this:

> We should name our test functions for the action and assertion

```java
public void addMoneyUpdateCustomerBalance(){ // -- }
public void computeXAndYReturnZZ(){ // -- }
public void startSystemShouldEnableFeaturesXY(){ // -- }
```

TODO: Need to list the different variants of naming, for example another convention is with `should`

## Tests structure patterns

### AAA

* Arrange
* Act
* Assert

### GWT

* Given
* When
* Then

## Better assertions / Custom assertions

Custom assertions are pretty easy to create, no need for external dependency:

Example:

```java
class Test {

  @Test
  public void shouldInvertName() {
    assertEquals("First", invert("First"));
    assertEquals("Last, First", invert("First Last"));
    assertEquals("Last, First", invert("   First    Last   "));
  }

  public String invert(String fullname) {
    // --
  }
}
```

This can be replaced by

```java
class Test {

  private void assertInverted(final String input, final String expected) {
    assertEquals(expected, invert(input));
  }

  @Test
  public void shouldInvertName() {
    assertInverted("First", "First");
    assertInverted("First Last", "Last, First");
    assertInverted("   First    Last   ", "Last, First");
  }

  public String invert(String fullname) {
    // --
  }
}
```

TODO: Describing setup helpers and assertion helpers

## Test FIRST

### F for *Fast*

This allows you to get **quick feedback.**

If your tests are slow, you are less likely to run them often.

### I for *Isolated*

A test must not depend on another one.

You can run test in aleatory order and this will always pass.

### R for *Repeatable*

A test can be repeated indefinitely.

If a unit test is network, database, or file dependent, it is more likely to fails.

### S for *Self-validating*

Test should contain one assertion validating the result of the function tested.
See [One assertion per test](#one-assertion-per-test)

### T for *Thorough or Timely*

#### Thoroughness

When we test a function, we need to test for happy and unhappy paths.

##### Timely

Unit tests should be written just before the production code that makes the test pass.

## Tests are SOLID

TODO: Let's review this section, what's the source of this?

#### S for *Single responsability principle*

Each test function, and each test class, should have one,

and only one responsibility.

which is the responsibility of the class it is testing.

##### O for *Open/Closed principle*

The rule for tests is that, the production code should be open for extension, but the tests should be closed for
modification.

> We should be able to change the production code without changing the tests.

#### L for *Liskov substitution principle*

Since we use polymorphism in our tests, we comply with the Liskov substitution principle.

#### I for *Interface segregation principle*

If tests use an interface that contains methods that the tests don’t call, then the tests have too much knowledge.

##### D for *Dependency inversion principle*:

Tests are low level detail, they are clients of the production code

Test code should depend on production code.

Production code should not depend on test code.

## Fragile tests

## Agnostic tests

## Test-code importance

Test code is as important as production code.

Test code may be more important than production code since you can recreate the production code from the tests, but you
can't recreate the tests from the production cdoe.

## Assert first

## Baby steps

## Mocking

## Transformation priority premise

* ({}–>nil) no code at all->code that employs nil
* (nil->constant)
* (constant->constant+) a simple constant to a more complex constant
* (constant->scalar) replacing a constant with a variable or an argument
* (statement->statements) adding more unconditional statements.
* (unconditional->if) splitting the execution path
* (scalar->array)
* (array->container)
* (statement->recursion)
* (if->while)
* (expression->function) replacing an expression with a function or algorithm
* (variable->assignment) replacing the value of a variable.

## Getting stuck

When following TDD, getting stuck is a symptom of making the production code too specific.

As the tests get more specific, the code gets more generic.
