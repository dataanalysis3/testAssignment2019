Test statistical assignment
================
Alexey Bessudnov (add your student number here in the parentheses)
21 January 2019

Introduction
------------

Please change the author and date fields above as appropriate. Do not fotget to add your student number. Do not change the output format. Once you have completed the assignment you want to knit your document into a markdown document in the "github\_document" format and then commit both the .Rmd and .md files (and all the associated files with graphs) to your private assignment repository on Github.

Reading data (40 points)
------------------------

First, we need to read the data into R. For this assignment, I ask you to use data from the youth self-completion questionnaire (completed by children between 10 and 15 years old) from Wave 8 of the Understanding Society. It is one of the files you have downloaded as part of SN6614 from the UK Data Service. To help you find and understand this file you will need the following documents:

1.  The Understanding Society Waves 1-8 User Guide: <https://www.understandingsociety.ac.uk/sites/default/files/downloads/documentation/mainstage/user-guides/mainstage-user-guide.pdf>
2.  The youth self-completion questionnaire from Wave 8: <https://www.understandingsociety.ac.uk/sites/default/files/downloads/documentation/mainstage/questionnaire/wave-8/W8-youth-questionnaire.pdf>
3.  The codebook for the file: <https://www.understandingsociety.ac.uk/documentation/mainstage/dataset-documentation/wave/8/datafile/h_youth>

``` r
library(tidyverse)
```

    ## -- Attaching packages ----------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.1.0     v purrr   0.2.5
    ## v tibble  2.0.1     v dplyr   0.7.8
    ## v tidyr   0.8.2     v stringr 1.3.1
    ## v readr   1.3.1     v forcats 0.3.0

    ## -- Conflicts -------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
# This attaches the tidyverse package. If you get an error here you need to install the package first. 

h_youth <- read_tsv("C:/Users/ab789/datan3_2019/data/UKDA-6614-tab/tab/ukhls_w8/h_youth.tab")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double()
    ## )

    ## See spec(...) for full column specifications.

``` r
# You need to add between the quotation marks a full path to the required file on your computer.
```

Tabulate variables (10 points)
------------------------------

In the survey children were asked the following question: "Do you have a social media profile or account on any sites or apps?". In this assignment we want to explore how the probability of having an account on social media depends on children's age and gender.

Tabulate three variables: children's gender, age (please use derived variables) and having an account on social media.

``` r
# add your code here

table(h_youth$h_sex_dv)
```

    ## 
    ##    1    2 
    ## 1619 1651

``` r
table(h_youth$h_age_dv)
```

    ## 
    ##  10  11  12  13  14  15  16 
    ## 553 512 531 584 483 602   5

``` r
table(h_youth$h_ypsocweb)
```

    ## 
    ##   -9    1    2 
    ##   11 2634  625

Recode variables (10 points)
----------------------------

We want to create a new binary variable for having an account on social media so that 1 means "yes", 0 means "no", and all missing values are coded as NA. We also want to recode gender into a new variable with the values "male" and "female" (this can be a character vector or a factor).

``` r
h_youth2 <- h_youth %>%
        mutate(socmedia = recode(h_ypsocweb,
                                 `1` = 1L, `2` = 0L, `-9` = NA_integer_)) %>%
        mutate(gender = recode(h_sex_dv, `1` = "male", `2` = "female"))
```

Calculate means (10 points)
---------------------------

Produce code that calculates probabilities of having an account on social media (i.e. the mean of your new binary variable produced in the previous problem) by age and gender (please filter out children who are aged 16).

``` r
summary <- h_youth2 %>%
        filter(h_age_dv < 16) %>%
        group_by(h_age_dv, gender) %>%
        summarise(
                meanSocMedia = mean(socmedia, na.rm = TRUE)
        )
summary
```

    ## # A tibble: 12 x 3
    ## # Groups:   h_age_dv [?]
    ##    h_age_dv gender meanSocMedia
    ##       <dbl> <chr>         <dbl>
    ##  1       10 female        0.542
    ##  2       10 male          0.482
    ##  3       11 female        0.720
    ##  4       11 male          0.658
    ##  5       12 female        0.885
    ##  6       12 male          0.809
    ##  7       13 female        0.917
    ##  8       13 male          0.879
    ##  9       14 female        0.972
    ## 10       14 male          0.912
    ## 11       15 female        0.970
    ## 12       15 male          0.929

Write short interpretation (10 points)
--------------------------------------

Probability of having an account on social media increses with age. At each age, girls are more likely to have an account than boys.

Visualise results (20 points)
-----------------------------

Create a statistical graph (only one, but it can be faceted) illustrating your results (i.e. showing how the probability of having an account on social media changes with age and gender). Which type of statistical graph would be most appropriate for this?

``` r
summary %>%
        ggplot(aes(x = h_age_dv, y = meanSocMedia, colour = gender)) +
        geom_line() +
        xlab("Age") +
        ylab("proportion with an account on soc media")
```

![](testAssignment_files/figure-markdown_github/unnamed-chunk-5-1.png)

Conclusion
----------

This is a test formative assignment and the mark will not count towards your final mark. If you cannot answer any of the questions above this is fine -- we are just starting this module! However, please do submit this assignment in any case to make sure you understand the procedure, it works correctly and we do not have any problems with summative assignments later.
