# Belegarbeit - Simple budget app

**Name:** Thi Phuöng Hien Tran
**Matrikelnummer:** 585489

## 1. **Anwendungsfall**
Es handelt sich um eine einfache Java-Anwendung, mit der Transaktionen mit den folgenden Eingabefeldern erfasst werden können: Datum, Name, Kategorie und Betrag. Zu den unterstützten Funktionen gehören Hinzufügen, Entfernen, Bearbeiten, Löschen, Sortieren, Suchen und Speichern, die im Folgenden erläutert werden:

- Alle erfassten Transaktionen ausdrucken
- Eine neue Transaktion hinzufügen
- Eine Transaktion löschen
- Alle Transaktionen löschen
- Eine Transaktion bearbeiten
- Alle Transaktionen nach Betrag, Datum, Namen oder Kategorie sortieren
- Alle Transaktionen nach Betrag, Datum, Namen oder Kategorie suchen
- Alle Transaktionen in der csv-Datei "budget.csv" speichern, damit sie beim nächsten Start wieder abgerufen werden können

Um die Funktionalitäten der App zu veranschaulichen, wird die csv-Datei "budget.csv", die alle aufgezeichneten Transaktionen enthält, mit einigen Mustertransaktionen vorgeladen. Default-Kategorien werden auch aus der Datei "categories.csv" importiert.


## **2. Installationsanleitung**
### Option 1: Mit einer IDE ausführen
1. Die Datei ```BudgetApp.zip``` entpacken
2. Den Ordner ``Belegarbeit`` mit einer IDE nach Wahl öffnen
3. Zu diesem Dateipfad navigieren ```\Belegarbeit\app\src\main\java\budgetapp\App.java``` und ```App.java``` ausführen

### Option 2: JAR-Datei mit dem Terminal ausführen
1. Die Datei ```BudgetApp.zip``` entpacken
2. Den Ordner ``Belegarbeit`` öffnen
3. Terminal in diesem Ordner öffnen
4. Das Kommando ``java -jar app-all.jar`` ausführen


## **3. Benutzeranleitung**
### 1. Programm starten
Beim Start der Anwendung werden Transaktionen aus der Datei ``budget.csv`` geladen. Änderungen an der Liste der Transaktionen könnten auch in dieser Datei gespeichert und beim nächsten Start abgerufen werden. Hier ist der vorgelandene Inhalt dieser Datei:
![[Screenshot (60).png | 500]]
Jede Zeile der csv-Datei ist wie folgt formatiert: Datum, Name, Kategorie, Betrag. Ausgaben werden als negative Werte und Einnahmen als positive Werte angegeben.
Standardkategorien werden aus der Datei ``categories.csv``  importiert:
![[Screenshot (61).png | 500]]
Die folgende Optionen werden gezeigt. Geben Sie die entsprechende Nummer ein, um den gewählten Befehl auszuführen.
![[Screenshot (57).png | 500]]
Eine ungültige Option führt zu einem Fehlermeldung und das Programm wird Sie auffordern, einen gültigen Wert einzugeben.
![[Screenshot (62).png | 500]]
### 2. Alle Transaktionen anzeigen
Die erste Option \[0] im Hauptmenü wählen. Alle erfassten Transaktionen und die Summe der Transaktionen werden ausgedruckt.
![[Screenshot (63).png | 500]]
### 3. Transaktionen hinzufügen, löschen, und bearbeiten
- Die  Option \[1] der Hauptmenü wählen, um eine neue Transaktion hinzufügen. Sie werden dann aufgefordert, Datum, Name, Kategorie und Betrag der Transaktion einzugeben. Das Standarddatum ist das aktuelle Systemdatum. Sie können auch eine neue Kategorie hinzufügen.
![[Screenshot (64).png | 500]]
- Die Option \[2] der Hauptmenü wählen, um eine Transaktion zu löschen. Sie können die zu löschende Transaktion durch Eingabe ihrer ID-Nummer auswählen.
![[Screenshot (65).png | 500]]
- Die Option \[3] der Hauptmenü wählen, um alle Transaktion zu löschen.

- Die Option \[4] der Hauptmenü wählen, um eine Transaktion zu bearbeiten. Sie können die zu bearbeitende Transaktion durch Eingabe ihrer ID-Nummer auswählen.  Sie werden dann aufgefordert, Datum, Name, Kategorie und Betrag der neuen Transaktion einzugeben.
![[Screenshot (66).png | 500]]
### 4. Transaktionen sortieren
Die Option \[5] der Hauptmenü wählen, um die Transaktionen zu sortieren. Die Transaktionen können nach Betrag \[0], Datum \[1], Namen \[2], Kategorie \[3] sortiert werden.
![[Screenshot (67).png | 500]]
![[Screenshot (68).png | 500]]
![[Screenshot (69).png | 500]]
![[Screenshot (70).png | 500]]
### 5. Transaktionen suchen
Die Option \[6] der Hauptmenü wählen, um die Transaktionen zu sortieren. Die Transaktionen können nach Namen \[0] (case insensitive), Kategorie \[1], Datum \[2] durchgesucht werden. 
![[Screenshot (71).png | 500]]
![[Screenshot (72).png | 500]]
![[Screenshot (73).png | 500]]
### 6. Transaktionen speichern und Programm beenden
Alle Transaktionen können in der csv-Datei "budget.csv" gespeichert werden, damit sie beim nächsten Start wieder abgerufen werden können. Wenn Sie die Änderungen an der Transaktionsliste speichern möchten, können Sie die Option \[7] des Hauptmenüs wählen. 
![[Screenshot (76).png | 500]]
Der Inhalt der Datei "budget.csv" wird dann ebenfalls geändert:
![[Screenshot (74).png | 500]]
Die Option \[8] wählen, um das Programm zu beenden.
![[Screenshot (77).png | 500]]

## 4. Datenstruktur und Datentypen
Für diese Anwendung wird die Datenstruktur einfach verketteten Liste gewählt. Sie erweitert die Interface List\<Item> und hat die folgenden Methoden: 
- ``void add(Item data)``: Element am Ende der Liste hinzufügen
- ``void add(Item data, int index)``: Hinzufügen eines Elements an einem bestimmten Index
- ``void clear()``: Alle Elemente löschen
- ``Item remove(int index)``: Entfernt Element an einem angegebenen Index und gibt das entfernte Element zurück
- ``void set(int index, Item data)``: Element mit einem bestimmten Index auf ein anderes Element ersetzen
- ``Item get(int index)``: Element an einem bestimmten Index abrufen
- ``int size()``: Größe der verketteten Liste ermitteln
- ``public void printAll()``: Alle Elemente der verketteten Liste drucken. Für diese spezifische Anwendung würde jedes Element mit dem folgenden Format gedruckt werden: ``"%3d| %s\n", i, node.data"``, wobei ``i`` der Index des Elements und ``node.data`` die Daten des Elements in Form einer String sind.
- ``boolean contains(Item data)``: Prüfen, ob die verkettete Liste das angegebene Element enthält
- ``Node clone(Node head)``: Eine Kopie der verketteten Liste erstellen

Jede Transaktion wird als eine Instanz der Klasse ``Transaction`` gespeichert. Diese Klasse hat die privaten Variablen date (Type: LocalDate), name (Type: String), category (Type: String) und amount (Type: double). Der Konstruktor ``Transaction(String date, String name, String category, double amount)`` nimmt einen Datums-String (der dann mit der Klassenmethode ``convertDate(String string)`` in LocalDate umgewandelt wird), einen Name-String, einen Kategorie-String und einen Betrag-Double auf.

Die Transaktionen werden in einer ``TransactionList`` gespeichert, die von der Klasse ``LinkedList`` erbt. Zusätzlich zu den Methoden der Elternklasse ist die Klasse ``TransactionList`` auch für die Aufnahme von Benutzereingaben verantwortlich. Sie hat auch zusätzliche Methoden, um Transaktionen zu bearbeiten, Transaktionen zu importieren/exportieren (von und zu einer csv-Datei), die Transaktionen zu sortieren (mit dem Quicksort-Algorithmus), die Transaktionen zu suchen (mit einfacher linearer Suche). Eine Instanz dieser Klasse wird auch in einem speziellen Format gedruckt, um dem Anwendungsfall zu entsprechen.



## Analyse der Laufzeitkomplexität der Sortier- und Suchalgorithmen



## Englisch
Each transaction is saved as an instance of the class ``Transaction``. This class has the private variables date (Type: LocalDate), name (Type: String), category (Type: String), and amount (Type: double). It constructor ``Transaction(String date, String name, String category, double amount)`` takes in a date string (which will then be converted to LocalDate using the class method ``convertDate(String string)``), a name string, a category string, and a double amount.

Transactions will be saved in a ``TransactionList``, which inherits from the class ``LinkedList``. In addition to the parent class' methods, the class ``TransactionList`` is also responsible for taking in user input as well as creating command line prompts for the users. It also has additional methods to edit transactions, import/export transactions (from and to a csv file), sort the transactions (using the quicksort algorithm), search the transactions (using simple linear search. An instance of this class is also printed in a special format in order to fit the use case.