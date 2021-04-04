---
title: UnitTestingForBeginner
date: 2021-04-04 22:05:08
tags:
- Csharp
- UnitTesting
- Mosh
---
# Type of Tests
- Unit
- Integration
- End-to-End

## Unit test
Tests a unit of an application without its **external** dependencies

- Cheap to write
- Execute fast
- Don't give a lot of confidence

## Integration Test
Tests the application with its **external** dependencies

- Take longer to execute
- Give more confidence 

## End to End Test
Drives an application through its UI

- Give you the greatest confidence
- very slow
- very brittle

# Testing Frameworks
- NUnit
- MSTest `(embed in VisualStudio)`
- xUnit

All of the Testing Frameworks have:
- Library 
- Test runner 

Good pracitce for naming the TestingMetod:
**FunctionName_Condition_ExpectReturn**


Inside every **UnitTest** with triple A
1. Arrange
2. Act
3. Assert


# Test-driven Development (TDD
1. write a failing test
2. Write the simplest code to make the test pass.
3. Refactor if necessary.

## Benefits of TDD
- Testable Source Code
- Full Coverage by Tests
- Simpler Implementation