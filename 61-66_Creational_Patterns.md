# 61-66_Creational_Patterns
## Abstract Factory
Object based
## Factory
Class based
## Prototype
Object based
## Singleton
- Object based
- One of the simplest patterns, yet one of the most difficult to implement
- Ensures that a class only has one single instance and provides a global point of access to that one instance (or a limited amount of instances)
Example:  An application that uses a database manager, it really only needs one object to control the connection to the database

```java
public class DBManager {
private static DBManager _instance = new DBManager();
    private DBManager(){
    }
    public static DBManager getInstance(){
        return _instance;
    }
    @Override
    public String to String(){
        return String.format("%s %d", getClass(), hashCode());
    }
}

public class Sample{
    public static void main(String[] args) {
        DBManager dbManager = DBManager.getInstance();
        System.out.println(dbManager);
    }
}
```
However, in Java we can easily go around this using reflection, and is also not thread safe. Implementing a singleton as an enum takes care of serilization and thread safety for us, but we cannot extend it later on.

## Builder
- Object based
- Helps to streamline or standarize object creation while allowing each step of creation to vary
- To perserve the family of products use abstract factory. To preserve the steps in the creation use the builder pattern.
- Here the pizza builder preserves all the steps of making a pizza, but we can vary the details in the implementation. The Director is standarized, but the builder changes

```java
public class Sauce{
}

public class Crust{
}

public class Pizza {
    private Crust _crust;
    private Sauce _sauce;
    private String _cheese;
    private String[] _toppings;
    
    public void createCrust(){
        _crust = new Crust();
    }
    @Override
    public String to String(){
        String toppings = Stream.of(_toppings).collect(joining(","));
        return " " + _crust + " " + _sauce + " " + _cheese + " " + toppings;
    }
    public void spreadSauce() {
        _sauce = new Sauce();
    }
    public void addCheese(){
        _cheese = "Mozzarrella";
    }
    public void addToppings(String... toppings){
        _toppings = toppings;
    }
    public void Bake(){
        System.out.println("Baking");
    }
}

public abstract class PizzaBuilder {
    private Pizza _pizza = new Pizza();
    protected Pizza getPizza(){
        return _pizza;
    }
    public void createCrust(){
        _pizza.createCrust();
    };
    public abstract void spreadSauce();
    public abstract void addCheese();
    public abstract void addToppings();
    public abstract void bake();
    public Pizza create() 
}

public class CheesePizzaBuilder extends PizzaBuilder {
    @Override
    public void spreadSauce(){
        getPizza().spreadSauce();
    }
    @Override
    public void addCheese(){
        getPizza().addCheese();
    }
    @Override
    public void addToppings(){
        getPizza().addToppings("cheese");
    }
    @Override
    public void bake(){
        getPizza().bake();
    }
}

class PizzaDirector{
    public void makePizza(PizzaBuilder pizzaBuilder){
        System.out.println("creating crust");
        pizzaBuilder.createCrust();
        pizzaBuilder,spreadSauce();
        pizzaBuilder.addCheese();
        pizzaBuilder.addToppings("cheese");
        pizzaBuilder.bake();
    }
}

public class Sample{
    public static void main(String[] args) {
        PizzaDirector pizzaDirector = new PizzaDirector();
        PizzaBuilder cheesePizzaBuilder = new CheesePizzaBuilder();
        pizzaDirector.makePizza(cheesePizzaBuilder);
    }
}
```
