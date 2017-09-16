# 17 - Design Benefits of TDD I
## How can TDD influence the design of an application?
It is hard to take arbitrary code and test it.  Well designed code is easier to test, will require fewer tests, and will fail less.


Much code in the industry is 10000+ lines.  This means there is a huge number of permutations and combinations possible making testing impossible.
Long methods are filled with design flaws and hard to maintain.  If we start by writing test cases, the need to make methods modular becomes obvious.  This is similar for dependencies.  It is hard to write tests for code with many dependencies.  This leads to low coupling.


undefined"Unit testing is an act of design more than an act of verification"


The quality of the test is important too.  If the test is poor it will not influence a better design.


## Types of Unit Tests


**Positive tests** - checks if the code is doing what it is supposed to do.


**Negative tests** - When the preconditions are not being met, or when the surroundings situations are not what you expect, or the state of the object is not in a valid state to receive a particular call...What then?  


**Exception tests** - make sure the code is throwing the right kind of exceptions.


**Performance tests** - test the performance of a certain feature.  Good performance is tied to good feedback.  If there is no way to get immediate feedback about performance, changes can have dramatic effect.  If performance is important, write a test for it.  There are tools to help with this, like JUnitPerf in Java.  "As important as any other kind of test".


A unit test is a test on a unit of code...So what is a Unit of code?


**Unit of Code**:  The smallest piece of code that does useful work.  


NOTE:  don't write tests for getters and setters.  Generally setters and getters are not doing "useful" work.  The getters and setters will come into the picture while writing tests for other functions.  Instead focus on testing BEHAVIOR rather than STATE.


A unit is NOT a class, a unit is often a method or small function.


How to write classes?  Dont create a class with implementation in mind.  Instead start with the tests.  Ask "how does TDD change this?"


The first few test cases will appear trivial and will return "fake" results from the methods.   The reason for this is to grow the interface of the method first, and then work on the internals of the function.  The first few tests are meant to help think about the usage of the function, the later tests help define the internal structure or implementation of the function.


Write the test first before writing any code.  The first test is a failing test, then the bare minimum of code is written to pass the test.


## Benefits of Test first coding
 - **reverts **the way we think of code.  Rather than rushing to the implementation, the code is grown in a minimal, practical way.
 - **robustness **from the regression benefit.  Confidence the code that worked yesterday still wroks today.  Confidence that the edge cases have been taken care of as well.
 - **verification**.  "Regression is really verification"
 - **confidence**.  Confidence is gained by having feedback when making changes
 - **decoupling**.  Difficult to test code with many dependencies, this leads to decoupling during the design process.
 - **form of documentation**.  Unit tests can illuminate how code actually behaves.
 
 
TDD leads to "clean code that works".  This gives a certain level of predicatability.


## Programming by intention
TDD helps program by intention.  Focus on passing tests.  If something new needs to be done, write a test first, then write the minimal code to pass it.


Unit Testing is done in the Tactical Design phase.
    
## Tic Tac Toe game design
Initial set of classes:
Game
Player
Peg
Grid
Cell
Turn
Rule
Score
ScoreHistory


UML diagram






