---
title: "Bash_extras"
author: "Radhika Khetani"
date: 2017-07-06
duration: 45
---

## Overview

* [Setting up](#setup)
* [Regular expressions (regex) in `bash`](#regex)
* [Reintroducing `grep`](#grep)
    * [`grep` examples](#example1)
* [Introducing `sed`](#sed)
    * [`sed` examples](#example2)
* [Reintroducing `awk`](#awk)
    * [`awk` examples](#example3)

***

## Setting up <a name="setup"></a>

```bash

# Change directories
$ cd ~/unix_lesson

# Copy over a file
$ cp /n/groups/hbctraining/ngs-data-analysis-longcourse/unix_lesson/bicycle.txt .
```
***

## Regular expressions (regex) in `bash` <a name="regex"></a>

"A regular expression, regex or regexp (sometimes called a rational expression) is a sequence of characters that define a search pattern. Usually this pattern is then used by string searching algorithms for "find" or "find and replace" operations on strings." -[Wikipedia](https://en.wikipedia.org/wiki/Regular_expression)

"The specific syntax rules vary depending on the specific implementation, programming language, or library in use. Additionally, the functionality of regex implementations can vary between versions of languages." -[Wikipedia](https://en.wikipedia.org/wiki/Regular_expression)

Below is a small subset of characters that can be used for pattern generation in `bash`.

**Special Characters:**

*  `.` : *match any character (except new line)
*  `\` : *make next character literal*
*  `^` : *matches at the start of the line*
*  `$` : *matches at the end of line*
*  `*` : *repeat match
*  `?` : *preceding character is optional
*  `[ ]` : *sequence of characters*
      * `[a-z]` : any one from a through z 
      * `[aei]` : either a, e, i
      * `[0-9]` : any one from 1 through 9

**Examples:**

* `.at` == any three-character string ending with "at", including "hat", "cat", and "bat".
* `ab*c` == "ac", "abc", "abbc", "abbbc", and so on*
* `colou?r` == "color" or "colour"*
* `[hc]at` == "hat" and "cat".
* `[^b]at` == all strings matched by .at except "bat".
* `[^hc]at` == all strings matched by .at other than "hat" and "cat".
* `^[hc]at` == "hat" and "cat", but only at the beginning of the string or line.
* `[hc]at$` == "hat" and "cat", but only at the end of the string or line.
* `\[.\]` == any single character surrounded by "[" and "]" since the brackets are escaped, for example: "[a]" and "[b]".
* `s.*` == "s" followed by zero or more characters, for example: "s" and "saw" and "seed" and "shawshank".

>  above examples excerpted from [Wikipedia](-[Wikipedia](https://en.wikipedia.org/wiki/Regular_expression))

**Non printable characters:**

* `\t` : tab
* `\n` : new line (Unix)
* `\s` : space

***

## Reintroducing `grep` (GNU regex parser) <a name="grep"></a>

As we have seen in session I, `grep` is a line by line parser by default displays matching lines to the pattern of interest that allows the use of regular expressions (regex) in the specified pattern.

**`grep` usage:**

`cat file | grep pattern`

OR

`grep pattern file`

**`grep` common options:**

* `c` : count the number of occurrences
* `v` : invert match, print non-matching lines
* `R` : recursively through directories
* `o` : only print matching part of line
* `n` : print the line number

***

### Examples `grep` usage <a name="example1"></a>

```bash
$ grep -c bicycle bicycle.txt

$ grep "bicycle bicycle" bicycle.txt 

$ grep ^bicycle bicycle.txt
$ grep ^Bicycle bicycle.txt 

$ grep yeah$ bicycle.txt

$ grep [SJ] bicycle.txt

$ grep ^[SJ] bicycle.txt 
```
***

## Introducing `sed` <a name="sed"></a>

`sed` takes a stream of stdin and pattern matches and returns the replaced text to stdout ("Think amped-up Windows Find & Replace").

**`sed` usage:** 

`cat file | sed ‘command’`

OR

`sed ‘command’  file`

**`sed` common options:**

* `4d` : *delete line 4*
* `2,4d` : *delete lines 2-4*
* `/here/d` : *delete line matching here*
* `/here/,/there/d` : *delete lines matching here to there*
* `s/pattern/text/` : *switch text matching pattern*
* `s/pattern/text/g` : *switch text matching pattern globally*
* `/pattern/a\text` : *append line with text after matching pattern*
* `/pattern/c\text` : *change line with text for matching pattern*

### Examples `sed` usage <a name="example2"></a>

```bash
$ sed '1,2d' bicycle.txt

$ sed 's/Superman/Batman/' bicycle.txt 

$ sed 's/bicycle/car/' bicycle.txt 
$ sed 's/bicycle/car/g' bicycle.txt 

$ sed 's/.icycle/car/g' bicycle.txt

$ sed 's/bi*/car/g' bicycle.txt

$ sed 's/bicycle/tri*cycle/g' bicycle.txt | sed 's/tri*cycle/tricycle/g'   ## does this work?
$ sed 's/bicycle/tri*cycle/g' bicycle.txt | sed 's/tri\*cycle/tricycle/g'

$ sed 's/\s/\t/g' bicycle.txt
$ sed 's/\s/\\t/g' bicycle.txt

$ sed 's/\s//g' bicycle.txt
```
***

## Reintroducing `awk` <a name="awk"></a>

`awk is command/script language that turns text into records and fields which can be selected to display as kind of an ad hoc database.  With awk you can perform many manipulations to these fields or records before they are displayed. 

**`awk` usage:** 

`cat file | awk ‘command’`

OR

`awk ‘command’  file`

**`awk` concepts:**

*Fields:*

Fields are separated by white space, or you can specifying a field separator (FS). The fields are denoted $1, $2, ..., while $0 refers to the entire line. If there is no FS, the input line is split into one field per character.

The `awk` program has some internal environment variables that are useful (more exist and change upon platform)

* `NF` – number of fields in the current record
* `NR` – number of the current record (somewhat similar to row number)
* `FS` – regular expression used to separate fields; also settable by option -Ffs (default whitespace)
* `RS` – input record separator (default newline)
* `OFS` – output field separator (default blank)
* `ORS` – output record separator (default newline)

`awk` also supports more complex statements, some examples are below: 

* if (expression) statement [ else statement ]
* while (expression) statement
* for (expression ; expression ; expression) statement
* for (var in array) statement
* do statement while (expression)

Please note that awk is a language on it's own, and we will only be looking at some examples os its usage.

### Examples `awk` usage <a name="example3"></a>

```bash
$ awk '{print $3}' reference_data/chr1-hg19_genes.gtf | head

$ awk '{print $3 | "sort -u"}' reference_data/chr1-hg19_genes.gtf 

$ awk '{OFS = "\t" ; if ($3 == "stop_codon") print $1,$4,$5,$3,$10}' reference_data/chr1-hg19_genes.gtf | head
$ awk '{OFS = "\t" ; if ($3 == "stop_codon") print $1,$4,$5,$3,$10}' reference_data/chr1-hg19_genes.gtf | sed 's/"//g' | sed 's/;//g' | head

$ awk -F "\t" '{print $10}' reference_data/chr1-hg19_genes.gtf | head
$ awk -F "\t" '{print $9}' reference_data/chr1-hg19_genes.gtf | head

# head other/bad-reads.count.summary
$ awk -F ":" 'NR > 1 {sum += $2} END {print sum}' other/bad-reads.count.summary

# head ../rnaseq/results/counts/Mov10_featurecounts.Rmatrix.txt
$ awk 'NR > 1 {sum += $2} END {print sum}' ../rnaseq/results/counts/Mov10_featurecounts.Rmatrix.txt
```

***
*These materials are adapted from training materials generated by [FAS Reseach Computing at Harvard University](https://www.rc.fas.harvard.edu/training/training-materials/).*
