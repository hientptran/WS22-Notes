[[Algorithms and data structures]]
## **Definitionen:
- **primitive Datentypen:** bool, int, shorts, lang, string, byte, double 
- **abstrakte Datentypen:** Datentyp  + Operation | *legen fest, welche Operationen was tun (Semantik), aber nicht wie* -> Array, lineare Liste, Queue, Stack, Bäume, Mengen, usw.
- **Datenstruktur:** formalisierte Objekte zur Speicherung, Verwaltung von bzw. Zugriff auf Daten

> [!NOTE]- Data type vs Data structure
> 
> | Data type                                                                                                                                                             | Data structure                                                                                                                                                                                            |              
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | 
| The data type is the form of a variable to which a value can be assigned. It defines that the particular variable will assign the values of the given data type only. | Data structure is a collection of different kinds of data. That entire data can be represented using an object and can be used throughout the program.                                                    |     
| It can hold value but not data. Therefore, it is dataless.                                                                                                            | It can hold multiple types of data within a single object.                                                                                                                                                |     
| The implementation of a data type is known as abstract implementation.                                                                                                | Data structure implementation is known as concrete implementation.                                                                                                                                        |    
| There is no time complexity in the case of data types.                                                                                                                | In data structure objects, time complexity plays an important role.                                                                                                                                       |    
| In the case of data types, the value of data is not stored because it only represents the type of data that can be stored.                                            | While in the case of data structures, the data and its value acquire the space in the computer’s main memory. Also, a data structure can hold different kinds and types of data within one single object. |     
| Data type examples are int, float, double, etc. |   Data structure examples are stack, queue, tree, etc.                                                                                                                                                                    |                                                                                                                                                                                                           |     |     |     |

![[../../_assets/classification of data structure.png]]

## **List:**
- ***Lineare Liste:***
	- einfügen(element, position) (Operation: 1)
	- löschen(position) (Operation: (n-1))
	- suchen(element) -> position (Operation: n)
	- zugreifen(position) -> element (Operation: 1)****
- ***Linked (einface verkettete) List:***
	- einfügen(element, position) (Operation: 1)
	- löschen(position) **(Operation: (n))**
	- suchen(element) -> position (Operation: n)
	- zugreifen(position) -> element (Operation: 1)

## Stack:
**Prinzip**: LIFO
- push(\_head): Element hinzufügen
- pop(\_head): das letzte Element löschen
- peak: das letzte Element zugreifen (nicht löschen)
- empty: alles löschen
## Queue:
**Prinzip**: FIFO
- push_tail: Element am Ende hinzufügen
- pop_head: erstes Element löschen
- peek
- empty
-> **Double-ended queue:** push(\_head || tail); pop(\_head || tail)
## Deque
-> Double ended queue / Combination of Stack and Queue
- getFirst
- getLast
- addFirst
- addLast
