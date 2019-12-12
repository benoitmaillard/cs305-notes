# Design patterns recap
## Observer pattern
### Observer class
```java
public interface Observer {
    void update(T data);
}
```
### Subject class
```java
public abstract class Subject {
    private final List<Observer> observers = new LinkedList<>();

    public void registerObserver(Observer o) {
        observers.add(o);
    }

    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    public void notifyObservers(T data) {
        for (Observer o: observers) {
            o.update(data);
        }
    }
}
```

## Builder pattern
### Builder class
```java
public class SomethingBuilder {
    private int attribute1 = 0;
    private String attribute2 = "";
    ...

    public SomethingBuilder setAttribute1(int v) {
        attribute1 = v;
        return this; // allows to chain calls
    }

    ...

    public Something build() {
        checkValidity(...);
        return Something(attribute1, attribute2, ...);
    }
} 
```

## Adapter pattern
### Adapter class
```java
// We want to adapt a New interface to Old interface
public class NewToOldAdapter implements Old {
    private New underlyingObject = new NewImpl();

    @Override
    public A doSomething(B message) {
        return underlyingObject.doSomethingInASlightlyDifferentWay(message, 123);
    }

    ...
}
```

## Factory pattern
### Static factory method
```java
abstract public class Random {
    public static Random createRandomType1 {
        Random random = new RandomType1(...);
        ...
        return random;
    }

    public static Random createRandomType2 {
        ...
    }
}
```

### Abstract factory
The goal is to have an object that "stores" parameters. These parameters
can then be used to create as many instances as we want in an elegant way.

```java
public class RandomFactory {
    private T1 arg1;
    private T2 arg2;
    ...

    public RandomFactory(T1 arg1, T2 arg2, ...) {
        this.arg1 = arg1;
        this.arg2 = arg2;
        ...
    }

    public Random createRandom() {
        return new Random(arg1, arg2, ...)
    }
}
```

## Singleton pattern
### Classic version
```java
public class Random {
    private static Random instance = new Random();

    /* important to actually declare a private constructor
    so that there is no YOLO default public constructor */
    private Random() {
        if (instance != null)
            throw new IllegalStateException();
    }

    public getInstance() {
        return instance;
    }

    ...
}
```

### Enum version
```java
public enum Random {
    INSTANCE;

    public getInstance() {
        return INSTANCE;
    }

    ...
}
```

## Proxy pattern
The goal of the Proxy pattern is to intercept
methods calls of some object in order to perform verifications,
fetch resources, monitor actions, etc.

```java
public class ProxyRandom implements Random {
    private Random random = new RealRandom();

    @Override
    public someMethod(...) {
        // perform some checks or other things
        doSomething(...)

        // forward the call to underlying object
        random.someMethod();
        ...
    }
    ...
}
```