# 15 - Test Driven Development
## Driving development using test cases
Developers are given a set of User Stories from the **Project Backlog**.  User stories can be different sizes.  The **Release Backlog** consists of the User Stories that will be implemented in the current release.  The **Sprint backlog **or **Iteration Backlog **includes what is planned for the "current sprint", meaning the high priority features.  
undefined"The time is takes to implement a story should be a fraction of a sprint"


User stories should be broken into smaller stories until it is possible to implement them in a sprint.  How to go from User Stories to tests?


## 3 C's
1. Card - Write the user story on an **Index Card**.  
1. Conversation - Testers, programmers, business analysts can sit and talk through the details of a particular story.
1. Confirmation - On the back of the index card** **write down potential test cases.  This is mainly done by the testers and business analysts.  For example, if a User story is "A customer can deposit money in a bank account", then a possible test is "Customer deposits $100.00", or "Customer deposits $5000.00 or more" (maybe a different rule for large sums). or "Someone other than the customer deposits money in the Customers account" (different rule for others making desposits), etc.


## INVEST
I - Stories should be **Independent **of each other
N - Stories should be **Negotiable**.  "Is this really important or can this be done differently?"  Conversation is important here.
V - Stories should be **Valuable **to the Customer.  
E - Stories shoule be **Estimable**.  Meaning, it should not be difficult to estimate how long it will take to implement the story. 
S - Stories should be** Small**.  
T - Stories should be **Testable**.  The stories should be written in a way that makes them testable.


## Levels of tests
Pyramid structure with Unit tests at bottom.  Unit tests -> functional tests -> acceptance tests ->UI tests
Unit tests are predominantly driven by programmers.
Functional and acceptance tests are driven by business analysts and testers.  Programmers must support the testers for them to be able to run higher level automated tests.  Testers must support to the programmers by writing automated tests so that programmers can get immediate feedback when making changes.


### Some tools for turning requirements into test examples:
R-Spec
SpecFlow
Cucumber
FIT
FITNess


undefined"Software is nonlinear.  It is easy to change something over here and break something over there"
Often we find that things "went wrong" when someone stumbles upon a problem by using a particular piece of code.  Using automated tests allows for many scenarios to run automatically as soon as the code is changed, giving much faster feedback.  Changes can be pushed to profuction much quicker.  This "**Regression benefit**" is pretty long lasting.  Automated testing can give a  "**Design benefit**" as well.  Writing test cases helps put developers in the shoes of the people using the code.  In order to obtain an design benefit, it is important for developers to "listen to the tests" and steer the design in a better direction.


undefined"This code is not testable" is a euphemism for  "this code and its design sucks"


Applying TDD changes the way we think and approach problems






















