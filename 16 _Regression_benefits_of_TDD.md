# 16 - Regression benefits of TDD


Software systems grow in size and complexity as time goes by.  Developers may come and go, but the software will evolve.  How do we know what worked before continues to work going forward?  It becomes impossible to test everything manually.  So only critical things get tested...and then developers must wait to hear about erros from customers or users.


## Feedback
Assume TDD is not being used.  If something goes wrong when modifying a piece of code, when will we find out?  It can take years...long enough that the developer does not remember the implementation details.  This drives up the cost of making changes.  Additionally, "fixing" a problem means making changes to the code and waiting to see if the users find any more problems.  This leads to fixing the same problem over and over as each new change results in more effects.


## The importance of feedback loops
Without a feedback loop, the only way to know code is broken is when a particular execution of the code runs into the scenario responsible for the error.  Additionally, things can "fail quietly" and pass incorrect data around for quite a while before the effects are seen.


## Calculator test example
CalculatorTest.java

```java
import org.junit.Before;
import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class CalculatorTest{
    //define field for calculator
    private Calculator calculator;
    
    @Before
    public void setUp(){
        calculator = new Calculator();
    }
    
    @Test
    public void AddTwoPositiveNumbersReturnsTheirSum(){
        assertEquals(6, calculator.add(2,4));
    }
    @Test
    public void AddAPositiveAndNegativeNumberReturnsTheirSum(){
        assertEquals(3, calculator.add(6, -3);
    }
    @Test
    public void DivideOfTwoPositiveNumbersReturnsAPositiveResult(){
        assertEquals(6, calculator.divide(12, 2);
    }
    @Test
    public void DivideOfAPositiveNumberByANegativeNumberReturnsANegativeResult(){
        assertEquals(-3, calculator.divide(12, -4);
    }
    
    //We still need a test for division by 0.  This is an "Exception Test". If the code does not throw an exception, we want the test to fail.
    //The test passes if the code fails, the test fails if the code passes.  This test establishes an expectation in the code.  
    //The expectation is that if there is a division by zero we expect an exception. 
    //NOTE this test will fail if using doubles rather than ints.  Floating point divinsion doesnt throw an exception like integer division
    //does.  Often floating point division returns NaN.
    @Test
    public void DivideByZeroThrowsAnException(){
        try{
            calculator.divide(6,0);
            //landmine method...it will blow up if you step on it.
            fail("Expected exception for division by zero");
        } catch(ArithmeticException ex){
            assertTrue(true);
            //:)
        }
    }
    
    //Example of an additional test to ensure everything is working. This was used with the other tests commented out to show why testing 
    //is important
    //@Test
    //public void testRun(){
    //    assertTrue(true);
    //}
}
```




Calculator.java

```java
public class Calculator {
    public int add(int op1, int op2){
        return op1 + op2;
    }
    
    public int divide(int numerator, int denominator){
        return numerator/denominator;
    }
}
```
Note if the test cases are removed (commented out) in the code above it will still run without problems.


If we want to refactor the divide function so that it uses doubles rather than ints, the tests will reveal problems.  Most notably the divide by zero test.  In many languages, floating point division returns NaN when dividing by 0 rather than throwing an exception.  NOTE, even if this problem was unexpected, the test illuminated the problem immediately.  The developer didnt have to wait for someone to encounter this problem by manually trying to divide by 0 after the change had been made.



```java
"You'd rather have your code tell you that your code sucks than have a colleague tell you your code sucks"
```


## TDD and Bugs
TDD does NOT guarantee bug free code.  There will always be bugs.  TDD gives us the ability to ensure the bugs do not recur in the code.
What to do when you find a bug?  DO NOT fix it first.  Instead, first write a test that fails because the bug is there.  Once the test is written and failing, fix the code, and then ensure the test is passing.  Now the test is there to guard you from similar failures in the future.


