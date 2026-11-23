# Anforderungen und User Stories – SmartTaskManager

## Inhalt

1. [Projektidee und Problem](#1-projektidee-und-problem)
2. [Zielgruppe und Anwendungsfälle](#2-zielgruppe-und-anwendungsfälle)
3. [User Stories](#3-user-stories)
4. [Funktionale Anforderungen](#4-funktionale-anforderungen)
5. [Nichtfunktionale Anforderungen](#5-nichtfunktionale-anforderungen)
6. [Priorisierung (MoSCoW)](#6-priorisierung-moscow)
7. [Priorisierungsregel](#7-priorisierungsregel)
8. [Widersprüche und Unklarheiten](#8-widersprüche-und-unklarheiten)

Der MVP-Scope ist in [mvp.md](mvp.md) beschrieben.

---

## 1. Projektidee und Problem

**Idee:** Eine App zur Aufgabenverwaltung, die Aufgaben automatisch nach Wichtigkeit sortiert.

**Problem:** Viele Menschen haben zu viele Aufgaben und wissen nicht, womit sie anfangen sollen. Normale To-do-Listen zeigen nur eine Liste, helfen aber nicht beim Priorisieren. Dadurch werden wichtige Aufgaben vergessen oder zu spät erledigt.

## 2. Zielgruppe und Anwendungsfälle

### Zielgruppe

- **Studierende** – Abgaben, Prüfungen
- **Berufstätige** – Projekte, Termine
- **Privatpersonen** mit vielen Alltagsaufgaben

### Wichtigste Anwendungsfälle

| ID | Anwendungsfall | Abgedeckt durch |
|---|---|---|
| UC1 | Aufgabe schnell anlegen | US2, US3, US4 |
| UC2 | Sehen, was heute am wichtigsten ist | US5, US10 |
| UC3 | Aufgabe als erledigt markieren | US6 |
| UC4 | An Fristen erinnert werden | US8 |

## 3. User Stories

| ID | User Story | Anforderung | Priorität |
|---|---|---|---|
| US1 | Als Nutzer möchte ich ein Konto erstellen, damit meine Aufgaben gespeichert bleiben. | F1 | Must |
| US2 | Als Nutzer möchte ich eine Aufgabe mit Titel anlegen, damit ich nichts vergesse. | F2 | Must |
| US3 | Als Nutzer möchte ich eine Frist (Datum) setzen, damit die App weiß, wie dringend die Aufgabe ist. | F3 | Must |
| US4 | Als Nutzer möchte ich die Wichtigkeit (1–10) angeben, damit die Priorisierung besser wird. | F3 | Must |
| US5 | Als Nutzer möchte ich, dass die App meine Aufgaben automatisch sortiert, damit ich weiß, womit ich anfangen soll. | F4 | Must |
| US6 | Als Nutzer möchte ich eine Aufgabe als erledigt markieren, damit meine Liste übersichtlich bleibt. | F5 | Must |
| US7 | Als Nutzer möchte ich Aufgaben bearbeiten und löschen, damit ich Fehler korrigieren kann. | F2 | Must |
| US8 | Als Nutzer möchte ich eine Erinnerung vor der Frist bekommen, damit ich nichts verpasse. | F6 | Should |
| US9 | Als Nutzer möchte ich Aufgaben in Kategorien (z. B. Uni, Arbeit, Privat) einteilen, damit ich besser filtern kann. | F7 | Could |
| US10 | Als Nutzer möchte ich eine Aufgabe anpinnen, damit sie ganz oben steht, wenn ich mit der automatischen Sortierung nicht einverstanden bin. | F8 | Should |
| US11 | Als Nutzer möchte ich mein Konto mit allen Aufgaben löschen, damit meine Daten nicht weiter gespeichert werden. | F9 | Must |
| US12 | Als Nutzer möchte ich mein Passwort zurücksetzen, wenn ich es vergessen habe, damit ich wieder an meine Aufgaben komme. | F10 | Should |

### Akzeptanzkriterien

**US1 – Konto erstellen**
- Nutzer kann sich mit E-Mail und Passwort registrieren und anmelden.
- Das Passwort muss mindestens 8 Zeichen lang sein.
- Nach der Registrierung ist der Nutzer sofort angemeldet (keine E-Mail-Bestätigung im MVP).
- Nach erneutem Login sind alle zuvor angelegten Aufgaben wieder sichtbar.

**US2 – Aufgabe anlegen**
- Eine Aufgabe kann mit nur einem Titel angelegt werden (Frist und Wichtigkeit sind optional).
- Ein leerer Titel wird abgelehnt.

**US3 – Frist setzen**
- Pro Aufgabe kann ein Datum als Frist gesetzt oder entfernt werden.
- Die Frist wird in der Aufgabenliste angezeigt.
- Überfällige Aufgaben (Frist liegt vor dem heutigen Datum) werden farblich markiert.

**US4 – Wichtigkeit angeben**
- Wichtigkeit ist eine ganze Zahl von 1 bis 10; andere Werte werden abgelehnt.
- Ohne Angabe wird der Standardwert 5 verwendet.

**US5 – Automatische Sortierung**
- Die offene Liste ist nach der Priorisierungsregel aus [Kapitel 7](#7-priorisierungsregel) sortiert.
- Mit den Beispieldaten aus Kapitel 7 ergibt sich genau die dort angegebene Reihenfolge.
- Nach Anlegen oder Ändern einer Aufgabe wird die Liste sofort neu sortiert.

**US6 – Als erledigt markieren**
- Erledigte Aufgaben verschwinden aus der offenen Liste.
- Erledigte Aufgaben sind in einer eigenen Ansicht „Erledigt“ sichtbar.
- In der Ansicht „Erledigt“ kann die Markierung rückgängig gemacht werden; die Aufgabe erscheint dann wieder in der offenen Liste.

**US7 – Bearbeiten und löschen**
- Titel, Frist und Wichtigkeit können nachträglich geändert werden.
- Vor dem Löschen erfolgt eine Bestätigung.

**US8 – Erinnerung**
- Beim Öffnen der App wird ein Hinweis angezeigt, wenn offene Aufgaben morgen fällig sind (1 Tag vor der Frist).
- Der Hinweis nennt die betroffenen Aufgaben.
- Erinnerungen per E-Mail oder Push sind nicht Teil dieser Story (spätere Erweiterung).

**US9 – Kategorien**
- Einer Aufgabe kann eine Kategorie zugeordnet werden.
- Die Liste kann nach Kategorie gefiltert werden.

**US10 – Aufgabe anpinnen**
- Eine offene Aufgabe kann angepinnt und wieder gelöst werden.
- Angepinnte Aufgaben stehen immer über allen nicht angepinnten Aufgaben.
- Mehrere angepinnte Aufgaben sind untereinander nach der Priorisierungsregel sortiert.

**US11 – Konto löschen**
- Nutzer kann sein Konto löschen; dazu muss er sein Passwort erneut eingeben.
- Danach sind das Konto und alle zugehörigen Aufgaben aus der Datenbank entfernt.
- Ein Login mit den alten Zugangsdaten ist danach nicht mehr möglich.

**US12 – Passwort zurücksetzen**
- Nutzer kann über „Passwort vergessen“ einen Link an seine E-Mail-Adresse anfordern.
- Über den Link kann ein neues Passwort (mindestens 8 Zeichen) gesetzt werden; der Link ist nur einmal und nur zeitlich begrenzt gültig.
- Danach ist ein Login nur noch mit dem neuen Passwort möglich.

## 4. Funktionale Anforderungen

| ID | Anforderung | User Stories |
|---|---|---|
| F1 | Registrierung und Login | US1 |
| F2 | Aufgaben anlegen, bearbeiten, löschen | US2, US7 |
| F3 | Frist und Wichtigkeit pro Aufgabe angeben | US3, US4 |
| F4 | Automatische Priorisierung nach Frist und Wichtigkeit | US5 |
| F5 | Aufgaben als erledigt markieren, Ansicht „Erledigt“ | US6 |
| F6 | Erinnerung in der App vor der Frist | US8 |
| F7 | Kategorien und Filter | US9 |
| F8 | Aufgaben anpinnen (manuelle Priorität) | US10 |
| F9 | Konto mit allen Aufgaben löschen | US11 |
| F10 | Passwort per E-Mail-Link zurücksetzen | US12 |

## 5. Nichtfunktionale Anforderungen

| ID | Kategorie | Anforderung |
|---|---|---|
| NF1 | Bedienbarkeit | Eine Aufgabe kann in unter 10 Sekunden angelegt werden. |
| NF2 | Leistung | Die Aufgabenliste lädt in unter 2 Sekunden. |
| NF3 | Sicherheit | Passwörter werden gehasht gespeichert, nie im Klartext. |
| NF4 | Verfügbarkeit | Die App ist als Web-App in aktuellen Versionen von Chrome, Firefox, Edge und Safari nutzbar. |
| NF5 | Datenschutz | DSGVO-Grundanforderungen: Gespeichert werden nur E-Mail, Passwort-Hash und die Aufgaben sowie die technisch nötigen Sitzungs- und Protokolldaten des Auth-Dienstes; keine weiteren Profildaten. Das Konto kann mit allen Aufgaben gelöscht werden (F9). Daten werden in der EU gespeichert. |
| NF6 | Bedienbarkeit | Das Layout ist responsiv und auf Smartphones ab 360 px Bildschirmbreite bedienbar. |

## 6. Priorisierung (MoSCoW)

| Priorität | Anforderungen |
|---|---|
| **Must** | F1, F2, F3, F4, F5, F9, NF1, NF3, NF4, NF5 |
| **Should** | F6, F8, F10, NF2, NF6 |
| **Could** | F7 |
| **Won't (vorerst)** | Native Handy-App, Erinnerungen per E-Mail oder Push, E-Mail-Bestätigung bei der Registrierung, Teamfunktionen, Kalender-Anbindung |

Das MVP umfasst genau die Must-Anforderungen, siehe [mvp.md](mvp.md).

## 7. Priorisierungsregel

> Je näher die Frist und je höher die Wichtigkeit, desto weiter oben steht die Aufgabe. Wichtigkeit ist schwerer gewichtet als Frist.

Die Regel wurde vom Team festgelegt (Stand 23.11.2026):

```
Tage bis zur Frist T = Kalendertage zwischen heute und dem Fristdatum
                       (lokales Datum des Nutzers; Frist ist ein Datum ohne Uhrzeit)

Dringlichkeit D (1–10):
  keine Frist                    → 1
  Frist heute oder überfällig    → 10
  sonst                          → max(1, 10 − T)

Score = 0,6 × Wichtigkeit + 0,4 × Dringlichkeit
```

Für die Umsetzung: Sortiert und verglichen wird mit der ganzen Zahl `6 × Wichtigkeit + 4 × Dringlichkeit` (= 10 × Score). Kommazahlen wie 0,6 und 0,4 kann ein Computer nicht exakt darstellen (0,6 × 6 + 0,4 × 4 ergibt 5,1999… statt 5,2); dadurch würde der Gleichstand-Vergleich unzuverlässig. Angezeigt wird der Score mit einer Nachkommastelle.

Sortierreihenfolge der offenen Liste:

1. Angepinnte Aufgaben (F8), untereinander absteigend nach Score
2. Alle übrigen Aufgaben (mit und ohne Frist), absteigend nach Score
3. Bei Gleichstand: frühere Frist zuerst (Aufgaben ohne Frist zuletzt), danach älteste Aufgabe zuerst

Überfällige Aufgaben werden zusätzlich farblich markiert (siehe US3).

**Beispiel** (keine Aufgabe angepinnt):

| Rang | Aufgabe | Wichtigkeit | Frist | D | Score |
|---|---|---|---|---|---|
| 1 | Prüfung lernen | 9 | in 3 Tagen | 7 | 8,2 |
| 2 | Abgabe Übungsblatt | 6 | morgen | 9 | 7,2 |
| 3 | Steuererklärung | 10 | – | 1 | 6,4 |
| 4 | Wäsche waschen | 3 | heute | 10 | 5,8 |
| 5 | Buch lesen | 7 | – | 1 | 4,6 |
| 6 | Pflanzen umtopfen | 1 | in 30 Tagen | 1 | 1,0 |

Das Beispiel zeigt: Eine sehr wichtige Aufgabe ohne Frist (Steuererklärung) steht über einer unwichtigen Aufgabe, die heute fällig ist (Wäsche waschen).

## 8. Widersprüche und Unklarheiten

| # | Problem | Entscheidung |
|---|---|---|
| 1 | **Unklar:** Was ist „wichtig“? Die Regel für die Priorisierung war nicht definiert. | Klare Regel festgelegt: Frist + Wichtigkeit (siehe [Kapitel 7](#7-priorisierungsregel)). |
| 2 | **Widerspruch:** Automatische Sortierung (US5) vs. manuelle Priorität (US10) – was gilt? | Manuelle Priorität hat immer Vorrang. |
| 3 | **Unklar:** Was passiert mit Aufgaben ohne Frist? | Sie erhalten die Dringlichkeit 1 und werden über den Score einsortiert (siehe #12). |
| 4 | **Unklar:** Wann kommt die Erinnerung? | Standard: 1 Tag vor der Frist. |
| 5 | **Unklar:** NF1 „schnell anlegen“ war zu ungenau. | Messbar gemacht: unter 10 Sekunden. |
| 6 | **Widerspruch:** NF4 forderte Nutzung „im Browser und am Handy“, war aber nur als Could („Handy-App“) priorisiert. | Getrennt: Web-App im Browser ist Must (NF4), responsives Layout ist Should (NF6), native Handy-App ist Won't. |
| 7 | **Unklar:** NF5 (DSGVO) war nur Should, aber durch das Login (F1) werden personenbezogene Daten gespeichert. | DSGVO-Grundanforderungen sind Must (NF5): Datensparsamkeit, Konto löschen (F9, US11), Speicherung in der EU. |
| 8 | **Unklar:** Wie genau sieht die manuelle Priorität (US10) aus? | Aufgabe anpinnen: angepinnte Aufgaben stehen ganz oben. Freie Reihenfolge per Drag & Drop ist nicht vorgesehen. |
| 9 | **Unklar:** Wie werden überfällige Aufgaben behandelt? | Höchste Dringlichkeit (D = 10) und farbliche Markierung. |
| 10 | **Unklar:** Pflichtfelder beim Anlegen – ist eine Aufgabe ohne Wichtigkeit gültig? | Nur der Titel ist Pflicht; Wichtigkeit hat den Standardwert 5, Frist ist optional. |
| 11 | **Unklar:** Die Formel war nur ein „Vorschlag“, US5 verweist aber darauf – US5 war so nicht testbar. | Formel ist verbindlich festgelegt (Kapitel 7). |
| 12 | **Widerspruch:** Aufgaben ohne Frist standen bisher immer unter allen Aufgaben mit Frist. Damit stand „Wichtigkeit 10 ohne Frist“ unter „Wichtigkeit 1, Frist in 30 Tagen“ – im Widerspruch zu „Wichtigkeit ist schwerer gewichtet“. | Aufgaben ohne Frist erhalten D = 1 und werden gemeinsam mit allen anderen nach Score sortiert. |
| 13 | **Unklar:** Erledigte Aufgaben „verschwinden“, die Markierung soll aber rückgängig gemacht werden können – wo? | Eigene Ansicht „Erledigt“, dort kann die Markierung aufgehoben werden. |
| 14 | **Unklar:** Über welchen Kanal kommt die Erinnerung (US8)? | Hinweis in der App beim Öffnen. E-Mail und Push sind spätere Erweiterungen (Won't vorerst), da sie Mail-Dienst bzw. Scheduler und damit zusätzliche Infrastruktur und Kosten erfordern. |
| 15 | **Unklar:** Muss die E-Mail-Adresse bei der Registrierung bestätigt werden? | Nein, im MVP nicht. Der Nutzer ist nach der Registrierung sofort angemeldet. Bewusst in Kauf genommen: Die E-Mail-Adresse ist nicht verifiziert. Grund: Die Demo soll nicht vom (limitierten) Mailversand abhängen. |
| 16 | **Lücke:** Was passiert, wenn ein Nutzer sein Passwort vergisst? | Neue Story US12 (Should). Der Auth-Dienst (Supabase, siehe [architecture.md](architecture.md)) bringt die Funktion mit; für den E-Mail-Versand an beliebige Adressen ist aber ein eigener SMTP-Dienst nötig (siehe [hosting.md](hosting.md)). |
| 17 | **Widerspruch:** NF5 sagte, es würden „nur E-Mail und Passwort-Hash“ gespeichert. Der Auth-Dienst (Supabase) speichert aber zusätzlich Sitzungs- und Protokolldaten (u. a. IP-Adresse, Browser-Kennung). Die Protokolleinträge werden beim Konto-Löschen nicht automatisch mitgelöscht. | NF5 korrigiert. Konto und Aufgaben werden vollständig gelöscht (F9). **Offen bis vor einem Echtbetrieb:** Aufbewahrungsdauer und Löschung der Auth-Protokolle bei Supabase klären und in der Datenschutzerklärung nennen. Im Semester werden nur Testkonten verwendet. |
| 18 | **Lücke:** Konto löschen war nur mit einer Bestätigung abgesichert – an einem unbeaufsichtigten, angemeldeten Rechner könnte jemand fremdes das Konto löschen. | Zum Löschen muss das Passwort erneut eingegeben werden (US11). |
