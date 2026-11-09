# ADR-0004: Authentifizierung mit Auth.js (E-Mail + Passwort)

- **Status:** Vorgeschlagen
- **Datum:** 2026-11-09
- **Betroffene Anforderungen:** F1, US1, NF3, NF5

## Kontext

Nutzer registrieren sich mit E-Mail und Passwort (US1). Passwörter müssen gehasht gespeichert werden (NF3). Daten sollen DSGVO-konform und sparsam gespeichert werden (NF5). Login ist sicherheitskritisch; Fehler sind hier besonders teuer.

## Betrachtete Optionen

1. **Auth.js (NextAuth) mit Credentials-Provider**, Nutzer in unserer eigenen Datenbank
2. **Externer Auth-Dienst** (z. B. Clerk, Auth0, Supabase Auth)
3. **Komplett selbst gebaut** (eigene Sessions, eigene Cookies)

## Entscheidung

Wir nutzen **Auth.js mit E-Mail + Passwort**.

- Passwort-Hash mit **argon2id** (Fallback: bcrypt mit Kostenfaktor ≥ 12)
- Mindestlänge Passwort: 10 Zeichen
- Session als verschlüsseltes Cookie (`HttpOnly`, `Secure`, `SameSite=Lax`)
- **Rate Limiting** auf Login und Registrierung (z. B. 5 Versuche pro Minute und IP/E-Mail)
- Gleiche Fehlermeldung bei falscher E-Mail und falschem Passwort (verrät nicht, ob ein Konto existiert)

## Konsequenzen

**Positiv**
- Bewährte Bibliothek statt Eigenbau; Cookie- und CSRF-Schutz sind bereits gelöst.
- Nutzerdaten bleiben in unserer Datenbank, kein weiterer Anbieter mit personenbezogenen Daten (NF5).
- Kostenlos, keine Nutzer-Limits.
- Später leicht um „Login mit Google“ oder Magic Links erweiterbar.

**Negativ / Risiken**
- Passwort-Zurücksetzen und E-Mail-Bestätigung müssen wir selbst bauen (nicht im MVP).
- Wir sind selbst für sichere Speicherung der Hashes verantwortlich.
- Der Credentials-Provider von Auth.js ist bewusst eingeschränkt (z. B. nur JWT-Sessions); das passt aber zum MVP.

**Warum nicht die anderen?**
- *Externer Dienst:* Weniger Arbeit, aber ein weiterer Anbieter mit personenbezogenen Daten und Kosten ab einer bestimmten Nutzerzahl.
- *Eigenbau:* Hohes Risiko für Sicherheitsfehler, kein Mehrwert für das Projekt.

## AI-Einsatz

Die AI hat alle drei Optionen genannt. Bei der kritischen Prüfung fiel auf, dass im ersten Entwurf kein Schutz gegen Passwort-Raten (Rate Limiting) vorgesehen war. Das wurde ergänzt.
