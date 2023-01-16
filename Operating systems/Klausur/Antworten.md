## [Process and threads](Process%20and%20threads.md)
1. Heap contains dynamically allocated memory
2. Stack contains local variables and will be reserved when the local variable is declared. The stack will be freed up when the variable goes out of scope (when the process finishes running)
3. Heap vs Stack
- Heap contains dynamically allocated memory, and stacks contains local variables and can be freed up when the variable goes out of scope
4. **A process is an instance of a program in execution**
5. Process memory: Stack, Heap, Data, Text
6. Stack: i_a_1, i_a_2, heap: ip_a, data: i_g_1, i_g_2
7. Stack a: i_a_1, i_a_2, stack b: i_b_1, i_b_2, heap: ip_a, ip_b, data: i_g_1, i_g_2
8. **Process creation procedure:**
	- Systemstart
	- Ausführung eines Programms
	- Nutzeraktion
	- Start eines Batchjobs
9. Process termination:
	- Normaler Abbruch
	- Abbruch wegen eines Fehlers
	- Abbruch wegen eines schweren Fehler
	- **Abbruch durch einen anderen Prozess (kill)**
10. Process states:
	- Ready:  Prozesse befinden sich in einer Warteschleife. Scheduler wählt einen Prozess für die CPU aus
	- Running: Prozess wird von der CPU gearbeitet. Nach einer Zeitscheibe kommt der Prozess zurück in die Warteschleife, solange er nicht abgearbeitet oder blockiert ist
	- Blocked: Prozess wartet auf ein externes Ereignis und verbraucht kein CPU-Ressourcen. Beim Eintreten des Ereignisses kehrt der Prozess wieder in die Warteschleife zurück.
11. **Process table:**
	- Informationen über den Prozess werden in Process-Control-Blocks gespeichert.
	- Eine Prozesstabelle ist ein Array von Process-Control-Blöckern.
	--> Eine Process table speichert Informationen zum Prozesszustand. Diese Informationen werden am Anfang/am Ende der Zeitscheibe geladen bzw. gespeichert. 
12. Process control block: register, program counter, **pointer, process state, process number**
13. Threads ermöglichen die parallele Ausführung von Prozessen. Jeder Thread wird getrennt von anderen Threads des gleichen Prozess bearbeitet, und hat eine eigene Programmzähler. Die Threads eines Prozesses teilen sich jedoch einen gemeinsamen Speicher.
14. Some applications of threads:
	- Multithreaded web server: The dispatcher thread reads incoming requests and assign the request to a suitable worker thread (which is only woken up by the dispatcher thread when assigned a request) --> Each worker thread can handle a request: fetching image, loading text, etc.
	- Word processor: A thread can handle keyboard input, a thread can handle displaying text on the screen, a thread can handle spellchecking, etc.
15. Data, text and heap are shared between threads of the same process. The register and the stack isn't shared.
16. A thread table contains per process information and per thread information.
	- Per process information: Systemaspekte, die von allen Threads eines Prozesses geteilt werden z.B global variables, child processes, etc.
	- Per thread items: Systemaspekte, die jeder Thread selbst kontrolliert: Program counter, registers, stack, state
17. Thread memory: register, heap, data, text
18. Single thread 
```C
int sum;
void *runner(void *param); 

int main (int argc, char *argv[]) {
	pthread_t tid; //thread ID
	pthread_attr_t attr; //thread attribute
	// program must takes exactly 1 argument
	if (argc != 2) {
		fprintf(stderr, "usage: a.out <integer value>\n");
		return -1;
	}
	// _atoi_(const char *str) converts the string argument str to an integer
	// if the value < 0 --> error
	if (atoi(argv[1]) < 0) {
		fprintf(stderr, "%d must be >= 0\n", atoi(argv[1]));
		return -1;
	}
	// create thread attribute
	pthread_attr_init(&attr);
	// the function pthread_create creates a thread with the attributes attr and run the function runner with the argument argv[1]. Before returning, the function stores the thread id in the buffer pointed to by tid
	pthread_create(&tid,&attr,runner,argv[1]);
	// the function pthread_join waits for the thread tid to finish execution.
	pthread_join(tid,NULL);
	printf("sum = %d\n",sum);
}

void *runner(void *param) {
	int i, upper = atoi(param);
	sum = 0;
	for (i = 1; i <= upper; i++)
		sum += i;
	pthread_exit(0);
}
```
19. Multi-threaded
```C
int zeit = 0; 

void *prog_a(void *arg) { 
...
	return 0;
}

void *prog_b(void *arg) { 
...
	return 0;
}

int main() {
	pthread_t id1, id2; 
	char *ch_id;
	void *res;

	ch_id="a_";
	// the function pthread_create creates a thread with the attributes NULL and run the function prog_a with the argument ch_id. Before returning, the function stores the thread id in the buffer pointed to by id1
	pthread_create(&id1, NULL, prog_a, (void *)ch_id);
	printf("Thread a: ID=%p, Zeit=%d s\n", (void *)id1, zeit);

// the function pthread_create creates a thread with the attributes NULL and run the function prog_b with the argument ch_id. Before returning, the function stores the thread id in the buffer pointed to by id2
	ch_id="b_";
	pthread_create(&id2, NULL, prog_a, (void *)ch_id);
	printf("Thread a: ID=%p, Zeit=%d s\n", (void *)id2, zeit);

// the main thread waits for the threads id1 and id2 to terminate and saves the thread state to the buffer pointed to by res
	pthread_join(id1, &res);
	pthread_join(id2, &res);
	printf("Gesamtzeit = %d s\n", zeit);
	return 0;
}
```
20. User level threads are implemented from the user library, and is only visible to the user (not the kernel). ULTs are managed by the user, and the kernel manages them as a single threaded process.
![500](sketch-ult.png)
21. Kernel level threads are managed by the kernel. In addition to the process table, there is also a master thread table to manage all kernel level threads.
![500](klt-sketch.png)
22. Hybrid implementation of threads: The kernel is only aware of and will only manage kernel level threads. On top of kernel level threads may be additional user level threads.
![500](hybrid-threads-sketch.png)
23. Pop up threads wird beim Ankunft einer Mitteilung erzeugt.
24. Pop up threads
	- Vorteil: Vereinigung der Vorteile von ULTs und KLTs. **Kein blocked Threads, die auf Mitteilung wartet**
	- Nachteil: wenn es eine große Menge an Mitteilungen gibt -> ineffektiv
25. **Race condition occurs when 2 processes want to access the same global variable at the same time**
26. Example for race condition: print spooler
	- The spooler has 2 global variables in (index to add file to queue) and out (index of file to print next).
	- A process wants to add the file A into the queue. It reads the variable in (=7). Suddenly a time interrupt occurs before it puts the file in slot 7
	- Another process wants to add the file B into the printing queue. The variable in is still 7. Therefore this process adds the file B in slot 7 and increments the variable in. 
	- The first process runs again (in=7) and puts the file A in slot 7, overriding the file B and increments the variable in. The file B is not going to be printed, yet the daemon will not notice any inconsistency
27. **Kritische Region: Teil eines Programms, der auf einen gemeinsamen Speicher mehrerer Prozesse zugreift**
28. **Race condition solution:**
	- 2 processes cannot be in their critical region at the same time
	- No assumption may be made about the speed and number of CPUs
	- Any process running outside of their critical region cannot block other processes
	- Any process shouldn't have to wait forever to enter its critical region
29. Busy waiting: When a process continuously checks for a variable/waits for an event to occur. The CPU does nothing in this period -> wastes resources
30. Disadvantages of busy waiting
	- CPU does nothing during busy waiting -> Wastes resources
	- Violates condition 3 when one process is faster than the other
31. Dijkstra solution
	- Implements semaphore (1, 2, ..., N), up(), down()
	- When up() is called: semaphore++
	- When down() is called: semaphore-- (semaphore > 0), process is blocked (semaphore = 0)
	- **up and down sind atomar: sie werden vom Betriebssystem nicht unterbrochen, bevor sie beendet sind bzw. den Zustand blocked erreicht haben**
32. Producer-Consumer problem
	- 2 processes share a fixed sized buffer
		- Producer: puts information in the buffer
		- Consumer: takes information out of the buffer
	- Problem: Producer wants to add to a full buffer/consumer wants to read from an empty buffer
33. 3 semaphoren: mutex empty and full
	- mutex: 0,1 -> mutual exclusion: only 1 process can read from/write into the buffer
	- empty: initial value N; full: initial value 0 -> synchronisation: producer can't write to buffer when it is full (empty = 0, full = N), consumer can't read from buffer when it is empty (empty = N, full = 0)
34. Processes will be blocked at the barrier until all process arrives at the barrier -> No process can move into the next phase until all processes are ready to proceed
35. **semaphore, N, LEFT, RIGHT, THINKING, HUNGRY, EATING** => keine Deadlocks, maximale Parallelisierung
36. **The part of the OS which makes the choice which process will run next is called the scheduler, its algorithm the scheduling algorithm**
37. Scheduling algorithms
	- Batch system
		- Shortest job first: The average completion time is calculated and the process with the shortest one will be processed first
	- Interactive systems
		- **Three-level-scheduling: 3 Schedulers**
			- Admission scheduler: incoming processes will be put into an admission queue. The admission scheduler will choose a process to load into memory
			- Memory scheduler: Because processes need memory and sometimes there are multiple processes in the RAM, the memory scheduler will choose which one to swap out to disk/swap in from disk
			- CPU scheduler: The CPU scheduler will choose which process to be processed by the CPU
		- Round robin scheduling: Each process is assigned a quantum (a time interval). If the process is still running at the end of the quantum, the CPU will switch to another process. If the process finishes/is blocked before end of quantum, the CPU will also switch to next process.
		- Priority scheduling: Processes will be divided into priority classes. Processes in the highest priority class will be executed first (usually scheduled with the round robin algorithm)
		- Lottery scheduling: Each process is assigned a lottery ticket. Processes with higher priority classes will receive more tickets to increase the chance of being executed first. A random ticket would be drawn, and the process t=with the matching number will be executed.
	- Real-time systems
		- Thread scheduling: scheduling of ULTs (Kernel picks a process) and KLTs
		- Hyperthreading: the operating system will emulates multiple CPUs

## [Deadlock](Deadlock.md)
1. Deadlocks sind möglich, Prozesse exklusiven Zugriff auf Ressourcen haben
2. **A set of processes is deadlocked, when  each process is waiting for an event that can only be generated by an another process in the set. None of the processes can continue to run, release resources, or be freed.**
3. **Conditions for deadlocks:**
	1. Mutual exclusion: A resource can only be assigned to a process, or is available
	2. Hold and wait: A process which is currently assigned a resource, can ask for access to other resources
	3. Non-preemption: Access to a resource cannot be forcibly withdrawn from the process. Only a process can explicitly release resources
	4. Circular wait: A closed chain of 2 or more processes exists. Each process waits for a resource, to which the next process in the chain has access.
4. Ressourcen sind Geräte, auf die exklusiv zugegriffen werden kann (z.B Drucker, CD-Player, usw.)
5. Preemptable resources: Can be removed without any risk (Memory); Non-preemptable resources: Cannot be removed without risks (Printer)
6. Request resource - Use resource - Release resource
7. **Ways to recover from a deadlock:**
	- Recovery through preemption: Take a resource away from its current owner and give it to another process -> difficult
	- Recovery through rollbacks: The processes can make checkpoints periodically and can rollback to a previous checkpoint if a deadlock is detected
	- Recovery through killing processes: crude but simple: Kill one or more processes. It is best to kill a process that can be rerun with no negative effects
8. Safe state: The system can guarantee the the processes will be able to finish; Unsafe: the sytem cannot guarantee that the processes can finish
9. Vectors: P (All resources assigned to a process), E (All existing resources), A (Available resources which haven't been assigned) --> E = P + A
10. **Ways to avoid deadlocks: Negation of conditions**
	1. Mutual exclusion: A resource can be assigned to multiple processes -> Spooling resources
			-> Some resources (like printers) can be spooled: Multiple processes can generate output at the same time, but only the printer daemon has access to the physical printer and never requests any other resources 
	2. Non preemption: Forcibly take resources from a process
	3. Hold and wait: Require all processes to request all their resources before execution. If everything is available, the process will get what it needs and run to completion
	4. Circular wait: Order resources nummerically A process can only request resources in nummerical order
11. Non-resource (Communication) deadlocks can happen when processes wait for the result of other processes (process A is trying to send a message to process B, which is trying to send one to process C, which is trying to send one to A). They can also happen when processes use down() on semaphores (mutex and others) in the wrong order
12. **Ways to deal with deadlocks**
	1. Ignore the problem
	2. Detection and recovery
		1. Detection: Look for a cycle (1 copy of each resource), using resource vectors (multiple copies of each resource)
		2. Recovery: Through preemption, rollback, or killing processes
	3. Avoidance: Safe/unsafe states using the banker's algorithm
	4. Prevention: Make sure at least one of the conditions is never satisfied (spooling, taking away resources, make processes request resources before execution, order resources nummerically) 
13. Banker's algorithm: Like a small-town banker. The algorithm has to check to see if granting the request leads to an unsafe state or not.