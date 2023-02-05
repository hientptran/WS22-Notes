## Implementation
Nun kommen wir zur Implementation unserer Datenbank. Nach dem Relationsschema haben wir 7 Tabellen erstellt, nämmlich Spende, Lieferung, liefert, Fahrer, faehrt_in, Gebiet, und Spender.

Wir haben also pgAdmin4 benutzt, um diesen Tabellen zu erstellen und Datensätzen hinzuzufügen. 

Zusätzlich zu den Primary- und Foreign Keys Constraints in den Tabellen haben wir noch ein Constraint bei Menge angelegt, um die Menge von Spenden auf 10 zu beschränken. Hier könnt ihr sehen wie wir diesen erstellt haben, und zwar mit CHECK (Menge <= 10 AND Menge > 0). Wir haben diesen Constraint direkt bei der Erstellung der Tabelle Spende angelegt.

Darüber hinaus haben wir noch 2 Enumerations bei den Tabellen Lieferung und Spende. Die erste Enum heißt kategorien hat den Werten: ('gemuese','fleisch','fisch', 'milchprodukte', 'teigwaren', 'getraenke', 'speisereste', 'sonstiges', 'tiefkuehlprodukte'). Sie verweist auf die Kategorie einer Spende.

Die zweite Aufzählung ist StatusType, die den aktuellen Status der Lieferung angibt. 
Eine Lieferung kann neu sein, d. h. sie wurde gerade vom Spender erstellt und ist noch keinem Fahrer zugewiesen. Die Lieferung kann auch einem Fahrer zugewiesen, von einem Fahrer abgeholt oder bereits an eine Annahmestelle geliefert worden sein. Deswegen hat diese Enumeration 4 Werten: neu, zugeordnet, abgeholt, und ausgeliefert.

## Rechtevergabe
Jetzt stelle ich die Rechtevergabe in unserem Datenbank vor, bzw. welche Benutzer, Rollen und Sichten wir in unserer Datenbank angelegt haben. Da es unser Ziel ist, den Zustellungsprozess zu optimieren und die Kommunikation zu verbessern, sind die wichtigsten Rollen unserer Datenbank der Fahrer und der Spender.

Zum Veranschaulichen haben wir als Beispiel der Fahrer Max Müller ausgewählt. Er hat die FahrerID 100 und fährt jeztz im Bezirk Mitte.

- Wenn er z. B. für eine Lieferung mit 600 Donuts zugewiesen wird, was mehr ist, als er tragen kann, kann er die View aktiveKollegen verwenden, um zu sehen, welche Fahrer derzeit in der Gegend aktiv sind, und sie anrufen, und um Hilfe zu bitten.
- Max kann auch mit Hilfe der Sicht meineLieferungen die ihm zugeordneten Lieferungen sehen. Er kann auch Informationen über seine Lieferungen sehen, z.B Spender-/Lieferaddresse, Spendemenge, Kategorie, usw.
- Außerdem kann er mit der Sicht neueLieferungen neue Lieferungen in seinem Radius und deren Informationen  sehen, die noch keinem Fahrer zugeordnet sind. So kann er entscheiden, ob er diese Lieferung annehmen möchte.

So erhalten auch andere Fahrer diese 3 Views, die auf ihn persönlich angepasst sind. Er kann keine Lieferungen von anderen Fahrern, keine Lieferungen aus Gebieten, für die er nicht zuständig ist sehen, und er kann keine Fahrer außerhalb seines Gebiets kontaktieren.

Für den Spender haben wir nur eine Sicht angelegt, und als Beispiel nehmen wir den Spender mit dem Benutzernamen JuliaJogurt. 
- Wenn Julia z.B SELECT * FROM JuliaJogurt ausführt, sieht sie 2 Spalten. Diese sind die Lieferungen mit Spenden von ihr, die den Status zugeordnet, abgeholt, oder ausgeliefert haben. Julia kann damit Kontakt zu den Fahrern aufnehmen, die für ihre Spenden zuständig sind. Sie kann auch Informationen über ihre Spenden/Lieferungen mit dem View erhalten, z.B Lieferadresse oder Status.

Auf der rechten Seite könnt ihr den Quellcode von dieser View sehen. Ihr könnt auf jedem Fall bei pgAdmin unter Schemas > Public > Views die Quellcodes von allen Views sehen.

## Fazit
Und damit möchte ich unsere Präsentation mit dem Fazit beenden.

Ehrlich gesagt, sind wir von Anfang an auf viele Probleme gestoßen. Zunächst haben wir viel Zeit mit der Erstellung des ER-Diagramms verbracht. Zu Beginn hatten wir eine sehr komplizierte Formulierung von Entitäten, die ihr bereits im ersten Entwurf des ERDiagramms gesehen habt. Aus diesem Grund mussten wir den Entwurf unserer Datenbank sehr oft ändern, und infolgedessen brauchten wir auch viel Zeit, um die Daten zu generieren, Tabellen zu erstellen usw.

Bei der Implementierung hatten wir auch viele Schwierigkeiten. Die Normalisierung der Datenbank war für uns ein bisschen verwirrend. Die Erstellung und das Einfügen von Datensätzen war auch an einigen Stellen problematik, nämmlich bei der Zuweisung von Adressen und Postleitzahlen und bei der Beziehung zwischen Fahrer und Lieferung. 

Wir haben trotzdem Lösungen zu diesen Problemen gefunden, wie Witali und Bernadette euch vorher erklärt haben. Trotz vielen Schwierigkeiten würde ich sagen, dass wir jetzt

Trotz der Schwierigkeiten kann ich sagen, dass wir viele Erfahrungen mit der Erstellung und Arbeit mit einer Datenbank gesammelt haben, was wir sehr interessant finden. Wir haben auch viele andere Dinge gelernt und angewandt, von Excel bis Figma bis ChatGPT.

Auf der anderen Seite gibt es noch Luft nach oben. Wir können unsere Datenbank in vielen Aspekten verbessern. Einige Erweiterungsmöglichkeiten sind: Erweiterung um Entität Ausgabestelle mit Öffnungszeiten und Kapazität, die Berechnung von Entfernung/Fahrzeit. Erweiterung um Entität Kunde, und das Einbinden in App.

Damit beende ich unsere Präsentation, danke fürs Zuhören. Bei Fragen stehen wir für euch jetzt zur Verfügung, bevor wir zu den Beispielabfragen übergehen.
