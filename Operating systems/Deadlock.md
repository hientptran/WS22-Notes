### Resources
[8 Deadlock.pptx - Yandex.Documents](https://docs.yandex.ru/docs/view?url=ya-disk-public%3A%2F%2FkRjigWHi8JPSapAg3QaJbA%2F4mCnOKvQf6vx%2BgrHBxFw%3D&name=8%20Deadlock.pptx)

### Definition
- Deadlocks sind möglich, wenn Prozesse exklusivedn Zugriff auf Geräte besitzen
- Eine Menge von Prozessen ist deadlocked, falls jeder Prozess in der Menge auf ein Ereignis wartet, das nut von einem anderen Prozess in der Menge erzeugt werden kann. Keiner der Prozese kann weiterlaufen, Ressoucen freigeben oder befreit werden.
- Notwendige Bedingungen:
	1. Wechselsseitige Ausschlussbedingung: jede Ressource ist entweder einem Prozess zugeordnet oder verfügbar
	2. Zugriffs- und Wartebedingung: Jeder Prozess mit Zugriff auf Ressourcen kann weitere Ressourcen anfordern
	3. Nicht-Enteignungsbedingung: Bereits erteiler Zugriff auf Ressourcen kann einem Prozess nicht zwangsweise entzogen werden. Ressourcefreigabe nur durch Prozess
	4. Kreisketten-Wartebedingungen: Es existiert geschlossene Kette von 2 oder mehr Prozessen. Jeder Prozess wartet auf eine Ressource, worauf der nächste Prozess in der Kette Zugriff hat

### Ressourcen
- Definition: Gerät, auf das exklusiv zugegriffen werden kann
- Premptivierbare Ressourcen: könne von einem Prozess ohne Schaden entfernt werden (Memory)
- Nicht-preemptivierbare Ressourcen: führen zu Prozessabstürzen, falls sie entfernt werden (CD recorder) -> Deadlocks involve nonpreemptable resources
- Ereignisabfolge, um eine Ressource nutzen zu können: Ressource anfordern - nutzen - freigeben

### Auflösen von Deadlocks
- durch Wegnehmen: nimm eine R von einem anderen P -> Erfolg abhängig von der Natur der P
- durch Rollback: P erzeugt periodisch Checkpoints, speichern der Infos bis Checkpoint, Neustart des Ps ab letztem Checkpoint, falls Deadlock entdeckt -> kontrollierte Datenverlust
- durch Prozessabbruch: gewaltsam, aber einfach -> einen der deadlocked P abbrechen, die anderen P erhalten ihre R, wähle P, der ohne Verlust neu starten kann

### Sichere und unsichere Zustände
- Safe: durch sorgfältiges Scheduling kann Deadlock vermieden werden -> the system can guarantee that all processes will finish
- Unsafe: schlechte Scheduling -> no guarantee that all processes will finish

### Banker-Algorithmus
- Für eine Ressource: Check to see if granting the request leads to a safe state
- Für mehrere Ressourcen

### Deadlocks vermeiden
**1. Mutual exclusion**
	- **Negation**: mehrere Prozesse sind einer Ressource zugeordnet
	- **Idee**: Eine Geräte z.B Printer können gespoolt werden
	- **Problem**: nicht alle Geräte können gespoolt werden
**2. Hold and wait condition**
	- **Negation**: Prozesse fordern keine weiteren Ressourcen an
	- **Idee**: 
		- Ressourcen schon bei Prozessstart anfordern -> Ein Prozess muss niemals auf R warten. 
		- If everything is available, the process will be allocated whatever it needs and can run to completion. If one or more resources are busy, nothing will be allocated and the process would just wait.
	- **Problem**: 
		- Bei Prozessstart oft nicht bekannt, welche Ressourcen wann benötigt werden
		- Bindung von Ressourcen, die auch für andere Prozesse notwendig sein können
	- **Variation**: Prozess gibt alle bishherigen R kurzfristig frei und fordert dann alle R, die benötigt werden, auf einmal an
**3. No preemption condition**
	- **Negation**: Einem Prozess wird der Zugriff auf eine Ressource entzogen
	- **Idee**: Lösche Zugriff per Hand (per Admin)
	- **Problem**: Im allgemein keine sinvolle Option
**4. Circular wait condition**
	- **Negation**: Lasse geschlossene Kreisketten nicht zu
	- **Idee**: Einführung einer globalen Ressourcennummerierung
		- P darf nur R mit höherer Nummer, als die er bereits hat, anfordern
		- Processes can request resources whenever they want to, but all requests must be made in numerical order. A request may request first a scanner and then a plotter, but may not request first a plotter and then a scanner
	- **Problem**: Eine Nummerierung zu finden, die allen gefällt, ist schwer

### Nicht-Ressourcen-Deadlocks
- Prozesse können sich egenseitig blockieren: jeder Prozess wartet auf ein Ergebnis des anderen
- Kann bei Semaphoren vorkommen: Angenommen, jeder Prozess wendet down() auf zwei Semaphoren an (mutex und another). Falls Reihenfolge falsch, ist Deadlock nötig

