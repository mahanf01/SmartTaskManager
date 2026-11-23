# Architecture Decision Records

Wichtige Architekturentscheidungen werden hier als kurze Records festgehalten. Ein ADR wird nicht nachträglich umgeschrieben; ändert sich eine Entscheidung, entsteht ein neuer ADR, der den alten ersetzt („Status: ersetzt durch ADR-00x“).

| Nr. | Titel | Status | Datum |
|---|---|---|---|
| [001](001-modularer-monolith-nextjs.md) | Modularer Monolith mit Next.js | angenommen | 23.11.2026 |
| [002](002-supabase-fuer-datenbank-und-auth.md) | Supabase für Datenbank und Auth | angenommen | 23.11.2026 |
| [003](003-lokales-datum-als-api-parameter.md) | Lokales Datum des Nutzers als API-Parameter | angenommen | 23.11.2026 |
| [004](004-hosting-vercel-und-supabase-eu.md) | Hosting auf Vercel Hobby und Supabase Free in Frankfurt | angenommen | 23.11.2026 |

## Vorlage

```markdown
# ADR-00x: Titel

- **Status:** vorgeschlagen | angenommen | ersetzt durch ADR-00y
- **Datum:** TT.MM.JJJJ

## Kontext
Welches Problem? Welche Anforderungen und Rahmenbedingungen?

## Entscheidung
Was wurde entschieden?

## Betrachtete Alternativen
Welche Optionen gab es, warum wurden sie verworfen?

## Konsequenzen
Was wird dadurch einfacher, was schwieriger? Welche Risiken entstehen?
```
