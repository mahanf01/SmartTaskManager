# ADR-003: Lokales Datum des Nutzers als API-Parameter

- **Status:** vorgeschlagen (Review durch das Team ausstehend)
- **Datum:** 23.11.2026

## Kontext

Die Dringlichkeit einer Aufgabe hängt von den Kalendertagen bis zur Frist ab, gemessen am **lokalen Datum des Nutzers** (requirements.md Kapitel 7). Der Server läuft in UTC. In Mitteleuropa hätte er zwischen 0 und 1 Uhr (Sommerzeit: 2 Uhr) noch das Datum vom Vortag – die Sortierung wäre falsch und US5 verletzt (Risiko R1 in architecture.md).

## Entscheidung

Der Browser schickt sein lokales Datum als Query-Parameter `today` (`YYYY-MM-DD`) mit: `GET /api/tasks?status=open&today=2026-11-23`. Der Server validiert das Format und übergibt das Datum an die reine Funktion `prioritize(tasks, today)` im Modul `prioritization`. Fehlt der Parameter bei `status=open`, antwortet der Server mit `400`.

Folgen für die Umsetzung:

- Die Seite mit der offenen Liste ist eine Client Component, die `/api/tasks` im Browser aufruft – eine vom Server gerenderte Seite kennt das lokale Datum nicht.
- Der Browser bildet `today` mit `toLocaleDateString('en-CA')` (lokale Zeit), **nicht** mit `toISOString()` (UTC – würde das Problem wieder einführen).
- Der Server rechnet die Tage bis zur Frist nur mit Kalenderdaten ohne Uhrzeit, damit Sommer-/Winterzeit-Umstellungen keine Fehler erzeugen.

## Betrachtete Alternativen

- **Server rechnet mit UTC:** verworfen – falsche Sortierung in den Nachtstunden.
- **Zeitzone im Nutzerkonto speichern:** verworfen – zusätzliches Datum pro Nutzer (Datensparsamkeit, NF5) und falsch, wenn der Nutzer verreist.
- **Sortierung nur im Browser:** verworfen – API liefert unsortierte Daten, die Sortierung wäre nicht über HTTP-Tests prüfbar.

## Konsequenzen

- Die Sortierlogik bleibt auf dem Server und in einer reinen Funktion.
- Tests können ein festes Datum übergeben und sind damit reproduzierbar – auch Ende-zu-Ende-Tests des Demo-Ablaufs.
- Ein Nutzer kann ein falsches Datum schicken. Das betrifft nur seine eigene Ansicht und ist kein Sicherheitsproblem.
- Die Erinnerung in der App (F6, „morgen fällig“) nutzt denselben Parameter.
