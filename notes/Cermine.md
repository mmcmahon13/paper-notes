# CERMINE

*Tkaczyk, Dominika, et al. "CERMINE: automatic extraction of structured metadata from scientific literature." International Journal on Document Analysis and Recognition (IJDAR) 18.4 (2015): 317-335.*

## Intro:
* Digital libraries need metadata to provide search tools, similar docs, citation networks, etc.
* Issues:
    * Different layouts
    * PDF doesn't presever structure (words, paragraphs, reading order, lists, etc. need to be reverse engineered)

* CERMINE provides:
     * Set of document metadata
     * List of references with metadata
     * Structured full text with sections, subsections, etc.
     * Modular workflow
     
## State of the Art
### Rule-based:

* psototext (content extractor for PostScript files) used extract text, rules used to extract metadata
* PDFX (converts PDF to XML representation, extracts metadata and unparsed references)
* PDF-extract (ids and extracts "semantically significant" regions using visual cues, content traits, divide up sections and references) 

### ML:
Different approaches use different granularity of classifcication (lines, chunks, blocks)

#### SVM 
* Han et al.
* Kovacevic et. al. 
* Lu et. al

#### Neural Classifier
* Marinai
    * uses JPedal to extract chars
    * rule-based page segmentation
    * neural classifcation over zones

#### HMM
* Cui, Chen
    * use pdftohtml to do extraction and segmentation

#### Max Entropy
* Kern et al.

#### CRF
* Lopez 
      * GROBID (full text, metadata, list of parsed references)
* ParsCit (Luong et. al)
      * metadata, structured full text, bibliography
      * uses text only and doesn't use geometric features
