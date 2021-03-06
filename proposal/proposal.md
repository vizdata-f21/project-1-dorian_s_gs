Project proposal
================
dorian’s gs

``` r
library(tidyverse)
library("tidytuesdayR")
```

## Dataset

``` r
superbowl <- readr::read_csv(
  'https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-03-02/youtube.csv')
```

## Description

This dataset is created by FiveThirtyEight, originally collected through
superbowl-ads.com. There are 247 different commercials with 25 different
variables, including the year, brand, and view count. However, they only
take from 10 different brands for their dataset. Their purpose for
creating this dataset is to identify defining characteristics of
superbowl ads from popular brands like Toyota and Bud Light. Some
questions they asked included; Was it funny? Was it patriotic? Did it
include animals? Did it use sex to sell this product? Afterwards, they
explored how these categories cluster with each other, and found some
unique combinations such as ads that included both sex appeal and
animals.

There are different types of variables included in this dataset. 7 of
them are integers, including the `view_count`, `like_count`,
`dislike_count`. 10 variables are characters, including `brand`,
`title`, and `description`. 7 variables are logical, which shows either
`TRUE` or `FALSE` for some categories, including whether or not the ad
is funny, patriotic, includes animals, and includes sex. These logical
variables were determined by the FiveThirtyEight team as they watched
all of the advertisements.

## Why this dataset?

We chose this dataset due to a number of factors. First, all the members
of our group have an interest in the Super Bowl. All of us watch the
game every year. In our personal experiences, there have been certain
memorable ads, and we are aware of the popularity surrounding ads during
the Super Bowl. As a group, we thought it would be interesting to look
at what makes ads popular and if there is statistical reasoning behind
why we remember specific ads and not others.

The dataset itself also seems as if it fits the parameters discussed in
the project description; there are a wide array of numerical and
categorical variables. The dataset also allows for us to ask two
distinct questions, one related to the popularity of ads depending on
what categories they include (or do not include) in the ads, and the
other looking at analysis of the variables over time. There are many
different angles that we can analyze the dataset from, and potentially
interesting visualizations we can create.

## Questions

### What factors contribute to the most viewed ads and has the relationship between those factors and the views changed over time?

For our first question, we want to get a general sense of which of the
characteristics of the ad contributes to relatively high view counts.
Specifically, we want to investigate how the variables `animals`,
`celebrity`, `use_sex` affect `view_count`. Even though the dataset
provides a lot more variables related to ad characteristics, we chose to
focus on these three because we think they have the most potential to
influence view counts based on our personal experiences and
observations. For the second part of the question, we will primarily
investigate how the trends and relationship we explored in the first
question has changed over time.

### What is the relationship between popularity of a Superbowl Ad and how well it is interacted with?

This question deals with whether popularity (number of views) is
connected with rating, as well as overall interaction with a video. In
this question we will look at how the variables `view_count`,
`like_count`, `dislike_count`, `comment_count`, and `favorite_count` are
related. For the first part of this question we can look at the number
of views and proportion of likes to dislikes. In the latter part of
question two, we can use the number of comments and favorites to show
the degree of interaction with videos.

## Analysis plan

### Question 1 Plan

For the first plot, we will create 3 different bar graphs for each of
the different logical variables: `animals`, `celebrity`, and `use_sex`.
Each graph will have 2 bars (one for true, one for false) and we will
plot average view count on the y-axis. We then will plot the like to
dislike ratio as a green:red fill on the bars. This means that we will
have to caculate average view count for each specific category and
boolean value (3x2 = 6 different bars/calculations). We will then
identify trends based off the newly-created plots.

For the second plot, we would like to create a line plot with `year` on
the x-axis and `view_count` on the y-axis. We may need to mutate year in
some way so it fits cleanly on the x-axis. We then want to have 6
different lines with points on plotted as well (we can layer geom\_point
and geom\_line). These 6 lines will have 3 different colors for
`animals`, `celebrity`, and `use_sex`. We then can fill these lines for
when these conditions are true and dash them when they are false (3X2 =
6 total, unique lines). If the plot seems messy with all 6 lines, we
will facet the plot by the logical variable, potentially having three
different graphs with two lines each. We will then identify trends based
off the newly-created plots.

### Question 2 Plan

For the first part of this question, we first will need to a create a
variable for the like:dislike ratio. We will do this by dividing the
number of likes by the number of dislikes. Then, we will create another
new variable that divides the number of views into specific ranges
(similar to bins in a histogram). These ranges can be decided by viewing
the distribution of view count in a histogram to see if there are
already breaks in view count. If not, we will set each range. We will
then use a boxplot with each range of view count on the x-axis and
like:dislike ratio on the y-axis to see what the overall spread looks
like for each range of view count.

For the second part of our question, our biggest challenge will be
figuring out how to graph comments and favorites. Combining the
variables by adding them might not be the best approach as we want to
see if there is a relationship among the trend of comments and
favorites. One idea for this is to have a bar chart with the same ranges
for view count (as in the previous visualization) on the x-axis and
having two side-by-side bars (`geom_col(position = "dodge")`) for each
range, one being total number of comments and the other being total
number of favorites. Another way to do this would be using a
scatterplot. View count would be on the x-axis (continuous, not in
ranges) and then comment count and favorite count would be mapped
together on the y-axis. Color and/or shape could be used to distinguish
between whether an observation was related to comment count or favorite
count. If applicable, `geom_smooth()` could be used to fit a smoothed
line of best fit for both `comment_count` and `favorite_count`.
