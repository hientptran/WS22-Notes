![[../_assets/Pasted image 20221227115025.png | 500]]

## ER-Modell
- Name des Gebiets?
- bei liefert: Lieferadresse sinnvoll?
	- Nein: keine Abholort-Entität -> Erweiterungsmöglichkeit
- Eurobox:  es gibt verschiedene Eurobox-Größen
## **Gliederung:**
1. Konzeptueller Entwurf
	1. Motivation
	2. Anforderungen
	3. ER-Modell 
2. Datenbanksschema
3. Implementation
	1. Datenbank erstellen
	2. Beispieldaten
	3. Beispielabfragen
4. Fazit
	1. Probleme
	2. Verbesserungsmöglichkeiten

## **Anforderungen**
- Bei diesem Foodsharing-Projekt sind viele **Fahrer** in einem oder mehreren verschiedenen **Gebieten** tätig, die durch ihre Postleitzahlen eindeutig definiert sind. Für jeden Fahrer ist gespeichert, ob er gerade Dienst hat, oder nicht (true/false). Sie holen die Lebensmittel von den Spendern in ihrem jeweiligen Gebiet ab und bringen sie zu geeigneten Addressen. Dabei wird der Status der Lieferung (Neu, Zugeordnet (zu einem Fahrer), Abgeholt, Ausgeliefert) verfolgt. Es wird auch gespeichert, wann die Abholung abgefertigt wurde. 
- Ein **Spender** kann eine Einzelperson oder ein Unternehmen (z. B. ein Supermarkt, ein Restaurant usw.) sein und kann mehrere Produkte spenden.
- Jeder Lebensmittelspende wird eine **SpendenID** zugewiesen. Zusätzliche Informationen wie enthaltenen Produkte oder weiteren Anmerkungen werden als Liste im Feld Beschreibung erfasst. Eine Lieferung kann aus vielen verschiedenen Spenden bestehen, die an vielen verschiedenen Orten abgeholt werden. Ein Fahrer kann verschiedene Lieferungen an viele verschiedene Adressen vornehmen. 
- Die Menge der gespendeten Lebensmittel wird durch Eurobox-Einheiten geschätzt. Durch die Verwendung einer standardisierten Einheit kann der Fahrer die Lebensmittel effektiv abholen und an Orte mit ausreichendem Lagerraum liefern.

## Erstellung von Beispieldaten
- Im Datenbankprojekt verwendeten Beispieldaten wurden mit Hilfe von [Mockaroo - Random Data Generator and API Mocking Tool | JSON / CSV / SQL / Excel](https://www.mockaroo.com/) erstellt. Mockaroo ist eine einfache Webanwendung, die es den Testern ermöglichte, ihre eigenen Daten von Grund auf zu erstellen.
- Die Namen der Attribute können unter "Feldname" eingegeben werden. Dann kann man für jedes Feld einen geeigneten Datentyp (z.B "first name", "last name", usw.) wählen und den Tabellennamen eintragen.
- Nachdem wir die Beispieldaten erstellt haben, haben wir sie als .sql-Dateien heruntergeladen, wobei auch automatisch Insert-Statements generiert werden.
- Wenn nötig, haben wir die Beispieldaten mit Excel zusätzlich bearbeitet.

## Beschreibung des ER-Modells
- **Spender**
	- Spender können Privatpersonnen, Supermärkte, Restaurants, usw. sein
	- Der Spender wird durch die Kombination von Namen und Adresse eindeutig identifiziert
- **Gebiet**
	- Ein Gebiet wird eindeutig durch seine Postleitzahl identifiziert
	- Wir haben in unserem Projekt die Postleitzahlen in Berlin verwendet
- **Spende**
	- Jeder Lebensmittelspende wird eine 4-stellige SpendenID, die mit 1000 anfängt, zugewiesen
	- Zusätzliche Informationen wie enthaltenen Produkte oder weiteren Anmerkungen werden als Liste im Feld Beschreibung erfasst. Zur Vereinfachung wird dieses Feld In den Beispieldatensätzen entweder mit "kühl und trocken lagern", "im Kühlschrank aufbewahren", "im Gefrierschrank lagern," "zebrechliche Verpackung" oder "läuft bald ab" ausgefüllt, und 30 % davon werden als Nullwerte belassen. Natürlich kann der Spender alle weiteren Angaben machen, die er für sinnvoll hält.
- **Lieferung**
	- Jede Lieferung wird durch eine 5-stellige LieferID ab 10000 eindeutig identifiziert.
	- Der Status der Lieferung kann auch verfolgt werden ("neu", "zugeordnet" (zu einem Fahrer), "abgeholt", "ausgeliefert"). Dieses Attribut  ist vom Typ "Status", einem ENUM-Objekt, das bei der Erstellung der Tabelle angelegt wird.
	- Eine Lieferung kann aus mehreren Spenden bestehen, die von mehreren Spendern stammen.
- **Fahrer**
	- Jeder Fahrer erhält eine eindeutige 3-stellige FahrerID, Jeder Fahrer erhält eine eindeutige 3-stellige Fahrer-ID, die mit 100 beginnt.
	- Der Vor-, Nachname und Handynummer des Fahrers werden auch zur erleichterten Kommunikation zwischen den Fahrern erfasst 
	- Für jeden Fahrer ist gespeichert, ob er gerade Dienst hat (Attribut aktiv), oder nicht (true/false)
- **bietet_an**: Ist eine Beziehungstabelle, aus der hervorgeht, welcher Spender eine Spende getätigt hat und wie viele Eurobox-Einheiten ungefähr gespendet worden sind
- **enthaelt**: Aus dieser Tabelle lässt sich ablesen, wie viele Eurobox-Einheiten einer bestimmten Spende in einer bestimmten Lieferung enthalten sind
- **liefert**: Diese Tabelle zeigt, welcher Fahrer eine Lieferung zustellt oder zugestellt hat. Zusätzlich wird hier auch die Lieferzeit vermerkt.
- **nach**: Zeigt über das Zusatzattribut lieferadresse an, wohin eine Lieferung zugestellt werden soll
- **fahrer_in und spender_in:** Zeigt das Gebiet, in dem ein Fahrer aktiv ist und in dem sich ein Spender befindet

## Englisch
In the foodsharing project, many drivers operates in one or many different areas, which are uniquely defined through their zipcodes. They will pick up food from donators in their respective areas and delivers them to suitable pickup locations. Since this database focuses only on the pickup and delivery processes of donated produce, no further information about pickup locations would be specified.

The donated food will be uniquely defined through a DonationID. All included products and other comments will be recorded in the field Description. A delivery can consist of many different donations, which are picked up at many different locations. A driver can make different deliveries to many different addresses. 

The amount of food donated will be estimated through Eurobox units (from 1 to 10). By using a standardized unit, the driver can effectively deliver food to locations with sufficient storage.
