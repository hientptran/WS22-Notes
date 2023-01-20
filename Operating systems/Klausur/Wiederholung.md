# 0. Unix

## Shell: Elementare Kommandos
- ls: Auflisten des Verzeichnisinhalts
	- -l: in Langform
	- -a: einschließlich der .-Dateien (Dotfiles: versteckte Datei/Verzeichnis)
	- -L: symbolische Links auflösen (when  showing  file  information  for  a  symbolic  link,  show information for the file the link references rather than for the link itself)
	- -i: Anzeigen der i-Node-Nummer
- mkdir: make directory
	- -p: make parent directory
- cd dir; cd..
- mv \[-i] destination: Move dile/directory mit Nachfrage
- cp: copy file
- cp sourcedir targetdir: copy directory
- pwd: print working directory
- rm file; rm -r dir (lösche Ordner und alle Unterobjekte)
- rmdir sourcedir: Lösche leeren Ordner
- ln \[-s] file link_name: Link auf die Datei file; ln dir link_dir -s symbolischer Link
	- ![](Pasted%20image%2020230110193743.png)
- quota -v: Anzeige Plattenplatz (benutzt, frei, limit)
- df: freier Plattenplatz -k in kB, -m in mB
- du -h -d 1: disk usage der Unterverzeichnisse im aktuellen Arbeitsverzeichnis: mit "human-readable" Ausgabe und Anzeigendistanz=1
- chmod u+rwx, g+r, o+r a.txt (chmod xOy file)
	- x = u user, g group, o other, a all
	- y = r read, w write, x execute
	- O = + hinzufügen, - entfernen
- chmod mode file (mode = N1N2N3) chmod 744 a.txt
	- Ni = 4r + 2w + 1x (r, w ,x in {0, 1})
- chown \[option] owner:group file: Inhaber ändern (als root)
	- (sudo) chown -R meier /home/meier/dir (R=rekursiv)
	- (sudo) chown -R root:admin file
- Sticky-Bit: mode = NsN1N2N3 -> Verfeinerung der Ausführ-Rechte (Löscheinschränkung)
	- f there's NO execute right (x): CAPITAL S/T; if there's x -> exchange x with normal s/t
	- t/T = Sticky bit: Nur Datei-Eigentümer und root kann Dateien in Ordnern mit Sticky-Bit löschen (ohne: jeder mit Schreibrechten kann löschen/umbenennen)
	- s/S = User/Group-ID-on-execution-bit; in einem solchen Ordner gehören alle Dateien und Unterverzeichnisse dem Ordner-Eigentümer und nicht der UID/GID des Prozesses, der sie erzeugt haben
	- SUID = 4 (user); SGID = 2 (group); Sticky = 1 (other)
	- chmod 5633 a.ord --> d rwS -wx -wt a.ord
	- ![](Pasted%20image%2020230110195815.png)
- Access control list: erwiterte Rechtevergabe
- ssh
	- Client initiates the connection by contacting server  
	- Server sends server public key (fingerprint) --> client checks if its the intended host, agrees upon and establish encryption to protect future communication.  Client and server will generate a symmetric key, meaning that the same key used to encrypt a message can be used to decrypt it on the other side. 
	- The server prompts the client for the password of the account they are attempting to log in with. Server will authenticate the user and discover whether access to the server should be granted  
	- User can now log in to server host operating system. All further sessions will be encrypted by the symmetric key created

[The Basics of Using the Sed Stream Editor to Manipulate Text in Linux | DigitalOcean](https://www.digitalocean.com/community/tutorials/the-basics-of-using-the-sed-stream-editor-to-manipulate-text-in-linux)

