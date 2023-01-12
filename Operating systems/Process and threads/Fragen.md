1. Definition of heap (in process memory)
2. Defintion of stack (in process memory)
3. Difference between stack and heap (in process memory)
4. How is process memory divided?
5. What is the content of Stack, Heap, Daten when prog_a() finishes running? 
		![[../../_assets/process memory example 1.png | 250]]
5. What is the content of Stack, Heap, Daten when prog_a() finishes running? 
		![[../../_assets/process memory example 4.png | 250]]
6. Process creation procedure?
7. What are the types of process termination?
8. Describe the process states?
9. What is a process table? What is in a process table?
10. Give some examples of information in a process control block
11. What are threads? What are they used for?
12. Name and describe some applications of threads
13. What memory is shared between threads? Which isn't?
14. What is a thread table? What information is stored on a thread table?
15. Give some examples of the information stored on a thread table
16. What are the parts of thread memory?
17. Explain the following code excerpt:
```C
int sum;
void *runner(void *param); 

int main (int argc, char *argv[]) {
	pthread_t tid; 
	pthread_attr_t attr; 
	if (argc != 2) {
		fprintf(stderr, "usage: a.out <integer value>\n");
		return -1;
	}
	// _atoi_(const char *str) converts the string argument str to an integer
	if (atoi(argv[1]) < 0) {
		fprintf(stderr, "%d must be >= 0\n", atoi(argv[1]));
		return -1;
	}

	pthread_attr_init(&attr);
	pthread_create(&tid,&attr,runner,argv[1]);
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
18. Explain the following code excerpt:
```C
int zeit = 0; 

void *prog_a(void *arg) { 
	int i, t = 1;
	for (i = 0; i < 3; i++) {
		sleep(t); zeit = zeit + t;
		printf("%s%d: Zeit = %d s\n", (char *)arg, i, zeit);
	}
	return 0;
}

void *prog_b(void *arg) { 
	int i, t = 1;
	for (i = 0; i < 3; i++) {
		sleep(t); zeit = zeit + t;
		printf("%s%d: Zeit = %d s\n", (char *)arg, i, zeit);
	}
	return 0;
}

int main() {
	pthread_t id1, id2; 
	char *ch_id;
	void *res;

	ch_id="a_";
	pthread_create(&id1, NULL, prog_a, (void *)ch_id);
	printf("Thread a: ID=%p, Zeit=%d s\n", (void *)id1, zeit);

	ch_id="b_";
	pthread_create(&id2, NULL, prog_a, (void *)ch_id);
	printf("Thread a: ID=%p, Zeit=%d s\n", (void *)id2, zeit);

	pthread_join(id2, &res);
	printf("Gesamtzeit = %d s\n", zeit);
	return 0;
}
```
19. What are user-level threads? Sketch.
20. What are kernel-level threads? Sketch.
21. How can ULTs and KLTs be hybridly implemented? Sketch.
22. What are pop-up threads?
23. What are the advantages and disadvantages of pop-up threads?
24. When does race condition occur?
25. Give an example for race condition (print spooler)
26. What is a critical region?
27. What are the conditions for a solution to race conditions?
28. What is busy waiting?
29. What is a disadvantage of busy waiting?
30. What is the Dijkstra solution (Semaphore)?
31. What is the producer-consumer problem?
32. How can semaphores solve the producer-consumer problem?
33. What are barriers? How are they implemented?
34. How to solve the dining philosophers problem?
35. What does scheduling mean?
36. What are some scheduling algorithms for batch/interactive/real time systems? Describe these algorithms.

