# MVP-Scope – SmartTaskManager

## Ziel des MVP

Das MVP soll den Kern der Idee beweisen: Ein Nutzer legt Aufgaben mit Frist und Wichtigkeit an und sieht sofort, womit er anfangen sollte. Alles, was dafür nicht unverzichtbar ist, kommt später.

Das MVP umfasst genau die **Must**-Anforderungen aus [requirements.md](requirements.md#6-priorisierung-moscow).

## Demonstrierbarer Anwendungsfall

Der folgende Ablauf muss im MVP vollständig funktionieren und wird bei der Abnahme vorgeführt:

1. Nutzer registriert sich mit E-Mail und Passwort und meldet sich an.
2. Nutzer legt die sechs Aufgaben aus dem Beispiel in [requirements.md, Kapitel 7](requirements.md#7-priorisierungsregel) an – teils mit, teils ohne Frist.
3. Die Liste zeigt die Aufgaben in genau der dort angegebenen Reihenfolge.
4. Nutzer setzt bei „Buch lesen“ die Frist auf gestern – die Aufgabe ist farblich als überfällig markiert und rückt von Platz 5 auf Platz 1 (D = 10, Score 8,2). Sie hat damit denselben Score wie „Prüfung lernen“; die frühere Frist entscheidet (Gleichstand-Regel).
5. Nutzer ändert die Wichtigkeit einer Aufgabe – die Liste sortiert sich sofort neu.
6. Nutzer hakt eine Aufgabe ab – sie verschwindet aus der offenen Liste und erscheint in der Ansicht „Erledigt“. Dort macht er das Abhaken rückgängig.
7. Nutzer löscht eine Aufgabe nach Bestätigung.
8. Nutzer meldet sich ab und wieder an – alle Aufgaben sind noch da.
9. Nutzer löscht sein Konto nach erneuter Passworteingabe – danach ist kein Login mehr möglich.

## Enthaltene Funktionen

| ID | Funktion | User Stories |
|---|---|---|
| F1 | Registrierung und Login mit E-Mail und Passwort | US1 |
| F2 | Aufgaben anlegen, bearbeiten, löschen (nur Titel ist Pflicht) | US2, US7 |
| F3 | Frist (Datum, optional) und Wichtigkeit (1–10, Standard 5) | US3, US4 |
| F4 | Automatische Sortierung nach der Priorisierungsregel, Markierung überfälliger Aufgaben | US5 |
| F5 | Aufgaben abhaken, Ansicht „Erledigt“, Abhaken rückgängig machen | US6 |
| F9 | Konto mit allen Aufgaben löschen | US11 |

## Enthaltene nichtfunktionale Anforderungen

| ID | Anforderung |
|---|---|
| NF1 | Aufgabe in unter 10 Sekunden anlegen |
| NF3 | Passwörter gehasht speichern |
| NF4 | Nutzbar als Web-App in aktuellen Versionen von Chrome, Firefox, Edge und Safari |
| NF5 | DSGVO-Grundanforderungen: nur E-Mail, Passwort-Hash, Aufgaben sowie technisch nötige Sitzungs- und Protokolldaten des Auth-Dienstes, keine weiteren Profildaten; Konto mit allen Aufgaben löschen, Speicherung in der EU |

## Nicht im MVP

| Funktion | Priorität | Begründung |
|---|---|---|
| Erinnerung in der App (F6, US8) | Should | Nützlich, aber für den Kernnutzen (Sortierung) nicht nötig. Erste Erweiterung nach dem MVP. |
| Aufgaben anpinnen (F8, US10) | Should | Die automatische Sortierung muss zuerst allein funktionieren. Zweite Erweiterung. |
| Passwort zurücksetzen (F10, US12) | Should | Für die Demo nicht nötig. Funktion bringt der Auth-Dienst mit, benötigt aber einen eigenen SMTP-Dienst. |
| Ladezeit unter 2 Sekunden (NF2) | Should | Wird angestrebt, aber nicht formal abgenommen. |
| Responsives Layout für Smartphones (NF6) | Should | Das MVP wird am Desktop-Browser vorgeführt. |
| Kategorien und Filter (F7, US9) | Could | Nur bei verbleibender Zeit. |
| Erinnerungen per E-Mail oder Push | Won't (vorerst) | Benötigt Mail-Dienst bzw. Scheduler – zusätzliche Infrastruktur und Kosten. |
| Native Handy-App | Won't (vorerst) | Die Web-App deckt den Anwendungsfall ab. |
| Teamfunktionen (Aufgaben teilen) | Won't (vorerst) | Würde Rechte- und Freigabekonzept erfordern, sprengt den Semesterumfang. |
| Kalender-Anbindung | Won't (vorerst) | Abhängigkeit von externen APIs, nicht nötig für den Kernnutzen. |
| E-Mail-Bestätigung bei der Registrierung | Won't (vorerst) | Demo soll nicht vom limitierten Mailversand abhängen (requirements.md #15). |

## Reihenfolge nach dem MVP

1. F6 – Erinnerung in der App
2. F8 – Aufgaben anpinnen
3. F10 – Passwort zurücksetzen
4. NF6 – Responsives Layout
5. F7 – Kategorien und Filter

## Abnahme

Das MVP gilt als fertig, wenn

- der demonstrierbare Anwendungsfall oben ohne Fehler durchläuft,
- alle Akzeptanzkriterien der Must-Stories (US1–US7, US11) erfüllt sind,
- NF1, NF3, NF4 und NF5 nachweislich erfüllt sind.
