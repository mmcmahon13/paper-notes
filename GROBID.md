# GROBID: GeneRation Of BIbliographic Data.
*GROBID: Combining Automatic Bibliographic Data Recognition and Term Extraction for Scholarship Publications. P. Lopez. Proceedings of the 13th European Conference on Digital Library (ECDL), Corfu, Greece, 2009.*

“GROBID can be considered as production ready. Deployments in production includes ResearchGate, HAL Research Archive, the European Patent Office, INIST, Mendeley, CERN, …”

* “Extracts bibliographical data corresponding to the header information (title, authors, abstract, etc.) and to each reference (title, authors, journal title, issue, number, etc.). The references are associated to their respective citation contexts.”
* Uses CRFs (following the approach of Mallet) based on 1000 training examples for headers and 1200 for references
* Evaluated on CORA dataset: 98.6% accuracy per header field, 74.9% per complete header instance, 95.7% per citation field 78.9% per citation instance
* Grobid can optionally query Crossref web service to get publisher metadata, perform corrections, etc.
* Multi-level term extraction: 	optionally enrich header with lists of disciplinary domains related to the article, the most significant terms extracted from the article, and terms extracted from the article’s citations (score terms by “phraseness” and “informativeness”)

## MODELS:
[(link to presentation)](https://grobid.readthedocs.io/en/latest/grobid-04-2015.pdf) (screenshots are from this presentation)

* Based on 11 different CRF models (2 for patents)
* Each uses own feature set, training set, normalization

Features include:

- Position info (beginning/end of line in docs)
- Lexical info (vocab, “large gazetteers”)
- Layout info (font size, block, etc.)

Segmentation of document into
- Cover
- Header
- Body
- Footnotes
- Headnotes
- Bibliography
- Annexes

[todo add images]

* Segmentation uses “XY-cut algorithm”
### Header processing:
* CRF to segment the header into title, authors, affiliations, abstract, date, keywords
* CRF to further parse authors, affiliations, dates
### Reference parsing:
* Use CRF for segmentation, CRF for author sequence, dates
* Extract author, journals, dates, volumes, issues, pages

### “Cascading Model” Approach
Pros:
* Hierarchical structure from “flat” linear chain CRF
* Manage fine grained structures (nested labels)
* Model modularity (reuse date, name models)
* Quick to train lots of models with fewer labels and features

Propagation of errors

### Applications for Digital Libraries
* self-archiving of articles
* the pre-processing of documents for information retrieval
* generation of reliable OpenURL links or automatic citation suggestions.

