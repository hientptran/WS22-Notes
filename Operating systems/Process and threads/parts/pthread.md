[[C Programming]]
[[Process and threads]]

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
