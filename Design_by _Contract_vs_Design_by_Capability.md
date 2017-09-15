# Design by Contract vs Design by Capability


Mostly influenced by the programming languages we choose. Statically typed languages like Java, c++. and C# often lead us to design by contract, where dynamically typed languages like Smalltalk and ruby lead us to design by capability, and groovy provides the best of both worlds.


## Interfaces
An interface is an abstraction with a collection of methods. Abstractions do not allow to have any implementation in them, rather implementation is left to the user of the interface. When a class inherits from an entity, an abstraction, we call it *inheritance. *When a class inherits from an interface, we call it interface based inheritance.
- Interfaces in C++ come in the form of pure abstract classes with only pure virtual functions
- Java and C# define abstractions by specifically using the keyword *interface. *Both languages allow inheritance from one or more interface but support only single implementation inheritance


## Design by Contract
An interface sets certain preconditions that must be met by an implementation, and are strictly enforced by compilers. Useful for a plugin architecture. 


- One way we can see design by contract is by implementing the common swing interface for Java. When we implement the interface, we enter into a sort of "contractual agreement"  with swing. Swing provides several methods that handle mouse event listeners, and we can override those methods to provide a specific implementation.
- addMoustListener() passes in a new HandleMouse() object. HandleMouse() implemnts the MouseListener() interface, so we must override the various methods provided by MouseListener(), including MouseListener.mouseClick(), MouseListener.mousePressed(), MouseListener.mouseExited() and so on.
- If you try to compile without at least including the method signature for all of the MouseListener methods, you will get an error because you haven't "fulfilled the contract"



```java
import javax.swing.*;

public class Sample extended JFrame{
    private Jbutton button;

    @Override
    protected void frameInit() {
        super.frameInit();
        button = new Jbutton('click');
        getContentPane().add(button);
        
        button.addMouseListener(new HandleMouse());
    }
    
    public static void main(String[] args) {        
    JFrame frame = new Sample();
    frame.setSize(100, 100);
    fame.setVisible(true);
    }
    
    private class HandleMouse implements MouseListener {
        @Override
        public void mouseClicked(MouseEvent mouseEvent) {
            JOptionPane.showMessageDialog(mouseEvent.getComponent(), "You clicked");
        }
        
        public void mousePressed(MouseEvent mouseEvent){
        }
        
        public void mouseRealeased(MouseEvent mouseEvent){
        }
        
        public void mouseEntered(MouseEvent mouseEvent){
        }
        
        // more methods
    }
}
```


If we don't want to implement all of the methods and just want to implement mouseClicked() like in the example above, then we can instead *extend* the MouseAdapter and implement just what we need, eliminating the need for empty method signatures.



```java
 private class HandleMouse extends MouseAdapter {
        @Override
        public void mouseClicked(MouseEvent mouseEvent) {
            JOptionPane.showMessageDialog(mouseEvent.getComponent(), "You clicked");
        }
```


## Burden by Contracts
An almost sexist but useful representation of where contracts may cause a headache.


Imagine you need help moving a box from one side of the room to the other. You are in a room full of able and willing people, so you try to ask someone for help. You can create a new instance of Man named "bob", and call seekHelpContract(bob), and he will gladly come and help. But if you try to create a new instance of Woman named "sara" and try to call seekHelpContract(sara), you get a compile time error.


The seekHelpContract() method simply does not allow you to use a woman object, as it only accepts Man objects. A situation may arise where you try to implement a feature that simply cannot work because it violates the contract, even though in this case help() is exactly the same for both Man and Woman.

```java
class Man {
    public void help() {
        System.out.println("Man Helping");
    }
}

class Woman {
    public void help() {
        System.out.println("Woman Helping");
    }
}

public class Example {
    public static void seekHelpContract(Man helper) {
        helper.help();
    }
    public static void main(String[] args) {
        Man bob = new Man();
        seekHelpContract(bob);
        // "Man Helping"
        
        Woman sara = new Woman();
        seekHelpContract(sara);
        // compiler error
    }
}
```


A way around this is to implement an abstract Human class, extend it to the Man and Woman class, and change the seekHelpContract() method to take Human objects.
This brings our level of abstraction up another level.



```java
abstract class Human {
    abstract public void help();
}

class Man extends Human {
    public void help() {
        System.out.println("Man Helping");
    }
}

class Woman extends Human {
    public void help() {
        System.out.println("Woman Helping");
    }
}

public class Example {
    public static void seekHelpContract(Human helper) {
        helper.help();
    }
    public static void main(String[] args) {
        Man bob = new Man();
        seekHelpContract(bob);
        // "Man Helping"
        
        Woman sara = new Woman();
        seekHelpContract(sara);
        // "Woman Helping"
    }
}
```


Turns out however that there is an elephant in the room, and that the elephant is able to carry even more. We can't simply extend Human to our Elephant now can we? This is where your design should take on an interface instead of class implementation.



```java
interface Helper {
    public void help();
}

class Man implements Helper {
    public void help() {
        System.out.println("Man Helping");
    }
}

class Woman implements Helper {
    public void help() {
        System.out.println("Woman Helping");
    }
}

class Elephant implements Helper {
    public void help() {
        System.out.println("I can carry much more");
    }
}

public class Example {
    public static void seekHelpContract(Helper helper) {
        helper.help();
    }
    public static void main(String[] args) {
        Elephant eli = new Elephant();
        seekHelpContract(Elephant);
        // "I can carry much more"
    }
}
```


There are tradeoffs in any case. You may not want to start with an interface implementation from the get go as that can potentially over complicate a simple system. If you are only expecting one class, than our contract that only accepted a man object may be suitable. If you are expecting many different types of objects, than implementing an interface may be a good approach.


## Design by Capability
In dynamically typed languages, the burden of design by contract fades away. Functions works as long as the passed object is "capable and willing". 


Groovy is optionally typed, dynamic, and supports contracts. Here is a groovy example that enforces that contract just like java.

```java
interface Helper {
    public void help();
}

class Man implements Helper {
    public void help() {
        System.out.println("Man Helping");
    }
}

class Woman implements Helper {
    public void help() {
        System.out.println("Woman Helping");
    }
}

class Elephant implements Helper {
    public void help() {
        System.out.println("I can carry much more");
    }
}

public static void seekHelpContract(Helper helper) {
    helper.help();
}
    
Man bob = new Man();
seekHelpContract(bob);
// "Man Helping"
        
Woman sara = new Woman();
seekHelpContract(sara);
// "Woman Helping"
        
Elephant eli = new Elephant();
seekHelpContract(Elephant);
// "I can carry much more"
```


Let’s tear down the types and use groovy's dynamic definitions instead.



```groovy
class Man {
    def help() {
        println "man helping"
    }
}

class Woman {
    def help() {
        println "woman helping"
    }
}
public void seekHelpFrom(helper) { //remove object type
    helper.help();
}

def bob = new Man()
seekHelpFromHelper(bob)
// "man helping"

def sara = new Woman()
seekHelpFromHelper(sara)
// "woman helping"
```


Notice how we didn't have to change seekHelpFrom() and it accepts both Man and Woman objects as the helper parameter is non typed. We didn't have to introduce an abstraction or any type of hierarchy. seekHelpFrom() works with any object as long as it has the help() method. **This is design by capability.**


Now if we create a Fox class without a help() method, then we can a runtime error. Most dynamic languages have methods that can check if a particular object is "capable" of being using in a particular function. However, using these methods is not considered good practice, and instead you should model the system based on expectations.



```groovy
class Fox {
}

public void seekHelpFrom(helper) {
    if(helper.respondsTo('help'))
        helper.help()
    else
        println "Are you kidding..."
}

seekHelpFrom(new Fox());
// "Are you kidding..."
```


## Summary
Benefits of design by capability:
- Design is lightweight, no need to build deep hierarchies of abstraction
- Easier to evolve the design, more flexible


Downside of design by capability:
- Uncertainty of the capability of the objects being passed around


You can mitigate the downside as much as possible by running extensive automated tests that track down every path of execution, and having an overall simple design.


Benefits of design by contract:
- Compile time checks to make sure every implementation is correct and abides by the interface's contract


Downside of design by contract:
- Harder to evolve


Key takeaway: Don't be afraid to choose any of these designs, but the language you use may pick for you. Java and C# enforce design by contract, languages like Ruby and JavaScript are design by capability, and Groovy allows you to pick.
