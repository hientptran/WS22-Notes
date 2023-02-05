### Struktur des Programms
```java
public class myClass {
	public static void main(String[] args) {
		System.out.println("hello");
	}
}
```

### Zugriffsmodifikatoren
- public: Klasse öffentlich, jedes andere Objekt auf die Klasse zugreifen, eine Instanz der Klasse erzeugen und auf öffentliche Methoden und Felder zugreifen kann
- private:
- protected

### Klasse
- **Klasseninstanz**: ein Objekt, das aus einer Klasse erstellt wird --> Klasse ist ein Schema/Blueprint. Es kann viele Instanzen einer Klasse geben, und jede Instanz kann ihre eigenen Eigenschaften haben, unabhängig von den Eigenschaften der anderen Instanzen
- **static**: Wir können über den Punktoperator auf die Methode verweisen, ohne eine Instanz der Klasse erstellen zu müssen

### Kommentare
```java
//einzeiglig

/*
Mehrzeilig
hello world
*/

int x; 
x = 10 /* Kommentar */ + 50 /* innerhalb des Ausdrucks */;
```

### Datenausgabe
- **System.out, System.err**: werden zur Ausgabe normaler Meldungen in das Konsolenfenster bzw. Fehlermeldungen in Java verwendet --> Geben eine Instanz der Klasse PrintStream zurück,über die die folgenden grundlegenden Methoden verfügbar sind: print(), println(), printf()
- println(): sendet die Daten an den Standardausgabestrom und beendet die aktuelle Zeile

### Dateneingabe
- Das Objekt **System.in** wird zur Eingabe von Daten in Java verwendet
- **Scanner**: Hilfsklasse zur Dateneingabe --> nextInt(), nextLine(), usw.

### Operatoren
- **Definition**: ein Symbol, das verwendet wird, um eine Art von Berechnung mit seinen Operanden durchzuführen --> unär, binär, ternär | arithmetisch, relational, logisch, bitweise
- Operator ?                                                   
```java
<Variable> = <Logischer Ausdruck> ? <Ausdruck bei true> : <Ausdruck bei false>;
int x, y;
x = 0;
y = 30 + 10 / (x==0 ? 1: x);
// Weil x = 0, 10 wird durch 1 geteilt --> y = 30 + 10/1 = 40
x = 2;
y = 30 + 10 / (x==0 ? 1: x);
// Weil x = 2, 10 wird durch x geteilt --> y = 30 + 10/2 = 35
```

### Anweisungen
- **Definition**: eine Anweisung legt eine Aktion fest
	- Deklaration: Variable deklarieren
	- Ausdruck: Ausdruck auswerten
	- Kontrollfluss: steuert die Reihenfolge, in der andere Anweisungen ausgeführt werden
- **switch**-Anweisung: nimmt einen Ausdruck statt einer Bedingung. Je nach dem Wert des Ausdrucks wird einer der Blöcke ausgeführt, in denen der Wert angegeben ist.
	- Wenn keine break-Anweisung angegeben ist, wird der nächste case-Block unabhändig vom angegebenen Wert ausgeführt
```java
switch (<Ausdruck>) {
	case <Konstante 1>:
		...
		break;
	default:
		...
}
```
- Scheifenanweisungen: for, for-each, while, do-while
	- Alle Parameter in der for-Schleife sind optional
- break-Anweisung: Schleife vorzeitig beenden
- continue-Anweisung: die Ausführung des restlichen Codes für eine Schleife ignorieren und mit der nächsten Iteration fortfahren

### Array
- **Definition**: eine nummerierte Menge von Variablen desselben Typs
	- Ein Variable in einem Array wird als Element bezeichnet, und ihre Position in dem Array wird durch einen Index angegeben
- **Deklaration und Initialisierung**
```java
<Typ>[] <Variable>;
<Typ> <Variable>[];

<Variable> = new <Type>[<Anzahl der Elemente>]
<Typ>[]  <Variable> = {a, b, c}
```
- Längebestimmung: arr.length
- mehrdimensionale Arrays: ```
```java
<Typ>[][] <Variable> = new <Typ>[row][column]
```

### OOP
- Eine **Klasse** umfasst eine Reihe von Klassenmitglieder
	- Variablen: Felder/Klasseneigenschaften
	- Methoden zur Steuerung dieser Varablen
- Wenn eine Klasse erstellt wird, wird ihr Name zu einem neuen Datentyp
- Auf der Grundlage einer Klasse können mehrere Objekte erstellt werden
- Dabei wird für jedes Objekt ein anderer Satz lokaler Variablen erstellt

- **Objekt**: jedes Objekt speichert Informationen über seine bei der Initialisierung angegebene Zeichenfolge. Die Änderung eines Objekt hat keinen Einfluss auf den Wert des anderen Objekts
- **Methodenüberladung**: Methoden mit dem gleichen Namen, nimmt aber verschiedene Parametertypen an
- **Verkapselung**: Klassenentwickler kann die interne Implementierung einer Klasse ändern, ohne die Zugriffsschnittstelle zu ändern
	- Definition: Verstecken von Daten innerhalb einer Klasse
- **Vererbung und Polymorphismus**: Alle Klassen haben nun Zugriff auf die gleichnamige Methode, aber die Implementierungen dieser MEthode in ihnen sind unterschiedlich
	- **Vererbung**: Möglichkeit, abgeleitete Klassen auf der Grundlage einer Basisklasse zu erstellen. In diesem Fall erhält die abgeleitete Klasse automatisch die Fähigkeiten der Basisklasse und kann neue Funktionen hinzufügen oder einige ihrer Methoden umdefinieren
	- **Polymorphismus**: die Definition einer Aktion, die von einer Methode mit demselben Name ausgeführt wird, hängt von dem Objekt ab, an dem die Aktion ausgeführt wird
- **Zugriffsmodifikatoren**:
	- public: offen. Das Feld ist für die externe Nutzung verfügbar
	- private: Das Feld ist nur innerhalb der Klasse, in der es deklariert ist, zugänglich. Die gägigsten Klassenfelder werden als privat deklariert
	- protected: Das Feld ist für die externee Nutzung unzugänglich, aber
		- für die Klasse, in der sie deklariert ist
		- für die Nachkommen dieser Klasse
		- innerhalb der Klassen des Pakets
- **Static**:
	- eine Variable, Konstante oder Methode innerhalb einer Klasse erstellen, auf die Sie zugreifen können, ohne eine Instanz der Klasse zu erstellen
	- Statische Methoden haben nur Zugriff auf statische Klassenmitglieder
	- nicht-statische Methoden können sowohl auf statische als auch nicht-statische Klassenmitglieder zugreifen
- **this**: bezieht sich auf eine Instanz der Klasse
- **Konstruktor**:
	- **Definition**: Um Felder beim Anlegen einer Instanz einer Klasse Angfangswerte zuzuwiesen, müssen Sie eine Methode erstellen, die denselben Namen wie der Klasssenname hat
	- Ein Konstruktor wird immer automatisch aufgerufen, wenn ein Objekt mit dem new-Operator erstellt wird
	- Ein Konstruktor kann überladene Versionen haben, die sich durch den Typ der Parameter unterscheiden oder ihre Anzahl
	- Der Typ des Rückgabewerts wird nicht angegeben
	- Wenn innerhalb einer Klasse kein Konstruktor vorhanden ist, wird automatisch ein Standardkonstruktor erstellt, der keine Parameter hat
- **Initialisierungsblöcke**: Felder werden innerhalb eines anonymen Blocks initialisiert. Der Block kann auch statisch sein
```java
public class Rectangle {
	public Point topLeft;
	public Point topRight;
	{
	topLeft = new Point();
	bottomRight = new Point(2, 3);
	}
}
```
- **Innere Klassen**
	- Konventionelle: nicht-statische Klassen innerhalb einer äußeren Klasse
	- Statische: statische Klasse innerhalb einer äußeren Klass
	- Lokale: Klassen innerhalb von Methoden
	- Anonyme: Klassen, die im laufenden Betrieb erstellt werden --> oft in GUIs verwendet, um Ereignisbehandler zu erstellen
```java
public class MyClass {
	public static void main(String[] args) {
		A obj = new A();
		A.B obj2 = obj.new B();
		obj2.func();
	}
}
class A {
	private int x;
	public class B { // konventionelle innere Klasse
		public void func() {
			System.out.println("A.B.func()");
			System.out.println(A.this.x);
		}
	}
}
```

```java
public class MyClass {
	public static void main(String[] args) {
		A.B obj = new A.B();
		obj.func();
		A obj2 = obj.new A();
		obj2.func();
	}
}

class A {
	private static int x;
	public void func() {
			System.out.println("A.func()");
	}
	public static class B { // statische innere Klasse
		public void func() {
			System.out.println("A.B.func()");
			System.out.println(A.x);
		}
	}
}
```

```java
public class MyClass {
	public static void main(String[] args) {
		A obj = new A();
		obj.func(1);
	}
}

class A {
	private int x = 10;
	public void func(final int k) {
		int y = 20; // der Wert wird nur einmal vergeben
		class B { // lokale innere Klasse
			private int z = 30;
			public void func() {
				System.out.println("B.func()");
				// Zugriff auf Felder der Klasse A
				System.out.println(y);
				System.out.println(z); // lokale var der Klasse B
				System.out.println(k); // lokale constant
			}
		}
		System.out.println("A.func()");
		B obj = new B(); obj.func();
}
```

```java
public class Animal {
	public void meow() {
		System.out.println("Meow!");
	}
	public static void main(String[] args) {
		Animal anonTiger = new Animal() {
			@Override
			public void meow() {
				System.out.println("Raar!");
			}
		};
		Animal notAnonTiger = new Animal().new Tiger();
		anonTiger.meow();
		notAnonTiger.meow();
	}
	private class Tiger extends Animal {
		@Override
		public void meow() {
			System.out.println("Raaar!");
		}
	}
}
```
### Vererbung
```java
[<Modifikator>] class <abgeleitete Klasse> extends <Basisklasse> {}
```
- **Definition**: die Fähigkeit, abgeleitete Klassen auf der Grundlage einer Basisklasse zu erstellen
	- Die abgeleitete Klasse erhält die Fähigkeiten der Basisklasse und kann neue Funktionen hinzufügen oder einige Methoden überschreiben
- Nur public und protected Mitglieder können vererbt werden
- private Mitglieder werden nicht vererbt, und innerhalb der abgeleiteten Klasse besteht kein Zugriff auf sie
- **super**(werte): Werte an die Konstruktoren der Basisklasse zu übergeben
- **Override**: eine Methode mit demselben Namen, aber mit einer anderen Signatur erstellen --> Methodenüberladung
	- Um dem Compiler mitzuteilen, dass es sich um eine Überschreibung handelt, sollte die Annotation @Override vor der Methodenbeschreibung eingefügt werden
- final: Um die Vererbung der gesamten Klasse/Methode zu verhindern
- Jede Klasse in der Sprache Java erbt entweder explizit oder implizit die Objektklasse --> toString(), equals(), getClass()

### Aufzählungen
- Eine Sammlung von Konstanten, die alle zulässigen Werte einer Variablen beschreiben
- Der Name der Aufzählung wird zu einem neuen Datentyp, der bei der Deklaration einer Variablen angegeben wird
- Anschließend können Der Variablen null zugewiesen werden
```java
[Zugriffskodierer] enum <name> {comma getrennte Liste von Konstanten};
enum Color {RED, BLUE, GREEN, BLACK};
```

### Abstrakte Klassen
- Abstrakte Methoden enthalten **nur eine Methodendeklaration ohne jegliche Implementierung**
- Abstrakte Klassen können **nicht instanziiert** werden
- Wenn eine Klasse eine abstrakte Methode enthält, muss die Klasse abstrakt deklariert werden
- Eine Klasse kann als abstrakt deklariert werden, auch wenn sie keine abstrakten Methoden enthält
- Abstrakte Methoden sollen von Unterklassen **überschrieben** werden und eine **Implementierung** erhalten
- Eine abstrakte Klasse vereint die Eigenschaften einer regulären Klasse mit denen eines Interfaces: Manchmal ist nicht nur, wie bei einem Interface, klar, welche Funktionalität alle implementierenden Klassen anbieten sollen, sondern für einen Teil dieser Funktionalität ist auch klar, dass diese auf die gleiche Art implementiert sein wird. 
- Nehmen wir beispielsweise ein Programm zum Setzen von Texten, das unterschiedliche Typen von Aufzählungen unterstützen können soll. Zu jeder Aufzählung müssen Elemente hinzugefügt und entfernt werden können, und es müssen die vorhandenen Elemente entsprechend formatiert ausgegeben werden können.
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

### Interface
```java
public interface Walkable {
	void walk();
}

public class Person implements Walkable {
	public void walk() {
		System.out.println("A person is walking.")
	}
}

public class Duck implements Walkable {
	public void walk() {
		System.out.println("A duck is walking.")
	}
}

public class Walkables {
	public static void letThemWalk(Walkable[] list) {
		for (Walkable w : list) {
			w.walk();
		}
	}
}
```
- Definition:
	- Ein Interface ähnelt einer abstrakten Klasse und enthält ebenfalls nur eine Methodensignatur und keine Implementierung
	- Ein Interface ist jedoch keine Klasse und es ist nicht möglich, eine Instanz eines Interfaces zu erstellen
	- Eine Klasse, die ein Interface implementiert, stellt sicher, dass in ihr Methoden definiert sind, die im Interfaceblock beschrieben sind
	- Wenn eine Klasse nicht mindestens eine Methode umdefiniert, wird sie zu einer abstrakten Klasse und es kann keine Instanz dieser Klasse erstellt werden
- Erstellung: class A implements IRead {}
- [design patterns - Abstract class vs Interface in Java - Stack Overflow](https://stackoverflow.com/questions/10040069/abstract-class-vs-interface-in-java)

### Comparable
```java
public interface Comparable<T> {
	int compareTo(T t);
}

class A implements Comparable<A> {
	private int x;
	public A(int x) {
		this.x = x;
	}
	public int getX() {return x;}
	@Override
	public int compareTo(A other) {
		if (this.x > other.x) return 1;
		if (this.x < other.x) return -1;
		return 0;
		// oder:
		return Integer.compare(this.x, other.getX())
	}
}

Collections.sort(list)
```

### Comparator
```java
class A {
	private int x;
	public A(int x) {
		this.x = x;
	}
	public int getX() {return x;}
}

class ComparatorClassA implements Comparator<A> {
	@Override
	public int compare(A a1, A a2) {
		return Integer.compare(firstA.getX(), secondA.getX());
	}
}

Collections.sort(list, Comparator);
```
```java
Collections.sort(aList, new ComparatorClassA());
Collections.sort(aList, new Comparator<A>() {
	@Override
	public int compare(A a1, A a2) {
		return Integer.compare(firstA.getX(), secondA.getX());
	}
})

Comparator<A> byX = (A a1, A a2) -> Integer.compare(firstA.getX(), secondA.getX());
Collections.sort(list, byX);

Comparator<A> byX = Comparator.comparing(A::getX);
Collections.sort(list, byX);
```

### Observer pattern
- 2 Arten von Objekten: Beobachter und Subjekt
- Idee:
	- Beobachter interessieren sich für ein Subjekt und melden sich an, um Benachrichtigungen von diesem Objekt zu erhalten
	- Wenn sich der Zustand des Objekts ändert, werden alle Beobachter automatisch benachrichtigt
	- Verlieren sie daran ihr Interesse, melden sie sich einfach ab

### Ereignisbehandlung
```java
public class MyClass {
	public static void main(String[] args) {
		MyButton button = new MyCopyButton();
		button.click();
	}
}

class MyButton {
	public void click() {System.out.println("Clicked!");}
}

class MyCopyButton extends MyButoon {
	public void click() {System.out.println("Copied!");}
}
```

```java
public class MyClass {
	public static void main(String[] args) {
		MyButton button = new MyCopyButton() {
			@Override
			public void click() {System.out.println("Copied!");}
		}
		button.click();
	}
}

class MyButton {
	public void click() {System.out.println("Clicked!");}
}
```

```java
public class MyClass {
	public static void main(String[] args) {
		MyButton button = new MyButton();
		IClick a = new A();
		button.reg(a);
		button.click();
	}
}

interface IClick {
	void onClick();
}

class A implements IClick {
	@Override
	void onClick() {System.out.println("Clicked!")}
}
class MyButton {
	private IClick ic = null;
	public void reg(IClick ic) {
		this.ic = ic;
	}
	public void click() {
		if (this.ic != null) {ic.onClick();}
	}
}
```

```java
public class MyClass {
	public static void main(String[] args) {
		MyButton button = new MyButton();
		button.reg(new IClick() {
			@Override
			void onClick() {System.out.println("Clicked!")}
		});
		// oder:
		button.reg(
			() -> {System.out.println("Clicked!")}
		);
		button.click();
	}
}

interface IClick {
	void onClick();
}

class MyButton {
	private IClick ic = null;
	public void reg(IClick ic) {
		this.ic = ic;
	}
	public void click() {
		if (this.ic != null) {ic.onClick();}
	}
}
```

- Swing:
```java
button.addActionListener(new ActionListener() {
	@Override
	public void actionPerformed(ActionEvent e) {
		System.out.println("Clicked");
	}
});

button.addActionListener(
	event -> System.out.println("Clicked");
)
```

### Funktionale Interfaces und Lambda-Ausdrücke
- Ein Lambda-Ausdruck wird nur in Verbindung mit funktionalen Interfaces verwendet.
- Ein funktionales Interface beschreibt nur eine zu überschreibende Methode
```java
@FunctionalInterface
interface IClick {
	void onClick();
}
```
- Das funktionalen Interface Function stellt eine Übergangsfunktion von einem Objekt vom Typ T zu einem Objekt vom Typ R dar:
```java
public interface Funtion<T, R> {
	R apply(T t);
}

public interface Predicate<T> {
	boolean test(T t);
}

public interface BinaryOperator { 
	T apply(T t1, T t2); 
}

public interface UnaryOperator { 
	T apply(T t); 
}
```

### Callbacks
### Generische Typen
- Der generische Typ kann bei seiner Deklaration auf den Namen einer Basisklasse oder eines Interfaces eingeschränkt werden.
- Einschränkungen für Typparameter

### Generische Methoden
- Man kann die Methoden des Typs aufrufen
- These are some properties of generic methods:
	-   Generic methods have a type parameter (the diamond operator enclosing the type) before the return type of the method declaration.
	-   Type parameters can be bounded (we explain bounds later in this article).
	-   Generic methods can have different type parameters separated by commas in the method signature.
	-   Method body for a generic method is just like a normal method
```java
public class MyClass {
	public static void main(String[] args) {
		A a = new A();
		MyClass.<A>func(a);
		B b = new B();
		MyClass.<B>func(b);
	}
	public static <T extends A> void func(T obj) {
		obj.test();
	}
}

class A {
	public void test() {System.out.println("test1");}
}

class B extends A {
	@Override
	public void test() {System.out.println("test2");}
}
```