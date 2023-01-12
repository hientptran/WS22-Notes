[[Operating systems]]

## [[Process]]
#### Resources
[Operating Systems: Processes (uic.edu)](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/3_Processes.html)
[Operating System Concepts: 3. Processes (yale.edu)](https://view.officeapps.live.com/op/view.aspx?src=https%3A%2F%2Fcodex.cs.yale.edu%2Favi%2Fos-book%2FOS10%2Fslide-dir%2FPPTX-dir%2Fch3.pptx&wdOrigin=BROWSELINK)
[4 Process.pptx — Yandex.Disk](https://disk.yandex.ru/i/Q-JjdGg3rYrct)
### Definition
- A process is an instance of a program in execution
- Batch systems work in terms of "jobs." Many modern process concepts are still expressed in terms of jobs
### Process memory

![[../../_assets/process memory.png | 150]]

- **Text:** compiled program code, read in from non-volatile storage (_a type of computer memory that can retain stored information even after power is removed_) when the program is launched
- **Data**: global and static variables, initialized prior to executing main
- **Heap**: dynamic memory allocation, managed via calls to new, delete, malloc, free, etc.
- **Stack**: local variables, space reserved when local variable is declared, freed up when variable goes out of scope. Also used for function return value

> Stack and heap start at opposite ends of the process's free space and grow towards each other. If they meet, stack overflow error will occur, or new or malloc will fail due to insufficient memory

#### example (bsp_multithreading.c) 
[[Process and threads#multithreading.c]]
**2 processes:** after a process: stack freigegeben, heap nicht freigegeben, daten bleibt erhalten
	1. ![[../../_assets/process memory example 1.png | 250]]
	2. ![[../../_assets/process memory example 2.png | 250]]
	3. ![[../../_assets/process memory example 3.png | 250]]
**2 threads:** 2 threads run parallel and create 2 stacks, head and data are used by both threads (memory allocation different between different OS version)
	1. ![[../../_assets/process memory example 4.png | 250]]
	2. ![[../../_assets/process memory example 5.png | 250]]

### Process creation
1. Systemstart
2. Ausführung eines Programms, das einen neuen Prozess erzeugt
3. Nutzeraktion
4. Start eines Batchjobs

#### fork() [C Programming](C%20Programming.md)
```C
// fork() duplicates the parent process and creates child process. parent and child processes runs parallel
// fork() returns process id (pid) of the child process to the parent and 0 to the chile. returns -1 if there's an error
int main() {
	int child = fork(); //returns 0 in child process
	if (child == 0) {
		fork();
	}
	printf("hello\n"); // prints hello 3 times
}

int my_pid = getpid();
int parent_pid = getppid();

sleep(2); //wait 2 seconds (only ganzzahlen)

kill(pid, SIGTERM) //ends process with given pid
```
- pid_t **wait(int \*status_loc)**
	- suspends execution of the claling process until one of its children terminates
	- On success: returns the process ID of terminated child; on error -1
- pid_t **waitpid(pid_t pid, int \*status_loc, int optionen)**
	- Suspends execution of the calling process until a child specified by pid argument has changed state. Default: waits for terminated children (modified through option)
	- If _status_ is not NULL, **wait**() and **waitpid**() store status information in the _int_ to which it points.
	- On success, returns the process ID of the child whos state has changed

### Process termination
1. Normaler Abbruch
2. Fehlerabbruch
3. Abbruch aufgrund eines schweren Fehler (oft: Programmierfehler)
4. Abbruch durch anderen Prozess (Unix: kill; Windows: TerminateProcess)

### Process states
![[../../_assets/prozesszustände.png]]

1. **Ready**: Prozess befindet sich in Warteschleife (Anfangszustand). Scheduler wählt einen Prozess für die CPU aus
2. **Running**: Prozess wird von CPU bearbeitet. <u>Nach Ablauf der "Zeitscheibe"</u> kommt ein Prozess zurück in die Warteschlange (2), sofern er nicht abgearbeitei ist (5) oder blockiert wird (3)
3. **Blocked**: Prozess wartet z.B auf ein externes Ereignis, vebraucht keine CPU-Ressourcen. Nach Eintreten des Ereignisses wird Prozess wieder in Warteschlange gesteckt

> [!NOTE]- Full process states
> - **New**: the process is being created
> - **Ready**: The process has all the resources that it needs to run, but the CPU is not currently woring on this process's instructions
> - **Running**: CPU working on this process's instructions
> - Waiting: Process not running, waiting for some resource to become available or for some event to occur (keyboard input, disk access request, timer, child process, etc.)
> - **Terminated**: Process completed
> ![[../../_assets/full process states.png]]

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

## [[Threads]]
#### Resources
[Operating System Concepts: 4. Threads & Concurrency (yale.edu)](https://view.officeapps.live.com/op/view.aspx?src=https%3A%2F%2Fcodex.cs.yale.edu%2Favi%2Fos-book%2FOS10%2Fslide-dir%2FPPTX-dir%2Fch4.pptx&wdOrigin=BROWSELINK)
[Operating Systems: Threads (uic.edu)](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/4_Threads.html)
[5 Threads.pptx - Yandex.Documents](https://docs.yandex.ru/docs/view?url=ya-disk-public%3A%2F%2FCl65XG0DVTm1nBNKaTaPKALbXtzTPynvGQyMPqWpI5Q%3D&name=5%20Threads.pptx)
### Definition/Application
- Threads ermöglichen **parallele Ausführung von Prozessen** (=> Performance)
	- Jeder Thread wird getrennt von den anderen Threads des Prozesses bearbeitet
	- Jeder Thread besitzt einen eigenen Programmzähler
- Threads teilen sich einen gemeinsamen Speicher (=> Synchronisation)

- **Examples/Application:** 
	- In a browser, multiple tabs can be different threads
	- **Multithreaded web server:** 
		- Multiple threads allow for multiple requests to be satisfied simultaneously, without having to service requests sequentially or to fork off separate processes for every incoming request.
		- Dispatcher reads incoming requests from the netzrok. After examining the request, it chooses an idle worker thread and hands it the request. The dispatcher then wakes up the sleeping worker, moving it from blocked status
		- ![[../../_assets/thread application example 2.png]]
	- **Word processors** uses multiple threads: one thread to format the text, another thread to process inputs, etc. (a background thread may check spelling and grammar while a foreground thread processes user input ( keystrokes ), while yet a third thread loads images from the hard drive, and a fourth does periodic automatic backups of the file being edited.)
		- ![[../../_assets/thread application example 1.png]]
> [!NOTE]- Process vs Thread
> The primary difference is that threads within the same process run in a shared memory space, while processes run in separate memory spaces.  
> Threads are not independent of one another like processes are, and as a result threads share with other threads their code section, data section, and OS resources (like open files and signals). But, like process, a thread has its own program counter (PC), register set, and stack space.

### Thread table
- Per process items: Systemaspekte, die von allen Threads eines Prozesses geteilt werden
	- Die Threads eines Prozesses gehören demselben Nutzer => geringere Sicherheitsanforderungen
- Per thread items: Systemaspekte, die jeder Thread selbst kontrolliert

### Thread memory
![[../../_assets/thread memory.png | 400]]
- Jeder Thread besitzt einen eigenen Stack und Registers
- Threads teilen sich einen gemeinsamen Speicher (code, data, files). Heap und Daten können von Threads gemeinsam genutzt werden (Shared memory)

### pthread
[[C Programming]]
- **int pthread_create**(pthread_t \*restrict thread, 
				    const pthread_attr_t \*restrict attr,
				    void \*(\*start_routine)(void \*),
				    void \*restrict arg);****
	- The function *pthread_create()* starts a new thread in the calling process. The new thread starts execution by invoking *start_routine();* *arg* is passed as the sole arguments of *start_routine().*
	- The *attr* argument points to a *pthread_attr_t* structure whose contents are used at thread creation time to determine attributes for the new thread -> initialized using *pthread_attr_init()*. If attr is NULL then the thread is created with default attributes
	- Before returning, a successful call to *pthread_create()* stores the ID of the new thread in the buffer pointed to by *thread*
	- The new thread terminates in one of the following ways:
		- It calls pthread_exit, specifying an exit status calue that is availabe to another thread in the same process that calls pthread_join()
		- it returns from start_routine()
		- it is canceled
		- Any of the threads in the process calls exit(), or the main thread return from main()
- **int phtread_join**(phtread_t thread, void \*\*retval)
	- The *pthread_join()* function waits for the thread specified by *thread* to terminate. If that thread has already terminated, then *pthread_join()* returrns immediately.
	- If *retval* is not NULL, then *pthread_join()* copies the exit status of the target thread into the location pointed to by retval.
	- On success, *pthread_join()* returns 0; on error, it returns an error number
#### bsp_pthread.c
bsp_pthread.c
```C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h> //atoi

int sum; // this data is shared by the thread(s)
void *runner(void *param); //the thread

/*The main function of a C++ program has two parameters, by convention named argc and argv, which give it the command-line arguments used to launch the program.
argc is the count of arguments, and argv is an array of the strings (arg vector).
The program itself is the first argument, argv[0], so argc is always at least 1.
So, argc is 2 when the program is run with one command-line argument. */
int main (int argc, char *argv[]) {
	pthread_t tid; //thread identifier
	pthread_attr_t attr; //set of thread attributes
	if (argc != 2) {
		fprintf(stderr, "usage: a.out <integer value>\n");
		return -1;
	}
	// _atoi_(const char *str) converts the string argument str to an integer (type int)
	if (atoi(argv[1]) < 0) {
		fprintf(stderr, "%d must be >= 0\n", atoi(argv[1]));
		return -1;
	}
	//get the default attributes
	pthread_attr_init(&attr);
	//create thread
	pthread_create(&tid,&attr,runner,argv[1]);
	//wait for the thread to exit
	pthread_join(tid,NULL);
	printf("sum = %d\n",sum);
}

//the thread will begin control in this function
// void *param: it is just a general pointer passing to the function, your function should know what is the actual type of the pointer - and cast it apropriatly before accessing the data
void *runner(void *param) {
	int i, upper = atoi(param);
	sum = 0;
	for (i = 1; i <= upper; i++)
		sum += i;
	pthread_exit(0);
}
```

Command line: 
```bash
gcc bsp_pthread.c
./a.out 5
> sum = 15 //output
```

#### bsp_multithreading.c
bsp_multithreading.c
```C
#include <stdio.h>
#include <pthread.h>
#include <unistd.h> //sleep()

int zeit = 0; //global var, modified from both threads

void *prog_a(void *arg) { //thead 1
	int i, t = 1;
	for (i = 0; i < 3; i++) {
		sleep(t); zeit = zeit + t;
		printf("%s%d: Zeit = %d s\n", (char *)arg, i, zeit);
	}
	return 0;
}

void *prog_b(void *arg) { //thread 2
	int i, t = 1;
	for (i = 0; i < 3; i++) {
		sleep(t); zeit = zeit + t;
		printf("%s%d: Zeit = %d s\n", (char *)arg, i, zeit);
	}
	return 0;
}

int main() {
	pthread_t id1, id2; //tid: thread identifier
	char *ch_id;
	void *res;

	//2 threads run parallel
	ch_id="a_";
	//&: address of pointer
	pthread_create(&id1, NULL, prog_a, (void *)ch_id);
	//%p: pointer type
	printf("Thread a: ID=%p, Zeit=%d s\n", (void *)id1, zeit);

	ch_id="b_";
	pthread_create(&id2, NULL, prog_a, (void *)ch_id);
	printf("Thread a: ID=%p, Zeit=%d s\n", (void *)id2, zeit);

	//wait for the thread to end and copy result into the specified address (here: res)
	pthread_join(id1, &res);
	pthread_join(id2, &res);
	printf("Gesamtzeit = %d s\n", zeit);
	return 0;
}
```

Command line:
```bash
gcc bsp_multithreading.c
./a.out
>	Thread a: ID=0x7f4066e86640, Zeit=0 s
	 Thread a: ID=0x7f4066685640, Zeit=0 s
	 a_0: Zeit = 1 s
	 b_0: Zeit = 2 s
	 a_1: Zeit = 3 s
	 b_1: Zeit = 4 s
	 a_2: Zeit = 5 s
	 b_2: Zeit = 6 s
	 Gesamtzeit = 6 s
```
Memmory: [[Process and threads#example (bsp_multithreading.c)]]

### User level threads
- Is implemented in the user level library (POSIX, Windows, Java threads), not created using the system calls
- Thread switching does not need to call OS and to cause interrupt to Kernel
- Kernel doesn't know about the user-lever thread and manages them as if they were single-threaded processes. There's a thread table in each process
- Thread wird vom Nutzer verwaltet (Stack scheduling, usw.)
![[../../_assets/user level thread.png | 300]]
### Kernel level threads
- Kernel knows and manages the threads
- Instead of thread table in each process, the kernel has a master thread table that keeps track of all the threads in the system
- In addition, kernel also maintains the traditional process table to keep track of the processes. Kernel provides system calls to create and manage threads
- Threads wird com Kernel verwaltet => einfacher, aber weiniger performant als ULT
![[../../_assets/Kernel level threads.png | 300]]
### Hybrid implementation
- Vereinigung der Vorteile beider Methoden (ULT, KLT), unter Auslassung ihrer Nachteile
- The kernel is aware of only the kernel-level threads and schedule those. Some of those threads may have multiple ULT on top of them
![[../../_assets/Hybrid implementation ULT KLT.png | 300]]
### Pop-up threads
- Thread wird bei Ankunft einer Mitteilung erzeugt (a: vor Ankuft; b: nach Ankunft)
- Vorteil: kein blocked Thread, der auf Mitteilung wartet
- Nachteil: nicht günstig, falls zu viele Mitteilungen in kurzer Zeit - hier wäre eine Thread-Pool mit blocked Threads ressourcensschonender
![[../../_assets/Popup threads.png | 300]]

## [[Interprocess communication]]
### Race conditions
- Zwei Prozesse greifen zur gleichen Zeit auf gemeinsame globale Variable zu
> [!NOTE]- Extended definition
>  A race condition occurs when two or more threads can access shared data and they try to change it at the same time. Because the thread scheduling algorithm can swap between threads at any time, you don't know the order in which the threads will attempt to access the shared data. Therefore, the result of the change in data is dependent on the thread scheduling algorithm, i.e. both threads are "racing" to access/change the data.
>  
> Problems often occur when one thread does a "check-then-act" (e.g. "check" if the value is X, then "act" to do something that depends on the value being X) and another thread does something to the value in between the "check" and the "act". 

> [!NOTE] Print spooler example
> When a process wants to print a file, it enters the file name in a special spooler directory. Another process, the printer daemon, periodically checks to see if there are any files to be printed, and if there are, it prints them and then removes their names from the directory
> 
> Our spooler directory has many numbered slots, each one holds a file name. There are 2 shared variables out, which points to the next file to be printed, and in, which points to the next free slot in the directory
> 
> At a point, slots 4 to 6 are full, with files queued for printing. The processes A and B want to queue a file for printing
> 
> A reads in and stores the value 7. Just then a clock interrupt occurs and CPU switches to process B. Process B reads in, and also gets a 7. Now both processes think that the next available slot is 7
> 
> B continues to run, saves the name of its file in slot 7 and updates in to be an 8
> Process A runs again, writes its file name in slot 7, erasing B's file name. Then it sets in to 8. The spooler directory is now internally consistent, so the printer daemon will not notice anything wrong, but process B will never receive any output
> ![[../../_assets/race conditions example.png | 300]]
> 
> Source: [6 IPC.pptx (live.com)](https://view.officeapps.live.com/op/view.aspx?src=https%3A%2F%2Fweb.karabuk.edu.tr%2Fyasinortakci%2Fdokumanlar%2Fisletim_sistemleri%2F6%2520IPC.pptx%23%3A~%3Atext%3DCritical%2520Regions%26text%3DThat%2520part%2520of%2520the%2520program%2Cfrom%2520doing%2520the%2520same%2520thing.&wdOrigin=BROWSELINK)


### Critical region
- Teil eines Programm, der auf einen gemeinsamen Speicher mehrerer Prozesse zugreift
- ![[../../_assets/critical region.png | 300]]
### Solution to race conditions
4 conditions:
1. Any two processes cannot be simultaneously inside their critical regions
2. No assumptions may be made about speed or the number of CPUs
3. Any process running outside its critical region cannot block other processes
4. Any process should not have to wait forever to enter its critical region
#### Busy waiting
- **Definition:** Continously testing a variable until some value appears is called busy waiting. It should usually be avoided, since it wastes CPU time
	The repeated execution of a loop of code while waiting for an event to occur is called busy-waiting. The CPU is not engaged in any real productive activity during this period, and the process does not progress toward completion.
	
	Busy-waiting, busy-looping or spinning is a technique in which a process repeatedly checks to see if a condition is true, such as whether keyboard input or a lock is available. Busy waiting means a process simply spins, (does nothing but continue to test its entry condition) while it is waiting to enter its critical section. This continues to use (waste)
- **Example:**
	- ![[../../_assets/Busy waiting example.png | 350]]
	- An int variable turn (initially 0) keeps track of whose turn it is to enter the critical region and examine or update the shared memory. 
	- When process 1 leaves the critical region, it sets turn to 1, to allow process 2 to enter its critical region. Suppose that process 2 finishes its noncritical region, so both processes are in their noncritical regions (turn = 0)
	- Suddenly, process 1 finishes its noncritical region before process 2 finishes its noncritical region and goes back to the top of its loop (has to wait until process 2 finishes noncritical region). It is not permitted to enter critical region (turn = 1, needs turn = 0) => violates condition 3
	- ![[../../_assets/busy waiting problem.png]]
#### Semaphore
- **Dijkstrasche Lösung:** Einführung eines neuen Datentyps Semaphore und die Prozesse up und down
	- Semaphore = 0, 1, 2,...: int value to count the number of wakeup processes saved for future use
	- up and down:
		- Aufruf von up => Semaphone++
		- Semaphore > 0: Aufruf von down => Semaphore--
		- Semaphore = 0: Aufruf von down => down = blocked (process is put to sleep)
	- up und down sind atomar: sie werden vom Betriebssystem nicht unterbrochen, bevor sie beendet sind bzw. den Zustand blocked erreicht haben (when a semaphore operation has started, no other process can access the semaphore until the operation has completed or is blocked)
	- The semaphore can be initialized to the number of instances of the resource. Whenever a process wants to use that resource, it checks if the number of remaining instances is more than zero, i.e., the process has an instance available. Then, the process can enter its critical section thereby decreasing the value of the counting semaphore by 1. After the process is over with the use of the instance of the resource, it can leave the critical section thereby adding 1 to the number of available instances of the resource
- **Producer-consumer problem:**
	- Two processes share a common, fixed-size buffer. One of them, the producer, puts information into the buffer, and the other one, the comsumer, takes it out
		- Produzent: Schreibt Informationen in einen Puffer
		- Konsument: liest Informationen aus Puffer
	- Problem: When the producer wants to put a new item in an already full buffer/producer wants to get items from an empty buffer
	- The solution is for the producer to go to sleep, and sleeping producer will be awakened when the consumer has removed one or more items or vice versa
- **Semaphore solution:** 3 Semaphoren: mutex, empty, full
	- **mutex = 0, 1 (initially 1 true)** (binäre Semaphore) regelt den wechselseitigen Ausschluss (mutual exclusion) => Nur ein Prozess kann pro Zeit in den Puffer schreiben/aus dem Puffer lesen
	- **empty (initially N), full (initially 0) = 0, 1, ... N** dienen der Synchronisation => Bestimmte Ereignisse treten ein bzw. treten nicht ein (hier: leerer/voller Puffer). Ensures that the producer stops running when the buffer is full, and the consumer when the buffer is empty
	- ![[../../_assets/semaphore solution 1.png | 500]]
	- ![[../../_assets/semaphore solution 2.png | 500]]
	- ![[../../_assets/semaphore solution 3.png | 500]]
- **Barriers**
	- Intended for groups of processes rather than two-process producer-consumer situation
	- Some applications are divided into phases and have the rule that no process may proceed into the next phase until all processes are ready to proceed (z.B Zwischenergebnisse müssen abgewartet werden)
	- This is achieved by placing a barrier at the end of each phase
	- **Example**:
		- ![[../../_assets/barrier example.png | 500]]
		- 4 Prozesse/Threads nähern sich der Barriere
		- Alle Prozesse/Threads bis auf einen sind blockiert
		- Nachdem der letzte Prozess die Barriere erreicht hat (bin-fertig-Signal), werden die PRozesse durchgelasse
## [[Classical IPC problems]]
### Resources:
[Microsoft PowerPoint - os6.pptx (cu.edu.tr)](https://ceng.cu.edu.tr/uorhan/DersNotu/os6.pdf)
### Dining philosophers
- **Aufgabestellung:** Philosopher picks up 2 forks, eat, then put down the forks and think
- Wrong solutions:
	- 1. Solution: take_fork() waits until the specified fork is available and seizes it. However, when all philosophers take their left forks simultaneously, none will be able to take their right forks => deadlock
		- ![[../../_assets/dining philosophers 1.png | 400]]
	- 2. Solution: After taking left fork, the program checks to see if the right fork is available. If not, the philosopher puts down the left one, waits, and repeats the process. However, if all philosophers start the algorithm simultaneously, the process will go on forever => Starvation
	- 3. Solution: Mutex semaphore => avoid starvation, but only 1 philosopher can eat at any instance
- **Right solution: semaphore, N, LEFT, RIGHT, THINKING, HUNGRY, EATING** => keine Deadlocks, maximale Parallelisierung
	- Speicherung auch der anderen Zustände: Einführung eines Feld state\[N], in dem für jeden Philosophen festgehalten wird, ob dieser gerade denkt, hungrig ist, oder isst (THINKING, HUNGRY, EATING)
	- Einführung eines Semaphorenfeldes s\[N]: hungrige Philosophen können ggf. blockieren, falls eine der benögtigten Gabeln gerade in Benutzung ist
	- ![[../../_assets/dining philosophers 2.png | 400]]
### The sleeping barber
- **Aufgabestellung:** A barbershop consists of a waiting room with n chairs, and the barber room containing the barber chair. If there are no customers to be served, the barber goes to sleep. If a customer enters the barbershop and all chairs are occupied, then the customer leaves the shop. If the barber is busy, but chairs are available, then the customer sits in one of the free chairs. If the barber is asleep, the customer wakes up the barber
- Our solution uses **three semaphores,** **customers (counts waiting customers), barbers (the number of barbers who are idle), and mutex (mutual exclusion)**. We also need a variable, **waiting**, which also counts the waiting customers. The reason for having waiting is that there is no way to read the current value of a semaphore. In this solution, a customer entering the shop has to count the number of waiting customers. If it is less than the number of chairs, he stays; otherwise, he leaves.
- When the barber shows up for work in the morning, he executes the procedure barber, causing him to block on the semaphore customers because it is initially 0. The barber then goes to sleep. He stays asleep until the first customer shows up. When a customer arrives, he executes customer, starting by acquiring mutex to enter a critical region. If another customer enters shortly thereafter, the second one will no be able to do anything until the first one has released mutex.
- The customer then checks to see if the number of waiting customers is less than the number of chairs. If not, he releases mutex and leaves without a haircut. If there is an available chair, the customer increments the integer variable, waiting. Then he does an up on the semaphore customers, thus waking up the barber. At this point, the customer and the barber are both awake. When the customer releases mutex, the barber grabs it, does some housekeeping, and begins the haircut
- ![[../../_assets/the sleeping barber.png | 400]]

## [[Scheduling]]
### Resources
[7 Scheduling.pptx - Yandex.Documents](https://docs.yandex.ru/docs/view?url=ya-disk-public%3A%2F%2FeobncyTJjHQCWXltYjg%2FBol8aRcEQ2sy1fERMVDsyEw%3D&name=7%20Scheduling.pptx)
[Operating Systems: CPU Scheduling (uic.edu)](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/6_CPU_Scheduling.html)
### Definition
- The part of the OS which makes the choice which process will run next is called the scheduler, its algorithm the scheduling algorithm
### Process behavior
![[../../_assets/process behavior.png | 400]]
- Fig. a: processes spend most of their time computing => compute-bound processes
- Fig. b: processes spend most of their time waiting for I/O => I/O-bound processes
### Scheduling for batch systems: Shortest job first scheduling
![[../../_assets/shortest job first.png | 400]]
- Scheduler picks the job with the shortest run-time first
- a: t_mean = (8 + 12 + 16 + 20) / 4 = 14
- b: t_mean = (4 + 8 + 12 + 20) /4 = 11
### Three level scheduling
![[../../_assets/three level scheduling.png | 400]]
- Admission scheduler: processes are first stored into an admission queue. Processes are then admitted in the system
- Memory scheduler: Swap in / swap out. To be avoided: Disk storage is expensive in terms of time
- CPU scheduler: chooses a job from memory by using a scheduling algorithm and processes it
- Kritieren für Memory Scheduler: Wie lange ist Prozess auf Platte? Letzter CPU-Verbrauch? Prozessgröße? Wie wichtig ist Prozess?
>[!NOTE]+ 3-Level-Scheduling-Verfahren
>1. Am Beginn dieses Schedulingverfahrens steht ein Input queue
>2. Nun kommt der _Admission Scheduler_ (in der Folge AS genannt) ins Spiel. Der AS entscheidet, welcher Job ins weitere System (genauer gesagt in den Speicher) kommt. Die anderen Jobs verbleiben im Input Queue.
>3. Vom Admission Scheduler werden die Jobs dann weiter in den RAM geladen. Hier beginnt nun die Arbeit des _Memory Schedulers_. Da jeder Job im Speicher Platz benötigt und es durchaus vorkommen kann, dass mehrere Jobs im Speicher vorhanden sind, entscheidet der Memory Scheduler, welcher Job im Hauptspeicher verbleibt und welcher auf die Festplatte ausgelagert wird (Swap)
>4. Der _CPU Scheduler_ entnimmt nun aus dem Speicher einen Job und arbeitet diesen ab (bzw. arbeitet weiter daran). Hierzu wird ein Scheduling-Verfahren (meist Shortest job next) verwendet.

### Scheduling for interactive systems
#### Round robin scheduling
- Each process is assigned a time interval, called its quantum. If the process is still running at the end of the quantum, the CPU is switched and given to another process. If the process has blocked or finished before the quantum has elasped, the CPU is also switched
![[../../_assets/round robin scheduling.png | 500]]
- a: Ranfolge der laufbereiten Prozesse
- b: Rangfolge der laufbereiten Prozesse, nachdem Prozess B sein Quantum aufgebraucht hat
#### Priority scheduling
- Priorities can be assigned to processes statically (assigned priority does not change anymore) or dynamically. It is often convenient to **group processes into priority classes and use round robin on each class**
- Scheduler may decrease the priority of the currently running process at each clock tick (to prevent high priority processes from running forever). If new priority drops below the next highest process, CPU switches
![[../../_assets/priority scheduling.png | 500]]
#### Lottery scheduling
- Give processes lottery tickets for CPU time => Process with the randomized lottery ticket gets the CPU.
- More important processes can be given extra tickets to increase theit odds of winning
- Idee:
	- Jeder Prozess erhält eine Anzahl Lose
	- Der Prozess, dessen Los gezogen wurde, erhält die Ressource
	- Prozesse mit höherer Priorität erhalten mehr Lose
	--> Im Mittel wird die Ressource gerecht zugeteilt
	--> Wegen möglicher Ausreißer nicht in Echtzeitsystemen nutzbar
	- Modifikation: zusammengehörende Prozesse können Lose austauschen --> Falls ein Client-Prozess blockier, überträgt er seine Lose dem Server-Prozess und erhält sie nach Blockadeende zurück
### Scheduling for real-time systems
#### Thread scheduling
- The process scheduler schedules only the kernel threads
- User threads are mapped to kernel threads by the thread library. The OS and the scheduler is unaware of them
- Scheduling of ULTs:
![[../../_assets/thread scheduling.png | 400]]
- Scheduling of KLTs:
![[../../_assets/thread scheduling 1.png | 400]]
#### Hyperthreading
![[../../_assets/hyperthreading (ohne).png | 400]]
![[../../_assets/hyperthreading (mit).png | 400]]
- Das OS emuliert weitere CPUs
- Dadurch werden Prozessor-Einheiten effektiver genutzt. z.B können float und int-Rechnungen parallel durchgeführt werden
- Hyperthreading nicht so leistungsfähig wie ein klassisches Multi-Prozessorsystem: die Threads in den logischen Prozessoren müssen sich dieselben physikalischen Prozessor-Einheiten teilen, wobei es zu Konflikten kommen kann.


## [[Java threads]]

