## Resources
[9 Memory Management.pptx - Yandex.Documents](https://docs.yandex.ru/docs/view?url=ya-disk-public%3A%2F%2Fyph0F2DD8pSephsSwoLxQ0Rwx2gwXe9aHPFbp0cXg%2B8%3D&name=9%20Memory%20Management.pptx&nosw=1)

## Introduction
- Anforderungen an Speicher: groß, schnell, unkaputtbar
- Speicher-Hierarchie:
	- Cache: kleinster Teil, schnell, teuer
	- Hauptspeicher: mittelschnell, mittelteuer
	- Plattenspeicher: sehr viel Platz, langsam, günstig

## Grundlagen
### Mono-Programmierung ohne Swapping oder Paging
![[../_assets/Pasted image 20221213164057.png | 500]]
- Bild: 3 einfache Arten der Speicherorganisation: OS mit einem Nutzerprozess
- Run one program at a time by sharing the memory between that program and the OS
### Multi-Programmierung mit festen Partitionen
![[../_assets/Pasted image 20221213164215.png | 400]]
- Feste Speicherpartitionen: (a) getrennete Eingabeschlangen für jede Partition; (b) eine einzige Eingabeschlange
### Modellierung von Multiprogramming
I/O wait time is a metric used to measure the amount of time the CPU waits for disk I/O operations to complete. A high I/O wait time indicates an idle CPU and outstanding I/O requests (An outstanding request is one which has not been served yet)—while it might not make a system unhealthy, it will limit the performance of the CPU.
[G53OPS : Memory Management (nott.ac.uk)](https://www.cs.nott.ac.uk/~pszgxk/courses/g53ops/Memory%20Management/MM02-modelingmulti.html)
![[../_assets/Pasted image 20221213170431.png | 500]]
### Relokation und Protektion
- **In welche Speicherbereiche-/-partionen soll ein Programm geladen werden?**
	- Adressen von Variablen und Programmroutinen können nicht absolut angegeben werden: die absoluten Adressen eines kompilierten Binärcodes können nicht 1:1 als physikalische Speicheradressen genutzt werden -> Relokationsproblem
	- Ein Programm darf nicht in Speicherbereiche anderer Prozesse geschrieben werden -> Protektionsproblem
- **Mögliche Lösung: Nutzung von Basis- und Grenzadressen**
	- physikalische Adresse = absolute Adresse + Basisadresse (Lsg der Relokationsproblems)
	- Falls phys. Adressen > Grenzadresse => Fehler (Lsg des Protektionsproblems)

## Swapping
- Speicherbelegung ändert sich, falls Prozesse in den Speicher geladen werden/den Speicher verlassen
- Nachteil: Scchattierte Bereiche = ungenutzter Speicher
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
- Listen-Algorithmen, um neuen in Speicher zu laden:
	- First fit: suche 1. Loch (schnellste)
	- Next fit: schlechter als first fit (minor variation of first fir)
	- Best fit: Suche kleinstes Loch (zeitaufwendiger, aber Minilöcherproblem)
	- Quick fit: Tabelle mit Pointern auf Löcher bestimmter Größe

## Virtual memory & Paging
- Swapping: Welche Lücke im Hauptspeicher erhält ein Prozess?
- Paging: Welcher Prozess wird in den ausgelagerten Hauptspeicher auf der Festplatte geschoben?

## Page-Ersetzungsalgorithmen
