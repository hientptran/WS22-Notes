![](schema%20+%20sichten%20foodsharing.png)
## Datentypen
In dieser Datenbank werden folgende Datentypen verwendet:
-   VARCHAR(7) für PLZ in Tabelle Gebiet und Spender
-   VARCHAR(255) für Bezirk, Vorname, Nachname, LieferStrasse, SpenderName, SpenderStrasse, SpenderHausnummer in den Tabellen Gebiet, Fahrer, Lieferung und Spender
-   BIGINT für Handynummer in Tabelle Fahrer
-   BOOLEAN für Aktiv in Tabelle Fahrer
-   ENUM für Status in Tabelle Lieferung und Kategorie in Tabelle Spende
-   INT für FahrerID, LieferID, SpendenID, Menge in den Tabellen Fahrer, Lieferung, Spende
Diese Datentypen werden im Folgenden ausführlich beschrieben.

### INT und BIGINT
- Der Integer-Datentyp (int4) wird von den ID-Nummern von der Tabellen Spende, Lieferung, und Fahrer verwendet. Zur leichteren Unterscheidung zwischen SpendenID, LieferID und FahrerID haben wir uns entschlossen, eine dreistellige ID für die Fahrer, eine vierstellige für die Spenden und eine fünfstellige für die Lieferungen in unserer Datenbank zu vergeben.
- Ausgehend von der Annahme, dass unsere Datenbank nur innerhalb Deutschlands verwendet wird und daher keine Telefonvorwahl benötigt, haben wir festgelegt, dass der Datentyp der Handynummer des Fahrers BigInteger (int8) ist.

### VARCHAR
- Weitere Informationen zu den Spendern, den Lieferungen, den Fahrern und den Gebieten wie die Hausnummer, die Straße und die Namen werden dem Datentyp VARCHAR (variable-length string) mit der Länge 255 zugeordnet. 
- Die Postleitzahlen haben auch den Datentyp VARCHAR mit der Länge 7, da normalerweise keine Berechnungen mit Postleitzahlen vorgenommen werden. Außerdem wird dieser Datentyp verwendet, um Komplikationen zu vermeiden, wenn die Postleitzahl führende Nullen hat. Der ist auch hilfreich, wenn die Postleitzahl alphanumerisch ist oder andere Sonderzeichen enthält, was bei ausländischen Postleitzahlen der Fall ist. 

### BOOLEAN
- Da der Fahrer nur zwei Zustände hat, nämlich aktiv und inaktiv, haben wir BOOLEAN als Datentyp für das Attribut aktiv in der Tabelle Fahrer angegeben.

### ENUMERATION
Außerdem haben wir ENUM benutzt, um eine begrenzte Liste von möglichen Werten für eine Spalte bereitzustellen. Dies hilft bei der Überwachung der Integrität der Daten, da wir sicherstellen können, dass nur die zulässigen Werte in die Datenbank eingefügt werden.

Im konkreten Fall haben wir ENUM für den Status einer Lieferung und für die Kategorie einer Spende verwendet. Diese ENUM-Typen werden mit möglichen Werten für jeden Status bzw. jede Kategorie erstellt und können dann in den entsprechenden Tabellen als Datentyp verwendet werden.

Die erste Enum heißt Kategorien hat den Werten: ('gemuese','fleisch','fisch', 'milchprodukte', 'teigwaren', 'getraenke', 'speisereste', 'sonstiges', 'tiefkuehlprodukte'). Sie verweist auf die Kategorie einer Spende.

Die zweite Aufzählung ist StatusType, die den aktuellen Status der Lieferung angibt. Eine Lieferung kann neu sein, d. h. sie wurde gerade vom Spender erstellt und ist noch keinem Fahrer zugewiesen. Die Lieferung kann auch einem Fahrer zugewiesen, von einem Fahrer abgeholt oder bereits an eine Annahmestelle geliefert worden sein. Deswegen hat diese Enumeration 4 Werten: 'neu', 'zugeordnet', 'abgeholt', und 'ausgeliefert'.

Diese Aufzählungen haben wir direkt bei der Erstellung der Tabellen erstellt:
```sql
CREATE TYPE statusType AS ENUM ('neu','zugeordnet','abgeholt', 'ausgeliefert');

CREATE TYPE Kategorien AS ENUM ('gemuese','fleisch','fisch', 'milchprodukte', 'teigwaren', 'getraenke', 'speisereste', 'sonstiges', 'tiefkuehlprodukte');
```
## Constraints
Wir haben folgenden Constraints in unserer Datenbank angelegt:
-   FOREIGN KEY Constraints in Tabellen faehrt_in und liefert
-   PRIMARY KEY Constraints in Tabellen Gebiet, Fahrer, Lieferung, Spende
-   CHECK Constraint für die Menge in Tabelle Spende.

### Primärschlüssel
In unseren Tabellen haben wir die folgenden Spalten als Primärschlüssel definiert:
- Tabelle Spende: SpendenID
- Tabelle Gebiet: PLZ
- Tabelle Fahrer: FahrerID
- Tabelle Spender: (SpenderName, SpenderStrasse, SpenderHausnummer)

### Fremdschlüssel
- In Tabelle `Lieferung`, wird eine Verbindung zur Tabelle `Gebiet` hergestellt, indem die Spalte `PLZ` einen Fremdschlüssel auf den Primärschlüssel `PLZ` in der Tabelle `Gebiet` definiert.
- In Tabelle `Spende`, wird eine Verbindung zur Tabelle `Spender` hergestellt, indem die Spalten `SpenderName`, `SpenderStrasse` und `SpenderHausnummer` einen Fremdschlüssel auf den Primärschlüssel (`SpenderName`, `SpenderStrasse`, `SpenderHausnummer`) in der Tabelle `Spender` definieren.
- In Tabelle `Spende`, wird eine Verbindung zur Tabelle `Lieferung` hergestellt, indem die Spalte `LieferID` einen Fremdschlüssel auf den Primärschlüssel `LieferID` in der Tabelle `Lieferung` definiert.
- In Tabelle `faehrt_in`, wird eine Verbindung zwischen den Tabellen `Fahrer` und `Gebiet` hergestellt, indem die Spalten `FahrerID` und `PLZ` jeweils Fremdschlüssel auf die Primärschlüssel `FahrerID` und `PLZ` in den Tabellen `Fahrer` und `Gebiet` definieren.
- In Tabelle `liefert`, wird eine Verbindung zwischen den Tabellen `Fahrer` und `Lieferung` hergestellt, indem die Spalten `FahrerID` und `LieferID` jeweils Fremdschlüssel auf die Primärschlüssel `FahrerID` und `LieferID` in den Tabellen `Fahrer` und `Lieferung` definieren.

### Weitere Constraints
Der CHECK Constraint in der Tabelle Spende beschränkt den Wertebereich für das Attribut Menge. Der Constraint stellt sicher, dass der Wert für die Menge mindestens 1 und höchstens 10 beträgt. Der CHECK Constraint sorgt somit für Datenintegrität in dieser Tabelle.

Wir haben den Constraint bei der Erstellung der Tabelle Spende erstellt:
```postgresql
CREATE TABLE IF NOT EXISTS Spende (  
    SpendenID INT PRIMARY KEY,  
    Kategorie Kategorien NOT NULL,  
    Menge INT NOT NULL,  
    CHECK (Menge <= 10 AND Menge > 0),  
    LieferID INT REFERENCES Lieferung(LieferID),  
    SpenderName VARCHAR(255) NOT NULL,  
    SpenderStrasse VARCHAR(255) NOT NULL,  
    SpenderHausnummer VARCHAR(255) NOT NULL,  
    FOREIGN KEY (SpenderName, SpenderStrasse, SpenderHausnummer) REFERENCES Spender(SpenderName, SpenderStrasse, SpenderHausnummer)  
);
```
## Rechtevergabe
Da das Ziel unserer Datenbank darin besteht, den Lieferprozess von Spenden zu optimieren und die Kommunikation zwischen den beteiligten Parteien zu verbessern, stehen die beiden Rollen Fahrer und Spender im Mittelpunkt der Datenbank.

Zu diesem Zweck haben wir mehrere Sichten für die Fahrer und die Spender erstellt, damit sie die relevanten Informationen über ihre Lieferungen bzw. Spenden so effektiv wie möglich abrufen können. 

### Fahrer
Wir haben 3 Sichten für den Fahrer angelegt, und zwar aktiveKollegen, meineLieferungen, und neueLieferungen. Zum Veranschaulichen haben wir als Beispiel der Fahrer Max Müller ausgewählt, wer die FahrerID 100 hat und gerade im Bezirk Mitte fährt.

#### aktiveKollegen_MaxMueller
Mit Hilfe dieser View kann der Fahrer sehen, welche anderen Fahrer in der gleichen Region aktiv sind und auf deren Informationen (Vorname, Nachname, Handynummer) zugreifen. Der Quellcode von dieser View lautet:
```sql
CREATE VIEW aktiveKollegen_MaxMueller AS
 SELECT DISTINCT fahrer.vorname,
    fahrer.nachname,
    fahrer.handynummer
   FROM fahrer,
    faehrt_in,
    gebiet
  WHERE fahrer.fahrerid = faehrt_in.fahrerid
  AND NOT fahrer.fahrerid = 100
  AND faehrt_in.plz::text = gebiet.plz::text
  AND gebiet.bezirk::text = 'Mitte'::text
  AND fahrer.aktiv = true;
```
Im Fall von Max Müller kann er, wenn ihm eine Lieferung zugewiesen wird, die für ihn zu groß ist, diese View verwenden, um andere aktive Kollegen im Bezirk Mitte zu sehen und sie anzurufen und um Hilfe zu bitten.

![](Screenshot%20(98).png)

#### meineLieferungen_MaxMueller
Ein Fahrer kann die Sicht meineLieferungen verwenden, um Informationen zu den Lieferungen einzusehen, für die er verantwortlich ist.
```sql
CREATE VIEW meineLieferungen_MaxMueller AS
   SELECT DISTINCT lieferung.lieferstrasse,
	    lieferung.lieferhausnummer,
	    lieferung.lieferplz,
	    lieferung.status,
	    spende.kategorie,
	    spende.menge,
	    spender.spendername,
	    spende.spenderstrasse,
	    spende.spenderhausnummer,
	    spender.spenderplz
  FROM lieferung,
	    spende,
	    fahrer,
	    spender,
	    gebiet,
	    liefert
  WHERE NOT lieferung.status = 'neu'::statustype 
	  AND spender.spenderstrasse::text = spende.spenderstrasse::text 
	  AND spender.spenderhausnummer::text = spende.spenderhausnummer::text 
	  AND spender.spendername::text = spende.spendername::text 
	  AND spende.lieferid = lieferung.lieferid 
	  AND spender.spenderplz::text = gebiet.plz::text 
	  AND liefert.fahrerid = 100 
	  AND liefert.lieferid = lieferung.lieferid;
```
So kann Max zum Beispiel abrufen, welche Lieferungen er schon abgeholt hat, damit er sie ausliefern kann.
![](Screenshot%20(102).png)

#### neueLieferungen_MaxMueller
Außerdem kann er mit der Sicht neueLieferungen neue Lieferungen in seinem Radius und deren Informationen  sehen, die noch keinem Fahrer zugeordnet sind. So kann er entscheiden, ob er diese Lieferung annehmen will.
```sql
CREATE VIEW neueLieferungen_MaxMueller AS
 SELECT DISTINCT lieferung.lieferstrasse,
    lieferung.lieferhausnummer,
    lieferung.lieferplz,
    lieferung.status,
    spende.kategorie,
    spende.menge,
    spende.spenderstrasse,
    spende.spenderhausnummer,
    spender.spenderplz
   FROM lieferung,
    spende,
    fahrer,
    spender,
    gebiet
  WHERE lieferung.status = 'neu'::statustype
  AND spender.spenderstrasse::text = spende.spenderstrasse::text
  AND spender.spenderhausnummer::text = spende.spenderhausnummer::text
  AND spender.spendername::text = spende.spendername::text
  AND spende.lieferid = lieferung.lieferid
  AND spender.spenderplz::text = gebiet.plz::text
  AND gebiet.bezirk::text = 'Mitte'::text;
```
![](Screenshot%20(104).png)

So erhalten auch andere Fahrer diese 3 Views, die auf ihn persönlich angepasst sind. Er kann keine Lieferungen von anderen Fahrern, keine Lieferungen aus Gebieten, für die er nicht zuständig ist sehen, und er kann keine Fahrer außerhalb seines Gebiets kontaktieren.

## SQL
### Erstellung von Tabellen
### Datensätzen hinzufügen
### Beispielabfragen
- Allgemeine Abfragen (Admin-Abfragen):
	- Welche Gebieten sind gerade von keinem Fahrer gefahren?
	- Wie viele Spenden von JuliaJogurt werden noch nicht abgeholt?
	- Welche Spender befindet sich in Prenzlauer Berg?
	- Wo gibt es momentan Gemüse Spenden?
- Fahrer Arbeitsablauf
	- Max Müller will einsehen, für welche Lieferungen er gerade zuständig ist.
	- Er ist der Meinung, dass er nicht alle Spenden aus diesen Lieferungen transportieren kann und möchte einen anderen Fahrer um Hilfe bitten.
	- Er hat mit Hilfe seiner Kollegin alle Lieferungen ausgeliefert und möchte nun wissen, welche Spenden in seiner Nähe sind.
- Spender Abfragen
	- JuliaJogurt bietet 2 Spenden an, die bereits an Fahrer vergeben sind. Sie muss aber in einer Stunde losfahren und wird in etwa 6 Stunden zurückkommen. Sie möchte daher die Fahrer kontaktieren, um eine genaue Abholzeit zu vereinbaren.