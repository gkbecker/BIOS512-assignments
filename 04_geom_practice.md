Import the tidyverse.


```R
library('tidyverse')
options(repr.plot.width=4, repr.plot.height=3)
```

    ── [1mAttaching packages[22m ─────────────────────────────────────── tidyverse 1.2.1 ──
    
    [32m✔[39m [34mggplot2[39m 3.2.0     [32m✔[39m [34mpurrr  [39m 0.3.2
    [32m✔[39m [34mtibble [39m 2.1.3     [32m✔[39m [34mdplyr  [39m 0.8.3
    [32m✔[39m [34mtidyr  [39m 0.8.3     [32m✔[39m [34mstringr[39m 1.4.0
    [32m✔[39m [34mreadr  [39m 1.3.1     [32m✔[39m [34mforcats[39m 0.4.0
    
    ── [1mConflicts[22m ────────────────────────────────────────── tidyverse_conflicts() ──
    [31m✖[39m [34mdplyr[39m::[32mfilter()[39m masks [34mstats[39m::filter()
    [31m✖[39m [34mdplyr[39m::[32mlag()[39m    masks [34mstats[39m::lag()
    


# Bars

## Class Notes

What variables would be good for bar chart x axis?
- class
- manufacturer


```R
mpg %>% head
```


<table>
<caption>A tibble: 6 × 11</caption>
<thead>
	<tr><th scope=col>manufacturer</th><th scope=col>model</th><th scope=col>displ</th><th scope=col>year</th><th scope=col>cyl</th><th scope=col>trans</th><th scope=col>drv</th><th scope=col>cty</th><th scope=col>hwy</th><th scope=col>fl</th><th scope=col>class</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>audi</td><td>a4</td><td>1.8</td><td>1999</td><td>4</td><td>auto(l5)  </td><td>f</td><td>18</td><td>29</td><td>p</td><td>compact</td></tr>
	<tr><td>audi</td><td>a4</td><td>1.8</td><td>1999</td><td>4</td><td>manual(m5)</td><td>f</td><td>21</td><td>29</td><td>p</td><td>compact</td></tr>
	<tr><td>audi</td><td>a4</td><td>2.0</td><td>2008</td><td>4</td><td>manual(m6)</td><td>f</td><td>20</td><td>31</td><td>p</td><td>compact</td></tr>
	<tr><td>audi</td><td>a4</td><td>2.0</td><td>2008</td><td>4</td><td>auto(av)  </td><td>f</td><td>21</td><td>30</td><td>p</td><td>compact</td></tr>
	<tr><td>audi</td><td>a4</td><td>2.8</td><td>1999</td><td>6</td><td>auto(l5)  </td><td>f</td><td>16</td><td>26</td><td>p</td><td>compact</td></tr>
	<tr><td>audi</td><td>a4</td><td>2.8</td><td>1999</td><td>6</td><td>manual(m5)</td><td>f</td><td>18</td><td>26</td><td>p</td><td>compact</td></tr>
</tbody>
</table>




```R
# This plot shows you how many of each class of cars shows up in the data.

options(repr.plot.width=9, repr.plot.height=4)

p = ggplot(mpg, aes(x=class))
p = p + geom_bar()

p
```


![png](output_5_0.png)



```R
df = mpg %>%
    group_by(class) %>%
summarize(sum_displ = sum(displ))

p = ggplot(df, aes(x = class, y = sum_displ)) +
    geom_bar(stat = "identity")

p
```


![png](output_6_0.png)



```R
# An equivalent way of doing this with the table in its raw form, using weight argument to tell ggplot how to plot

p = ggplot(mpg, aes(x = class, weight = displ)) +
    geom_bar()

p

# Another version, with sorted bars and rotated axis + text

df.sorted = df %>% mutate(class = fct_reorder(class, sum_displ))

options(repr.plot.width=5, repr.plot.height=5)

p = ggplot(df.sorted, aes(x = class, y = sum_displ)) +
    geom_bar(stat = "identity") +
    theme(axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5)) +
    coord_flip()

p
```


![png](output_7_0.png)



![png](output_7_1.png)


Let's use the "diamonds" data set. Preview the first five rows of the diamonds dataset using the `head` function.  
(*Hint: try* `?head` *to get the help page for the* `head` *function*)


```R
diamonds %>% head
```


<table>
<caption>A tibble: 6 × 10</caption>
<thead>
	<tr><th scope=col>carat</th><th scope=col>cut</th><th scope=col>color</th><th scope=col>clarity</th><th scope=col>depth</th><th scope=col>table</th><th scope=col>price</th><th scope=col>x</th><th scope=col>y</th><th scope=col>z</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;ord&gt;</th><th scope=col>&lt;ord&gt;</th><th scope=col>&lt;ord&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0.23</td><td>Ideal    </td><td>E</td><td>SI2 </td><td>61.5</td><td>55</td><td>326</td><td>3.95</td><td>3.98</td><td>2.43</td></tr>
	<tr><td>0.21</td><td>Premium  </td><td>E</td><td>SI1 </td><td>59.8</td><td>61</td><td>326</td><td>3.89</td><td>3.84</td><td>2.31</td></tr>
	<tr><td>0.23</td><td>Good     </td><td>E</td><td>VS1 </td><td>56.9</td><td>65</td><td>327</td><td>4.05</td><td>4.07</td><td>2.31</td></tr>
	<tr><td>0.29</td><td>Premium  </td><td>I</td><td>VS2 </td><td>62.4</td><td>58</td><td>334</td><td>4.20</td><td>4.23</td><td>2.63</td></tr>
	<tr><td>0.31</td><td>Good     </td><td>J</td><td>SI2 </td><td>63.3</td><td>58</td><td>335</td><td>4.34</td><td>4.35</td><td>2.75</td></tr>
	<tr><td>0.24</td><td>Very Good</td><td>J</td><td>VVS2</td><td>62.8</td><td>57</td><td>336</td><td>3.94</td><td>3.96</td><td>2.48</td></tr>
</tbody>
</table>



Make a bar chart of the "cut" column. About how many rows are there for the cut category "Ideal"?


```R
diamonds$cut %>% table
```


    .
         Fair      Good Very Good   Premium     Ideal 
         1610      4906     12082     13791     21551 



```R
options(repr.plot.width=9, repr.plot.height=5)

p = ggplot(diamonds, aes(x = cut)) +
    geom_bar() +
    theme_gray(base_size = 24)

p

# There are approximately 21000 rows for the cut category "ideal"
```


![png](output_12_0.png)


Use `coord_flip` to rotate the chart by 90 degrees. 


```R
p = p + coord_flip()

p
```


![png](output_14_0.png)


# Lines

## Class notes




```R
test.data = data.frame(
    cat1=100 + c(0, cumsum(runif(49, -20, 20))),
    cat2=150 + c(0, cumsum(runif(49, -10, 10))),
    date=seq(as.Date("2002-01-01"), by="1 month", length.out=100)
) %>% gather(category, value, -date)

test.data %>% head(5)
```


<table>
<caption>A data.frame: 5 × 3</caption>
<thead>
	<tr><th scope=col>date</th><th scope=col>category</th><th scope=col>value</th></tr>
	<tr><th scope=col>&lt;date&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2002-01-01</td><td>cat1</td><td>100.0000</td></tr>
	<tr><td>2002-02-01</td><td>cat1</td><td>116.7923</td></tr>
	<tr><td>2002-03-01</td><td>cat1</td><td>133.0949</td></tr>
	<tr><td>2002-04-01</td><td>cat1</td><td>152.5969</td></tr>
	<tr><td>2002-05-01</td><td>cat1</td><td>154.4443</td></tr>
</tbody>
</table>




```R
p = ggplot(test.data, aes(x=date, y=value)) +
    geom_line()

p
```


![png](output_18_0.png)



```R
p = ggplot(test.data, aes(x = date, y = value, group = category)) +
    geom_line()

p

p = ggplot(test.data, aes(x = date, y = value, color = category)) +
    geom_line()

p
```


![png](output_19_0.png)



![png](output_19_1.png)


We'll use flight data from the Bureau of Transportation Statistics
https://www.transtats.bts.gov/DatabaseInfo.asp?DB_ID=120&Link=0


```R
# uncomment the following:

library(nycflights13)
 flight.data = flights %>%
     group_by(month, carrier, year) %>%
     summarize(N_flights = n()) %>%
     filter(carrier %in% c('UA', 'AA', 'US'))
```

What are the columns in `flight.data`? (*Hint: preview the table*)


```R
flight.data %>% head

# The columns are month, carrier, year, and the number of flights (N_flights)
```


<table>
<caption>A grouped_df: 6 × 4</caption>
<thead>
	<tr><th scope=col>month</th><th scope=col>carrier</th><th scope=col>year</th><th scope=col>N_flights</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td>AA</td><td>2013</td><td>2794</td></tr>
	<tr><td>1</td><td>UA</td><td>2013</td><td>4637</td></tr>
	<tr><td>1</td><td>US</td><td>2013</td><td>1602</td></tr>
	<tr><td>2</td><td>AA</td><td>2013</td><td>2517</td></tr>
	<tr><td>2</td><td>UA</td><td>2013</td><td>4346</td></tr>
	<tr><td>2</td><td>US</td><td>2013</td><td>1552</td></tr>
</tbody>
</table>



First, make a bar chart of the `carrier` column? Does this make sense?


```R
p = ggplot(flight.data, aes(x = carrier)) +
    geom_bar()

p

# There are the same number of rows for each carrier (1 for each month in a year).
```


![png](output_25_0.png)


Plot month versus number of flights grouped according to carrier.


```R
p = ggplot(flight.data, aes(x = month, y = N_flights, color = carrier)) +
    geom_line() +
    theme_bw(base_size = 24)

p
```


![png](output_27_0.png)


# Smooth

## Class Notes


```R
p = ggplot(mpg, aes(displ, y = hwy, color = factor(cyl))) +
    geom_point() +
    geom_smooth() +
    theme_bw()

p
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'
    



![png](output_30_1.png)


Plot a scatter plot (`geom_point`) of carat versus price with the diamonds dataset.


```R
p = ggplot(diamonds, aes(x = carat, y = price)) +
    geom_point()

p
```


![png](output_32_0.png)


There is a lot of overplotting in this figure. Make the same plot but use the `alpha` value to reduce
the opacity of the points.  
(*Hint: alpha values can be set from 0 (transparent) to 1 (opaque)*)


```R
p = ggplot(diamonds, aes(x = carat, y = price)) +
    geom_point(alpha = 0.2)

p
```

Facetting can also help with overplotting. Facet the chart by `cut`.


```R
options(repr.plot.width=7, repr.plot.height=6)

p = ggplot(diamonds, aes(x = carat, y = price)) +
    geom_point(alpha = 0.2) +
    facet_grid(cut ~ .)

p
```

Add a `geom_smooth` to your facetted plot to emphasize the trend in the data.


```R
(p = p + geom_smooth())
```

# Tiles

This is a dataset of otter skull morphology.


```R
# uncomment the following:
# otter.data = read.csv('https://jcoliver.github.io/learn-r/data/otter-mandible-data.csv') %>%
#     gather(characteristic, value, -species, -museum, -accession)

# otter.data %>% head
```

Make a bar chart of the museum column. What is this chart telling you?


```R
# options(repr.plot.width=4, repr.plot.height=3)
```

Make a heatmap of the data with `characteristic` on the x-axis and `species` on the y-axis.


```R
# uncomment this to get a scaled version of the data:

# otter.data.scaled = otter.data %>%
#     group_by(characteristic) %>%
#     mutate(value.scaled = scales::rescale(value))

# otter.data.scaled %>% head
```


```R
# options(repr.plot.width=5, repr.plot.height=2.5)
```

What can you do to make the chart more visually appealing?
- make the `color` white
- use `scale_fill_gradient(low = "white", high = "steelblue")`


```R

```

# Points with jitter

Let's stick with the otter data. We could also use a point+jitter plot to represent the data. Make a point+jitter plot below with species on the x-axis, value on the y-axis, and facet the chart by characteristic. 


```R

```

Make the axis test legible by rotating the text 45 degrees.


```R

```

What happens if you pass the argument `scales='free_y` to `facet_wrap`?


```R

```

Do you prefer the heatmap or the point+jitter plot?
