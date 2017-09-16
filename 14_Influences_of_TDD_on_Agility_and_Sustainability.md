# 14 - Influences of TDD on Agility and Sustainability


Projects already take a lot of time, why spend more time writing test cases?


Agile Development boils down to one thing:  Feedback.  Feedback loop is necessary to make relevant software.  
Ask customers:  is it serving your needs? Does it provide value? Is it useable and intuitive? 
However, be careful:
undefined"If your objective is to build what your customers wanted, you will fail.  You need to build what they still want."
The point is to deliver what is STILL relevant and useful.


## Circles of Feedback:
2 types:  
Inner circle - Code meets programmers expectations.  "Circle of Expectations".
Outer circle - Code meets customer's expectations. "Circle of Relevance".


Both are important.  If only the inner circle is focused on, the code may be developed using good programming principles, but may still be irrelevant to the customer's needs.  
undefined"It is not only important to develop the software right, it is important to develop the software"


If only the outer circle (the circle of expectation) is focused on, the proper technical practices may be ignored while focusing on the customer's expectattion.  This can lead to unsustainable development.  Meaning, the customer's expectations may be met initially, but as development continues and the application gets larger it will easier and easier to cause problems when making changes.  


undefined"For Agile development to be successful we want both of these circles of feedback available"


## Cost of Test Driven Development:
1. **Cost of Testing** - First, testing should be a natural part of the development process.  Try not to separate it.  For example,  asking "how much time do you spend writing classes?" is not terribly helpful or informative.  Writing classes is part of the process of developing code.  TDD does have benefits though.  Venkat gives an example of two teams (T1 and T2) that were asked to develop an application to solve the same problem.  T1 was told to just implement the solution.  T2 was told to use TDD and develop automated tests from the beginning.    T2 took 16% longer than T1 to deliver the application.  Both teams were then told to go make some changes to the code.  This time T2 came back first and when T1 did complete, their code had many bug issues.  Using TDD can raise the **manufacturing cost** a little bit, but this imporvement in quality can reduce the overall **maintenance cost** in the long run.  Do you want **Local Optimization **or** Global Optimization**?  Local Optimization means reducing the programming cost.  Global Optimization means reducing the overall cost of developing AND maintaining the software.
1. **Cost of Learning** - One of the biggest challenges of TDD.  We must learn to write test cases, and in many cases we must unlearn poor programming habits.  Pair programming or something similar is essential for learning how to work through test cases.  This learning/unlearning process can take several months but is necessary. 


## What is the cost of not doing TDD?


 - Project failures, massive bugs when making changes, higher cost of maintenance, etc.  Why repeat what doesnt work?




Using automated tests allows changes to be made faster.  They can also greatly boost "confidence" that the changes did not cause conflicts and the software still works by providing nearly instant feedback to the developer.  The developer can spend less time looking through the code for possible conflicts.  Without TDD, developers often "hope" changes work...this is Jesus Driven Development.  TDD is better than JDD...you get the feedback loop that tells you "the code that worked yesterday still works today".


## Software Entropy


Software changes, things go bad, fall apart, evolve, etc.  We must keep maintaining and improving the software the whole time.  Strategic and Tactical desing




## When and how to apply TDD?


TDD fits nicely into the Tactical Design phase.  
Automation of testing is important BUT that does NOT mean manual testing will never be performed.  Manual testing should be performed, just sparingly.  It should be done THE FIRST TIME a new feature is created.  This brings up the difference between **testing** and **verification.**  The phrase "automated testing" is a loose term.  "Automated verification" is more accurate.  Acceptance testing, exploration testing, testing to see if a new feature is intuitive to use should be performed manually.  But **verification**,** **meaning testing to see if what worked yesterday still works today, should be **automated.  **
undefined**"Don't confuse your inability with impossibility"**
If we dont understand automated testing, it is important to figure it out.


## Regression testing and project waterfalls
Example:  Developers might have 4 stories to test.  The testers ask for 2 days to perform the tests.  Later the developers come back with 4 more stories.  This time the testers ask for 5 days to test.  Why?  The testers are not just testing the new stories, they must test the entire application including the new stories.  This is **Regression testing, **it can cause massive delays as any code change resets the testing clock.  Postponing the testing for the code to stabilize leads to far too many manula verifications.  The alternative to this is to run a manual test once, extract all of the useful infromation, and then author test cases for a machine to run.  Then whenever the code is modified, the automated scripts can run the tests and the tester is not needed.  Instead, the tester can focus on authoring more automated test cases.
Automation is critical.  A tester's job is to manually verify something once, and then automate the verification for future execution.


## Types of testing
1. White box testing - The testing is interested in the design and implementation of the code.  Example:  Unit Testing.  In general, white box testing is in the **inner feedback loop**.  Unit testing is focused on meeting the programmer's expectations. 
1. Black box testing - The tester is NOT interested in the design or implementation of the code.  Example:  Functional and Acceptance testing.  In general, black box testing is in the **outer feedback loop**.


Developers must be careful where the tests are written.  "Writing tests at the UI level is dangerous"...Pathway to Hell testing.  UI testing is brittle, slow, and difficult.


## 3 Types of companies:
1. Companies with no automation.
1. Companies with automation, but programmers that are unwilling to automate the verification.  This leads to automation experts coming in to automate, often at the UI level.  "Pathway to hell" testing.  UI changes heavily, many tools are not capable of keeping up, tests become "brittle".  
1. Companies wtih automation at the **right level** to the **right measure**.  "Do as much automation at a low level as possible".  Testing pyramid:  many automated tests at the bottom level, fewer in the middle, and even fewer at the top level.


## If TDD is so important why is it unpopular?
it requires discipline and is difficult.  Venkat makes a comparison to exercise...most people will agree its good, but many lack the discipline and focus.


undefined"Unit testing is the software equivalent of exercise.  Automated testing is good for the health of the software."


undefined"I don't write automated tests because I have a lot of time, it is because I don't have enough"






