# SectLabel

*Minh-Thang Luong, Thuy Dung Nguyen and Min-Yen Kan (2010) Logical Structure Recovery in Scholarly Articles with Rich Document Features. International Journal of Digital Library Systems (IJDLS), 1(4), 1-23.
Extention of ParsCit (existing platform for reference string parsing)*

* Use OCR to incorporate font/positioning info in addition to raw text (raw text + xml for layout)
* Capture the doc’s logical structure (not only metadata like titles, authors, references, but sections, subsections, figures, tables, equations, footnotes, captions)
* SectLabel (logical structure classification and generic section classification)
    * Classify lines by the structures they belong to/their “roles”
    * Uses CRFs
    * Figure out general purpose of sections based on headers
“1) logical structure classification, and 2) generic section classification. In the first task, we consider a scholarly document as an ordered collection of text lines, and need to label each text line in a document with a semantic category, representing its logical role. In the second task, we take the headers of each section of text in a paper as evidence to deduce a generic logical purpose of the section”


“We summarize our contributions as follows: 
1. We present the first logical structure discovery tool that expressly caters for scientific documents. Unlike previous scholarly document processing systems, it attempts logical structure discovery over the span of the entire paper at a fine-grained (per line) level. 

2. We infer the generic logical purpose of each major section of text, mapping specific sections names to generic ones (i.e. “5. Text Features” → Methodology). This promotes comparative viewing of sections with identical purpose across articles. 

3. Our implemented system handles both rich formatted input from an optical character recognition system, as well as from plain text dumps of scholarly articles. We evaluate both modes of input and conclusively show that rich input enhances classification performance, especially for certain logical structure classes. 

4. We have created gold-standard training data for these tasks and made this dataset public at the original ParsCit site to encourage others to perform comparative evaluation. “

MODEL:
Pdf = set of lines that need to be labeled with a set of classes (only classifies entire lines, not pieces of lines?)
“Our system consists of two parts: a primary component, Logical Structure (LS), and a
subordinating part, namely Generic Section (GS). The LS component takes in full-text papers as input, while the GS component only deals with header lines”

Evaluation on a small dataset (only ~250 papers from ACM only?)
“We found that acceptable performance (~76 macro F1) results from careful feature engineering on raw text. A key finding is that modeling additional per-page spatial information, yields a significant improvement of over 9 macro F1 points, and significantly boosts detection of important categories such as paper metadata, captions, and hierarchical headers.”
