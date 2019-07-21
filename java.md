# Java Technical Round Questions
<mark style="background-color: lightblue">Marked text</mark>

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
3. What is defference between `deep` and `shallow` copy?
4. What are the advantages of `string immutability`?
5. What are different `access specifiers` of Java Class?
- `public`
  > accessible to any class in any package
- `default` (when no access specifier is specified)
  > accessible only to classes inside the same package
- Nested interfaces and classes can have all access specifiers(public, private, protcted, default)
6. What are different `access specifiers` in Java?
7. How to write `nested interfaces and classes` in Java?
8. What is difference between `abstract` class and `interface`?
9. What is the use of `final` keyword in Java?
