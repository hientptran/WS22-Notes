[[Algorithms and data structures]]

## **Interface** 
- abstrakt Datentyp -> nur welche Methode zur Verfügung stehn und aufgerufen werden, keine konkrete Inplementierung; group related methods with empty bodies [Java Interface (w3schools.com)](https://www.w3schools.com/java/java_interface.asp)
	- The methods can have different implementations (Bsp: unterschiedliche Implementation bei Linux, Windows, usw.) -> All implementations can be sorted under one abstract data type
	- Bsp: Interface Fahrzeug mit der Methode fahren(); die Klasse Fahrrad und Auto implementieren den Interface Fahrzeug; fahren() bei Fahrrad ist z.B pendeln, bei Auto z.B motor
	- Interface kann nich initialisiert werden, nur Class
- Class Console implements IConsole -> class implements interface
- Class App initialisiert Console
```java
// Interface
interface Animal {
  public void animalSound(); // interface method (does not have a body)
  public void sleep(); // interface method (does not have a body)
}
// Pig "implements" the Animal interface
class Pig implements Animal {
  public void animalSound() {
    // The body of animalSound() is provided here
    System.out.println("The pig says: wee wee");
  }
  public void sleep() {
    // The body of sleep() is provided here
    System.out.println("Zzz");
  }
}
class Main {
  public static void main(String[] args) {
    Pig myPig = new Pig();  // Create a Pig object
    myPig.animalSound();
    myPig.sleep();
  }
}
```

> [!NOTE]- Notes on Interfaces
> - interfaces cannot be used to create objects (initialized) and therefore cannot contain a constructor
> - interface methods do not have a body
> - on implementation of an interface, you must override all of its methods
> - interface methods are by default abstract and public
> - interface attributes are by default public static final
>

## **Command pattern:** 
All users operations are added to a list -> enables undoing, logging, etc.
	- Needs interface ICommand with execute() method -> Muster mit alle Operationen
	- Class Command with implementation of the method execute()
	- CommandInvoker/CommandFactory prints all commands 
	- Class App to run program

```java
//---------------------------COMMAND-----------------------------------------------
//-------------ICommand.java---------------------
public interface ICommand() {
	public void execute();
}
//-------------ExitComd.java------------------
public class ExitCommand implements ICommand {
	@Override
	public void execute() {
		System.out.println("Program closed.");
		System.exit(0);
	}
	@Override
	public String toString() {
		return "Exit program."
		}
}
//-----------------GCD.java------------------
public class GCDEuclidDivisionRestCommand implements ICommand {
    private final IConsole console;
    @Override
    public String toString() {
        return "Greatest Common Divisor (GCD) - (Recursive) Euclid's algorithm division rest.";
    }
    public GCDEuclidDivisionRestCommand(IConsole console) {
        super();
        this.console = console;
    }
    @Override
    public void execute() {
        int x = console.readInteger("Please, enter a number for x: ");
        int y = console.readInteger("Please, enter a number for y: ");
        int res = x + y; // TODO:
        console.writeString(String.format("gcd(%d, %d) = %d", x, y, res));      
    }
}
//-------------CmdFactory.java------------------
//creates a list of ICommands
public class CommandFactory {
    public static LinkedList<ICommand> returnCommands(IConsole console) {
        LinkedList<ICommand> commands = new LinkedList<ICommand>();
        commands.add(new ExitCommand());
        commands.add(new GCDEuclidSubstractionCommand(console));
        commands.add(new GCDEuclidDivisionRestCommand(console));
        return commands;
    }
}
//-------------CmdInvoker.java------------------
public class CommandInvoker {
    final String scSelectAnOption = "Please, select an option: ";
    final String scStudentInfo = "Console-Application: Exercise-1 <Vorname> <Nachname> <Matrikelnummer>";
    final IConsole console;
    public CommandInvoker(IConsole console) {
        super();
        this.console = console;
    }
    public void cli(LinkedList<ICommand> commands) {
        do {
            printCommandLineMenu(commands);
            ICommand cmd = selectAnOption(console, commands);
            cmd.execute();
            // selectAnOption(console, commands).execute();
        } while (true);
    }
    private ICommand selectAnOption(IConsole console, LinkedList<ICommand> commands) {
        do {
            int index = console.readInteger(scSelectAnOption);
            if (isValidOption(index, 0, commands.size())) {
                return commands.get(index);
            }
        } while (true);
    }
    private void printCommandLineMenu(LinkedList<ICommand> commands) {
        System.out.println();
        System.out.println(scStudentInfo + System.lineSeparator());
        for (int i = 1; i < commands.size(); i++) {
            System.out.println(i + ". " + commands.get(i));
        }
        System.out.println("0. " + commands.get(0) + System.lineSeparator());
    }
    private boolean isValidOption(int index, int min, int max) {
        return index >= min && index < max;
    }
}

//---------------------------CONSOLE-----------------------------------------------
//-------------IConsole.java---------------------
public interface IConsole {
	public int readInteger(String text);
	public String readString(String text);
	public void writeString(String text);
}
//-------------Console.java---------------------
public class Console implements IConsole {
	private Scanner scanner = new Scanner(System.in);
	@Override
	public String readString(String text) {
		return null
	}
	@Override
	public void writeString(String text) {
		System.out.println(text);
	}
	@Override
	public String readInteger(String text) {
		System.out.println(text);
		while (scanner.hasNextInt()) {
			return scanner.nextInt();
		}
		return -1;
	}
}

//---------------------------APP---------------------------------------------------
//-------------------App.java---------------------
public class App {
	public static void main(String[] args) {
		IConsole console = new Console();
		LinkedList<ICommand> commands = CommandFactory.returnCommands(console);
		CommandInvoker invoker = new CommandInvoker(console);
		invoker.cli(commands);
	}
}
```

![[../../_assets/command_patterns.png | 500]]

## Resources
[Command Design Pattern (sourcemaking.com)](https://sourcemaking.com/design_patterns/command)
