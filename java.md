# Java Technical Round Questions

1. What is `String Immutability`?
- Java String is immutable which means it cannot be changed.
  > Whenever we change any string, a new instance is created.
- For mutable strings, you can use StringBuffer and StringBuilder classes.
2. How to create an `immutable class` in Java?
- Declare the class as final so it can’t be extended.
- Make all fields private so that direct access is not allowed.
- Don’t provide setter methods for variables
- Make all mutable fields final so that it’s value can be assigned only once.
- Initialize all the fields via a constructor performing deep copy.
  > Read about Shallow vs Deep copy.
- Perform cloning of objects in the getter methods to return a copy rather than returning the actual object reference.

```java
import java.util.HashMap;
import java.util.Iterator;
public final class FinalClassExample {
	private final int id;
	private final String name;
	private final HashMap<String,String> testMap;

	public int getId() {
		return id;
	}
	public String getName() {
		return name;
	}
	public HashMap<String, String> getTestMap() {
		//return testMap;
		return (HashMap<String, String>) testMap.clone();
	}

	public FinalClassExample(int i, String n, HashMap<String,String> hm){
		this.id=i;
		this.name=n;
    System.out.println("Performing Deep  for Object initialization");
		HashMap<String,String> tempMap=new HashMap<String,String>();
		String key;
		Iterator<String> it = hm.keySet().iterator();
		while(it.hasNext()){
			key=it.next();
			tempMap.put(key, hm.get(key));
		}
		this.testMap=tempMap;
	}

	public static void main(String[] args) {
		HashMap<String, String> h1 = new HashMap<String,String>();
		h1.put("1", "first");
		h1.put("2", "second");
		String s = "original";
		int i=10;

		FinalClassExample ce = new FinalClassExample(i,s,h1);

		//Lets see whether its copy by field or reference
		System.out.println(s==ce.getName()); // true
		System.out.println(h1 == ce.getTestMap()); // false
		//print the ce values
		System.out.println("ce id:"+ce.getId()); // 10
		System.out.println("ce name:"+ce.getName()); // original
		System.out.println("ce testMap:"+ce.getTestMap()); // {2=second, 1=first}
		//change the local variable values
		i=20;
		s="modified";
		h1.put("3", "third");
		//print the values again
		System.out.println("ce id after local variable change:"+ce.getId()); // 10
		System.out.println("ce name after local variable change:"+ce.getName()); // original
		System.out.println("ce testMap after local variable change:"+ce.getTestMap()); // {2=second, 1=first}

		HashMap<String, String> hmTest = ce.getTestMap(); // we returned testMap's clone not reference
		hmTest.put("4", "new");

		System.out.println("ce testMap after changing variable from accessor methods:"+ce.getTestMap()); // {2=second, 1=first}
	}
}
```
3. What is difference between `deep` and `shallow` copy?
- > `Shallow copies` duplicate as little as possible. A shallow copy of a collection is a copy of the collection structure, not the elements. With a shallow copy, two collections now share the individual elements.

- > `Deep copies` duplicate everything. A deep copy of a collection is two collections with all of the elements in the original collection duplicated.
4. What are the advantages of `string immutability`?
- > Being immutable guarantees that `hashcode` will always the same, so that it can be cached without worrying the changes.That means, there is no need to calculate hashcode every time it is used. This is more efficient.
5. What are different `access specifiers` of Java Class?
- `public`
  > accessible to any class in any package
- `default` (when no access specifier is specified)
  > accessible only to classes inside the same package
- Nested interfaces and classes can have all access specifiers(public, private, protcted, default)
6. What are different `access specifiers` in Java?
- `public`
  > accessible to any class in any package
- `default` (when no specifier is mentioned)
  > accessible only to classes inside the same package
- `private`
  > accessible to only that particular class in which it is defined.
- `protected`
  > accessible to its particular class and by its subclass.
7. How to write `nested` interfaces and classes in Java?
8. What is difference between `abstract` class and `interface`?
- > Main difference is methods of a Java interface are implicitly abstract and cannot have implementations. A Java abstract class can have instance methods that implements a default behavior.
- > Variables declared in a Java interface is by default final. An  abstract class may contain non-final variables.
- > Members of a Java interface are public by default. A Java abstract class can have the usual flavors of class members like private, protected, etc..
- > Java interface should be implemented using keyword “implements”; A Java abstract class should be extended using keyword “extends”.
- > An interface can extend another Java interface only, an abstract class can extend another Java class and implement multiple Java interfaces.
- > A Java class can implement multiple interfaces but it can extend only one abstract class.
- > Interface is absolutely abstract and cannot be instantiated; A Java abstract class also cannot be instantiated, but can be invoked if a main() exists.
- > In comparison with java abstract classes, java interfaces are slow as it requires extra indirection.
9. What is the use of `final` keyword in Java?
10. Write a Java code to demonstrate `dynamic method dispatch`.
11. Can `static`, `private`, `final` methods be overridden in Java?
12. How will you achieve `Multiple Inheritance` in JAVA?
- > We CANNOT extend two objects, but we can `implement` two interfaces. Interfaces don't simulate multiple inheritance. Java creators considered multiple inheritance wrong, so there is no such thing in Java.
13. What is `static` class in Java?
- > `Static` classes are basically a way of grouping classes together in Java. Java doesn't allow you to create top-level static classes; only nested (inner) static classes. A static inner class is a nested class which is a static member of the outer class. It can be accessed without instantiating the outer class, using other static members. Just like static members, a static nested class does not have access to the instance variables and methods of the outer class.
14. What is the difference between `inner class` and `nested class`?
15. Explain final class, final method, final varaible.
- > `final with class` - The class cannot be subclassed. Whenever we declare any class as final, it means that we can’t extend that class or that class can’t be extended or we can’t make subclass of that class.
- > `final with method` - The method cannot be overridden by a subclass. Whenever we declare any method as final, then it means that we can’t override that method.
- > `final with variables` - The value of variable cannot be changed once intialized. If we declare any variable as final, we can’t modify its contents since it is final, and if we modify it then we get Compile Time Error.
`Note` : If a class is declared as final then `by default` all of the methods present in that class are automatically final but `variables are not`.
16. What is `singleton` class? Write a code to explain how does it work.
17. When can you override `clone method` of Object class?
18. What is `tagging interface`?
19. What is an `extensible framework`?
20. Why JAVA is `platform independent`?
21. Can we have `multiple constructors` in a class. How to implement it?
- > Yes, by constructor overloading.
22. What is `super` and `this`?
23. What is the difference between `String Builder` and `String Buffer`?
- Buffer is `thread safe`, but the other is not.
24. Explain internal working of `Hash-map` in java.
25.
