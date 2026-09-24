# RoKo (1790-1915)


RoKo is a corpus of German-language novels from the period 1790–1915. It was compiled as part of a doctoral dissertation in literary studies and contains 1,975 text files by 582 authors. Counting multi-volume novels as a single title, the corpus comprises 1,674 works. The corpus is divided into two sub-corpora: a **core corpus** (669 entries) and an **extension corpus** (1,306 entries). In total, RoKo contains almost 163 million tokens (excluding punctuation).

---

## Contents of this repository

| File/Folder | Description |
|---|---|
| `metadata.csv` | Metadata for all 1,975 entries (UTF-8, comma-separated) |
| `texts/` | Full texts as `.txt` files (UTF-8), filename = `id` |

---

## Corpus design

### Concept of the novel

RoKo uses a **historical concept of the novel**: what matters is not an abstract, timeless definition, but what was published under the generic label "novel" (*Roman*) at the respective historical moment within the period under study. The decisive criterion is the genre label of the first publication. This approach follows the genre-theoretical framework of Julian Schröter, who models genre concepts as historically variable and structured along prototype semantics.

The corpus treats four features as central to the novel of the long 19th century: fictionality, narrativity, prose form, and sufficient length. The minimum text length is an empirically determined threshold of **32,000 tokens** (excluding punctuation), derived from a comparative analysis of text-length distributions in the core corpus and in a corpus of novellas (Schröter 2026).

### Two-part structure

The corpus is divided into two sub-corpora:

- **Core corpus:** texts that were explicitly published as novels – i.e. that carry the generic label "Roman" (or a subgenre designation such as "historical novel", "epistolary novel", etc.) in the paratext. The core corpus serves as the empirical basis for determining the historical prototype of the genre.

- **Extension corpus:** texts that were not published under this label, or were published under a related label ("novella", "story", etc.), but which are close enough in their features to the prototype defined by the core corpus to count as part of the contemporary landscape of novel production. These are, accordingly, fictional, narrative prose texts of at least 32,000 tokens.

---

## Corpus construction

### Corpus basis

Four existing collections served as the starting point for corpus construction, from which candidate lists were drawn:

| Basis | Entries | Description |
|---|---|---|
| DeCorp | 1,558 | Corpus of German-language literature with a sub-corpus of fictional texts, based on Gutenberg-DE and Gutenberg-US (Rohrbacher 2025) |
| TextGrid Digital Library | 230 | [textgridrep.org](https://textgridrep.org) – digitized full texts, based on the zeno.org library |
| Gutenberg supplement | 226 | Texts from Project Gutenberg-DE missing from DeCorp |
| Bildungsroman corpus | 19 | Curated collection of German-language so-called *Bildungsromane* (Boucher et al. 2024) |

*(The entries add up to more than 1,975, since some works appeared in several collections and were deduplicated.)*

### Full-text sources

The actual provenance of the text files – i.e. which digital archive provides the full text – is documented in the metadata column `fulltext_source`:

| Source | Entries | URL |
|---|---|---|
| Project Gutenberg-DE | 1,573 | [projekt-gutenberg.org](https://www.projekt-gutenberg.org) |
| TextGrid Repository | 224 | [textgridrep.org](https://textgridrep.org) |
| Project Gutenberg-US | 148 | [gutenberg.org](https://www.gutenberg.org) |
| Deutsches Textarchiv (DTA) | 22 | [deutschestextarchiv.de](https://www.deutschestextarchiv.de) |
| Other | 8 | BYU Scholars Archive, BSB, and others |

### Procedure

#### Step 1: Candidate lists from the source collections

For each source collection, the available metadata was extracted and filtered. Novel candidates for the core corpus were identified via two signals: (1) novel designations in the title (regex-based) and (2) novel designations at the start of the text (first 2,500 characters). The resulting candidate lists were checked manually and annotated with decision votes.

#### Step 2: Core and extension corpus

For the **core corpus**, the rule was: include if a text was published as a novel (generic label in the paratext). For the **extension corpus**, the rule was: include if a text meets the features of the novel prototype despite a missing or differently worded generic label. The decision was based on a combination of automatic pre-selection and manual review.

For multi-volume novels: if one volume of a series is in the corpus, all remaining volumes of the same series are automatically added.

#### Step 3: Merging into the overall corpus

The core corpus and the extension corpus were merged into an overall corpus, and duplicates were removed.

#### Step 4: Cleaning

Metadata and holdings were extensively cleaned in several steps. Given the sheer volume of data, some errors are still to be expected.

#### Step 5: Work IDs and filenames

Each work is assigned a unique **work ID** following the scheme `Surname_ShortTitle` (with an appended volume number for multi-volume works). The surname follows the GND authority name; the short title is derived from the capitalized content words of the main title. Examples:

- *Der falsche Woldemar* (Willibald Alexis) → `Alexis_Woldemar`
- *Das Landhaus am Rhein, Band V* (Berthold Auerbach) → `Auerbach_Landhaus_5`
- *Leute aus dem Walde* (Wilhelm Raabe) → `Raabe_Leute`

The full-text files are stored under this ID in the `texts/` folder.

---

## Corpus description

### Scope

- **1,975 entries** (text files), of which 669 are core corpus and 1,306 extension corpus
- **1,674 works** (multi-volume novels counted as one title)
- **582 authors** (identified via GND IDs)
- Period: **1790–1915**
- Token count: **~163 million tokens** (excluding punctuation, tokenized with spaCy `de_core_news_sm`)
- Mean text length: ~82,700 tokens; median: ~67,700 tokens

### Authors

Of the 582 authors, according to GND, 117 are female (approx. 20%) and 369 male (approx. 63%); for the remainder, no gender entry is available in the GND. The distribution of works across authors is strongly right-skewed: 47% of authors are represented with only a single work. The most productive authors are Karl May (68 works), Friedrich Gerstäcker (42), and Friedrich Spielhagen (40).

### Temporal distribution

The number of texts increases over the period under study, with marked growth from the mid-19th century onward. The most productive decade is the 1900s. The earliest decades (1790–1819) are more weakly represented, with 112 titles.

---

## Metadata (`metadata.csv`)

The metadata file contains one row per text file (entry) with the following columns:

| Column | Description |
|---|---|
| `id` | Unique work ID, corresponds to the filename (without `.txt`) |
| `subcorpus` | `Kernkorpus` (core corpus) or `Erweiterungskorpus` (extension corpus) |
| `author` | GND authority name of the author |
| `author_gnd_id` | GND ID of the author |
| `title` | Work title (original spelling) |
| `title_normalised` | Normalised work title (without volume information) |
| `volume_number` | Volume number (empty for single-volume works) |
| `follows_after` | `id` of the preceding volume (for multi-volume works) |
| `year` | Year of first publication |
| `year_source` | Source of the year (if manually corrected) |
| `n_tokens` | Token count including punctuation (spaCy `de_core_news_sm`) |
| `n_tokens_no_punct` | Token count excluding punctuation |
| `author_gnd_gender` | Gender according to GND (`male`, `female`, `unbekannt` [unknown]) |
| `author_gnd_birth_year` | Year of birth according to GND |
| `author_gnd_death_year` | Year of death according to GND |
| `gnd_geo` | Geographic association according to GND |
| `base` | Source collection (DeCorp, TextGrid, GB supplement, Bildungsroman, DTA) |
| `fulltext_source` | Primary text source (PG-DE, TextGrid, PG-US, DTA, etc.) |
| `filename_orig` | Original filename in the source collection |
| `anmerkung` | Manual annotations (filled for 215 of the 1,975 entries), mostly bibliographic information on the first print (place, publisher, year), and occasionally notes on incomplete sources, publication history (e.g. prior serialisation in a journal, censorship), or name inconsistencies within the text |

---

## Text files

All texts are provided as UTF-8-encoded plain text files (`.txt`). Normalisation includes:

- Extraction of the running text from the source formats (HTML for Gutenberg, XML/TEI for TextGrid and DTA)
- Removal of metadata, tables of contents, editorial apparatus, and paratexts as far as possible
- Normalisation of Fraktur OCR artefacts: `ſ` → `s`, `ʒ` → `z`, vowel + combining e → umlaut
- Normalisation of whitespace (narrow no-break spaces, non-breaking spaces)
- Tokenisation with spaCy (`de_core_news_sm`)

---

## References


---

## Citation

If you use RoKo in your research, please cite:

> ...
(also cite DeCorp and BK?)

---

## License

The metadata (`metadata.csv`) is available under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The full texts are subject to the respective licence terms of the source collections. Texts from Project Gutenberg-US and Projekt Gutenberg-DE are in the public domain or available under the Project Gutenberg License; TextGrid texts are available under CC BY 3.0. Texts from the DTA are subject to the terms of use of the Deutsches Textarchiv.

---

## Changelog
