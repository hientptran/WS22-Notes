[[Operating systems]]

### c basics
1. Compile: gcc -o hallo.c //write output to output file (.o)
2. Link: gcc hallo.o -o hallo
3. Run: ./hallo

### **classes**
```c
//vergleich.c
#include "myHeader.h"
extern my_sub();
int main() {
	my_sub(IMIN, IMAX);
	return 0;
}
//myHeader.h
//my_sub.c
```
### pointers
- ein Pointer ziehgt auf die Startadresse eines Datenobjekts im Hauptspeicher
- &: Speicheradresse eines Datenobjekts
- \*: liefert/ändert den Inhalt einer Speicheradresse
```c
#include <stdio.h>
int main() {
    int i = 42;
    // & benutzt um auf die Speicheradresse der Variable i zuzugreifen, und hat den Datentyp pointer (hier: integer pointer int*)
    // Pointer declaration: int* ptr -> ein Datentyp
    int* i_ptr = &i;
    printf("pointer=%p\ni=%d\n", i_ptr, i); //42
    
	// Pointer dereference: *ptr -> eine Operation
	*i_ptr = 44;
	printf("pointer=%p\ni=%d\n", i_ptr, i); //44

    int a = 30;
    int* a_ptr = &a;
    foo(a_ptr);
    printf("a=%d\n", a); //31
    
    return 0;
}
```

### gcc
```c
gcc hello.c-o hello
./hello
//or:
gcc hello.c
./a.out
```

### variables
```c
//TYPES:
int myint = 10;
float myfloat = 1.232;
char mychar = 'c';
char* mystr = "hello!";
//Boolean: 0 = false; a = false (a!=0)
//#include <stdbool.h> to use true flase
```

### operators and expressions
```c
#include <stdio.h> //for printf
y += x > 10? 17: 37; //if x > 10, add 17 to y. otherwise add 37 to y
int a = 4;
printf("%zu\n", sizeof a); //prints 4 because int; %zu is the format specifier for the tupe size_t
//---------------IF---------------
if (expr) {
	...;
} else {
	...;
}
//--------------WHILE-------------
int i = 0;
while (i < 10) {
	...;
	i++;
}
//-----------DO-WHILE--------------
do {
	...;
	i++
} while (i < 10);
//---------------FOR---------------
for (i = 0; i < 10; i++) 
	{ printf("i is %d\n", i); 
}
//-------------SWITCH---------------
switch (number) {
	case 0:
		...;
		break;
	case 1;
		...;
		break;
	default:
		...;
		break;
}
```
### arrays
```c
float f[4] = {1, 2, 3};//you can have fewer items in your initializer than there is room
float f[4] = {1, 2, 3, 0} //is the same as above
printf("%zu\n", sizeof f); //8*4=32
```
### strings
```c
char *s = "Hello world";
char t[] = "Hello again :)";
strlen(s); //returns string length
```
### functions
```c
//main function:
int main() {
	return 0;
}
// echo $? to see exit code
```
### file i/o  
read(), write() return int number of characters read/written
```c
#include <fcnt.h> //file control header for open()
#include <unistd.h> //read, write, close

int filehandle = open("filename.txt", flags: O_RDWR | O_CREAT | O_TRUNC, mode); 
//returns a file descriptor which is an int
//if file doesn't exist then create it, if exists empty it
// flags: O_RDONLY O_WRONLY O_RDWR O_CREAT (creates file if doesn't exist) O_TRUNC (clear)
// modes: S_IRUSR S_IWUSR -> user permissions
//-----------------------------------

char buffer[256]; //define buffer with space for 256 characters
char nread = read(filehandle, buffer, 256); 
// read() reads up to count (256) bytes from file descriptor into the buffer and returns number of bytes read. 0 signals end of file; -1 signals error
while (nread != 0) {
	printf("%c", nread);
}
printf("End of file");
//-----------------------------------

const char* buffer = "new content";
char nwrite = write(filehandle, buffer, 20); 
//writes up to count (20) bytes starting at buffer to the file referred
//on success, the number of bytes written is returned.
//-----------------------------------
close(filehandle);
```
### processes
```c
#include <unistd.h>

// fork() duplicates the parent process and creates child process. parent and child processes runs parallel
// fork() returns process id (pid) of the child process to the parent and 0 to the chile. returns -1 if there's an error
int main() {
	int child = fork(); //returns 0
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
#### Signals: kill()
- int kill(pid_t pid, int signr) --> schickt Signale an andere Prozesse
	- pid > 0: Das Signal signr wird an den Prozess pid geschickt
		- 1 = SIGUP: hang up
		- 2 = SIGINT = Ctrl+C: Tastatur-Interrupt
		- 9 = SIGKILL: riskant: Kindprozess, temporary data, shared memory existieren weiter
	- pid <= 0
	- Rückgabewerte: 0 Erfold; -1 Fehler

### pthread
[[pthread]]
#### pthread.c
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

#### multithreading.c
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

### pipes
- Eigenschaften von pipes:
	- pipes können nur zwischen PRozessen eingerichtet werden, die gemeinsame Vorgahren besitzen
	- Pipes sind halbduplex - Daten können immer nut in eine Richtung fließen
		- Entweder Elternprozess schreibt, Kindprozess liest oder umgekehrt
- **pipe**() creates a pipe, a unidirectional data channel that can be used for interprocess communication.  The array _pipefd_ is used to return two file descriptors referring to the ends of the pipe _pipefd\[0]_ refers to the read end of the pipe.  _pipefd\[1]_ refers to the write end of the pipe.  Data written to the write end of the pipe is buffered by the kernel until it is read from the read end of the pipe.
``` c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h> //strlen

int main() {
//0: read; 1: write
    int filehandles[2];
    //filehandles[0]: file descriptor zum Lesen aus der Pipe
    //filehandles[1]: file descriptor zum Schreiben in die Pipe
    printf("before: %d %d\n", filehandles[0], filehandles[1]);
    pipe(filehandles);
    printf("after: %d %d\n", filehandles[0], filehandles[1]);

    int child = fork();
    if (child == 0) {
        //child
        close(filehandles[0]); //close stdin
        const char* message = "Hello world!";
        write(filehandles[1], message, strlen(message)+1); //write in stdout
    }
    else {
        //parent
        //read or write from the PIPE, not from a file
        //here: read from stdin, closes stdout
        close(filehandles[1]);
        char buffer[512]; //pipe writes content from stdout into buffer
        read(filehandles[0], buffer, 512);
        printf("buffer: %s\n", buffer);
    }
    return 0;
}
```

