# Essential R tips
Some common R commands that I believe will be useful for anyone new to the language. This includes the basics required to process and clean a dataset, then transforming that dataset and creating some basic visualisations.
### Cleaning data

The functions in this tutorial require installing the tidyverse package and library.
We also require R's built in palmer penguins dataset.
```
install.packages("tidyverse")
library(tidyverse)

install.packages("palmerpenguins")
library(penguins)
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
Creates consistent naming schemes
```
clean_names(penguins)
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

