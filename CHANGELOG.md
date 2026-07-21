# Changelog

## 1.3.1 - 2026-07-21

- Add documentation for `tag_nametag` and NE extraction from common vertical within `ann2standoff`.

## 1.3 - 2026-01-16

### process

- Support calling custom external tagger command via shell

### tag_ud

- Fixes for pre-tokenized input (vertical/conllu)
- Allow specification of custom URL for UDpipe and NameTag API

### ann2standoff

- Support vertical with an optional additional column with named entities created by tag_nametag

### standoff2xml

- Fix bug caused by empty elements

### xml2vrt

- BREAKING CHANGE: flattening of nested structures is not a default anymore, it must be turned on explicitly (it may have surprising consequences when dealing with complex nesting, which the user might prefer to solve by other means within pre- or post-processing)
- Fix removing tags within token strings
- Fix additional closing tag after empty elements

## tag_nametag

- An independent client for the NameTag NER API, which can also add named entity annotation to an existing vertical

## 1.2.1 - 2025-10-08

### tag_ud

- Added option to disable doubling of line breaks
- Fix configuration of punctuation symbol elements

### xml2vrt

- Fix configuration of punctuation symbol elements

##  1.2 - 2025-10-07

_Warning: defaults were changed and some options renamed (esp. the long option/configuration option names)._

### process

- Added as a new script: a simple wrapper to run the whole chain of scripts automatically and in several threads for a batch of files at once (all scripts needed adaptation)
- Added progress-bar
- Added option to keep temporary files (deleted by default)

### xml2standoff

- Support basic constraints for text elements and excluded elements: constraints on their attributes and ancestors

### tag_ud

- Support all features and options of the LINDAT UDPipe REST API
- Support LINDAT's NameTag API for recognition of named entities (NER) as an optional  layer added on top of the UD analysis

### ann2standoff

- Support creation of virtual XML subtokens (from the 2nd level of the UD tokenization)
- Support excluding (discarding) selected attributes from the analysis
- Allow underscore to be used as a void attribute name for attributes to be ignored/excluded (alternative way to exclude attributes from the analysis)
- Support annotation of named entities extracted from the CoNLL-U+NE format produced by NameTag in the form of additional XML elements (spans)
- Support special annotation of punctuation symbols (as suggested by TEI Guidelines)

### standoff2xml

- Support (preferable) nesting of original elements *inside* of the added annotation spans where possible
- Support merge of overlapping elements with the same name and length
- Removed the need to specify token and sentence element names (and the corresponding options)

### configuration

- Make all scripts configurable from a single common configuration file `xmlanntools.ini` (some options needed renaming to avoid conflicts)
- Make a new profile `tei_default` the default
- Change default first column attribute in CoNLL-U (token number within the sentence) name to `n` (in order to avoid confusion with `xml:id`)

## 1.1.1 - 2025-04-10

### xml2standoff

- Support line-break insertion also for empty elements
- Fix: Deal with line-breaks between XML element name and the attributes

### ann2standoff

- Remove the option `-us` and make skipping any unicode whitespace default again

### standoff2xml

- Configurable sentence and token element names
- Do NOT restart/keep broken elements between sentences by default (usually highlighted text)
- Fix: fix loosing elements at the end of the document in the merge process

### xml2vrt

- Automatic flattening of nested structures

### configuration

- Make `conllu` the default profile

### examples

- Added example `TEI_example1`
- Added example `Simple_poetry1`

## 1.0 - 2024-07-31

- Initial version derived and extended from the legacy `xml2standoff` tools used within the CNC research infrastructure 