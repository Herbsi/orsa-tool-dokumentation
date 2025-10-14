---
title: "ORSA-Tool Dokumentation"
date: 2025-10-13
draft: false
---

# ORSA-Tool Dokumentation

Dieses Dokument dient als Anleitung für das ORSA-Tool der Valucor AG (nachfolgend «Valucor»).
Die aktuelle Version des Tools ist `VERSION`; bei anderen Versionen kann es zu Abweichnungen oder Unstimmigkeiten zwischen Tool und Dokumentation kommen.
Weiters beschreibt es die Funktionsweise, Annahmen und Vereinfachungen des Tools.
Die Dokumentation ist von Aktuaren für Aktuare geschrieben und setzt aktuarielle Grundlagen, insbesondere zur Solvenzberechnung gemäss Schweizer Solvenztest (SST) voraus.

Das ORSA-Tool reduziert den ORSA-Prozess kleinerer Nichtleben- und Krankenversicherer im Wesentlichen auf folgende Schritte:
1. Finanzplanung und Szenarienfestlegung, durch den Versicherer.
2. Erzeugung einer Sammlung von SST Templates, automatisch durch das Tool.
3. Solvenzberechung pro Planungsjahr und Szenario mittels FINMA `sstCalculation` R-Tool, automatisch durch das Tool.
4. Sammlung der Ergebnisse.
5. Befüllen des ORSA-Formulars, zu weit wie möglich durch das Tool.

Das ORSA-Tool besteht aus zwei Komponenten: dem Graphical User Interface (GUI) zur Solvenzberechnung und der ORSA-Input-Datei, in der die Finanzplanung gemacht wird und die Szenarien festgelegt werden.

## Finanzplanung und Szenarienfestlegung

## Graphical User Interface (GUI)

![ORSA-Tool GUI](/images/interface.png)

## ORSA-Input-Datei

### Zellenformatvorlagen

Die ORSA-Input-Datei enthält unter anderem folgende Zellenformatvorlagen:
- **Input**: Lachsfarben, vom Versicherer zu befüllen.
- **Linked Cell**: Hellblau, automatisch vom Tool befüllt.
- **Calculaliot**: Grau, enthält Excel-Formel.
- **No Input**: Dunkelgrau, ist leer zu lassen.

Alle übrigen Zellenformatvorlagen enthalten Beschriftungen und dienen der Übersicht.

### Blatt «General Inputs»

![Inputblatt «General Inputs»](/images/blatt-general-inputs.png)

Folgende Konfigurationen werden auf dem Blatt «General Inputs» gemacht:
- **Anzahl Simulationen**: Die Solvenzberechung des FINMA `sstCalculation` R-Tools basiert darauf.
Bei weniger Simulation (10'000) läuft das Tool schneller, aber die Solvenz kann unterschätzt werden; mehr Simulationen (100'000 oder 1'000'000) führen zu einem genaueren Ergebnis.
Wir empfehlen 1'000'000 für die endgültige Berechnung zu verwenden.
- **Planungsjahre**:  Eine Zahl zwischen \(0\) und \(5\).
Die Anzahl an Jahren, die im ORSA-Prozess betrachtet wird.
Bei \(0\) wird nur der aktuelle SST reproduziert.
- **Szenario (kurzer Name)**:  Der Name eines Szenarios.  
Höchstens 26 Zeichen, da Excel-Blätter höchstens 30 Zeichen lang sein dürfen und jedes Szenario-Blatt mit `Sz - ` beginnen muss.
- **Erstelle Szenario**:  Der Knopf erzeugt ein neues Blatt für das Szenario, dessen Name im Feld **Szenario (kurzer Name)** steht.
Aktualisiert ausserdem die Liste der verfügbaren Szenarien.
- **Gewinn-/Verlustvortrag \(N-1\) in Mio. CHF**: Bilanzposition Gewinn-/Verlustvortrag des Vorjahres \(N-1\) in Millionen Schweizer Franken.
Wird für die Bilanz im ORSA-Formular benötigt.
- **Verwendete Szenarien**: Eine Checkliste.  Nur ausgewählte Szenarien werden berechnet. *Die Planung wird immer berechnet*.
- **Aktualisiere Liste**: Der Knopf aktualisiert die **Verwendete-Szenarien**-Checkliste.  Wird benötigt, falls ein Szenario-Blatt gelöscht wurde.

### Blatt «Planung»

Auf dem Blatt «Planung» wird die erwartete finanzielle Entwicklung über die Planungsperiode eingegeben.
Folgende Eingabeblöcke stehen in der ORSA-Input-Datei zur Verfügung:

#### Block «Erfolgsrechnung»

![Inputblock «Erfolgsrechnung»](/images/block-erfolgsrechnung.png)

Im Block «Erfolgsrechnung» wird die Erfolgsrechnung gemäss AVO-FINMA eingegeben, wie sie für die Planungsperiode geplant ist.
Diese Erfolgsrechnung wird als «Budget» bezeichnet.

#### Block «Aufteilung Aktiven»»

![Inputblock «Aufteilung Aktiven»](/images/block-erfolgsrechnung.png)

Der Block «Aufteilung Aktiven» enthält einen Teil der Aktivpositionen der SST-Bilanz.
Im Block wird eingegeben, welchen prozentualen Anteil eine Position eine Position an ihrer Superposition ausmacht.
Zum Beispiel bedeutet \(50\ \%\) in der Zeile «1.1.1 Immobilien», dass \(50\ \%\) der Position «1.1 Kapitalanlagen» auf «1.1.1 Immobilien» entfallen;
\(25\ \%\) in der Zeile «Kollektive Kapitalanlagen» bedeutet, dass \(25\ \%\) der Position «1.1.7 Übrige Kapitalanlagen» auf «Kollektive Kapitalanlagen» entfallen, *nicht* etwa \(25\ \%\) der Position «1.1 Kapitalanlagen».

Beim ersten Lauf des Tools wird die blaue Spalte automatisch mit den Werten des aktuellen SSTs befüllt.

#### Block «Aufteilung Preisabhängige Assets und Beteiligungen»

![Inputblock «Aufteilung Preisabhängige Assets und Beteiligungen»](/images/block-aufteilung-preisabhängige-assets-und-beteiligungen.png)

Der Block «Aufteilung Preisabhängige Assets und Beteiligungen» entspricht 1 : 1 dem Blatt «Asset Prices» aus dem SST Template.
Im Block wird eingegeben, welchen prozentualen Anteil eine Währung an der entsprechenden Art ausmacht.
Zum Beispiel bedeutet \(50\ \%\) in der Zeile «Aktien»/«CHF», dass \(50\ \%\) der Aktien auf CHF entfallen.

Der Wert für «Total (immaterielle) Beteiligungen» ist \(100\ \%\) oder \(0\ \%\), je nach dem, ob der Versicherer Beteiligungen hält oder nicht.

In den Zeilen «Wohnimmobilien» und «Geschäftsimmobilien» wird eingegeben, welchen prozentualen Anteil die Wohn- bzw. Geschäftsimmobilien an allen Immobilien ausmachen.

Beim ersten Lauf des Tools wird die blaue Spalte automatisch mit den Werten des aktuellen SSTs befüllt.

#### Block «Aufteilung Festverzinliche Wertpapiere»

![Inputblock «Aufteilung Festverzinliche Wertpapiere»](/images/block-aufteilung-festverzinsliche-wertpapiere.png)

Der Block «Aufteilung Festverzinsliche Wertpapiere» entspricht 1 : 1 dem Blatt «Fixed Income» aus dem SST Template.
Im Block wird eingegeben, welchen prozentualen Anteil eine Währung-Rating-Kombination an allen gehaltenen festverzinslichen Wertpapieren ausmacht.
Zum Beispiel bedeuet \(50\ \%\) in der Zeile «CHF»/«GOVI», dass \(50\ \%\) aller gehaltenen festverzinslichen Wertpapiere in CHF sind und Rating GOVI haben.

Beim ersten Lauf des Tools wird die blaue Spalte automatisch mit den Werten des aktuellen SSTs befüllt.

#### Block «lognormal parameters»

![Inputblock «lognormal parameters»](/images/block-lognormal-parameters.png)

Der Block «lognormal parameters» entspricht den Spalten «lognormal parameters» auf dem Blatt «Non Life» aus dem SST Template.
Falls für die Berechnung des versicherungstechnischen Risikos die Parameter der Lognormalverteilung gewählt werden, sind die Parameter \(\mu\) respektive \(\sigma\) der Lognormalverteilung einzutragen.

Beim ersten Lauf des Tools wird die blaue Spalte automatisch mit den Werten des aktuellen SSTs befüllt.

#### Block «Anrechnung der Aktiven am gebundenen Vermögen»

![Inputblock «Anrechnung der Aktiven am gebundenen Vermögen»](/images/block-anrechnung-der-aktiven-am-gebundenen-vermögen.png)

Der Block «Anrechnung der Aktiven am gebundenen Vermögen» enthält einen Teil der Aktivpositionen der Bilanz gemäss AVO-FINMA.
Im Block wird angegeben, welcher prozentuale Anteil einer Position dem gebundenen Vermögen zugewiesen wird.
Zum Beispiel bedeuet \(50 \%\) in der Zeile «1.1.1 Immobilien», dass \(50\ \%\)  der Position «1.1.1 Immobilien» dem gebundenen Vermögen zugewiesen werden.

Die grauen Positionen dienen der Übersicht; sie können nicht dem gebundenen Vermögen zugewiesen werden.

#### Block «Fortschreibung der statutarischen Bilanz»

![Inputblock «Fortschreibung der statutarischen Bilanz»](/images/block-fortschreibung-der-statutarischen-bilanz.png)

Der Block «Fortschreibung der statutarischen Bilanz» führt alle Aktivpositionen der Bilanz gemäss AVO-FINMA, die sich im ORSA-Prozess verändern können.
Im Block wird angegeben, wie die statuarische Position abhängig von der entsprechenden marktnah-bewerteten Position in der SST Bilanz verändert wird.
Das Tool unterstützt drei Möglichkeiten, wie sich der statutarische Wert von Position \(A\) von Jahr \(N\) auf Jahr \(N+1\) ändern kann:
- **relativ**: Ändert sich der marktnahe Wert von \(A\) um \(X\ \%\), dann ändert sich der statutarische Wert von A um \(X\ \%\).
- **absolut**: Ändert sich der martknahe Wert von \(A\) um \(X\ \%\) CHF, dann ändert sich der statutarische Wert von A um \(X\) CHF.
- **konstant**: Der statutarische Wert von \(A\) ändert sich nicht.

### Blatt «Sz - `SZENARIO`»

Alle Blöcke aus dem Blatt «Planung» sind auch Teil des Szenario-Blattes und werden beim Erstellen des Blattes automatisch befüllt.
Zusätzlich sind folgene Blöcke verfügbar, um ein Szenario festzulegen:

#### Block «Auslenkung der Erfolgsrechnung»

![Inputblock «Auslenkung der Erfolgsrechnung»](/images/block-auslenkung-der-erfolgsrechnung.png)

Der Block «Auslenkung der Erfolgsrechnung» ist eine zweite Erfolgsrechnung gemäss AVO-FINMA.
Im Block wird ein optionaler Delta-Wert zum Budget eingegeben.
Eine leere Eingabe lässt die Position unberührt.
Der Block dient dazu, zwischen dem Budget und der *effektiv eingetretenen* Erfolgsrechnung zu unterscheiden.

Zum Beispiel bedeutet \(1\) Mio. CHF Bruttoprämie im Budget für Jahr \(N\) und \(-0.2\) Mio. CHF Bruttoprämie in der Auslenkung der Erfolgsrechnung für Jahr \(N\), dass für das Jahr \(N\) \(1\) Mio. CHF geplant oder budgetiert werden, aber effektiv (im ORSA-Prozess) nur \((0.8 = 1 - 0.2)\) Mio. CHF verdient werden.

In der Planung stimmen also Budget und effektiv eingetretene Erfolgsrechnung überein.
Das Budget dient unter anderem als Grundlage für das erwartete versicherungstechnische Ergebnis; die effektiv eingetretene Erfolgsrechnung (im Jahr \(N\)) hat unter anderem Auswirkung auf den Wert der Aktiven zum Anfang vom Jahr \(N+1\).

In die letzte Spalte kann nichts eingegeben werden, weil die Solvenz jeweils zum Anfang jedes Jahres berechnet wird.

#### Block «Wertveränderung der Aktiven»

![Inputblock «Wertveränderung der Aktiven»](/images/block-wertveränderung-der-aktiven.png)

Der Block «Wertveränderung der Aktiven» enthält alle Aktivpositionen, die sich im ORSA-Prozess verändern können.
Eine leere Eingabe lässt die Position unberührt.
Im Block wird angegeben, um welchen prozentualen Wert sich eine Position verändert.
Zum Beispiel bedeutet \(-50 \%\) in der Zeile «1.1.1 Immobilien» in der Spalte \(N\), dass die Position «1.1.1 Immobilien» während des Jahres \(N\) \(50 %\) des Werts verliert und der Wert der Position am Anfang des Jahres \(N+1\) entsprechend tiefer ist.

In die letzte Spalte kann nichts eingegeben werden, weil die Solvenz jeweils zum Anfang jedes Jahres berechnet wird.

#### Block «Auslenkung der gesamten Zinskurve»

![Inputblock «Auslenkung der gesamten Zinskurve»](/images/block-auslenkung-der-gesamten-zinskurve.png)

Der Block «Auslenkung der gesamten Zinskurve» enthält alle Währungen, die im Blatt «Market Initial Values» im SST Template eine Zinskurve hat.
Im Block wird angegeben, um wie viele Basispunkte eine Zinskurve ausgelenkt wird.
Eine leere Eingabe lässt die Zinskurve unberührt.
Zum Beispiel bedeutet \(-50\) in der Zeile «CHF» in der Spalte \(N\), dass die Zinskurve von CHF während des Jahres \(N\) um \(50\) Basispunkte tiefer ist als im SST.
Dies hat unter anderem Auswirkungen auf den Diskontierungsfaktor in der Berechnung vom bestmöglicher Schätzwert der Versicherungsverpflichtungen zum Anfang des Jahres \(N+1\).

In die letzte Spalte kann nichts eingegeben werden, weil die Solvenz jeweils zum Anfang jedes Jahres berechnet wird.

#### Block «Konstante Zinskurve»

![Inputblock «Konstante Zinskurve»](/images/block-konstante-zinskurve.png)

Der Block «Konstante Zinskurve» enthält alle Währungen, die im Blatt «Market Initial Values» im SST Template eine Zinskurve hat.
Im Block wird angegeben, welchen konstanten Wert eine Zinskurve haben soll.
Eine leere Eingabe lässt die Zinskurve unberührt.
Zum Beispiel bedeutet \(100\) in der Zeile «CHF» in der Spalte \(N\), dass die Zinskurve von CHF während des Jahres \(N\) konstant \(100\) Basispunkte ist.
Dies hat unter anderem Auswirkungen auf den Diskontierungsfaktor in der Berechnung vom bestmöglicher Schätzwert der Versicherungsverpflichtungen zum Anfang des Jahres \(N+1\).

In die letzte Spalte kann nichts eingegeben werden, weil die Solvenz jeweils zum Anfang jedes Jahres berechnet wird.

## ORSA-Prozess

In diesem Abschnitt wird beschrieben, wie das Tool ein SST Template von Jahr \(N\) zu Jahr \(N+1\) überführt, basierend auf den Eingaben in der ORSA-Input-Datei.
Es werden alle Blätter im SST Template behandelt, die verändert werden, und zwar in der Reihenfolge, in der sie das Tool verändert; die Reihenfolge kann von der Reihenfolge der Blätter im SST Template abweichen.
