## constructor chaining
[Constructor Chaining In Java with Examples - GeeksforGeeks](https://www.geeksforgeeks.org/constructor-chaining-java-examples/)
Constructor chaining is the process of calling one constructor from another constructor with respect to current object.
-  **Within same class**: It can be done using **this()** keyword for constructors in the same class
-  **From base class:** by using **super()** keyword to call the constructor from the base class.
```java
// Java program to illustrate Constructor Chaining
// within same class Using this() keyword
class Temp {
	// default constructor 1
	// default constructor will call another constructor
	// using this keyword from same class
	Temp() {
		// calls constructor 2
		this(5);
		System.out.println("The Default constructor");
	}

	// parameterized constructor 2
	Temp(int x) {
		// calls constructor 3
		this(5, 15);
		System.out.println(x);
	}

	// parameterized constructor 3
	Temp(int x, int y) {
		System.out.println(x * y);
	}

	public static void main(String args[]) {
		// invokes default constructor first
		new Temp();
	}
}

```

## inheritance
[Inheritance in Java - GeeksforGeeks]
```java
[<Modificator>] class <Class> extends <Superclass> {
}
//----------------------------------------------------
class A {
	public int x;
	public A(int x) { this.x = x; }
}

class B extends A {
	public int y;
	public B(int x, int y) { super(x); this.y = y; }
}

class C extends B {
	public int z;
	public C(int x, int y, int z) { super(x, y); this.z = z; }
}

public class MyClass {
	public static void main(String[] args) {
		C obj = new C(10, 20, 30);
		System.out.printf("x=%d; y=%d; z=%d", obj.x, obj.y, obj.z);
	}
}
```
- super(): Elternklasse zugreifen; Constructor von der Elternklasse aufrufen
- this(): Constructor von der Kindklasse aufrufen
## @Override
[Overriding in Java - GeeksforGeeks](https://www.geeksforgeeks.org/overriding-in-java/)
- Beim Überschreiben einer Methode kann es passieren, dass wir eine Methode mit demselben Namen, aber mit einer anderen Signatur erstellen. Das Ergebnis ist eine Methodenüberladung und nicht eine Überschreibung. 
- Um dem Compiler mitzuteilen, dass es sich um eine Überschreibung handelt, sollte die Annotation @Override vor der Methodenbeschreibung eingefügt werden
- In any object-oriented programming language, Overriding is a feature that allows a subclass or child class to provide a specific implementation of a method that is already provided by one of its super-classes or parent classes. When a method in a subclass has the same name, same parameters or signature, and same return type(or sub-type) as a method in its super-class, then the method in the subclass is said to _override_ the method in the super-class.
```java
class B extends A {
	@Override
	public void func() {
		System.out.println("B.func()");
		super.func; //Aufruf einer Methode der Basisklasse
	}
}
```
## access modifiers
(https://www.geeksforgeeks.org/inheritance-in-java/)
![[access modifiers java.png]]
### static
Erstellung durch die Deklaration einer statischen Klassenkonstante. Sie können 
- bei der Deklaration der Konstante oder 
- innerhalb eines statischen Initialisierungsblocks einen Anfangswert zuweisen. 
```java
public static final double PI = 3.14; 
```
Ein Zugriff auf statische Konstanten außerhalb der Klasse kann erfolgen, **ohne eine Instanz der Klasse zu erzeugen**. 
```java
System.out.println(Class1.PI); 
```
Es ist auch möglich, über eine Klasseninstanz zu adressieren:
```java
Class c = new Class(10); System.out.println(c.PI); // Besser Class.PI verwenden
```
>[!NOTE] Statische Klassenmitglieder
>- Sie können eine Variable, Konstante oder Methode innerhalb einer Klasse erstellen, auf die Sie zugreifen können, ohne eine Instanz der Klasse zu erstellen. Geben Sie dazu das Schlüsselwort static an, bevor Sie die Variable, Konstante oder Methode deklarieren. 
>- Statische Klassenmitglieder existieren in einer einzigen Instanz, unabhängig von der Anzahl der erstellten Objekte. 
>- Beachten Sie, dass Sie innerhalb der statischen Methode nicht auf reguläre Felder zugreifen können, sondern nur auf die statischen Klassenmitglieder.
>- Sie können einen Anfangswert zuweisen, wenn Sie eine Variable (oder Konstante) deklarieren oder innerhalb eines statischen Initialisierungsblocks. 
>- Statische Klassenvariablen werden automatisch initialisiert, wenn ihnen noch keine Anfangswerte zugewiesen wurden. 
>- Ganzzahlige Variablen werden mit dem Wert 0 initialisiert, reelle Variablen mit dem Wert 0.0, logische Variablen mit dem Wert false und Objektvariablen mit dem Wert null.

### public
Erstellung durch Deklaration einer nicht-statischen Klassenkonstante. Jede Instanz einer Klasse kann ihren eigenen konstanten Wert haben. Sie können einen Anfangswert zuweisen, 
- wenn Sie eine Konstante deklarieren, 
- innerhalb eines nicht-statischen Initialisierungsblocks oder 
- innerhalb eines Klassenkonstruktors. 
```java
public final int MY_CONST = 3; 
```
Auf eine Klassenkonstante kann außerhalb der Klasse zugegriffen werden, nachdem eine Instanz der Klasse erstellt wurde. 
```java
Klasse c = new Class(10); 
System.out.println(c.MY_CONST); 
```
Innerhalb der Klasse können Sie nur den Namen der Konstante oder ``this.<Konstante>`` angeben.
## enum
- An `enum` is a special "class" that represents a group of **constants** (unchangeable variables, like `final` variables).
- To create an `enum`, use the `enum` keyword (instead of class or interface), and separate the constants with a comma. Note that they should be in uppercase letters:
```java
[<Zugriffkodierer>] enum <Enum-Name> {
	<Komma-getrennte Liste von Konstanten>
}

enum Color { RED, BLUE, GREEN, BLACK }
Color color1, color2;
color1 = Color.RED;
color2 = Color.BLACK;
color3 = null;
```
>[!NOTE] Enum notes
>#### Difference between Enums and Classes
>- An `enum` can, just like a `class`, have attributes and methods. The only difference is that enum constants are `public`, `static` and `final` (unchangeable - cannot be overridden).
>- An `enum` cannot be used to create objects, and it cannot extend other classes (but it can implement interfaces).
>#### Why And When To Use Enums?
>- Use enums when you have values that you know aren't going to change, like month days, days, colors, deck of cards, etc.

## abstract classes
- Eine abstrakte Klasse vereint die Eigenschaften einer regulären Klasse mit denen eines Interfaces: Manchmal ist nicht nur, wie bei einem Interface, klar, welche Funktionalität alle implementierenden Klassen anbieten sollen, sondern für einen Teil dieser Funktionalität ist auch klar, dass diese auf die gleiche art implementiert sein wird. 
- Nehmen wir beispielsweise ein Programm zum Setzen von Texten, das unterschiedliche Typen von Aufzählungen unterstützen können soll. Zu jeder Aufzählung müssen Elemente hinzugefügt und entfernt werden können, und es müssen die vorhandenen Elemente entsprechend formatiert ausgegeben werden können.

## generics
- Bei der Implementierung einer generischen Klasse werden die Datentypen ihrer Variablen nicht endgültig festgelegt, sondern statt dessen als Platzhalter sog. Typparameter angegeben.
- **Beispiel:** ArrayList\<Integer> arrayList = new ArrayList() --> Die Klasse ArrayList ist eine generische Klasse und funktioniert mit verschiedenen Datentypen z.B Integer, String, usw. Sie wird so implementiert: class ArrayList\<T> (T: Typparameter)
```java
//EINSCHRÄNKUNGEN FÜR TYPPARAMETER
class A {
	public void test() { System.out.println("Test A");}
}
class B extends A {
	@Override
	public void test() { System.out.println("Test B");}
}
class ClassA<T extends A> { //Einschränkung für Typparameter aus eine Basisklasse
							// funktioniert mit den Klassen A, B
	public T obj;
	public ClassA(T obj) {
		this.obj = obj;
	}
	public void test() {
		this.obj.test();
	}
}

// und auch:
interface ITest1 { void test1(); }
interface ITest2 { void test2(); }
class test implements ITest1, ITest2 {
	@Override
	public void test1() { System.out.println("ITest 1");}
	@Override
	public void test2() { System.out.println("ITest 2");}
}
class ClassI<T extends ITest1 & ITest2> { //Einschränkungen für Typparameter auf 2 Interfaces --> die Klasse verlangt die Funktionen test1() und test2() aus diesen Interfaces
	public T obj;
	public ClassI(T obj) {
		this.obj = obj;
	}
	public void test() {
		this.obj.test1(); //Aus ITest1
		this.obj.test2(); //Aus ITest2
	}
}
```
- Erweiterung:
	- [java - Why is "extends T" allowed but not "implements T"? - Stack Overflow](https://stackoverflow.com/questions/976441/why-is-extends-t-allowed-but-not-implements-t)
	- [What is the difference between 'E', 'T', and '?' for Java generics? - Stack Overflow](https://stackoverflow.com/questions/6008241/what-is-the-difference-between-e-t-and-for-java-generics)
	- pair-Klasse() -> 2 Werte zurückgeben
 ```java
public class MyClass {
	public static void main(String[] args) {
		System.out.println(func().getX()); //10
		System.out.println(func().getY()); //String 1
	}
	public static Box<Integer, String> func() {
		return new Box<Integer, String>(10, "String 1");
	}
}

class Box<T1, T2> {
	private T1 x;
	private T2 y;
	public Box(T1 x, T2 y) {
		this.x = x;
		this.y = y;
	}
	public T1 getX() {
		return x;
	}
	public T2 getY() {
		return y;
	}
}
```

## threads
[[Java threads]]

## abstract classes and methods
- Abstract methods contains only a method declaration without implementation
- Abstract classes cannot be instantiated
- When a class has an abstract method, this class must be declared as abstract
- A class can be declared as abstract, even when it doesn't contain an abstract method
- Abstract methods must be overriden and implemented by the child class
```java
public class MyClass {
	public static void main(String[] args) {
		B obj = new B();
		obj.func();
		obj.print();
	}
}

abstract class A {
	public abstract void func();
	public void print() {
		System.out.println("A.print()");
	}
}

class B extends A {
	@Override
	public void func() {
		System.out.println("B.func()");
	}
}
```

## interface
- Ein Interface ähnelt einer abstrakten Klasse und enthält ebenfalls nur eine Methodensignatur und keine Implementierung. 
- Ein Interface ist jedoch keine Klasse und es ist nicht möglich, eine Instanz eines Interfaces zu erstellen. 
- Eine Klasse, die ein Interface implementiert, stellt sicher, dass in ihr Methoden definiert sind, die im Interfaceblock beschrieben sind. 
- Wenn eine Klasse nicht mindestens eine Methode umdefiniert, wird sie zu einer abstrakten Klasse und es kann keine Instanz dieser Klasse erstellt werden.
- [oop - Interface vs Abstract Class (general OO) - Stack Overflow](https://stackoverflow.com/questions/761194/interface-vs-abstract-class-general-oo)
- [oop - When to use an interface instead of an abstract class and vice versa? - Stack Overflow](https://stackoverflow.com/questions/479142/when-to-use-an-interface-instead-of-an-abstract-class-and-vice-versa)
- [interface vs abstract class : Java Glossary (mindprod.com)](https://www.mindprod.com/jgloss/interfacevsabstract.html)
-----------------
- Ein Interface ist ähnlich wie eine Klasse, allerdings implementiert es keine Logik - es deklariert nur Methoden. 
- Eine Klasse, die von einem Interface erbt (hier verwendet man den Begriff `implements` und nicht `extends`), verpflichtet sich, alle diese Methoden auch zu implementieren. 
- Wenn eine Klasse von mehreren Interfaces erbt, dann ist sichergestellt, dass sie alle darin beschriebenen Methoden hat, aber es kann keine Konflikte geben, weil die Interfaces nicht vorschreiben, was in diesen Methoden passieren soll.
```
All Implemented Interfaces:
    Serializable, Cloneable, Iterable<E>, Collection<E>, Deque<E>, List<E>, Queue<E> 
```
- Das ist die Liste der Interfaces, die `LinkedList` implementiert - also die Liste der Funktionalität, die es garantiert, anzubieten
- Some interfaces: Iterable\<T>, Iterable\<T>, Comparable\<T>, Comparator\<T>

## observer pattern
- 2 Arten von Objekten: Beobachter und Subjekt
- Idee:
	- Beobachter interessieren sich für ein Subjekt und melden sich an, um Benachrichtigungen von diesem Objekt zu erhalten. 
	- Wenn sich der Zustand des Objekts ändert, werden alle Beobachter automatisch benachrichtigt.
	- Verlieren sie daran ihr Interesse, melden sie sich einfach ab.

## functional interfaces
- interface mit nur einer Klasse

## lambda functions
[The Double Colon Operator in Java 8 | Baeldung](https://www.baeldung.com/java-8-double-colon-operator)
[Guide to the Java 8 forEach | Baeldung](https://www.baeldung.com/foreach-java)
```java
//Uebungsblatt 12
switch(op) {
	case 1:
		System.out.ptrintln("Ausgabe aller Kunden");
		                   //lambda shorthand
		allClients.forEach(System.out::println);
		                   //was kommt in die Methode rein
		                            //was macht die Methode
		allClients.forEach(client -> System.out.println(client));
		allClients.forEach(new Constumer<Client>() {
			@Override
			public void accept(Client c) {
				System.out.println(client);
			}
		})
	case 2:
		allClients.forEach(client -> {if (client.getSex()==Client.Sex.male && client.getSmoker()) System.out.println(client)})
}
```

```java
//class in function
Collections.sort(allClients, new Comparator<Client>() {
	@Override
	public int compare(Client c1, Client c2) {
		if (c1.getName().compareToIgnoreCase(c2.getName) > 0) return 1;
		else if (c1.getName().compareToIgnoreCase(c2.getName) < 0) return -1;
		return 0;
	}
})

//lambda expression
Collections.sort(allClients, (Client c1, Client c2) -> Double.compare(c1.getIncome(), c2.getIncome()))

//lambda shorthand
Collections.sort(allClients, Comparator.comparingInt(Customer::getId));  

//separate class
//Collections.sort(aList, new ComparatorClassA());
static class sortByIncomeClass implements Comparator<Customer> {  
    @Override  
    public int compare(Customer c1, Customer c2) {  
        return c1.getIncome() - c2.getIncome();  
    }  
}  
Collections.sort(allClients, new sortByIncomeClass());
```