# Using baRcodeR with command line prompts

BaRcodeR is an open-source program to facilitate repeatable label
generation and management for labelling, tracking and curating data from
biological samples.

Flowchart of major functions
![drawing](https://raw.githubusercontent.com/yihanwu/baRcodeR/master/man/figures/Flowchart.png)

For a quick start, see the
[introduction](https://docs.ropensci.org/baRcodeR/).

## Cheat Sheet

A 2-page, quick-reference guide is available via
[Figshare](https://dx.doi.org/10.6084/m9.figshare.7043309)

## Overview

Creating unique, scannable 2-D barcodes involves two steps:

1.  Generate unique ID codes with
    [`uniqID_maker()`](https://docs.ropensci.org/baRcodeR/reference/uniqID_maker.md)
    or
    [`uniqID_hier_maker()`](https://docs.ropensci.org/baRcodeR/reference/uniqID_hier_maker.md)
2.  Create a PDF file containing unique ID codes coupled with 2D barcode
    using
    [`create_PDF()`](https://docs.ropensci.org/baRcodeR/reference/create_PDF.md)

If you already have ID codes saved in a CSV file, skip step \#1 and
import directly as a
[`data.frame()`](https://rdrr.io/r/base/data.frame.html) object for step
\#2: see [Create barcodes](#create-barcodes).

For using the baRcodeR RStudio addin, refer to the package vignette *Use
baRcodeR addin* through
[`vignette("use-addin", package="baRcodeR")`](https://docs.ropensci.org/baRcodeR/articles/use-addin.md).

The functions in `baRcodeR` will accept parameters in two ways: 1) On
the command line as part of the function, and 2) interactively through
user input when `user=TRUE` is specified as part of the function. This
vignette will cover the two different ways to obtain the same text
identifiers and PDF sheets needed for printing.

## Create simple ID codes

Simple ID codes can be generated using the
[`uniqID_maker()`](https://docs.ropensci.org/baRcodeR/reference/uniqID_maker.md)
function. One-level ID codes consist of two parts, a leading string
shared by all samples and a number unique to each sample.

For example, if we want to generate basic ID codes for 5 individuals:

    example005
    example006
    example007
    example008
    example009
    example010

First, load the `baRcodeR` library.

``` r

library(baRcodeR)
```

    ## Loading required package: qrcode

### By user prompt

Run the `uniqID_make(r))` function in interactive mode.

``` r

IDcodes <- uniqID_maker(user = TRUE)
```

> NOTE: When typing into the console for user prompts, strings should
> not be quoted.

User prompts will appear in the console. Inputs received from the user
are passed as parameters to the
[`uniqID_maker()`](https://docs.ropensci.org/baRcodeR/reference/uniqID_maker.md)
to create the object `IDcodes`, which is a
[`data.frame()`](https://rdrr.io/r/base/data.frame.html) object
containing a vector of unique IDs in the first column, and additional
columns for strings and individual numbers provided by the user.

> `Please enter string for level:`

The string entered here defines the leading part of the ID code that
will be the same for each individual code. This can be the name of a
population, a species or a location. In this example, the string entered
by the user into the console as denoted by the `>` symbol is “example”.

> `Please enter string for level:` example

The second user prompt is:

> `Please enter the starting number for level (integer):`

The prompt asks for the starting number to generate unique IDs. These
codes do not have to start from 1.

> `Please enter the starting number for level (integer):` 5

The third user prompt is:

> `Enter the ending number for level:`

The prompt asks for the ending number. Unique IDs are generated
sequentially from the starting number to the ending number. Note that a
higher number can be used as the starting number to generate a reverse
order. It is possible to generate IDs that are not sequentially numbered
by passing a vector through
[`uniqID_maker()`](https://docs.ropensci.org/baRcodeR/reference/uniqID_maker.md)
(e.g. [`seq()`](https://rdrr.io/r/base/seq.html)) (see [By
arguments](#IDcode-maker-arguments))

> `Enter the ending number for level:` 10

After the starting and ending numbers are set, the function generates a
series of numbers. The user is then asked for the number of digits in
the unique ID.

> `Number of digits to print for level:`

This number must be \>= the maximum number of digits in the unique ID
code and will add leading zeros as needed. This is particularly useful
for generating a smaller number ID codes that are expected to be part of
a much larger sample set.

> `Number of digits to print for level:` 3

### By arguments

It is also possible to create unique ID codes directly, without user
prompts, by defining parameters in `uniqID_maker`. The interactive
example above can be reproduced a single line of code:

``` r

IDcodes <- uniqID_maker(string = "example", level = 5:10, digits = 3)
IDcodes
```

    ##        label ind_string ind_number
    ## 1 example005    example        005
    ## 2 example006    example        006
    ## 3 example007    example        007
    ## 4 example008    example        008
    ## 5 example009    example        009
    ## 6 example010    example        010

More complicated, non-sequential ID codes can be generated via the
`levels` parameter:

``` r

number_sequence <- seq(1, 10, 2)
IDcodes <- uniqID_maker(string = "example", level = number_sequence, digits = 3)
```

The output will then be:

ID codes can be saved to a text file for use with other programs
(e.g. spreadsheet):

``` r

write.csv(IDcodes, "IDcodes.csv")
```

## Create hierarchical ID codes

`uniqID_hier_maker` is used to make unique ID codes that follow a
hierarchical (i.e. nested) structure, for example we might sample B
individuals from A populations at each of C time points. Similar to
`uniqID_maker`, this function can be run interactively or by directly
defining parameters. In contrast to `uniqID_maker`, `uniqID_hier_maker`
is used to generate nested pairs of strings and unique ID codes. Below
is an example of a list of hierarchical identifier codes with three
levels (a, b, c) and varying numbers of individuals for each level (a=3,
b=2, c=2).

    a1-b1-c1
    a1-b1-c2
    a1-b2-c1
    a1-b2-c2
    a2-b1-c1
    a2-b1-c2
    a2-b2-c1
    a2-b2-c2
    a3-b1-c1
    a3-b1-c2
    a3-b2-c1
    a3-b2-c2

### By user prompts

To create hierarchical ID codes in interactive mode, start with the
argument `user=T` in the function.

``` r

IDcodes <- uniqID_hier_maker(user = TRUE)
```

The first prompt that appears in the console is:

> `What is the number of levels in hierarchy:`

In this example, we have levels a, b, and c; so three levels in total.

> `What is the # of levels in hierarchy:` 3

The second prompt asks if a string should be appended to the end of the
IDs.

> `String at end of label:`

> `1 : Yes`

> `2 : No`

There are only two possible inputs, 1 or 2. Typing any other number will
result in an “invalid input” warning.

In this example, there is no ending string.

> `String at end of label:`

> `1 : Yes`

> `2 : No`

> 2

A series of prompts will repeat for each level, allowing the user to set
the number of digits to be printed, the leading string, the starting
number, and the ending number. These are similar to `uniqID_maker`(see
[uniqID_maker](#uniqID-maker-user-prompt) for step by step
instructions). The number of digits to print applies to all levels.

### By argument

Instead of interactive mode, it is possible to define arguments directly
into `uniqID_hier_maker`. Unlike `uniqID_maker`, a `list` object is
required to specify the parameters in each level of the hierarchy.
First, define a vector for each level with three parameters: the leading
string, the start value, and the end value. Second, combine vectors into
a `list` object in the order of their hierarchy. For the example below,
‘c’ is nested within ‘b’ within ‘a’.

``` r

level_one <- c("a", 1, 3)
level_two <- c("b", 1, 2)
level_three <- c("c", 10, 12)
hier_list <- list(level_one, level_two, level_three)
```

You can specify a custom suffix string for all ID codes using the `end`
argument, and the number of digits to print for all levels through the
`digits` argument. It is not possible at this time to vary the number of
digits printed for each level, however this can be done using
interactive mode (i.e. user=T).

The list can then be passed into `uniqID_hier_maker` to generate the
unique ID codes.

``` r

IDcodes <- uniqID_hier_maker(hierarchy = hier_list, digits = 1)
```

    ## Warning in uniqID_hier_maker(hierarchy = hier_list, digits = 1): Digits
    ## specified less than max level number. Increasing number of digits for level

The output will be:

``` r

IDcodes
```

    ##        label  a  b   c
    ## 1  a1-b1-c10 a1 b1 c10
    ## 2  a1-b1-c11 a1 b1 c11
    ## 3  a1-b1-c12 a1 b1 c12
    ## 4  a1-b2-c10 a1 b2 c10
    ## 5  a1-b2-c11 a1 b2 c11
    ## 6  a1-b2-c12 a1 b2 c12
    ## 7  a2-b1-c10 a2 b1 c10
    ## 8  a2-b1-c11 a2 b1 c11
    ## 9  a2-b1-c12 a2 b1 c12
    ## 10 a2-b2-c10 a2 b2 c10
    ## 11 a2-b2-c11 a2 b2 c11
    ## 12 a2-b2-c12 a2 b2 c12
    ## 13 a3-b1-c10 a3 b1 c10
    ## 14 a3-b1-c11 a3 b1 c11
    ## 15 a3-b1-c12 a3 b1 c12
    ## 16 a3-b2-c10 a3 b2 c10
    ## 17 a3-b2-c11 a3 b2 c11
    ## 18 a3-b2-c12 a3 b2 c12

The data frame contains ID codes in the first column, and a separate
column for each level of the hierarchy, with the user-defined string as
the header. This can be saved to a CSV:

``` r

write.csv(IDcodes, "IDcodes.csv")
```

This file is useful for archiving ID codes and as a starting point for
data entry. For example, it can be opened in a spreadsheet program to
add data measurement columns. It is also the input for creating
printable, QR-coded labels with `create_PDF`.

## Create barcodes

2D barcodes (i.e. QR codes) paired with ID code strings are created from
an input vector of text labels. Users can manually create their own ID
codes as a vector, as the first column of an existing
[`data.frame()`](https://rdrr.io/r/base/data.frame.html) object, or as a
[`data.frame()`](https://rdrr.io/r/base/data.frame.html) from
`<-uniqID_maker` or `<-uniqID_hier_maker`.

The function
[`create_PDF()`](https://docs.ropensci.org/baRcodeR/reference/create_PDF.md)
produces a pdf file containing barcodes that fit to the dimensions of
ULINE 1.75” \* 0.5” WEATHER RESISTANT LABEL for laser printer; item \#
S-19297 (uline.ca). If needed, the page setup can be modified using
advanced options in [custom_create_PDF](#custom-create-pdf).

The first step is to read in the vector of ID codes, in this example
from a CSV file:

``` r

# Reading in from a csv file
IDcodes<-read.csv("IDcodes.csv")
```

### By user prompt

In the following example, the IDcodes data.frame object is used to
create a PDF file called “example.pdf”, with a font size of 3.5, and an
error correction level “Q” meaning the barcode can tolerate up to 25%
damage. The parameter `user=T` will prompt the user to guide creation of
the pdf file containing scannable barcodes.

If `IDcodes` is a vector, the vector will be directly used to generate
barcodes. If `IDcodes` is a data frame, the function will use the column
called `label` or else the first column in the data frame.

``` r

create_PDF(user=TRUE, Labels=IDcodes)
```

A user prompt is printed into the console. For example:

> `Please enter name for PDF output file:`

Any combination of letters and numbers can be used for the name of the
pdf file. Here, the file name is set to “example.”

> `Please enter name for PDF output file:` example

The next user prompt is to set the size of the text printed on each
barcode.

> `Please enter a font size:`

This font size is the point size and will apply to the size of the text
on the PDF. For example, entering 12 will create 12 point font on the
PDF.

> `Please enter a font size:` 12

The last basic parameter to set is the error correction level. There are
four possible levels: L, M, Q, and H.

Level “L” - up to 7% damage – ideal for very small labels (large pixels)
Level “M” - up to 15% damage Level “Q” - up to 25% damage Level “H” - up
to 30% damage – good for bigger labels (small pixels)

The user prompt for error correction level is similar to previous
prompts.

> `Select an error correction level.`

> `1 : L (up to 7% damage)`

> `2 : M (up to 15% damage)`

> `3 : Q (up to 25% damage)`

> `4 : H (up to 30% damage)`

This example uses an error correction level “Q” so `3` should be
entered.

> `Select an error correction level.`

> `1 : L (up to 7% damage)`

> `2 : M (up to 15% damage)`

> `3 : Q (up to 25% damage)`

> `4 : H (up to 30% damage)`

> 3

The last user prompt asks whether the user wants to modify the advanced
parameters.

> `Edit advanced parameters?`

> `1 : Yes`

> `2 : No`

In this example, no advanced parameters are modified (input `2`). Using
advanced parameters are covered in [advanced
options](#custom-create-pdf)

> `Edit advanced parameters?`

> `1 : Yes`

> `2 : No`

> 2

### By arguments

The same example above can be reproduced directly with the following
parameters:

``` r

create_PDF(Labels = IDcodes, name = "example", ErrCorr = "Q", Fsz = 2.5)
```

## Advanced Options for pdf output

There are advanced options for the pdf output which can be accessed
interactively or by specifying additional arguments in `create_PDF`. The
user prompts are similar to the ones shown above but allow customization
of the output document for other printing formats. Documentation of the
advanced options can be found using through the man page
[`?custom_create_PDF`](https://docs.ropensci.org/baRcodeR/reference/custom_create_PDF.md).

Arguments can be passed from `create_PDF` to `custom_create_PDF` as
`create_PDF` is just a wrapper for `custom_create_PDF`.

``` r

## This will create a pdf sheet where the labels are printed in columns then rows. It will skip 3 rows from the top and 1 column from the left. 
create_PDF(Labels = Labels, name = "example_advanced", ErrCorr = "Q", Fsz = 2.5, Across = F, ERows = 3, ECol = 1)
```

## Formatting of text labels

Trying to format text labels in the baRcodeR GUI is not recommended.

### New Lines

It is possible to force formatting of labels by inserting `\n` as line
breaks.

### Tabs

Using `\t` will create error due to the underlying `qrcode` library used
in this package. One solution is to use the space character in place of
tabs.

``` r

# original label
X <- "text\ttext"
cat(X)
```

    ## text text

``` r

cat(gsub("\\t", "\x20\x20\x20\x20", X))
```

    ## text    text

### When reading from file

R automatically escapes the escape characters when reading from file.
For example, `text\ntext` will be read in as `text\\ntext` from
`read.csv`.

Use a global substitution to replace the double slashes.

``` r

X <- "text\\ntext"
cat(X)
```

    ## text\ntext

``` r

cat(gsub("\\\\n","\n",X))
```

    ## text
    ## text
