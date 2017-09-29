# 20-23 - Testing with Dependencies
When writing unit tests for a piece of software, is often hard to write tests for every scenerio or to fully simulate a transaction. In the case of writing tests for some sort of online store, you would want to write test cases for entering the wrong credit card information, making sure the transaction went through, making sure the credit card isn't expired etc. Rather than testing your software with real credit card numbers, vendors set up proxy servers that allow you to test your software with fake credit card numbers that simulate real world transactions. You provide a credit card with a valid 16 digit number, and the server responds with the correct message.


There are 4 different types of tests that we can write that can help us with this sort of simulation: **Mock, fake, spy, and stub.**


Spy: forwards calls to another object while replacing certain methods


Fake: a class or service that is like the real one but is only used for testing and is not suitable for the production environment.


Mock: Simulates ill-behavior or good behavior of the true object.


Stub: Stands in for a real object and returns a "cooked up" response.
Both mock and stub stand in during tests in place of the real object.


## CurrencyExchanger
We are writing software for a currency exchange service. We have 3 vendors that we query for the price, and markdown the higheset by 2% and retain it as profit for our company. We query vendor 1 who gives us 5.22 usd for every Norweigan Kronos (NOK). We query vendor 2, get an error response, so we log the error. We query vendor 3, who gives us the highest price of 5.76, we mark it down 2%, and return 5.65 to the user.


What are some possible test cases we could write?
- markdown by 2% of value of 100
- markdown by 2% a value of 50
- markdown by 2% of a value of 97
- markdown by 2% of a value of 0
- get rate when there is no vendor
- get rate when there is one vendor
- get rate when there is one vendor with error
- get rate when two vendors with first having higher rate
- get rate when two vendors with decond having higher rate
- get rate should log error when a vendor fails


Start with writing tests that do not involve the dependency (markdowns)


/test

```java
package example.exchange;
import org.junit.Test;
import static org.junit.Assert.assertTrue;
import static org.junit.Assert.assertEquals;
public class CurrencyExchangeTest {
    private CurrencyExchange currencyExchange;
    
    @Before
    public void init(){
        currencyExchange = new CurrencyExchange();
    }
    
    @Test
    public void canary() {
        assertTrue(true);
    }
    @test
    public void mardownForProfitForRateOf100() {
        //assume were not dealing with doubles
        assertEquals(9800, currencyExchange.markdown(10000);
    }
    @test
    public void mardownForProfitForRateOf50() {
        assertEquals(4900, currencyExchange.markdown(5000);
    }
    @test
    public void mardownForProfitForRateOf97() {
        assertEquals(9506, currencyExchange.markdown(9700);
    }
    @test
    public void mardownForProfitForRateOf0() {
        assertEquals(0, currencyExchange.markdown(0);
    }
    @test
    public void getRateWhenThereAreNoVendors() {
        assertEquals(0, currencyExchange.getRate("NOK");
    }
    @test
    public void getRateWhenThereIsOneVendor() {
        assertEquals(9800, currencyExchange.getRate("NOK");
    }
}
```
/src

```java
package example.exchange;
public class CurrencyExchange {
    public int mardown(int rate) {
        return rate - rate * 2/100;
    }
    public int getRate(String currency) {
        return 0;
    }
}
```


After those are done, then we deal with dependencies.


1. Get rate when there is no vender. Solution: we could throw an exception, but does our user really need that? All they need to know is if we can do it or not.
CurrencyExchange is going to talk to one vendor, so we need to introduce some kind of vendor object. We don't want our currency exchanger to to tighly couple to one vendor, **Dependency inversion principle,** we dont want our currency exchange to talk to a vendor, but rather have it talk to interface that the vendor then implements. We will always be adding, changing, or removing vendors. Let's inject a **mock **object


There are a couple way to inject a mock object:
- constructor based: pass a mock object to the contructor. Venkat is not a fan as it is not flexible

```java
@test
public void getRateWhenThereIsOneVendor() {
    RateService rateService = null;
    new CurrencyExchange(rateService);
    assertEquals(9800, currencyExchange.getRate("NOK");
}

//src
public class CurrencyExchange {
    RateService rateService;
    public CurrencyExchange(RateService rateService){
        theService = rateService
    }
}
```
- setter based. Venkat likes this one. A mock should only focus on the specific interaction needed for that one test to pass

```java
@Test
public void getRateWhenThereIsOneVendor() {
    RateService rateService = new RateService() {
        @Override
        public int rateForCurrency(String currency) {
                return 10000;
        }
    };
    currencyExchange.setRateService(rateService);
    assertEquals(9800, currencyExchange.getRate("NOK");
}

// if you have 5 methods they you need to mock, just write a dummy class
//class RateServiceDummyImplements RateService {
    // all the methods here throw a new RunTimeException("Not Implemented");
//}
// rateSeriveWithError = new RateServiceDummy(), and implement that method

@Test
public void getRateWhenOnlyVedorAndThatVendorHasError(){
    RateService rateServiceWithError = new RateService() {
        @Override
        public int rateForCurrency(String currency) {
                throw new RuntimeExpception("Error Processing");
        }
    };  
    currencyExchange.setRateService(rateService);
    assertEquals(0, currencyExchange.getRate("NOK"));
}

@Test
public void getRateLogsErrorWhenAVendorFails(){
    CurrencyExchange currencyExchange = new CurrencyExchange() {
        String logMessage = "";
        @Override
        void writeToLog(String message) {
               assertEquals("Error talking to vendor: Error processing", logMessage);
        }
    };
    RateService rateServiceWithError = new RateService() {
        @Override
        public int rateForCurrency(String currency) {
                throw new RuntimeExpception("Error Processing");
        }
    };
    currencyExchange.getRateService(rateServiceWithError);
    currencyExchange.getRate("NOK");
    }  
}
//src

public class CurrencyExchange {
    RateService theService;
    public void setRateService(RateService rateService){
        theService = rateService
    }
    
    void writeToLog(String message){
        throw new RuntimeExpcetion("Not implemented yet");
    }
}

public int getRate(String currency) {
    if(theService != null){
        try{
            int rate =  theService.rateForCurrency(currency);
            return markDown(theRate);
        } catch (Exception ex) {
                String message = String.format("Error talking to vendor: %s", ex.getMessage());
                writeToLog(message);
                return 0;
        }
    }
}
```
- factory (tools like Spring and Guice) (Venkat is not a big fan as it adds complexity without much value, unless you have multiple dependencies and do not want to inject objects into each one)

```java
//test
//Configure the service factory to return either a fake or a mock object, or in a production run, return a real object
//src
public class CurrencyExchange {
    RateService theService;
    public void getRate(RateService rateService){
        serviceFactory.getService();
        return 0;
    }
}
```
- overriding


## When do we mock?
What is the purpose of the mock anyway? To make tests easier to write and faster to run. If you can write tests without mocks, do not use mocks. Try to avoid dependencies if you can get away with it. Maybe try to develop fake services insead to stand in place for tests. Mock when tests are slow, unpredictable, or you are trying to simulate unlikely failure situations that are hard to test for.


What about working with databases? An alternative to mocking when working with databases is to configure the tests to run the transactions in roll-back only mode.
[http://blog.agiledeveloper.com/2009/10/try-to-knockout-before-you-consider-to.html](http://blog.agiledeveloper.com/2009/10/try-to-knockout-before-you-consider-to.html)


