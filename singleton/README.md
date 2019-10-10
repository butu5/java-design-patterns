---
layout: pattern
title: Singleton
folder: singleton
permalink: /patterns/singleton/
categories: Creational
tags:
 - Java
 - Gang Of Four
 - Difficulty-Beginner
---

## Intent
Ensure a class only has one instance, and provide a global point of
access to it.


## Explanation
Real world example

> There can only be one ivory tower where the wizards study their magic. The same enchanted ivory tower is always used by the wizards. Ivory tower here is singleton.

In plain words

> Ensures that only one object of a particular class is ever created.

Wikipedia says

> In software engineering, the singleton pattern is a software design pattern that restricts the instantiation of a class to one object. This is useful when exactly one object is needed to coordinate actions across the system.

**Programmatic Example**

Joshua Bloch, Effective Java 2nd Edition p.18

> A single-element enum type is the best way to implement a singleton

```java
public enum EnumIvoryTower {
  INSTANCE;
}
```

Then in order to use

```java
EnumIvoryTower enumIvoryTower1 = EnumIvoryTower.INSTANCE;
EnumIvoryTower enumIvoryTower2 = EnumIvoryTower.INSTANCE;
assertEquals(enumIvoryTower1, enumIvoryTower2); // true
```

## Applicability
Use the Singleton pattern when

* there must be exactly one instance of a class, and it must be accessible to clients from a well-known access point
* when the sole instance should be extensible by subclassing, and clients should be able to use an extended instance without modifying their code

## Typical Use Case

* the logging class
* managing a connection to a database
* file manager

## Real world examples

* [java.lang.Runtime#getRuntime()](http://docs.oracle.com/javase/8/docs/api/java/lang/Runtime.html#getRuntime%28%29)
* [java.awt.Desktop#getDesktop()](http://docs.oracle.com/javase/8/docs/api/java/awt/Desktop.html#getDesktop--)
* [java.lang.System#getSecurityManager()](http://docs.oracle.com/javase/8/docs/api/java/lang/System.html#getSecurityManager--)


## Consequences

* Violates Single Responsibility Principle (SRP) by controlling their own creation and lifecycle.
* Encourages using a global shared instance which prevents an object and resources used by this object from being deallocated.     
* Creates tightly coupled code. The clients of the Singleton become difficult to test.
* Makes it almost impossible to subclass a Singleton.

## Credits

* [Design Patterns: Elements of Reusable Object-Oriented Software](http://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612)
* [Effective Java (2nd Edition)](http://www.amazon.com/Effective-Java-Edition-Joshua-Bloch/dp/0321356683)




```java
public enum EnumIvoryTower {

  INSTANCE;

  @Override
  public String toString() {
    return getDeclaringClass().getCanonicalName() + "@" + hashCode();
  }
}
```

```java
/**
 * Singleton class. Eagerly initialized static instance guarantees thread safety.
 */
public final class IvoryTower {

  /**
   * Private constructor so nobody can instantiate the class.
   */
  private IvoryTower() {}

  /**
   * Static to class instance of the class.
   */
  private static final IvoryTower INSTANCE = new IvoryTower();

  /**
   * To be called by user to obtain instance of the class.
   *
   * @return instance of the singleton.
   */
  public static IvoryTower getInstance() {
    return INSTANCE;
  }
}
```

```java

/**
 * The Initialize-on-demand-holder idiom is a secure way of creating a lazy initialized singleton
 * object in Java.
 * <p>
 * The technique is as lazy as possible and works in all known versions of Java. It takes advantage
 * of language guarantees about class initialization, and will therefore work correctly in all
 * Java-compliant compilers and virtual machines.
 * <p>
 * The inner class is referenced no earlier (and therefore loaded no earlier by the class loader) than
 * the moment that getInstance() is called. Thus, this solution is thread-safe without requiring special
 * language constructs (i.e. volatile or synchronized).
 *
 */
public final class InitializingOnDemandHolderIdiom {

  /**
   * Private constructor.
   */
  private InitializingOnDemandHolderIdiom() {}

  /**
   * @return Singleton instance
   */
  public static InitializingOnDemandHolderIdiom getInstance() {
    return HelperHolder.INSTANCE;
  }

  /**
   * Provides the lazy-loaded Singleton instance.
   */
  private static class HelperHolder {
    private static final InitializingOnDemandHolderIdiom INSTANCE =
        new InitializingOnDemandHolderIdiom();
  }
}
```


```java
/**
 * Thread-safe Singleton class. The instance is lazily initialized and thus needs synchronization
 * mechanism.
 *
 * Note: if created by reflection then a singleton will not be created but multiple options in the
 * same classloader
 */
public final class ThreadSafeLazyLoadedIvoryTower {

  private static ThreadSafeLazyLoadedIvoryTower instance;

  private ThreadSafeLazyLoadedIvoryTower() {
    // protect against instantiation via reflection
    if (instance == null) {
      instance = this;
    } else {
      throw new IllegalStateException("Already initialized.");
    }
  }

  /**
   * The instance gets created only when it is called for first time. Lazy-loading
   */
  public static synchronized ThreadSafeLazyLoadedIvoryTower getInstance() {
    if (instance == null) {
      instance = new ThreadSafeLazyLoadedIvoryTower();
    }

    return instance;
  }
}
```
