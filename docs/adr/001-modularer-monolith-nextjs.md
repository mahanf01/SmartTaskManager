# ADR-001: Modularer Monolith mit Next.js

- **Status:** angenommen
- **Datum:** 23.11.2026

## Kontext

Das MVP besteht im Kern aus Login, CRUD für Aufgaben und einer Sortierregel ([mvp.md](../mvp.md)). Es gibt keine Hintergrundprozesse, keine Echtzeit-Funktionen und keine Skalierungsanforderung. Das Team ist klein, hat ein Semester Zeit und kennt Next.js und Vercel. Die Priorisierungsregel (F4) muss isoliert testbar sein.

## Entscheidung

Eine einzige Next.js-Anwendung (TypeScript, App Router) enthält Oberfläche und Backend. Das Backend stellt eine REST-API über Route Handlers unter `/api/...` bereit. Intern ist der Code in Module getrennt: `auth`, `tasks`, `prioritization`, `account`, `ui`. Das Modul `prioritization` ist eine reine Funktion ohne Abhängigkeit zu Datenbank oder UI.

## Betrachtete Alternativen

- **Getrenntes Frontend und Backend** (Next.js + Express/NestJS): verworfen – zwei Codebasen, zwei Deployments, CORS, doppelte Typen; kein Nutzen für den MVP-Umfang.
- **Next.js mit Backend-as-a-Service, Browser greift direkt auf die Datenbank zu** (stärkste Alternative, wäre umsetzbar): verworfen – Konto löschen braucht den Admin-Schlüssel und damit ohnehin Server-Code (zusätzliche Edge Function), Logik wäre auf Browser, Datenbankregeln und Edge Function verteilt; Sicherheit hinge allein an den Zugriffsregeln in der Datenbank; keine explizite, dokumentierbare API.
- **Microservices**: verworfen – Overengineering für eine Kern-Entität.
- **Server Actions statt REST-API**: verworfen – keine eigenständig aufrufbaren Endpunkte; eine REST-API lässt sich mit HTTP-Tests direkt prüfen, auch die Sortierung mit festem Datum (ADR-003).

Details und Bewertungstabelle: [architecture.md](../architecture.md).

## Konsequenzen

- Ein Repository, ein Deployment, eine Sprache.
- Die Sortierregel kann mit den Beispieldaten aus requirements.md Kapitel 7 als Unit-Test geprüft werden.
- Oberfläche und Backend sind gekoppelt; ein späteres Herauslösen des Backends wäre Aufwand. Die explizite REST-API und die Modulgrenzen halten diesen Aufwand klein.
- Login, Zugriffsregeln und Datenzugriff sind an Supabase gebunden (ADR-002); die Geschäftslogik liegt aber im eigenen Code.
- Module dürfen nur über ihre öffentliche Schnittstelle zugreifen – das muss im Code-Review geprüft werden.
