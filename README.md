Superbowl Ads
================
by Dorian’s Gs

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.5     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

## Introduction

The dataset `superbowl` is created by FiveThirtyEight, originally
collected through superbowl-ads.com. There are 247 different superbowl
commercials with 25 different variables, but observe only 10 different
ads Additionally, the dataset includes defining characteristics of
superbowl ads from popular brands like Toyota and Bud Light.

There are different types of variables included in `superbowl`: 7 of
them are integers, including the view\_count, like\_count,
dislike\_count. 11 variables are characters, including brand, title, and
description. 7 variables are logical, which shows either TRUE or FALSE
for some categories, including whether or not the ad is funny,
patriotic, includes animals, and includes sex. These logical variables
were determined by the FiveThirtyEight team as they watched and
categorized all of the advertisements.

Using the dataset `superbowl`, we answer the following questions:

## 1\. What factors contribute to the most viewed ads and has the relationship between those factors and the views changed over time?

### Introduction

We investigated how including different factors such as animals,
celebrity, and sexuality affect the number of views on an ad. Even
though the dataset provides a lot more variables related to ad
characteristics, we chose to focus on these three because we think they
have the most potential to influence view counts based on our personal
experiences and observations. For the second part of the question, we
will primarily investigate how the trends and relationships we explored
in the first part have changed over the years.

We were interested in this question because we’ve observed how
influential ads can be if they include characteristics that appeal to
our emotions, such as our adoration for a cute puppy, longing to be like
a popstar, or simply our sexual appetite. However, we are unsure which
emotional appeal would foster more views and think it is important for
advertisement agencies to know. Additionally, we observed that certain
ad appeals, especially use of sexuality, have grown in popularity over
the years, and would like to observe such trends with our data analysis.

The relevant variables include:

  - `animals`: logical variable, whether or not the ad includes an
    animal

  - `celebrity`: logical variable, whether or not the ad includes a
    celebrity

  - `use_sex`: logical variable, whether or not the ad includes
    sexuality

  - `view_count`: numerical variable, number of views on the ad

  - `year`: numerical variable, year the ad was released

### Approach

(1-2 paragraphs) Describe what types of plots you are going to make to
address your question. For each plot, provide a clear explanation as to
why this plot (e.g. boxplot, barplot, histogram, etc.) is best for
providing the information you are asking about. The two plots should be
of different types, and at least one of the two plots needs to use
either color mapping or facets.

### Analysis

(2-3 code blocks, 2 figures, text/code comments as needed) In this
section, provide the code that generates your plots. Use scale functions
to provide nice axis labels and guides. You are welcome to use theme
functions to customize the appearance of your plot, but you are not
required to do so. All plots must be made with ggplot2. Do not use base
R or lattice plotting functions.

### Discussion

(1-3 paragraphs) In the Discussion section, interpret the results of
your analysis. Identify any trends revealed (or not revealed) by the
plots. Speculate about why the data looks the way it does.

## 2\. What is the relationship between popularity of a Superbowl Ad and how well it is interacted with?

### Introduction

We were interested in examining the relationship between different
popularity and interaction metrics of the superbowl ads because we want
to more clearly understand what a “successful ad” entails. Some agencies
could prefer more comments and likes to an ad to show that consumers are
engaging with the content instead of merely looking at view count. If
so, that agency would create a commercial that fosters more likes than
pure views.

The relevant variables are all numerical and include: `view_count`,
`like_count`, `dislike_count`, `comment_count`, and `favorite count`.
The `view_count` is the measure for popularity, while the other
variables define interaction with the ad.

### Approach

For the first plot, we examined the relationship between `view_count`
and the like percentage of the video using a density ridge plot. More
specifically, we wanted to observe whether or not the amount of views on
a Superbowl ad correlates with a higher or lower like percentage. In
order to plot the like percentage, we created a new variable `ratio`
which is the `like_count` divided by the sum of `like_count` and
`dislike_count`, and plotted the x-axis labels to show in percentage
(since the like ratio on Youtube is shown in percentage form). We
categorized `view_count` into 6 distinct categories (ranging from few to
viral). We chose a density ridge plot because it clearly shows changes
in like percentage while comparing these changes by the view count
category. The peaks of the denstiy ridges make comparisons of like
percentages among view categories simple to track. Additionally, since
we categorized view count into 6 distinct groups, a categorical y-axis
was necessary.

(1-2 paragraphs) Describe what types of plots you are going to make to
address your question. For each plot, provide a clear explanation as to
why this plot (e.g. boxplot, barplot, histogram, etc.) is best for
providing the information you are asking about. The two plots should be
of different types, and at least one of the two plots needs to use
either color mapping or facets.

### Analysis

(2-3 code blocks, 2 figures, text/code comments as needed) In this
section, provide the code that generates your plots. Use scale functions
to provide nice axis labels and guides. You are welcome to use theme
functions to customize the appearance of your plot, but you are not
required to do so. All plots must be made with ggplot2. Do not use base
R or lattice plotting functions.

### Discussion

(1-3 paragraphs) In the Discussion section, interpret the results of
your analysis. Identify any trends revealed (or not revealed) by the
plots. Speculate about why the data looks the way it does.

## Presentation

Our presentation can be found [here](presentation/presentation.html).

## Data

Include a citation for your data here. See
<http://libraryguides.vu.edu.au/c.php?g=386501&p=4347840> for guidance
on proper citation for datasets. If you got your data off the web, make
sure to note the retrieval date.

## References

List any references here. You should, at a minimum, list your data
source.
