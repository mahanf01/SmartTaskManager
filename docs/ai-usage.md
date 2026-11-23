# AI-Einsatz und Reflexion – SmartTaskManager

Dieses Dokument beschreibt, wie AI in Phase 1 (Anforderungsanalyse) und Phase 2 (Architektur & Design) eingesetzt wurde und was wir daraus gelernt haben.

## Eingesetzte Tools

| Tool | Zweck |
|---|---|
| Claude (Chat) | Ideen sammeln, User Stories formulieren, Anforderungen auf Widersprüche prüfen |
| Claude Code | Phase 1: Aufbereitung der Ergebnisse als Markdown-Dokumentation (README, requirements, mvp, ai-usage), Review gegen die Aufgabenstellung. Phase 2: Architekturvarianten, Entwürfe für architecture, data-model, api, hosting und ADRs, Recherche mit Websuche, Review durch mehrere unabhängige AI-Prüfer |

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

Diese Ergänzungen waren zunächst als **Vorschläge** gekennzeichnet. Das Team hat sie im Rahmen des Reviews (Abschnitt 5) geprüft und entschieden.

### 5. Review der Phase-1-Dokumente

**Prompt (sinngemäß):**
> „Hier ist die Aufgabenstellung für Phase 1. Passt unsere Dokumentation dazu?“

**Ergebnis:** Claude Code hat die Dokumente mit den geforderten Deliverables und der Definition of Done verglichen und u. a. gefunden:

- Das Deliverable `docs/mvp.md` fehlte; ausgeschlossene Funktionen waren nirgends ausdrücklich aufgelistet.
- Die Phasen-Tabelle im README beschrieb Phase 2 falsch („Continuous Deployment → Vercel“) und legte damit eine Hosting-Plattform fest, bevor Alternativen verglichen wurden.
- Die offenen Punkte #6–#10 waren noch nicht entschieden, obwohl sie Datenmodell und Hosting in Phase 2 beeinflussen.
- Die Formel war nur ein „Vorschlag“ – damit war US5 nicht testbar.
- Neue Unklarheiten: Wo landen erledigte Aufgaben (US6)? Über welchen Kanal kommt die Erinnerung (US8)?
- Ein Widerspruch in der eigenen früheren Formel (siehe unten).

Zu jedem Punkt hat die AI Optionen mit einer Empfehlung genannt. Das Team hat die Optionen geprüft und die Empfehlungen übernommen; die Entscheidungen stehen in [requirements.md – Kapitel 8](requirements.md#8-widersprüche-und-unklarheiten) (#6–#14). Aus der DSGVO-Entscheidung (#7) entstand die neue Story US11 (Konto löschen). Anschließend hat Claude Code `docs/mvp.md` erstellt und README und requirements.md angepasst. US12 und die Entscheidungen #15–#18 kamen in Phase 2 hinzu (siehe dort).

**Beispiel für einen korrigierten AI-Vorschlag:** Der ursprüngliche Formelvorschlag (aus Schritt 4) sortierte Aufgaben ohne Frist immer unter alle Aufgaben mit Frist. Dadurch stand „Wichtigkeit 10 ohne Frist“ unter „Wichtigkeit 1, Frist in 30 Tagen“ – obwohl unsere Regel sagt, dass Wichtigkeit schwerer gewichtet ist. Erst die zweite Prüfung hat diesen Fehler aufgedeckt. Jetzt erhalten Aufgaben ohne Frist die Dringlichkeit 1 und werden über denselben Score sortiert. Das macht die Regel konsistent und die Umsetzung einfacher (kein Sonderfall).

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
- **Fehler in eigenen Vorschlägen:** Die von der AI vorgeschlagene Formel enthielt einen Widerspruch zu unserer eigenen Regel. Dass die AI ihn später selbst gefunden hat, zeigt: Auch AI-Vorschläge, die plausibel aussehen, müssen gegen die Anforderungen geprüft werden.

### Was wir gelernt haben

- Gute Ergebnisse hängen stark von präzisen Prompts ab – je mehr Kontext (Zielgruppe, Umfang), desto brauchbarer die Antwort.
- AI eignet sich besonders gut als „Reviewer“, um eigene Anforderungen auf Lücken und Widersprüche zu prüfen – am besten, indem man ihr die Aufgabenstellung mitgibt, damit sie gegen konkrete Kriterien prüft.
- Jede AI-Ausgabe muss kritisch geprüft werden; Verantwortung und Entscheidungen bleiben beim Team.

---

# Phase 2 – Architektur & Design

## 1. Architekturvarianten erarbeiten

**Tool:** Claude Code

**Prompt (sinngemäß):**
> „Vergleiche Architekturvarianten für unser MVP. Das Team kennt Next.js und Vercel.“

**Ergebnis:** Drei Varianten (A: modularer Monolith mit Next.js, B: getrenntes Frontend und Backend, C: Next.js mit Backend-as-a-Service wie Supabase), Microservices als bewusst verworfen. Bewertung anhand der Anforderungen aus Phase 1, Empfehlung Variante A. Zusätzlich hat die AI Risiken identifiziert, u. a. das Zeitzonenproblem bei der Priorisierung (R1) – siehe [architecture.md](architecture.md).

**Unsere Entscheidung:** Variante A übernommen.

## 2. Kritisch hinterfragt: Login und Datenbank

**AI-Empfehlung:** Für das Login eine Bibliothek wie Auth.js oder Better Auth, für den Datenbankzugriff ein ORM wie Prisma oder Drizzle. Variante C (Supabase) wurde u. a. mit der Begründung abgewertet, die Zugriffsregeln in Supabase seien „neues Wissen fürs Team“.

**Unser Einwand:** Diese Annahme war falsch – im Team gibt es Erfahrung mit Supabase aus mehreren Projekten. Supabase bietet Login und Datenbank in einem Dienst.

**Ergebnis:** Wir bleiben bei Variante A, nutzen aber Supabase als Infrastruktur für Datenbank und Login. Dadurch fallen zwei offene Technologieentscheidungen (Auth-Bibliothek, ORM) weg, weniger eigener sicherheitskritischer Code ist nötig, und die Bewertung von Variante C beim Team-Know-how wurde korrigiert. Die Grenze zu Variante C bleibt klar: Der Browser greift nie direkt auf die Datenbank zu, die Geschäftslogik läuft auf dem eigenen Server.

**Lehre:** Die AI hat eine Annahme über unser Team getroffen, ohne sie zu kennen. Ihre Empfehlung war technisch plausibel, aber für unsere Situation nicht die beste. Kontext, den nur das Team hat (Vorerfahrung), muss aktiv eingebracht werden.

## 3. Kritisch hinterfragt: Begründung gegen Variante C

**Unsere Frage:** „Ist nicht ohnehin klar, dass man Variante C nicht macht?“

**Antwort der AI:** Nein – C ist ein verbreiteter, von Supabase vorgesehener Ansatz und wäre für unsere App umsetzbar. Gerade deshalb ist C die aussagekräftigste Alternative. Dabei hat die AI eine Schwäche in ihrer eigenen Begründung eingeräumt: Sie hatte C u. a. verworfen, weil die Sortierlogik im Browser „vom Nutzer manipulierbar“ sei – in ADR-003 aber selbst festgehalten, dass ein Nutzer damit nur seine eigene Ansicht verändert.

**Ergebnis:** Das Manipulations-Argument wurde gestrichen. Die Begründung stützt sich jetzt auf belastbare Punkte: Konto löschen braucht ohnehin Server-Code (Admin-Schlüssel), zwei Schutzschichten statt einer, explizite REST-API. Die Bewertungstabelle in [architecture.md](architecture.md) wurde fairer gestaltet (Testbarkeit bei C hochgestuft, Kriterien „Konto löschen“ und „Explizite API“ ergänzt).

**Lehre:** Die AI liefert Argumente, die plausibel klingen, aber nicht immer zueinander passen. Erst die Nachfrage hat den Widerspruch zwischen zwei Dokumenten sichtbar gemacht.

## 4. Hosting recherchieren

**Tool:** Claude Code mit Websuche

**Prompt (sinngemäß):**
> „Recherchiere aktuelle Preise und Gratis-Limits für das Hosting und vergleiche die Anbieter.“

**Vorgehen:** Die Websuche lieferte überwiegend Blogs von Drittanbietern, die sich teilweise widersprachen (z. B. bei Funktionslaufzeiten und Freikontingenten). Die AI hat die entscheidenden Angaben deshalb auf den offiziellen Seiten von Vercel, Supabase, Netlify, Render und Hetzner nachgeprüft. Angaben, die nur aus Drittquellen stammen, sind in [hosting.md](hosting.md) gekennzeichnet.

**Was die Recherche verändert hat:**

- **Eigene frühere Aussage korrigiert:** Die AI hatte zuvor geschrieben, der eingebaute E-Mail-Versand von Supabase „genügt für die Demo“ von „Passwort zurücksetzen“. Laut offizieller Doku liefert er aber nur an Adressen von Team-Mitgliedern (2 pro Stunde). F10 braucht daher einen eigenen SMTP-Dienst – korrigiert in architecture.md (R7), requirements.md (#16) und mvp.md.
- **Neues Risiko gefunden:** Vercel bietet einen Vertrag zur Auftragsverarbeitung (DPA) nur ab dem Pro-Tarif. Für das Semester mit Testdaten akzeptiert, für einen Echtbetrieb dokumentiert.
- **Region bestätigt:** Vercel führt Funktionen standardmäßig in Washington aus; im Gratis-Tarif lässt sich eine Region (Frankfurt) wählen. Ohne diese Einstellung wären App und Datenbank auf zwei Kontinenten.
- **Alternativen begründet verworfen:** Render Free (ca. 1 Minute Kaltstart, Datenbank nach 30 Tagen gelöscht), Railway (kein Gratis-Tarif).

**Lehre:** Aussagen der AI zu Preisen und Limits sind ohne Quelle nicht verlässlich – selbst eine plausible Aussage („genügt für die Demo“) war falsch. Auch die Websuche allein reicht nicht: Drittquellen widersprechen sich, maßgeblich sind die offiziellen Seiten mit Abrufdatum.

## 5. Review durch mehrere AI-Prüfer („Council“)

**Tool:** Claude Code, drei parallel arbeitende Prüf-Agenten, die nur lesen durften

**Auftrag (sinngemäß):** „Geht das Projekt durch und prüft, ob alles für Phase 2 passt.“ Jeder Prüfer hatte einen anderen Blickwinkel und bekam die Dokumente, aber nicht die Einschätzung der AI, die sie geschrieben hatte:

1. **Prüfer-Sicht:** Abgleich mit Aufgabenstellung, Bewertungsschwerpunkten und Definition of Done
2. **Konsistenz:** IDs, Prioritäten, Formel, Datenmodell, API, Diagramme und ADRs dokumentübergreifend
3. **Technik:** SQL, Zugriffsregeln, Supabase/Next.js, API-Design, Sicherheit, Mermaid-Syntax

**Wichtigste Funde und was wir geändert haben:**

| Fund | Änderung |
|---|---|
| Die Aussage „nur E-Mail und Passwort-Hash gespeichert“ ist falsch – Supabase speichert auch IP-Adressen in Sitzungen und Protokollen, die beim Konto-Löschen nicht mitgelöscht werden | NF5 korrigiert, offener Punkt #17, Risiko R11 |
| Session-Cookies von Supabase sind standardmäßig **nicht** HTTP-only | Explizite Cookie-Konfiguration in api.md und ADR-002 |
| Kommazahlen beim Score machen den Gleichstand-Vergleich unzuverlässig | Interne Sortierung mit ganzen Zahlen (requirements.md Kapitel 7) |
| Demo-Schritt in mvp.md konnte die Beispiel-Reihenfolge nicht erzeugen | Demo-Ablauf korrigiert, Gleichstand-Regel wird jetzt sichtbar vorgeführt |
| `today`-Parameter funktioniert nicht mit vom Server gerenderten Seiten | Listenseite als Client Component festgelegt (ADR-003, api.md) |
| CSRF, Session-Erneuerung, Rate-Limiting, 404 unter RLS, PATCH-Details fehlten | In api.md ergänzt; Risiken R8–R10 |
| „Registrierung meldet vergebene E-Mail“ widerspricht „Reset verrät keine E-Mail“ | Als bewusster Kompromiss dokumentiert |
| Vergleichstabelle bevorzugte Variante A, REST-Begründung war zirkulär bzw. hypothetisch („native App“) | Tabelle neu bewertet, Gewichtung erklärt; REST mit Testbarkeit begründet |
| Vorschau-Deployments und Tests hätten die Produktivdatenbank genutzt | Zweites Supabase-Projekt für Entwicklung (hosting.md) |

**Team-Entscheidungen dazu:** Konto löschen nur nach erneuter Passworteingabe (#18); ADR-003 blieb zunächst „vorgeschlagen“ und wurde danach vom Team angenommen.

**Nicht übernommen:** Ein Prüfer meinte, „Vercel Hobby nur nicht-kommerziell“ stehe in Vercels Fair-Use-Richtlinien. Die Recherche hatte genau diese Seite ohne Treffer durchsucht. Da sich die Aussagen widersprechen, bleibt die Angabe in hosting.md als „nur aus Drittquellen“ gekennzeichnet.

# Reflexion Phase 2

> Von Claude Code auf Basis des Projektverlaufs und von Stichworten des Teams formuliert.

### Was gut funktioniert hat

- **Alternativen schnell sichtbar:** Die AI hat in kurzer Zeit drei ernsthafte Varianten mit Diagrammen und einer Bewertung nach unseren Anforderungen geliefert. Das war eine gute Grundlage für unsere Entscheidung.
- **Risiken früh erkannt:** Die AI hat das Zeitzonenproblem bei der Priorisierung (R1) schon im Entwurf aufgezeigt. Es hat zu einer eigenen Architekturentscheidung (ADR-003) geführt.
- **Recherche mit Quellen:** Auf die Nachfrage „Warum recherchieren, ist doch kostenlos?“ hat die AI begründet, warum Gratis-Tarife Bedingungen haben, die unsere Anforderungen betreffen. Die Recherche hat tatsächlich Wichtiges gefunden (Standard-Region USA, DPA erst ab Vercel Pro, E-Mail-Versand nur an Team-Adressen).
- **Mehrere unabhängige Prüfer:** Das Review mit drei AI-Prüfern hat echte technische Fehler gefunden, die in den Entwürfen plausibel aussahen.

### Grenzen und Probleme

- **Falsche Annahmen über uns:** Die AI hat angenommen, wir kennen Supabase nicht, und darauf eine Empfehlung gebaut. Erst unser Einwand hat zu einer besseren Lösung geführt.
- **Plausibel, aber falsch:** Mehrere Aussagen klangen überzeugend und waren falsch – „eingebauter E-Mail-Versand genügt für die Demo“, „HTTP-only-Cookies“, „nur E-Mail und Passwort-Hash gespeichert“. Ohne Recherche und Review wären sie in der Abgabe gelandet.
- **Widersprüche zwischen Dokumenten:** Die AI hat Variante C mit einem Argument (Manipulierbarkeit) verworfen, das sie an anderer Stelle selbst entkräftet hatte. Aufgefallen ist das erst, als wir zu Variante C nachgefragt haben und die AI ihre Begründung noch einmal geprüft hat.
- **Viel Text, viel Detail:** Die AI erzeugt schnell umfangreiche Dokumente. Es kostet Zeit, alles so weit zu verstehen, dass man es selbst erklären kann (z. B. RLS, CSRF, DPA).
- **AI prüft AI:** Auch das Council besteht aus AI-Prüfern. Auch die Bewertung ihrer Funde hat großteils die AI vorgenommen; wir haben die zwei offenen Punkte entschieden (Passwort beim Konto löschen, Status von ADR-003). In einem Punkt (Vercel nicht-kommerziell) widersprach ein Prüfer der Recherche.

### Was wir gelernt haben

- Kontext, den nur das Team hat (Vorerfahrung, Präferenzen), müssen wir **am Anfang** einbringen – nicht erst, wenn eine Empfehlung schon steht.
- „Warum?“ fragen lohnt sich: Unsere Nachfragen (Recherche, Variante C) haben jeweils zu besseren Begründungen geführt.
- Fakten zu Preisen, Limits und Vendor-Verhalten nur mit offizieller Quelle und Datum übernehmen.
- Review funktioniert am besten mit expliziten Kriterien (Aufgabenstellung, Definition of Done) und mehreren getrennten Blickwinkeln.
- Die Verantwortung bleibt bei uns: Jede Entscheidung in den ADRs müssen wir selbst erklären und verteidigen können.

### Unsere eigene Einschätzung

**Wo wir unsicher waren:** Beim Monolithen waren wir uns am Anfang nicht sicher. Ein getrenntes Backend (Variante B) trennt die Zuständigkeiten klarer, bedeutet für uns aber zwei Anwendungen, zwei Deployments und mehr Abstimmung. Für eine App mit einer Tabelle und einem kleinen Team wäre das unnötig kompliziert. Wir sind deshalb zum Schluss gekommen, dass der Monolith für dieses Projekt am besten passt. Die Aufteilung in Module war uns dabei wichtig, damit der Code trotzdem übersichtlich bleibt und die Priorisierung für sich getestet werden kann.

**Wo wir uns sicher waren:** Der Tech-Stack war für uns ein klarer Sieger. Next.js bringt Oberfläche und API in einem Projekt mit, Vercel übernimmt das Deployment, und wir haben mit beiden schon gearbeitet. So können wir uns auf die eigentliche Funktion konzentrieren, statt uns zuerst in neue Werkzeuge einzuarbeiten. Uns ist bewusst, dass diese Vorliebe unsere Entscheidung beeinflusst hat. Deshalb war der Vergleich mit anderen Varianten und Hosting-Anbietern sinnvoll, auch wenn er unsere Wahl am Ende bestätigt hat.

**Was wir gut erklären können:** Den Aufbau der App (Browser, Next.js-App, Supabase), die Priorisierungsregel und die Gründe für unsere Entscheidungen. Bei den Sicherheitsthemen kennen wir die Idee dahinter: RLS sorgt dafür, dass die Datenbank jedem Nutzer nur seine eigenen Aufgaben zeigt, und der CSRF-Schutz verhindert, dass fremde Websites im Namen eines angemeldeten Nutzers Anfragen auslösen. Die technischen Details wollen wir uns bei der Umsetzung genauer ansehen.

**Was wir in Phase 3 anders machen:** Wir wollen der AI gezielt verschiedene Rollen geben, zum Beispiel Entwickler, Tester und Security-Reviewer. Das Council hat gezeigt, dass getrennte Blickwinkel Fehler finden, die in einem einzelnen Gespräch nicht auffallen. Davon erwarten wir uns genauere Antworten und bessere Qualität. Außerdem wollen wir unseren Kontext gleich am Anfang mitgeben und Code erst übernehmen, wenn wir ihn selbst verstehen.
