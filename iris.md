Assignment 5
================

Author: Ray Wu

Bringing in Rectangular data
----------------------------

First, we load the `gapminder` and `tidyverse` packages:

``` r
library(gapminder)
library(tidyverse)
```

    ## Note: the specification for S3 class "difftime" in package 'lubridate' seems equivalent to one from package 'hms': not turning on duplicate class definitions for this class.

    ## ── Attaching packages ────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
    ## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ── Conflicts ───────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

Factor Management
-----------------

### Drop Oceania

First, let's take a look at the dataset:

``` r
gapminder
```

    ## # A tibble: 1,704 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # ... with 1,694 more rows

So now, we know that we are dropping the 'Oceania' level from the 'continent' factor

Let's take a look what would happen when we drop Oceania:

``` r
gapminder %>% 
  filter(continent == 'Oceania')
```

    ## # A tibble: 24 x 6
    ##    country   continent  year lifeExp      pop gdpPercap
    ##    <fct>     <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Australia Oceania    1952    69.1  8691212    10040.
    ##  2 Australia Oceania    1957    70.3  9712569    10950.
    ##  3 Australia Oceania    1962    70.9 10794968    12217.
    ##  4 Australia Oceania    1967    71.1 11872264    14526.
    ##  5 Australia Oceania    1972    71.9 13177000    16789.
    ##  6 Australia Oceania    1977    73.5 14074100    18334.
    ##  7 Australia Oceania    1982    74.7 15184200    19477.
    ##  8 Australia Oceania    1987    76.3 16257249    21889.
    ##  9 Australia Oceania    1992    77.6 17481977    23425.
    ## 10 Australia Oceania    1997    78.8 18565243    26998.
    ## # ... with 14 more rows

Since we have 24 rows and 12 years for each country, we should have 24 entries less or 2 countries less after the modification, whatever one would prefer.

We can also get more information about the dataset as follows:

``` r
gapminder %>% 
  str()
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    1704 obs. of  6 variables:
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...

### Actually dropping Oceania

``` r
(gapminder_no_oceania = gapminder %>% 
  filter(continent != 'Oceania'))
```

    ## # A tibble: 1,680 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # ... with 1,670 more rows

Let's check the modified factor:

``` r
gapminder_no_oceania$continent %>% 
  levels()
```

    ## [1] "Africa"   "Americas" "Asia"     "Europe"   "Oceania"

We still have Oceania! We need to call the `droplevels()` function to *actually* drop Oceania.

``` r
(gapminder_no_oceania = gapminder_no_oceania %>% 
  droplevels())
```

    ## # A tibble: 1,680 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # ... with 1,670 more rows

``` r
gapminder_no_oceania$continent %>% 
  levels()
```

    ## [1] "Africa"   "Americas" "Asia"     "Europe"

Now, we see that Oceania is actually gone for good.

Creating a smaller version of the dataset to read/write from the disk (dataset filtered down to data from 2002) and to reorder factors

``` r
gapminder_2002 = gapminder %>% 
  filter(year == 2002)
```

``` r
(gapminder_asia_2002 = gapminder_2002 %>% 
  filter(continent == 'Asia'))
```

    ## # A tibble: 33 x 6
    ##    country          continent  year lifeExp        pop gdpPercap
    ##    <fct>            <fct>     <int>   <dbl>      <int>     <dbl>
    ##  1 Afghanistan      Asia       2002    42.1   25268405      727.
    ##  2 Bahrain          Asia       2002    74.8     656397    23404.
    ##  3 Bangladesh       Asia       2002    62.0  135656790     1136.
    ##  4 Cambodia         Asia       2002    56.8   12926707      896.
    ##  5 China            Asia       2002    72.0 1280400000     3119.
    ##  6 Hong Kong, China Asia       2002    81.5    6762476    30209.
    ##  7 India            Asia       2002    62.9 1034172547     1747.
    ##  8 Indonesia        Asia       2002    68.6  211060000     2874.
    ##  9 Iran             Asia       2002    69.5   66907826     9241.
    ## 10 Iraq             Asia       2002    57.0   24001816     4391.
    ## # ... with 23 more rows

Factor reordering
-----------------

Now we will see what happens before we reorder the factors:

``` r
gapminder_asia_2002 %>% 
  ggplot(aes(pop, country)) + 
  geom_point() + 
  scale_x_log10() + 
  ggtitle('log(Population) of Asian Countries, 2002')
```

![](iris_files/figure-markdown_github/unnamed-chunk-11-1.png)

It's pretty diffcult to get any sense of ordering on this graph.

Now we will reorder the levels and re-make this plot:

``` r
gapminder_asia_2002 %>% 
  mutate(country = fct_reorder(country, pop, .fun=median)) %>% 
  ggplot(aes(pop, country)) + 
  geom_point() + 
  scale_x_log10() + 
  ggtitle('log(Population) of Asian Countries, 2002')
```

![](iris_files/figure-markdown_github/unnamed-chunk-12-1.png)

This is clearly a much better graph as it also allows us to - view the extreme points much easily - view the distribution much easily

### arrange

It seems that we should be able to do the same thing with `arrange()`. After all, we are only sorting the data before plotting.

``` r
gapminder_asia_2002 %>% 
  arrange(pop)  %>% 
  ggplot(aes(pop, country)) + 
  geom_point() + 
  scale_x_log10() + 
  ggtitle('log(Population) of Asian Countries, 2002')
```

![](iris_files/figure-markdown_github/unnamed-chunk-13-1.png)

This does not work because we are not changing the factors, which the plot is based off of. We are changing the rows in the table, but the categories are still plotted alphabetically.

Using `fct_reorder`, on the other hand, actually relabels the categories according to the ranking of their population. Hence, the plot with `fct_reorder` is different because the first category corresponds to the country with highest population, instead of the first country that comes along alphabetically.

file i/o
--------

We will demonstrate file i/o with the `gapminder_2002` data frame.

### write\_csv()/read\_csv()

``` r
write_csv(gapminder_2002, 'gapminder_2002.csv')
```

confirm that the file exists:

``` r
list.files(pattern = "gapminder_2002.csv")
```

    ## [1] "gapminder_2002.csv"

We see that the file `gapminder_2002.csv` exists so we know that `write_csv` worked as intended

``` r
read_data = read_csv('gapminder_2002.csv')
```

    ## Parsed with column specification:
    ## cols(
    ##   country = col_character(),
    ##   continent = col_character(),
    ##   year = col_integer(),
    ##   lifeExp = col_double(),
    ##   pop = col_integer(),
    ##   gdpPercap = col_double()
    ## )

``` r
read_data %>% str()
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    142 obs. of  6 variables:
    ##  $ country  : chr  "Afghanistan" "Albania" "Algeria" "Angola" ...
    ##  $ continent: chr  "Asia" "Europe" "Africa" "Africa" ...
    ##  $ year     : int  2002 2002 2002 2002 2002 2002 2002 2002 2002 2002 ...
    ##  $ lifeExp  : num  42.1 75.7 71 41 74.3 ...
    ##  $ pop      : int  25268405 3508512 31287142 10866106 38331121 19546792 8148312 656397 135656790 10311970 ...
    ##  $ gdpPercap: num  727 4604 5288 2773 8798 ...
    ##  - attr(*, "spec")=List of 2
    ##   ..$ cols   :List of 6
    ##   .. ..$ country  : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_character" "collector"
    ##   .. ..$ continent: list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_character" "collector"
    ##   .. ..$ year     : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_integer" "collector"
    ##   .. ..$ lifeExp  : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ pop      : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_integer" "collector"
    ##   .. ..$ gdpPercap: list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   ..$ default: list()
    ##   .. ..- attr(*, "class")= chr  "collector_guess" "collector"
    ##   ..- attr(*, "class")= chr "col_spec"

We don't see a factor anywhere. This indicates that the factors are not preserved after writing to a CSV file. We will see a better method to do this in the next section.

### saveRDS()/readRDS()

``` r
gapminder_2002 %>% saveRDS('gapminder_2002.rds')
```

Check to make sure that the file exists:

``` r
list.files(pattern = "gapminder_2002.rds")
```

    ## [1] "gapminder_2002.rds"

As expected, the file exists.

Now, read in the file again:

``` r
rds_file = readRDS('gapminder_2002.rds')
```

No errors! That's a good start, now let's check the dataset description:

``` r
rds_file %>% str()
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    142 obs. of  6 variables:
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 4 1 1 2 5 4 3 3 4 ...
    ##  $ year     : int  2002 2002 2002 2002 2002 2002 2002 2002 2002 2002 ...
    ##  $ lifeExp  : num  42.1 75.7 71 41 74.3 ...
    ##  $ pop      : int  25268405 3508512 31287142 10866106 38331121 19546792 8148312 656397 135656790 10311970 ...
    ##  $ gdpPercap: num  727 4604 5288 2773 8798 ...

As expected, we do not encounter any problems with reading in the `.rds` file. In particular, we note that country and continent are factors as expected.

(Note that I filtered from the *original* data frame, so we still have 142 countries and 5 continents)

part 3
------

I am going to re-make a plot I handed in for assignment 2:

**original:**

``` r
ggplot(gapminder, aes(continent)) + 
  geom_bar(fill = 'dark green')
```

![](iris_files/figure-markdown_github/unnamed-chunk-22-1.png)

Let's see how we can improve this: - count (on the y-axis is unclear). It seems that we are recording the number of countries, but it is not apparent from the axis - we should give a title - entries from different years are all mixed together. It is hard to imagine this being useful. - could be more colourful although the current scheme is readable

Instead, I am going to do the following: - contrast the total population of the 5 continents - express these values as percentages in order to make it easy to see which continents have increased and decreased their proportion of world population - give the graph a meaningful title - use colour to contrast the change or some other meaningful way - separate the years

First, let's calculate the sum of population for each continent/each year

``` r
plot_data = gapminder %>% 
  group_by(continent,year) %>% 
  summarize(totalPop = sum((as.double(pop)))) # we need this to prevent integer overflow 
```

We also need to get the world population for each point in time, which the following code block does:

``` r
plot_data = plot_data %>%
  group_by(year) %>% 
  mutate(popRatio = totalPop/sum(totalPop))
```

Finally, we generate a stacked-area graph, which allows us to accurately visualize the proportion of categories over time; in this case, it is the continents and how their population progresses as a proportion of the world population.

``` r
(improved_graph = plot_data %>%
  ggplot(aes(year, popRatio, fill=continent)) + 
  geom_area(position = 'stack') + 
  xlab('year') +
  ylab('Percentage of world population') +
  ggtitle('Proportion of world population in continents over time'))
```

![](iris_files/figure-markdown_github/unnamed-chunk-25-1.png)

We can see from this graph that Asia has the majority of the world's population, and Americas' hasn't changed must in the last 60 years or so. Africa's population has increased and Europe's has decreased. Oceania has always been rather un-populated.

### plotly

``` r
library(plotly)
```

    ## 
    ## Attaching package: 'plotly'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     last_plot

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

    ## The following object is masked from 'package:graphics':
    ## 
    ##     layout

``` r
improved_graph_plotly = improved_graph %>% ggplotly()
```

Let's take a look at the new file:

``` r
improved_graph_plotly
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-fb2c1537ffdf97c5f4d2">{"x":{"data":[{"x":[1952,1957,1962,1967,1972,1977,1982,1987,1992,1997,2002,2007,2007,2007,2002,1997,1992,1987,1982,1977,1972,1967,1962,1957,1952,1952],"y":[0.901269326294405,0.900601530267599,0.89774515277225,0.895791222509111,0.893798723273815,0.889807640351506,0.883586446047309,0.877472689563738,0.871039154350339,0.865130479245811,0.858378275641128,0.85129775519867,0.85129775519867,1,1,1,1,1,1,1,1,1,1,1,1,0.901269326294405],"text":["year: 1952<br />popRatio: 0.098730674<br />continent: Africa","year: 1957<br />popRatio: 0.099398470<br />continent: Africa","year: 1962<br />popRatio: 0.102254847<br />continent: Africa","year: 1967<br />popRatio: 0.104208777<br />continent: Africa","year: 1972<br />popRatio: 0.106201277<br />continent: Africa","year: 1977<br />popRatio: 0.110192360<br />continent: Africa","year: 1982<br />popRatio: 0.116413554<br />continent: Africa","year: 1987<br />popRatio: 0.122527310<br />continent: Africa","year: 1992<br />popRatio: 0.128960846<br />continent: Africa","year: 1997<br />popRatio: 0.134869521<br />continent: Africa","year: 2002<br />popRatio: 0.141621724<br />continent: Africa","year: 2007<br />popRatio: 0.148702245<br />continent: Africa","year: 2007<br />popRatio: 0.148702245<br />continent: Africa","year: 2007<br />popRatio: 0.148702245<br />continent: Africa","year: 2002<br />popRatio: 0.141621724<br />continent: Africa","year: 1997<br />popRatio: 0.134869521<br />continent: Africa","year: 1992<br />popRatio: 0.128960846<br />continent: Africa","year: 1987<br />popRatio: 0.122527310<br />continent: Africa","year: 1982<br />popRatio: 0.116413554<br />continent: Africa","year: 1977<br />popRatio: 0.110192360<br />continent: Africa","year: 1972<br />popRatio: 0.106201277<br />continent: Africa","year: 1967<br />popRatio: 0.104208777<br />continent: Africa","year: 1962<br />popRatio: 0.102254847<br />continent: Africa","year: 1957<br />popRatio: 0.099398470<br />continent: Africa","year: 1952<br />popRatio: 0.098730674<br />continent: Africa","year: 1952<br />popRatio: 0.098730674<br />continent: Africa"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"transparent","dash":"solid"},"fill":"toself","fillcolor":"rgba(248,118,109,1)","hoveron":"points","name":"Africa","legendgroup":"Africa","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1952,1957,1962,1967,1972,1977,1982,1987,1992,1997,2002,2007,2007,2002,1997,1992,1987,1982,1977,1972,1967,1962,1957,1952,1952],"y":[0.757871490566419,0.755370614923654,0.748330435227943,0.746374018840961,0.74580107424885,0.74271833722675,0.736646196427035,0.731941994183974,0.726387224111586,0.720638935179628,0.714030390738133,0.70750167634545,0.85129775519867,0.858378275641128,0.865130479245811,0.871039154350339,0.877472689563738,0.883586446047309,0.889807640351506,0.893798723273815,0.895791222509111,0.89774515277225,0.900601530267599,0.901269326294405,0.757871490566419],"text":["year: 1952<br />popRatio: 0.143397836<br />continent: Americas","year: 1957<br />popRatio: 0.145230915<br />continent: Americas","year: 1962<br />popRatio: 0.149414718<br />continent: Americas","year: 1967<br />popRatio: 0.149417204<br />continent: Americas","year: 1972<br />popRatio: 0.147997649<br />continent: Americas","year: 1977<br />popRatio: 0.147089303<br />continent: Americas","year: 1982<br />popRatio: 0.146940250<br />continent: Americas","year: 1987<br />popRatio: 0.145530695<br />continent: Americas","year: 1992<br />popRatio: 0.144651930<br />continent: Americas","year: 1997<br />popRatio: 0.144491544<br />continent: Americas","year: 2002<br />popRatio: 0.144347885<br />continent: Americas","year: 2007<br />popRatio: 0.143796079<br />continent: Americas","year: 2007<br />popRatio: 0.143796079<br />continent: Americas","year: 2002<br />popRatio: 0.144347885<br />continent: Americas","year: 1997<br />popRatio: 0.144491544<br />continent: Americas","year: 1992<br />popRatio: 0.144651930<br />continent: Americas","year: 1987<br />popRatio: 0.145530695<br />continent: Americas","year: 1982<br />popRatio: 0.146940250<br />continent: Americas","year: 1977<br />popRatio: 0.147089303<br />continent: Americas","year: 1972<br />popRatio: 0.147997649<br />continent: Americas","year: 1967<br />popRatio: 0.149417204<br />continent: Americas","year: 1962<br />popRatio: 0.149414718<br />continent: Americas","year: 1957<br />popRatio: 0.145230915<br />continent: Americas","year: 1952<br />popRatio: 0.143397836<br />continent: Americas","year: 1952<br />popRatio: 0.143397836<br />continent: Americas"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"transparent","dash":"solid"},"fill":"toself","fillcolor":"rgba(163,165,0,1)","hoveron":"points","name":"Americas","legendgroup":"Americas","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1952,1957,1962,1967,1972,1977,1982,1987,1992,1997,2002,2007,2007,2002,1997,1992,1987,1982,1977,1972,1967,1962,1957,1952,1952],"y":[0.178153089264593,0.168830338446573,0.163335903840644,0.154089418118683,0.144463086056978,0.135978957305828,0.128143103979123,0.119934196601093,0.113303712897236,0.107191960153313,0.102205026250874,0.0976879201041275,0.70750167634545,0.714030390738133,0.720638935179628,0.726387224111586,0.731941994183974,0.736646196427035,0.74271833722675,0.74580107424885,0.746374018840961,0.748330435227943,0.755370614923654,0.757871490566419,0.178153089264593],"text":["year: 1952<br />popRatio: 0.579718401<br />continent: Asia","year: 1957<br />popRatio: 0.586540276<br />continent: Asia","year: 1962<br />popRatio: 0.584994531<br />continent: Asia","year: 1967<br />popRatio: 0.592284601<br />continent: Asia","year: 1972<br />popRatio: 0.601337988<br />continent: Asia","year: 1977<br />popRatio: 0.606739380<br />continent: Asia","year: 1982<br />popRatio: 0.608503092<br />continent: Asia","year: 1987<br />popRatio: 0.612007798<br />continent: Asia","year: 1992<br />popRatio: 0.613083511<br />continent: Asia","year: 1997<br />popRatio: 0.613446975<br />continent: Asia","year: 2002<br />popRatio: 0.611825364<br />continent: Asia","year: 2007<br />popRatio: 0.609813756<br />continent: Asia","year: 2007<br />popRatio: 0.609813756<br />continent: Asia","year: 2002<br />popRatio: 0.611825364<br />continent: Asia","year: 1997<br />popRatio: 0.613446975<br />continent: Asia","year: 1992<br />popRatio: 0.613083511<br />continent: Asia","year: 1987<br />popRatio: 0.612007798<br />continent: Asia","year: 1982<br />popRatio: 0.608503092<br />continent: Asia","year: 1977<br />popRatio: 0.606739380<br />continent: Asia","year: 1972<br />popRatio: 0.601337988<br />continent: Asia","year: 1967<br />popRatio: 0.592284601<br />continent: Asia","year: 1962<br />popRatio: 0.584994531<br />continent: Asia","year: 1957<br />popRatio: 0.586540276<br />continent: Asia","year: 1952<br />popRatio: 0.579718401<br />continent: Asia","year: 1952<br />popRatio: 0.579718401<br />continent: Asia"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"transparent","dash":"solid"},"fill":"toself","fillcolor":"rgba(0,191,125,1)","hoveron":"points","name":"Asia","legendgroup":"Asia","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1952,1957,1962,1967,1972,1977,1982,1987,1992,1997,2002,2007,2007,2002,1997,1992,1987,1982,1977,1972,1967,1962,1957,1952,1952],"y":[0.00443963283683717,0.00448204303867395,0.00458086626451101,0.00453784369542481,0.00450271256666493,0.0043864628675052,0.00428840677369666,0.004172334907741,0.00409329622219672,0.00403274803552923,0.00398418860701423,0.00392735486184455,0.0976879201041275,0.102205026250874,0.107191960153313,0.113303712897236,0.119934196601093,0.128143103979123,0.135978957305828,0.144463086056978,0.154089418118683,0.163335903840644,0.168830338446573,0.178153089264593,0.00443963283683717],"text":["year: 1952<br />popRatio: 0.173713456<br />continent: Europe","year: 1957<br />popRatio: 0.164348295<br />continent: Europe","year: 1962<br />popRatio: 0.158755038<br />continent: Europe","year: 1967<br />popRatio: 0.149551574<br />continent: Europe","year: 1972<br />popRatio: 0.139960373<br />continent: Europe","year: 1977<br />popRatio: 0.131592494<br />continent: Europe","year: 1982<br />popRatio: 0.123854697<br />continent: Europe","year: 1987<br />popRatio: 0.115761862<br />continent: Europe","year: 1992<br />popRatio: 0.109210417<br />continent: Europe","year: 1997<br />popRatio: 0.103159212<br />continent: Europe","year: 2002<br />popRatio: 0.098220838<br />continent: Europe","year: 2007<br />popRatio: 0.093760565<br />continent: Europe","year: 2007<br />popRatio: 0.093760565<br />continent: Europe","year: 2002<br />popRatio: 0.098220838<br />continent: Europe","year: 1997<br />popRatio: 0.103159212<br />continent: Europe","year: 1992<br />popRatio: 0.109210417<br />continent: Europe","year: 1987<br />popRatio: 0.115761862<br />continent: Europe","year: 1982<br />popRatio: 0.123854697<br />continent: Europe","year: 1977<br />popRatio: 0.131592494<br />continent: Europe","year: 1972<br />popRatio: 0.139960373<br />continent: Europe","year: 1967<br />popRatio: 0.149551574<br />continent: Europe","year: 1962<br />popRatio: 0.158755038<br />continent: Europe","year: 1957<br />popRatio: 0.164348295<br />continent: Europe","year: 1952<br />popRatio: 0.173713456<br />continent: Europe","year: 1952<br />popRatio: 0.173713456<br />continent: Europe"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"transparent","dash":"solid"},"fill":"toself","fillcolor":"rgba(0,176,246,1)","hoveron":"points","name":"Europe","legendgroup":"Europe","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1952,1957,1962,1967,1972,1977,1982,1987,1992,1997,2002,2007,2007,2002,1997,1992,1987,1982,1977,1972,1967,1962,1957,1952,1952],"y":[0,0,0,0,0,0,0,0,0,0,0,0,0.00392735486184455,0.00398418860701423,0.00403274803552923,0.00409329622219672,0.004172334907741,0.00428840677369666,0.0043864628675052,0.00450271256666493,0.00453784369542481,0.00458086626451101,0.00448204303867395,0.00443963283683717,0],"text":["year: 1952<br />popRatio: 0.004439633<br />continent: Oceania","year: 1957<br />popRatio: 0.004482043<br />continent: Oceania","year: 1962<br />popRatio: 0.004580866<br />continent: Oceania","year: 1967<br />popRatio: 0.004537844<br />continent: Oceania","year: 1972<br />popRatio: 0.004502713<br />continent: Oceania","year: 1977<br />popRatio: 0.004386463<br />continent: Oceania","year: 1982<br />popRatio: 0.004288407<br />continent: Oceania","year: 1987<br />popRatio: 0.004172335<br />continent: Oceania","year: 1992<br />popRatio: 0.004093296<br />continent: Oceania","year: 1997<br />popRatio: 0.004032748<br />continent: Oceania","year: 2002<br />popRatio: 0.003984189<br />continent: Oceania","year: 2007<br />popRatio: 0.003927355<br />continent: Oceania","year: 2007<br />popRatio: 0.003927355<br />continent: Oceania","year: 2002<br />popRatio: 0.003984189<br />continent: Oceania","year: 1997<br />popRatio: 0.004032748<br />continent: Oceania","year: 1992<br />popRatio: 0.004093296<br />continent: Oceania","year: 1987<br />popRatio: 0.004172335<br />continent: Oceania","year: 1982<br />popRatio: 0.004288407<br />continent: Oceania","year: 1977<br />popRatio: 0.004386463<br />continent: Oceania","year: 1972<br />popRatio: 0.004502713<br />continent: Oceania","year: 1967<br />popRatio: 0.004537844<br />continent: Oceania","year: 1962<br />popRatio: 0.004580866<br />continent: Oceania","year: 1957<br />popRatio: 0.004482043<br />continent: Oceania","year: 1952<br />popRatio: 0.004439633<br />continent: Oceania","year: 1952<br />popRatio: 0.004439633<br />continent: Oceania"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"transparent","dash":"solid"},"fill":"toself","fillcolor":"rgba(231,107,243,1)","hoveron":"points","name":"Oceania","legendgroup":"Oceania","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":48.9497716894977},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":"Proportion of world population in continents over time","titlefont":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[1949.25,2009.75],"tickmode":"array","ticktext":["1950","1960","1970","1980","1990","2000"],"tickvals":[1950,1960,1970,1980,1990,2000],"categoryorder":"array","categoryarray":["1950","1960","1970","1980","1990","2000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":"year","titlefont":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.05,1.05],"tickmode":"array","ticktext":["0.00","0.25","0.50","0.75","1.00"],"tickvals":[0,0.25,0.5,0.75,1],"categoryorder":"array","categoryarray":["0.00","0.25","0.50","0.75","1.00"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":"Percentage of world population","titlefont":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"annotations":[{"text":"continent","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"cloud":false},"source":"A","attrs":{"5dd735cd02c0":{"x":{},"y":{},"fill":{},"type":"scatter"}},"cur_data":"5dd735cd02c0","visdat":{"5dd735cd02c0":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script>
<!--/html_preserve-->
The most distinctive thing about the `plotly` graph is *interactivity*: I can hover my mouse over a data point and I can read it off. This seems to be better for people using `Rmd` but not necessarily for publishing graphs in papers because obviously such a feature is not possible on paper or `pdf`.

Also, this is not checkable on github because it is rendered in `md`.

Part 4: saving figures to file
------------------------------

``` r
ggsave('pop_prop_time.png', plot = improved_graph)
```

    ## Saving 7 x 5 in image

![Graph of Population Proportions over time](https://github.com/STAT545-UBC-students/hw05-rning-wu/blob/master/pop_prop_time.png)
