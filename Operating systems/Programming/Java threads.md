- Package: java.lang
- **Konstruktionsmethoden**: Erweiterung der Klasse Threads/Implementierung von Runnable
``` java
public class MyProg {
	public static void main(String[] args) {
		MyClass output = new MyClass();
		output.start();
		
		MyRun myRun = new MyRun("Hello!");
	}
}

public class MyClass extends Threads {
	@Override
	public run() {
		 System.out.println("...");
	}
}

public class MyRun implements Runnable {
	private String msg;
	public MyRun(String msg) {this.msg = msg;}
	@Override
	public void run() {
		 
	}
}
```

#### Thread synchronization
**- Safe/Not safe thread:**
	- 0.3.3/0.3.4 (S.8, 9): Beide Threads haben Zugriff auf i -> Verändern von Speicherinhalten nicht atomar; Race condition -> not thread safe
	- (S.10) Synchronized method iPlusPlus -> Nur ein Thread kann pro Zeit diese Methode ausführen -> thread safe
	- (S.13) Lock/unlock -> kritischer Bereich -> Nur ein Thread kann pro Zeit diesen Bereich eintreten
- Cyclic barrier: Threads warten an einer Barriere