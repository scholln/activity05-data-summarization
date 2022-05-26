Activity 5
================
Name

## Data and packages

Again, we will load all of the `{tidyverse}` for this Activity.

``` r
library(tidyverse)
```

We continue our exploration of college majors and earnings from the data
behind the FiveThirtyEight story [The Economic Guide To Picking A
College
Major](https://fivethirtyeight.com/features/the-economic-guide-to-picking-a-college-major/).
Remember that there are many considerations that go into picking a
major. Earning potential and employment prospects are two (important) of
these considerations, but they do not tell the entire story.

We read in the same data from Activity 4 below, but notice that this
code is now surrounded in parentheses.

``` r
(college_recent_grads <- read_csv("data/recent-grads.csv"))
```

    ## # A tibble: 173 x 21
    ##     rank major_code major           major_category total sample_size   men women
    ##    <dbl>      <dbl> <chr>           <chr>          <dbl>       <dbl> <dbl> <dbl>
    ##  1     1       2419 Petroleum Engi… Engineering     2339          36  2057   282
    ##  2     2       2416 Mining And Min… Engineering      756           7   679    77
    ##  3     3       2415 Metallurgical … Engineering      856           3   725   131
    ##  4     4       2417 Naval Architec… Engineering     1258          16  1123   135
    ##  5     5       2405 Chemical Engin… Engineering    32260         289 21239 11021
    ##  6     6       2418 Nuclear Engine… Engineering     2573          17  2200   373
    ##  7     7       6202 Actuarial Scie… Business        3777          51  2110  1667
    ##  8     8       5001 Astronomy And … Physical Scie…  1792          10   832   960
    ##  9     9       2414 Mechanical Eng… Engineering    91227        1029 80320 10907
    ## 10    10       2408 Electrical Eng… Engineering    81527         631 65511 16016
    ## # … with 163 more rows, and 13 more variables: sharewomen <dbl>,
    ## #   employed <dbl>, employed_fulltime <dbl>, employed_parttime <dbl>,
    ## #   employed_fulltime_yearround <dbl>, unemployed <dbl>,
    ## #   unemployment_rate <dbl>, p25th <dbl>, median <dbl>, p75th <dbl>,
    ## #   college_jobs <dbl>, non_college_jobs <dbl>, low_wage_jobs <dbl>

Compare this code output to the `load_data` chunk in your knitted
Activity 4 `.md` report. What does enclosing an assignment code (i.e.,
`object_name <- r_code`) in parentheses do?

**It shows a couple more rows than not doing that**:

### Data Codebook

Descriptions of the variables are again provided below. Again note that
the ACS only asks [one
question](https://www.census.gov/acs/www/about/why-we-ask-each-question/sex/)
about a person’s sexual identity.

| Header                         | Description                                                                 |
|:-------------------------------|:----------------------------------------------------------------------------|
| `rank`                         | Rank by median earnings                                                     |
| `major_code`                   | Major code, FO1DP in ACS PUMS                                               |
| `major`                        | Major description                                                           |
| `major_category`               | Category of major from Carnevale et al                                      |
| `total`                        | Total number of people with major                                           |
| `sample_size`                  | Sample size (unweighted) of full-time, year-round ONLY (used for earnings)  |
| `men`                          | Male graduates                                                              |
| `women`                        | Female graduates                                                            |
| `sharewomen`                   | Women as share of total                                                     |
| `employed`                     | Number employed (ESR == 1 or 2)                                             |
| `employed_full_time`           | Employed 35 hours or more                                                   |
| `employed_part_time`           | Employed less than 35 hours                                                 |
| `employed_full_time_yearround` | Employed at least 50 weeks (WKW == 1) and at least 35 hours (WKHP &gt;= 35) |
| `unemployed`                   | Number unemployed (ESR == 3)                                                |
| `unemployment_rate`            | Unemployed / (Unemployed + Employed)                                        |
| `median`                       | Median earnings of full-time, year-round workers                            |
| `p25th`                        | 25th percentile of earnings                                                 |
| `p75th`                        | 75th percentile of earnings                                                 |
| `college_jobs`                 | Number with job requiring a college degree                                  |
| `non_college_jobs`             | Number with job not requiring a college degree                              |
| `low_wage_jobs`                | Number in low-wage service jobs                                             |

The questions we will answer in this activity are:

-   How do the distributions of median income compare across major
    categories?
-   Do women tend to choose majors with lower or higher earnings?

## Analysis

### Median Earnings Description

### Median … Median Earnings

For the rest of this semester, I will no longer provide you with R code
chunks. Have no fear! There are a number of ways to create a code chunk:

-   Tired: Copy-and-paste a previous code chunk, delete the code, then
    add your new code. **This is likely to create more work for you and
    I strongly encourage you not to use this method.**
-   Wired: Click on the ![new chunk icon](README-img/new-chunk-icon.png)
    and select ![r chunk icon](README-img/r-chunk-icon.png) (notice all
    the different types of code chunks that you can use within an
    RMarkdown file!)
-   Inspired: Ctrl/Command + Alt/Option + I. *This is my preferred
    method.*

1.  Below, create a code chunk and name it `median_earnings`. Make sure
    there is an empty line above and below the code chunk.
2.  In your newly created R code chunk, verify that the median income
    for all majors was $36,000.

-   Specify that you want to use the `college_recent_grads` dataset,
    *then*, using the appropriate functions from `{dplyr}`, obtain the
    *median* summary statistic for the variable “median earnings of
    full-time, year-round workers” (this very wordy variable is simply
    `median` in the dataset).
-   In the appropriate *summarizing* step, refer to your numerical
    summary `median_earnings`.

``` r
college_recent_grads %>%
  summarise(median) %>%
  mutate(the_median = median(median))
```

    ## # A tibble: 173 x 2
    ##    median the_median
    ##     <dbl>      <dbl>
    ##  1 110000      36000
    ##  2  75000      36000
    ##  3  73000      36000
    ##  4  70000      36000
    ##  5  65000      36000
    ##  6  65000      36000
    ##  7  62000      36000
    ##  8  62000      36000
    ##  9  60000      36000
    ## 10  60000      36000
    ## # … with 163 more rows

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Additional Summaries of Median Earnings

Often we would like more information than the median to help us to
better understand the distribution of a variable. Note that wWhen I
obtain numerical summaries for variables, I like to include the variable
name in my summary name (e.g., `median_earnings = median(median)`).

1.  Below, create a code chunk and name it `summary_earnings`. Make sure
    there is an empty line above and below the code chunk.
2.  Specify that you want to use the `college_recent_grads` dataset,
    *then*, using the appropriate functions from `{dplyr}`, obtain the
    sample size (i.e., *n*umber of observations), *mean*, *s*tandard
    *d*eviation, *min*imum, *median*, and *max*imum summaries for the
    variable “median earnings of full-time, year-round workers”.

-   Be careful when you name your output summaries as we are dealing
    with things that could use the same name as functions that you will
    be using (i.e., “median”).

``` r
college_recent_grads %>%
group_by(major) %>%
  summarise(n = sum(sample_size), 
            median_earnings = median(median),
            the_mean = mean(median_earnings),
            sd(median_earnings),
            min(median_earnings),
            max(median_earnings))
```

    ## # A tibble: 173 x 7
    ##    major           n median_earnings the_mean `sd(median_earni… `min(median_ear…
    ##    <chr>       <dbl>           <dbl>    <dbl>             <dbl>            <dbl>
    ##  1 Accounting   2042           45000    45000                NA            45000
    ##  2 Actuarial …    51           62000    62000                NA            62000
    ##  3 Advertisin…   681           35000    35000                NA            35000
    ##  4 Aerospace …   147           60000    60000                NA            60000
    ##  5 Agricultur…    44           40000    40000                NA            40000
    ##  6 Agricultur…   273           40000    40000                NA            40000
    ##  7 Animal Sci…   255           30000    30000                NA            30000
    ##  8 Anthropolo…   247           28000    28000                NA            28000
    ##  9 Applied Ma…    45           45000    45000                NA            45000
    ## 10 Architectu…    26           54000    54000                NA            54000
    ## # … with 163 more rows, and 1 more variable: max(median_earnings) <dbl>

Provide a discussion on what you believe the distribution of median
earnings will look like. You should discuss the measures of center,
spread, and potential shape only using these values. DO NOT create any
data visualizations here. Use your understanding of how these numerical
summaries are related to the measures of center, spread, and potential
shape.

**I believe it will look skewed to the STEM majors as those jobs have a
tendency to be higher paying**:

### Median Earnings by Major Category

Now we will see how the different major categories compare to the
overall distribution of median earnings.

*Arrange* this summary table by the median earning. 1. Below, create a
code chunk and name it `major_earnings`. Make sure there is an empty
line above and below the code chunk. 2. Specify that you want to use the
`college_recent_grads` dataset, *then*, using the appropriate functions
from `{dplyr}`, obtain the similar summaries of the variable “median
earnings of full-time, year-round workers” as your `summary_earnings`
code chunk, except now *by* each `major_category`.

``` r
major_earnings <- college_recent_grads %>%
group_by(major) %>%
  summarise(n = sum(sample_size), 
            median_earnings = median(median),
            the_mean = mean(median_earnings),
            sd(median_earnings),
            min(median_earnings),
            max(median_earnings))
  

arrange(major_earnings, desc(median_earnings))
```

    ## # A tibble: 173 x 7
    ##    major           n median_earnings the_mean `sd(median_earni… `min(median_ear…
    ##    <chr>       <dbl>           <dbl>    <dbl>             <dbl>            <dbl>
    ##  1 Petroleum …    36          110000   110000                NA           110000
    ##  2 Mining And…     7           75000    75000                NA            75000
    ##  3 Metallurgi…     3           73000    73000                NA            73000
    ##  4 Naval Arch…    16           70000    70000                NA            70000
    ##  5 Chemical E…   289           65000    65000                NA            65000
    ##  6 Nuclear En…    17           65000    65000                NA            65000
    ##  7 Actuarial …    51           62000    62000                NA            62000
    ##  8 Astronomy …    10           62000    62000                NA            62000
    ##  9 Aerospace …   147           60000    60000                NA            60000
    ## 10 Biomedical…    79           60000    60000                NA            60000
    ## # … with 163 more rows, and 1 more variable: max(median_earnings) <dbl>

Is anything noteworthy when comparing your output from
`summary_earnings` to `major_earnings`? Also, compare your code in
`summary_earnings` to `major_earnings`. What is the same? Different?

**The higher sample size had the lower because there were more
participants while this was showing it by major and showing the sample
size**:

Before we continue, knit your document and take note of what the output
for your `major_earnings` code chunk looks like.

Now, add the following to the end of your pipeline (you will need to
pipe first) in your `major_earnings` code chunk:

    knitr::kable()

Knit your document and look at the output for your `major_earnings` code
chunk. What changed about the output? When would this `knitr::kable`
code be useful?

**Response**:

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Visualize Median Earnings by Major Category

Let us see how well your descriptions in the [Median Earnings by Major
Category](#median-earnings-by-major-category) section compare to the
actual distributions.

1.  Below, create a code chunk and name it `major_boxplot`.
2.  Plot the distribution of the variable `median` earnings of
    full-time, year-round workers for each `major_category` using the
    *boxplot* and *jitter* geometries.

Provide a discussion on how your descriptions in the Median Earnings by
Major Category section compares.

**Response**:

<img src="README-img/noun_pause.png" alt="pause" width = "20"/>
<b>Planned Pause Point</b>: If you feel that you have a good
understanding of these commands, feel free to start working on your
project. The remainder of this activity will help to expand these
commands.

### Multiple Rankings

#### Ranking by `major_category`

The current rankings provided in the data are by `major`. Here we will
develop a series of rankings to see how the `major_category` levels
perform. 1. Below, create a code chunk and name it `category_rankings`.
2. In this code chunk, - Group `college_recent_grads` by
`major_category`. - Summarize the variable `total` as the *sum* across
all majors (to get the total number of majors within a `major_category`)
and the following variables by their *median* value: `sharewomen`,
`unemployment_rate`, and `median` earnings. Provide a meaningful name to
each summarized value. - Assign/create a *rank* for each summarized
value (rank for `total`, rank for `sharewomen`, etc.) and provide a
meaningful name to each ranked column value. This ranking function might
be confusing. Read the help documentation and ask for clarification. -
Arrange the results so that `major_category` appear in alphabetical
order (“A” at the top).

Provide a discussion on how the `major_category` rankings compare.

**Response**:

![](README-img/noun_pause.png) **(Final) Planned Pause Point**: If you
have any questions, contact your instructor. Otherwise feel free to
continue on.

Knit, then stage everything listed in your **Git** pane, commit (with a
meaningful commit message), and push to your GitHub repo. Go to GitHub
and verify that your `activity04-data-pieplines.Rmd` file appears as you
intended it to.

You can now go back to the `README` file.

## Attribution

This activity is inspired by a lab from [Dr. Mine
Çetinkaya-Rundel](http://www2.stat.duke.edu/~mc301/)’s STA 199 course.
