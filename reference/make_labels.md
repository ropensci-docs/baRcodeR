# baRcodeR GUI

This addin will allow you to interactive create ID codes and generate
PDF files of QR codes.

## Usage

``` r
make_labels()
```

## Value

Opens RStudio addin gadget window for making labels and barcodes in a
GUI

## Examples

``` r
if(interactive()){
library(baRcodeR)
make_labels()
}
```
