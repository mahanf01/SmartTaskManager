# ADR-0005: Erinnerungen per täglichem Cron-Job und E-Mail

- **Status:** Vorgeschlagen
- **Datum:** 2026-11-09
- **Betroffene Anforderungen:** F6, US8 (Should)

## Kontext

Nutzer sollen standardmäßig **1 Tag vor der Frist** erinnert werden (US8). Dafür muss etwas zeitgesteuert laufen, auch wenn der Nutzer die App nicht offen hat. In einer Serverless-Architektur (ADR-0001) gibt es keinen dauerhaft laufenden Server für einen Scheduler.

Fristen sind reine Daten ohne Uhrzeit. Eine minutengenaue Erinnerung ist daher nicht nötig.

## Betrachtete Optionen

1. **Vercel Cron** ruft einmal täglich einen geschützten API-Endpunkt auf; dieser verschickt E-Mails
2. **GitHub Actions (schedule)** ruft den Endpunkt auf
3. **Nachrichten-Warteschlange mit Zeitplan** (z. B. Upstash QStash): pro Aufgabe eine geplante Nachricht
4. **Nur Hinweis in der App** (z. B. „Morgen fällig“), keine E-Mail

## Entscheidung

Wir nutzen **Option 1: Vercel Cron, einmal täglich (z. B. 07:00 Uhr Wiener Zeit)**, plus **E-Mail über Resend**.

- Endpunkt `GET /api/cron/reminders`, geschützt mit `CRON_SECRET`
- Sucht offene Aufgaben mit `due_date = morgen` (je Nutzer-Zeitzone), `reminder_sent_at IS NULL` und `reminders_enabled = true`
- Pro Nutzer **eine** E-Mail mit allen betroffenen Aufgaben
- Danach `reminder_sent_at` setzen, damit nichts doppelt verschickt wird
- Ändert der Nutzer die Frist, wird `reminder_sent_at` zurückgesetzt
- Zusätzlich zeigt die App fällige und überfällige Aufgaben farblich an (offener Punkt #9)

## Konsequenzen

**Positiv**
- Kein eigener Server und keine Warteschlange nötig.
- Der Endpunkt ist ein normaler HTTP-Aufruf; er kann auch von GitHub Actions oder einem anderen Cron-Dienst aufgerufen werden (geringe Anbieterbindung).
- Idempotent durch `reminder_sent_at`: mehrfaches Aufrufen schadet nicht.

**Negativ / Risiken**
- Auf dem Gratis-Plan läuft Vercel Cron nur einmal pro Tag und nicht minutengenau. Für „1 Tag vorher“ reicht das; genaue Uhrzeiten („2 Stunden vorher“) wären damit nicht möglich → dann Option 3.
- Ein einzelner Lauf muss alle Nutzer in der maximalen Laufzeit einer Funktion abarbeiten. Für das MVP unkritisch; bei vielen Nutzern in Teilen verarbeiten.
- Bei Nutzern in sehr unterschiedlichen Zeitzonen kommt die E-Mail zu unterschiedlichen Ortszeiten an.
- Mit E-Mails kommt ein weiterer Anbieter mit E-Mail-Adressen dazu (DSGVO: Auftragsverarbeitungsvertrag).

## AI-Einsatz

Die AI hat alle vier Optionen genannt, darunter eine Warteschlange pro Aufgabe (Option 3). Diese ist technisch am genauesten, für das MVP aber zu aufwendig, weil jede Änderung der Frist die geplante Nachricht neu planen müsste. Die einfachere Lösung wurde gewählt, weil Fristen ohnehin nur ein Datum ohne Uhrzeit sind.
