# Make barcodes and print labels

Input vector or data.frame of ID codes to produce a PDF of QR codes
which can be printed. This is a wrapper function for
[`custom_create_PDF`](https://docs.ropensci.org/baRcodeR/reference/custom_create_PDF.md).
See details of
[`custom_create_PDF`](https://docs.ropensci.org/baRcodeR/reference/custom_create_PDF.md)
on how to format text labels if needed.

## Usage

``` r
create_PDF(
  user = FALSE,
  Labels = NULL,
  name = "LabelsOut",
  type = "matrix",
  ErrCorr = "H",
  Fsz = 12,
  ...
)
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

- ...:

  advanced arguments to modify the PDF layout. See
  [`custom_create_PDF`](https://docs.ropensci.org/baRcodeR/reference/custom_create_PDF.md)
  for arguments. The advanced options can be accessed interactively with
  `user = TRUE` and then entering TRUE when prompted to modify advanced
  options.

## Value

a PDF file containing QR-coded labels, saved to the default directory.

## Details

The default PDF setup is for ULINE 1.75" \* 0.5" WEATHER RESISTANT LABEL
for laser printer; item \# S-19297 (uline.ca). The page format can be
modified using the `...` (advanced arguments) for other label types.

## See also

[`custom_create_PDF`](https://docs.ropensci.org/baRcodeR/reference/custom_create_PDF.md)

## Examples

``` r
## data frame
example_vector <- as.data.frame(c("ao1", "a02", "a03"))

if (FALSE) { # \dontrun{
## run with default options
## pdf file will be "example.pdf" saved into a temp directory

temp_file <- tempfile()

create_PDF(Labels = example_vector, name = temp_file)

## view example output from temp folder
system2("open", paste0(temp_file, ".pdf"))
} # }

## run interactively. Overrides default pdf options
if(interactive()){
    create_PDF(user = TRUE, Labels = example_vector)
}

if (FALSE) { # \dontrun{
## run using a data frame, automatically choosing the "label" column
example_df <- data.frame("level1" = c("a1", "a2"), "label" = c("a1-b1",
"a1-b2"), "level2" = c("b1", "b1"))
create_PDF(user = FALSE, Labels = example_df, name = file.path(tempdir(), "example_2"))
} # }

if (FALSE) { # \dontrun{
## run using an unnamed data frame
example_df <- data.frame(c("a1", "a2"), c("a1-b1", "a1-b2"), c("b1", "b1"))
## specify column from data frame
create_PDF(user = FALSE, Labels = example_df[,2], name = file.path(tempdir(), "example_3"))
} # }
if (FALSE) { # \dontrun{
## create linear (code128) label rather than matrix (2D/QR) labels
example_df <- data.frame(c("a1", "a2"), c("a1-b1", "a1-b2"), c("b1", "b1"))
## specify column from data frame
create_PDF(user = FALSE, Labels = example_df, name = file.path(tempdir(),
"example_4", type = "linear"))
} # }
```
