# ADR-0002: PostgreSQL mit Prisma als Datenbank

- **Status:** Vorgeschlagen
- **Datum:** 2026-11-09
- **Betroffene Anforderungen:** F1–F8, NF2, NF5

## Kontext

Wir brauchen eine Datenbank für Nutzer, Aufgaben und später Kategorien. Die Daten sind klar strukturiert und hängen zusammen (ein Nutzer hat viele Aufgaben). Es gibt feste Regeln, z. B. Wichtigkeit 1–10 und „Titel ist Pflicht“. Wird ein Konto gelöscht, müssen alle Aufgaben mit gelöscht werden (DSGVO).

Auf Vercel gibt es keine lokale Festplatte für eine Datenbank. Die DB muss also ein gemanagter Dienst sein.

## Betrachtete Optionen

1. **PostgreSQL** (gemanagt, z. B. Neon, Supabase, Render) + **Prisma ORM**
2. **PostgreSQL + Drizzle ORM**
3. **MongoDB** (Dokument-Datenbank, z. B. MongoDB Atlas)
4. **SQLite** (Datei-Datenbank)

## Entscheidung

Wir nutzen **PostgreSQL** bei **Neon in der Region Frankfurt** und greifen mit **Prisma** darauf zu.

- Regeln stehen direkt in der Datenbank: `CHECK (importance BETWEEN 1 AND 10)`, `NOT NULL`, Fremdschlüssel mit `ON DELETE CASCADE`.
- Das Schema (`prisma/schema.prisma`) und die Migrationen liegen im Repository.
- Für Serverless nutzen wir den Connection-Pooler von Neon.

## Konsequenzen

**Positiv**
- Relationale Daten passen gut zu Nutzer → Aufgaben → Kategorien.
- Datenbank prüft Regeln selbst; Fehler im Code führen nicht zu kaputten Daten.
- Konto löschen löscht dank `CASCADE` automatisch alle Aufgaben (NF5).
- PostgreSQL ist Standard: Wechsel des Anbieters (Neon → Supabase, Render, Hetzner) ist nur eine neue Verbindungs-URL.
- Prisma erzeugt TypeScript-Typen und Migrationen; das Schema dient als Dokumentation.

**Negativ / Risiken**
- Die Gratis-DB von Neon geht bei Inaktivität schlafen. Die erste Anfrage danach ist langsamer (Risiko für NF2).
- Prisma ist eine zusätzliche Abstraktion; komplexe SQL-Abfragen sind teilweise umständlich (dann `$queryRaw`).
- Neon ist ein US-Anbieter; für DSGVO ist ein Auftragsverarbeitungsvertrag nötig (siehe [hosting.md](../hosting.md)).

**Warum nicht die anderen?**
- *MongoDB:* Kein Schema und keine Fremdschlüssel; unsere Daten sind klar relational.
- *SQLite:* Auf Vercel nicht dauerhaft speicherbar (keine feste Festplatte).
- *Drizzle:* Gute Alternative, leichter als Prisma. Prisma wurde wegen besserer Doku und Migrationswerkzeuge gewählt; ein Wechsel später ist möglich.

## AI-Einsatz

Die AI hat PostgreSQL empfohlen und die Anbieter verglichen. Die Limits der Gratis-Stufen (z. B. „Supabase pausiert nach 1 Woche Inaktivität“) wurden im Web nachgeprüft, weil sich solche Angaben oft ändern. Dieser Punkt war für die Wahl von Neon statt Supabase entscheidend: Ein Uni-Projekt wird oft mehrere Wochen nicht genutzt.
