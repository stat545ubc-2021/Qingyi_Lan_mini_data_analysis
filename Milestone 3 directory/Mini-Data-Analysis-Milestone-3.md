Mini Data Analysis Milestone 3
================

# Welcome to your last milestone in your mini data analysis project\!

In Milestone 1, you explored your data and came up with research
questions. In Milestone 2, you obtained some results by making summary
tables and graphs.

In this (3rd) milestone, you’ll be sharpening some of the results you
obtained from your previous milestone by:

  - Manipulating special data types in R: factors and/or dates and
    times.
  - Fitting a model object to your data, and extract a result.
  - Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

## Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-3.Rmd)
directly. Fill in the sections that are tagged with `<!--- start your
work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from `output:
html_document` to `output: github_document`. Commit and push all of your
work to your mini-analysis GitHub repository, and tag a release on
GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 40 points (compared to the usual 30
points): 30 for your analysis, and 10 for your entire mini-analysis
GitHub repository. Details follow.

**Research Questions**: In Milestone 2, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

From Milestone 2, you chose two research questions. What were they? Put
them here.

<!-------------------------- Start your work below ---------------------------->

1.  *Which parameters are highly correlated to the diagnosis types?*
2.  *Whether the relationship between cancer sample parameters and
    diagnosis type is consistent for mean, SE, and worst*
    <!----------------------------------------------------------------------------->

# Exercise 1: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

<!-------------------------- Start your work below ---------------------------->

\#Since the diagnosis only has two categories within it. I need to
create a variable with at least three groups.From the previous
milestones, we know that radius\_mean and diagnosis type seems to be
highly correlated. I would like to invest this question further through
visualization between diagnosis type and radius\_mean. However, I would
categorize radius mean into four categories: “small”, “moderate”,
“large”, “very large”

\#the four categories are based on quartile values of radius mean, shown
below

``` r
quantile(cancer_sample$radius_mean)
```

    ##     0%    25%    50%    75%   100% 
    ##  6.981 11.700 13.370 15.780 28.110

\#Group the radius mean into four groups

``` r
Cancer_Mean_Radius <- cancer_sample %>% 
   mutate(Radius_level = case_when(radius_mean < 11.700 ~ "small",
                                 radius_mean < 13.370 ~ "moderate",
                                 radius_mean < 15.780 ~ "large",
                                 TRUE ~ "very large"))
```

\#Histogram plot grouped by diagnosis type is used to visualize
relationship of two categorical variables

``` r
#Radius Mean Level in M diagnosis type
Cancer_Mean_Radius%>%filter(diagnosis == "M")%>%ggplot(aes(y=Radius_level)) + geom_bar(aes(fill=Radius_level)) +
  theme(axis.text.x = element_blank(),
        axis.ticks.x = element_blank()) +
  labs(y="Radius Mean Level", x = "", title = "Radius Mean Level in M diagnosis type")
```

![](Mini-Data-Analysis-Milestone-3_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
#Radius Mean Level in B diagnosis type
Cancer_Mean_Radius%>%filter(diagnosis == "B")%>%ggplot(aes(y=Radius_level)) + geom_bar(aes(fill=Radius_level)) +
  theme(axis.text.x = element_blank(),
        axis.ticks.x = element_blank()) +
  labs(y="Radius Mean Level", x = "", title = "Radius Mean Level in B diagnosis type")
```

![](Mini-Data-Analysis-Milestone-3_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->
<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats`
    package (3 points). Then, in a sentence or two, briefly explain why
    you chose this grouping (1 point here for demonstrating
    understanding of the grouping, and 1 point for demonstrating some
    justification for the grouping, which could be subtle or
    speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year):
    
    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)
          - Note that you might first have to *make* a time-based column
            using a function like `ymd()`, but this doesn’t count.
          - Examples of something you might do here: extract the day of
            the year from a date, or extract the weekday, or let 24
            hours elapse on your dates.
    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).
          - For example, you could say something like “Investigating the
            day of the week might be insightful because penguins don’t
            work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

\#citation:
\#<https://wilkelab.org/SDS375/slides/getting-things-in-order.html#56>
\#<https://cran.r-project.org/web/packages/ggmosaic/vignettes/ggmosaic.html>

\#With fct\_infreq function, the factor within the dataframe will be
reodered by the count of the factor even if the dataframe doesn’t seem
to change \#The reason for me to choose grouping based on the frequency
of each radius mean level is that we can know in each diagnosis type,
whether “large”, very large" , “moderate”, or “small” take the
dominiation, and by that we can have an idea of the mean\_radius size in
each category.

**Task Number**: \#1

``` r
#reorder the Cancer_mean_radius level by their counts in M type
Reorder_M<-Cancer_Mean_Radius%>%filter(diagnosis == "M")%>%mutate(Radius_level=fct_infreq(Radius_level))%>%ggplot(aes(y=Radius_level)) + geom_bar(aes(fill=Radius_level)) +
  theme(axis.text.x = element_blank(),
        axis.ticks.x = element_blank()) +
  labs(y="Radius Mean Level", x = "", title = "Radius Mean Level in M diagnosis type")
Reorder_M
```

![](Mini-Data-Analysis-Milestone-3_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
#reorder the Cancer_mean_radius level by their counts in B type
Reorder_B<-Cancer_Mean_Radius%>%filter(diagnosis == "B")%>%mutate(Radius_level=fct_infreq(Radius_level))%>%ggplot(aes(y=Radius_level)) + geom_bar(aes(fill=Radius_level)) +
  theme(axis.text.x = element_blank(),
        axis.ticks.x = element_blank()) +
  labs(y="Radius Mean Level", x = "", title = "Radius Mean Level in B diagnosis type")
Reorder_B
```

![](Mini-Data-Analysis-Milestone-3_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->
\#According to the result, “very large” is the dominate group in M
diagnosis type while \#“small” is the dominate group in B diagnosis
group. Radius\_mean is really correlated with the diagnosis type.
<!----------------------------------------------------------------------------->

<!-------------------------- Start your work below ---------------------------->

**Task Number**:\#2 \#With the function fct\_lump\_n, we can group the
catagories that are not of our interest to the “other” category. I
choose to just keep the first two categories with the highest frequency
and group the other two categories into “others”. By doing so, we can
reduce the categories to a manageable amount (n=3), and the results
seems to be better visualized.

``` r
#Plotting the graphs with the most frequent two categories and "others" for the infrequent two categories
#For people diagnosed with M type
groupbyothers_M<-Cancer_Mean_Radius%>%filter(diagnosis == "M")%>%
  mutate(Radius_level = fct_lump_n(fct_rev(Radius_level), 2)) %>%#group the infrequent ones into "others"
  ggplot(aes(y = fct_rev(Radius_level))) + 
  geom_bar(aes(fill=Radius_level)) +
  theme(axis.text.x = element_blank(),
        axis.ticks.x = element_blank()) +
  labs(y="Radius Mean Level", x = "", title = "Radius Mean Level in M diagnosis type")

groupbyothers_M
```

![](Mini-Data-Analysis-Milestone-3_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
#Plotting the graphs with the most frequent two categories and "others" for the infrequent two categories
#For people diagnosed with B type
groupbyothers_B<-Cancer_Mean_Radius%>%filter(diagnosis == "B")%>%
  mutate(Radius_level = fct_lump_n(fct_rev(Radius_level), 2)) %>% #group the infrequent ones into "others"
  ggplot(aes(y = fct_rev(Radius_level))) + 
  geom_bar(aes(fill=Radius_level)) +
  theme(axis.text.x = element_blank(),
        axis.ticks.x = element_blank()) +
  labs(y="Radius Mean Level", x = "", title = "Radius Mean Level in B diagnosis type")

groupbyothers_B
```

![](Mini-Data-Analysis-Milestone-3_files/figure-gfm/unnamed-chunk-6-2.png)<!-- -->

<!----------------------------------------------------------------------------->

# Exercise 2: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: *Whether the relationship between cancer sample
parameters and diagnosis type is consistent for mean, SE, and worst*

\#This is one of the research question I proposed, and it seems to be
reasoable test the relastionship between radius\_mean and radius\_se, to
see how they are correlated withe ach other. If we could use the
radius\_se to predict the radius\_mean, then it possibly means
radius\_mean will change with radius\_se for each diagnosis type

**Variable of interest**: the X variable would be radius\_se, and the Y
variable would be radius\_mean

<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

  - **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.
      - You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
      - You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
      - You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

``` r
#I will fit a model called mod1, using radius_se and explanartory variable, and radius_mean as outcome variable.
mod1<-lm(radius_mean ~ radius_se, cancer_sample )
#the output of this model will be shown as below
mod1
```

    ## 
    ## Call:
    ## lm(formula = radius_mean ~ radius_se, data = cancer_sample)
    ## 
    ## Coefficients:
    ## (Intercept)    radius_se  
    ##       10.63         8.63

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

  - Be sure to indicate in writing what you chose to produce.
  - Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
  - Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

``` r
#I will fit a model called mod1, using radius_se and explainatory variable, and radius_mean as outcome variable.
#I would use broom::tidy function to pull out p values and estimate for intercept and slope
broom::tidy(mod1)
```

    ## # A tibble: 2 × 5
    ##   term        estimate std.error statistic   p.value
    ##   <chr>          <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)    10.6      0.192      55.3 1.56e-230
    ## 2 radius_se       8.63     0.392      22.0 3.63e- 78

\#p values for radius\_se is \<0.05, indicating this model has
statistical significance and we could use radius\_se to predict
radius\_mean. When radius\_se is equal to 0, the radius mean would be
10.6307, and for every 1 unit increase in radius\_se, radius mean will
increase by 8.6298.
<!----------------------------------------------------------------------------->

# Exercise 3: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 3.1 (5 points)

Take a summary table that you made from Milestone 2 (Exercise 1.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

  - **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
  - **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
#to pull out the summary table from miletone 2#

tidy_cancer_sample<-cancer_sample %>% 
  rename(concave.points_mean = concave_points_mean, concave.points_se=concave_points_se,
         concave.points_worst=concave_points_worst, fractal.dimension_se=fractal_dimension_se, fractal.dimension_mean=fractal_dimension_mean, fractal.dimension_worst=fractal_dimension_worst)%>% #rename some variables to format the column names
  pivot_longer(cols      = c(-ID, -diagnosis), 
               names_to  = c(".value", "measure"),
               names_sep = "_")

mean<-tidy_cancer_sample%>%  
  filter(measure == "mean")%>%
  pivot_longer(cols = c(-ID, -diagnosis, -measure), 
               names_to  = "parameter", 
               values_to = "mean_value")%>%
  group_by(parameter, diagnosis)%>%summarise(mean_mean = mean(mean_value, na.rm = TRUE))
```

    ## `summarise()` has grouped output by 'parameter'. You can override using the `.groups` argument.

``` r
##################################################Codes for the summary table finished

#Create a folder named "output" in the directory
dir.create(here::here("output"))
```

    ## Warning in dir.create(here::here("output")): '/Users/janette/Desktop/
    ## Qingyi_Lan_mini_data_analysis/output' already exists

``` r
#write csv file named "milestone2_exported_file.csv" within the folder
write_csv(mean, here::here("output", "milestone2_exported_file.csv"))
dir(here::here("output"))
```

    ## [1] "milestone2_exported_file.csv" "model1.rds"

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Exercise 2 to an R binary file (an RDS),
and load it again. Be sure to save the binary file in your `output`
folder. Use the functions `saveRDS()` and `readRDS()`.

  - The same robustness and reproducibility criteria as in 3.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->

``` r
#save the model to RDS file
saveRDS(mod1, here::here("output", "model1.rds"))

#readRDS and pull out the model
readRDS(here::here("output", "model1.rds"))
```

    ## 
    ## Call:
    ## lm(formula = radius_mean ~ radius_se, data = cancer_sample)
    ## 
    ## Coefficients:
    ## (Intercept)    radius_se  
    ##       10.63         8.63

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

  - In a sentence or two, explains what this repository is, so that
    future-you or someone else stumbling on your repository can be
    oriented to the repository.
  - In a sentence or two (or more??), briefly explains how to engage
    with the repository. You can assume the person reading knows the
    material from STAT 545A. Basically, if a visitor to your repository
    wants to explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without
them\! They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least four folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a
sentence or two what is in the folder, in plain language (it’s enough to
say something like “This folder contains the source for Milestone 1”).

## Output (2 points)

All output is recent and relevant:

  - All Rmd files have been `knit`ted to their output, and all data
    files saved from Exercise 3 above appear in the `output` folder.
  - All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
  - There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B\!

## Error-free code (1 point)

This Milestone 3 document knits error-free. (We’ve already graded this
aspect for Milestone 1 and 2)

## Tagged release (1 point)

You’ve tagged a release for Milestone 3. (We’ve already graded this
aspect for Milestone 1 and 2)
