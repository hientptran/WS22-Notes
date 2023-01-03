![[Pasted image 20221227115025.png | 500]]

### ER-Modell
- Name des Gebiets?
- bei liefert: Lieferadresse sinnvoll?
	- Nein: keine Abholort-Entität -> Erweiterungsmöglichkeit
- Eurobox:  es gibt verschiedene Eurobox-Größen
### **Gliederung:**
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

### **Anforderungen**
- Bei diesem Foodsharing-Projekt sind viele **Fahrer** in einem oder mehreren verschiedenen **Gebieten** tätig, die durch ihre Postleitzahlen eindeutig definiert sind. Für jeden Fahrer ist gespeichert, ob er gerade Dienst hat, oder nicht (true/false). Sie holen die Lebensmittel von den Spendern in ihrem jeweiligen Gebiet ab und bringen sie zu geeigneten Abholstellen. Dabei wird der Status der Lieferung (Neu, Zugeordnet (zu einem Fahrer), Abgeholt, Ausgeliefert) verfolgt. Es wird auch gespeichert, wann die Abholung abgefertigt wurde. 
- Jeder Lebensmittelspende wird eine **SpendenID** zugewiesen und das aktuelle Datum und die Uhrzeit werden gespeichert. Alle enthaltenen Produkte werden als Liste im Feld Beschreibung erfasst. Eine Lieferung kann aus vielen verschiedenen Spenden bestehen, die an vielen verschiedenen Orten abgeholt werden. Ein Fahrer kann verschiedene Lieferungen an viele verschiedene Adressen vornehmen. 
- Die Menge der gespendeten Lebensmittel wird durch Eurobox-Einheiten (von 1 bis 10) geschätzt. Durch die Verwendung einer standardisierten Einheit kann der Fahrer die Lebensmittel effektiv an Orte mit ausreichendem Lagerraum liefern.
- Da sich diese Datenbank nur auf die Abhol- und Liefervorgänge von gespendeten Lebensmitteln konzentriert, werden keine weiteren Informationen über Abholorte angegeben (=> Erweiterungsmöglichkeit?).

### Beschreibung des ER-Modells
- **Spender**
	- Spender können Privatpersonnen, Supermärkte, Restaurants, usw. sein
	- Der Spender wird durch die Kombination von Namen und Adresse eindeutig identifiziert
	- Jeder Spender wird durch 
- **Gebiet**
	- Ein Gebiet wird eindeutig durch seine Postleitzahl identifiziert
- **Spende**
	- Jeder Lebensmittelspende wird eine n-stellige SpendenID zugewiesen und das aktuelle Datum und die Uhrzeit werden gespeichert
	- Alle enthaltenen Produkte werden als Liste im Feld Beschreibung erfasst
- **Lieferung**
	- Jede Lieferung wird eindeutig durch eine n-stellige LieferID identifiziert
	- Der Status der Lieferung kann auch verfolgt werden (Neu, Zugeordnet (zu einem Fahrer), Abgeholt, Ausgeliefert) 
	- Es wird auch gespeichert, wann die Abholung abgefertigt wurde. 
- **Fahrer**
	- Jeder Fahrer erhält eine eindeutige n-stellige FahrerID
	- Der Name und Handynummer des Fahrers werden auch zur erleichterten Kommunikation zwischen den Fahrern erfasst 
	- Für jeden Fahrer ist gespeichert, ob er gerade Dienst hat (Attribut aktiv), oder nicht (true/false)

### Englisch
In the foodsharing project, many drivers operates in one or many different areas, which are uniquely defined through their zipcodes. They will pick up food from donators in their respective areas and delivers them to suitable pickup locations. Since this database focuses only on the pickup and delivery processes of donated produce, no further information about pickup locations would be specified.

The donated food will be uniquely defined through a DonationID. All included products and other comments will be recorded in the field Description. A delivery can consist of many different donations, which are picked up at many different locations. A driver can make different deliveries to many different addresses. 

The amount of food donated will be estimated through Eurobox units (from 1 to 10). By using a standardized unit, the driver can effectively deliver food to locations with sufficient storage.
