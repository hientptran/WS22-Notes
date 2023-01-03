[[Process and threads]]
## Interprocess communication
## Rescources
Advanced: [Chapter 0   Preface — Computer Systems Fundamentals (jmu.edu)](https://w3.cs.jmu.edu/kirkpams/OpenCSF/Books/csf/html/index.html)
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
> ![[../../../_assets/race conditions example.png | 300]]
> 
> Source: [6 IPC.pptx (live.com)](https://view.officeapps.live.com/op/view.aspx?src=https%3A%2F%2Fweb.karabuk.edu.tr%2Fyasinortakci%2Fdokumanlar%2Fisletim_sistemleri%2F6%2520IPC.pptx%23%3A~%3Atext%3DCritical%2520Regions%26text%3DThat%2520part%2520of%2520the%2520program%2Cfrom%2520doing%2520the%2520same%2520thing.&wdOrigin=BROWSELINK)



### Critical region
- Teil eines Programm, der auf einen gemeinsamen Speicher mehrerer Prozesse zugreift
- ![[../../../_assets/critical region.png | 400]]
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
	- ![[../../../_assets/Busy waiting example.png | ]]
	- An int variable turn (initially 0) keeps track of whose turn it is to enter the critical region and examine or update the shared memory. 
	- When process 1 leaves the critical region, it sets turn to 1, to allow process 2 to enter its critical region. Suppose that process 2 finishes its noncritical region, so both processes are in their noncritical regions (turn = 0)
	- Suddenly, process 2 finishes its noncritical region before process 1 finishes its noncritical region and goes back to the top of its loop. It is not permitted to enter critical region (turn = 0, needs turn = 1) => violates condition 3
	- ![[../../../_assets/Busy waiting example.png]]
	- Above: process 2; Below: Process 1![[../../../_assets/busy waiting problem.png]]
#### Semaphore
- New data type Semaphore and the processes up and down
	- Semaphore = 0, 1, 2,...: int value to count the number of wakeup processes saved for future use
	- up and down:
		- up => Semaphone++
		- Semaphore > 0: down => Semaphore--
		- Semaphore = 0: down => down = blocked (process is put to sleep)
- up und down sind atomar: sie werden vom Betriebssystem nicht unterbrochen, bevor sie beendet sind bzw. den Zustand blocked erreicht haben (when a semaphore operation has started, no other process can access the semaphore until the operation has completed or blocked)
- **Producer-consumer problem:**
	- Two processes share a common, fixed-size buffer. One of them, the producer, puts information into the buffer, and the other one, the comsumer, takes it out
	- Problem: When the producer wants to put a new item in an already full buffer/producer wants to get items from an empty buffer
	- The solution is for the producer to go to sleep, and sleeping producer will be awakened when the consumer has removed one or more items or vice versa
- **Semaphore solution:**
	- **mutex = 0, 1 (initially 1 true)** (binäre Semaphore) regelt den wechselseitigen Ausschluss (mutual exclusion) => Nur ein Prozess kann pro Zeit in den Puffer schreiben/aus dem Puffer lesen
	- **empty (initially N), full (initially 0) = 0, 1, ... N** dienen der Synchronisation => Bestimmte Ereignisse treten ein bzw. treten nicht ein (hier: leerer/voller Puffer). Ensures that the producer stops running when the buffer is full, and the consumer when the buffer is empty
	- ![[../../../_assets/semaphore solution 1.png | 500]]
	- ![[../../../_assets/semaphore solution 2.png | 500]]
	- ![[../../../_assets/semaphore solution 3.png | 500]]
- **Barriers**
	- Intended for groups of processes rather than two-process producer-consumer situation
	- Some applications are divided into phases and have the rule that no process may proceed into the next phase until all processes are ready to proceed (z.B Zwischenergebnisse müssen abgewartet werden)
	- This is achieved by placing a barrier at the end of each phase
	- **Example**:
		- ![[../../../_assets/barrier example.png | 500]]
		- 4 Prozesse/Threads nähern sich der Barriere
		- Alle Prozesse/Threads bis auf einen sind blockiert
		- Nachdem der letzte Prozess die Barriere erreicht hat (bin-fertig-Signal), werden die PRozesse durchgelasse

