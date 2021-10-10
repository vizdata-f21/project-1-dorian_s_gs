Superbowl Ads
================
by Dorian’s Gs

## Introduction

The dataset `superbowl` is created by FiveThirtyEight, originally
collected through superbowl-ads.com. There are 247 different superbowl
commercials with 25 different variables, but the team observed only 10
different ads Additionally, the dataset includes defining
characteristics of superbowl ads from popular brands like Toyota and Bud
Light.

There are different types of variables included in `superbowl`: 7 of
them are integers, including the views, number of likes, and number of
dislikes. 11 variables are characters, including the brand, title, and
description. 7 variables are logical, which shows either `TRUE` or
`FALSE` for some categories, including whether or not the ad is funny,
patriotic, includes animals, and includes sex. These logical variables
were determined by the FiveThirtyEight team as they watched and
categorized all of the advertisements. The list of relevant variables
and definitions (codebook) are in the introduction of each question.

Using the dataset `superbowl`, we answer the following questions:

## 1. What factors contribute to the most viewed ads and has the relationship between those factors and the views changed over time?

### Introduction

The relevant variables include:

-   `animals`: logical variable, whether or not the ad contains animals

-   `show_product_quickly`: logical variable, whether or not the ad show
    a product quickly

-   `funny`: logical variable, whether or not the ad contains humor

-   `year`: numerical variable, year the ad was released

-   `brand`: the brand that ran the ad

More specifically, we investigate how companies choose to include
animals, humor, and show their product quickly. Even though the dataset
provides a lot more variables related to ad characteristics, we chose to
focus on these three because they occurred more than any of the other ad
types within the dataset, leading us to believe that companies cared the
most about these three characteristics. For the second part of the
question, we primarily investigate how the trends and relationships we
explored in the first part have changed over the years.

We were interested in this question because we have observed how
influential ads can be if they include characteristics that appeal to
our emotions, such as our adoration for a cute puppy, longing to be like
a popstar, or simply our sexual appetite. However, we were curious to
see if companies, the ones who provided the ads, thought the same as us.
Emotional appeal would foster more views and think it is important for
advertisement agencies to know. Additionally, we observed that certain
ad appeals, especially use of sexuality, have grown in popularity over
the years, and would like to observe such trends with our data analysis.

### Approach

Our initial plan was to create visualizations centered around the
relationship between the ad type (i.e., what characteristics an ad
possesses) and view count. After transforming and cleaning the data, we
ran into the issue of visualizations being overplotted, no distinct
relationships appearing, and outliers significantly skewing the data.
Since the data does not contain a ton of observations (247), we found
that the outliers (very low or very high view count) had a large
influence when visualizing the data. To address these issues, we shifted
our focus to looking at commercials from a brand standpoint, as well as
decided to quantify ad usage by frequency (number of ads) and not view
count. To accomplish the latter, we added a new variable `ads` to the
data frame that was the number of occurrences of each ad type in a year.
To show the relationship between brand and ad type, we displayed brand
and ad type in a segmented bar chart. The segments showed how each
category was used by the ten brands in the dataset. To answer the part
of the question related to how the ad types have fared from 2000 to
2020, we looked at the frequency of the ad types over time, showing this
relationship in a line graph with the year on the x-axis and ad
occurrence on the y-axis.

We chose a bar chart as they are effective at displaying two variables,
one continuous and the other discrete. It is easy to compare between
bars, based on relative height of the bars and/or relative bar segments.
For the other visualization for this question, we opted for a line chart
as we were mapping a discrete variable over time. We eventually settled
on using estimates of the conditional mean functions for each ad type as
there was overplotting and using a regular line chart led to messy
visualizations that were difficult to interpret.

### Analysis

``` r
# Message related to grouping by 'brand' and 'year' in summarise, 
# Which is what we want
superbowl_viz <- superbowl %>%
  select(year, brand, animals, celebrity, use_sex, 
         funny, show_product_quickly, patriotic, danger) %>%
  pivot_longer(cols = c(animals, celebrity, use_sex, funny, 
                        show_product_quickly, patriotic, danger), 
               names_to = 'ad_type', 
               values_to = 'have') %>%
  filter(have == TRUE) %>%
  group_by(brand, year, ad_type) %>%
  summarise(ads = n()) %>%
  mutate(brand = case_when(brand == 'Hynudai' ~ 'Hyundai',
                           TRUE ~ as.character(brand))) %>%
  mutate(category = case_when(brand == 'Bud Light' ~ 'Alcohol',
                          brand == 'Budweiser' ~ 'Alcohol',
                          brand == 'Doritos' ~ 'Food & Beverage',
                          brand == 'Pepsi' ~ 'Food & Beverage',
                          brand == 'Coca-Cola' ~ 'Food & Beverage',
                          brand == 'Hyundai' ~ 'Automobile',
                          brand == 'Toyota' ~ 'Automobile',
                          brand == 'Kia' ~ 'Automobile',
                          brand == 'E-Trade' ~ 'Financial Services',
                          brand == 'NFL' ~ 'Sports')) %>%
  relocate(year, .before = brand) %>%
  relocate(category, .before = brand)

#Brand Total Labels
brand_totals <- superbowl_viz %>%
  filter(ad_type != 'celebrity',
         ad_type != 'use_sex',
         ad_type != 'danger',
         ad_type != 'patriotic') %>%
  group_by(brand)  %>%
  summarise(brand_totals = n())

# Message related to grouping in summarise(), which is how we want it
brand_viz <- superbowl_viz %>%
  filter(ad_type != 'celebrity',
         ad_type != 'use_sex',
         ad_type != 'danger',
         ad_type != 'patriotic') %>%
  group_by(brand, ad_type)  %>%
  summarise (ads = n()) %>% 
  ggplot(aes(factor(brand, 
                    levels = c("Bud Light", "Budweiser", "Doritos", "Pepsi", 
                               "Coca-Cola", "Hyundai", "E-Trade", 
                               "Toyota", "Kia", "NFL")), 
                    ads, 
                    fill = ad_type)) +
  geom_bar(stat = "identity", 
           position = position_stack(0.1)) +
  geom_text(data = brand_totals, 
            aes(x = brand, y = brand_totals, 
               label = brand_totals, 
               fontface = 3,
               fill = NULL), 
            size = 8,
            nudge_y = 2) +
  scale_y_continuous(breaks = seq(0, 60, 10)) + 
  scale_fill_manual(values = c("#56B4E9", "#E69F00", "#009E73"), 
                    labels = c("Shows Product Quickly", "Funny", "Animals")) +
  labs(title = "Ad Occurrences by Brand",
       y = "Ad Occurrences", 
       x = "Brand",
       fill = "Ad Type",
       caption = "Source: FiveThirtyEight") +
  guides(fill = guide_legend(reverse = TRUE)) +
  theme_minimal() +
  theme(plot.title = element_text(face = "bold", 
                                  size = 14, 
                                  hjust = 0.5, vjust = 1),
        plot.caption = element_text(size = 10, hjust = 1.55),
        legend.position = c(0.85, 0.7),
        axis.text.x = element_blank(),
        strip.text = element_blank(),
        axis.title.y = element_text(vjust = 1, 
                                    size = 12),
        axis.text.y = element_text(hjust = 1.75),
        axis.title.x = element_text(vjust = -2,
                                    size = 12),
        axis.text = element_text(size = 10, 
                                 lineheight = 1),
        legend.title = element_text(size = 12),
        legend.text = element_text(size = 10))

images <- axis_canvas(brand_viz, axis = 'x') + 
  draw_image("https://1000logos.net/wp-content/uploads/2021/04/Bud-Light-logo-768x432.png", 
             x = 0.5, scale = 0.75) +
  draw_image("https://logos-world.net/wp-content/uploads/2020/12/Budweiser-Logo-2016-present.jpg", 
             x = 1.5, scale = 1.1) +
  draw_image("https://1000logos.net/wp-content/uploads/2020/07/Doritos-Logo-768x480.png",
             x = 2.5, scale = 1.1) +
  draw_image("https://1000logos.net/wp-content/uploads/2017/05/Pepsi-Logo-768x631.png", 
             x = 3.5, scale = 0.9) +
  draw_image("https://1000logos.net/wp-content/uploads/2021/05/Coca-Cola-logo-768x432.png",
             x = 4.5, scale = 1) +
  draw_image("https://1000logos.net/wp-content/uploads/2018/04/Hyundai-logo-768x432.png",
             x = 5.5, scale = 1.1) +
  draw_image("https://res.cloudinary.com/crunchbase-production/image/upload/c_lpad,h_256,w_256,f_auto,q_auto:eco,dpr_1/v1476500866/ukg833gylj7adlryeuig.png", 
             x = 6.5, scale = 1) +
  draw_image("https://1000logos.net/wp-content/uploads/2021/04/Toyota-logo-768x432.png", 
             x = 7.5, scale = 0.8) +
  draw_image("https://1000logos.net/wp-content/uploads/2020/02/Kia-Logo-2012-1024x635.jpg", 
             x = 8.5, scale = 0.9) +
  draw_image("https://1000logos.net/wp-content/uploads/2017/05/NFL-logo-768x518.png", 
             x = 9.5, scale = 1.1) 
ggdraw(insert_xaxis_grob(brand_viz, images, position = "bottom"))
```

<img src="README_files/figure-gfm/q1p1-1.png" width="70%" style="display: block; margin: auto;" />

``` r
# Message related to grouping in summarise(), which is how we want it
ad_labels <- superbowl %>%
  pivot_longer(cols = c(animals, celebrity, use_sex, funny, 
                        show_product_quickly, patriotic, danger), 
               names_to = 'ad_type', 
               values_to = 'have') %>%
  filter(have == TRUE,
         ad_type != 'celebrity',
         ad_type != 'use_sex',
         ad_type != 'danger',
         ad_type != 'patriotic') %>%
  group_by(ad_type, year) %>%
  summarise(ads = n()) %>%
  filter(year == 2005) %>%
  mutate(ads = case_when(ads == 6 ~ 4.1,
                         ads == 11 ~ 9.9,
                         ads == 8 ~ 8.5))
```

``` r
# message related to geom_smooth() method and formula, 
# which are both what we want
superbowl %>%
  pivot_longer(cols = c(animals, celebrity, use_sex, funny, 
                        show_product_quickly, patriotic, danger), 
               names_to = 'ad_type', 
               values_to = 'have') %>%
  filter(have == TRUE,
         ad_type != 'celebrity',
         ad_type != 'use_sex',
         ad_type != 'danger',
         ad_type != 'patriotic') %>%
  group_by(year, ad_type) %>%
  summarise(ads = n()) %>%
  ggplot(aes(year, ads, color = ad_type)) + 
  geom_line(aes(linetype = ad_type), 
            stat = "smooth",
            size = 3, 
            se = F,
            show.legend = FALSE) +
  geom_text_repel(data = ad_labels, 
                  aes(label = c("Shows Product Quickly", "Funny", "Animals")),
                  nudge_x = 0.25,
                  nudge_y = -0.8,
                  size = 4,
                  show.legend = FALSE) +
  scale_color_manual(values = c("#56B4E9", "#E69F00", "#009E73"), 
                     labels = c("Shows Product Quickly", "Funny", "Animals"),
                     guide = guide_legend(reverse = TRUE)) +
  scale_y_continuous(breaks = seq(0, 13, 2)) +
  scale_x_continuous(breaks = seq(2000, 2020, 4)) +
  theme_minimal() + 
  theme(plot.title = element_text(face = "bold", 
                                  size = 14, 
                                  hjust = 0.5, vjust = 1),
        plot.subtitle = element_text(size = 12, 
                                  hjust = 0.5, vjust = 1),
        plot.caption = element_text(size = 10, hjust = 1.55),
        axis.title.x = element_text(vjust = -2, 
                                    size = 12),
        axis.title.y = element_text(vjust = 1, 
                                    size = 12),
        axis.text = element_text(size = 10, 
                                 lineheight = 1)) +
  labs(y = "Ad Occurrences", 
       x = "Year",
       color = "Ad Type",
       caption = "Source: FiveThirtyEight",
       title = "Ad Occurrences by Type",
       subtitle = "From 2000 - 2020") 
```

<img src="README_files/figure-gfm/q1p2-1.png" width="70%" style="display: block; margin: auto;" />

### Discussion

In the bar plot, it is clear that the most common ad types are ”Animals”
and “Funny.” All of these companies aired ads in these categories
(funny, animals, and shows product quickly), which is indicative of
their effectiveness in the advertisement industry. It is clear that beer
companies, such as Bud Light or Budwiser, have more advertisements in
all three categories when compared to car companies such as Kia or
Toyota. One interesting observation is that the only company with no ads
in the “Shows Product Quickly” category is the NFL. This makes sense
because the NFL hosts the Super Bowl and does not have to worry about
paying for extra runtime. One limitation with our plot is that the
categories “double count” ads. If an ad is both funny and contains
animals, then it will look as if there were two different ads, one funny
and one with animals. Fortunately, the main goal of this plot was to
focus on which ad types were popular and the colored stacked bar plots
accomplish this goal.

In the line plot, there are three smooth curves that graph each ad type
and show the trend of their yearly occurrences over time. Lines are
defined by different colors, labels, and type so it is easier to
distinguish between them. The smooth curves allow the viewer to see
overall trends more easily. Funny ads peak in occurrence in the
mid-2000s, but then drop off all the way into the present. Both ads that
show the product quickly and have animals were on an uptrend, but
dropped off in the late 2000s/early 2010s. Just like the bar plot, it is
clear that animal ads and funny ads were the most common types of ads.
One limitation of this plot is that we plotted an estimate of the
conditional mean function for each of the three ad types. This means
that we are not able to look directly at a year and see the exact number
occurrences of a specific ad-type. However, the original line plot was
very difficult to spot overall trends due to overplotting, and the
smooth lines allow us to accomplish our goal of seeing overall trends
over time.

## 2. What is the relationship between popularity of a Superbowl Ad and how much interaction it got?

### Introduction

The relevant variables are all numerical and include:

-   `view_count`: number of views

-   `like_count`: number of likes

-   `dislike_count`: number of dislikes

-   `comment_count`: number of comments

The `view_count` is the measure for popularity, while the other
variables define interaction with the ad. We were interested in
examining the relationship between different popularity and interaction
metrics of the advertisements because we want to more clearly understand
what a “successful ad” entails. Some agencies could prefer more comments
to an ad to show that consumers are engaging with the content, but that
might be more heavily associated with the like to dislike ratio instead
of the view count. If so, that agency would create a commercial that
fosters more likes than pure views.

### Approach

For the following two plots, the range of `view count` was so large
(from 10 views to more than 10 million views) that it was nearly
impossible to visualize any interaction with just one plot. We decided
to facet our plot by 6 categories according to the quantiles of the
standard distribution for view count: few, some, moderate, many, high,
and viral views. In the first plot, we examined the relationship between
the view count and the ratio of likes. In order to plot the like
percentage, we created a new variable `ratio` which is the `like_count`
divided by the sum of `like_count and dislike_count`, and plotted the
x-axis labels to show percentage (since the like ratio on Youtube is
shown in percentage form). We chose a density ridge plot because it
clearly shows trends in like percentage while comparing these changes by
the view count category, and the peaks of the density ridges stacked
vertically make comparisons between view categories simple to track.

For the second plot, we wanted to look more specifically at the
relationship between view count and the number of interactions a video
gets. We defined interactions as the number of likes, dislikes and
comments a video got. We wanted to plot both `view_count` and
`interactions` as continuous variables. In order to find the best plot,
we experimented with scatter plots, line plots, step plots, and area
plots to see which plot was the most informative. The view counts were
clustered in the thousands of views but contained very few videos with
millions of views, so the scatter plot was not helpful. We found that a
line plot or step by itself was too fragmented and the connection
between points zig-zagged a lot and made it difficult to see the overall
trend. The combined plot shows each point but the area also makes it
much easier to trace the trend for each view category. We then added
labels, captions and made a few styling choices to get to our final
presentation.

### Analysis

``` r
superbowl <- superbowl %>%
  mutate(ratio = like_count / (like_count + dislike_count),
         interactions = like_count + dislike_count + comment_count,
         view_category = case_when(
            view_count < 4000 ~ "Few\nLess than 4K",
            view_count >= 4000 & view_count < 30000 ~ "Some\n4K to 30K",
            view_count >= 30000 & view_count < 90000 ~ "Moderate\n30K to 90K",
            view_count >= 90000 & view_count < 500000 ~ "Many\n90K to 500K",
            view_count >= 500000 & view_count < 10000000 ~ "High\n500K to 10M",
            TRUE ~ "Viral\n10M+"))
superbowl %>%
  mutate(Views = fct_relevel(
            view_category, "Viral\n10M+", "High\n500K to 10M", "Many\n90K to 500K", "Moderate\n30K to 90K","Some\n4K to 30K","Few\nLess than 4K"
         )) %>%
  ggplot(aes(x = ratio, y = Views, fill = Views)) +
    geom_density_ridges(scale = 0.9, show.legend = FALSE) +
    coord_cartesian(xlim = c(0.5, 1.0)) +
    scale_fill_discrete_sequential(palette = "Greens", order = c(6:1)) +
    scale_x_continuous(expand = c(0, 0.1), labels = scales::percent_format(accuracy = 1)) +
    scale_y_discrete(expand = expand_scale(mult = c(0, .2))) +
    labs(
      title = "Youtube Superbowl Ads and Like Percentages",
      x = "Like Percentage",
      y = "Views",
      caption = "(Percentage of likes to sum of likes and dislikes on the video)") +
    theme_ridges() +
    theme(
      plot.title.position = "plot",
      plot.title = element_text(face = "bold", size = 14, hjust = 0.36),
      plot.caption = element_text(size = 8),
      axis.title.y = element_text(face = "bold", size = 12, hjust = 0.54, vjust = 1),
      axis.text = element_text(size = 10, lineheight = 1),
      axis.text.x = element_text(vjust = -1))
```

<img src="README_files/figure-gfm/q2p1-1.png" width="70%" style="display: block; margin: auto;" />

``` r
superbowl <- superbowl %>%
  mutate(view_category = paste(view_category, "Views"))
superbowl <- superbowl %>%
  mutate(view_category = fct_relevel(
  view_category, "Few Views","Some Views","Moderate Views","Many Views", "High Views", "Viral Views"))

ggplot(data = superbowl, aes(x = view_count, y = interactions)) +
  geom_area(aes(fill = view_category), show.legend = NULL) +
  geom_line() +
  facet_wrap(vars(view_category),  
             scales ="free",
             nrow = 3,
             strip.position = "top") +
    scale_y_continuous(label = label_number_si()) +
  scale_x_continuous(label = label_number_si()) +
  scale_fill_discrete_sequential(palette = "Greens") +
  labs(x = "View Count", y ="Number of Interactions", title ="Assessing Superbowl Ad Interactions",
       caption ="Interaction is defined by the sum of \nlikes, dislikes and comments on a video")+
  theme_minimal() +
  theme(panel.grid.minor = element_blank(),
        strip.background = element_rect(colour = "black"),
        strip.placement = "inside",
        plot.title = element_text(face = "bold", size = 14, hjust = 0.5, vjust = 1),
        plot.caption = element_text(size = 10, hjust = 0),
        panel.spacing = unit(1.4, "lines"),
        axis.text.x = NULL,
        axis.title.x = element_text(size = 12, vjust = -.5),
        axis.title.y = element_text(size = 12, hjust = 0.5, vjust = 1),
        axis.text = element_text(size = 10, lineheight = 1),
        strip.text.x = element_text(size = 10))
```

<img src="README_files/figure-gfm/q2p2-1.png" width="70%" style="display: block; margin: auto;" />

### Discussion

In the density ridges plot, most videos had a like percentage of over
60%, showing that most of the viewers who interacted with the ad enjoyed
the video. An interesting finding was that there was generally a
negative relationship between views and like percentage. That is, the
category with the fewest video views peaked close to 100% likes and was
more precisely close to 100 compared to higher view categories.
Interestingly, the viral videos peaked at close to 75% likes (with a
smaller peak at 90%) and is the most spread out with like percentages.
The other categories from some to high views, peak at similar
percentages with high views having slightly lower like percentages.
Overall, this plot shows that having more views does not necessarily
mean that the video is on average more likable. Instead it shows the
opposite: higher view counts would predict a lower like percentage,
meaning that companies who value video satisfaction rates (and want to
avoid high dislikes ratios) would not want a viral ad video.

The area plot of view count by number of interactions reveals quite a
few interesting results. The first is that on average, the number of
views for ads is larger by a factor of about 100 that the number of
interactions on that ad. On average, about 100 views resulted in 1
interaction with the video. Our guess prior to seeing the results of our
visualization was that as the number of views increased, the number of
proportions would also increase on a statistically significant rate. In
terms of our data that means that we expected each higher category of
views would have a significantly high level of interaction. As an
example, we expected that proportionally, the number of view count to
interactions would be higher for ads with “Viral Views” compared to
“Mega Views” and ads with “Mega Views” would be higher that graphs with
“Many Views”. Our results turned out to be a bit more complicated than
that. We found that only ads on either extreme end followed this trend.
So ads with a “few views” had much a smaller views to interaction ratio
than videos in all other categories and ads with “viral views” had much
bigger views to interaction ratio than videos in all other “categories.”
For videos in the middle categories(“Some Views”,“Moderate Views”,“Many
Views” and “High Views”), proportion of view counts to interactions
seemed to be much more random and didn’t follow any discernible
patterns. Each panel also had no discernible trend in the way they were
plotted unlike the “few” and “viral” categories which had a discernible
linear pattern each.

## Presentation

Our presentation can be found
[here](https://vizdata-f21.github.io/project-1-dorian_s_gs/presentation/presentation.html#1).

## Data

Best R and Mehta D, 2021, *Super Bowl Ads*, Electronic Dataset, Github
Repository, Retrieved September 13, 2021
<https://github.com/fivethirtyeight/superbowl-ads>.

## References

1.  Data Source: Our data source is from [The Tidy Tuesday
    Project](https://github.com/rfordatascience/tidytuesday/blob/master/data/2021/2021-03-02/readme.md)
    by way of [FiveThirtyEight’s Github
    Repository](https://github.com/fivethirtyeight/superbowl-ads).
2.  Viral Reference:
    <https://www.forbes.com/sites/robertwynne/2018/03/09/there-are-no-guarantees-or-exact-statistics-for-going-viral/?sh=6224f76a5e8c>

Presentation Images:

3.  Title image: <https://www.pinterest.com/pin/746542075709663730/>
4.  Youtube logo: Youtube
5.  Tv image: FiveThirtyEight
6.  Logos directly from: Budweiser, Bud Light, Pepsi, Doritos,
    Coca-Cola, Hyundai, E Trade, Kia, Toyota, and NFL
7.  Youtube interaction bar:
    <https://www.tubefilter.com/2021/03/30/youtube-experiment-hiding-public-dislikes-creator-well-being/>
