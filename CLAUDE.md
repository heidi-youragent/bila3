# Projekt: bila3 (Bilaterale III Informationsplattform)

## Projektziel
`bila3` ist eine neutrale, mobile-responsive Webplattform zur Information der Schweizer Bevölkerung über das Verhandlungspaket Schweiz–EU (Bilaterale III). Die Plattform bereitet die ca. 2'000 Seiten umfassende Botschaft des Bundesrats transparent und übersichtlich auf.

## Architektur & Prinzipien
- **Decoupled Architecture:** Die UI (`index.html`) ist vollständig von den Daten (`themen.json`) getrennt.
- **Data Policy:** Alle inhaltlichen Anpassungen, Argumente, Parteipositionen und Quellenangaben werden **ausschliesslich** in der `themen.json` gepflegt.
- **Design:** Tailwind CSS via CDN, neutral, übersichtlich und mobile-first.
- **Keine Desinformation/Cherry-Picking:** Klare Kennzeichnung von Verträgen als "Hauptpaket" (nicht separat abwählbar) vs. "Optionales Paket" (separates Referendum möglich).

## Datenstruktur (`themen.json`)
Jedes Thema in der `themen.json` muss zwingend folgender Struktur folgen:

```json
{
  "id": "eindeutige-id-in-kebab-case",
  "titel": "Titel des Themas",
  "paket": "hauptpaket" | "optionales_paket",
  "kategorien_branchen": ["kmu", "gemeinden", "exportindustrie", etc.],
  "kategorien_werte": ["souveraenitaet", "direkte_demokratie", "foederalismus", "finanzen"],
  "tags": ["#Souveränität", "#EuGH"],
  "ziel": "Kurze Zusammenfassung des Hauptziels",
  "status_quo": "Aktuelle rechtliche/politische Lage",
  "nutzen_pro": [
    "Chancen und Nutzen aus Schweizer Sicht (Punkt 1)",
    "Chancen und Nutzen aus Schweizer Sicht (Punkt 2)"
  ],
  "risiken_contra": [
    "Kompromisse, Risiken oder Bürokratie (Punkt 1)",
    "Kompromisse, Risiken oder Bürokratie (Punkt 2)"
  ],
  "foederalismus_check": "Konkrete Auswirkungen auf Kantone und Gemeinden",
  "positionen": {
    "bundesrat": {"haltung": "ja", "kommentar": "..."},
    "kdk": {"haltung": "ja_mit_auflagen" | "nein" | "keine_position", "kommentar": "..."},
    "svp": {"haltung": "nein", "kommentar": "..."},
    "sp": {"haltung": "ja_mit_auflagen", "kommentar": "..."},
    "fdp": {"haltung": "ja", "kommentar": "..."},
    "mitte": {"haltung": "ja_mit_auflagen", "kommentar": "..."},
    "glp": {"haltung": "ja", "kommentar": "..."},
    "gruene": {"haltung": "ja_mit_auflagen", "kommentar": "..."}
  },
  "quelle": "Botschaft Paket Schweiz–EU, Kapitel X.X (S. XXX–XXX)"
}