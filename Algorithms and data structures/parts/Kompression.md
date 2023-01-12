[Algorithms and data structures](../Algorithms%20and%20data%20structures.md)
## Kanal und Symbolkodierung
- Kanal: Übertragung von Nachrichten von einer Quelle an eine Senke
- Kanal kann im einfachten Fall nur 2 Zustände übertragen, wie EIN/AUS, HIGH/LOW --> durch binäre Werte repräsentiert werden
- Die Symbole einer Quelle müssen als Folge von Bits kodiert werden, um einen Kanal passierenzu können --> **Symbolkodierung**
- Ziel: min. Anzahl von Bits, effektivste Kodierung/Dekodierung

## Binärkodierung: Fix-Length-Code (FLC)
- Jedem Symbol/Wert wird ein Bitwert/Code mit fester Länge zugewiesen

## Binärkodierung als Baum: Variable-Length-Code (VLC)
- Binärbaum erstellen -> Werte in Kategorien aufteilen (irgendwelche Kriterium) --> Ja/rechts: 1, nein/links: 0
- **Vorteil** (vs. FLC): 
	- mittlere Länge ist kleiner (Some symbols only need 2 bits (im Beispiel: 2 und 5 brauchen nur 2 bits))
	- Kompression ist verlustfrei
- **Nachteil:**
	- aufwendigere Kodierung/Dekodierung
	- Einzeilne Bitfehler können die Dekodierbarkeit aller folgenden Symbole verhindern (Fehleranfälligkeit)

## Huffman-Kodierung

> [!CALLOUT] Huffman-Kodierung
> 1. Zeichenhäufigkeiten bestimmen
> 2. Einen Knoten für jedes Zeichen mit der Häufigkeit > 0
> 3. Verschmelze die beiden Bäume mit der geringsten Häufigkeit zu einem neuen Binärbaum *(die Häufigkeit der Zeichen des neuen Baums ist die Summe der Häufigkeiten der beiden Teilbäume)*, solange bis nur noch ein Baum vorhanden ist

### Beispiel: Fünfundfünfzig
| Symbol | Häufigkeit | Code |
| ------ | ---------- | ---- |
| F      | 4          | 00   |
| Ü      | 2          | 101  |
| N      | 3          |   11   |
| D      | 1          |    0111  |
| U      | 1          |  0100    |
| Z      | 1          |  0101    |
| I      | 1          |    0110  |
| G      | 1          |   100   |
Visualisierung: [Huffman-Kodierung](Huffman-Kodierung.svg)

## Lauflängenkodierung (L-Code)
- Idee: gleiche Zeichen werden nicht einzeln kodert sondern nur das Zeichen und dessen Häufigkeit
- Ein wenig benutztes Zeichen (z.B Q) als Escape-Zeichen benutzen und das folgende Zeichen entsprechend der Stellung im Alphabet als Anzahl interpretieren
- Beispiel:
	- AAAABBCCCDDDDDDDE --> QDA BB QCC QGD E
	- AAAAABBBBBBCCC**Q**DDDDDDDE --> QEA QFB QCC **QAQ** QGD E
- Sonderfall: Bitmuster z.B S/W-Scanner