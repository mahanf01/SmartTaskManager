# SmartTaskManager
SmartTaskManager project for the course PAI, group 5

Smart Task Manager – Anforderungsanalyse 

1. Projektidee und Problem 

Idee: Eine App zur Aufgabenverwaltung, die Aufgaben automatisch nach Wichtigkeit sortiert. 

Problem: Viele Menschen haben zu viele Aufgaben und wissen nicht, womit sie anfangen sollen. Normale To-do-Listen zeigen nur eine Liste, helfen aber nicht beim Priorisieren. Dadurch werden wichtige Aufgaben vergessen oder zu spät erledigt. 

2. Zielgruppe und Anwendungsfälle 

Zielgruppe: 

Studierende (Abgaben, Prüfungen) 

Berufstätige (Projekte, Termine) 

Privatpersonen mit vielen Alltagsaufgaben 

Wichtigste Anwendungsfälle: 

Aufgabe schnell anlegen 

Sehen, was heute am wichtigsten ist 

Aufgabe als erledigt markieren 

An Fristen erinnert werden 

. Priorisierung (MoSCoW) 

Priorität 

Anforderungen 

Must 

F1, F2, F3, F4, F5, NF1, NF3 

Should 

F6, F8, NF2, NF5 

Could 

F7, NF4 (Handy-App) 

Won't (vorerst) 

Teamfunktionen, Kalender-Anbindung 

7. MVP-Scope 

Das MVP enthält nur die Must-Anforderungen: 

Login 

Aufgaben anlegen, bearbeiten, löschen 

Frist und Wichtigkeit angeben 

Automatische Sortierung 

Aufgaben abhaken 

Einfache Regel für die Priorisierung im MVP: 

 Je näher die Frist und je höher die Wichtigkeit, desto weiter oben steht die Aufgabe. Wichtigkeit ist schwerer gewichtet als Frist. 

8. Widersprüche und Unklarheiten 

Problem 

Lösung 

Unklar: Was ist „wichtig“? Die Regel für die Priorisierung war nicht definiert. 

Klare Regel festlegen: Frist + Wichtigkeit (siehe MVP). 

Widerspruch: Automatische Sortierung (US5) vs. manuelle Priorität (US10) – was gilt? 

Manuelle Priorität hat immer Vorrang. 

Unklar: Was passiert mit Aufgaben ohne Frist? 

Sie werden nach Wichtigkeit sortiert und unten eingereiht. 

Unklar: Wann kommt die Erinnerung? 

Standard: 1 Tag vor der Frist. 

Unklar: NF1 „schnell anlegen“ war zu ungenau. 

Messbar gemacht: unter 10 Sekunden. 




