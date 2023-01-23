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
	- System initialization
	- Execution of a program
	- A user request to create a new thread
	- Initiation of a batch job
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
27. **Critical region: Part of the program where the shared memory is accessed
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
1. Deadlocks sind möglich, wenn Prozesse exklusiven Zugriff auf Ressourcen haben
2. **A set of processes is deadlocked, when each process is waiting for an event that can only be generated by an another process in the set. None of the processes can continue to run, release resources, or be freed.**
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

## [Memory management](Memory%20management.md)
1. Types of memory
	- Cache: klein, schnell, teuer
	- Hauptspeicher: mittelschnell, mittelteuer
	- Plattenspeicher: langsam, günstig, groß
2. Memory management = Steuerung der Speicher-Hierachie
3. Monoprogramming: Runs 1 program at a time by sharing the memory with the OS
4. Multiprogramming: Runs multiple programms at the same time by partitioning the memory
	- 1 input queue: all programs will be loaded into one input queue. Whenever a partition is freed up, the program at the start of the queue that fits in the partition will be loaded in
	- multiple input queues: each queue goes in one partition
5. Relocation problem: When a program is run it does not know in advance what location it will be loaded at. Therefore, the program cannot simply generate static addresses (e.g. from jump instructions). Instead, they must be made relative to where the program has been loaded.
6. Protection problem: Once you can have two programs in memory at the same time there is a danger that one program can write to the address space of another program.
7. Solution to relocation and protection problem: using base and limit registers
	- When a process is scheduled, the base register is loaded with the start address of the partition, and the limit register is loaded with the length of the partition
	- physical address = absolute address + base address
	- If physical addresses is also checked: if physical address > limit register -> Error
8. Swapping: Excess processes are kept on the disk and swapped in when needed. Memory allocation is dynamically changed when processes are swapped in/swapped out of memory. The disadvantage of swapping is when the memory needed for the process is smaller than the size of the partition, there will be memory in the partition that isn't utilized.
9. Ways to keep track of memory: bitmap (1 is occupied, 0 is free), linked list (Hole/Process, starts at, end)
10. bitmap (1 is occupied/process, 0 is free/hole)
11. Linked list: Each entry in the list specifies a hole (H) or process (P), the address at which it starts, the length, and a pointer to the next entry
12. Algorithms to load new processes into memory:
	- First fit: first hole with enough place, starts from beginning
	- Next fit: doesn't start from the beginning but where the last iteration left of
	- Best fit: goes through the entire list and choose the smallest hole that fits
	- Quick fit: Has separate lists with pointers to the holes with the most used sizes
13. Virtual memory: OS keeps parts of the program currently in use in main memory and the other parts in the disk. When a part of the program is needed, it can be swapped in.
14. Paging:  program generates virtual addresses and form the virtual address space. When virtual memory is used, the virtual addresses do not go directly to the memory bus. Instead, they go to and MMU that maps the virtual addresses onto the physical memory. The virtual address space is divided into pages. The corresponding units in the physical memory are called page frames. This information is stored on a page table.
15. Pages need to be replaced when a page fault occurs. The OS has to choose a page to remove from memory to make space for the page that has to be brought in
16. Page replacement algorithm: FIFO, LRU
17. FIFO page replacement: first in first out - manages a list of pages, the first page on this list will be replaced first -> Disadvantage: The page that stays longest in memory (at the end of queue and is swapped out) is the one most frequently used.
18. Least recently used page replacement: It is assummed that the page which has just been used will be used again soon -> Swap out least-used pages. A linked list of pages is managed, with pages recently used in front and pages not recently used at the back
19. The LRU hardware saves a nxn matrix (initially only 0). When a page is used, all values on the n row will be set to to 1 and all values on the n column will be set to 0. At any time, the row with the lowest binary value is least recently used.
20. Problems with LRU: the matrix needs to be updated after each page call, time-consuming when finding and moving a page, needs special hardware
21. **Belady anomaly:** more page frames do not reduce the number of page faults
22. Stack algorithms: no Belady anomaly
23. Disadvantage of FIFO page replacement: On one hand, the page replaced may be an initialization module that was used a long time ago and is no longer needed. On the other hand, it could contain a heavily used variable that was initialized early and is in constant use.

## [File management](File%20management.md)
1. Some data types: ASCII files, binary files, archive files
2. 2 types of file access: sequential and random access
3. Code:
```C
#include <sys/types.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[]);

#define BUF_SIZE 4096 //define buffer size
#define OUTPUT_MODE 0700 //define output mode

int main(int argc, char *argv[]) {
	int in_fd, out_fd, rd_count, wt_count; // initialize integer values for
										   // i/o file handles and return values
										   // of read() and write()
	char buffer[BUF_SIZE]; // initialize buffer for reading/writing file
	if (argc != 3) exit(1); // if there are less/more than 2 arguments given
							// --> error

	in_fd = open(argv[1], O_RDONLY); // open file specified by first argument
									 // read only rights, return file handle
	if (in_fd < 0) exit(2); // if fail to read (open returns -1), exit
	out_fd = creat(argv[2], OUTPUT_MODE); // create file with the file name specified by 2nd argument, output mode is specified above
	if (out_fd < 0) exit(3); //if fail to create new file (creat() returns -1),
							// exit program

	while (TRUE) {
		rd_count = read(in_fd, buffer, BUF_SIZE); //read buf_size bytes blocks from file pointed to by in_fd and save to buffer. read() returns number of bytes read
		if (rd_count <= 0) break; // if end of file (rd_count = 1) or fail to read (rd_count < 0), break
		wt_count = write(out_fd, buffer, rd_count); //write rd_count bytes blocks from buffer and write to file pointed to by in_fd. write() returns number of bytes written
		if (wt_count <= 0) exit(4); // if end of file (wt_count = 1) or fail to write (wr_count < 0), break
	}

	close(in_fd); //close input/output file handles 
	close(out_fd);
	if (rd_count == 0) exit(0); //if end of file --> exit with exit code 0
	else exit(5) //error
}
```
4. Memory swapped files: Abbildung einer Datei (auf Festplatte) in den Adressraum eines Prozesses (im Hauptspeicher). The mapping of a file (on disk) in the address space of the calling process (in main memory)
5. Benefits of memory swapped files:
	- Performace: schneller Ersatz für die Systemaufrufe read, write, seek
	- Integrierter Mechanismus für dynamisches Binden (= Laden von Programmteilen während der Laufzeit)
	- Integrierter File-Sharing-Mechanismus
6. mmap() creates new file mapping in the virtual address space of the calling process. munmap() removes this mapping.
7. File lock: the OS blocks the process which wants access to a file currently accessed by another process
8. Types of file locks:
	- Exclusive lock (write lock): only 1 process has access to the file
	- Shared lock (read lock): multiple processes can have acces to the file
9. Types of directories: 1 layer, 2 layer, hierachical
10. Types of file paths: absolute path, relative path, special directories (. or ..)
11. mmap() establishes a mapping between the address space of the process at an address _addr_ for _length_ bytes to the memory object represented by the file descriptor _fd_ at offset _offset_ for _length_ bytes.
