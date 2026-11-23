# ADR-002: Supabase für Datenbank und Auth

- **Status:** angenommen
- **Datum:** 23.11.2026

## Kontext

Die App braucht eine relationale Datenbank (Nutzer 1 : n Aufgaben) und ein sicheres Login mit E-Mail und Passwort (F1, NF3). Daten müssen in der EU liegen, Konten vollständig löschbar sein (F9, NF5). Auf einer serverlosen Plattform gibt es kein dauerhaftes Dateisystem, eine dateibasierte Datenbank wie SQLite fällt aus. Sicherheitskritischen Code wollen wir nicht selbst schreiben. Im Team gibt es Erfahrung mit Supabase aus mehreren Projekten.

## Entscheidung

Supabase (Region Frankfurt, `eu-central-1`) stellt PostgreSQL und Auth bereit.

- **Auth:** Supabase Auth mit E-Mail/Passwort; Sessions als Cookies über `@supabase/ssr`, explizit als HTTP-only konfiguriert (Standard wäre für JavaScript lesbar). Eine Middleware erneuert abgelaufene Token; der Nutzer wird mit `getUser()` ermittelt. E-Mail-Bestätigung ist im MVP aus; Mindestlänge des Passworts in Supabase auf 8 gesetzt.
- **Datenzugriff:** ausschließlich serverseitig über `supabase-js`, mit der Session des Nutzers. Row Level Security (`user_id = auth.uid()`) ist als zweite Schutzschicht aktiv.
- **Admin-Zugriff:** Der Service-Role-Key wird nur im Modul `account` zum Löschen von Konten verwendet, über einen eigenen Admin-Client ohne Cookies, und nur als Server-Umgebungsvariable gespeichert.
- **Schema:** SQL-Migrationen mit der Supabase CLI, versioniert im Repository.

## Betrachtete Alternativen

- **Eigene Auth-Bibliothek (Auth.js, Better Auth) + Datenbank + ORM (Prisma/Drizzle):** ursprüngliche AI-Empfehlung; verworfen – mehr eigener Code, zwei zusätzliche Technologieentscheidungen, keine Team-Erfahrung. Prisma/Drizzle würden sich als Datenbank-Besitzer verbinden und RLS umgehen.
- **Externer Auth-Dienst (z. B. Clerk):** verworfen – Nutzerdaten bei einem weiteren Drittanbieter (NF5).
- **SQLite/libSQL, MongoDB:** verworfen – SQLite ohne dauerhaftes Dateisystem nicht nutzbar; die Daten sind relational.

## Konsequenzen

- Passwort-Hashing (NF3), Sessions und „Passwort zurücksetzen“ (F10) bringt Supabase mit.
- `ON DELETE CASCADE` auf `auth.users` löscht beim Konto-Löschen alle Aufgaben automatisch (F9).
- `supabase-js` greift über HTTP zu – keine Probleme mit Datenbankverbindungen in serverloser Umgebung.
- Abhängigkeit von Supabase. Die Daten liegen in normalem PostgreSQL und sind exportierbar; der Supabase-Zugriff ist auf drei Module beschränkt.
- Der eingebaute E-Mail-Versand ist nur für Team-Adressen nutzbar (2 pro Stunde). Für F10 ist ein eigener SMTP-Dienst nötig.
- Der Service-Role-Key ist ein kritisches Geheimnis (Risiko R6 in architecture.md).
- Supabase Auth speichert neben E-Mail und Passwort-Hash auch Sitzungs- und Protokolldaten (u. a. IP-Adresse); die Protokolle werden beim Konto-Löschen nicht mitgelöscht (requirements.md #17, Risiko R11).
- Login-Anfragen kommen alle vom eigenen Server; die IP-basierten Limits von Supabase schützen deshalb nicht pro Angreifer (Risiko R8).
