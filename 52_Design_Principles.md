# 52_Design_Principles
## Liskov Substitution Principle
- Inheritance should only be used when a derived object can directly substitute a base object
- Violating LSP may also mean we are in violation of the Open Closed Principle

Bird show example:
```java
class Bird {
    public void showBeak(){
        System.out.println("showing beak");
    }
}

class Eagles extends Bird {
    @Override
    public void showBeak(){
        System.out.println("show that excellent beak");
        bird.fly(); // lets make our eagle fly
    }
}

class Performer {
    public static void showBird(Bird bird) {
        bird.showBeak();
    }
}

public class Sample{
    public class void main(String[] args){
        Performer performer = new Performer();
        perfromer.showBird(new Eagle());
    }
}
```

If we want our eagle to also be able to fly, well then we would have to go back to the Bird base class and make a fly() method as well. The issue is that if we want new functionality in a derived class, then we always have to go back to the base class and any other derived class to make changes as well if we want to uphold LSP. This can easily become a nightmare as your classes grow.

And what about if we want to add penguines? They don't fly so we would have to throw some kind of excepetion, and our poor penguine fails at runtime. This fails LSP. We cannot use a penguine everywhere a regular bird can be used. 

Another terrible design would be to do type checking for the performer, which would vioalte the Open Closed Principle. We don't want to have to change the implementation of the Performer class every single time we add a new bird. 
The user of an object should never know the difference between the base object or a dervied object. It should treat all object of that base type the same. LSP.

```java
class Performer {
    public static void showBird(Bird bird) {
        bird.showBeak();
        if(!(bird inststanceof Penguine)){ //terrible
                bird.fly();
        }
    }
}
```

A better idea would be to create a Bird class, with a derived FlyingBird class.

## LSP in Java
- It is used in Java in at least two places
  - Overriding methods can not throw new unrelated expcetions
  - Override method's access can't be more restrictive than the overridden method
    - You cannot override a public method as protected or private in a derived class
Access:
```java
class Bird{
    public void showBeak(){}
}
class Parrot extrends Bird {
    protected void showBeak(); // Java does not allow this
}
```
C++ is wild and does would allow you to do this. C# would make you have showBeak() be protected methods in both the base and dervied class.
The reason why the "extends" keyword is used in Java, is because we are broadening usage, not restricting it. Require less promise more.
```java
class Bird{
    protected void showBeak(){}
}
class Parrot extrends Bird {
    public void showBeak(); // Java allows this, since we are broadening usage. C# would not allow this
}
```

Exceptions:
```java
class E1 extends Exception {}
class E2 extends Exception {}
class Bird {
    public void showBeak() throws E1{//...}
}
class Parrot extends Bird{
    public void showBeak() throws E2{//...} // Java does not allow you to do this. Exceptions must be related between dervied and base classes
}
```
But this would be OK:
```java
class E1 extends Exception {}
class E2 extends E1 {}
class Bird {
    public void showBeak() throws E1{//...}
}
class Parrot extends Bird{
    public void showBeak() throws E2{//...} // Great, E2 is compatible to E1
}
```
Heres the reason why:
```java
class E1 extends Exception {}
class E2 extends Exception {}
class Bird {
    public void showBeak() throws E1{//...}
}
class Parrot extends Bird{
    public void showBeak() throws E2{//...}
}

public class Sample{
    public static void showBird(Bird bird){
        try {
                bird.showBeak();
        } catch (E1 e1){
                //handle the exception
        }
    }
}
```
If E2 is of a different class, then the try catch block won't know how to handle it since it can only catch exceptions of class E1. 
If E2 extends class E1, then it would work cause E1 is somewhat subsitutable for E2,. It can be argued that Java doesn't go far enough to uphold LSP. If E2 is thrown where E1 is expected, although they are related they aren't enirely the same. Otherwise you wouldn't need to use inheritance . Java doesn't completely uphold LSP

Another place that Java fails with LSP is the stack implementation
```java
public class Stack<E> extends Vector<E>;
```
Vectors are designed to have values inteserted and removed at any point, where as a stack you can only push and pop one element at a time in a "First in Last Out" fashion.
If we have code base that tosses a stack into a function that accepts a vector, its going to start severely abusing that stack. A stack cannot subsitute a vector and vice-versa.
They should have used **delegation** instead of inheritance: The stack would interally use a vector for push() and pop() instead of extending the vector class.


If you want to reuse a class then use delegation. If you want an instance to be used in place of another, then use inheritance, but ensure substitutability in that case.
