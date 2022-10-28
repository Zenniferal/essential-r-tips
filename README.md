# Essential R tips
Some common R commands that I believe will be useful for anyone new to the language. This includes the basics required to process and clean a dataset, then transforming that dataset and creating some basic visualisations.

## Table of Contents
1. [Cleaning Data](#cleaningdata)
2. [Transforming Data](#transformingdata)
3. [Visualisation](#visualisation)

### Cleaning Data <a name="cleaningdata"></a>

The functions in this tutorial require installing the tidyverse package and library.
We also require R's built in palmer penguins dataset.
```
install.packages("tidyverse")
library(tidyverse)

install.packages("palmerpenguins")
library(palmerpenguins)
```

The data() function loads the dataset to be used in this tutorial and the View() command allows us to see the records in the dataset.
```
data(penguins)
View(penguins)
```

Returns only the column you selected.
```
penguins %>%  
  select(species)
# Adding a - in front of the column will exclude that column
penguins %>%  
  select(-species)
```

Rename a column
```
penguins %>%
  rename(island_new=island)
# Rename with can use toupper and tolower to change cases
penguins %>%  
  rename_with(tolower)
```

To create consistent naming schemes we will load the janitor package and library which allows us to then use the clean_names() function.
```
install.packages("janitor")
library(janitor)

penguins <- clean_names(penguins)
penguins_raw <- clean_names(penguins_raw)
```

Arrange column in ascending or descending order
```
penguins %>%  
  arrange(bill_length_mm)
penguins %>%  
  arrange(-bill_length_mm)
```

Here we make changes to the existing data frame and then save it as a new data frame
```
penguins2 <- penguins %>% 
  arrange(-bill_length_mm)
  
View(penguins2)
```

How to use group by with summarize and mean
```
penguins %>%  
  group_by(island) %>%  
  drop_na() %>%  
  summarize(mean_bill_length_mm = mean(bill_length_mm))
```

How to use group by with summarize and max
```
penguins %>%  
  group_by(island) %>%  
  drop_na() %>%  
  summarize(max_bill_length_mm = max(bill_length_mm))
```

How to use group by with summarize and mean and max
```
penguins %>%  
  group_by(species, island) %>%  
  drop_na() %>%  
  summarize(max_bl_mm = max(bill_length_mm), mean_bl_mm = mean(bill_length_mm))
```

With the filter() function we can return only data that matches the criteria
```
penguins %>%  
  filter(species == "Adelie")
```

### Transforming Data <a name="transformingdata"></a>

To separate one column into multiple columns we can use the separate() function. As an example we will separate the stage column into stage_only and maturity. We use sep="," to indicate that the comma is the position in the string where the separation will occur.
Then we can use the trimws() function to remove leading whitespace from the records in the maturity column.
```
penguins_raw <- penguins_raw %>%  separate(stage, c("stage_only","maturity"), sep=",")
penguins_raw$maturity <- trimws(penguins_raw$maturity) 
```

Now let's unite the two columns, stage_only and maturity so that they once again ,contain their data in a single column called stage.
```
penguins_raw <- penguins_raw %>%  unite("stage", stage_only, maturity, sep=", ")
```

We can use the mutate() function which will both create a new column and allow us to use data from existing columns to populate the new column. In this example we will create a column called culmen_length_cm. The data of this column is the culmen_length_mm column divided by 10 to convert it to centimetres.
```
penguins_raw <- penguins_raw %>%  mutate(culmen_length_cm = culmen_length_mm/10)
```

### Visualisation <a name="visualisation"></a>

To create a scatter plot in R we can use the geom_point function with the ggplot package. Below we will create a scatter plot that maps the body mass on the x-axis and the flipper length on the y-axis.

```
ggplot(data=penguins_raw) + geom_point(mapping=aes(x=body_mass_g, y=flipper_length_mm))
```

You might notice that there is a clear correlation between body mass and flipper length. However we don't know much else about this correlation. Let's say we want to know which species is represented by which point on the scatter plot. To do so we can add the species to the color aesthetic.

```
ggplot(data=penguins_raw) + geom_point(mapping=aes(x=body_mass_g, y=flipper_length_mm, color=species))
```

Now we know which species is represented by each point by looking at their colors and the associated legend. We can also have each species showing on their own chart by using the facet_wrap() function.

```
ggplot(data=penguins_raw) + 
  geom_point(mapping=aes(x=body_mass_g, y=flipper_length_mm, color=species)) + 
  facet_wrap(~species)
```
