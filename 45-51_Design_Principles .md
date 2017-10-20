# 45-51 - Design Principles 
YAGNI - you aren't gunna need it (yet). Don't plan for the future in terms of extensibility. Solve the problem at hand and expand when necessary
DRY - don't repeat yourself
SRP - single responsibility principle, keep methods/classes/modules small and transparent to what their functionalities are
High cohesion - keep components that can be logically grouped together as part of the same module and other out
Low coupling - reduce dependencies between modules. Use interfaces when connecting modules together.
## Open closed principle


> Software entities (Classes, Modules, Functions, etc.) should be open for extension, but closed for modification" - Bertrand Meyer


Characterstics of poor design:
- Single change results in cascade of changes
- Program is fragile, rigid, and unpredictable
Characteristics of good design:
- Modules never change
- Extend module's behabior by adding new code, not changing existing code


Here's an example of a class depending on another **concrete** class, and then we will fix it.

```java
public class Car {
     private int year;
     private Engine engine; // tightly coupling engine
     public Car (int theYear, Engine anEngine) {
          year = theYear;
          engine = anEngine;
     }
     
     public Car(Car other){
          year = other.year;
          engine = new Engine(other.engine); 
                // new is not polymorphic
          // If we wanted a car with TurboEngine instead, where TurboEngine extends from Engine
          // engine.toString() would output that it would still be a class Engine
          // We would have to modify the code to accomodate sublcasses of engine to get the code
          // to do what we want it to do, breaking OCP's "closed for modification" clause
     }
     @Override
     public String toString() {
          return "Car: " + year " " + engine.toString(); 
    }
}

public class Engine{
    public Engine() {
    }
    public Engine(Engine other) {
    }

    @Override
    public String toString(){
        return getClass().toString() + ":" hashCode();
    }
}

public class Sample {
    public void main(String[] args){
        Car car1 = new Car(2015, new Engine()); // what about Car car1 = new Car(2015, new TurboEngine());?
        System.out.println(car1);
        
        Car car2 = new Car(car1); //making a copy of car1
        System.out.println(car2);
    }
}
```


Instead of modifying the code and having to constantly go back and modify the copy constructor method everytime we make a new subclass of engine, lets make Engine an abstract class so we can use polymorphism. A class should **not** depend on a concrete class but an abstract class.



```java
public class Car {
     private int year;
     private Engine engine; // tightly coupling engine
     public Car (int theYear, Engine anEngine) {
          year = theYear;
          engine = anEngine;
     }
     
     public Car(Car other){
          year = other.year;
          engine = other.engine.makeCopy(); // Java's clone() is broken, so lets make our own makeCopy() method
          // now it will call the proper makeCopy() from the Engine subclasses, and return the right engine
     }
     @Override
     public String toString() {
          return "Car: " + year " " + engine.toString(); 
    }
}

public class Engine{
    public Engine() {
    }
    public Engine(Engine other) {
        super(other);
    }
    @Override
    public String toString(){
        return getClass().toString() + ":" hashCode();
    } 
    public Engine makeCopy(){
        return new Engine(this);
    }
}

public class TurboEngine extends engine{
    public TurboEngine() {
        super();
    }
    public TurboEngine(Engine other) {
        super(other);
    }
    @Override
    public Engine makeCopy(){
        return new TurboEngine(this);
    }
}

public class PistonEngine extends engine{
    public PistonEngine() {
        super();
    }
    public PistonEngine(Engine other) {
    }
    @Override
    public Engine makeCopy(){
        return new PistonEngine(this);
    }
}
```


So now this will work, and we wont have to touch either the Car or Engine class ever again.

```java
public class Sample {
    public void main(String[] args){
        Car car1 = new Car(2015, new TurboEngine());
        System.out.println(car1);
        
        Car car2 = new Car(car1);
        System.out.println(car2);
    }
}

// Car: 2015 class TurboEngine:12848384
// Car: 2015 class TurboEngine:84839209
```


We could stop there, but lets abstract this further to improve it a little more by using an **abstract **engine class

```java
abstract public class Engine{
    public Engine() {
    }
    public Engine(Engine other) {
        super(other);
    }
    @Override
    public String toString(){
        return getClass().toString() + ":" hashCode();
    } 
    public abstract Engine makeCopy(); // implementation is left to subclasses of engine
}
```


What about this?

```java
public class Car {
     private int year;
     private Engine engine; // tightly coupling engine
     public Car (int theYear, Engine anEngine) {
          year = theYear;
          engine = anEngine;
     }
     
     public Car(Car other){
          year = other.year;
          engine = other.engine.makeCopy();
          engine = new Engine(other.engine); // java wont allow this because its an abstract class. great!
          engine = new TurboEngine(other.engine); // it allows us to do this however..
          // how do we keep the copying limited to just the makeCopy() method?
     }
     @Override
     public String toString() {
          return "Car: " + year " " + engine.toString(); 
    }
}
```


How do we keep the copying limited to just the makeCopy() method? By protecting it, but make sure they're not part of the same package as protected includes classes in the same package in Java.

```java
public class TurboEngine extends engine{
    public TurboEngine() {
        super();
    }
    protected TurboEngine(Engine other) {
        super(other);
    }
    @Override
    public Engine makeCopy(){
        return new TurboEngine(this);
    }
}

public class PistonEngine extends engine{
    public PistonEngine() {
        super();
    }
    protected PistonEngine(Engine other) {
        super(other);
    }
    @Override
    public Engine makeCopy(){
        return new PistonEngine(this);
    }
}

// we can also abstract once more to make Car also have a makeCopy() method so instead of calling Car car2 = new Car(car1);
// we can call Car car2 = car1.makeCopy();

public class Car {
     private int year;
     private Engine engine; // tightly coupling engine
     protected Car (int theYear, Engine anEngine) {
          year = theYear;
          engine = anEngine;
     }
     public Car makeCopy(){
          return new Car(this);
     }
     
public class Sample {
    public void main(String[] args){
        Car car1 = new Car(2015, new TurboEngine());
        System.out.println(car1);
        
        Car car2 = car1.makeCopy();
        System.out.println(car2); 
    }
}
```




## Decisions on using OCP
- Do not force yourself to over extend your program. If extensibility isn't needed. don't extend it by making all your classes abstract!
- No program can be 100% closed.
- The designer must decide what kinds of changes to close the design for.
- Closure is strategic and cannot be for every single detail
  - Example: Asking the car to contain two engines. Can we ask our current implementation of Car to do that without changing it? No, plan strategically
  - Ask domain experts if certain capabilities need to be implemented, or maybe even become a domain expert


OCP...
- Make all member variables private
- encapsulation, all classes that depend on my class are closed from change to variable names or implementation. Member functions of my class are never closed from these changes.
- No global variables
- Runtime Type Identification is ugly and dangerous (RTTI)
  - C++:  dynamic_cast (didn't get RTTI until 1998, way after Java since it makes code less extensible)
  - Java: instanceof
  - C#: is

```java
if (other.engine instanceOf ...) {}; // evil!
// every time you introduce a new object you would have to write more code to do RTTI

// use
instance.abstractMethod(); // define abstract method implementation in the subclasses, like we saw above
```


## Liskov's Substitution Principle
- Inheritance is used to realize abstraction and polymorphism which are key to OCP
- How do we measure the quality of inheritance?
  - Functions that use pointers or references to base classes must be able to use object of derived classes without knowing it
Behavior
- Advertised behavior of an object
- Advertised requirements (pre-condition)
- Advertised promise (post-condition)
Design by contract
- Advertised behavior of the derived class is substitutable for that of the base class.
- Substitutability: dervice class services require no more and promise no less than the specfications of the corresponding services in the base class.

```java
interface Random{
    public int nextRand(int from, int to);
    //pre-condition
    // from is >= 0
    // to is >= from
    
    //post-condition
    //from >= return value <= to
    // the result will be in a random distribution
}

class Randomizer implements Random{
    @Override
    public int nextRand(int from, int to){
        if(from <= 0 || to < from) 
            throw new RuntimeException("from should be >= 0 && to >= from");
        return from;
    }
}

class EfficientRandomizer extends Randomizer {
// to should be at least 2 times greater than from
    @Override
    public int nextRandom(int from, int to){
        if(to < 2*from...)
                throw new RuntimeException("...");
        return from + 1;
    }
```


EfficientRandomizer breaks Liskov's Subsitution principle since it is not directly substitutable where Randomizer is, as it require 'to' to be 2 x greater than from, where as Randomizer just want any int to and from. We stay within the contract of Random, but fail substitutability.
