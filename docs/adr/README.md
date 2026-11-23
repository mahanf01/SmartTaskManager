# Architecture Decision Records (ADRs)

Hier dokumentieren wir wichtige Architekturentscheidungen. Jede Entscheidung bekommt eine eigene Datei mit fortlaufender Nummer.

## Übersicht

| Nr. | Titel | Status | Datum |
|---|---|---|---|
| [0001](0001-nextjs-monolith.md) | Full-Stack-Monolith mit Next.js statt getrenntem Front- und Backend | Vorgeschlagen | 2026-11-09 |
| [0002](0002-postgresql-mit-prisma.md) | PostgreSQL mit Prisma als Datenbank | Vorgeschlagen | 2026-11-09 |
| [0003](0003-score-zur-laufzeit-berechnen.md) | Prioritäts-Score zur Laufzeit berechnen statt speichern | Vorgeschlagen | 2026-11-09 |
| [0004](0004-auth-mit-authjs.md) | Authentifizierung mit Auth.js (E-Mail + Passwort) | Vorgeschlagen | 2026-11-09 |
| [0005](0005-erinnerungen-per-cron.md) | Erinnerungen per täglichem Cron-Job und E-Mail | Vorgeschlagen | 2026-11-09 |

## Status-Werte

- **Vorgeschlagen** – von AI und/oder einem Teammitglied vorgeschlagen, noch nicht vom Team bestätigt
- **Angenommen** – vom Team beschlossen
- **Abgelehnt** – geprüft und verworfen (Datei bleibt zur Nachvollziehbarkeit)
- **Ersetzt durch ADR-XXXX** – durch eine spätere Entscheidung abgelöst

Eine angenommene ADR wird nicht mehr geändert. Ändert sich die Entscheidung, wird eine neue ADR geschrieben.

## Vorlage

Neue ADRs mit [template.md](template.md) anlegen.
