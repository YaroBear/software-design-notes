# 53-54_Design_Principles
Nature of bad design
- Ridid: hard to change since changes affect too many parts
- Fragile: unexpected parts break upon change
- Immobile: hard to seperate from current application for resuse in another
## Dependency Inversion Principle
Say a person needs to wake up early in the morning. You design a system where a Person class Depends on the Clock class. Clocks may have other features other than just having an alarm, and there are also many other system that make use of an alarm. SmartPhones have alarms, TVs can have alarms, etc. Maybe instead we should have our Person class depend on the Alarm. The alarm can be an interface, that many other classes implement.
We went from a concrete class depending on another concrete class, to two concrete classes depending on an interface. We can swtich between using any implementation of Alarm easily.

- A higher level module should not depend on low level modules. Both should depend upon abstractions
- Don't force it. If the code needs to be extended, then use it. If you only need one class depending on another class, then theres no need to create any extra complexity or code.

```java
class TV {
    private int volume;
    public int getVolume() {return volume}
    public void setVolume(int newVolume){
        volume = newVolume;
    }
}

class Remote{
    priavte TV tv;
    public Remote(TV theTV){
        tv = theTV;
    }
    public void increaseVolume(){
        tv.setBoume(tv.getVolume() + 1);
    }
}

public class Sample {
    public static void main (String[] args){
        TV tv1 = new TV();
        
        Remote remote1 = new Remote(tv1);
        System.out.println(tv.getVolume());
        remote1.increaseVolume();
        System.out.println(tv.getVolume());
        
    }
}
```
This is fine if we only ever expect to have one remote that is tied to the tv in this domain. But what if we have a universal remote that can control a fan and the tv too? Here is one option that may or may not work. It depends on how far you plan to extend this.
```java
interface Appliance{
    int getCurrentValue();
    void setCurrentValue(int value);
}

class TV implements Appliance {
    private int volume;
    public int getVolume() {return volume}
    public void setVolume(int newVolume){
        volume = newVolume;
    }
    
    @Override
    public int getCurrentValue(){
        return getVolume();
    }
    
    @Override
    public int setCurrentValue(int value){
        setVolume(value);
    }
}

class Remote{
    private Appliance appliance;;
    public Remote(Appliance anAppliance){
        appliance = anAppliance;
    }
    public void increase(){
        appliance.setCurrentValue(appliance.getCurrentValue() + 1);
    }
}

class Fan implements Appliance{
    private int speed;
    public int getSpeed(){
        return speed;
    }
    public int setSpeed(int newSpeed){
        speed = newSpeed;
    }
    
     @Override
    public int getCurrentValue(){
        return getSpeed();
    }
    @Override
    public int setCurrentValue(int value){
        setSpeed(value)
    }
}

public class Sample {
    public static void main (String[] args){
        TV tv1 = new TV();
        
        Remote remote1 = new Remote(tv1);
        System.out.println(tv.getVolume());
        remote1.increase();
        System.out.println(tv.getVolume());
        
        Fan fan1 = new Fan();
        System.out.println(fan1.getSpeed());
        
        Remote remote2 = new Remote(fan1);
        remote2.increase();
        
    }
}
```
This is okay, but we can go a step further:
- We forced a fan and a tv to comply to an appliance interface, maybe don't want to group them together
- We incremented the volume and the speed of the fan. What if we want to increment the brightness, or change what we want to control with the fan? The appliance interface doesn't give us this flexability 
- The code smells. What in the world is getValue() setValue()? We bolted on something artificially just to make this work.
- Also, our remote us using the public method of the appliance, as if the remote walks up to the TV and pushes the buttons that directly control the volume. This isn't really the case in reality, as the remote should be going through some other interface

```java
class TV  {
    private int volume;
    public int getVolume() {return volume;}
    public void setVolume(int newVolume){
        volume = newVolume;
    }
    public Remote getRemote(){
        return new Remote(){ // anonymous inner class
            @Override
            public void increase(){
                volume++;
            }
        };
    }
}

interface Remote{
    public void increase();
}

class Fan{
    private int speed;
    public int getSpeed(){
        return speed;
    }
    public void setSpeed(int newSpeed){
        speed = newSpeed;
    }

    public Remote getRemote(){
        return new Remote(){
            @Override
            public void increase() {
                speed++;
            }
        };
    }

}

public class Sample {
    public static void main (String[] args){
        TV tv1 = new TV();
        Remote remote1 = tv1.getRemote();
        remote1.increase();
        System.out.println(tv1.getVolume());

        Fan fan1 = new Fan();
        Remote remote2 = fan1.getRemote();
        remote2.increase();
        System.out.println(fan1.getSpeed());
    }
}
```
Advantages:
- Don't force Fan and TV to comply to a common interface
  - Let the tv return a remote and let the fan return a remote
- The user of these things depends on the interface of the remote
- The implementations are hidden, and the implentation is internal so it can directly talk to the class variables and no longer has to use public methods

We see the same thing in Java for iterators
```java
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;

public class Sample {
    public static void main (String[] args){
        List<Integer> numbers = Arrays.asList(1,2,3);

        for (int e : numbers){
            System.out.println(e);
        }

        Iterator<Integer> iterator = numbers.iterator(); //iterator is an interface
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}
```
Iterators better be efficient, you dont want to go to the public interface accessing values. Iterators are implemented internally, but you still get a common interface.

## Interface segregation principle
- Keep your interfaces very narrow and focused