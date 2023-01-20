## Resources
[9 Memory Management.pptx - Yandex.Documents](https://docs.yandex.ru/docs/view?url=ya-disk-public%3A%2F%2Fyph0F2DD8pSephsSwoLxQ0Rwx2gwXe9aHPFbp0cXg%2B8%3D&name=9%20Memory%20Management.pptx&nosw=1)

## Introduction
- Anforderungen an Speicher: groß, schnell, unkaputtbar
- Speicher-Hierarchie:
	- Cache: kleinster Teil, schnell, teuer
	- Hauptspeicher: mittelschnell, mittelteuer
	- Plattenspeicher: sehr viel Platz, langsam, günstig
- Speicher-Management = Steuerung der Speicher-Hierachie

## Grundlagen
### Mono-Programmierung ohne Swapping oder Paging
![[../_assets/Pasted image 20221213164057.png | 500]]
- Bild: 3 einfache Arten der Speicherorganisation: OS mit einem Nutzerprozess
- Run one program at a time by sharing the memory between that program and the OS
- As soon as the user types a command, the OS copies the requested program from disk to memory and executes it
### Multi-Programmierung mit festen Partitionen
![[../_assets/Pasted image 20221213164215.png | 400]]
- Fixed partitions: Divide memory up into n partitions. When a job arrives, it can be put into the input queue for the smallest partition large enough to hold it
	- (a) fixed partition with separate input queues (disadvantage: queue for large partition is empty but queue for small partition is full)
	- (b) only one input queue: whenever a partition becomes free, the job at the front of queue that fits could be loaded into it
### Modellierung von Multiprogramming
I/O wait time is a metric used to measure the amount of time the CPU waits for disk I/O operations to complete. A high I/O wait time indicates an idle CPU and outstanding I/O requests (An outstanding request is one which has not been served yet)—while it might not make a system unhealthy, it will limit the performance of the CPU.
[G53OPS : Memory Management (nott.ac.uk)](https://www.cs.nott.ac.uk/~pszgxk/courses/g53ops/Memory%20Management/MM02-modelingmulti.html)
![[../_assets/Pasted image 20221213170431.png | 500]]
### Relokation und Protektion
- **In welche Speicherbereiche-/-partionen soll ein Programm geladen werden?**
	- Adressen von Variablen und Programmroutinen können nicht absolut angegeben werden: die absoluten Adressen eines kompilierten Binärcodes können nicht 1:1 als physikalische Speicheradressen genutzt werden -> Relokationsproblem
	- Ein Programm darf nicht in Speicherbereiche anderer Prozesse geschrieben werden -> Protektionsproblem
- **Relocation**
	- Addresses of variables and program routines cannot be absolute: the absolute addresses of a compiled binary code cannot be used 1:1 as physical memory addresses -> relocation problem
- **Protection**: 
	- A program must not be allowed to write into memory spaces of other processes -> protection problem
- **Mögliche Lösung: Nutzung von Basis- und Grenzadressen**
	- physikalische Adresse = absolute Adresse + Basisadresse (Lsg der Relokationsproblems)
	- Falls phys. Adressen > Grenzadresse => Fehler (Lsg des Protektionsproblems)
- **Solution: base and limit registers**
	- When a process is scheduled, the base register is loaded with the address of the start of the partition, and the limit register is loaded with the length of the partition
	- physical address = absolute address + base address (CALL 100 = CALL 100 + 100K)
	- If physical addresses is also checked: if physical address > limit register -> Error

## Swapping
- Excess processes are kept on disk and swapped in to run dynamically -> Memory allocation changes if processes are loaded into/leave the memory
- Nachteil: Schattierte Bereiche = ungenutzter Speicher
- Picture: Initially only Process A is in memory. Then processes B and C are created or swapped in from disk. In Fig. (d) A is swapped out to disk. Then D comes in and B goes out. Finally A comes in again
![[../_assets/Pasted image 20221213191026.png | 500]]
- Picture: Most processes grow as they run, it is probably a good idea to allocate a little extra memory whenever a process is swapped in or moved, to reduce the overhead of moving or swapping processes. When swapping processes to disk, only the memory actually in use should be swapped: it is wasteful to swap the extra memory as well.
![[../_assets/Pasted image 20221213191055.png | 500]]

## Memory management with bitmaps
- With a bitmap, memory is divided up into allocation units, perhaps as small as a few words and perhaps as large as several kilobytes. Corresponding to each allocation unit is a bit in the bitmap. If it is 0, the unit is free and if it is 1 it is occupied (not empty).
![[../_assets/Pasted image 20221213191520.png | 500]]
- (a) a part of memory with 5 processes and 3 holes. The tick marks show the memory allocation units. The shaded regions (0 in the bitmap) are free
- (b) the corresponding bitmap
- (c) the same information as list

## Memory management with linked lists
- Another way of keeping track of memory is to maintain a linked list of allocated and free memory segments, where a segment is either a process or a hope between 2 processes.
- Each entry in the list specifies a hole (H) or process (P), the address at which it starts, the length, and a pointer to the next entry
![[../_assets/Pasted image 20221213192054.png | 500]]
- In this example, the segment list is kept sorted by address. Sorting this way has the advantage that when a process terminates or is swapped out, updating the list is straightforward. A terminating process normaly has 2 neighbors. These may be either processses or holes, leading to the 4 combinations
- When the processes and holes are kept on a list sorted by address, several algorithms can be used to allocate memory for a newly created process (or an existing process being swapped in form disk)
- **Listen-Algorithmen, um neuen in Speicher zu laden:**
	- First fit: scans until the first hole that is big enough. The hole is then broken into 2 pieces, one for the used memory and one for the unused memory -> fast
	- Next fit: (minor variation of first fit) Starts searching from the list from the position where it left off during thew previous fit -> worse than first fit
	- Best fit: Searches entire list and takes the smalles holoe that is appropriate -> slower, Minilöcherproblem
	- Quick fit: Tabelle mit Pointern auf Löcher bestimmter Größe (has separate lists with pointers to the holes with more common sizes requested)

## Virtual memory & Paging
- **Virtual memory**: OS keeps parts of the program currently in use in main memory, and the rest on the disk. Pieces of the program can be swapped between disk and memory as needed
- **Paging**: Welcher Prozess wird in den ausgelagerten Hauptspeicher auf der Festplatte geschoben?
	- Program generates virtual addresses and form the virtual address space. When virtual memory is used, the virtual addresses do not go directly to the memory bus. Instead, they go to an MMU (Memory management unit) that maps the virtual addreses onto the physical memory
	- The virtual address space is divided into pages. The corresponding units in the physical memory are called page frames -> this information is stored on a page table
- **Page replacement
	- When a page fault occurs, the OS has to choose a page to remove from memory to make space for the page that has to be brought in
	- If the removed page has been modified while in memory, it must be rewritten to the disk
	- If the page has not been changed, no rewrite is needed
	- A page which is not frequently used should be chosen, because there's a high chance that it will not be needed again
- **Algorithms:** FIFO, LRU
- **Swapping**: Welche Lücke im Hauptspeicher erhält ein Prozess?

## Page-Ersetzungsalgorithmen
- **First in first out (FIFO):** manages a list of all pages, which is sorted by origin. The first page on the list will be replaced (first in first out)
	--> Disadvantage: The page which stays the longest in the memory is usually the one frequently used (because the swapped in page is at the end of the queue)
	--> pure FIFO is rarely used
- **Least recently used (LRU):** 
	- **Assumption**: page which has just been used will be used again soon --> remove page which hasn't been used for the longest time
	- Management of a linked list of pages: pages recently used in front, pages not used at the back
	- Problem: needs management
		- List needs to be updated after each memory reference
		- It is time consuming to look for and move a page in the list
		- Needs special hardware
	- **Simpler LRU algorithm:**
		- A counter is incremented after each page call
		- Current counter value will be saved on the pages
		- Remove page with lowest counter value
		- Counter value will be periodically set to 0
- **LRU algorithm:** 
	- If we have n page table entries a matrix of n x n bits, initially all zero, is maintained. 
	- When a page frame, k, is referenced then all the bits of the k row are set to one and all the bits of the k column are set to zero. 
	- At any time the row with the lowest _**binary**_ value is the row that is the least recently used (where row number = page frame number). The next lowest entry is the next recently used; and so on.

## Modelling page replacement algorithms
- **Belady anomaly:** more page frames doesn't reduce the number of page faults
![500](belady%20anomaly.png)
- **Stack algorithm:** [Belady's Anomaly | Page Fault | Paging | Gate Vidyalay](https://www.gatevidyalay.com/beladys-anomaly-page-fault-paging/)
![500](stack%20algorithmen.png)
- Zustand des Speicher-Arrays M, nachdem jeder Eintrag im Referenz-String nach LRU Algorithmus prozessiert wurde: jetzt führt einem Hauptspeichervergrößerung zu weniger Page-Fehlern.
- Definition: Ein Page-Ersetzungsalg. heißt Stackalgorithmus, falls für jede Position r des Ref.-Strings gilt: M(m,r) ⊆ M(m+1,r) (m=#Page-Frames) (d.h.: bei Speichervergrößerung treten nicht mehr Page-Fehler auf als zuvor)