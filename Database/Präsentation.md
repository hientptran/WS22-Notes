### Tierheim
1. Einleitung zum Thema -> Motivation?
2. Idee -> Was macht die Datenbank?
3. ER-Modell: Entitäten, Attributen
	1. Adresse: composite primary key
4. Datenbankschema: Kardinalitäten
5. Relationsschema: Datentypen -> Interessante Entitäten
6. Rechtevergabe: Nutzer, Mitarbeiter, Leiter, Admin
7. Rechtevergabe: View, alternatative view
8. Demonstration mit Split screen
9. Fazit
```sql
-- alle Katzen in Berlin
select katze.name, katzenid, alter, stadt
from katze, adresse, tierheim
where katze.tierheimid = tierheim.tierheimid
and tierheim.tierheimid = adresse.tierheimid
and adresse.stadt='berlin';

-- alle Katzen mit Partnerkatzen (müssen zusammen adoptiert werden)
select katze.name, katze.beschreibung
from katze, partnerkatze
where katze.katzenid = partnerkatze.katzenid;

-- mit join
select katze.name, katze.beschreibung
from katze
join partnerkatze on katze.katzenid = partnerkatze.katzenid;
```

### Zahnarzt Terminverwaltung
1. Konzept: Einsatzgebiet -> Szenario
2. Anforderungen
3. Aufgaben, Nutzer, Verwendung
4. Wireframe --> Systemintegration
5. ERD
6. Datenbankschema -> Kreuztabelle (Zahnarzt_Patient)
7. SQL file -> create table queries
8. Constraints: Warum? Beispiel constraints: plz, CONSTRAINT termin_constraint UNIQLE(arztID, Datum, Uhrzeit)
9. Datengenerierung
10. Typische Abfragen
11. Fazit: Erweiterung?

```sql
-- Wie viele Termine hat Dr. Ziehzahn noch?
select count(*)
from zahnarzt, zahnarzt_patient
where zahnarzt.name = 'Dr. Ziehzahn'
and zahnarzt.arzt_id = zahnarzt_patient.arzt_id
and zahnarzt_patient.termin_wahrgenommen is NULL;

-- Welche Patienten brauchen noch ein "OP Vergespräch"?
select patient.* 
from patient, patient_behandlung, behandlungsart
where patient.patient_id = patient_behandlung.patient_id
and patient_behandlung.behandlungs_id = behandlungsart.behandlungs_id
and behandlungsart.behandlungsart='OP Vorgespräch';

-- Welche Behandlungen bekommt "Ronny Anlauf"?
```

### Theaterkassen
1. Anforderungen: Was muss die Datenbank alles können?
2. Aufbau: ERD, Relationsschema, Problem bei der Implementierung (Datengenerierung, Encodierung UTF-8), Norminalisierung, funktionale Abhängigkeiten
3. Typische Anfragen


> [! NOTE] 
> - All groups asked the audience to write the queries themselves (even though the audience cannot see the ERD/Relationsschema clearly) --> ineffective, time consuming 