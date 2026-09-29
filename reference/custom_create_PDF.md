# Make barcodes and print labels

Input a vector or data frame of ID codes to produce a PDF of barcode
labels that can then be printed. The PDF setup is for the ULINE 1.75" \*
0.5" WEATHER RESISTANT LABEL for laser printer; item \# S-19297
(uline.ca). See details for how to format text labels properly.

## Usage

``` r
custom_create_PDF(
  user = FALSE,
  Labels = NULL,
  name = "LabelsOut",
  type = "matrix",
  ErrCorr = "H",
  Fsz = 12,
  Across = TRUE,
  ERows = 0,
  ECols = 0,
  trunc = TRUE,
  numrow = 20,
  numcol = 4,
  page_width = 8.5,
  page_height = 11,
  width_margin = 0.25,
  height_margin = 0.5,
  label_width = NA,
  label_height = NA,
  x_space = 0,
  y_space = 0.5,
  alt_text = NULL,
  replace_label = FALSE,
  denote = c("\n(", ")")
)

qrcode_make(Labels, ErrCorr)

code_128_make(Labels)

code_128_make2(Labels)
```

## Arguments

- user:

  logical. Run function using interactive mode (prompts user for
  parameter values) Default is `FALSE`

- Labels:

  vector or data frame object containing label names (i.e. unique ID
  codes) with either UTF-8 or ASCII encoding.

- name:

  character. Name of the PDF output file. Default is `"LabelsOut"`. A
  file named `name.pdf` will be saved to the working directory by
  default. Use `"dirname/name"` to produce a file called `name.pdf` in
  the `dirname` directory.

- type:

  character. Choice of `"linear"` for code 128, `"linear2"` for extended
  code 128, `"matrix"` for QR code (i.e. 2D barcode) with text to the
  right, or `"matrix2"` for QR code with text above or below, depending
  on `y_space` value (0 = below, 1 = above).

- ErrCorr:

  error correction value for matrix labels only. Level of damage from
  low to high: `"L"`, `"M"`, `"Q"`, `"H"`. Default is `"H"`. See details
  for explanation of values.

- Fsz:

  numerical. Sets font size in points. Longer ID codes may be shrunk to
  fit if truncation is not used for matrix labels. Default font size is
  `5`. ID codes are also shrunk automatically to fit on the label if
  actual size is bigger than label dimensions.

- Across:

  logical. When `TRUE`, print labels across rows, left to right. When
  `FALSE`, print labels down columns, top to bottom. Default is `TRUE`.

- ERows:

  number of rows to skip. Default is `0`. Example: setting ERows to 6
  will begin printing at row 7. ERows and ECols are useful for printing
  on partially-used label sheets.

- ECols:

  number of columns to skip. Default is `0`. Example: setting ECols to 2
  will put the first label at column 3. ERows and ECols are useful for
  printing on partially-used label sheets.

- trunc:

  logical. Text is broken into multiple lines for longer ID codes, to
  prevent printing off of the label area. Default is `TRUE`. If
  `trunc = FALSE`, and text is larger than the physical label, the text
  will be shrunk down automatically.

- numrow:

  numerical. Number of rows per page. Default is `20`.

- numcol:

  numerical. Number of columns per page. Default is `4`.

- page_width:

  numerical. Width of page (in inches). Default is set to `8.5`.

- page_height:

  numerical. Height of page (in inches). Default is set to `11`.

- width_margin:

  numerical. The width margin of the page (in inches). Default is
  `0.25`.

- height_margin:

  numerical. The height margin of the page (in inches). Default is
  `0.5`.

- label_width:

  numerical. The width of label (in inches). Will be calculated as
  `(page_width - 2 * width_margin)/numcol` if `label_width` is set as
  `NULL`.

- label_height:

  numerical. The height of the label (in inches). Will be calculated as
  `(page_height - 2 * height_margin)/numrow` if `label_height` is set as
  `NULL`.

- x_space:

  numerical. A value between `0` and `1`. This sets the distance between
  the QR code and text of each label. Only applies when
  `type = "matrix"`. Default is `0`.

- y_space:

  numerical. The height position of the text on the physical label as a
  proportion of the label height. Only applies when `type = "matrix"` or
  `"matrix2"`. A value between `0` and `1`. Default is `0.5`.

- alt_text:

  vector containing alternative names that are printed along with Labels
  BUT ARE NOT ENCODED in the barcode image. Use with caution!

- replace_label:

  logical. Replace label text with `alt_text`. Generated barcode will
  contain more information than text label. Use with caution!

- denote:

  character (prefix) or vector of length 2 (prefix, suffix). Denotes
  alt_text that is not encoded in the barcode image. Default is brackets
  before and after ().

## Value

a PDF file containing QR-coded labels, saved to the default directory.

## Details

`qrcode_make` is the helper function for generating a QR code matrix.
`code_128_make` is the helper function for generating a linear barcode
according to code 128 set B. `code_128_make2` is the extended helper
function for generating a linear barcode according to code 128 set B,
all Latin-1 characters, set C and partially A. `custom_create_PDF` is
the main function which sets page layout, and creates the PDF file.

Correction levels for QR codes refer to the level of damage a label can
tolerate before the label become unreadable by a scanner (L = Low (7%),
M = Medium (15%), Q = Quantile (25%), H = High (30%)). So a label with L
correction can lose up to at most 7 while a H label can lose up to 30
can be printed at smaller sizes compared to H codes.

The escape characters `\n` and `\s` (and the hex equivalents `\x0A` and
`\x20` can be used to format text labels. Tab character `\t` (`\x09`)
does not work for QR codes and should be replaced by a number of space
characters. See the package vignette for examples.

If `ECol` or `ERow` is greater than `numcol` and `numrow`, the labels
will be printed starting on the second page.

## See also

[`create_PDF`](https://docs.ropensci.org/baRcodeR/reference/create_PDF.md)

## Examples

``` r

## this is the same examples used with create_PDF
## data frame
example_vector <- as.data.frame(c("ao1", "a02", "a03"))

if (FALSE) { # \dontrun{
## run with default options
## pdf file will be "example.pdf" saved into a temp directory
temp_file <- tempfile()

custom_create_PDF(Labels = example_vector, name = temp_file)

## view example output from temp folder
system2("open", paste0(temp_file, ".pdf"))
} # }

## run interactively. Overrides default pdf options
if(interactive()){
    custom_create_PDF(user = TRUE, Labels = example_vector)
}

if (FALSE) { # \dontrun{
## run using a data frame, automatically choosing the "label" column
example_df <- data.frame("level1" = c("a1", "a2"), "label" = c("a1-b1",
 "a1-b2"), "level2" = c("b1", "b1"))

custom_create_PDF(user = FALSE, Labels = example_df, name = file.path(tempdir(), 
 "example_2"))
 } # }
if (FALSE) { # \dontrun{
## run using an unnamed data frame
example_df <- data.frame(c("a1", "a2"), c("a1-b1", "a1-b2"), c("b1", "b1"))
## specify column from data frame
custom_create_PDF(user = FALSE, Labels = example_df[,2], name = file.path(tempdir(), "example_3"))
} # }
if (FALSE) { # \dontrun{
## create linear (code128) label rather than matrix (2D/QR) labels
example_df <- data.frame(c("a1", "a2"), c("a1-b1", "a1-b2"), c("b1", "b1"))
## specify column from data frame
custom_create_PDF(user = FALSE, Labels = example_df, name = file.path(tempdir(),
"example_4", type = "linear"))
} # }
if (FALSE) { # \dontrun{
## Include text for the user that is NOT encoded into the barcode image
## Excluded text is denoted with brackets by default
example_df <- data.frame(ID = floor(runif(3) * 10000), name = c("A", "B", "C"),
 dob = c("1/1/2020", "12/6/2001", "2/8/1986"))
 
## linear (1d) barcodes with custom denote parameter
custom_create_PDF(Labels = example_df$ID, alt_text = paste(example_df$name,
 example_df$dob), type = "linear", denote=".")
} # }  
```
