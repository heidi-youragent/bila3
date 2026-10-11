# bila3 – Paket Schweiz–EU (Bilaterale III) im Überblick

Neutrale, mobile-taugliche Informationsplattform zum Verhandlungspaket Schweiz–EU (Bilaterale III). Die rund 2'000 Seiten der Botschaft des Bundesrats werden nach Themen aufbereitet: Ziel, heutige Lage, Chancen, Risiken, Auswirkungen auf Kantone und Gemeinden sowie die Positionen von Bundesrat, Kantonen (KdK) und Parteien.

## Aufbau

| Datei / Ordner | Inhalt |
|---|---|
| `index.html` | Die Oberfläche (Tailwind CSS via CDN, kein Build-Schritt) |
| `themen.json` | **Alle Inhalte**: Themen, Argumente, Positionen, Quellenangaben, UI-Texte |
| `assets/Botschaft_DE.pdf`, `_FR.pdf`, `_IT.pdf` | Botschaft des Bundesrats in den drei Amtssprachen, die inhaltliche Grundlage |
| `LICENSE`, `LICENSE-INHALTE.md` | Lizenzen: Code MIT, Inhalte CC BY 4.0 |
| `AGENTS.md` | Arbeitsregeln für AI-Assistenten und Mitarbeitende (`CLAUDE.md` verweist darauf) |

Die Plattform ist viersprachig (DE, FR, IT, EN). Deutsch ist die Referenz; FR und IT folgen der Terminologie der offiziellen Botschaft, EN ist eine eigene Übersetzung (keine offizielle englische Fassung vorhanden).

Oberfläche und Daten sind strikt getrennt: Inhaltliche Änderungen werden ausschliesslich in `themen.json` gemacht, `index.html` bleibt davon unberührt.

## Lokal ansehen

`index.html` lädt `themen.json` per `fetch`. Das funktioniert nicht beim direkten Öffnen der Datei (`file://`), sondern braucht einen kleinen Webserver – in VS Code am einfachsten mit der Erweiterung **Live Server** („Go Live“ unten rechts, dann http://localhost:5500), oder im Terminal:

```bash
python3 -m http.server 8000
# dann im Browser: http://localhost:8000
```

## Veröffentlichung

Die Seite läuft über **GitHub Pages** unter **https://bila3.zuri.se**. Jeder Push auf `main` wird automatisch veröffentlicht (nach ca. 1–2 Minuten).

- `CNAME` enthält die eigene Domain für GitHub Pages.
- `.nojekyll` sorgt dafür, dass GitHub die Dateien unverändert ausliefert.
- DNS bei Hoststar (Domain zuri.se): `CNAME`-Eintrag `bila3` → `heidi-youragent.github.io`.

## Grundsätze

- **Neutralität:** Chancen und Risiken werden gleichwertig dargestellt, Parteipositionen mit Quelle wiedergegeben.
- **Kein Cherry-Picking:** Jedes Abkommen ist klar gekennzeichnet als Teil des **Hauptpakets** (nicht separat abwählbar) oder als **optionales Paket** (separates Referendum möglich).
- **Quellen:** Jede Aussage verweist auf das entsprechende Kapitel der Botschaft.

## Quellen

- Botschaft zum Paket Schweiz–EU: [europa.eda.admin.ch](https://www.europa.eda.admin.ch/de/paket-schweiz-eu)
- Portal des Bundes: [admin.ch/de/paket-schweiz-eu](https://www.admin.ch/de/paket-schweiz-eu)

Die Botschaft ist ein amtliches Dokument und nach Art. 5 URG urheberrechtlich nicht geschützt.

## Lizenz

- **Inhalte** (Texte in `themen.json`): [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.de) – weiterverwenden erlaubt mit Quellenangabe „bila3 – https://bila3.zuri.se“. Details: [LICENSE-INHALTE.md](LICENSE-INHALTE.md)
- **Code** (`index.html`): [MIT](LICENSE)
- **Botschaft des Bundesrats** (`assets/`): amtliches Werk, nicht urheberrechtlich geschützt (Art. 5 URG)
