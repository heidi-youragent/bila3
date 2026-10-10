# Projekt: bila3 (Bilaterale III Informationsplattform)

> Arbeitsregeln für alle AI-Assistenten (Claude, ChatGPT/Codex, Cursor, Copilot, Gemini …) und Menschen, die an diesem Projekt arbeiten. `CLAUDE.md` verweist auf diese Datei.

## Projektziel
`bila3` ist eine neutrale, mobile-responsive Webplattform zur Information der Schweizer Bevölkerung über das Verhandlungspaket Schweiz–EU (Bilaterale III). Die Plattform bereitet die ca. 2'000 Seiten umfassende Botschaft des Bundesrats transparent und übersichtlich auf.

## Architektur & Prinzipien
- **Decoupled Architecture:** Die UI (`index.html`) ist vollständig von den Daten (`themen.json`) getrennt.
- **Data Policy:** Alle inhaltlichen Anpassungen, Argumente, Parteipositionen und Quellenangaben werden **ausschliesslich** in der `themen.json` gepflegt.
- **Design:** Tailwind CSS via CDN, neutral, übersichtlich und mobile-first.
- **Keine Desinformation/Cherry-Picking:** Klare Kennzeichnung von Verträgen als Hauptpaket (nicht separat abwählbar), optionales Paket (separates Referendum möglich) oder Begleitmassnahme.
- **Mehrsprachigkeit:** Sprachen `de`, `fr`, `it`, `en`. Deutsch ist die Referenz und Fallback: Fehlt eine Übersetzung, zeigt die UI den deutschen Text mit Hinweis (`ui.<lang>.onlyDe`).
- **Quellen:** Offizielle Botschaft in `assets/Botschaft_DE.pdf`, `assets/Botschaft_FR.pdf`, `assets/Botschaft_IT.pdf` (Seitenzahlen unterscheiden sich je Sprache). Quellenangaben in `inhalt.fr`/`inhalt.it` verlinken auf die jeweilige Sprachfassung (`#page=N`); `inhalt.en` verlinkt auf die deutsche Fassung mit Vermerk „(German ed.)“ – eine offizielle englische Botschaft gibt es nicht.
- **Übersetzungen:** FR/IT verwenden die Terminologie der offiziellen FR/IT-Botschaft (z. B. *reprise dynamique du droit*, *recepimento dinamico del diritto*). Neue oder geänderte deutsche Inhalte müssen in allen vier Sprachen nachgeführt werden.

## Veröffentlichung
GitHub Pages aus dem Branch `main` (Root), Domain **bila3.alpenos.ch** (`CNAME`-Datei nicht löschen). Ein Push auf `main` ist eine Veröffentlichung – vorher lokal prüfen und nur mit Freigabe der Herausgeberin pushen. Das Repo ist öffentlich: keine Zugangsdaten, Entwürfe oder privaten Notizen committen.

## Lokal ansehen
`index.html` lädt `themen.json` per `fetch` – direktes Öffnen per `file://` funktioniert nicht. In VS Code die Erweiterung **Live Server** nutzen („Go Live“ unten rechts in der Statusleiste) oder `python3 -m http.server 8000`.

## Datenstruktur (`themen.json`)

Übersetzbare Texte sind immer Objekte `{"de": "...", "fr": "...", "it": "...", "en": "..."}` („i18n-Objekt“). Ausnahme: Themeninhalte sind pro Sprache als ganzer Block unter `inhalt.<lang>` abgelegt.

### Oberste Ebene
```json
{
  "meta":    { "stand": "YYYY-MM-DD", "botschaftUrl": i18n, "portalUrl": i18n, "edaUrl": i18n,
               "redaktion": i18n },
  "ui":      { "de": { "<key>": "..." }, "fr": {...}, "it": {...}, "en": {...} },
  "verlauf": { "etappen": [ { "id", "label": i18n } ], "entfallen": [ "<etappen-id>" ],
               "eintraege": [ { "datum", "etappe", "erledigt", "titel": i18n, "text": i18n, "quelle": { "label": i18n, "url": i18n } } ] },
  "systemCheck": { "de": { "titel", "untertitel", "intro", "schritte": [ { "titel", "text" } ],
                           "proTitel", "pro": [], "contraTitel", "contra": [] }, "fr": {...}, "it": {...}, "en": {...} },
  "tags":    { "<tag-id>": i18n },
  "gruppen": {
    "branchen": [ { "id": "industrie", "label": i18n, "hinweis": i18n /* optional */ } ],
    "system":   [ { "id": "souveraenitaet", "label": i18n } ]
  },
  "akteure": [ ... ],
  "themen":  [ ... ]
}
```

- `ui.de` ist vollständig; andere Sprachen dürfen Keys weglassen (Fallback auf `de`), z. B. `herausgeberName`, `kontaktWert`.
- URLs in `meta` dürfen ein i18n-Objekt oder ein einfacher String sein; fehlende Sprachen fallen auf `de` zurück.
- `systemCheck` ist die Erklärbox „Kernmechanismus“ oberhalb der Themen; Icons und Farben der 5 Schritte stehen in `index.html` (`STEP_STYLE`), die Texte nur hier.
- Branchen-IDs: `industrie`, `banken`, `gesundheit`, `energie`, `landwirtschaft`, `lohn`, `kantone`
- System-IDs: `souveraenitaet`, `foederalismus`, `demokratie`, `finanzen`

### Thema (`themen[]`)
```json
{
  "id": "institutionell",
  "icon": "scale",
  "paket": "haupt" | "optional" | "begleit",
  "redaktion": "fertig" | "entwurf",
  "kapitel": "2.1",
  "tags": ["stimmvolk", "export"],          // IDs aus "tags"
  "branchen": ["industrie", "kantone"],     // IDs aus gruppen.branchen
  "system": ["souveraenitaet", "demokratie"], // IDs aus gruppen.system
  "inhalt": {
    "de": {
      "titel": "...",
      "teaser": "Ein Satz für die Kartenansicht",
      "ziel": "Hauptziel des Abkommens",
      "statusQuo": "Heutige rechtliche/politische Lage",
      "pro": ["Chance/Nutzen aus Schweizer Sicht", "..."],
      "contra": ["Risiko/Kompromiss/Bürokratie", "..."],
      "kantone": ["Auswirkung auf Kantone und Gemeinden", "..."],
      "positionen": {
        "svp":    { "status": "dafuer" | "vorbehalt" | "dagegen" | "offen", "text": "..." },
        "sp": {...}, "fdp": {...}, "mitte": {...}, "glp": {...}, "gruene": {...}, "kdk": {...}
      },
      "quellen": [
        { "ref": "S. 91–104", "url": "assets/Botschaft_DE.pdf#page=91", "titel": "Botschaft, Kap. 2.1 – ..." }
      ]
    },
    "fr": { /* gleiche Struktur */ }, "it": {...}, "en": {...}
  }
}
```

- `paket`: `haupt` = Hauptpaket (nicht separat abwählbar), `optional` = optionales Paket (eigenes Referendum möglich), `begleit` = Begleitmassnahme.
- `status: "offen"` wird als „Keine Position dokumentiert“ angezeigt – nur verwenden, wenn kein öffentlicher Beleg existiert.
- Pro- und Contra-Listen sollen ausgewogen sein (ähnliche Anzahl und Gewichtung).

### Akteur (`akteure[]`)
```json
{
  "id": "svp", "kurz": "SVP",
  "name": i18n,
  "status": "dafuer" | "vorbehalt" | "dagegen",
  "begruendung": i18n,
  "unterstuetzt": [ { "thema": "<themen-id>", "text": i18n } ],
  "kritisiert":   [ { "thema": "<themen-id>", "text": i18n } ],
  "quelle": { "label": i18n, "url": i18n | "https://..." }
}
```
Akteur-IDs: `bundesrat`, `kdk`, `svp`, `sp`, `fdp`, `mitte`, `glp`, `gruene`.

## Status-Updates („Wo steht das Geschäft?“)

**Wunsch der Herausgeberin – verbindlich:**
- Auf der Seite ist zugeklappt **nur die Fortschrittsleiste sichtbar** (auf dem Handy plus eine Zeile „Etappe X von 7“). Der Text des neuesten Eintrags und die Historie erscheinen erst nach Klick auf „Details & Verlauf anzeigen“ und bleiben standardmässig **zugeklappt** und darf die Seite nicht verlängern – nicht aufgeklappt anzeigen, nicht an anderer Stelle wiederholen, keine zusätzlichen Status-Boxen einbauen.
- Die Historie wird trotzdem **lückenlos aufbewahrt**: neue Ereignisse als zusätzlichen Eintrag anfügen, alte Einträge nie löschen oder überschreiben (ausser bei sachlichen Korrekturen).
- Kurzbefehl: Sagt die Herausgeberin z. B. „Status aktualisieren“ oder „Nationalrat hat zugestimmt“, ist genau das folgende Vorgehen gemeint.

Die Fortschrittsleiste ergibt sich automatisch aus dem **neuesten** Eintrag in `verlauf.eintraege` (nach `datum` sortiert). Aufgeklappt zeigt die Box zuerst diesen neuesten Eintrag ausführlich, darunter die älteren als Zeitstrahl.

Neues Ereignis erfassen:
1. Fakten an offizieller Quelle prüfen (parlament.ch, Geschäft **26.023**; admin.ch-Medienmitteilungen).
2. **Neuen Eintrag hinzufügen** – bestehende Einträge nie überschreiben oder löschen (Korrekturen ausgenommen). `titel` und `text` immer in allen vier Sprachen.
3. `etappe` = ID der Etappe, um die es geht (`vernehmlassung`, `botschaft`, `staenderat`, `nationalrat`, `differenzen`, `schlussabstimmung`, `volksabstimmung`); `erledigt: true`, wenn die Etappe damit abgeschlossen ist – dann springt die Leiste zur nächsten.
4. Entfällt eine Etappe (z. B. keine Differenzen zwischen den Räten), ihre ID in `verlauf.entfallen` eintragen.
5. `meta.stand` auf das Datum der Aktualisierung setzen.
