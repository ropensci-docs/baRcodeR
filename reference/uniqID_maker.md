# Generate a list of ID codes

Create ID codes consisting of a text string and unique numbers
(string001, string002, ...). Can be run in interactive mode, prompting
user for input. The data.frame output can be saved as CSV for (i) the
[`create_PDF`](https://docs.ropensci.org/baRcodeR/reference/create_PDF.md)
function to generate printable QR-coded labels; and (ii) to downstream
data collection software (spreadsheets, relational databases, etc.)

## Usage

``` r
uniqID_maker(
  user = FALSE,
  string = NULL,
  level,
  digits = 3,
  ending_string = NULL
)
```

## Arguments

- user:

  logical. Run function using interactive mode (prompts user for
  parameter values). Default is `FALSE`

- string:

  character. Text string for label. Default `null`.

- level:

  integer vector. Defines the numerical values to be appended to the
  character string. Can be any sequence of numbers (see examples).

- digits:

  numerical. Default is `2`. Number of digits to be printed, adding
  leading 0s as needed. This will apply to all levels when `user=FALSE`.
  When the numeric value of the label has a greater number of digits
  than `digits`, `digits` is automatically increased for the entire
  level. Default is `3`.

- ending_string:

  a character string or vector of strings to attach to the label. If a
  vector is used, all combinations of that vector with a unique label
  will be produced.

## Value

data.frame with text labels in the first column, along with string and
numeric values in two additional columns.

## Details

When the function is called with `user = TRUE`, a sequence of numbers is
generated between the starting and ending number provided by the user.
When `user = FALSE`, a vector of custom numbers can be provided. See
example below.

## See also

[`uniqID_hier_maker`](https://docs.ropensci.org/baRcodeR/reference/uniqID_hier_maker.md)

## Examples

``` r


## sequential string of numbers in label
Labels <- uniqID_maker(string = "string", level = c(1:5), digits = 2)
Labels
#>      label ind_string ind_number
#> 1 string01     string         01
#> 2 string02     string         02
#> 3 string03     string         03
#> 4 string04     string         04
#> 5 string05     string         05

## can also use nonsequential strings in input for levels
level <- c(1:5, 8:10, 999:1000)
Labels <- uniqID_maker(string = "string", level = level, digits = 4)
Labels
#>         label ind_string ind_number
#> 1  string0001     string       0001
#> 2  string0002     string       0002
#> 3  string0003     string       0003
#> 4  string0004     string       0004
#> 5  string0005     string       0005
#> 6  string0008     string       0008
#> 7  string0009     string       0009
#> 8  string0010     string       0010
#> 9  string0999     string       0999
#> 10 string1000     string       1000


## Using the ending_string to produce labels with unique endings
## this is different from hierarchical labels with two levels as there 
## is no numbering, just the text string


Labels <- uniqID_maker(string = "string", level = c(1:5), digits = 2, ending_string = "A")
Labels
#>        label ind_string ind_number end_string
#> 1 string01-A     string         01          A
#> 2 string02-A     string         02          A
#> 3 string03-A     string         03          A
#> 4 string04-A     string         04          A
#> 5 string05-A     string         05          A

Labels <- uniqID_maker(string = "string", level = c(1:5), 
                       digits = 2, ending_string = c("A", "B"))
Labels
#>         label ind_string ind_number end_string
#> 1  string01-A     string         01          A
#> 2  string02-A     string         02          A
#> 3  string03-A     string         03          A
#> 4  string04-A     string         04          A
#> 5  string05-A     string         05          A
#> 6  string01-B     string         01          B
#> 7  string02-B     string         02          B
#> 8  string03-B     string         03          B
#> 9  string04-B     string         04          B
#> 10 string05-B     string         05          B


if(interactive()){
## function using user prompt does not use any of the other parameters
Labels <- uniqID_maker(user = TRUE)
Labels
}
```
