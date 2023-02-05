- Ready: Process is in a waiting loop. The scheduler chooses a process to load into the CPU
- Running: Process is being executed in the CPU. After some time, the process goes back to the waiting loop unless it's finished execution or is blocked.
- Blocked: The process waits for an event to happen. When the event has happened, the process will go back to the waiting loop

- Process table is an array of process control blocks and saves information about the state of the process. It will be updated at the beginning/end of a time slice. 

- Register, program counter, pointer, etc.

- Threads allow the parallel execution of processes. Each thread run independently from other threads, and have its own program counter. However, threads in the same process will share code, data and files.

- multithreaded web servers: requests will the received by the dispatcher thread. it will then choose a suitable worker thread to handle the request and wake this worker thread up -> web browser can handle multiple requests at the same time
- multithreaded text editor: one thread will handle user input, one thread will update the document, etc.

- Memory shared btween threads: data, text, heap; code, data, files
- Not shared: stack, register

- thread table: saves information on the state of the threads
	- per thread table: system aspects controlled by the thread --> program counter, register, stack, etc.
	- per process table: system aspects controlled by the process --> child processes, global variables, etc.

- Thread memory: Threads have their own register and stack but share code, file, and data with other threads of the same process