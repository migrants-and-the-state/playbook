# Project glossary

## `a-file`
- an abstract archival concept for all immegration documents related to a single person (identified by an `a-number`) collated together into a single "file" on someone
- the primary *unit of study* for the project
- must have an associated `a-number`, might have additional associated metadata from original NARA cataloging and/or extracted from M/S ML/OCV/AI pipelines


## `order-group`
- a set of `a-file-orders` retrieved together (e.g., from kansas city in summer 2023)

## `a-file-order`
- a specific file (PDF) or group of files (folder of JPGs/TIFs) shared by NARA *at a given point in time* that comprise the `a-file` in question
- contains `pages`
- the primary *material* of the project's dataset

## `a-number`
- a unique identifier for an individual person; all immigration documents related to that individual person *should* be tagged with their `a-number`  

## `document`
- a group of one or more `pages` within an `a-file-order` that make up a logical document (e.g., recto and verso of an index card, 3 pages of legal letter, etc.)
- NOTE: this type of data is not currently in scope to parse.  

## `page`
- an abstract archival concept enveloping a single image scan (e.g., JPG) within an `a-file`
- must have an index number to signifify order within the `a-file`, might have associated metadata as extracted from M/S ML/OCV/AI pipelines


