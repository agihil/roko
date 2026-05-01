# RoKo – Romankorpus deutschsprachiger Romane des langen 19. Jahrhunderts

\[später englisch]

RoKo ist ein Korpus deutschsprachiger Romane aus dem Zeitraum 1790–1915. Es wurde im Rahmen einer literaturwissenschaftlichen Dissertation erstellt und enthält 1.975 Textdateien von 582 Autor:innen. Zählt man mehrbändige Romane als einen Titel, umfasst das Korpus 1.674 Werke. Das Korpus ist in zwei Teilkorpora gegliedert: ein **Kernkorpus** (669 Einträge) und ein **Erweiterungskorpus** (1.306 Einträge). Insgesamt enthält RoKo fast 163 Millionen Token (ohne Interpunktionszeichen).

---

## Inhalt dieses Repositorys

| Datei/Ordner | Beschreibung |
|---|---|
| `metadaten.csv` | Metadaten zu allen 1.975 Einträgen (UTF-8, Komma-separiert) |
| `texts/` | Volltexte als `.txt`-Dateien (UTF-8), Dateiname = `werk_id` |

---

## Korpuskonzeption

### Romanbegriff

RoKo verwendet einen **historischen Romanbegriff**: Maßgeblich ist nicht eine abstrakte, zeitlose Definition, sondern das, was im jeweiligen historischen Moment des Untersuchungszeitraums mit dem Gattungslabel »Roman« veröffentlicht wurde. Als entscheidendes Kriterium gilt das Label der Erstpublikation. Dieser Ansatz orientiert sich am gattungstheoretischen Framework von Julian Schröter, der Gattungsbegriffe als historisch variabel und prototypensemantisch strukturiert modelliert.

Das Korpus setzt vier Merkmale als zentral für den Roman des langen 19. Jahrhunderts: Fiktionalität, Narrativität, Prosaform und ausreichender Umfang. Als Mindesttextlänge gilt ein empirisch ermittelter Schwellenwert von **32.000 Token** (ohne Interpunktionszeichen), der durch eine vergleichende Analyse der Textlängenverteilungen im Kernkorpus und in einem Novellenkorpus (Schröter 2026) gewonnen wurde.

### Zweiteilige Struktur

Das Korpus ist in zwei Teilkorpora gegliedert:

- **Kernkorpus:** Texte, die explizit als Romane publiziert wurden – d.h. die das Gattungslabel »Roman« (oder eine Subgattungsbezeichnung wie »historischer Roman«, »Briefroman« etc.) im Paratext tragen. Das Kernkorpus dient als empirische Grundlage für die Bestimmung des historischen Prototypen der Gattung.

- **Erweiterungskorpus:** Texte, die zwar nicht oder unter einem verwandten Label (»Novelle«, »Erzählung« u.ä.) publiziert wurden, die aber hinsichtlich ihrer Merkmale nah genug am durch das Kernkorpus definierten Prototypen liegen, um als Teil der zeitgenössischen Romanlandschaft gelten zu können. Es handelt sich also um fiktionale, narrative Prosatexte von mindestens 32.000 Token.

---

## Korpuserstellung

### Korpusbasis

Als Ausgangspunkt der Korpuserstellung dienten vier bereits bestehende Sammlungen, aus denen Kandidatenlisten gewonnen wurden:

| Basis | Einträge | Beschreibung |
|---|---|---|
| DeCorp | 1.558 | Korpus deutschsprachiger Literatur mit Teilkorpus fiktionaler Texte, basierend auf Gutenberg-DE und Gutenberg-US (Rohrbacher 2025) |
| TextGrid digitale Bibliothek | 230 | [textgridrep.org](https://textgridrep.org) – digitalisierte Volltexte, basierend auf der zeno.org Bibliothek |
| Gutenberg-Ergänzung | 226 | Texte von Projekt Gutenberg-DE, die in DeCorp fehlten |
| Bildungsromankorpus | 19 | Kuratierte Sammlungen deutschsprachiger sog. Bildungsromane (Boucher et al. 2024) |

*(Die Einträge summieren sich auf mehr als 1.975, da einige Werke in mehreren Sammlungen vorkamen und dedupliziert wurden.)*

### Volltextquellen

Die tatsächliche Herkunft der Textdateien – d.h. welches digitale Archiv den Volltext bereitstellt – ist in der Metadatenspalte `quelle_detail` dokumentiert:

| Quelle | Einträge | URL |
|---|---|---|
| Projekt Gutenberg-DE | 1.573 | [projekt-gutenberg.org](https://www.projekt-gutenberg.org) |
| TextGrid Repository | 224 | [textgridrep.org](https://textgridrep.org) |
| Project Gutenberg-US | 148 | [gutenberg.org](https://www.gutenberg.org) |
| Deutsches Textarchiv (DTA) | 22 | [deutschestextarchiv.de](https://www.deutschestextarchiv.de) |
| Sonstige | 8 | BYU Scholars Archive, BSB u.a. |

### Vorgehen

#### Schritt 1: Kandidatenlisten aus den Quellbeständen

Für jeden Quellbestand wurden die verfügbaren Metadaten ausgelesen und gefiltert. Romankandidaten für das Kernkorpus wurden über zwei Signale identifiziert: (1) Roman-Bezeichnungen im Titel (Regex-basiert) und (2) Roman-Bezeichnungen im Textanfang (erste 2.500 Zeichen). Die resultierenden Kandidatenlisten wurden manuell geprüft und mit Entscheidungsvoten versehen.

#### Schritt 2: Kern- und Erweiterungskorpus

Beim **Kernkorpus** galt: Aufnahme, wenn ein Text als Roman publiziert wurde (Gattungslabel im Paratext). Beim **Erweiterungskorpus** galt: Aufnahme, wenn ein Text trotz fehlendem oder anderslautendem Gattungslabel die Merkmale des Romanprototypen erfüllt. Die Entscheidung beruhte auf einer Kombination aus automatischer Vorauswahl und manueller Prüfung.

Für mehrbändige Romane gilt: Ist ein Band einer Reihe im Korpus, werden alle übrigen Bände derselben Reihe automatisch ergänzt.

#### Schritt 3: Zusammenführung zum Gesamtkorpus

Kernkorpus und Erweiterungskorpus wurden zu einem Gesamtkorpus zusammengeführt und Duplikate entfernt.

#### Schritt 4: Bereinigung

Metadaten und Bestand wurden umfassend in mehreren Schritten gesäubert. Bei der schieren Menge der Daten ist trotzdem noch mit Fehlern zu rechnen.

#### Schritt 5: Werk-IDs und Dateinamen

Jedes Werk erhält eine eindeutige **Werk-ID** nach dem Schema `Nachname_Kurztitel` (bei mehrbändigen Werken mit angehängter Bandnummer). Der Nachname folgt dem GND-Normnamen; der Kurztitel ergibt sich aus den großgeschriebenen Inhaltswörtern des Haupttitels. Beispiele:

- *Der falsche Woldemar* (Willibald Alexis) → `Alexis_Woldemar`
- *Das Landhaus am Rhein, Band V* (Berthold Auerbach) → `Auerbach_Landhaus_5`
- *Leute aus dem Walde* (Wilhelm Raabe) → `Raabe_Leute`

Die Volltext-Dateien sind unter dieser ID im Ordner `texts/` abgelegt.

---

## Korpusbeschreibung

### Umfang

- **1.975 Einträge** (Textdateien), davon 669 Kern- und 1.306 Erweiterungskorpus
- **1.674 Werke** (mehrbändige Romane als ein Titel gezählt)
- **582 Autor:innen** (identifiziert über GND-IDs)
- Zeitraum: **1790–1915**
- Tokenumfang: **~163 Millionen Token** (ohne Interpunktionszeichen, tokenisiert mit spaCy `de_core_news_sm`)
- Mittlere Textlänge: ~82.700 Token; Median: ~67.700 Token

### Autor:innen

Von den 582 Autor:innen sind laut GND 117 weiblich (ca. 20%) und 369 männlich (ca. 63%); für die übrigen liegt kein Gendereintrag in der GND vor. Die Verteilung der Werke auf die Autor:innen ist stark rechtschief: 47% der Autor:innen sind mit nur einem einzigen Werk vertreten. Die produktivsten Autor:innen sind Karl May (68 Werke), Friedrich Gerstäcker (42) und Friedrich Spielhagen (40).

### Zeitliche Verteilung

Die Zahl der Texte steigt über den Untersuchungszeitraum an, mit einem deutlichen Wachstum ab der Mitte des 19. Jahrhunderts. Die produktivste Dekade ist die 1900er Jahre. Die ersten Dekaden (1790–1819) sind mit 112 Titeln schwächer vertreten.

---

## Metadaten (`metadaten.csv`)

Die Metadatendatei enthält eine Zeile pro Textdatei (Eintrag) mit folgenden Spalten:

| Spalte | Beschreibung |
|---|---|
| `werk_id` | Eindeutige ID, entspricht dem Dateinamen (ohne `.txt`) |
| `basis` | Quellbestand (DeCorp, TextGrid, GB-Ergänzung, GB, Bildungsroman) |
| `teilkorpus` | `Kernkorpus` oder `Erweiterungskorpus` |
| `gnd_pref_name` | GND-Normname der Autorin/des Autors |
| `gnd_id` | GND-ID der Autorin/des Autors |
| `title` | Werktitel (Originalschreibung) |
| `title_normalisiert` | Normalisierter Werktitel (ohne Bandangabe) |
| `volume_number` | Bandnummer (leer bei einbändigen Werken) |
| `follows_after` | `werk_id` des vorherigen Bandes (bei mehrbändigen Werken) |
| `year` | Erstveröffentlichungsjahr |
| `year_source` | Quelle der Jahresangabe (wenn manuell korrigiert) |
| `n_tokens` | Tokenanzahl inkl. Interpunktion (spaCy `de_core_news_sm`) |
| `n_tokens_no_punct` | Tokenanzahl ohne Interpunktionszeichen |
| `gnd_gender` | Gender laut GND (`m` = männlich, `w`/`f` = weiblich) |
| `gnd_birth_year` | Geburtsjahr laut GND |
| `gnd_death_year` | Todesjahr laut GND |
| `gnd_geo` | Geografischer Bezug laut GND |
| `quelle_detail` | Primäre Textquelle (PG-DE, TextGrid, PG-US, DTA usw.) |
| `filename_orig` | Originaldateiname in der Quellsammlung |
| `anmerkung` | Manuelle Anmerkungen |

---

## Textdateien

Alle Texte liegen als UTF-8-kodierte Klartextdateien (`.txt`) vor. Die Normalisierung umfasst:

- Extraktion des Fließtexts aus den Quellformaten (HTML bei Gutenberg, XML/TEI bei TextGrid und DTA)
- Entfernung von Metadaten, Inhaltsverzeichnissen, editorischen Apparaten und Paratexten soweit möglich
- Normalisierung von Fraktur-OCR-Artefakten: `ſ` → `s`, `ʒ` → `z`, Vokal + Combining-e → Umlaut
- Normalisierung von Leerzeichen (schmale geschützte Leerzeichen, Non-breaking spaces)
- Tokenisierung mit spaCy (`de_core_news_sm`)

---

## Literaturangaben


---

## Zitation

Wenn Sie RoKo in Ihrer Forschung verwenden, zitieren Sie bitte:

> ...
(auch decorp und BK zitieren?

---

## Lizenz

Die Metadaten (`metadaten.csv`) stehen unter [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Die Volltexte unterliegen den jeweiligen Lizenzbedingungen der Quellbestände. Texte aus Project Gutenberg-US und Projekt Gutenberg-DE sind gemeinfrei oder unter der Project Gutenberg License verfügbar; TextGrid-Texte stehen unter CC BY 3.0. Für Texte aus dem DTA gelten die Nutzungsbedingungen des Deutschen Textarchivs.

---

## Changelog

