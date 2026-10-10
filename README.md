# bila3 – Paket Schweiz–EU (Bilaterale III) im Überblick

Neutrale, mobile-taugliche Informationsplattform zum Verhandlungspaket Schweiz–EU (Bilaterale III). Die rund 2'000 Seiten der Botschaft des Bundesrats werden nach Themen aufbereitet: Ziel, heutige Lage, Chancen, Risiken, Auswirkungen auf Kantone und Gemeinden sowie die Positionen von Bundesrat, Kantonen (KdK) und Parteien.

## Aufbau

| Datei / Ordner | Inhalt |
|---|---|
| `index.html` | Die Oberfläche (Tailwind CSS via CDN, kein Build-Schritt) |
| `themen.json` | **Alle Inhalte**: Themen, Argumente, Positionen, Quellenangaben, UI-Texte |
| `assets/Botschaft_DE.pdf` | Botschaft des Bundesrats (deutsch), die inhaltliche Grundlage |
| `CLAUDE.md` | Arbeitsregeln für die Weiterentwicklung mit Claude Code |

Oberfläche und Daten sind strikt getrennt: Inhaltliche Änderungen werden ausschliesslich in `themen.json` gemacht, `index.html` bleibt davon unberührt.

## Lokal ansehen

`index.html` lädt `themen.json` per `fetch`. Das funktioniert nicht beim direkten Öffnen der Datei (`file://`), sondern braucht einen kleinen Webserver:

```bash
python3 -m http.server 8000
# dann im Browser: http://localhost:8000
```

## Grundsätze

- **Neutralität:** Chancen und Risiken werden gleichwertig dargestellt, Parteipositionen mit Quelle wiedergegeben.
- **Kein Cherry-Picking:** Jedes Abkommen ist klar gekennzeichnet als Teil des **Hauptpakets** (nicht separat abwählbar) oder als **optionales Paket** (separates Referendum möglich).
- **Quellen:** Jede Aussage verweist auf das entsprechende Kapitel der Botschaft.

## Quellen

- Botschaft zum Paket Schweiz–EU: [europa.eda.admin.ch](https://www.europa.eda.admin.ch/de/paket-schweiz-eu)
- Portal des Bundes: [admin.ch/de/paket-schweiz-eu](https://www.admin.ch/de/paket-schweiz-eu)

Die Botschaft ist ein amtliches Dokument und nach Art. 5 URG urheberrechtlich nicht geschützt.
