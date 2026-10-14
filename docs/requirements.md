# Anforderungen und User Stories – SmartTaskManager

## Inhalt

1. [Projektidee und Problem](#1-projektidee-und-problem)
2. [Zielgruppe und Anwendungsfälle](#2-zielgruppe-und-anwendungsfälle)
3. [User Stories](#3-user-stories)
4. [Funktionale Anforderungen](#4-funktionale-anforderungen)
5. [Nichtfunktionale Anforderungen](#5-nichtfunktionale-anforderungen)
6. [Priorisierung (MoSCoW)](#6-priorisierung-moscow)
7. [MVP-Scope](#7-mvp-scope)
8. [Widersprüche und Unklarheiten](#8-widersprüche-und-unklarheiten)

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
| US10 | Als Nutzer möchte ich die Priorität einer Aufgabe selbst ändern, wenn ich mit der automatischen Sortierung nicht einverstanden bin. | F8 | Should |

### Akzeptanzkriterien

**US1 – Konto erstellen**
- Nutzer kann sich mit E-Mail und Passwort registrieren und anmelden.
- Nach erneutem Login sind alle zuvor angelegten Aufgaben wieder sichtbar.

**US2 – Aufgabe anlegen**
- Eine Aufgabe kann mit nur einem Titel angelegt werden (Frist und Wichtigkeit sind optional).
- Ein leerer Titel wird abgelehnt.

**US3 – Frist setzen**
- Pro Aufgabe kann ein Datum als Frist gesetzt oder entfernt werden.
- Die Frist wird in der Aufgabenliste angezeigt.

**US4 – Wichtigkeit angeben**
- Wichtigkeit ist eine ganze Zahl von 1 bis 10.
- Ohne Angabe wird ein Standardwert verwendet (Vorschlag: 5).

**US5 – Automatische Sortierung**
- Die Liste wird nach der Priorisierungsregel aus [Kapitel 7](#7-mvp-scope) sortiert.
- Nach Anlegen oder Ändern einer Aufgabe wird die Liste sofort neu sortiert.

**US6 – Als erledigt markieren**
- Erledigte Aufgaben verschwinden aus der offenen Liste.
- Die Markierung kann rückgängig gemacht werden.

**US7 – Bearbeiten und löschen**
- Titel, Frist und Wichtigkeit können nachträglich geändert werden.
- Vor dem Löschen erfolgt eine Bestätigung.

**US8 – Erinnerung**
- Standardmäßig erfolgt eine Erinnerung 1 Tag vor der Frist.

**US9 – Kategorien**
- Einer Aufgabe kann eine Kategorie zugeordnet werden.
- Die Liste kann nach Kategorie gefiltert werden.

**US10 – Priorität manuell ändern**
- Nutzer kann eine Aufgabe manuell höher oder niedriger einstufen.
- Die manuelle Priorität hat Vorrang vor der automatischen Sortierung und kann zurückgesetzt werden.

## 4. Funktionale Anforderungen

| ID | Anforderung | User Stories |
|---|---|---|
| F1 | Registrierung und Login | US1 |
| F2 | Aufgaben anlegen, bearbeiten, löschen | US2, US7 |
| F3 | Frist und Wichtigkeit pro Aufgabe angeben | US3, US4 |
| F4 | Automatische Priorisierung nach Frist und Wichtigkeit | US5 |
| F5 | Aufgaben als erledigt markieren | US6 |
| F6 | Erinnerungen vor der Frist | US8 |
| F7 | Kategorien und Filter | US9 |
| F8 | Priorität manuell überschreiben | US10 |

## 5. Nichtfunktionale Anforderungen

| ID | Kategorie | Anforderung |
|---|---|---|
| NF1 | Bedienbarkeit | Eine Aufgabe kann in unter 10 Sekunden angelegt werden. |
| NF2 | Leistung | Die Aufgabenliste lädt in unter 2 Sekunden. |
| NF3 | Sicherheit | Passwörter werden verschlüsselt (gehasht) gespeichert. |
| NF4 | Verfügbarkeit | Die App ist im Browser und am Handy nutzbar. |
| NF5 | Datenschutz | Daten werden DSGVO-konform gespeichert. |

## 6. Priorisierung (MoSCoW)

| Priorität | Anforderungen |
|---|---|
| **Must** | F1, F2, F3, F4, F5, NF1, NF3 |
| **Should** | F6, F8, NF2, NF5 |
| **Could** | F7, NF4 (Handy-App) |
| **Won't (vorerst)** | Teamfunktionen, Kalender-Anbindung |

## 7. MVP-Scope

Das MVP enthält nur die Must-Anforderungen:

- Login (F1)
- Aufgaben anlegen, bearbeiten, löschen (F2)
- Frist und Wichtigkeit angeben (F3)
- Automatische Sortierung (F4)
- Aufgaben abhaken (F5)

### Priorisierungsregel im MVP

> Je näher die Frist und je höher die Wichtigkeit, desto weiter oben steht die Aufgabe. Wichtigkeit ist schwerer gewichtet als Frist.

**Konkreter Vorschlag zur Umsetzung** (kann im Team angepasst werden):

```
Dringlichkeit D (1–10):
  Frist heute oder überfällig  → 10
  sonst                        → max(1, 10 − Tage bis zur Frist)

Score = 0,6 × Wichtigkeit + 0,4 × Dringlichkeit
```

Sortierreihenfolge:

1. Aufgaben mit manueller Priorität (sobald F8 umgesetzt ist)
2. Aufgaben **mit** Frist, absteigend nach Score
3. Aufgaben **ohne** Frist, absteigend nach Wichtigkeit
4. Bei Gleichstand: frühere Frist zuerst, danach älteste Aufgabe zuerst

**Beispiel:**

| Aufgabe | Wichtigkeit | Frist | D | Score |
|---|---|---|---|---|
| Prüfung lernen | 9 | in 3 Tagen | 7 | 8,2 |
| Abgabe Übungsblatt | 6 | morgen | 9 | 7,2 |
| Wäsche waschen | 3 | heute | 10 | 5,8 |
| Buch lesen | 7 | – | – | unten einsortiert |

## 8. Widersprüche und Unklarheiten

| # | Problem | Lösung |
|---|---|---|
| 1 | **Unklar:** Was ist „wichtig“? Die Regel für die Priorisierung war nicht definiert. | Klare Regel festlegen: Frist + Wichtigkeit (siehe [Kapitel 7](#7-mvp-scope)). |
| 2 | **Widerspruch:** Automatische Sortierung (US5) vs. manuelle Priorität (US10) – was gilt? | Manuelle Priorität hat immer Vorrang. |
| 3 | **Unklar:** Was passiert mit Aufgaben ohne Frist? | Sie werden nach Wichtigkeit sortiert und unten eingereiht. |
| 4 | **Unklar:** Wann kommt die Erinnerung? | Standard: 1 Tag vor der Frist. |
| 5 | **Unklar:** NF1 „schnell anlegen“ war zu ungenau. | Messbar gemacht: unter 10 Sekunden. |

### Weitere offene Punkte

Bei der abschließenden Prüfung sind folgende Punkte aufgefallen, die das Team noch entscheiden sollte:

| # | Problem | Vorschlag |
|---|---|---|
| 6 | **Widerspruch:** NF4 fordert Nutzung „im Browser und am Handy“, ist aber nur als Could („Handy-App“) priorisiert. | Trennen: Web-App im Browser ist Teil des MVP; responsives Layout bzw. native Handy-App bleibt Could. |
| 7 | **Unklar:** NF5 (DSGVO) ist nur Should, aber durch das Login (F1) werden personenbezogene Daten gespeichert. | DSGVO-Konformität ist gesetzlich verpflichtend – Grundanforderungen (z. B. Datensparsamkeit, Löschen des Kontos) sollten ins MVP. |
| 8 | **Unklar:** Wie genau sieht die manuelle Priorität (US10) aus? | Vorschlag: Aufgabe „anpinnen“ (ganz oben) oder Wichtigkeit direkt ändern. |
| 9 | **Unklar:** Wie werden überfällige Aufgaben behandelt? | Mit höchster Dringlichkeit (D = 10) einsortieren und farblich markieren. |
| 10 | **Unklar:** Pflichtfelder beim Anlegen – ist eine Aufgabe ohne Wichtigkeit gültig? | Nur der Titel ist Pflicht; Wichtigkeit hat einen Standardwert (5), damit NF1 erreichbar bleibt. |
