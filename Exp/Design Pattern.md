# Singleton Pattern
- Singleton pattern is a convention for ensuring **one and only one object is instatiated** for a given class and provides a global point of access to it.
- Why do we need Singleton Pattern instead of global variables ? 
-> To reduce resource intensive global varibles. Only instantiate them when they are needed

``` java
public class Singleton {
	// a static variable to hold one instance of the class Singleton
	private static Singleton uniqueSingleton = new Singleton();
	// private constructor 
	private Singleton() {}  
	public static Singleton getInstance(){
		return uniqueInstance;
	}
}
```
# Factory Pattern
- The Factory Method Pattern defines **an interface for creating an object**, but **lets subclasses decide which class to initiate**. Factory Method lets a class defer instantiation to subclasses.

![[Screenshot from 2023-05-21 12-00-01.png]]
- The Abstract Factory Pattern provides an interface for creating families of related dependent objects without specifying their concrete classes.

# Proxy Pattern

# Decorator Pattern