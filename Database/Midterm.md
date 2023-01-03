## Introduction
**Datenbank** (DB): Ansammlung miteinander in Beziehung stehender Daten 
- anpassbare Repräsentation einiger Aspekte der realen Welt 
- logisch zusammenhängend, keine zufällige Mischung von Daten 
- für einen bestimmten Zweck und Anwendungskontext und eine bestimmte Gruppe von Nutzer\*innen gedacht 
- persistente Speicherung 
- konkurrenter lesender und schreibender Zugriff auf dieselben Daten durch mehrere Nutzer\*innen möglich 
- nur relationale DBen: flexible Abfragesprache mit Aggregations- und Join-Operatoren 
- nur relationale DBen: Nutzung von Konsistenzregeln (constraints)

>[!NOTE]+ Relational (SQL) vs. non-relational (NoSQL) database
>[Relational vs. Non-Relational Database: Pros & Cons | The Aloa Blog](https://aloa.co/blog/relational-vs-non-relational-database-pros-cons#:~:text=So%2C%20what's%20the%20difference%3F,of%20a%20laundry%20list%20order.)
>A relational database is structured, meaning the data is organized in tables. Many times, the data within these tables have relationships with one another, or dependencies. A non relational database is document-oriented, meaning, all information gets stored in more of a laundry list order.
>
>For example, Facebook Messenger uses a NoSQL database, because the information that is being gathered isn’t structured enough to be segmented into tables and define relationships between each other. With tons of unstructured information, it needs to be held in a non-relational database. Think of the information as being stored on one large word document. Everything is there. As more information gets entered, the document gets longer. If you want to find and pull data, you have to in essence ‘control/command + F’ and search for the data itself.

**Daten**: bekannte Fakten, die aufgezeichnet werden können und eine implizite Bedeutung haben (Telefonnummern, Adressen, Namen, …)

**Datenbankmanagementsystem** (DBMS): rechnergestütztes System zum Entwurf und Wartung von Datenbanken, in der Regel General-Purpose-Software, die die Definition, Konstruktion, Manipulation und das Teilen von Daten erlaubt 

**Datenbanksystem** (DBS): Datenbank + DBMS + Anwendung mit Abfragen und Updates der Datensätze

![[../_assets/datenbankumgebung.png | 400]]

**Ein Modell** wird konstruiert, um das Verständnis zu verbessern und um Details zu abstrahieren
	- Verständnis: Die Bedeutung der Daten und ihre Beziehungen untereinander als Informationsstrukturen darstellen
	- Abstraktion: Vernachlässigung der Details (z.B. individueller Datenwerte)

--------------------------------------------------------
## ER MODEL
[Introduction of ER Model - GeeksforGeeks](https://www.geeksforgeeks.org/introduction-of-er-model/)

In einem **Entity-Relationship-Modell** werden Objekte der realen Welt, die für eine bestimmte Aufgabe relevant sind, mit ihren Beziehungen untereinander in abstrakter Weise beschrieben, d.h. modelliert

**Entität** (entity) (oft auch „Objekt“): 
- individuelles, identifizierbares Exemplar 
- beschrieben durch Eigenschaften (Attribute)
- Beispiel: Bestellung von Kundin Susanne Meier am 14.1.202

**Entitätstyp** (entity type) (oft auch „Objekttyp“ , „Entitätsmenge," vereinfacht ungenau auch „Entität“) 
- Zusammenfassung von Entitäten mit gleichartigen Eigenschaften
- Name (Substantiv) als Oberbegriff für alle Entitäten der Menge
- Beispiele: Bestellung, Artikel
- An Entity is an object of Entity Type and set of all entities is called as entity set. e.g.; E1 is an entity having Entity Type Student and set of all students is called Entity Set.
- [Difference between entity, entity set and entity type - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-entity-entity-set-and-entity-type/)

**Attribut** (attribute, property) • 
- Eigenschaft von allen Entitäten eines Entitätstyps 
- Name entsprechend fachlicher Bedeutung 
- vorgegebener Wertebereich (auch „Domäne“ domain) 
- beschreibende / identifizierende Attribute
- Types of attributes:
	- The attribute which **uniquely identifies each entity** in the entity set is called **key attribute**.For example, Roll_No will be unique for each student. In ER diagram, key attribute is represented by an oval with underlying lines.
	- An attribute **composed of many other attribute** is called as **composite attribute.** For example, Address attribute of student Entity type consists of Street, City, State, and Country. In ER diagram, composite attribute is represented by an oval comprising of ovals.
	- A **multivalued attribute** is an attribute consisting **more than one value** for a given entity. For example, Phone_No (can be more than one for a given student). In ER diagram, multivalued attribute is represented by double oval.
	- An attribute which can be **derived from other attributes** of the entity type is known as derived attribute. e.g.; Age (can be derived from DOB). In ER diagram, derived attribute is represented by dashed oval.
	- ![[../_assets/ER_attributes.png]]

**Schlüssel** (key): identifizierende Attributkombination
- identifizierendes Attribut einer Entität ist unterstrichen 
- Hinweis: Es ist möglich, dass mehrere Attribute zur Identifizierung benötigt werden.
- The attribute which **uniquely identifies each entity** in the entity set is called key attribute.

**Assoziation** (relationship) 
- Zusammenfassung von gleichartigen Beziehungen zwischen Entitäten 
- Name (Verbform/Prädikat) als Oberbegriff für die gleichartigen Relationen zwischen den Entitäten zweier Entitätstype
- Beispiel: ein Bauteil setzt sich aus anderen Bauteilen zusammen, die sich wiederum aus Bauteilen zusammen setzen, usw.
- Types of relationships:
	- A relationship between two entities of a similar entity type is called a **recursive**/**unary** relationship. Here the same entity type participates more than once in a relationship type with a different role for each instance. For example, one person is married to only one person.
	- When there are **TWO entities set participating in a relation**, the relationship is called as binary relationship. For example, Student is enrolled in Course.
	- **Dreistellige (oder höherwertigere) Beziehungen** involvieren drei (oder mehr) Entitätstypen und sind somit noch komplexer als binäre Beziehungen. Ternäre Beziehungen können nicht automatisch in binäre Beziehungen aufgebrochen werden (3. pdf-Datei, S. 48, 49)
>[!NOTES]+ Assoziation
>**1:1-Assoziation** - Zu jedem Auto gehört genau ein Kfz-Kennzeichen. Jedes Kfz-Kennzeichen gehört zu genau einem Auto
>**1:M-Assoziation (1 to \[at least 1])** - Jedes Lebewesen besteht aus mehreren Zellen. Jedes Lebewesen besteht aus mindestens einer Zelle. Jede Zelle gehört zu genau einem Lebewesen.
>**M:M-Assoziation (\[at least 1] to \[at least 1])**
>**1:C-Assoziation (1 to \[0 or 1]**
>**1:MC-Assoziation (1 to \[any number])**
>**M:MC-Assoziation (\[at least 1] to \[any number])**
>**MC:MC-Assoziation (\[any number] to \[any number])**
>**C:M-Assoziation (\[0 or 1] to \[at least 1])**
>**C:MC-Assoziation (\[0 or 1] to \[any number])
>C:C-Assoziation (\[0 or 1] to \[0 or 1])**

**Weak Entity Type and Identifying Relationship:**   
	As discussed before, an entity type has a key attribute which uniquely identifies each entity in the entity set. But there exists **some entity type for which key attribute can’t be defined**. These are called Weak Entity type. 
	For example, A company may store the information of dependents (Parents, Children, Spouse) of an Employee. But the dependents don’t have existence without the employee. So Dependent will be weak entity type and Employee will be Identifying Entity type for Dependent. 
	A weak entity type is represented by a double rectangle. The participation of weak entity type is always total. The relationship between weak entity type and its identifying strong entity type is called identifying relationship and it is represented by double diamond.
	**Schwache Abhängigkeit:** Eine Entität kann ohne die Existenz einer anderen Entität nicht existieren (wird mit doppelten Linien modelliert, je nach Autor\*in wird in der Darstellung manchmal nur um die Entität eine doppelte Linie gezogem)
	Beziehung zwischen "starken" und schwachem Typ ist immer 1:MC

**Participation Constraints**
- **Total Participation** − Each entity is involved in the relationship. Total participation is represented by double lines.
- **Partial participation** − Not all entities are involved in the relationship. Partial participation is represented by single lines.

-------------
## Relational model
**Tabellen-Schema** 
- Die Tabelle hat einen Namen. Dies entspricht dem Namen der Relation.  
- Jede Spalte in der Tabelle hat eine Spaltenüberschrift. Dies entspricht der Bezeichnung des Attributes

**Tabellen-Daten** 
- Einzelne Datensätze entsprechen den Zeilen der Tabelle. 
- In den Spalten stehen dabei Werte, die zum entsprechenden Attribut passen. Jeder Wert hat einen Datentyp, wobei in jeder Spalte nur Werte des gleichen Datentyps stehen

**Null-Werte**
- Attribute werden entsprechend ihrem Wertebereich mit Werten belegt. Sie können aber (eventuell) auch undefiniert bleiben. Dies wird mit dem symbolischen Wert NULL ausgedrückt. 
- Null ist nicht mit dem numerischen Wert 0 gleichzusetzen. 
- Nullwerte sind symbolische Werte und können mit keinen anderen Werten verglichen werden.

------------
## SQL

```sql
CREATE TABLE Kunde (
	Kundennummer integer,
	ssn numeric {(10, 0)},
	Name varchar[40],
	Vorname varchar[40],
	PRIMARY KEY (Kundennummer, ssn)
);

CREATE TABLE Produkt (
	ProduktID integer PRIMARY KEY,
	Name varchar[40],
	PRIMARY KEY (ProduktID)
);

/* default-Wert einer Spalte ist NULL. Ein fixer Default-Wert kann zugewiesen werden */
CREATE TABLE defaults (
	Id INTEGER NOT NULL PRIMARY KEY,
	Ort VARCHAR(80) DEFAULT „GARCHING“, 
	Alter SMALLINT DEFAULT 20, 
	Groesse SMALLINT NOT NULL);
);

SELECT CustomerName
FROM Customers
WHERE City='Stuttgart';
```

**Data types:** [PostgreSQL: Documentation: 15: Chapter 8. Data Types](https://www.postgresql.org/docs/current/datatype.html)
- **numerische Typen**: bigint, bit, double precision, integer, interval, numeric, decimal, real, smallint 
- **Zeichenketten:** varying, char, character varying, character, varchar 
- **Boole’scher Typ:** boolean 
- **Datumstypen**: date, time (with or without time zone), timestamp (with or without time zone) 
- **XML-Type:** xml.

**Constraints**: 
- **NOT NULL**: erzwingt definierte Attributwerte beim Einfügen von Tupeln. Zwingend für Schlüssel
- **DEFAULT**: ist NULL. ein fixer default-Wert kann zugewiesen werden
- **UNIQUE**: stellt Schlüsseleigenschaft für Attribut sicher, kann Null-Werte haben, kann Schlüsselkandidat sein
- **CHECK**-Klauseln: schränken Wertebereich eines Attributs ein
```sql
CREATE TABLE Professor (
	PNummer INTEGER NOT NULL PRIMARY KEY,
	Name VARCHAR(80) NOT NULL,
	Raum INTEGER
	CHECK (Raum > 0 AND Raum < 999999)
);	
```

**Foreign key:**
```sql
CREATE TABLE Professoren (
	PersNr INTEGER NOT NULL,
	Name VARCHAR(30) NOT NULL,
	Rang CHAR(2),
	Raum INTEGER,
	PRIMARY KEY (PersNr)
);

CREATE TABLE Vorlesungen (
	VorlNr INTEGER NOT NULL PRIMARY KEY,
	Titel VARCHAR(30),
	SWS INTEGER,
	gelesenVon INTEGER REFERENCES Professoren
)
```

**Integritätsbedingungen: Fremdschlüsselbedingung**
- Änderungen an Schlüsselattributen können automatisch propagiert werden 
- **set null:** alle Fremdschlüsselwerte, die auf einen Schlüssel zeigen, der geändert oder gelöscht wird, werden auf NULL gesetzt 
- **cascade**: alle Fremdschlüsselwerte, die auf einen Schlüssel zeigen, der geändert oder gelöscht wird, werden ebenfalls auf den neuen Wert geändert bzw. gelöscht

```sql
CREATE TABLE Vorlesungen (
	VorlNr INTEGER NOT NULL PRIMARY KEY,
	Titel VARCHAR(30),
	SWS INTEGER,
	gelesenVon INTEGER REFERENCES Professoren ON DELETE SET NULL
);

CREATE TABLE hoeren (
	MatrNr INTEGER REFERENCES Studenten ON DELETE CASCADE,
	VorlNR INTEGER REFERENCES Vorlesungen ON DELETE CASCADE,
	PRIMARY KEY (MatrNr, VorlNr)
);
```

---------------
## Norminalisierung

>[!NOTES] Notes
>**5. PDF-Datei S. 62, 63:** 
>- 1-M/ 1-C/ 1-MC Relationship: the relationship doesn't have its own table
>- M-N/ MC-C Relationships: separate table with foreign keys as primary keys of the entities

**Norminalisierung**
1. **Normalform**: 
	- Eine Tabelle liegt in der ersten Normalform vor, wenn jeder Attributwert eine atomare, nicht weiter zerlegbare Dateneinheit ist. Oder: 
	- Eine Tabelle ist nicht in der 1 NF, wenn Attribute mehrfach oder komplex in einer Spalte auftreten; d. h. 1 NF ist eine Strukturierungsvorschrift
2. **Normalform**:
	- Eine Tabelle liegt in der zweiten Normalform (2NF) vor, wenn sie in der 1NF ist und jedes Nichtschlüsselattribut voll funktional abhängig vom Primärschlüssel ist. Oder:
	- Eine Tabelle ist nicht in der 2 NF, wenn Attribute von einem Teil des Schlüssels eindeutig identifziert werden. Voraussetzung ist außerdem 1 NF.
	- Second Normal Form applies to relations with composite keys, that is, relations with a primary key composed of two or more attributes. A relation with a single-attribute primary key is automatically in at least 2NF
3. **Normalform**:
	- Eine Tabelle liegt in der dritten Normalform (3NF) vor, wenn sie sich in der 2NF befindet und jedes Nichtschlüsselattribut nicht transitiv abhängig vom Primärschlüssel ist. Oder:
	- Eine Tabelle ist nicht in der 3 NF, wenn Attribute von anderen Nicht-Schlüsselattributen identifziert werden. Voraussetzung ist außerdem 2 NF

**Informelle Design-Regeln:**
1. Bedeutung der Attribute klarstellen 
2. Reduzierung von redundanter Information in Tupeln 
3. Reduzierung von NULL-Werten in Tupeln 
4. Vermeiden der Erzeugung falscher Informationen (”spurious tupels“) 