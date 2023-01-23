## [Antworten](Antworten.md)
## [Process and threads](Process%20and%20threads.md) 
1. Definition of heap (in process memory)
2. Defintion of stack (in process memory)
3. Difference between stack and heap (in process memory)
4. **What is a process?**
5. How is process memory divided?
6. What is the content of Stack, Heap, Daten before prog_a() finishes running? 
		![[../../_assets/process memory example 1.png | 250]]
7. What is the content of Stack, Heap, Daten when prog_a() finishes running? 
		![[../../_assets/process memory example 4.png | 250]]
8. Process creation procedure?
9. What are the types of process termination?
10. Describe the process states?
11. What is a process table? What is in a process table?
12. Give some examples of information in a process control block
13. **What are threads? What are they used for?**
14. Name and describe some applications of threads
15. **What memory is shared between threads? Which isn't?**
16. What is a thread table? What information is stored on a thread table? Give some examples of the information stored on a thread table
17. What are the parts of thread memory?
18. Explain the following code excerpt:
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
19. Explain the following code excerpt:
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
20. What are user-level threads? Sketch.
21. What are kernel-level threads? Sketch.
22. How can ULTs and KLTs be hybridly implemented? Sketch.
23. What are pop-up threads?
24. What are the advantages and disadvantages of pop-up threads?
25. **When does race condition occur?**
26. Give an example for race condition (print spooler)
27. **What is a critical region?**
28. **What are the conditions for a solution to race conditions?**
29. **What is busy waiting?**
30. What is a disadvantage of busy waiting?
31. What is the Dijkstra solution (Semaphore)?
32. What is the producer-consumer problem?
33. How can semaphores solve the producer-consumer problem?
34. What are barriers? How are they implemented?
35. How to solve the dining philosophers problem?
36. What does scheduling mean?
37. **What are some scheduling algorithms for batch/interactive/real time systems? Describe these algorithms.**
## [Deadlock](Deadlock.md)
1. What are deadlocks?
2. **When are deadlocks possible?**
3. **What are the conditions for deadlocks?**
4. What are considered resources?
5. What are preemptable/non-preemptable resources?
6. Sequence of events to use a resource?
7. What are the ways to recover from deadlocks?
8. What is safe/unsafe state?
9. What are the vectors in the banker algorithm?
10. **What are the ways to avoid deadlocks?**
11. How can non-resource deadlocks happen?
12. What are the ways to deal with deadlocks?
13. What is the banker's algorithm?

## [Memory management](Memory%20management.md)
1. What are the types of memory?
2. What does memory management mean?
3. What is monoprogramming?  
4. What is multiprogramming? What are the types of multiprogramming?
5. **What is the relocation problem?**
6. **What is the protection problem?**
7. **What is a solution to the relocation and protection problem?**
8. What is swapping? What is an disadvantage of swapping?
9. What are the ways to keep track of memory?
10. How is memory managed with bitmaps?
11. How is memory managed with linked lists?
12. What are the algorithms to load new processes into memory?
13. What is virtual memory?
14. **What is paging?**
15. Why do pages have to be replaced? What are the requirements for that?
16. What are some page replacement algorithms?
17. What is the difference between paging and swapping?
18. Describe the FIFO page replacement algorithm.
19. Describe the LRU page replacement algorithm.
20. How is the LRU page management algorithm implemented?
21. What are some problems of the LRU page replacement algorithm, and how to solve them?
22. What is the Belady anomaly?
23. What are stack algorithms?
24. What is the disadvantage of the FIFO page replacement algorithm?

## [File management](File%20management.md)
1. What are some data types?
2. What are 2 types of file access?
3. What does the following code do?
```C
#include <sys/types.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[]);

#define BUF_SIZE 4096 
#define OUTPUT_MODE 0700 

int main(int argc, char *argv[]) {
	int in_fd, out_fd, rd_count, wt_count;
	char buffer[BUF_SIZE];
	if (argc != 3) exit(1); 

	in_fd = open(argv[1], O_RDONLY); 
	if (in_fd < 0) exit(2); 
	out_fd = creat(argv[2], OUTPUT_MODE); 
	if (out_fd < 0) exit(3); 

	while (TRUE) {
		rd_count = read(in_fd, buffer, BUF_SIZE); 
		if (rd_count <= 0) break; 
		wt_count = write(out_fd, buffer, rd_count); 
		if (wt_count <= 0) exit(4); 
	}

	close(in_fd);
	close(out_fd);
	if (rd_count == 0) exit(0); 
	else exit(5) 
}
```
4. **What are memory-swapped files?**
5. What are the benefits of memory swapped files?
6. **What do the functions mmap() and munmap() do?**
7. What is the function of a file lock?=
8. What are the types of file locks?
9. What are the types of directories?
10. What are the types of file paths?
11. How does mmap() work?