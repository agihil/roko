# RoKo – A Large Corpus of German-language Novels (1790–1915)

RoKo is a corpus of German-language novels from the period 1790–1915, compiled as part of a PhD in *Computational Literary Studies*. It contains 1,834 texts by 545 authors, with almost 153 million tokens (excluding punctuation).

RoKo is not yet perfect, and neither is this documentation. The background to specific decisions and detailed information on the individual steps of the procedure will be given in my dissertation. If anything is unclear in the meantime, please feel free to contact me (agnes.hilger@uni-wuerzburg.de).

## Citation

If you use RoKo in your research, please cite it as follows:

Hilger, Agnes: RoKo – A Large Corpus of German-language Novels (1790–1915), version 1.0, 2026. Zenodo. https://doi.org/[DOI]

Please also acknowledge the sources RoKo draws on: Projekt Gutenberg-DE, Project Gutenberg, the Deutsches Textarchiv (DTA), TextGrid, and the following corpora:

Boucher, Marie-Christine, J. Berenike Herrmann, Alexander Hummel, and James Wiebe. „Bildungsromankorpus“. Zenodo, 2024. https://doi.org/10.5281/ZENODO.14289198.

Rohrbacher, Katrin. „De-Corp: A Corpus of German-Language Fiction and Non-Fiction (1780–1930)“. Journal of Open Humanities Data 11, no. 1 (2025). https://doi.org/10.5334/johd.350.

---

## Contents of this repository

| File/Folder | Description |
|---|---|
| `metadata.csv` | Metadata for all 1,834 entries (UTF-8, comma-separated) |
| `txt_unnormalized/` | Full texts in their original spelling as `.txt` files (UTF-8), filename = `id` |
| `txt_normalized/` | Full texts with normalised spelling as `.txt` files (UTF-8), filename = `id` |

---

## Corpus design and construction

RoKo uses a historical concept of the novel (following Schröter 2019, 2026) and consists of two sub-corpora:

- The **core corpus** contains texts that are labelled as a novel (*Roman*, or a compound such as *Briefroman*) in their title or paratext.
- The **extension corpus** contains texts without this label that correspond to the assumed prototype of the novel in the period: fictional narrative prose of at least 32,000 words.

The texts were selected from five digital collections: 

| Priority | Basis | Entries in final corpus | Description |
|---|---|---|---|
| 1 | TextGrid Digital Library | 218 | [textgridrep.org](https://textgridrep.org) – digitised full texts, based on the zeno.org library |
| 2 | DeCorp | 1,381 | Corpus of German-language literature with a sub-corpus of fictional texts, based on Projekt Gutenberg-DE and Project Gutenberg (Rohrbacher 2025) |
| 3 | Gutenberg supplement | 204 | Texts from Projekt Gutenberg-DE missing from DeCorp |
| 4 | Bildungsroman corpus | 11 | Curated collection of German-language texts belonging to the genre of the (so-called) *Bildungsroman* (Boucher et al. 2024) |
| 5 | DTA Core Corpus | 18 | [Deutsches Textarchiv](https://www.dwds.de/d/korpora/dtak) – core corpus of the German Text Archive |

*(2 further works were added manually and are not attributed to one of these five collections.)*

Core and extension corpus were merged and deduplicated. The corpus was then cleaned extensively, with LLM-assisted error detection (Claude, Anthropic) and manual review.

### Full-text sources

The actual provenance of the text files – i.e. which digital archive provided the full text – is documented in the metadata column `fulltext_source`:

| Source | Entries | URL |
|---|---|---|
| Projekt Gutenberg-DE | 1,457 | [projekt-gutenberg.org](https://www.projekt-gutenberg.org) |
| TextGrid Repository | 218 | [textgridrep.org](https://textgridrep.org) |
| Project Gutenberg | 135 | [gutenberg.org](https://www.gutenberg.org) |
| Deutsches Textarchiv (DTA) | 19 | [deutschestextarchiv.de](https://www.deutschestextarchiv.de) |
| Other | 5 | Scans from the DNB, UB Bielefeld, BSB, Google Books and BYU ScholarsArchive, obtained via the Bildungsroman corpus |

Each entry has a unique ID of the form `Surname_ShortTitle`, with an appended volume number for multi-volume works (e.g. `Alexis_Woldemar`, `Auerbach_Landhaus_5`).

---

## Metadata (`metadata.csv`)

The metadata file contains one row per text file (entry) with the following columns:

| Column | Description |
|---|---|
| `id` | Unique entry ID, corresponds to the filename (without `.txt`) |
| `subcorpus` | `core` (core corpus) or `extension` (extension corpus) |
| `author` | Author name (as in GND pref name; for the one author without GND ID as in the source collection) |
| `title` | Work title (original spelling) |
| `year` | Year of first publication |
| `title_normalised` | Normalised work title (without volume information) |
| `volume_number` | Volume number (empty for single-volume works) |
| `follows_after` | `id` of the preceding volume (for multi-volume works) |
| `n_tokens` | Token count including punctuation (spaCy `de_core_news_sm`, whitespace not counted) |
| `n_tokens_no_punct` | Token count excluding punctuation |
| `filename_orig` | Original filename in the source collection |
| `year_source` | Source of the year (the source collection, or the reference used for manual corrections, e.g. `Wikipedia`, `title page`); see *Limitations* |
| `author_gnd_id` | GND ID of the author |
| `author_gnd_gender` | Gender according to GND (`male`, `female`, `unknown`; empty if not available) |
| `author_gnd_birth_year` | Year of birth according to GND |
| `author_gnd_death_year` | Year of death according to GND |
| `base` | Source collection (`TextGrid`, `DeCorp`, `Gutenberg supplement`, `Bildungsroman`, `DTA`, `manual` [added manually]) |
| `fulltext_source` | Primary text source (`PG-DE` [Projekt Gutenberg-DE], `PG-US` [Project Gutenberg], `TextGrid`, `DTA`, `other`) |
| `canonicity_score` | Canonicity score of the author, from the GermanCanon database (Eckardt et al. 2027, forthcoming), matched via GND ID (`0` = author not listed in the GermanCanon database, not a measured value; listed scores are always ≥ 1) |

---

## Text files

All texts are provided as UTF-8-encoded plain text files (`.txt`) in two versions. Both share the same extraction step: the running text was extracted from the source formats (HTML for Gutenberg, XML/TEI for TextGrid and DTA, TXT for DeCorp), and metadata, tables of contents, editorial apparatus, and paratexts were removed as far as possible.

**`txt_unnormalized/`** contains the texts as they appear in the respective digital collection, including historical characters (e.g. long s `ſ`, r rotunda `ꝛ`, superscript e as in `aͤ`) and line-end hyphenation.

**`txt_normalized/`** contains the texts with normalised spelling using [`hybrid_textnorm`](https://github.com/aehrm/hybrid_textnorm) (Ehrmanntraut 2024; version 0.1.0) , without LLM reranking.

---

## Limitations

RoKo underwent substantial cleaning. Nonetheless, it should not be expected to be flawless. Four problems are likely to be the most relevant:

1. **Non-novels in the extension corpus.** This is likely to concern above all collections of shorter narrative texts and factual texts that were wrongly classified as novels. With the help of agentic AI tools and repeated manual review, many such texts were identified and removed during corpus construction. Experience shows, however, that some will remain.
2. **Incorrect years.** The column `year` is intended to give the year of first publication. For texts taken from the DTA and TextGrid, the quality should be very good: the DTA digitised first editions, and for TextGrid the year was taken from the editorial note on the first printing (*Erstdruck*) that accompanies each text, even though the text itself often follows a later edition. For texts going back to Project Gutenberg, however, the quality is mixed: it is not always clear whether the year refers to the first publication or to a later edition, and in some cases it may simply be wrong. In addition, DeCorp estimated missing years from the authors' life dates, and a few years in RoKo are estimates as well (see `year_source`). Where possible, I have already made minor corrections. A more extensive correction on a more reliable philological basis is planned for a future release.
3. **Duplicates.** Despite several rounds of deduplication, some works may still be contained more than once (e.g. in different editions or under different titles).
4. **Normalisation errors.** The automatic normalisation in `txt_normalized/` is not error-free; for example, compounds written separately in historical spelling are not always joined, and proper names are occasionally altered.

If you notice such errors while working with RoKo, I would be grateful for any hints and observations, so that future releases can provide improved versions.

---

## References

Eckardt, Jana, Agnes Hilger, and Merten Kröncke. „1000 Punkte für Goethe: Eine empirische Rekonstruktion des Kanons der neueren deutschsprachigen Literatur“. Zeitschrift für Germanistik 37, no. 1 (2027, forthcoming).

Ehrmanntraut, Anton. „Historical German Text Normalization Using Type- and Token-Based Language Modeling“. arXiv:2409.02841, 2024. https://arxiv.org/abs/2409.02841.

Schröter, Julian. Quantitative Gattungsgeschichte. Zur Modellierung der deutschsprachigen Novelle im 19. Jahrhundert. Berlin/Boston, 2026.

Schröter, Julian. „Gattungsgeschichte und ihr Gattungsbegriff am Beispiel der Novellen“. Journal of Literary Theory 13, no. 2 (2019): 227–57.

---

## Licence

RoKo (metadata and documentation) is published under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The full texts are in the public domain in Germany; where the source collections publish their digitisations under a licence, that licence applies: TextGrid texts are available under [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/), DTA texts under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/), and texts from the Bildungsroman corpus under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The literary works from Project Gutenberg and Projekt Gutenberg-DE are in the public domain; the texts were taken over, largely via DeCorp (Rohrbacher 2025), as in other research corpora.

---

## Changelog

- **v1.0** (2026): initial release
