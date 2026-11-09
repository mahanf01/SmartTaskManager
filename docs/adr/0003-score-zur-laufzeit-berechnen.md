# ADR-0003: Prioritäts-Score zur Laufzeit berechnen statt speichern

- **Status:** Vorgeschlagen
- **Datum:** 2026-11-09
- **Betroffene Anforderungen:** F4, F8, US5, US10, NF2

## Kontext

Die Liste wird nach einem Score sortiert (siehe [requirements.md, Kapitel 7](../requirements.md#7-mvp-scope)):

```
Dringlichkeit D = 10, wenn Frist heute/überfällig, sonst max(1, 10 − Tage bis zur Frist)
Score = 0,6 × Wichtigkeit + 0,4 × D
```

Die Dringlichkeit hängt vom **heutigen Datum** ab. Dieselbe Aufgabe hat morgen einen anderen Score als heute, auch wenn niemand sie ändert. Außerdem hängt „heute“ von der Zeitzone des Nutzers ab.

## Betrachtete Optionen

1. **Score als Spalte speichern** und bei jeder Änderung der Aufgabe neu berechnen
2. **Score speichern + nächtlicher Job**, der alle Scores neu berechnet
3. **Score bei jeder Abfrage berechnen**, in einer reinen TypeScript-Funktion
4. **Score bei jeder Abfrage in SQL berechnen** (`ORDER BY` mit Formel)

## Entscheidung

Wir wählen **Option 3**: Der Score wird bei jedem `GET /api/v1/tasks` in der reinen Funktion `prioritize(tasks, today)` berechnet und nicht gespeichert.

- `today` wird aus der Zeitzone des Nutzers bestimmt und als Parameter übergeben.
- Die Funktion hat keinen Zugriff auf Datenbank oder Uhrzeit. Sie ist dadurch vollständig mit Unit-Tests prüfbar (z. B. mit den Beispielen aus requirements.md).
- Die Gewichte (0,6 / 0,4) liegen in einer Konfiguration.
- Die API liefert den Score als nur lesbares Feld `priority` mit aus.

## Konsequenzen

**Positiv**
- Der Score ist immer aktuell, auch um Mitternacht und bei verschiedenen Zeitzonen.
- Keine doppelte Datenhaltung, kein zusätzlicher Job, keine veralteten Werte.
- Änderungen an der Regel (siehe offener Punkt unten) betreffen nur eine Funktion.
- Die Sortierung inkl. manueller Priorität (`manualPin`) ist an einer Stelle zusammengefasst.

**Negativ / Risiken**
- Es werden immer alle offenen Aufgaben eines Nutzers geladen und im Speicher sortiert. Bei typischen Nutzern (unter einigen hundert offenen Aufgaben) ist das schnell genug. Bei sehr vielen Aufgaben wäre Option 4 (SQL) besser – das ist dann ein neues ADR.
- Seitenweises Laden (Pagination) ist so erst nach dem Sortieren möglich.

## Offener Punkt für das Team

Bei der Umsetzung ist ein Widerspruch in der Regel aufgefallen: „Wichtigkeit ist schwerer gewichtet als Frist“, aber Aufgaben **ohne** Frist stehen immer **unter** allen Aufgaben mit Frist. Eine Aufgabe mit Wichtigkeit 10 ohne Frist steht damit unter einer Aufgabe mit Wichtigkeit 1 und Frist in 30 Tagen. Vorschlag: Aufgaben ohne Frist mit D = 1 in den normalen Score aufnehmen. Durch diese ADR ist eine Änderung nur eine Anpassung in `prioritize()`.

## AI-Einsatz

Der Widerspruch oben wurde von der AI beim Ausarbeiten des Codes für die Sortierung gefunden. Die AI hat die Regel **nicht** eigenmächtig geändert, sondern sie wie beschlossen umgesetzt und den Punkt an das Team gemeldet.
