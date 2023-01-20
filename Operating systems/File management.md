[Operating systems](Operating%20systems.md)
## File
### Introduction
- **Data types:** 
	- **ASCII files:** Problem: Zeileinde (Uniz \n; Windows \r\n)
	- **Binary files (Unix):** Header starts with a magic number -> Identifikation des Datentyps; Es folgen verschiedene Längenangaben, Flags, Text- und Datenteil des Prog.
	- **Archive file (Unix):** Ansammlung von kompillierten Programmen
-  **File access:** sequential/random access
- **Bsp. für Datei-Systemaufruf**
Kommandozeile: copyfile a.dat b.dat
```C
#include <sys/types.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[]);

#define BUF_SIZE 4096 //use abuffer of 4096 bytes
#define OUTPUT_MODE 0700 //protection bits for output file

int main(int argc, char *argv[]) {
	int in_fd, out_fd, rd_count, wt_count;
	char buffer[BUF_SIZE];
	if (argc != 3) exit(1); //syntax error of argc is not 3 (number of arguments given > or < 2)

	in_fd = open(argv[1], O_RDONLY); //open source file
	if (in_fd < 0) exit(2); //error: cannot open file
	out_fd = creat(argv[2], OUTPUT_MODE); //create destination file
	if (out_fd < 0) exit(3); //error: cannot create file

	while (TRUE) {
		rd_count = read(in_fd, buffer, BUF_SIZE); //read block of data with the size 4096 from file in_fd and write to buffer. returns number of bytes read
		if (rd_count <= 0) break; //end of file (read 0 byte) or error (-1)
		wt_count = write(out_fd, buffer, rd_count); //write to out_fd from buffer in blocks of rd_count bytes
		if (wt_count <= 0) exit(4); //wt_count <= 0 is an error
	}

	close(in_fd);
	close(out_fd);
	if (rd_count == 0) exit(0); //no error on last read (0: end of file)
	else exit(5) //error on last read
}
```
### **Memory swap files**
- Zugriff auf Dateien über Handles ist unbequem --> MULTICS: abbilden (map) der Datei im Prozessspeicher
- **Definition:** Abbildung einer Datei (z.B auf Festsplatte) in den Adressraum eines Prozesses (im Hauptspeicher)
	- Der zu kopierende Abschnitt einer Datei kann über die Parameter offset und length ausgewählt werden
	- Große Dateien (>= Hauptspeicher) abschnittsweise laden
- **Vorteil:** 
	- **Performance**: schneller Ersatz für die Systemaufrufe read, write, seek
	- Integrierter Mechanismus für dynamisches Binden (= Laden von Programmteilen während der Laufzeit)
	- Integrierter File-Sharing-Mechanismus
- **Vorsicht:**
	- Programmierer ist für die Synchronization zwischen schreibendem Mapping-Prozess (-> Hauptspeicher) und lesneden weiteren Prozessen (-> Festplatte) verantwortlich
	- Falls die Datei sehr groß ist, passt die u.U nicht in den virtuellen Adressraum -> Ausweg: nut einen Teil kopieren
	- Aufruf unter Windows anders: CreateFileMapping / MapViewOfFile

```C
##include <sys/mman.h>
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
int munmap(void *addr, size_t length);
```
- **C-Programming: mmap/minmap** - Abbildung einer Datei in den Hauptspeicher/Speicher-Freigabe
![](memory%20swapped%20dateien.png)
	- *mmap()* creates a new mapping in the virtual address space of the calling process. The starting address for the new mapping is specified in *addr*. 
	- The contents of a file mapping are initialized using *length* bytes starting off at offset *offset* in the file referred to by the file descriptor *fd*.
	- The *prot* argument describes the desired memory **protection** of the mapping (and **must not conflict with the *open* mode of the file**) -> PROT_NONE, PROT_EXEC, usw.
		- PROT_WRITE: Speicherabbild darf verändert werden --> pages may be written
		- PROT_READ: Speicherabbild darf gelesen werden --> pages may be read
	- The *flags* argument determines whether updates to the mapping are visible to other processes mapping the same region, and wheter updates are carried through to the underlying file
		- MAP_SHARED: share this mapping. Updates to the mapping are visible to other processes mapping the same region
		- MAP_PRIVATE: updates to the mapping are not cisible to other processes mapping the same region
	- The *munmap()* system call deletes the mappings for the specified address range. The address *addr* must be a multiple of the page size.
	- *mmap()* returns a pointer to the mapped area and *munmap()* returns 0 or -1
>[!NOTE]+ mmap()
>The _mmap_() function shall establish a mapping between the address space of the process at an address _addr_ for _length_ bytes to the memory object represented by the file descriptor _fd_ at offset _offset_ for _length_ bytes.
>[How to use mmap function in C language? (linuxhint.com)](https://linuxhint.com/using_mmap_function_linux/)

```C
#include <unistd.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <sys/stat.h>

int main() {
	int fd, N_bytes=16;
	//void *map_addr; --> void false
	char *map_addr;

	//fd = open("test.txt", O_RDONLY); --> RDONLY clashes with MAP_SHARED
	fd = open("test.txt", O_RDWR); 
	
	// mmap creates a new mapping in the virtual address space specified by addr (0). the contents of this file mapping is initialized using N_bytes (16) starting off at offset (0) in the file specified by the file handle fd (test.txt)
	//map_addr = map(0, N_bytes, PROT_WRITE, MAP_SHARED, fd, 0);
	map_addr = map(0, N_bytes, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
	// addr: 0; number of bytes 16, pages may be read or written, updates to the mapping are visible to other processes, file handle fd, offset 0)
	
	close(fd);
	*(map_addr+1) = '_'; //datentyp von map_addr muss char sein (nicht void)

	write(1, map_addr, N_bytes); //Ausgabe "H_llo Welt!" (aus test.txt) auf Standardausgabe (1); test.txt ist auch verändert

	munmap(map_addr, N_bytes);
	return 0;
}
```

### File lock
- Idee: OS blockiert einen Prozess, der auf eine Datei zugreifen möchte, worauf ein anderer Prozess bereits Zugriff hat
	- Exclusive lock (write lock): only 1 process has access to file
	- Shared lock (read lock): multiple processes have access to file

## Directory
- **One-level directory**
![200](one%20level%20dir.png)
	- Root has 4 files with 3 owners A, B, C
	- Problem: override problem -> When different users accidentally choose the same file name
	--> Only used in small embedded systems
- **Two-level directory**
![200](two%20level%20dir.png)
	- A, B, C are owners of files and directories
	- Needs login
	- Not suitable for users with too many files
-  **Hierachical directory system**
	--> modern OSs
![300](hierachical%20dir.png)
- **Filepath**
	- Absolute path
	- relative path -> path from working directory
	- special directories: . (current dir) and .. (parent dir)
- 