# ADR-004: Hosting auf Vercel Hobby und Supabase Free in Frankfurt

- **Status:** angenommen
- **Datum:** 23.11.2026

## Kontext

Das Hosting muss Next.js mit Server-Funktionen ausführen, Daten in der EU speichern (NF5), die Liste in unter 2 Sekunden laden (NF2) und während des Semesters kostenlos sein. Das Team kennt Vercel und wünscht automatisches Deployment aus GitHub. Vergleich der Optionen: [hosting.md](../hosting.md).

## Entscheidung

- **App:** Vercel Hobby, Funktions-Region Frankfurt (`fra1`, in `vercel.json` gesetzt).
- **Datenbank und Auth:** Supabase Free, Region Frankfurt (`eu-central-1`).
- Deployment automatisch bei Push auf `main`, Vorschau-Deployments für Pull Requests.
- Ein Supabase-Projekt für das Semester (nur Testdaten). Optional ein zweites kostenloses für Entwicklung und Vorschau, spätestens vor echten Nutzern ([hosting.md](../hosting.md#5-umgebungen)).

## Betrachtete Alternativen

- **Render Free:** verworfen – Aufwachen nach Schlafmodus dauert ca. 1 Minute (verletzt NF2); Gratis-Datenbank wird nach 30 Tagen gelöscht.
- **Railway:** verworfen – kein dauerhaft kostenloser Tarif.
- **Netlify Free:** möglich, aber EU-Region nicht belegt und keine Team-Erfahrung.
- **Eigener Server bei Hetzner:** ab 6,49 €/Monat zzgl. USt.; verworfen wegen Kosten und Betriebsaufwand (Updates, TLS, Deployment selbst einrichten).

## Konsequenzen

- Kosten im Semester: 0 €. Die Gratis-Kontingente übersteigen den erwarteten Bedarf um Größenordnungen.
- App und Datenbank laufen in derselben Region – kurze Wege (NF2), Daten in der EU (NF5).
- Supabase Free pausiert nach 1 Woche Inaktivität und hat keine Backups: Projekt vor der Abnahme prüfen, Datenbank-Export anlegen.
- Vercel bietet einen DPA nur ab Pro. Für das Semester mit Testdaten akzeptiert; für einen Echtbetrieb wären Vercel Pro (20 $ pro Platz/Monat) und Supabase Pro (ab 25 $/Monat) nötig.
