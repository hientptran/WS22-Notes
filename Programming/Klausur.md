## Resources:
- [dabrowskiw/Skript-Programmieren2 (github.com)](https://github.com/dabrowskiw/Skript-Programmieren2)

## Themen
- Klasse, Variable, Methode
- Verkapselung, Vererbung und Polymorphismus
- Typumwandlung/konvertierung, innere Klassen
- Abstrakte Klassen und Methoden, Interface
- Generische Typen
- Interface Comparable, Comparator
- Funktionale Interfaces und Lambda-Ausdrücke
- Observer Pattern
- Ereignisbehandlung
- Callback
- Ereignisbehandlung

## Fragen
- Unterschied zwischen Klasse und Objekt?
	- A class is a template used for the creation of objects
	- An object is an instance of a class
- Erklärungsfragen (Konzepte, usw.)
- Funktion vs Methode
	- Function — a set of instructions that perform a task
	- Method — a set of instructions that are associated with an object (verknüpft an der Klasse)

## Notizen
**implements** -> Interface
**extends** -> (abstract) class
@**Override** -> Funktion schon in der Parent-Class ist (schon implementiert/existiert), aber wird in der neuen Klasse anders implementiert
**Objekterzeugung**: Klasse - Variablename - neue Instanz von Klassenkonstruktor(Parameter)

-----------
## Generische Typen
- Bei der Implementierung einer generischen Klasse werden die Datentypen ihrer Variablen nicht endgültig festgelegt, sondern statt dessen als Platzhalter sog. Typparameter angegeben.
- **Beispiel:** ArrayList\<Integer> arrayList = new ArrayList() --> Die Klasse ArrayList ist eine generische Klasse und funktioniert mit verschiedenen Datentypen z.B Integer, String, usw. Sie wird so implementiert: class ArrayList\<T> (T: Typparameter)
```java
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

## Interfaces
- Ein Interface ist ähnlich wie eine Klasse, allerdings implementiert es keine Logik - es deklariert nur Methoden. 
- Eine Klasse, die von einem Interface erbt (hier verwendet man den Begriff `implements` und nicht `extends`), verpflichtet sich, alle diese Methoden auch zu implementieren. 
- Wenn eine Klasse von mehreren Interfaces erbt, dann ist sichergestellt, dass sie alle darin beschriebenen Methoden hat, aber es kann keine Konflikte geben, weil die Interfaces nicht vorschreiben, was in diesen Methoden passieren soll.
```
All Implemented Interfaces:
    Serializable, Cloneable, Iterable<E>, Collection<E>, Deque<E>, List<E>, Queue<E> 
```
- Das ist die Liste der Interfaces, die `LinkedList` implementiert - also die Liste der Funktionalität, die es garantiert, anzubieten
- Some interfaces: Iterable\<T>, Iterable\<T>, Comparable\<T>, Comparator\<T>

## Abstrakte Klassen
- Eine abstrakte Klasse vereint die Eigenschaften einer regulären Klasse mit denen eines Interfaces: Manchmal ist nicht nur, wie bei einem Interface, klar, welche Funktionalität alle implementierenden Klassen anbieten sollen, sondern für einen Teil dieser Funktionalität ist auch klar, dass diese auf die gleiche art implementiert sein wird. 
- Nehmen wir beispielsweise ein Programm zum Setzen von Texten, das unterschiedliche Typen von Aufzählungen unterstützen können soll. Zu jeder Aufzählung müssen Elemente hinzugefügt und entfernt werden können, und es müssen die vorhandenen Elemente entsprechend formatiert ausgegeben werden können.


