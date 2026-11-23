# Hosting – SmartTaskManager

> Stand: 23.11.2026. Alle Preise und Limits wurden an diesem Tag recherchiert (Quellen am Ende). Preise ändern sich häufig – vor einer Entscheidung für einen kostenpflichtigen Tarif erneut prüfen.

## 1. Anforderungen an das Hosting

| Kriterium | Herkunft |
|---|---|
| Next.js mit Server-Funktionen (Route Handlers) muss laufen | Architektur Variante A ([architecture.md](architecture.md)) |
| Datenbank in der EU | NF5 |
| Server in derselben Region wie die Datenbank, keine langen Kaltstarts | NF2 (Liste lädt in unter 2 Sekunden) |
| Betrieb während des Semesters kostenlos | Rahmenbedingung |
| Automatisches Deployment aus GitHub | Team-Wunsch |
| Vertrag zur Auftragsverarbeitung (DPA/AVV) verfügbar | NF5 (für späteren Echtbetrieb) |

## 2. App-Hosting im Vergleich

| | Vercel Hobby | Netlify Free | Render Free | Railway | Hetzner Cloud (eigener Server) |
|---|---|---|---|---|---|
| **Preis** | 0 $ | 0 $ (300 Credits/Monat) | 0 $ | kein dauerhafter Gratis-Tarif, nur Testguthaben ¹ | ab 6,49 €/Monat zzgl. USt. (CPX02, 1 vCPU, 1 GB RAM) |
| **Nächster Tarif** | Pro: 20 $ pro Entwickler-Platz/Monat | Personal: 9 $/Monat | Starter: ca. 7 $/Monat ¹ | Hobby: 5 $/Monat ¹ | – |
| **Next.js-Server** | nativ (Hersteller von Next.js) | ja ² | ja (Node-Server) | ja (Node-Server) | ja, selbst einrichten |
| **EU-Region** | ja, eine Region wählbar, z. B. Frankfurt (`fra1`); Standard ist Washington (`iad1`) | auf der Preisseite nicht angegeben | Frankfurt verfügbar | nicht recherchiert | Falkenstein, Nürnberg |
| **Kaltstart / Schlafmodus** | Serverless, kurze Kaltstarts | Serverless | schläft nach 15 min ohne Anfragen, Aufwachen dauert ca. 1 Minute | – | läuft dauerhaft |
| **Deployment aus GitHub** | automatisch, mit Vorschau-Deployments | automatisch | automatisch | automatisch | selbst einrichten (z. B. GitHub Actions) |
| **Betriebsaufwand** | sehr gering | gering | gering | gering | hoch: Betriebssystem-Updates, TLS, Firewall, Überwachung |
| **DPA** | nur für Pro und Enterprise | vorhanden, Teil der Nutzungsbedingungen; ob für den Gratis-Tarif, nicht eindeutig ¹ | nicht geprüft (scheidet aus anderen Gründen aus) | nicht geprüft (scheidet aus anderen Gründen aus) | Auftragsverarbeitungsvertrag (AVV) im Kundenkonto abschließbar, nicht automatisch; Daten bei Standort Deutschland in der EU |
| **Bei Überschreiten der Limits** | Funktion pausiert bis zu 30 Tage, keine Kosten | Projekt pausiert ¹ | Dienste ruhen bis Monatsende | – | – |

¹ Nur aus Drittquellen, auf der offiziellen Seite nicht gefunden.
² Nicht auf der Preisseite angegeben; Netlify unterstützt Next.js über einen eigenen Adapter.

**Bewertung:**

- **Render Free** scheidet aus: Ein Kaltstart von etwa einer Minute verletzt NF2 deutlich.
- **Railway** scheidet aus: kein dauerhaft kostenloser Tarif.
- **Hetzner** ist ab etwa 6,50 € im Monat die günstigste kostenpflichtige Option mit Serverstandort in Deutschland. Server, Updates und Deployment müssten wir aber selbst betreiben – Zeit, die im Semester für die eigentliche App fehlt.
- **Netlify Free** wäre möglich, die EU-Region ist aber auf der Preisseite nicht belegt und das Team hat keine Erfahrung damit.
- **Vercel Hobby** erfüllt alle Kriterien für das Semester: kostenlos, Next.js nativ, Frankfurt als Region wählbar, kein Betriebsaufwand, Team-Erfahrung.

## 3. Datenbank und Auth

Supabase wurde bereits als Dienst für Datenbank und Auth gewählt ([ADR-002](adr/002-supabase-fuer-datenbank-und-auth.md)). Zu klären ist hier der Tarif.

| | Supabase Free | Supabase Pro |
|---|---|---|
| **Preis** | 0 $ | ab 25 $/Monat (enthält 10 $ Rechenguthaben für eine Micro-Instanz) |
| **Aktive Projekte** | 2 | je Projekt zusätzliche Rechenkosten |
| **Datenbankgröße** | 500 MB | 8 GB pro Projekt |
| **Nutzer (Auth)** | 50.000 aktive Nutzer pro Monat | 100.000 |
| **Datenübertragung** | 5 GB (+ 5 GB aus dem Cache) | 250 GB |
| **Backups** | keine | täglich, 7 Tage aufbewahrt |
| **Pausieren** | nach 1 Woche Inaktivität | nie |
| **Region** | Frankfurt (`eu-central-1`) verfügbar; Region ist später nicht änderbar | wie Free |
| **E-Mail-Versand (eingebaut)** | 2 E-Mails pro Stunde, **nur an Adressen von Team-Mitgliedern**, nicht für den Produktivbetrieb gedacht | wie Free; eigener SMTP-Dienst empfohlen |
| **DPA** | vorhanden, Tarif nicht genannt; Vertragspartner Supabase Pte. Ltd. (Singapur) mit EU-Standardvertragsklauseln | wie Free |

Alternative mit Ablaufdatum: Render Postgres Free wird nach 30 Tagen gelöscht – für ein Semester ungeeignet.

Reine Datenbank-Anbieter (z. B. Neon) wurden nicht im Detail verglichen: Sie bringen kein Login mit, wir bräuchten also zusätzlich eine Auth-Lösung – genau das wollte ADR-002 vermeiden.

## 4. Entscheidung

**Vercel Hobby (Region Frankfurt `fra1`) + Supabase Free (Region Frankfurt `eu-central-1`). Kosten im Semester: 0 €.**

Server und Datenbank liegen in derselben Region (Frankfurt). Das hält den Weg zwischen App und Datenbank kurz (NF2) und die gespeicherten Daten in der EU (NF5).

### Reicht das Gratis-Kontingent?

| Limit | Kontingent | Erwarteter Bedarf im Semester |
|---|---|---|
| Datenbank | 500 MB | Eine Aufgabe belegt grob unter 1 KB (inkl. Index). Selbst 100.000 Aufgaben brauchen weniger als 100 MB. |
| Nutzer | 50.000 pro Monat | Team und Testpersonen: unter 50 |
| Funktionsaufrufe (Vercel) | 1 Mio. pro Monat | Wenige tausend |
| Datenübertragung (Vercel) | 100 GB pro Monat | Deutlich unter 1 GB |

Die Limits sind für Entwicklung und Demo um Größenordnungen ausreichend.

## 5. Umgebungen

Für Semester und Demo reicht **ein** Supabase-Projekt (Frankfurt). Es wird von der Produktion (Deployment von `main`), den Vorschau-Deployments und der lokalen Entwicklung gemeinsam genutzt und enthält nur Testdaten. Das Schema liegt als Migration im Repository.

**Optional:** Supabase Free erlaubt zwei aktive Projekte. Ein zweites Projekt für Entwicklung und Vorschau wird sinnvoll, wenn E2E-Tests Konten anlegen und löschen, und ist spätestens vor echten Nutzern nötig. Die Trennung erfolgt dann über Umgebungsvariablen, die in Vercel getrennt für „Production“ und „Preview“ gesetzt werden.

## 6. Einschränkungen und Gegenmaßnahmen

| Einschränkung | Gegenmaßnahme |
|---|---|
| Vercel führt Funktionen standardmäßig in Washington (`iad1`) aus | Region in `vercel.json` auf `["fra1"]` setzen |
| Supabase Free pausiert nach 1 Woche Inaktivität | Während des Semesters wird regelmäßig entwickelt und getestet. Einen Tag vor der Abnahme prüfen, ob das Supabase-Projekt aktiv ist. |
| Supabase Free hat keine Backups | Vor der Abnahme und nach Schema-Änderungen einen Export mit `supabase db dump` erstellen. Das Schema liegt ohnehin als Migration im Repository. |
| Eingebauter E-Mail-Versand nur an Team-Adressen, 2 pro Stunde | MVP braucht keine E-Mails (Bestätigung ist aus). Für F10 (Passwort zurücksetzen) ist ein eigener SMTP-Dienst nötig – Auswahl und Kosten werden bei der Umsetzung von F10 geprüft. |
| Vercel bietet den DPA nur für Pro und Enterprise | Im Semester werden nur Testdaten von Team und Testpersonen verarbeitet. Für einen echten Betrieb mit fremden Nutzerdaten wäre Vercel Pro (oder ein anderer Hoster mit DPA) nötig. Vercel ist unter dem EU-U.S. Data Privacy Framework zertifiziert (alle Tarife). |
| Vercel Hobby ist laut Drittquellen nur für private, nicht-kommerzielle Nutzung erlaubt (Verweis auf die Nutzungsbedingungen; auf den gelesenen offiziellen Seiten nicht gefunden) | Für ein Semesterprojekt unkritisch. |

## 7. Kosten bei Echtbetrieb

Falls die App nach dem Semester mit echten Nutzern betrieben würde:

| Posten | Kosten pro Monat |
|---|---|
| Vercel Pro (1 Entwickler-Platz, enthält DPA) | 20 $ |
| Supabase Pro (keine Pausen, tägliche Backups) | ab 25 $ |
| Eigener SMTP-Dienst für Auth-E-Mails | abhängig vom Anbieter, nicht recherchiert |
| **Summe** | **ab ca. 45 $** |

Günstigere Alternative für die App: ein eigener Server bei Hetzner (ab 6,49 € zzgl. USt.) – dafür mit dem oben beschriebenen Betriebsaufwand.

## Quellen

Alle abgerufen am 23.11.2026.

**Offizielle Seiten:**

- Vercel: [Preise](https://vercel.com/pricing), [Hobby-Tarif](https://vercel.com/docs/plans/hobby), [Funktions-Regionen](https://vercel.com/docs/functions/configuring-functions/region), [Regionen-Liste](https://vercel.com/docs/regions), [Cron Jobs](https://vercel.com/docs/cron-jobs/usage-and-pricing), [Fair-Use-Richtlinien](https://vercel.com/docs/limits/fair-use-guidelines), [DPA](https://vercel.com/legal/dpa)
- Supabase: [Preise](https://supabase.com/pricing), [Regionen](https://supabase.com/docs/guides/platform/regions), [SMTP](https://supabase.com/docs/guides/auth/auth-smtp), [Rate Limits](https://supabase.com/docs/guides/auth/rate-limits), [DPA](https://supabase.com/legal/dpa)
- Netlify: [Preise](https://www.netlify.com/pricing/)
- Render: [Preise](https://render.com/pricing), [Gratis-Tarif](https://render.com/docs/free)
- Hetzner: [Cloud](https://www.hetzner.com/cloud/), [Datenschutz bei Hetzner (AVV)](https://docs.hetzner.com/de/general/company-and-policy/data-protection-at-hetzner/)

**Drittquellen** (nur für Angaben mit ¹):

- [Vercel Hobby: nicht-kommerzielle Nutzung (convika.com)](https://convika.com/compare/vercel-hobby-commercial-use/)
- [Railway vs Render 2026 (dev.to)](https://dev.to/selfhost/railway-vs-render-pricing-reliability-the-2026-verdict-2fpp)
- [Netlify Preisübersicht 2026 (toolchase.com)](https://toolchase.com/blog/netlify-pricing-guide/)
- Netlify DPA: [GDPR-Seite](https://www.netlify.com/gdpr-ccpa/), [DPA (PDF)](https://www.netlify.com/pdf/netlify-dpa.pdf) – offizielle Seiten, aber ohne eindeutige Aussage zum Gratis-Tarif
