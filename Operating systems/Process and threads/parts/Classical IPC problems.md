[[Process and threads]]
## Classical IPC problems
### Resources
[Microsoft PowerPoint - os6.pptx (cu.edu.tr)](https://ceng.cu.edu.tr/uorhan/DersNotu/os6.pdf)
### Dining philosophers
- **Aufgabestellung:** Philosopher picks up 2 forks, eat, then put down the forks and think
- Wrong solutions:
	- 1. Solution: take_fork() waits until the specified fork is available and seizes it. However, when all philosophers take their left forks simultaneouslyy, none will be able to take their right forks => deadlock
		- ![[../../../_assets/dining philosophers 1.png | 400]]
	- 2. Solution: After taking left fork, the program checks to see if the right fork is available. If not, the philosopher puts down the left one, waits, and repeats the process. However, if all philosophers start the algorithm simultaneously, the process will go on forever => Starvation
	- 3. Solution: Mutex semaphore => avoid starvation, but only 1 philosopher can eat at any instance
- **Right solution: semaphore, N, LEFT, RIGHT, THINKING, HUNGRY, EATING** => keine Deadlocks, maximale Parallelisierung
	- ![[../../../_assets/dining philosophers 2.png | 400]]
### The sleeping barber
- **Aufgabestellung:** A barbershop consists of a waiting room with n chairs, and the barber room containing the barber chair. If there are no customers to be served, the barber goes to sleep. If a customer enters the barbershop and all chairs are occupied, then the customer leaves the shop. If the barber is busy, but chairs are available, then the customer sits in one of the free chairs. If the barber is asleep, the customer wakes up the barber
- Our solution uses **three semaphores,** **customers (counts waiting customers), barbers (the number of barbers who are idle), and mutex (mutual exclusion)**. We also need a variable, **waiting**, which also counts the waiting customers. The reason for having waiting is that there is no way to read the current value of a semaphore. In this solution, a customer entering the shop has to count the number of waiting customers. If it is less than the number of chairs, he stays; otherwise, he leaves.
- When the barber shows up for work in the morning, he executes the procedure barber, causing him to block on the semaphore customers because it is initially 0. The barber then goes to sleep. He stays asleep until the first customer shows up. When a customer arrives, he executes customer, starting by acquiring mutex to enter a critical region. If another customer enters shortly thereafter, the second one will no be able to do anything until the first one has released mutex.
- The customer then checks to see if the number of waiting customers is less than the number of chairs. If not, he releases mutex and leaves without a haircut. If there is an available chair, the customer increments the integer variable, waiting. Then he does an up on the semaphore customers, thus waking up the barber. At this point, the customer and the barber are both awake. When the customer releases mutex, the barber grabs it, does some housekeeping, and begins the haircut
- ![[../../../_assets/the sleeping barber.png | 400]]
