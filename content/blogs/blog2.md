---
categories:
- ""
- ""
date: "2017-10-31T22:26:09-05:00"
description: Lorem Etiam Nullam
draft: false
image: pic09.jpg
keywords: ""
slug: magna
title: Magna
---



```{r load-libraries, warning=FALSE, message=FALSE, echo=FALSE, show= FALSE}
#above is just to exclude lines where they are warnings and message that come in red
library(tidyverse)  # Load ggplot2, dplyr, and all the other tidyverse packages
library(gapminder)  # gapminder dataset
library(here)
library(janitor)
```

## Hello!

My name is **Tanisha Yadav** and I'm a *tech enthusiast and a keen
believer in learning.* Having worked at the *BlackRock India* Investment
Accounting business (IAG) for over two years straight out of university,
I developed a broad understanding of financial world and clients.\
I also had the opportunity to understand deep implication of the data
analytics on the finance industry. Over these two years with the firm, I
became a critical resource of the team and gained a reputation of being
proactive, quick learner and a team player.\
I graduated from the **Shri Ram College of Commerce** in 2019 with a
Bachelors in Commerce. While in school, I earned *excellence award* for
my exemplary academic performance and leadership skills.\
I love to go for hikes and treks though I never been to only [Kareri
Lake trek](http://indiahikes.com/documented-trek/kareri-lake-trek/#gref)
but somehow it still excites me.

A little more info about me goes as follows: \* \#\#\# Expert MIS and
Data Analytics \* \#\#\# Beginner Python and R \* \#\#\# Intermediate
Tableau

![](https://commonmark.org/help/images/favicon.png)

# Task 2: `gapminder` country comparison

You have seen the `gapminder` dataset that has data on life expectancy,
population, and GDP per capita for 142 countries from 1952 to 2007. To
get a glimpse of the dataframe, namely to see the variable names,
variable types, etc., we use the `glimpse` function. We also want to
have a look at the first 20 rows of data.

```{r}
glimpse(gapminder)

head(gapminder, 20) # look at the first 20 rows of the dataframe

```

Your task is to produce two graphs of how life expectancy has changed
over the years for the `country` and the `continent` you come from.

I have created the `country_data` and `continent_data` with the code
below.

```{r}
country_data <- gapminder %>% 
            filter(country == "India") # just choosing Greece, as this is where I come from

continent_data <- gapminder %>% 
            filter(continent == "Asia")
```

First, create a plot of life expectancy over time for the single country
you chose. Map `year` on the x-axis, and `lifeExp` on the y-axis. You
should also use `geom_point()` to see the actual data points and
`geom_smooth(se = FALSE)` to plot the underlying trendlines. You need to
remove the comments **\#** from the lines below for your code to run.

```{r, lifeExp_one_country}
plot1 <- ggplot(continent_data, mapping = aes(x = year, y = lifeExp))+
geom_col() +
geom_smooth(se = FALSE)+
NULL 


```

Next we need to add a title. Create a new plot, or extend plot1, using
the `labs()` function to add an informative title to the plot.

```{r, lifeExp_one_country_with_label}
plot1 <- plot1 +
labs(title = "Change in Life Expectancy over the years in India ",
     x = "Year",
     y = "Life Expectancy") +
  NULL



```

Secondly, produce a plot for all countries in the *continent* you come
from. (Hint: map the `country` variable to the colour aesthetic. You
also want to map `country` to the `group` aesthetic, so all points for
each country are grouped together).

```{r lifeExp_one_continent}
ggplot(gapminder, mapping = aes(x = year , y =lifeExp, colour= country, group =country))+
geom_point() + 
geom_smooth(se = FALSE) +
NULL
```

Finally, using the original `gapminder` data, produce a life expectancy
over time graph, grouped (or faceted) by continent. We will remove all
legends, adding the `theme(legend.position="none")` in the end of our
ggplot.

```{r lifeExp_facet_by_continent}
ggplot(data = gapminder , mapping = aes(x = year , y = lifeExp , colour= continent))+
geom_point()+ 
geom_smooth(se = FALSE) +
scale_y_log10()+
facet_wrap(~continent) +
theme(legend.position="none") + #remove all legends
NULL
```

Given these trends, what can you say about life expectancy since 1952?
Again, don't just say what's happening in the graph. Tell some sort of
story and speculate about the differences in the patterns.

> Type your answer after this blockquote.

Life Expectancy since 1952 has increased across all the continents.
However, the life expectancy in Africa remained almost the same post
1990 with one outlier in 1991. For rest of the continents, the life
expectancy has increased since 1952 though we can see lot of outliers in
Asia and Europe.

# Task 4: Animal rescue incidents attended by the London Fire Brigade

[The London Fire
Brigade](https://data.london.gov.uk/dataset/animal-rescue-incidents-attended-by-lfb)
attends a range of non-fire incidents (which we call 'special
services'). These 'special services' include assistance to animals that
may be trapped or in distress. The data is provided from January 2009
and is updated monthly. A range of information is supplied for each
incident including some location information (postcode, borough, ward),
as well as the data/time of the incidents. We do not routinely record
data about animal deaths or injuries.

Please note that any cost included is a notional cost calculated based
on the length of time rounded up to the nearest hour spent by Pump,
Aerial and FRU appliances at the incident and charged at the current
Brigade hourly rate.

```{r load_animal_rescue_data, warning=FALSE, message=FALSE}

url <- "https://data.london.gov.uk/download/animal-rescue-incidents-attended-by-lfb/8a7d91c2-9aec-4bde-937a-3998f4717cd8/Animal%20Rescue%20incidents%20attended%20by%20LFB%20from%20Jan%202009.csv"

animal_rescue <- read_csv(url,
                          locale = locale(encoding = "CP1252")) %>% 
  janitor::clean_names()


glimpse(animal_rescue)
```

One of the more useful things one can do with any data set is quick
counts, namely to see how many observations fall within one category.
For instance, if we wanted to count the number of incidents by year, we
would either use `group_by()... summarise()` or, simply
[`count()`](https://dplyr.tidyverse.org/reference/count.html)

```{r, instances_by_calendar_year}

animal_rescue %>% 
  dplyr::group_by(cal_year) %>% 
  summarise(count=n())

animal_rescue %>% 
  count(cal_year, name="count")

```

Let us try to see how many incidents we have by animal group. Again, we
can do this either using group_by() and summarise(), or by using count()

```{r, animal_group_percentages}
animal_rescue %>% 
  group_by(animal_group_parent) %>% 
  
  #group_by and summarise will produce a new column with the count in each animal group
  summarise(count = n()) %>% 
  
  # mutate adds a new column; here we calculate the percentage
  mutate(percent = round(100*count/sum(count),2)) %>% 
  
  # arrange() sorts the data by percent. Since the default sorting is min to max and we would like to see it sorted
  # in descending order (max to min), we use arrange(desc()) 
  arrange(desc(percent))


animal_rescue %>% 
  
  #count does the same thing as group_by and summarise
  # name = "count" will call the column with the counts "count" ( exciting, I know)
  # and 'sort=TRUE' will sort them from max to min
  count(animal_group_parent, name="count", sort=TRUE) %>% 
  mutate(percent = round(100*count/sum(count),2))


```

Do you see anything strange in these tables?

Finally, let us have a loot at the notional cost for rescuing each of
these animals. As the LFB says,

> Please note that any cost included is a notional cost calculated based
> on the length of time rounded up to the nearest hour spent by Pump,
> Aerial and FRU appliances at the incident and charged at the current
> Brigade hourly rate.

There is two things we will do:

1.  Calculate the mean and median `incident_notional_cost` for each
    `animal_group_parent`
2.  Plot a boxplot to get a feel for the distribution of
    `incident_notional_cost` by `animal_group_parent`.

Before we go on, however, we need to fix `incident_notional_cost` as it
is stored as a `chr`, or character, rather than a number.

```{r, parse_incident_cost,message=FALSE, warning=FALSE}

# what type is variable incident_notional_cost from dataframe `animal_rescue`
typeof(animal_rescue$incident_notional_cost)

# readr::parse_number() will convert any numerical values stored as characters into numbers
animal_rescue <- animal_rescue %>% 

  # we use mutate() to use the parse_number() function and overwrite the same variable
  mutate(incident_notional_cost = parse_number(incident_notional_cost))

# incident_notional_cost from dataframe `animal_rescue` is now 'double' or numeric
typeof(animal_rescue$incident_notional_cost)

```

Now tht incident_notional_cost is numeric, let us quickly calculate
summary statistics for each animal group.

```{r, stats_on_incident_cost,message=FALSE, warning=FALSE}

animal_rescue %>% 
  
  # group by animal_group_parent
  group_by(animal_group_parent) %>% 
  
  # filter resulting data, so each group has at least 6 observations
  filter(n()>6) %>% 
  
  # summarise() will collapse all values into 3 values: the mean, median, and count  
  # we use na.rm=TRUE to make sure we remove any NAs, or cases where we do not have the incident cos
  summarise(mean_incident_cost = mean (incident_notional_cost, na.rm=TRUE),
            median_incident_cost = median (incident_notional_cost, na.rm=TRUE),
            sd_incident_cost = sd (incident_notional_cost, na.rm=TRUE),
            min_incident_cost = min (incident_notional_cost, na.rm=TRUE),
            max_incident_cost = max (incident_notional_cost, na.rm=TRUE),
            count = n()) %>% 
  
  # sort the resulting data in descending order. You choose whether to sort by count or mean cost.
  arrange((desc(mean_incident_cost)))

```

Compare the mean and the median for each animal group. waht do you think
this is telling us? Anything else that stands out? Any outliers?

Finally, let us plot a few plots that show the distribution of
incident_cost for each animal group.

```{r, plots_on_incident_cost_by_animal_group,message=FALSE, warning=FALSE}

# base_plot
base_plot <- animal_rescue %>% 
  group_by(animal_group_parent) %>% 
  filter(n()>6) %>% 
  ggplot(aes(x=incident_notional_cost))+
  facet_wrap(~animal_group_parent, scales = "free")+
  theme_bw()

base_plot + geom_histogram()
base_plot + geom_density()
base_plot + geom_boxplot()
base_plot + stat_ecdf(geom = "step", pad = FALSE) +
  scale_y_continuous(labels = scales::percent)



```

Which of these four graphs do you think best communicates the
variability of the `incident_notional_cost` values? Also, can you please
tell some sort of story (which animals are more expensive to rescue than
others, the spread of values) and speculate about the differences in the
patterns . I believe cumulative distribution graph best communicates the
variability of incident notional cost. Per graphs, Horses are more
expensive to rescue than others animals.

# Submit the assignment

Knit the completed R Markdown file as an HTML document (use the "Knit"
button at the top of the script editor window) and upload it to Canvas.

## Details

If you want to, please answer the following

-   Who did you collaborate with: TYPE NAMES HERE: did it myself
-   Approximately how much time did you spend on this problem set:
    ANSWER HERE: i spent 2.5 hours
-   What, if anything, gave you the most trouble: ANSWER HERE: I felt it
    was bit confusing

