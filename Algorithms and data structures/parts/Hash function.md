[[Algorithms and data structures]]

- **Definition**: Hashfunktion h ist eine Abbildung, die jedem (möglichen) Schlüssel einen Index der Hashtabelle H (Hashadresse) zuordnet
- **Problem**: mehrere Schlüssel den gleichen Index bekommen -> Kollision 
	- [Hash collision - Wikipedia](https://en.wikipedia.org/wiki/Hash_collision#:~:text=In%20computer%20science%2C%20a%20hash,a%20fixed%20length%20of%20bits.) 
	- [What is collision in a hash algorithm? - Quora](https://www.quora.com/What-is-collision-in-a-hash-algorithm)

- **Division rest method: h(k) = k mod m**
	- **Wahl von m:**
		- **ungeeignet**: gerade Zahl, Potenz der Basis eines Zahlensystems, m in der Nähe einer Potenz der Basis eines Zahlsystems
			- [algorithm - why are composite numbers bad for hashing by division? - Stack Overflow](https://stackoverflow.com/questions/9456790/why-are-composite-numbers-bad-for-hashing-by-division)
			- [language agnostic - Why should hash functions use a prime number modulus? - Stack Overflow](https://stackoverflow.com/questions/1145217/why-should-hash-functions-use-a-prime-number-modulus)
		- **geeignet**: m ist Primzahl and m in der Nähe einer Potenz der Basis eines Zahlsystems; m sollte gleich die Anzahl von Schlüsseln sein
	- **Beispiel**: 15, 2, 53, 12, 43, 5, 19 (m=7) 
		    -->  1, 2,   4,   5,  1,  5,   5
		- Kollision bei 1 und 5

- **Kollisionbehandlung**: 
	- **Beispiel**: 15, 2, 53, 12, 43, 5, 19 (m=7) 
		    -->  1, 2,   4,   5,  1,  5,   5
		    --> Kollision bei 1 und 5
	- **Verkettung der Synonyme:** Create singly linked/doubly list at collision site and next pointer points at the synonym(s)
		- [7.4.  Handling Collisions with Linked Lists (uc3m.es)](https://www.it.uc3m.es/pbasanta/asng/course_notes/dynamic_data_structures_hash_table_collision_en.html)
		- [Separate Chaining Collision Handling Technique in Hashing - GeeksforGeeks](https://www.geeksforgeeks.org/separate-chaining-collision-handling-technique-in-hashing/)
		- **Direkte Verkettung:** 
		- ![[Pasted image 20221215151142.png | 400]]
		- **Separate Verkettung:** 
		- ![[Pasted image 20221215151421.png | 400]]

- **Data retrieval:** search, add, remove
	- Why not use linear list (array)? High time complexity: search = O(n), add = O(n), remove = O(n)
	- Singly linked list: Best case Ω(1); Worst case: search = O(n), add = remove = O(1) ([Time Complexity Analysis of Linked List (opengenus.org)](https://iq.opengenus.org/time-complexity-of-linked-list/))
