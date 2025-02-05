---
title: Writing Data
teaching: 10
exercises: 10
source: Rmd
---

> ## Learning Objectives {.objectives}
> 
> * To be able to write out plots and data from R.
> 
> ##### Questions {.questions}
> 
> * How can I save plots and data created in R?


~~~ {.r}
library("ggplot2")
gapminder <- read.csv("data/gapminder_data.csv", header = TRUE)
dir.create("cleaned-data")
~~~

## Saving plots

You have already seen how to save the most recent plot you create in `ggplot2`,
using the command `ggsave`. As a refresher:

~~~ {.r}
ggsave("My_most_recent_plot.pdf")
~~~

You can save a plot from within RStudio using the 'Export' button
in the 'Plot' window. This will give you the option of saving as a
.pdf or as .png, .jpg or other image formats.

Sometimes you will want to save plots without creating them in the
'Plot' window first. Perhaps you want to make a pdf document with
multiple pages: each one a different plot, for example. Or perhaps
you're looping through multiple subsets of a file, plotting data from
each subset, and you want to save each plot, but obviously can't stop
the loop to click 'Export' for each one.

In this case you can use a more flexible approach. The function
`pdf` creates a new pdf device. You can control the size and resolution
using the arguments to this function.

~~~ {.r}
pdf("Life_Exp_vs_time.pdf", width=12, height=4)
ggplot(data=gapminder, aes(x=year, y=lifeExp, colour=country)) +
  geom_line() +
  theme(legend.position = "none")

# You then have to make sure to turn off the pdf device!

dev.off()
~~~

Open up this document and have a look.


> ## Challenge 1 {.challenge}
> 
> Rewrite your 'pdf' command to print a second
> page in the pdf, showing a facet plot (hint: use `facet_grid`)
> of the same data with one panel per continent.
> 
> 
> ##### Solution
> 
> ~~~ {.r}
> pdf("Life_Exp_vs_time.pdf", width = 12, height = 4)
> p <- ggplot(data = gapminder, aes(x = year, y = lifeExp, colour = country)) +
>   geom_line() +
>   theme(legend.position = "none")
> p
> p + facet_grid(~continent)
> dev.off()
> ~~~



The commands `jpeg`, `png` etc. are used similarly to produce
documents in different formats.

## Writing data

At some point, you'll also want to write out data from R.

We can use the `write.table` function for this, which is
very similar to `read.table` from before.

Let's create a data-cleaning script, for this analysis, we
only want to focus on the gapminder data for Australia:

~~~ {.r}
aust_subset <- gapminder[gapminder$country == "Australia",]

write.table(aust_subset,
  file="cleaned-data/gapminder-aus.csv",
  sep=","
)
~~~

Let's switch back to the shell to take a look at the data to make sure it looks
OK:

~~~{r, engine="bash"}
head cleaned-data/gapminder-aus.csv
~~~

Hmm, that's not quite what we wanted. Where did all these
quotation marks come from? Also the row numbers are
meaningless.

Let's look at the help file to work out how to change this
behaviour.

~~~ {.r}
?write.table
~~~

By default R will wrap character vectors with quotation marks
when writing out to file. It will also write out the row and
column names.

Let's fix this:

~~~ {.r}
write.table(
  gapminder[gapminder$country == "Australia",],
  file="cleaned-data/gapminder-aus.csv",
  sep=",", quote=FALSE, row.names=FALSE
)
~~~

Now lets look at the data again using our shell skills:

~~~{.bash}
head cleaned-data/gapminder-aus.csv
~~~

That looks better!


> ## Challenge 2 {.challenge}
> 
> Write a data-cleaning script file that subsets the gapminder
> data to include only data points collected since 1990.
> 
> Use this script to write out the new subset to a file
> in the `cleaned-data/` directory.
> 
> 
> ##### Solution
> 
> ~~~ {.r}
> write.table(
>   gapminder[gapminder$year > 1990, ],
>   file = "cleaned-data/gapminder-after1990.csv",
>   sep = ",", quote = FALSE, row.names = FALSE
> )
> ~~~



~~~ {.r}
# We remove after rendering the lesson, because we don't want this in the lesson
# repository
unlink("cleaned-data", recursive=TRUE)
~~~

> ## Keypoints {.objectives}
> 
> * Save plots from RStudio using the 'Export' button.
> * Use `write.table` to save tabular data.



