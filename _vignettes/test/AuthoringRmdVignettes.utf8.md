---
title: "Authoring R Markdown vignettes"
author: Andrzej Oleś, Wolfgang Huber and Martin Morgan
date: "Last update: 22 October, 2017" 
output:
  BiocStyle::html_document:
    toc_float: true
    code_folding: show
  BiocStyle::pdf_document: default
package: systemPipeR 1.10.0
vignette: |
  %\VignetteIndexEntry{Authoring R Markdown vignettes}
  %\VignetteEngine{knitr::rmarkdown}
  %\VignetteEncoding{UTF-8}
bibliography: bibtex.bib
---


<!--
- Compile from command-line
Rscript -e "rmarkdown::render('AuthoringRmdVignettes.Rmd', c('BiocStyle::html_document'), clean=F); knitr::knit('AuthoringRmdVignettes.Rmd', tangle=TRUE)"; Rscript ../md2jekyll.R AuthoringRmdVignettes.knit.md 7; Rscript -e "rmarkdown::render('AuthoringRmdVignettes.Rmd', c('BiocStyle::pdf_document'))"
-->


# Prerequisites

_Bioconductor_ R Markdown format is build on top of _R_ package *[bookdown](https://CRAN.R-project.org/package=bookdown)*, which in turn relies on *[rmarkdown](https://CRAN.R-project.org/package=rmarkdown)* and [pandoc](http://johnmacfarlane.net/pandoc/) to compile the final output document.
Therefore, unless you are using RStudio, you will need a recent version of [pandoc](http://johnmacfarlane.net/pandoc/) (>= 1.17.2). See the [pandoc installation instructions](https://github.com/rstudio/rmarkdown/blob/master/PANDOC.md) for details on installing pandoc for your platform.


# Getting started

To enable the _Bioconductor_ style in your R Markdown vignette you need to [@H_Backman2016-bt]:

- Edit the `DESCRIPTION` file by adding

        VignetteBuilder: knitr
        Suggests: BiocStyle, knitr, rmarkdown

- Specify `BiocStyle::html_document` or `BiocStyle::pdf_document` as output format and add vignette metadata in the document header:

        ---
        title: "Vignette Title"
        author: "Vignette Author"
        package: PackageName
        output: 
          BiocStyle::html_document
        vignette: >
          %\VignetteIndexEntry{Vignette Title}
          %\VignetteEngine{knitr::rmarkdown}
          %\VignetteEncoding{UTF-8}  
        ---

The `vignette` section is required in order to instruct _R_ how to build the vignette.^[`\VignetteIndexEntry` should match the `title` of your vignette] The `package` field which should contain the package name is used to print the package version in the output document header. It is not necessary to specify `date` as by default the document compilation date will be automatically included.
See the following section for details on specifying author affiliations and abstract.

BiocStyle's `html_document` and `pdf_document` format functions extend the corresponding original _rmarkdown_ formats, so they accept the same arguments as `html_document` and `pdf_document`, respectively. For example, use `toc_float: true` to obtain a floating TOC as in this vignette.

## Use with R markdown v1

Apart from the default markdown engine implemented in the *[rmarkdown](https://CRAN.R-project.org/package=rmarkdown)* package,
it is also possible to compile _Bioconductor_ documents with the older markdown v1 engine 
from the package *[markdown](https://CRAN.R-project.org/package=markdown)*. There are some
differences in setup and the resulting output between these two
engines. 

To use the *[markdown](https://CRAN.R-project.org/package=markdown)* vignette builder engine:

- Edit the `DESCRIPTION` file to include

        VignetteBuilder: knitr
        Suggests: BiocStyle, knitr
        
- Specify the vignette engine in the `.Rmd` files (inside HTML
  comments)

        <!--
        %% \VignetteEngine{knitr::knitr}
        -->

- Add the following code chunk at the beginning of your `.Rmd`
  vignettes

        ```{r style, echo = FALSE, results = 'asis'}
        BiocStyle::markdown()
        ```

The way of attaching CSS files when using
*[markdown](https://CRAN.R-project.org/package=markdown)* differs from how this is done with *[rmarkdown](https://CRAN.R-project.org/package=rmarkdown)*.
In the former case additional style sheets can be
used by providing them to the `BiocStyle::markdown` function.
To include `custom.css` file use

    ```{r style, echo = FALSE, results = 'asis'}
    BiocStyle::markdown(css.files = c('custom.css'))
    ```

# Document header

## Author affiliations

The `author` field allows for specifing author names along with affiliation and email information.

In the basic case, when no additional information apart from author names is provided, these can be entered as a single character string

    author: "Single Author"
    
or a list

    author:
    - First Author
    - Second Author
    - Last Author
    
which will print as "First Author, Second Author and Last Author".

Author affiliations and emails can be entered in named sublists of the author list. Multiple affiliations per author can be specified this way.

    author:
    - name: First Author
      affiliation: 
      - Shared affiliation
      - Additional affiliation
    - name: Second Author
      affiliation: Shared affiliation
      email: corresponding@author.com

A list of unique affiliations will be displayed below the authors, similar as in this document.

For clarity, compactness, and to avoid errors,
repeated nodes in YAML header can be initially denoted by an anchor entered with an ampersand &,
and later referenced with an asterisk *. For example, the above affiliation metadata is equivalent to the shorthand notation

    author:
    - name: First Author
      affiliation: 
      - &id Shared affiliation
      - Additional affiliation
    - name: Second Author
      affiliation: *id
      email: corresponding@author.com


## Abstract and running headers

Abstract can be entered in the corresponding field of the document front matter,
as in the example below.

    ---
    title: "Full title for title page"
    shorttitle: "Short title for headers"
    author: "Vignette Author"
    package: PackageName
    abstract: >
      Document summary
    output: 
      BiocStyle::pdf_document
    ---

The `shorttitle` option specifies the title used in running headers 
instead of the document title.^[only relevant to PDF output]


# Style macros



*[BiocStyle](http://bioconductor.org/packages/BiocStyle)* introduces the following macros useful when
referring to _R_ packages:

* `Biocpkg("IRanges")` for _Bioconductor_ software, annotation and experiment data packages,
  including a link to the release landing page or if the package is only in devel, to the devel landing page, *[IRanges](http://bioconductor.org/packages/IRanges)*.

* `CRANpkg("data.table")` for _R_ packages available on
  CRAN, including a link to the FHCRC CRAN mirror landing page, *[data.table](https://CRAN.R-project.org/package=data.table)*.

* `Githubpkg("rstudio/rmarkdown")` for _R_ packages
  available on GitHub, including a link to the package repository, *[rmarkdown](https://github.com/rstudio/rmarkdown)*.

* `Rpackage("MyPkg")` for _R_ packages that are _not_
  available on _Bioconductor_, CRAN or GitHub; *MyPkg*.

These are meant to be called inline, e.g., `` `r Biocpkg("IRanges")` ``.


# Code chunks

The line length of output code chunks is set to the optimal width of typically 
80 characters, so it is not neccessary to adjust it manually through `options("width")`.


# Figures

_BiocStyle_ comes with three predefined figure sizes. Regular figures not otherwise specified appear indented with respect to the paragraph text, as in the example below.


```r
plot(cars)
```

<img src="AuthoringRmdVignettes_files/figure-html/no-cap-1.png" width="100%" />

Figures which have no captions are just placed wherever they were generated in the _R_ code.
If you assign a caption to a figure via the code chunk option `fig.cap`, the plot will be automatically labeled and numbered^[for PDF output it will be placed in a floating figure environment], and it will be also possible to reference it. These features are provided by *[bookdown](https://CRAN.R-project.org/package=bookdown)*, which defines a format-independent syntax for specifying cross-references, see Section \@ref(cross-references). The figure label is generated from the code chunk label^[for cross-references to work the chunk label may only contain alphanumeric characters (a-z, A-Z, 0-9), slashes (/), or dashes (-)] by prefixing it with `fig:`, e.g., the label of a figure originating from the chunk `foo` will be `fig:foo`. To reference a figure, use the syntax `\@ref(label)`^[do not forget the leading backslash!], where `label` is the figure label, e.g., `fig:foo`. For example, the following code chunk was used to produce Figure \@ref(fig:plot).

    ```{r plot, fig.cap = "Regular figure. The first sentence...", echo = FALSE}
    plot(cars)
    ```

<div class="figure">
<img src="AuthoringRmdVignettes_files/figure-html/plot-1.png" alt="Regular figure. The first sentence of the figure caption is automatically emphasized to serve as figure title." width="100%" />
<p class="caption">(\#fig:plot)Regular figure. The first sentence of the figure caption is automatically emphasized to serve as figure title.</p>
</div>

In addition to regular figures, *BiocStyle* provides small and wide figures which can be specified by `fig.small` and `fig.wide` code chunk options.
Wide figures are left-aligned with the paragraph and extend on the right margin, as Figure \@ref(fig:wide).
Small figures are meant for possibly rectangular plots which are centered with respect to the text column, see Figure \@ref(fig:small).

<div class="figure">
<img src="AuthoringRmdVignettes_files/figure-html/wide-1.png" alt="Wide figure. A plot produced by a code chunk with option `fig.wide = TRUE`." width="100%"  class="widefigure" />
<p class="caption">(\#fig:wide)Wide figure. A plot produced by a code chunk with option `fig.wide = TRUE`.</p>
</div>

<div class="figure">
<img src="AuthoringRmdVignettes_files/figure-html/small-1.png" alt="Small figure. A plot produced by a code chunk with option `fig.small = TRUE`." width="100%"  class="smallfigure" />
<p class="caption">(\#fig:small)Small figure. A plot produced by a code chunk with option `fig.small = TRUE`.</p>
</div>


# Tables

Like figures, tables with captions will also be numbered and can be referenced. The caption is entered as a paragraph starting with `Table:`^[or just `:`], which may appear either before or after the table. When adding labels, make sure that the label appears at the beginning of the table caption in the form `(\#tab:label)`, and use `\@ref(tab:label)` to refer to it. For example, Table \@ref(tab:table) has been produced with the following code.

```markdown
Fruit   | Price
------- | -----
bananas | 1.2
apples  | 1.0
oranges | 2.5
  
: (\#tab:table) A simple table. With caption.
```
        
Fruit   | Price
------- | -----
bananas | 1.2
apples  | 1.0
oranges | 2.5
  
: (\#tab:table) A simple table. With caption.
    
The function `knitr::kable()` will automatically generate a label for a table environment, which is the chunk label prefixed by `tab:`, see Table \@ref(tab:mtcars).


```r
knitr::kable(
  head(mtcars[, 1:8], 10), caption = 'A table of the first 10 rows of `mtcars`.'
)
```



Table: (\#tab:mtcars)A table of the first 10 rows of `mtcars`.

                      mpg   cyl    disp    hp   drat      wt    qsec   vs
------------------  -----  ----  ------  ----  -----  ------  ------  ---
Mazda RX4            21.0     6   160.0   110   3.90   2.620   16.46    0
Mazda RX4 Wag        21.0     6   160.0   110   3.90   2.875   17.02    0
Datsun 710           22.8     4   108.0    93   3.85   2.320   18.61    1
Hornet 4 Drive       21.4     6   258.0   110   3.08   3.215   19.44    1
Hornet Sportabout    18.7     8   360.0   175   3.15   3.440   17.02    0
Valiant              18.1     6   225.0   105   2.76   3.460   20.22    1
Duster 360           14.3     8   360.0   245   3.21   3.570   15.84    0
Merc 240D            24.4     4   146.7    62   3.69   3.190   20.00    1
Merc 230             22.8     4   140.8    95   3.92   3.150   22.90    1
Merc 280             19.2     6   167.6   123   3.92   3.440   18.30    1


# Equations

To number and reference equations, put them in equation environments and append labels to them following the syntax `(\#eq:label)`^[due to technical constraints equation labels must start with `eq:`], e.g.,

```latex
\begin{equation}
  f\left(k\right) = \binom{n}{k} p^k\left(1-p\right)^{n-k}
  (\#eq:binom)
\end{equation}
```
renders the equation below.

$$
  f\left(k\right) = \binom{n}{k} p^k\left(1-p\right)^{n-k}
$$

You may then refer to Equation \@ref(eq:binom) by `\@ref(eq:binom)`. Note that in HTML output only labeled equations will appear numbered.


# Cross-references

Apart from referencing figures (Section \@ref(figures)), tables (Section \@ref(tables)), and equations (Section \@ref(equations)), you can also use the same syntax `\@ref(label)` to reference sections, where `label` is the section ID. By default, Pandoc will generate IDs for all section headers, e.g., `# Hello World` will have an ID `hello-world`. In order to avoid forgetting to update the reference label after you change the section header, you may also manually assign an ID to a section header by appending `{#id}` to it.

When a referenced label cannot be found, you will see two question marks like \@ref(non-existing-label), as well as a warning message in the _R_ console when rendering the document.


# Margin notes

Footnotes are displayed as side notes on the right margin^[this is a side note entered as a footnote],
which has the advantage that they appear close to the place where they are defined. 


# Session info {.unnumbered}

Here is the output of `sessionInfo()` on the system on which this
document was compiled:


```
## R version 3.4.2 (2017-09-28)
## Platform: x86_64-pc-linux-gnu (64-bit)
## Running under: Ubuntu 14.04.5 LTS
## 
## Matrix products: default
## BLAS: /usr/lib/libblas/libblas.so.3.0
## LAPACK: /usr/lib/lapack/liblapack.so.3.0
## 
## locale:
##  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
##  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
##  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8   
##  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                 
##  [9] LC_ADDRESS=C               LC_TELEPHONE=C            
## [11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
## 
## attached base packages:
## [1] stats     graphics  utils     datasets  grDevices base     
## 
## other attached packages:
## [1] BiocStyle_2.5.41
## 
## loaded via a namespace (and not attached):
##  [1] compiler_3.4.2  backports_1.1.0 bookdown_0.4    magrittr_1.5   
##  [5] rprojroot_1.2   tools_3.4.2     htmltools_0.3.6 yaml_2.1.14    
##  [9] Rcpp_0.12.11    stringi_1.1.5   rmarkdown_1.6   highr_0.6      
## [13] knitr_1.16      methods_3.4.2   stringr_1.2.0   digest_0.6.12  
## [17] evaluate_0.10.1
```

# References
