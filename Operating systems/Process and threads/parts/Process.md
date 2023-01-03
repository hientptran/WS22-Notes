[[Process and threads]]
## Process
#### Resources
[Operating Systems: Processes (uic.edu)](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/3_Processes.html)
[Operating System Concepts: 3. Processes (yale.edu)](https://view.officeapps.live.com/op/view.aspx?src=https%3A%2F%2Fcodex.cs.yale.edu%2Favi%2Fos-book%2FOS10%2Fslide-dir%2FPPTX-dir%2Fch3.pptx&wdOrigin=BROWSELINK)
[4 Process.pptx — Yandex.Disk](https://disk.yandex.ru/i/Q-JjdGg3rYrct)
### Definition
- A process is an instance of a program in execution
- Batch systems work in terms of "jobs." Many modern process concepts are still expressed in terms of jobs
### Process memory

![[../../../_assets/process memory.png | 150]]

- **Text:** compiled program code, read in from non-volatile storage (_a type of computer memory that can retain stored information even after power is removed_) when the program is launched
- **Data**: global and static variables, initialized prior to executing main
- **Heap**: dynamic memory allocation, managed via calls to new, delete, malloc, free, etc. -> Big objects
- **Stack**: local variables, space reserved when local variable is declared, freed up when variable goes out of scope. Also used for function return value

> Stack and heap start at opposite ends of the process's free space and grow towards each other. If they meet, stack overflow error will occur, or new or malloc will fail due to insufficient memory

#### example (bsp_multithreading.c) 
[[Process and threads#multithreading.c]]
**2 processes:** after a process: stack freigegeben, heap nicht freigegeben, daten bleibt erhalten
	1. ![[../../../_assets/process memory example 1.png | 250]]
	2. ![[../../../_assets/process memory example 2.png | 250]]
	3. ![[../../../_assets/process memory example 3.png | 250]]
**2 threads:** 2 threads run parallel and create 2 stacks, head and data are used by both threads (memory allocation different between different OS version)
	1. ![[../../../_assets/process memory example 4.png | 250]]
	2. ![[../../../_assets/process memory example 5.png | 250]]

### Process creation
1. Systemstart
2. Ausführung eines Programms, das einen neuen Prozess erzeugt
3. Nutzeraktion
4. Start eines Batchjobs
### Process termination
1. Normaler Abbruch
2. Fehlerabbruch
3. Abbruch aufgrund eines schweren Fehler (oft: Programmierfehler)
4. Abbruch durch anderen Prozess (Unix: kill; Windows: TerminateProcess)

### Process states
![[../../../_assets/prozesszustände.png]]

1. **Ready**: Prozess befindet sich in Warteschleife (Anfangszustand). Scheduler wählt einen Prozess für die CPU aus
2. **Running**: Prozess wird von CPU bearbeitet. <u>Nach Ablauf der "Zeitscheibe"</u> kommt ein Prozess zurück in die Warteschlange (2), sofern er nicht abgearbeitei ist (5) oder blockiert wird (3)
3. **Blocked**: Prozess wartet z.B auf ein externes Ereignis, vebraucht keine CPU-Ressourcen. Nach Eintreten des Ereignisses wird Prozess wieder in Warteschlange gesteckt

> [!NOTE]- Full process states
> - **New**: the process is being created
> - **Ready**: The process has all the resources that it needs to run, but the CPU is not currently woring on this process's instructions
> - **Running**: CPU working on this process's instructions
> - Waiting: Process not running, waiting for some resource to become available or for some event to occur (keyboard input, disk access request, timer, child process, etc.)
> - **Terminated**: Process completed
> ![[../../../_assets/full process states.png]]

### Process table
- A process control block (PCB) contains information about the process, i.e. registers, quantum, priority, etc. 
- The process table is an array of PCB’s, that means logically contains a PCB for all of the current processes in the system.
- Einigen wichtige Informationen zum Prozesszustand (systemabhängig), werden zu Beginn und am Ende der Zeitscheibe geladen bsz. gespeichert

> [!NOTE]- Some examples of information in PCBs
> - **Pointer –** It is a stack pointer which is required to be saved when the process is switched from one state to another to retain the current position of the process.
> - **Process state –** It stores the respective state of the process.
> - **Process number –** Every process is assigned with a unique id known as process ID or PID which stores the process identifier.
> - **Program counter –** It stores the counter which contains the address of the next instruction that is to be executed for the process.
> - **Register –** These are the CPU registers which includes: accumulator, base, registers and general purpose registers.
> - **Memory limits –** This field contains the information about memory management system used by operating system. This may include the page tables, segment tables etc.
> - **Open files list –** This information includes the list of files opened for a process
> 
> These can be sorted into the following categories:
> - **Process state:** running, waiting, etc.
> - **Process ID** and parent PID
> - **CPU registers and Program counter**
> - **CPU scheduling information
> - Memory management information
> - Accounting information
> - I/O status information**


