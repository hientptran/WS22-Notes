[[Process and threads]]
## Threads
#### Resources
[Operating System Concepts: 4. Threads & Concurrency (yale.edu)](https://view.officeapps.live.com/op/view.aspx?src=https%3A%2F%2Fcodex.cs.yale.edu%2Favi%2Fos-book%2FOS10%2Fslide-dir%2FPPTX-dir%2Fch4.pptx&wdOrigin=BROWSELINK)
[Operating Systems: Threads (uic.edu)](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/4_Threads.html)
[5 Threads.pptx - Yandex.Documents](https://docs.yandex.ru/docs/view?url=ya-disk-public%3A%2F%2FCl65XG0DVTm1nBNKaTaPKALbXtzTPynvGQyMPqWpI5Q%3D&name=5%20Threads.pptx)
### Definition/Application
- Threads ermöglichen **parallele Ausführung von Prozessen** (=> Performance)
	- Jeder Thread wird getrennt von den anderen Threads des Prozesses bearbeitet
	- Jeder Thread besitzt einen eigenen Programmzähler
- Threads teilen sich einen gemeinsamen Speicher (=> Synchronisation)
- Examples/Application: 
	- In a browser, multiple tabs can be different threads
	- **Multithreaded web server:** 
		- Multiple threads allow for multiple requests to be satisfied simultaneously, without having to service requests sequentially or to fork off separate processes for every incoming request.
		- Dispatcher reads incoming requests from the netzrok. After examining the request, it chooses an idle worker thread and hands it the request. The dispatcher then wakes up the sleeping worker, moving it from blocked status
		- ![[../../../_assets/thread application example 2.png]]
	- **Word processors** uses multiple threads: one thread to format the text, another thread to process inputs, etc. (a background thread may check spelling and grammar while a foreground thread processes user input ( keystrokes ), while yet a third thread loads images from the hard drive, and a fourth does periodic automatic backups of the file being edited.)
		- ![[../../../_assets/thread application example 1.png]]
> [!NOTE]- Process vs Thread
> The primary difference is that threads within the same process run in a shared memory space, while processes run in separate memory spaces.  
> Threads are not independent of one another like processes are, and as a result threads share with other threads their code section, data section, and OS resources (like open files and signals). But, like process, a thread has its own program counter (PC), register set, and stack space.

### Thread table
- Per process items: Systemaspekte, die von allen Threads eines Prozesses geteilt werden
	- Die Threads eines Prozesses gehören demselben Nutzer => geringere Sicherheitsanforderungen
- Per thread items: Systemaspekte, die jeder Thread selbst kontrolliert

### Thread memory
![[../../../_assets/thread memory.png | 400]]
- Jeder Thread besitzt einen eigenen Stack und Registers
- Threads teilen sich einen gemeinsamen Speicher (code, data, files)

### pthread
[[C Programming]]
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
- Kernel doesn't know about the user-lever thread and manages them as if they were singe-threaded processes
- Thread wird vom Nutzer verwaltet (Stack scheduling, usw.)
![[../../../_assets/user level thread.png | 300]]
### Kernel level threads
- Kernel knows and manages the threads
- Instead of thread table in each process, the kernel has a master thread table that keeps track of all the threads in the system
- In addition, kernel also maintains the traditional process table to keep track of the processes. Kernel provides system calls to create and manage threads
- Threads wird com Kernel verwaltet => einfacher, aber weiniger performant als ULT
![[../../../_assets/Kernel level threads.png | 300]]
### Hybrid implementation
- Vereinigung der Vorteile beider Methoden (ULT, KLT), unter Auslassung ihrer Nachteile
- The kernel is aware of only the kernel-level threads and schedule those. Some of those threads may have multiple ULT on top of them
![[../../../_assets/Hybrid implementation ULT KLT.png | 300]]
### Pop-up threads
- Thread wird bei Ankunft einer Mitteilung erzeugt (a: vor Ankuft; b: nach Ankunft)
- Vorteil: kein blocked Thread, der auf Mitteilung wartet
- Nachteil: nicht günstig, falls zu viele Mitteilungen in kurzer Zeit - hier wäre eine Thread-Pool mit blocked Threads ressourcensschonender
![[../../../_assets/Popup threads.png | 300]]
