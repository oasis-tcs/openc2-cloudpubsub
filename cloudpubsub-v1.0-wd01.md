
![OASIS Logo](https://docs.oasis-open.org/templates/OASISLogo-v2.0.jpg)
# OASIS Committee Note
-------

# OpenC2 Use of Cloud-Based Pub/Sub Messaging Version 1.0

## Committee Note Draft 01 /<br>Public Review Draft 01

## 09 April 2020

#### Technical Committee:
[OASIS Open Command and Control (OpenC2) TC](https://www.oasis-open.org/committees/openc2/)

#### Chairs:
Joe Brule (jmbrule@nsa.gov), [National Security Agency](https://www.nsa.gov/) \
Duncan Sparrell (duncan@sfractal.com), [sFractal Consulting LLC](http://www.sfractal.com/)

#### Editors:
Robert O'Brien (robert.obrien@omnyon.com), [G2, Inc.](https://www.g2-inc.com/) \
David Lemire (david.lemire@hii-tsd.com), [G2, Inc.](https://www.g2-inc.com/)

#### Additional artifacts:
This document is one component of a Work Product that also includes:
* XML schemas: (list file names or directory name)
* Other items (list titles and/or file names)

#### Related work:
This document replaces or supersedes:
* Specifications replaced by this document (include hyperlink, preferably to HTML format)

This document is related to:
* Related specifications (include hyperlink, preferably to HTML format) \
`(remove "Related work" section or the "replaces" or "related" subsections if no entries)`

#### Abstract:
Open Command and Control (OpenC2) is a concise and extensible language to enable the command and control of cyber defense components, subsystems and/or systems in a manner that is agnostic of the underlying products, technologies, transport mechanisms or other aspects of the implementation. Google Cloud Platform (GCP) Pub/Sub (i.e., publish / subscribe) is an asynchronous communication method that enables messages to be exchanged without knowing the identity of the sender or recipient. This document describes the use of GCP Pub/Sub as a transfer mechanism for OpenC2 messages, and captures experience and lessons learned from utilizing GCP Pub/Sub at the January 2020 OpenC2 Plug Fest. The information contained within this document can be applied when using the GCP Pub/Sub implementation for conveying OpenC2 command and response messages, and may also be useful with other publish / subscribe protocol implementations.

#### Status:
This is a Non-Standards Track Work Product. The patent provisions of the OASIS IPR Policy do not apply.

This document was last revised or approved by the OASIS Open Command and Control (OpenC2) TC on the above date. The level of approval is also listed above. Check the "Latest version" location noted above for possible later revisions of this document. Any other numbered Versions and other technical work produced by the Technical Committee (TC) are listed at https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=openc2#technical.

TC members should send comments on this document to the TC's email list. Others should send comments to the TC's public comment list, after subscribing to it by following the instructions at the "Send A Comment" button on the TC's web page at https://www.oasis-open.org/committees/openc2/.

#### URI patterns:
Initial publication URI:  
https://docs.oasis-open.org/openc2/cloudpubsub/v1.0/cnd01/cloudpubsub-v1.0-cnd01.html

Permanent "Latest version" URI:  
https://docs.oasis-open.org/openc2/cloudpubsub/v1.0/cloudpubsub-v1.0.html

(Note: Publication URIs are managed by OASIS TC Administration; please don't modify.)

#### Citation format:
When referencing this document the following citation format should be used:

**[Cloud-Pub/Sub-v1.0]**

_OpenC2 Use of Cloud-Based Pub/Sub Messaging Version 1.0_. Edited by Robert O'Brien and David Lemire. 09 April 2020. OASIS Committee Note Draft 01 / Public Review Draft 01. https://docs.oasis-open.org/openc2/cloudpubsub/v1.0/cnprd01/cloudpubsub-v1.0-cnprd01.html. Latest version: https://docs.oasis-open.org/openc2/cloudpubsub/v1.0/cloudpubsub-v1.0.html.

-------

## Notices
Copyright © OASIS Open 2020. All Rights Reserved.

All capitalized terms in the following text have the meanings assigned to them in the OASIS Intellectual Property Rights Policy (the "OASIS IPR Policy"). The full [Policy](https://www.oasis-open.org/policies-guidelines/ipr) may be found at the OASIS website.

This document and translations of it may be copied and furnished to others, and derivative works that comment on or otherwise explain it or assist in its implementation may be prepared, copied, published, and distributed, in whole or in part, without restriction of any kind, provided that the above copyright notice and this section are included on all such copies and derivative works. However, this document itself may not be modified in any way, including by removing the copyright notice or references to OASIS, except as needed for the purpose of developing any document or deliverable produced by an OASIS Technical Committee (in which case the rules applicable to copyrights, as set forth in the OASIS IPR Policy, must be followed) or as required to translate it into languages other than English.

The limited permissions granted above are perpetual and will not be revoked by OASIS or its successors or assigns.

This document and the information contained herein is provided on an "AS IS" basis and OASIS DISCLAIMS ALL WARRANTIES, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO ANY WARRANTY THAT THE USE OF THE INFORMATION HEREIN WILL NOT INFRINGE ANY OWNERSHIP RIGHTS OR ANY IMPLIED WARRANTIES OF MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE.

-------

# Table of Contents
[[TOC will be inserted here]]

-------

# 1 Introduction

Introductory material.

Here is a customized command line which will generate HTML from this markdown file (named cloudpubsub-v1.0-wd01.md):

pandoc -f gfm -t html cloudpubsub-v1.0-wd01.md -c styles/markdown-styles-v1.8-cn.css --toc --toc-depth=5 -s -o cloudpubsub-v1.0-wd01.html --metadata title="OpenC2 Use of Cloud-Based Pub/Sub Messaging Version 1.0"

We are currently using pandoc 2.6 from https://github.com/jgm/pandoc/releases/tag/2.6.

This also requires the presence of a .css file containing the HTML styles (like styles/markdown-styles-v1.8-cn.css).

Note this command generates a Table of Contents (TOC) in HTML which is located at the top of the HTML document, and which requires additional editing in order to be published in the expected OASIS style. This editing will be handled by OASIS staff during publication. Alternatively, the TC may generate a TOC via other tools or processes.

## 1.1 Terminology

## 1.2 References

(Reference sources:
For references to IETF RFCs, use the approved citation formats at:  
http://docs.oasis-open.org/templates/ietf-rfc-list/ietf-rfc-list.html.  
For references to W3C Recommendations, use the approved citation formats at:  
http://docs.oasis-open.org/templates/w3c-recommendations-list/w3c-recommendations-list.html.  
Remove this note before submitting for publication.)

###### [OpenC2-HTTPS-v1.0]
_Specification for Transfer of OpenC2 Messages via HTTPS Version 1.0_. Edited by David Lemire. Latest version: http://docs.oasis-open.org/openc2/open-impl-https/v1.0/open-impl-https-v1.0.html
###### [OpenC2-SLPF-v1.0]
_Open Command and Control (OpenC2) Profile for Stateless Packet Filtering Version 1.0_. Edited by Joe Brule, Duncan Sparrell, and Alex Everett. Latest version: http://docs.oasis-open.org/openc2/oc2slpf/v1.0/oc2slpf-v1.0.html

## 1.3 Some markdown usage examples

**Text.**

Note that text paragraphs in markdown should be separated by a blank line between them -

Otherwise the separate paragraphs will be joined together when the HTML is generated.
Even if the text appears to be separate lines in the markdown source.

To avoid having the usual vertical space between paragraphs,  
append two or more space characters to the end of the lines  
which will generate an HTML break tag instead of a new paragraph tag  
(as demonstrated here).

### 1.3.1 Figures and Captions

FIGURE EXAMPLE:
<note caption is best ABOVE figure, to allow a link to it to display image - same for table captions>

###### Figure 1 -- Title of Figure
![image-label should be meaningful](images/image_0.png) (this image is missing)

###### Figure 2 -- OpenC2 Message Exchange
![message exchange](images/image_1.png)


### 1.3.2 Tables

#### 1.3.2.1 Basic Table
**Table 1-1. Table Label**

| Item | Description |
| :--- | :--- |
| Item 1 | Something<br>(second line) |
| Item 2 | Something |
| Item 3 | Something<br>(second line) |
| Item 4 | text |

#### 1.3.2.2 Table with Three Columns and Some Bold Text
text.

| Title 1 | Title 2 | title 3 |
| :--- | :--- | :--- |
| something | something | something else that is a long string of text that **might** need to wrap around inside the table box and will just continue until the column divider is reached |
| something | something | something |

#### 1.3.2.3 Table with a caption which can be referenced

###### Table 1-5. See reference label construction

| Name | Description |
| :--- | :--- |
| **content** | Message body as specified by content_type and msg_type. |

Here is a reference to the table caption:
Please see [Table 1-5 or other meaningful label](#table-1-5-see-reference-label-construction) 


### 1.3.3 Lists

Bulleted list:
* bullet item 1.
* **Bold** bullet item 2.
* bullet item 3.
* bullet item 4.

Indented or multi-level bullet list - add two spaces per level before bullet character (* or -):
* main bullet type
  * Example second bullet
    * See third level
      * fourth level

Numbered list:
1. item 1
2. item 2
3. item 3

### 1.3.4 Reference Label Construction

REFERENCES and ANCHORS
- in markdown source, format the Reference tags as level 6 headings like: `###### [OpenC2-HTTPS-v1.0]`
###### [OpenC2-HTTPS-v1.0]
_Specification for Transfer of OpenC2 Messages via HTTPS Version 1.0_. Edited by...

- reference text has to be on a separate line below the tag

- format cross-references (citations of the references) like: `see [[OpenC2-HTTPS-v1.0](#openc2-https-v10)]`  
"see [[OpenC2-HTTPS-v1.0](#openc2-https-v10)]"  
(note the outer square brackets in markdown will appear in the visible HTML text)

- The text in the Reference tag (following ###### ) will become an HTML anchor using the following conversion rules:  
-- punctuation marks will be dropped (including "[" )  
-- leading white spaces will be dropped  
-- upper case will be converted to lower  
-- spaces between letters will be converted to a single hyphen

- The same HTML anchor construction rules apply to cross-references and to section headings.  
-- Thus, a section heading like "## 1.2 References"  
-- becomes an anchor in HTML like `<a href="#12-references">`  
-- referenced in the markdown like: see [Section 1.2](#12-references)  
-- (in markdown: `"see [Section 1.2](#12-references"`)  
-- similar HTML anchors are also used in constructing the TOC

### 1.3.5 Code Blocks

Text to appear as an indented code block with grey background and monospace font - use three back-ticks before and after the code block).

Note the actual backticks will not appear in the HTML version. If it's necessary to display visible backticks, place a back-slash before them like: \``` .

```
{   
    "target": {
        "x_kmip_2.0": {
            {"kmip_type": "json"},
            {"operation": "RekeyKeyPair"},
            {"name": "publicWebKey11DEC2017"}
        }
    }
}
```

Text to be highlighted as code can also be surrounded by a single "backtick" character: 
`code text`

## 1.4 Page Breaks
Add horizontal rule lines where page breaks are desired in the PDF - before each major section
- insert the line rules in markdown by inserting 3 or more hyphens on a line by themselves:  ---
- place these before each main section in markdown (usually "#" - which generates the HTML `<h1>` tag)

-------

# 2 Section Heading
text.

## 2.1 Level 2 Heading
text.

### 2.1.1 Level 3 Heading
text.

#### 2.1.1.1 Level 4 Heading
text.

##### 2.1.1.1.1 Level 5 Heading
This is the deepest level, because six # gets transformed into a Reference tag.


## 2.2 Next Heading
text.

-------


# Appendix A. Acknowledgments

(Note: A Work Product approved by the TC must include a list of people who participated in the development of the Work Product. This is generally done by collecting the list of names in this appendix. This list shall be initially compiled by the Chair, and any Member of the TC may add or remove their names from the list by request.  
Remove this note before submitting for publication.)

The following individuals have participated in the creation of this specification and are gratefully acknowledged:

**OpenC2 TC Members:**

| First Name | Last Name | Company |
| :--- | :--- | :--- |
Philippe | Alcoy | Arbor Networks
Alex | Amirnovin | Viasat
Kris | Anderson | Trend Micro
Darren | Anstee | Arbor Networks

-------

# Appendix B. Revision History
| Revision | Date | Editor | Changes Made |
| :--- | :--- | :--- | :--- |
| cloudpubsub-v1.0-wd01 | yyyy-mm-dd | Editor Name | Initial working draft |

