# 67-68_Structural_Patterns
- Concerned with how classes are related to each other, and how objects collaborate
- **Class patterns **use **inheritance** to compose interfaces or implementation. **Object patterns** describe ways to **compose** object to realize new functionality.

## Adapter
- Both in class scope and object scope
- "Afterthought pattern", as it comes in handy when you are integrating your code with another system.
- Convert the interface of a class into another interface that clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

Say we have a computer that uses an abstract memory that supports a given interface Memory that stores a byte and address in one write method.
We want to be able to use Memory2 that has a setAddr() method and a write() method.
```java
public class Computer{
    private Memory _memory;
    public Computer(Memory memory){
        _memory = memory;
    }
    public void work(){
        _memory.store(11111. (byte) 0);
    }
}

public interface Memory{
    void store(){int addr, byte value};
}

public class Memory1 implements Memory{
    @Override
    public void store(int adr, byte value){
        System.out.println("storing in memory1");
    }
}

//3rd party class we are integrating with
public class Memory2{
    public void setAddr(int addr){
    }
    public void write(byte value){
    }
}

public class Sample{
    public static void main(String[] args){
        Computer computer = new Computer(new Memory1());
        computer.work();
    }
}
```
Class Adapter uses inheritance. We wrap Memory.write() around Memory2.setAddr and Memory2.write()
```java
public class Memory2Adapter1 extends Memory2 implements Memory{
    @Override
    public void store(int adr, byte value){
        super.setAddr(addr);
        super.write(value);
    }
}

public class Sample{
    public static void main(String[] args){  
        Computer computer2 = new Computer(new Memory2Adapter1());
        computer2.work();
    }
}
```
Consequences:
- Languages like Java and C# do not support multiple inheritance like C++, so if memory was not an interface then we could not use the class adapter pattern

Object adapter uses aggregation/delegation
```java
public class Memory2Adapter2 implements Memory {
    private Memory2 _memory2;
    public Memory2Adapter2(Memory2 memory2){
        _memory2 = memory2;
    }
    @Override
    public void store(int addr, byte value){
        _memory2.setAddr(addr);
        _memory2.write(value);
    }
}

public class Sample{
    public static void main(String[] args){  
        Computer computer3 = new Computer(new Memory2Adapter2(new Memory2()));
        computer2.work();
    }
}
```
If memory2 had been a final class, then our class adapter (Memory2Adapter1) would stop working, but our object adapter (Memory2Adapter2) would continue to work. So object adapters are a little bit more flexible.

Adapter can also be used proactively as a forethought in testing, where you want to mock a service that will eventually be replaced by a real service

Class adapter:
- Adapts adaptee to target by committing to a concrete adapter class
- Can't adapt a class and all its sublasses
- Lets adapter override some of adaptee's behavior
- No extra object due to adapter usage

Object adapter:
- Let's a single adapter work with many adaptees - a class and its subclasses
- Can add functionaliuty to all adaptees
- Harer to override adaptee behavior

Adapter vs other interfaces:
- Structure is similar to bridge but with different intent
  - Bridge: seperate interface from implementation
  - Adapter: change the interface
- Decorator enhances another object without changing interface