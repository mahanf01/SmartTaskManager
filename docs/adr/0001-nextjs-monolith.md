# ADR-0001: Full-Stack-Monolith mit Next.js statt getrenntem Front- und Backend

- **Status:** Vorgeschlagen
- **Datum:** 2026-11-09
- **Betroffene Anforderungen:** alle F-Anforderungen, NF2, NF4; Phase 2 (GitHub → CD → Vercel)

## Kontext

Wir müssen festlegen, wie die Anwendung grundsätzlich aufgebaut ist. Rahmenbedingungen:

- kleines Team, begrenzte Zeit im Semester
- kleines MVP (Login, Aufgaben-CRUD, Priorisierung, Abhaken)
- Phase 2 plant Continuous Deployment von GitHub auf **Vercel**
- die App soll im Browser und am Handy nutzbar sein (NF4)

## Betrachtete Optionen

1. **Next.js-Monolith** – UI und REST-API in einer Next.js-Anwendung, serverless auf Vercel
2. **Getrenntes Frontend und Backend** – React-SPA + eigenes REST-Backend (z. B. Spring Boot oder Express) als Container
3. **Backend-as-a-Service** – React-Frontend direkt gegen Supabase (Auth, DB, Row Level Security)

Ausführlicher Vergleich: [architecture.md, Kapitel 3](../architecture.md#3-untersuchte-architekturvarianten)

## Entscheidung

Wir nutzen **Option 1: einen modularen Monolithen mit Next.js (TypeScript)**.

- Frontend und API liegen in einem Repository und werden gemeinsam deployt.
- Die API ist trotzdem eine echte REST-API unter `/api/v1` (keine reinen Server Actions), damit sie testbar ist und später von anderen Clients genutzt werden kann.
- Die Logik (z. B. Priorisierung) liegt in eigenen Modulen unter `lib/`, getrennt vom Framework.

## Konsequenzen

**Positiv**
- Nur eine Sprache, ein Projekt, ein Deployment – passt zum Teamumfang.
- Vercel unterstützt Next.js direkt; Phase 2 wird einfach.
- Kein eigener Server zu betreiben; Gratis-Stufe reicht für das MVP.
- Die Priorisierungslogik bleibt im eigenen Code und ist mit Unit-Tests prüfbar.

**Negativ / Risiken**
- Frontend und Backend sind enger gekoppelt als bei Option 2.
- Serverless bringt Kaltstarts und kurze DB-Verbindungen mit sich (Gegenmaßnahme: Connection-Pooler, Region Frankfurt, NF2 messen).
- Teilweise Bindung an Vercel (Cron, Integration). Gegenmaßnahme: Next.js kann als Docker-Container auch woanders laufen.
- Wird später eine native App gebaut oder das Backend stark wachsen, kann eine Aufteilung nötig werden. Die versionierte REST-API macht das möglich.

## AI-Einsatz

Die AI (Claude Code) hat alle drei Optionen vorgeschlagen und verglichen und Option 1 empfohlen. Ohne den Hinweis auf Phase 2 (Vercel) und das kleine Team wäre die Empfehlung weniger eindeutig gewesen. Die Entscheidung für eine **REST-API statt Server Actions** wurde nach kritischer Prüfung bewusst getroffen (mehr Code, aber besser testbar).
