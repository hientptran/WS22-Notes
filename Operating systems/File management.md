## File
- **Data types:** 
	- **ASCII files:** Problem: Zeileinde (Uniz \n; Windows \r\n)
	- **Binary files (Unix):** Header starts with a magic number -> Identifikation des Datentyps; Es folgen verschiedene Längenangaben, Flags, Text- und Datenteil des Prog.
	- **Archive file (Unix):** Ansammlung von kompillierten Programmen
-  **File access:** sequential/random access
- **Bsp. für Datei-Systemaufruf**
	![[../_assets/datei-systemaufruf bsp.png | 500]]
	- **Memory-swapped files:**
		- 