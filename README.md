# Kurswahl verteilen

Ein schlankes Werkzeug, um Kurswahlen an Schulen fair zu verteilen – zum Beispiel Sportkurse, Wahlpflichtkurse, AGs oder Projektwochen. Die Wünsche werden über eine normale **itslearning-Umfrage** gesammelt, das Tool verteilt die Plätze und erstellt fertige Kurs- und Klassenlisten.

Das gesamte Werkzeug ist **eine einzige HTML-Datei**. Es braucht keinen Server, keine Installation und keine Anmeldung und läuft im Browser auf Mac, PC und iPad – auch offline.

![Ergebnis einer Verteilung mit Beispieldaten](docs/screenshot-verteilung.png)

## Schnellstart

1. `kurswahl-verteilen.html` herunterladen und im Browser öffnen.
2. „Mit Beispieldaten ausprobieren“ anklicken – oder die Dateien aus `testdaten/` laden.
3. Kurse, Plätze und Regeln einstellen, „Kurse verteilen“, Ergebnis als Excel sichern.

## Online nutzen

Über GitHub Pages lässt sich das Tool direkt im Browser öffnen, ohne Download. Die Startseite ist `index.html`. Auch online werden alle Daten nur lokal im Browser verarbeitet; beim Aufruf der Seite erfährt GitHub als Anbieter allerdings die IP-Adresse. Wer das vermeiden will, nutzt die heruntergeladene Datei oder legt sie auf einen schulischen Server.

Eine ausführliche Anleitung von der itslearning-Umfrage bis zu den Listen steht in [`docs/anleitung-itslearning.md`](docs/anleitung-itslearning.md).

## Ablauf in der Schule

1. **Umfrage in itslearning** anlegen: eine Matrixfrage mit den Spalten „1. Wahl“, „2. Wahl“, „3. Wahl“ und den Kursen als Optionen. Für mehrere Halbjahre in einem Durchgang je Halbjahr eine Matrixfrage. Empfohlen: zusätzlich eine Frage „Klasse“.
2. **Export** der Ergebnisse als Excel-Datei.
3. **Einlesen** im Tool, dazu optional:
   - **Klassenlisten** aus der Schulverwaltung (eine oder mehrere Dateien, z. B. eine pro Klasse; Excel oder CSV, auch UTF-16/tabulatorgetrennt). Genutzt werden Name, Klasse und – empfohlen – Username.
   - **Historie** aus früheren Runden (die CSV, die das Tool selbst exportiert).
   - **Schulprofil** mit Kursen, Plätzen, Kategorien und Regeln (JSON, enthält keine Schülerdaten).
4. **Verteilen und prüfen**, bei Bedarf einzelne Zuteilungen von Hand ändern.
5. **Exportieren**: Excel mit Übersicht, einem Blatt pro Kurs (für die Kursleitungen), einem Blatt pro Klasse (für die Klassenleitungen), Kategorien-Übersicht, Hinweisen und Protokoll – plus die fortlaufende Historie als CSV für das nächste Halbjahr.

## Wie verteilt wird

Jede Zuteilung bekommt Strafpunkte: 1. Wunsch 0, 2. Wunsch 3, 3. Wunsch 7, Restplatz 40. Gesucht wird die Verteilung mit den wenigsten Punkten für alle zusammen (Min-Cost-Flow, exakt und in Millisekunden). Name, Reihenfolge in der Datei und Abgabezeit spielen keine Rolle.

Einstellbare Regeln:

| Regel | Wirkung |
|---|---|
| Kurs nicht wiederholen | Niemand bekommt einen Kurs, den er schon hatte. |
| Kategorie begrenzen | Höchstens x Kurse pro Kategorie (z. B. Bewegungsfeld). |
| Vielfalt vor Wiederholung | Eine Kategorie erst wieder nach x verschiedenen Kategorien. |
| Kein Vorteil durch Mehrfachnennung | Dreimal denselben Kurs anzukreuzen verschafft keinen Vorrang. |
| Ausgleich bei Gleichstand | Wer zuletzt nur den 3. Wunsch oder einen Restplatz bekam, ist bei Gleichstand zuerst dran. |
| Restplätze vergeben | Wer keinen Wunsch bekommt, landet in einem Kurs mit freien Plätzen. |

Bei echten Gleichständen entscheidet das Los. Die **Losnummer** wird angezeigt: Mit denselben Dateien, Einstellungen und derselben Nummer entsteht exakt dieselbe Verteilung. Das Protokoll-Blatt im Export dokumentiert Regeln, Losnummern, jede manuelle Änderung und jede Namenszuordnung.

## Namen abgleichen

- Abweichende Schreibweisen (z. B. „Juna“ / „Juna-Marie“, „Lüders“ / „Lueders“) schlägt das Tool zur Prüfung vor; ein Mensch entscheidet.
- Gleiche Namen werden nie still zusammengeführt: Das Tool fragt „eine Person oder zwei?“ und lässt die Einträge der Klassenliste von Hand zuordnen.
- Mit einer **Username-Spalte** in der Klassenliste (und idealerweise im Umfrage-Export) ist die Zuordnung eindeutig; die Historie läuft dann über den Benutzernamen.

## Datenschutz

- Alle Daten werden **ausschließlich lokal im Browser** verarbeitet. Die Datei baut keine Verbindung nach außen auf; die Excel-Bibliothek ist eingebettet, es werden keine Schriften oder Skripte nachgeladen.
- Es wird nichts im Browser gespeichert. Ergebnisse existieren nur als Dateien, die man selbst exportiert.
- Aus Klassenlisten werden nur Name, Klasse und Username/E-Mail gelesen. Weitere Spalten (z. B. Personalnummern) werden ignoriert und nicht exportiert.
- Exportierte Ergebnislisten und die Historie-CSV enthalten personenbezogene Daten und gehören auf Dienstgeräte bzw. in die schulische Ablage – **nicht in dieses Repository**. Die `.gitignore` schließt typische Exportdateien und einen Ordner `echtdaten/` aus.

## Testdaten

Alle Dateien in `testdaten/` sind **frei erfunden**:

| Datei | Inhalt |
|---|---|
| `testdaten-wahl-hj1.xlsx`, `testdaten-wahl-hj2.xlsx` | itslearning-Exporte für zwei Halbjahre, 2. Halbjahr mit neuen Kursen |
| `testdaten-wahl-export.xlsx` | ein Export mit zwei Halbjahren in einer Umfrage, inklusive Stolpersteinen |
| `testdaten-klassenliste.xlsx` | Klassenliste 10a–10c (Nachname, Vorname, Klasse) |
| `testdaten-schulverwaltung-10a/b/c.csv` | dieselben Klassen im Format der Schulverwaltung (UTF-16, Tabulator, mit Username) |
| `testdaten-historie.csv` | Kurse aus Jahrgang 9 |
| `testdaten-schulprofil.json` | Kurse, Plätze, Kategorien und Regeln |

## Status

Prototyp. Vor dem Einsatz an einer Schule bitte mit der Schulleitung und den Datenschutzverantwortlichen klären. Entstanden im Dialog mit einer KI (Claude) im Team digitale Schulentwicklung, Referat 10, Senator für Kinder und Bildung Bremen.

## Lizenzen

Das Tool steht unter der MIT-Lizenz (siehe `LICENSE`) und darf von Schulen frei genutzt, angepasst und weitergegeben werden. Eingebettet ist [SheetJS Community Edition](https://sheetjs.com) 0.18.5 (Apache License 2.0), siehe `THIRD_PARTY_NOTICES.md`.
