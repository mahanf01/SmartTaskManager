# AI-Einsatz und Reflexion – SmartTaskManager

Dieses Dokument beschreibt, wie AI in Phase 1 (Anforderungsanalyse) und in der Architekturphase eingesetzt wurde und was wir daraus gelernt haben.

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

---

## AI-Einsatz in der Architekturphase

### Eingesetzte Tools

| Tool | Zweck |
|---|---|
| Claude Code | Repository lesen, Architekturvarianten vorschlagen und vergleichen, Diagramme (Mermaid) erstellen, ADRs und Dokumente schreiben, Hosting-Preise per Websuche nachprüfen |

### Prompt

> „Lies dich durch die Dateien im Repo und erstelle eine nachvollziehbare Softwarearchitektur mit AI-Unterstützung: mindestens zwei Architekturvarianten, Technologien auswählen und begründen, Architekturdiagramm (z. B. C4), Datenmodell, API-Design, mindestens zwei ADRs, die AI-generierte Architektur kritisch hinterfragen, Hosting-Möglichkeiten vergleichen.“

Wichtig war, dass die AI das ganze Repository als Kontext hatte (Anforderungen, MVP-Scope, geplantes Vercel-Deployment).

### Ergebnisse

| Ergebnis | Dokument |
|---|---|
| Drei Architekturvarianten mit Vergleich und Empfehlung | [architecture.md, Kap. 3](architecture.md#3-untersuchte-architekturvarianten) |
| Technologieauswahl mit Alternativen | [architecture.md, Kap. 5](architecture.md#5-technologieauswahl-und-begründung) |
| C4-Diagramme (Kontext, Container, Komponenten), ER-Diagramm, Sequenzdiagramme | [architecture.md](architecture.md), [diagrams/](diagrams/) |
| Datenmodell und REST-API | [architecture.md, Kap. 7 und 8](architecture.md#7-datenmodell) |
| Fünf ADRs | [adr/](adr/README.md) |
| Hosting-Vergleich mit Kosten | [hosting.md](hosting.md) |
| Kritische Prüfung (12 Punkte) | [architecture.md, Kap. 10](architecture.md#10-kritische-prüfung-der-ai-generierten-architektur) |

### Was wir kritisch geprüft haben

- **Überdimensionierung:** Architekturen mit Microservices, Cache und Warteschlangen wären für unser MVP zu viel. Wir bleiben bei einem Monolithen.
- **Veraltete Fakten:** Preise und Gratis-Limits aus dem AI-Wissen können falsch sein. Sie wurden per Websuche auf den Anbieterseiten nachgeprüft.
- **Fachliche Fehler:** Ein gespeicherter Score wäre am nächsten Tag falsch; „heute“ hängt von der Zeitzone ab.
- **Sicherheit:** Schutz gegen Passwort-Raten und gegen Zugriff auf fremde Aufgaben musste ausdrücklich ergänzt werden.
- **Widerspruch in unseren eigenen Anforderungen:** Beim Ausarbeiten der Sortierung fiel auf, dass Aufgaben ohne Frist immer unten stehen, obwohl „Wichtigkeit schwerer gewichtet“ sein soll. Die AI hat das gemeldet, aber nicht selbst geändert – die Entscheidung liegt beim Team.

### Reflexion

**Gut:**
- Schneller Überblick über mehrere Varianten mit klaren Vor- und Nachteilen.
- Diagramme als Code (Mermaid) sind versionierbar und leicht zu ändern.
- Die AI hat beim Umsetzen in konkrete Strukturen (Datenmodell, API) Lücken in den Anforderungen gefunden.

**Grenzen:**
- Ohne Kontext (Teamgröße, Vercel, Zeitrahmen) schlägt die AI eher zu große Lösungen vor.
- Anbieterdaten müssen immer nachgeprüft werden.
- Die AI kann Vor- und Nachteile nennen, aber nicht wissen, was dem Team wichtig ist (z. B. Lernziel vs. wenig Aufwand). Deshalb haben alle ADRs den Status *Vorgeschlagen*, bis das Team sie bestätigt.
