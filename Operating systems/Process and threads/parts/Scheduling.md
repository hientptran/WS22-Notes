1### Resources
[7 Scheduling.pptx - Yandex.Documents](https://docs.yandex.ru/docs/view?url=ya-disk-public%3A%2F%2FeobncyTJjHQCWXltYjg%2FBol8aRcEQ2sy1fERMVDsyEw%3D&name=7%20Scheduling.pptx)
[Operating Systems: CPU Scheduling (uic.edu)](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/6_CPU_Scheduling.html)
### Definition
- The part of the OS which makes the choice which process will run next is called the scheduler, its algorithm the scheduling algorithm
### Process behavior
![[../../../_assets/process behavior.png | 400]]
- Fig. a: processes spend most of their time computing => compute-bound processes
- Fig. b: processes spend most of their time waiting for I/O => I/O-bound processes
### Scheduling for batch systems
#### Shortest job first scheduling
![[../../../_assets/shortest job first.png | 400]]
- Scheduler picks the job with the shortest run-time first
- a: t_mean = (8 + 12 + 16 + 20) / 4 = 14
- b: t_mean = (4 + 8 + 12 + 20) /4 = 11
### Three level scheduling
![[../../../_assets/three level scheduling.png | 400]]
- Admission scheduler: processes are first stored into an admission queue. Processes are then admitted in the system
- Memory scheduler: Swap in / swap out. To be avoided: Disk storage is expensive in terms of time
- CPU scheduler: chooses a job from memory by using a scheduling algorithm and processes it
- Kritieren für Memory Scheduler: Kriterien für Memory Scheduler: Wie lange ist Prozess auf Platte? Letzter CPU-Verbrauch? Prozessgröße? Wie wichtig ist Prozess?
>[!NOTE] 3-Level-Scheduling-Verfahren
>1. Am Beginn dieses Schedulingverfahrens steht ein Input queue
>2. Nun kommt der _Admission Scheduler_ (in der Folge AS genannt) ins Spiel. Der AS entscheidet, welcher Job ins weitere System (genauer gesagt in den Speicher) kommt. Die anderen Jobs verbleiben im Input Queue.
>3. Vom Admission Scheduler werden die Jobs dann weiter in den RAM geladen. Hier beginnt nun die Arbeit des _Memory Schedulers_. Da jeder Job im Speicher Platz benötigt und es durchaus vorkommen kann, dass mehrere Jobs im Speicher vorhanden sind, entscheidet der Memory Scheduler, welcher Job im Hauptspeicher verbleibt und welcher auf die Festplatte ausgelagert wird (Swap)
>4. Der _CPU Scheduler_ entnimmt nun aus dem Speicher einen Job und arbeitet diesen ab (bzw. arbeitet weiter daran). Hierzu wird ein Scheduling-Verfahren (meist Shortest job next) verwendet.

### Scheduling for interactive systems
#### Round robin scheduling
- Each process is assigned a time interval, called its quantum. If the process is still running at the end of the quantum, the CPU is switched and given to another process. If the process has blocked or finished before the quantum has elasped, the CPU is also switched
![[../../../_assets/round robin scheduling.png | 500]]
- a: Ranfolge der laufbereiten Prozesse
- b: Rangfolge der laufbereiten Prozesse, nachdem Prozess B sein Quantum aufgebraucht hat
#### Priority scheduling
- Priorities can be assigned to processes statically (assigned priority does not change anymore) or dynamically. It is often convenient to group processes into priority classes and use round robin on each class
- Scheduler may decrease the priority of the currently running process at each clock tick (to prevent high priority processes from running forever). If new priority drops below the next highest process, CPU switches
![[../../../_assets/priority scheduling.png | 500]]
#### Lottery scheduling
- Give processes lottery tickets for CPU time => Process with the randomized lottery ticket gets the CPU.
- More important processes can be given extra tickets to increase theit odds of winning
- Idee:
	- Jeder Prozess erhält eine Anzahl Lose
	- Der Prozess, ddessen Los gexogen wurde, erhält die Ressource
	- Prozesse mit höherer Priorität erhalten mehr Lose. "Im Mittel" wird die Ressource "gerecht" zugeteilt
- Modifikation: "zusammengehörende" Prozesse können Lose austauschen
### Scheduling for real-time systems
#### Thread scheduling
- The process scheduler schedules only the kernel threads
- User threads are mapped to kernel threads by the thread library. The OS and th scheduler is unaware of them
- Scheduling of ULTs:
![[../../../_assets/thread scheduling.png | 400]]
- Scheduling of KLTs:
![[../../../_assets/thread scheduling 1.png | 400]]
#### Hyperthreading
![[../../../_assets/hyperthreading (ohne).png | 400]]
![[../../../_assets/hyperthreading (mit).png | 400]]
- Das OS emuliert weitere CPUs
- Dadurch werden Prozessor-Einheiten effektiver genutzt. z.B können float und int-Rechnungen parallel durchgeführ werden
- Hyperthreading nicht so leistungsfähig wie ein klassisches Multi-Prozessorsystem: die Threads in den logischen Prozessoren müssen sich dieselben physikalischen Prozessor-Einheiten teilen, wobei es zu Konflikten kommen kann.