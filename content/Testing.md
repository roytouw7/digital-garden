---
title: Testing
draft: true
tags:
---
[[Software Architecture]]

## Cautions
- Unit tests should limit it's scope of assertions to the code they are testing to prevent rigidity/brittle tests, they should mock all dependencies.
## Levels of Tests
### Unit Test
- **Scope:** Unit tests focus on the smallest testable parts of an application, typically individual functions or methods. These tests are highly focused and isolated, meaning all dependencies of the unit under test are mocked or stubbed.
- **Purpose:** The main goal is to ensure that the specific unit of code works as expected in isolation. This includes verifying both the logic and the state of the unit.
 - **Mocking:** Dependencies are mocked(or stubbed or faked) to prevent the test from being affected by changes in other parts of the codebase or external systems. This isolation ensures that the tests don't become brittle during refactoring and only fail if there is an issue within the unit itself.
- **Assertions:** Assertions are typically made on the internal state and outputs of the unit, depending on the specific behavior being tested.
### Component Test
- **Scope:** Component tests evaluate the collaboration between several units of code that work together as a component or module. These tests ensure that the integrated units within a component interact correctly.
- **Purpose:** The goal is to verify that the combined functionality of related units works as expected. This level of testing focuses on the behavior of a larger piece of the application without testing the entire system.
- **Mocking:** Typically, only the external dependencies at the “edges” (like HTTP requests, databases, or external services) are mocked. Internal interactions between the units within the component are tested without mocking.
- **Assertions:** Assertions are made on the output and the behavior of the component as a whole. 
### Fuzz Test
- **Scope**: Fuzz tests focus on either a similar level as unit or component tests, but with more generated input; as long as the test is reasonably fast to run due to repetition
- **Purpose**: Finding unexpected behavior in edge cases caused by repeatedly repeating tests with newly thoughtfully generated input. Especially powerful for detecting security vulnerabilities and robustness issues.
- **Mocking**: outer systems should be mocked due to runtime impact
- **Assertions**: are made on the output and the behavior of the unit
- **Cons**: Fuzz tests are slow by nature due to repetition
### Contract Testing
[[Contract Testing|dedicated not for contract testing]]
- **Scope**: Contract tests systems from a consuming system's perspective, assuring new changed don't break the contract with the external consumer, shines in distributed systems architectures.
- **Purpose**: Detect breaking changes downstream before deployment, especially powerful in distributed systems
- **Mocking**: outer systems are mocked as we try to prevent breaking changes down stream, and changes in outer systems are outside our control and covered by [[#End-to-end Test]]
- **Level** contract testing isn't defined as for example a unit test which tests a single unit, or a component test testing the whole component including dependencies. You can write contract tests at varying levels. Usually the sweet spot for a contract test is on par of that of a [[#Component Test]]. This gives a good level of trust in the contract, but requires much less effort on infrastructure than on an integration test level.

> [!QUESTION] Why add a contract test when component tests should/could have covered this?
> Because a contract test can add assertions for a real use case making the test more specific, and because a test simply being a contract test it hints at a consumer use case failing when it breaks.

> [!QUESTION]
> Do you really need a tool for contract testing? Can't you simply write a component tests which makes an assert on the response for a given input request simulating a given scenario?
> 
> On what level do you require contract tests when you write them like that?

> [!TIP]
> Contract testing is intended to lossen deployment of services?

> [!TODO]
> checkout [go pact](https://github.com/pact-foundation/pact-go?tab=readme-ov-file)
### Integration Test
- **Scope:** Integration tests focus on testing how different parts of the system interact with each other. This includes testing the interaction between multiple components, services, or even different systems.
- **Purpose:** The goal is to identify issues in the way that different parts of the system integrate and work together. Integration tests are crucial for ensuring that data and control flow correctly between components or services.
- **Mocking:** Similar to component tests, integration tests may mock external systems or services, but this should be the exception.
- **Assertions:** Assertions are made on the output of the integrated components or systems, ensuring that they work together as expected.
### End-to-end Test
- **Scope:** E2E tests examine the entire application workflow, testing the complete system from the user’s perspective. These tests simulate real user interactions and test the system in an environment that closely resembles production.
- **Purpose:** The main objective is to verify that the entire application works together as expected, from the frontend to the backend, including databases, external services, and network interactions.
- **Mocking:** E2E tests generally avoid mocking. The idea is to test the system as a whole, interacting with real databases, services, and other components to validate the end-to-end functionality. Mocking is limited to systems outside our control.
- **Assertions:** Assertions are made on the final output of the system, often focusing on the user-facing results (e.g., UI changes, API responses).
- **Cons**: e2e tests are generally harder to write, relatively very slow, and run after deployments

### Architecture Test
- **Scope**:
- **Purpose**: automated way of enforcing architectural rules on a system.
[^1]

> [!TODO]
> smoke tests?

> [!TODO]
> Benchmarks?

> [!TODO]
> Stress testing?

> [!TODO]
> Chaos Engineering

## Test Doubles
![Pasted image 20240916102915.png](app://399ec3cd8b68d3a0e0182acc0fc60c471d94/Users/roytouw/Documents/Obsidian/Attachements/Pasted%20image%2020240916102915.png?1726475355882)
### Dummy

### Stub

### Spy

### Mock

### Fake
## Testing Pyramid
![Pasted image 20250223205015.png](app://399ec3cd8b68d3a0e0182acc0fc60c471d94/Users/roytouw/Documents/Obsidian/Attachements/Pasted%20image%2020250223205015.png?1740340215472)
Gives the ratio of tests for a given system. Most unit tests as they are cheap to write and run. This also is a reason for them to be isolated, otherwise they become brittle and refactoring a unit will break the tests of another unit.

There are several dimensions present in the testing pyramid:
1. **cost** rises going up the pyramid
2. **confidence** rises going up the pyramid
3. **coverage** rises going up the pyramid
4. **implementation knowledge** lowers going up the pyramid
5. **feedback time** lowers going up the pyramid


## Testing Quadrant

![Pasted image 20250222145116.png](app://399ec3cd8b68d3a0e0182acc0fc60c471d94/Users/roytouw/Documents/Obsidian/Attachements/Pasted%20image%2020250222145116.png?1740232276990)
## Literature
### Deep Search on Benefits and Downsides of Automated Testing
Using Open AI Deep Search I asked a report about the actual benefits and downsides of automated testing according to scientific literature. The consensus of several papers was that automated tests result in higher quality software, quicker deployments, and saved time in the long run. [^2]

### Alternatives to Testing Pyramid Distributions
> [!TODO]
> There are some people debating alternative test distributions than the testing pyramid, but this is much less popular, but might good to check out sometime.
  > [testing trophy](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications)
  > 
  > [testing dorrito](https://kentcdodds.com/blog/write-tests)

[^1]: https://www.milanjovanovic.tech/blog/enforcing-software-architecture-with-architecture-tests

[^2]: https://chatgpt.com/share/67c2fbe2-ebb8-8005-a215-7f2f321fed37
