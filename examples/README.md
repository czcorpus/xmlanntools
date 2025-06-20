## TEI_example1

A few short fragments from the English text "Red as a rose is she" by Rhoda Broughton as available from the [GitHub repository](https://github.com/COST-ELTeC/ELTeC-eng/blob/master/level1/ENG18871_Broughton.xml) of the [COST ELTeC project](https://www.distant-reading.net/eltec/).

Looking at the source file `TEI_example1.xml`, we can see typical TEI structure with a `teiHeader` and `text` elements within the `TEI` root element. Let's say that we want to process the file contents in the following way:

- rather than extracting text from the whole `<text>` element, we want to extract text from the single paragraph-level elements `<p>` and `<head>`, possibly also the `<quote>` elements (see below for more discussion); the extraction will temporarily remove any line-breaks within the inner contents of these text elements, so that the tagger may span sentences freely across the purely technical line-breaks in the XML source
- we want to treat single verses within the `<l>` element as independent text segments too, not allowing the tagger to span sentences across the verse breaks (another approach may also be appropriate, though); these elements occur both as nested within `<p>` and within `<quote>`
- in the presented fragment, the `<quote>` element occuring *outside* the `<p>` element only consists of `<l>` elements, which we want to treat separately, and does not contain any other text contents on its own. Therefore, there would in principle be no need to define it as another text element. However, we might want to extract the `<quote>`s nested *within* a `<p>` as separate text elements (separated by line-break for the tagger) and in that case we should also add it to the list of text elements
- we want to exclude anything within the `<teiHeader>` and `<front>`-matter (these parts may otherwise also contain some above mentioned elements defined as text elements!)
- we also want to exclude text parts within the `<foreign>` element, since there is no reason to analyze them with an English tagger

In this situation, we should run `xml2standoff` with the text elements (option `-t`) specified as `p,head,quote,l` and excluding (option `-e`) anything within the elements `teiHeader,front,foreign`: 

```
xml2standoff -t p,head,quote,l -e teiHeader,front,foreign TEI_example1.xml
```

We get the extracted plain text file `TEI_example.txt` and a separate standoff description of the original XML mark-up in the file `TEI_example.json`. We can now pass the former file (option `-f`) to the tagger, e.g. to the UD Pipe using an appropriate language model (option `-m`), and send the resulting output to the file `TEI_example.conllu`:

```
tag_ud -f TEI_example1.txt -m en >TEI_example1.conllu
```

Now, we can convert the resulting analysis from `TEI_example1.conllu` into another standoff XML-type mark-up by matching it to the original `TEI_example1.txt`. This can be done by the script `ann2standoff` using its CoNLL-U parser, i.e. the profile `conllu` (option `-p`):

```
ann2standoff -p conllu TEI_example1.conllu
```

This will create the `TEI_example1.ann.json` file (if the results from the tagger can be successfully matched with the plain text file).

In the next step, we can construct a new annotated XML by merging both the original standoff XML mark-up and the standoff annotation produced by the tagger/analyzer with the plain text contents:

```
standoff2xml TEI_example1.txt
```

The resulting file will be named `TEI_example1.ann.xml` and in addition to the original XML mark-up, it will contain sentences delimited by the elements `<s>` and analyzed tokens enclosed within the elements `<w>` with attributes containing the analysis produced by the tagger/analyzer. Any subtoken annotation (resulting from the two level tokenization of UD) will be merged as described in the main `README.md`.

In case we need to index the final annotated XML text file by a search engine using the vertical format as input (CQP/Manatee), we may use the supplementary script `xml2vrt` to create the vertical format containing just the relevant contents from the annotated XML file. In this case we would probably only include (option `-i`) the `text` element (avoiding the `teiHeader` completely) and also exclude (option `-e`) the whole `front`-matter. By using the preconfigured profile (option `-p`) `conllu`, the created vertical will have a similar structure as the CoNLL-U format, just with the ortographic "word" element placed into the first column (before the token ID) as required by the indexing engines.

```
xml2vrt -p conllu -i text -e front TEI_example1.ann.xml >TEI_example1.vrt
```

 All other changes required by the CQP/Manatee engines are applied as described in the main `README.md`. Fragments of text which were excluded from the annotation/analysis by the tagger (i.e. those within the `<foreign>` elements) are kept as untokenized strings on separate lines: from the perspective of the search engine they will appear as long, single string "tokens" without further annotation/analysis - they won't be thus easilly searchable (by single words), but they will be kept in the output of the concordances. If desired, they could be completely discarded from the vertical using the option `-df`.

## Simple_poetry1

A simple example where a whole poem (stanza) is contained in a single `<p>` element with the line break element `<lb/>` separating single verses (lines).

In such case, we can also add the empty element `lb` to the list of "text elements" (`head` and `p`) to enforce insertion of linebreaks:

```
xml2standoff -t p,head,lb Simple_poetry1.xml
```

The rest of the process will be very similar as above, just simpler:

```
tag_ud -f Simple_poetry1.txt -m cs >Simple_poetry1.conllu
ann2standoff -p conllu Simple_poetry1.conllu
standoff2xml Simple_poetry1.txt
```
