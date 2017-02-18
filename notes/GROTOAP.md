# GROTOAP2 — The Methodology of Creating a Large Ground Truth Dataset of Scientific Articles

GROTOAP2 is a "a large dataset of ground truth files containing labelled fragments of scientific articles in PDF format, useful for training and evaluation of document content analysis-related solutions."

* Used to train CERMINE
* Based on articles from PubMed Central Open Access Subset
* Algorithm can be ajdusted to build large datasets from other data sources

## 1. Intro
* Want to understand the roles of different "zones" or "blocks" in documents (classify them)
* Need to evaluate zone classification with a reliable test set
* Problem is challenging (e.g. there are 500 different publishers w/ different layouts in a subsample of 125,000 docs from PubMed)
* Want the dataset to preserve size, position, distance info for zones
* GROTOAP2 (GRound Truth for Open Access Publications)
    * 13,210 ground truth articles
    * Records contain full text of article, zone labels, geometric text features
    * Can obtain coresponding PDFs using provided scripts
* Dataset was created semi-automatically w/ short manual phase

## 2. Previous Work
* UW-II - document images adn structure-related ground truth info (closed-source)
* MARG - scanned pages from biomedical journal (first pages only, only some zones)
* PRImA - doc images
* MediaTeam Oulu Document Database
* UvA dataset
* Tobacco800

Open Access Subset of PubMed Central
* ~500,000 life science pdfs + metadata in NLM files
* NLM files contain:
    * title, authors, affiliations, abstract, journal
    * full text
    * references with metadata
    * doesn't contain geometrical information (only text)
    
## GROTOAP2 Dataset
* Number of:
    * publishers: 208
    * documents: 13,210
    * pages: 119,334
    * zones: 1,640,973
    * zone labels: 22
* XML format
* list of URLs to PDF articles
* bash script to download the PDFs from PMC repo

### Ground Truth Structure
* heirarchical structure
    * article = list of pages
    * page = list of zones
    * zone = list of lines
    * lines = lists of words
    * words = lists of characters
 * contains reading order, labels for all zones
 * bounding boxes defined by left upper, lower right corners
 * 22 zone labels
     * abstract — document's abstract, usually one or more zones placed in the first page of the document;
     * acknowledgments — sections containing information about document's acknowledgments or funding;
     * affiliation — authors' affiliations sections, which sometimes contain contact information (emails, addresses) as well;
     * author — a list of document's authors,
     * bib_info — zones containing all kinds of bibliographic information, such as journal/publisher name, volume, issue, DOI, etc.; often includes pages' headers or footers; headers and footers containing document's title or author names are also labelled as bib_info;
     * body_content — the text content of the document, contains paragraphs and section titles;
     * conflict_statement — conflict statement declarations;
     * copyright — copyright or license-related sections;
     * correspondence — the authors' contact information, such as emails or addresses;
     * dates —important dates related to the document, such as received, revised, accepted or published dates;
     * editor — the names of the document's editors;
     * equation — equations placed in the document;
     * figure — zones containing figures' captions and other text fragments belonging to figures;
     * glossary — important terms and abbreviations used in the document;
     * keywords — keywords listed in the document;
     * page number —zones containing the numbers of pages;
     * references —all fragments containing bibliographic references listed in the document;
     * table — the text content of tables and table captions;
     * title — the document's title;
     * title_author — zones containing both document's title and the list of authors;
     * type —the type of the document, usually mentioned on the first page near the title, such as "research paper", "case study" or * "editorial";
     * unknown — used for all the zones that do not fall in any of the above categories.
* (paper contains graph about prevalence of labels)
### Ground Truth File Format
* TrueViz (XML format to store geometrical/logical structure)

## Building the Dataset
1. download PubMed PDFs, NML files
2. Process PDFs using CERMINE tools, contruct geometric heirarchy and reading order
3. Compare extracted text to corresponding NLM files to get most probable labels for zones
    * use Smith-Waterman sequence alignment algorithm to measure similarity of text strings
    * for each zone, choose strin gwith highest sim. score above a threshold
    * (see 4.1 for more details)
4. Filter out files where zones couldnt' be auto-labeled
5. Inspect files (human), identify common errors, use heuristics to correct them
6. choose final dataset randomly from corrected files
    * made sure that the final dataset had a similar distribution of publishers to the original dataset
(can apply this same process to other datasets)

## Evaluation
* Manual
    * randomly sampled 50 documents, overall accuracy of annotations is 0.93 (after heuristic rules are applied)
* Used 1000 docs to train CERMINE (for evaluation)   
    * got F1 of 79.34%
    

