# Make hierarchical ID codes

Generate hierarchical ID codes for barcode labels. Hierarchical codes
have a nested structure: e.g. Y subsamples from each of X individuals.
Use
[`uniqID_maker`](https://docs.ropensci.org/baRcodeR/reference/uniqID_maker.md)
for sequential single-level labels. Can be run in interactive mode,
prompting user for input. The data.frame can be saved as CSV for (i) the
[`create_PDF`](https://docs.ropensci.org/baRcodeR/reference/create_PDF.md)
function to generate printable QR-coded labels; and (ii) to downstream
data collection using spreadsheet, relational database, etc.

## Usage

``` r
uniqID_hier_maker(user = FALSE, hierarchy, end = NULL, digits = 2)
```

## Arguments

- user:

  logical. Run function using interactive mode (prompts user for
  parameter values). Default is `FALSE`

- hierarchy:

  list. A list with each element consisting of three members a vector of
  three elements (string, beginning value, end value). See examples.
  Used only when `user=FALSE`)

- end:

  character. A string to be appended to the end of each label.

- digits:

  numerical. Default is `2`. Number of digits to be printed, adding
  leading 0s as needed. This will apply to all levels when `user=FALSE`.
  When the max number of digits in the ID code is greater than number of
  digits defined in `digits`, then `digits` is automatically increased
  to avoid errors.

## Value

data.frame of text labels in the first column, with additional columns
for each level in the hierarchy list, as defined by the user.

## See also

[`uniqID_maker`](https://docs.ropensci.org/baRcodeR/reference/uniqID_maker.md)

## Examples

``` r
if(interactive()){
## for interactive mode
uniqID_hier_maker(user = TRUE)
}

## how to make hierarchy list

## create vectors for each level in the order string_prefix, beginning_value,
## end_value and combine in list

a <- c("a", 3, 6)
b <- c("b", 1, 3)
c <- list(a, b)
Labels <- uniqID_hier_maker(hierarchy = c)
Labels
#>      label   a   b
#> 1  a03-b01 a03 b01
#> 2  a03-b02 a03 b02
#> 3  a03-b03 a03 b03
#> 4  a04-b01 a04 b01
#> 5  a04-b02 a04 b02
#> 6  a04-b03 a04 b03
#> 7  a05-b01 a05 b01
#> 8  a05-b02 a05 b02
#> 9  a05-b03 a05 b03
#> 10 a06-b01 a06 b01
#> 11 a06-b02 a06 b02
#> 12 a06-b03 a06 b03

## add string at end of each label
Labels <- uniqID_hier_maker(hierarchy = c, end = "end")
Labels
#>         label   a   b
#> 1  a03-b01end a03 b01
#> 2  a03-b02end a03 b02
#> 3  a03-b03end a03 b03
#> 4  a04-b01end a04 b01
#> 5  a04-b02end a04 b02
#> 6  a04-b03end a04 b03
#> 7  a05-b01end a05 b01
#> 8  a05-b02end a05 b02
#> 9  a05-b03end a05 b03
#> 10 a06-b01end a06 b01
#> 11 a06-b02end a06 b02
#> 12 a06-b03end a06 b03
```
