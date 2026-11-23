# Hosting-Vergleich – SmartTaskManager

Dieses Dokument vergleicht Hosting-Möglichkeiten für die gewählte Architektur (Next.js + PostgreSQL + täglicher Job, siehe [architecture.md](architecture.md)).

> **Stand der Preise: 9. November 2026.** Preise in USD bzw. EUR, ohne Mehrwertsteuer. Anbieter ändern Preise und Gratis-Limits oft – vor einer Entscheidung auf den Anbieterseiten nachprüfen (Quellen am Ende).

## 1. Was wir brauchen

| Bedarf | Warum |
|---|---|
| Next.js-App (UI + API) | Kern der Anwendung |
| PostgreSQL-Datenbank | Nutzer und Aufgaben ([ADR-0002](adr/0002-postgresql-mit-prisma.md)) |
| Täglicher Job | Erinnerungen ([ADR-0005](adr/0005-erinnerungen-per-cron.md)) |
| Deployment aus GitHub | Phase 2: Continuous Deployment |
| EU-Region | DSGVO (NF5), kurze Wege für Nutzer in Österreich (NF2) |
| Kein Einschlafen / Löschen bei Inaktivität | Uni-Projekte werden oft wochenlang nicht genutzt |

## 2. Vergleichskriterien

- **Kosten** – im Uni-Projekt (wenige Nutzer) und als kleines echtes Produkt (ca. 1.000 Nutzer)
- **Aufwand** – Einrichtung und laufender Betrieb (Updates, Backups)
- **CD aus GitHub** – automatisches Deployment bei jedem Push
- **DSGVO** – EU-Region, Firmensitz, Auftragsverarbeitungsvertrag (DPA)
- **Einschränkungen der Gratis-Stufe** – Schlafen, Löschen, kommerzielle Nutzung
- **Anbieterbindung**

## 3. Optionen im Überblick

### Option 1 – Vercel + Neon (empfohlen für das Projekt)

App und Cron auf **Vercel**, Datenbank bei **Neon** (Region Frankfurt).

| | Gratis | Bezahlt |
|---|---|---|
| **Vercel** | Hobby: 0 $. U. a. 1 Mio. Funktionsaufrufe, 4 CPU-Stunden, 100 GB Datenübertragung pro Monat. **Nur nicht-kommerzielle Nutzung.** | Pro: 20 $ pro Entwickler-Platz und Monat, plus Nutzung über dem enthaltenen Guthaben |
| **Neon** | Free: 0 $. 100 CU-Stunden Rechenzeit pro Projekt/Monat, 1 GB Speicher pro Projekt | Launch: nutzungsbasiert ohne Grundgebühr (0,106 $/CU-Stunde, 0,35 $/GB-Monat); kleine DB laut Anbieter ca. 15 $/Monat |

- ➕ Kein Server zu betreuen, CD aus GitHub ist eingebaut, Preview-Deployments pro Pull Request
- ➕ Neon schläft bei Inaktivität, löscht aber die Datenbank nicht
- ➖ Vercel Hobby verbietet kommerzielle Nutzung → für ein echtes Produkt Pro nötig
- ➖ Beide Anbieter sind US-Firmen (EU-Region wählbar, aber DPA nötig)
- ➖ Kaltstart der Gratis-DB nach Inaktivität (erste Anfrage langsamer)

### Option 2 – Vercel + Supabase

App auf Vercel, PostgreSQL bei **Supabase** (Region Frankfurt).

| | Gratis | Bezahlt |
|---|---|---|
| **Supabase** | Free: 0 $. 500 MB Datenbank, 50.000 aktive Nutzer, max. 2 aktive Projekte. **Projekt wird nach 1 Woche Inaktivität pausiert.** | Pro: ab 25 $/Monat |

- ➕ Zusätzlich Auth, Speicher und Dashboard dabei (bräuchten wir aber laut [ADR-0004](adr/0004-auth-mit-authjs.md) nicht)
- ➖ Pausieren nach 1 Woche ist für ein Uni-Projekt störend (z. B. vor der Präsentation)
- ➖ Pro-Plan teurer als Neon für unseren kleinen Bedarf

### Option 3 – Render (alles bei einem Anbieter)

Next.js als Web-Service, PostgreSQL und Cron-Job bei **Render** (Region Frankfurt verfügbar).

| | Gratis | Bezahlt |
|---|---|---|
| **Web-Service** | 0 $, schläft nach ca. 15 Min. ohne Anfragen ein (langer Kaltstart) | ab 7 $/Monat (512 MB RAM) |
| **PostgreSQL** | 0 $ (256 MB RAM), Gratis-Datenbanken laufen nach begrenzter Zeit ab | ab 6 $/Monat |

- ➕ Alles in einem Dashboard, Docker möglich, CD aus GitHub
- ➖ Gratis-Stufe für eine Live-Demo kaum brauchbar (Einschlafen, DB läuft ab)
- ➖ Realistisch ca. 13 $/Monat + Cron-Job

### Option 4 – Railway

Next.js, PostgreSQL und Cron als Dienste bei **Railway**.

| | Kosten |
|---|---|
| **Hobby** | 5 $/Monat, darin 5 $ Nutzung enthalten; darüber wird zusätzlich abgerechnet |
| **Test** | Einmalig 5 $ Guthaben, danach Free-Plan mit 1 $ Guthaben pro Monat |

- ➕ Sehr einfache Einrichtung, alles an einem Ort, CD aus GitHub
- ➖ Keine dauerhafte Gratis-Stufe für eine App mit Datenbank
- ➖ Kosten schwanken mit der Nutzung

### Option 5 – Eigener Server in der EU (Hetzner + Docker)

Ein kleiner virtueller Server bei **Hetzner** (Deutschland/Finnland). Next.js und PostgreSQL laufen als Docker-Container, z. B. verwaltet mit Coolify. Cron über den Server.

| | Kosten |
|---|---|
| **Kleinster Cloud-Server** (z. B. CX22: 2 vCPU, 4 GB RAM) | ca. 4–6 € pro Monat (Preise wurden 2026 angepasst) |
| **Backups** | ca. +20 % des Serverpreises |

- ➕ Am günstigsten für ein echtes Produkt, feste Kosten
- ➕ Rein europäischer Anbieter, Daten nur in der EU → einfachste DSGVO-Lage
- ➕ Keine Anbieterbindung, kein Einschlafen
- ➖ **Viel Eigenaufwand:** Updates, Sicherheit, Backups, SSL, Monitoring selbst erledigen
- ➖ CD muss selbst eingerichtet werden (GitHub Actions oder Coolify)
- ➖ Ein einzelner Server = keine Ausfallsicherheit

### Option 6 – Große Cloud-Anbieter (AWS, Azure, Google Cloud)

- ➕ Alles möglich, sehr gut skalierbar, EU-Regionen
- ➕ Teilweise Guthaben für Studierende
- ➖ Für ein kleines MVP viel zu komplex (Netzwerk, Rechte, Kostenkontrolle)
- ➖ Kosten schwer vorherzusagen; Gefahr unerwarteter Rechnungen
- → Für dieses Projekt **nicht** empfohlen

## 4. Vergleichstabelle

| Kriterium | 1 Vercel + Neon | 2 Vercel + Supabase | 3 Render | 4 Railway | 5 Hetzner (eigener Server) | 6 AWS/Azure/GCP |
|---|---|---|---|---|---|---|
| Kosten Uni-Projekt | **0 $** | 0 $ | 0 $ (mit Einschränkungen) | ca. 5 $ | ca. 5 € | unklar |
| Kosten echtes Produkt (~1.000 Nutzer) | ca. 25–40 $ | ca. 45 $ | ca. 13–20 $ | ca. 5–15 $ | **ca. 5–10 €** | 20–60 $+ |
| Einrichtung | sehr einfach | sehr einfach | einfach | sehr einfach | aufwendig | aufwendig |
| Laufender Betrieb | kaum | kaum | wenig | wenig | **hoch** | mittel–hoch |
| CD aus GitHub | ✅ eingebaut, Preview pro PR | ✅ | ✅ | ✅ | selbst einrichten | selbst einrichten |
| EU-Region | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| EU-Firma (DSGVO einfacher) | ❌ (US) | ❌ (US) | ❌ (US) | ❌ (US) | ✅ | ❌ (US) |
| Gratis-Stufe schläft / wird gelöscht | schläft (DB), wird nicht gelöscht | pausiert nach 1 Woche | schläft, DB läuft ab | – | – | – |
| Kommerziell auf Gratis-Stufe | ❌ (Vercel Hobby) | ❌ | ✅ | – | – | – |
| Anbieterbindung | mittel | mittel | gering | gering | **keine** | hoch |
| Passt zu Phase 2 („→ Vercel“) | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |

Die Werte für „echtes Produkt“ sind grobe Schätzungen für eine kleine App mit einem Entwickler-Platz. Hinzu kommen ggf. E-Mail-Versand (Resend o. ä., für kleine Mengen gratis) und eine eigene Domain (ca. 10–20 € pro Jahr).

## 5. Empfehlung

### Für das Uni-Projekt: Option 1 – Vercel Hobby + Neon Free (Frankfurt)

- **Kosten: 0 €**
- passt direkt zu Phase 2 (GitHub → CD → Vercel)
- Preview-Deployment für jeden Pull Request – praktisch für Code-Reviews im Team
- Datenbank wird bei Inaktivität nicht gelöscht
- Vercel-Funktionen und Neon-DB beide in Frankfurt → kurze Wege (NF2)

### Wenn daraus ein echtes Produkt wird

Zwei sinnvolle Wege:

1. **Bequem:** Vercel Pro + Neon Launch – ca. 25–40 $/Monat, kein Betriebsaufwand. Auftragsverarbeitungsverträge mit allen Anbietern abschließen.
2. **Günstig und DSGVO-freundlich:** Umzug auf einen eigenen Server bei Hetzner – ca. 5–10 €/Monat, aber Betrieb selbst übernehmen.

Der Umzug ist möglich, weil die Architektur auf Standards setzt (Next.js als Docker-Container, PostgreSQL, Cron als normaler HTTP-Aufruf). Siehe [architecture.md, Kapitel 10, Punkt 8](architecture.md#10-kritische-prüfung-der-ai-generierten-architektur).

## 6. DSGVO-Hinweise zum Hosting

- Region **Frankfurt (EU)** bei allen Diensten wählen.
- Bei US-Anbietern: **Auftragsverarbeitungsvertrag (DPA)** abschließen und prüfen, ob der Anbieter unter dem EU-US Data Privacy Framework zertifiziert ist.
- Alle eingesetzten Dienste (Hosting, Datenbank, E-Mail) in der Datenschutzerklärung nennen.
- Keine personenbezogenen Daten in Logs schreiben.
- Backups und Löschfristen festlegen; beim Löschen des Kontos werden alle Aufgaben mitgelöscht.

## 7. AI-Einsatz bei diesem Dokument

Die AI hat die Optionen und Kriterien vorgeschlagen. Preise und Limits aus dem Wissen der AI können veraltet sein. Deshalb wurden sie per Websuche auf den Anbieterseiten nachgeprüft (Stand 9. November 2026). Dabei zeigte sich z. B., dass Gratis-Stufen sehr unterschiedliche Haken haben (Pausieren nach 1 Woche, Ablauf der Datenbank, Verbot kommerzieller Nutzung) – diese Punkte waren für die Empfehlung wichtiger als der reine Preis.

## Quellen

- [Vercel – Hobby Plan (Doku)](https://vercel.com/docs/plans/hobby)
- [Neon – Pricing](https://neon.com/pricing)
- [Supabase – Pricing](https://supabase.com/pricing)
- [Render – Pricing](https://render.com/pricing)
- [Railway Pricing 2026 (temps.sh)](https://temps.sh/blog/railway-pricing-2026)
- [Hetzner Cloud Servers Pricing (cloudpricecheck)](https://cloudpricecheck-com.pages.dev/hetzner/cloud-servers-pricing)
- [Hetzner: neue Cloud-Server-Pläne (DataCentreNews UK)](https://datacentrenews.uk/story/hetzner-unveils-new-cloud-server-plans-from-eur-3-79-per-month)
