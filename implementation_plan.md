# Hallenfußball Dienstagskick – Live Statistik & Quoten-Tracker

Eine schlanke, webbasierte Echtzeit-Applikation für eure Hallenfußball-Runde am Dienstagabend. Entwickelt für die blitzschnelle mobile Erfassung in der Halle (1-Klick-Auswahl der Verlierer) mit automatischer Berechnung aller absoluten und relativen Niederlagenquoten, geräteübergreifender Sofort-Synchronisierung und Link-Freigabe für alle Mitspieler ohne Login-Hürde.

## User Review & Critical Decisions

> [!IMPORTANT]
> Basierend auf euren Antworten sind folgende Kernentscheidungen für die App festgelegt:

- **Einfachste Erfassung**: Nur die Verlierer des Spiels werden per Fingertipp ausgewählt (kein mühsames Eintippen von Torständen nötig; Spielende bei 10 Toren ist implizit).
- **Fokus-Statistiken**: Reine Auswertung von Niederlagen, Einsätzen, Gesamtspielen sowie zwei Vergleichsquoten (Niederlagen / eigene Spiele und Niederlagen / alle Spiele der Gruppe).
- **Sofort-Zugriff ohne Login**: Jeder Mitspieler mit dem Link kann die Seite öffnen, aktuelle Statistiken sehen und neue Spiele sofort eintragen.
- **Vorkonfigurierte Spieler**: Die 12 Stammspieler (*Max, Jonas, Felix, Lukas Henrichs, Basti, Elton, Lukas Heider, Nico, Sandro, Stefan, Tom, Robin*) sind direkt hinterlegt; neue Spieler können bei Bedarf ergänzt werden.

---

## 1. Overview & Core Concept

- **Zweck**: Schnelle, frustfreie Erfassung der Hallenfußball-Spiele am Dienstagabend direkt am Spielfeldrand auf dem Smartphone, mit direkter Auswertung darüber, wer am häufigsten und am seltensten verliert.
- **Zielgruppe**: Eure 6 bis 10 anwesenden Kicker pro Abend und die gesamte 12er-Runde.
- **Hauptnutzen**: 
  - Keine Zettelwirtschaft oder WhatsApp-Debatten mehr.
  - Mit 2 Klicks ist ein Spiel erfasst: Anwesende Spieler wählen -> Verlierer antippen -> Speichern.
  - Sofortige Live-Aktualisierung auf allen Smartphones der Mitspieler.
  - Transparente Filter: Wer ist der "Pechvogel" des Abends/der Saison und wer hat die beste Quote?

---

## 2. User Experience & Visual Design

### Key User Flows

1. **Dashboard & Rangliste (Startansicht)**:
   - Oben: Gesamtspiele-Zähler (`Gesamtspiele: X`) und Datum des aktuellen/letzten Spieltags.
   - Schnellfilter: "Beste Quote (am wenigsten verloren)", "Meiste Niederlagen (absolut)", "Meiste Einsätze".
   - Ausführliche Tabelle & Kachelansicht mit Sortierung nach:
     - Name
     - Niederlagen (absolut)
     - Spiele / Einsätze (absolut)
     - Quote A: Niederlagen % bezogen auf eigene Einsätze ($Niederlagen / Spiele \times 100$)
     - Quote B: Niederlagen % bezogen auf Gesamtspiele aller Runden ($Niederlagen / Gesamtspiele \times 100$)
   - Auszeichnung von Spitzenreitern (z. B. "Wenigste Niederlagen", "Dauerbrenner").

2. **Spieltag & Schnell-Eingabe ("Spiel eintragen")**:
   - Modus "Wer ist heute da?": Zu Beginn des Abends hakt man schnell die 6–10 anwesenden Spieler an.
   - Pro Spiel bis 10: Einfach die Spieler der Verlierermannschaft antippen (z. B. 3, 4 oder 5 Personen).
   - Button "Spiel werten & speichern": Sofortige Verrechnung in Echtzeit!
   - Letztes Spiel rückgängig machen / löschen (falls versehentlich falsch getippt wurde).

3. **Verlauf & Filterung**:
   - Übersicht aller bisherigen Spiele mit Zeitstempel und beteiligten Verlierern.
   - Filter nach heutigem Spieltag vs. Ewige Tabelle (Gesamt-Statistik).
   - "Teilen"-Button: Kopiert den Link oder generiert eine Zusammenfassung für eure WhatsApp-Gruppe.

### Visual Identity & Theme

- **Aesthetic Direction**: Sportlich, modern, clean (Inspiriert von professionellen Sport-Analytics wie SofaScore und Kicker, aber ohne Werbe-Überladung).
- **Farben**:
  - Dominante Basis (60%): Tiefes Anthrazit / Schiefergrau (`#0f172a` bis `#1e293b`) mit klarem Hellmodus-Toggle oder edlem Hallenboden-/Rasen-Look.
  - Strukturelle Flächen (30%): Subtile Kartenoberflächen mit weichen Kanten (`rounded-2xl`) und feinen Haarlinien (`border-slate-800`).
  - Akzente (10%): Energiegeladenes Flutlicht-Smaragdgrün (`#10b981` / `#059669`) für Siege/Aktionen und sanftes Korallrot (`#f43f5e`) zur klaren Kennzeichnung von Niederlagen.
- **Typografie**:
  - Zahlen und Tabellen mit `font-mono tabular-nums` für perfekte visuelle Ausrichtung.
  - Moderne, dynamische Sans-Serif für Überschriften und Spielernamen.
- **Mobile Ergonomie**:
  - Riesige Touch-Ziele ($\ge 48\text{px}$) für schweißnasse Hände in der Halle.
  - Sticky-Footer mit Schnellzugriff auf "+ Neues Spiel".

---

## 3. Key Product Decisions & Trade-Offs

- **Backend & Daten-Synchronisation**:
  - *Entscheidung*: Vollwertige Server-Persistenz über Express API mit JSON-Dateispeicher und automatischer Live-Synchronisierung (Polling/SSE).
  - *Warum*: Funktioniert sofort ohne Konfigurationshürden, ist stabil, erfordert keinen Drittanbieter-Login und speichert alle Spiele dauerhaft auf dem Server ab. Jeder Mitspieler, der den Link im Browser öffnet, sieht exakt denselben Stand.
- **Berechnungslogik für das Hauptziel**:
  - *Absoluter Vergleich*: Wer hat in Summe die meisten / wenigsten Niederlagen?
  - *Relativer Einsatz-Vergleich*: $Quote_{Einsatz} = \frac{Niederlagen}{Einsätze} \times 100\%$ (z. B. 3 Niederlagen bei 10 Spielen = 30%).
  - *Relativer Gesamt-Vergleich*: $Quote_{Gesamt} = \frac{Niederlagen}{Gesamtspiele} \times 100\%$ (z. B. 3 Niederlagen bei 20 Gesamtspielen der Gruppe = 15%).
  - Spieler mit weniger als z. B. 3 Einsätzen werden mit Hinweis markiert, damit Einmaleinsätze die Quote nicht verfälschen.
- **Teilnehmerauswahl pro Spiel**:
  - Da an einem Abend 6 bis 10 Personen da sind, wechseln oft die Teams. Die App speichert, wer im jeweiligen Spiel auf dem Platz stand und wer verloren hat (die anderen Teilnehmer des Spiels zählen automatisch als Gewinner).

---

## 4. Technical Architecture & Data Strategy

```
┌─────────────────────────────────────────────────────────────────┐
│                      Client (React 19 + Vite)                   │
├────────────────────────────────┬────────────────────────────────┤
│      Übersicht / Tabelle       │      Spiel-Erfassung           │
│  - Gesamtspiele-Zähler         │  - Anwesende Kicker wählen     │
│  - Rangliste nach Quote/Loss   │  - Verlierer antippen          │
│  - Filter & Sortierer          │  - Sofort-Save & Haptik        │
├────────────────────────────────┴────────────────────────────────┤
│              Data Sync Layer (Live Sync State Manager)          │
└───────────────────────────────┬─────────────────────────────────┘
                                │ Fetch & Sync (/api/data)
┌───────────────────────────────▼─────────────────────────────────┐
│              Server (Node.js + Express auf Port 3000)           │
├─────────────────────────────────────────────────────────────────┤
│  REST Endpoints:                                                │
│  - GET  /api/state       -> Spieler, Spiele, Spieltage          │
│  - POST /api/matches     -> Neues Spiel mit Verlierern anlegen │
│  - DELETE /api/matches/:id -> Spiel stornieren / löschen        │
│  - POST /api/players     -> Neuen Mitspieler hinzufügen         │
│  - POST /api/reset-seed  -> Initialdaten (12 Spieler)           │
├─────────────────────────────────────────────────────────────────┤
│  Dateibasierter Speicher: data/football-tracker.json             │
└─────────────────────────────────────────────────────────────────┘
```

### Datenmodell

- **Player**: `{ id: string, name: string, active: boolean, createdAt: string }`
- **Match**: `{ id: string, timestamp: string, participantIds: string[], loserIds: string[], scoreTo: 10, note?: string }`
- **Calculated Stats (pro Spieler)**:
  - `totalGames`: Gesamtspiele der Runde
  - `appearances`: Eigene Einsätze (Spiele des Spielers)
  - `losses`: Anzahl Niederlagen
  - `wins`: Berechnete Siege ($Einsätze - Niederlagen$)
  - `lossRatePersonal`: $\frac{losses}{appearances} \times 100\%$
  - `lossRateTotal`: $\frac{losses}{totalGames} \times 100\%$
  - `rankAbsoluteLosses`: Rang nach absoluten Niederlagen
  - `rankRelativeLosses`: Rang nach prozentualer Niederlagenquote

---

## 5. Nächste Schritte nach Freigabe

1. Express-Backend-Routen für Spielerverwaltung, Match-Erfassung und dauerhafte Speicherung aufsetzen.
2. Initiales Spieler-Seeding für eure 12 Spieler (*Max, Jonas, Felix, Lukas Henrichs, Basti, Elton, Lukas Heider, Nico, Sandro, Stefan, Tom, Robin*) einrichten.
3. Responsive mobile Dashboard- und Eingabekomponenten mit animierter Interaktion und Echtzeit-Aktualisierung erstellen.
4. Filter, Sortierung und Statistikauswertung mit WhatsApp-Exportfunktion fertigstellen und testen.
