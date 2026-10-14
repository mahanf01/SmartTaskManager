# AI-Einsatz und Reflexion – SmartTaskManager

Dieses Dokument beschreibt, wie AI in Phase 1 (Anforderungsanalyse) eingesetzt wurde und was wir daraus gelernt haben.

## Eingesetzte Tools

| Tool | Zweck |
|---|---|
| Claude (Chat) | Ideen sammeln, User Stories formulieren, Anforderungen auf Widersprüche prüfen |
| Claude Code | Aufbereitung der Ergebnisse als Markdown-Dokumentation im Repository (README, requirements, ai-usage) |

## Einsatz in der Anforderungsanalyse

### 1. Ideen sammeln

**Prompt:**
> „Welche Funktionen braucht eine Task-App mit automatischer Priorisierung?“

**Ergebnis:** Eine Liste möglicher Funktionen (z. B. Fristen, Wichtigkeit, Erinnerungen, Kategorien, Teamfunktionen, Kalender-Anbindung).

**Was wir übernommen haben:** Die Kernfunktionen für die Priorisierung (Frist, Wichtigkeit, automatische Sortierung) sowie Erinnerungen und Kategorien. Teamfunktionen und Kalender-Anbindung haben wir bewusst als *Won't (vorerst)* eingestuft, um den Umfang klein zu halten.

### 2. User Stories formulieren

**Prompt (sinngemäß):**
> „Bringe diese Rohideen in das Format ‚Als … möchte ich … damit …‘.“

**Ergebnis:** Ausformulierte User Stories US1–US10.

**Was wir angepasst haben:** Formulierungen vereinfacht und an unsere Zielgruppe angepasst; die Skala für die Wichtigkeit haben wir selbst auf 1–10 festgelegt.

### 3. Anforderungen prüfen

**Prompt:**
> „Finde Widersprüche und unklare Stellen in diesen Anforderungen.“

**Ergebnis:** Die AI hat u. a. folgende Punkte gefunden:

- Die Priorisierungsregel war nicht definiert („Was ist wichtig?“).
- Automatische Sortierung (US5) und manuelle Priorität (US10) widersprechen sich.
- Aufgaben ohne Frist waren nicht berücksichtigt.
- Der Zeitpunkt für Erinnerungen war offen.
- NF1 („schnell anlegen“) war nicht messbar.

**Unsere Entscheidungen:** Die Lösungen (z. B. „Manuelle Priorität hat immer Vorrang“, „Erinnerung 1 Tag vor der Frist“, „unter 10 Sekunden“) haben wir als Team getroffen. Die AI hat Probleme aufgezeigt, die Entscheidung lag bei uns. Details siehe [requirements.md – Kapitel 8](requirements.md#8-widersprüche-und-unklarheiten).

### 4. Dokumentation

Mit Claude Code wurden die Ergebnisse in die Markdown-Dokumente dieses Repositories überführt. Dabei wurden zusätzlich ergänzt:

- Akzeptanzkriterien pro User Story
- Zuordnung User Stories ↔ funktionale Anforderungen
- ein konkreter Formelvorschlag für die Priorisierungsregel
- weitere offene Punkte (#6–#10 in Kapitel 8)

Diese Ergänzungen sind als **Vorschläge** gekennzeichnet und müssen vom Team noch geprüft und bestätigt werden.

## Grundsatz

**Die Ergebnisse der AI wurden immer selbst geprüft und angepasst.** Die AI diente als Werkzeug zum Strukturieren und Hinterfragen – nicht als Entscheider.

## Reflexion

### Was gut funktioniert hat

- **Schneller Einstieg:** Aus einer groben Idee entstand in kurzer Zeit eine umfangreiche Funktionsliste.
- **Einheitliches Format:** Die AI hat Rohideen zuverlässig in sauber formulierte User Stories übersetzt.
- **Kritischer Blick:** Die Prüfung auf Widersprüche hat Lücken aufgedeckt, die uns selbst nicht aufgefallen waren – vor allem den Konflikt zwischen automatischer und manueller Priorisierung.

### Grenzen und Probleme

- **Zu viele Ideen:** Die AI schlägt eher zu viele Funktionen vor. Die Abgrenzung des MVP mussten wir selbst vornehmen.
- **Keine Entscheidungen:** Bei Widersprüchen nennt die AI mehrere Möglichkeiten, die Entscheidung für eine Lösung muss das Team treffen.
- **Unpräzise Formulierungen:** Vorschläge wie „schnell“ oder „einfach“ sind nicht messbar und mussten konkretisiert werden.
- **Kontext fehlt:** Die AI kennt unsere Zielgruppe und unseren Zeitrahmen nicht, wenn man sie nicht ausdrücklich nennt.

### Was wir gelernt haben

- Gute Ergebnisse hängen stark von präzisen Prompts ab – je mehr Kontext (Zielgruppe, Umfang), desto brauchbarer die Antwort.
- AI eignet sich besonders gut als „Reviewer“, um eigene Anforderungen auf Lücken und Widersprüche zu prüfen.
- Jede AI-Ausgabe muss kritisch geprüft werden; Verantwortung und Entscheidungen bleiben beim Team.
