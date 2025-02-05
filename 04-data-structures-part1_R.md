---
title: Data Structures
teaching: 40
exercises: 15
source: Rmd
---

> ## Learning Objectives {.objectives}
> 
> * To be able to identify the 5 main data types.
> * To begin exploring data frames, and understand how they are related to vectors and lists.
> * To be able to ask questions from R about the type, class, and structure of an object.
> * To understand the information of the attributes "names", "class", and "dim".
> 
> ##### Questions {.questions}
> 
> * How can I read data in R?
> * What are the basic data types in R?
> * How do I represent categorical information in R?


One of R's most powerful features is its ability to deal with tabular data -
such as you may already have in a spreadsheet or a CSV file. Let's start by
making a toy dataset in your `data/` directory, called `feline-data.csv`:

~~~ {.r}
cats <- data.frame(coat = c("calico", "black", "tabby"),
                    weight = c(2.1, 5.0, 3.2),
                    likes_string = c(1, 0, 1))
~~~

We can now save `cats` as a CSV file. It is good practice to call the argument
names explicitly so the function knows what default values you are changing. Here we
are setting `row.names = FALSE`. Recall you can use `?write.csv` to pull
up the help file to check out the argument names and their default values.

~~~ {.r}
getwd()
write.csv(x = cats, file = "./Bioinfo_Course/feline-data.csv", row.names = FALSE)
~~~

The contents of the new file, `feline-data.csv`:

~~~ {.output}
"coat","weight","likes_string"
"calico",2.1,1
"black",5,0
"tabby",3.2,1
~~~


> ### Tip: Editing Text files in R {.callout}
> 
> Alternatively, you can create `data/feline-data.csv` using a text editor (Nano),
> or within RStudio with the **File -> New File -> Text File** menu item.


We can load this into R via the following:

~~~ {.r}
cats <- read.csv(file = "./Bioinfo_Course/feline-data.csv")
cats
~~~
~~~ {.output}
    coat weight likes_string
1 calico    2.1            1
2  black    5.0            0
3  tabby    3.2            1
~~~

The `read.table` function is used for reading in tabular data stored in a text
file where the columns of data are separated by punctuation characters such as
CSV files (csv = comma-separated values). Tabs and commas are the most common
punctuation characters used to separate or delimit data points in csv files.
For convenience R provides 2 other versions of `read.table`. These are: `read.csv`
for files where the data are separated with commas and `read.delim` for files
where the data are separated with tabs. Of these three functions `read.csv` is
the most commonly used.  If needed it is possible to override the default
delimiting punctuation marks for both `read.csv` and `read.delim`.


> ### Check your data for factors
> 
> In recent times, the default way how R handles textual data has changed. Text
> data was interpreted by R automatically into a format called "factors". But
> there is an easier format that is called "character". We will hear about
> factors later, and what to use them for. For now, remember that in most cases,
> they are not needed and only complicate your life, which is why newer R
> versions read in text as "character". Check now if your version of R has
> automatically created factors and convert them to "character" format:
> 
> 1. Check the data types of your input by typing `str(cats)`
> 2. In the output, look at the three-letter codes after the colons: If you see
>   only "num" and "chr", you can continue with the lesson and skip this box.
>   If you find "fct", continue to step 3.
> 3. Prevent R from automatically creating "factor" data. That can be done by
>   the following code: `options(stringsAsFactors = FALSE)`. Then, re-read
>   the cats table for the change to take effect.
> 4. You must set this option every time you restart R. To not forget this,
>   include it in your analysis script before you read in any data, for example
>   in one of the first lines.
> 5. For R versions greater than 4.0.0, text data is no longer converted to
>   factors anymore. So you can install this or a newer version to avoid this
>   problem. If you are working on an institute or company computer, ask your
>   administrator to do it.


We can begin exploring our dataset right away, pulling out columns by specifying
them using the `$` operator:

~~~ {.r}
cats$weight
~~~
~~~ {.output}
[1] 2.1 5.0 3.2
~~~
~~~ {.r}
cats$coat
~~~
~~~ {.output}
[1] "calico" "black"  "tabby" 
~~~

We can do other operations on the columns:

~~~ {.r}
## Say we discovered that the scale weighs two Kg light:
cats$weight + 2
~~~
~~~ {.output}
[1] 4.1 7.0 5.2
~~~
~~~ {.r}
paste("My cat is", cats$coat)
~~~
~~~ {.output}
[1] "My cat is calico" "My cat is black"  "My cat is tabby"
~~~

But what about

~~~ {.r}
cats$weight + cats$coat
~~~
~~~ {.error}
Error in cats$weight + cats$coat: non-numeric argument to binary operator
~~~

Understanding what happened here is key to successfully analyzing data in R.

### Data Types

If you guessed that the last command will return an error because `2.1` plus
`"black"` is nonsense, you're right - and you already have some intuition for an
important concept in programming called *data types*. We can ask what type of
data something is:

~~~ {.r}
typeof(cats$weight)
~~~
~~~ {.output}
[1] "double"
~~~

There are 5 main types: `double`, `integer`, `complex`, `logical` and `character`.
For historic reasons, `double` is also called `numeric`.

~~~ {.r}
typeof(3.14)
~~~
~~~ {.output}
[1] "double"
~~~

~~~ {.r}
typeof(1L) # The L suffix forces the number to be an integer, since by default R uses float numbers
~~~
~~~ {.output}
[1] "integer"
~~~

~~~ {.r}
typeof(1+1i)
~~~
~~~ {.output}
[1] "complex"
~~~

~~~ {.r}
typeof(TRUE)
~~~
~~~ {.output}
[1] "logical"
~~~

~~~ {.r}
typeof('banana')
~~~
~~~ {.output}
[1] "character"
~~~

No matter how
complicated our analyses become, all data in R is interpreted as one of these
basic data types. This strictness has some really important consequences.

A user has provided details of another cat. We will save this information in the file
`data/feline-data_v2.csv`.

~~~ {.r}
cats2<- data.frame(coat = c("calico", "black", "tabby","white"),
                    weight = c(2.1, 5.0,3.2,"2.3 or 2.4"),
                    likes_string = c(1, 0, 1,1))
write.csv(x = cats2, file = "./Bioinfo_Course/feline-data_v2.csv", row.names = FALSE)
~~~

Load the new cats data like before, and check what type of data we find in the
`weight` column:

~~~ {.r}
cats2 <- read.csv(file="./Bioinfo_Course/feline-data_v2.csv")
cats2
~~~
~~~ {.output}
    coat     weight likes_string
1 calico        2.1            1
2  black          5            0
3  tabby        3.2            1
4  white 2.3 or 2.4            1
~~~

~~~ {.r}
typeof(cats2$weight)
~~~
~~~ {.output}
[1] "character"
~~~

Oh no, our weights aren't the double type anymore! If we try to do the same math
we did on them before, we run into trouble:

~~~ {.r}
cats2$weight + 2
~~~
~~~ {.error}
Error in cats$weight + 2 : non-numeric argument to binary operator
~~~

What happened?
The `cats` data we are working with is something called a *data frame*. Data frames
are one of the most common and versatile types of *data structures* we will work with in R.
A given column in a data frame cannot be composed of different data types.
In this case, R does not read everything in the data frame column `weight` as a *double*, therefore the entire
column data type changes to something that is suitable for everything in the column.

When R reads a csv file, it reads it in as a *data frame*. Thus, when we loaded the `cats`
csv file, it is stored as a data frame. We can recognize data frames by the first row that
is written by the `str()` function:

~~~ {.r}
str(cats2)
~~~
~~~ {.r}
'data.frame':	4 obs. of  3 variables:
 $ coat        : chr  "calico" "black" "tabby" "white"
 $ weight      : chr  "2.1" "5" "3.2" "2.3 or 2.4"
 $ likes_string: int  1 0 1 1
~~~

*Data frames* are composed of rows and columns, where each column has the
same number of rows. Different columns in a data frame can be made up of different
data types (this is what makes them so versatile), but everything in a given
column needs to be the same type (e.g., vector, factor, or list).

Let's explore more about different data structures:


### Vectors and Type Coercion

To better understand this behavior, let's meet another of the data structures:
the *vector*.

~~~ {.r}
my_vector <- vector(length = 3)
my_vector
~~~
~~~ {.output}
[1] FALSE FALSE FALSE
~~~

A vector in R is essentially an ordered list of things, with the special
condition that *everything in the vector must be the same basic data type*. If
you don't choose the datatype, it'll default to `logical`; or, you can declare
an empty vector of whatever type you like.

~~~ {.r}
another_vector <- vector(mode='character', length=3)
another_vector
~~~
~~~ {.output}
[1] "" "" ""
~~~

You can check if something is a vector:

~~~ {.r}
str(another_vector)
~~~
~~~ {.output}
 chr [1:3] "" "" ""
~~~

The somewhat cryptic output from this command indicates the basic data type
found in this vector - in this case `chr`, character; an indication of the
number of things in the vector - actually, the indexes of the vector, in this
case `[1:3]`; and a few examples of what's actually in the vector - in this case
empty character strings. If we similarly do

~~~ {.r}
str(cats$weight)
~~~
~~~ {.output}
 num [1:3] 2.1 5 3.2
~~~

we see that `cats$weight` is a vector, too - *the columns of data we load into R
data.frames are all vectors*, and that's the root of why R forces everything in
a column to be the same basic data type.

#### Coercion by combining vectors

You can also make vectors with explicit contents with the combine function:

~~~ {.r}
combine_vector <- c(2,6,3)
combine_vector
~~~
~~~ {.output}
[1] 2 6 3
~~~

Given what we've learned so far, what do you think the following will produce?

~~~ {.r}
quiz_vector <- c(2,6,'3')
~~~

This is something called *type coercion*, and it is the source of many surprises
and the reason why we need to be aware of the basic data types and how R will
interpret them. When R encounters a mix of types (here double and character) to
be combined into a single vector, it will force them all to be the same
type. Consider:

~~~ {.r}
coercion_vector <- c('a', TRUE)
coercion_vector
~~~
~~~ {.output}
[1] "a"    "TRUE"
~~~

~~~ {.r}
another_coercion_vector <- c(0, TRUE)
another_coercion_vector
~~~
~~~ {.output}
[1] 0 1
~~~


#### The type hierarchy

The coercion rules go: `logical` -> `integer` -> `double` ("`numeric`") ->
`complex` -> `character`, where -> can be read as *are transformed into*. For
example, combining `logical` and `character` transforms the result to
`character`:

~~~ {.r}
c('a', TRUE)
~~~
~~~ {.output}
[1] "a"    "TRUE"
~~~


A quick way to recognize `character` vectors is by the quotes that enclose them
when they are printed.

You can try to force
coercion against this flow using the `as.` functions:

~~~ {.r}
character_vector_example <- c('0','2','4')
character_vector_example
~~~
~~~ {.output}
[1] "0" "2" "4"
~~~

~~~ {.r}
character_coerced_to_double <- as.double(character_vector_example)
character_coerced_to_double
~~~
~~~ {.output}
[1] 0 2 4
~~~

~~~ {.r}
double_coerced_to_logical <- as.logical(character_coerced_to_double)
double_coerced_to_logical
~~~
~~~ {.output}
[1] FALSE  TRUE  TRUE
~~~

As you can see, some surprising things can happen when R forces one basic data
type into another! Nitty-gritty of type coercion aside, the point is: if your
data doesn't look like what you thought it was going to look like, type coercion
may well be to blame; make sure everything is the same type in your vectors and
your columns of data.frames, or you will get nasty surprises!

But coercion can also be very useful! For example, in our `cats` data
`likes_string` is numeric, but we know that the 1s and 0s actually represent
`TRUE` and `FALSE` (a common way of representing them). We should use the
`logical` datatype here, which has two states: `TRUE` or `FALSE`, which is
exactly what our data represents. We can 'coerce' this column to be `logical` by
using the `as.logical` function:

~~~ {.r}
cats$likes_string
~~~
~~~ {.output}
[1] 1 0 1
~~~

~~~ {.r}
cats$likes_string <- as.logical(cats$likes_string)
cats$likes_string
~~~
~~~ {.output}
[1]  TRUE FALSE  TRUE
~~~

### Some basic vector functions

The combine function, `c()`, will also append things to an existing vector:

~~~ {.r}
ab_vector <- c('a', 'b')
ab_vector
~~~
~~~ {.output}
[1] "a" "b"
~~~

~~~ {.r}
combine_example <- c(ab_vector, 'SWC')
combine_example
~~~
~~~ {.output}
[1] "a"   "b"   "SWC"
~~~

You can also make series of numbers:

~~~ {.r}
mySeries <- 1:10
mySeries
~~~
~~~ {.output}
 [1]  1  2  3  4  5  6  7  8  9 10
~~~

~~~ {.r}
seq(10)
~~~
~~~ {.output}
 [1]  1  2  3  4  5  6  7  8  9 10
~~~

~~~ {.r}
seq(1,10, by=0.1)
~~~
~~~ {.output}
 [1]  1.0  1.1  1.2  1.3  1.4  1.5  1.6  1.7  1.8  1.9  2.0  2.1  2.2  2.3  2.4  2.5  2.6  2.7
[19]  2.8  2.9  3.0  3.1  3.2  3.3  3.4  3.5  3.6  3.7  3.8  3.9  4.0  4.1  4.2  4.3  4.4  4.5
[37]  4.6  4.7  4.8  4.9  5.0  5.1  5.2  5.3  5.4  5.5  5.6  5.7  5.8  5.9  6.0  6.1  6.2  6.3
[55]  6.4  6.5  6.6  6.7  6.8  6.9  7.0  7.1  7.2  7.3  7.4  7.5  7.6  7.7  7.8  7.9  8.0  8.1
[73]  8.2  8.3  8.4  8.5  8.6  8.7  8.8  8.9  9.0  9.1  9.2  9.3  9.4  9.5  9.6  9.7  9.8  9.9
[91] 10.0
~~~

We can ask a few questions about vectors:

~~~ {.r}
sequence_example <- 20:25
head(sequence_example, n=2)
~~~
~~~ {.output}
[1] 20 21
~~~

~~~ {.r}
tail(sequence_example, n=4)
~~~
~~~ {.output}
[1] 22 23 24 25
~~~

~~~ {.r}
length(sequence_example)
~~~
~~~ {.output}
[1] 6
~~~

~~~ {.r}
typeof(sequence_example)
~~~
~~~ {.output}
[1] "integer"
~~~

We can get individual elements of a vector by using the bracket notation:

~~~ {.r}
first_element <- sequence_example[1]
first_element
~~~
~~~ {.output}
[1] 20
~~~

To change a single element, use the bracket on the other side of the arrow:

~~~ {.r}
sequence_example[1] <- 30
sequence_example
~~~
~~~ {.output}
[1] 30 21 22 23 24 25
~~~


> ### Challenge 1 {.challenge}
> 
> Start by making a vector with the numbers 1 through 26.
> Then, multiply the vector by 2.
> 
> ###### Solution
> 
> ~~~ {.r}
> x <- 1:26
> x <- x * 2
> ~~~


### Lists

Another data structure you'll want in your bag of tricks is the `list`. A list
is simpler in some ways than the other types, because you can put anything you
want in it. Remember *everything in the vector must be of the same basic data type*,
but a list can have different data types:

~~~ {.r}
list_example <- list(1, "a", TRUE, 1+4i)
list_example
~~~
~~~ {.output}
[[1]]
[1] 1

[[2]]
[1] "a"

[[3]]
[1] TRUE

[[4]]
[1] 1+4i
~~~

When printing the object structure with `str()`, we see the data types of all
elements:

~~~ {.r}
str(list_example)
~~~
~~~ {.output}
List of 4
 $ : num 1
 $ : chr "a"
 $ : logi TRUE
 $ : cplx 1+4i
~~~

What is the use of lists? They can **organize data of different types**. For
example, you can organize different tables that belong together, similar to
spreadsheets in Excel. But there are many other uses, too.

We will see another example that will maybe surprise you in the next chapter.

To retrieve one of the elements of a list, use the **double bracket**:

~~~ {.r}
list_example[[2]]
~~~
~~~ {.output}
[1] "a"
~~~

The elements of lists also can have **names**, they can be given by prepending
them to the values, separated by an equals sign:

~~~ {.r}
another_list <- list(title = "Numbers", numbers = 1:10, data = TRUE )
another_list
~~~
~~~ {.output}
$title
[1] "Numbers"

$numbers
 [1]  1  2  3  4  5  6  7  8  9 10

$data
[1] TRUE
~~~

This results in a **named list**. Now we have a new function of our object!
We can access single elements by an additional way!

~~~ {.r}
another_list$title
~~~
~~~ {.output}
[1] "Numbers"
~~~

## Names

With names, we can give meaning to elements. It is the first time that we do not
only have the **data**, but also explaining information. It is *metadata*
that can be stuck to the object like a label. In R, this is called an
**attribute**. Some attributes enable us to do more with our
object, for example, like here, accessing an element by a self-defined name.

### Accessing vectors and lists by name

We have already seen how to generate a named list. The way to generate a named
vector is very similar. You have seen this function before:

~~~ {.r}
pizza_price <- c( pizzasubito = 5.64, pizzafresh = 6.60, callapizza = 4.50 )
~~~

The way to retrieve elements is different, though:

~~~ {.r}
pizza_price["pizzasubito"]
~~~
~~~ {.output}
pizzasubito
       5.64
~~~

The approach used for the list does not work:

~~~ {.r}
pizza_price$pizzafresh
~~~
~~~ {.error}
Error in pizza_price$pizzafresh : 
  $ operator is invalid for atomic vectors
~~~

It will pay off if you remember this error message, you will meet it in your own
analyses. It means that you have just tried accessing an element like it was in
a list, but it is actually in a vector.

### Accessing and changing names

If you are only interested in the names, use the `names()` function:

~~~ {.r}
names(pizza_price)
~~~
~~~ {.output}
[1] "pizzasubito" "pizzafresh"  "callapizza"
~~~

We have seen how to access and change single elements of a vector. The same is
possible for names:

~~~ {.r}
names(pizza_price)[3]
~~~
names(pizza_price)[3] <- "call-a-pizza"
~~~ {.output}
[1] "callapizza"
~~~

~~~ {.r}
names(pizza_price)[3]
names(pizza_price)[3] <- "call-a-pizza"
pizza_price
~~~
~~~ {.output}
 pizzasubito   pizzafresh call-a-pizza
        5.64         6.60         4.50
~~~


> ### Challenge 2 {.challenge}
> 
> - What is the data type of the names of `pizza_price`? You can find out
>   using the `str()` or `typeof()` functions.
> 
> ###### Solution
> 
> You get the names of an object by wrapping the object name inside
> `names(...)`. Similarly, you get the data type of the names by again
> wrapping the whole code in `typeof(...)`:
> 
> ~~~
> typeof(names(pizza))
> ~~~
> 
> alternatively, use a new variable if this is easier for you to read:
> 
> ~~~
> n <- names(pizza)
> typeof(n)
> ~~~


> ### Challenge 3 {.challenge}
> 
> Instead of just changing some of the names a vector/list already has, you can
> also set all names of an object by writing code like (replace ALL CAPS text):
> 
> ~~~
> names( OBJECT ) <-  CHARACTER_VECTOR
> ~~~
> 
> Create a vector that gives the number for each letter in the alphabet!
> 
> 1. Generate a vector called `letter_no` with the sequence of numbers from 1
>   to 26!
> 2. R has a built-in object called `LETTERS`. It is a 26-character vector, from
>   A to Z. Set the names of the number sequence to this 26 letters
> 3. Test yourself by calling `letter_no["B"]`, which should give you the number
>   2!
> 
> ###### Solution
> 
> ~~~
> letter_no <- 1:26   # or seq(1,26)
> names(letter_no) <- LETTERS
> letter_no["B"]
> ~~~



## Data frames

We have data frames at the very beginning of this lesson, they represent
a table of data. We didn't go much further into detail with our example cat
data frame:

~~~ {.r}
cats
~~~
~~~ {.output}
    coat weight likes_string
1 calico    2.1         TRUE
2  black    5.0        FALSE
3  tabby    3.2         TRUE
~~~

We can now understand something a bit surprising in our data.frame; what happens
if we run:

~~~ {.r}
typeof(cats)
~~~
~~~ {.output}
[1] "list"
~~~

We see that data.frames look like lists 'under the hood'. Think again what we
heard about what lists can be used for:

> Lists organize data of different types

Columns of a data frame are vectors of different types, that are organized
by belonging to the same table.

A data.frame is really a list of vectors. It is a special list in which all the
vectors must have the same length.

How is this "special"-ness written into the object, so that R does not treat it
like any other list, but as a table?

~~~ {.r}
class(cats)
~~~
~~~ {.output}
[1] "data.frame"
~~~

A **class**, just like names, is an attribute attached to the object. It tells
us what this object means for humans.

You might wonder: Why do we need another what-type-of-object-is-this-function?
We already have `typeof()`? That function tells us how the object is
**constructed in the computer**. The `class` is the **meaning of the object for
humans**. Consequently, what `typeof()` returns is *fixed* in R (mainly the
five data types), whereas the output of `class()` is *diverse* and *extendable*
by R packages.

In our `cats` example, we have an integer, a double and a logical variable. As
we have seen already, each column of data.frame is a vector.

~~~ {.r}
cats$coat
~~~
~~~ {.output}
[1] "calico" "black"  "tabby"
~~~

~~~ {.r}
cats[,1]
~~~
~~~ {.output}
[1] "calico" "black"  "tabby"
~~~

~~~ {.r}
typeof(cats[,1])
~~~
~~~ {.output}
[1] "character"
~~~

~~~ {.r}
str(cats[,1])
~~~
~~~ {.output}
 chr [1:3] "calico" "black" "tabby"
~~~
~~~ {.output}
~~~

Each row is an *observation* of different variables, itself a data.frame, and
thus can be composed of elements of different types.

~~~ {.r}
cats[1,]
~~~
~~~ {.output}
    coat weight likes_string
1 calico    2.1         TRUE
~~~
~~~ {.r}
typeof(cats[1,])
~~~
~~~ {.output}
[1] "list"
~~~
~~~ {.r}
str(cats[1,])
~~~
~~~ {.output}
'data.frame':	1 obs. of  3 variables:
 $ coat        : chr "calico"
 $ weight      : num 2.1
 $ likes_string: logi TRUE
~~~

> ### Tip: Renaming data frame columns {.callout}
> 
> Data frames have column names, which can be accessed with the `names()` function.
> 
> ~~~ {.r}
> names(cats)
> ~~~
> 
> If you want to rename the second column of `cats`, you can assign a new name to the second element of `names(cats)`.
> 
> ~~~ {.r}
> names(cats)[2] <- "weight_kg"
> cats
> ~~~


~~~ {.r}
# reverting cats back to original version
cats <- cats_orig
~~~

### Matrices

Last but not least is the matrix. We can declare a matrix full of zeros:

~~~ {.r}
matrix_example <- matrix(0, ncol=6, nrow=3)
matrix_example
~~~
~~~ {.output}
     [,1] [,2] [,3] [,4] [,5] [,6]
[1,]    0    0    0    0    0    0
[2,]    0    0    0    0    0    0
[3,]    0    0    0    0    0    0
~~~

What makes it special is the `dim()` attribute:

~~~ {.r}
dim(matrix_example)
~~~
~~~ {.output}
[1] 3 6
~~~

And similar to other data structures, we can ask things about our matrix:

~~~ {.r}
typeof(matrix_example)
~~~
~~~ {.output}
[1] "double"
~~~

~~~ {.r}
class(matrix_example)
~~~
~~~ {.output}
[1] "matrix" "array"
~~~

~~~ {.r}
str(matrix_example)
~~~
~~~ {.output}
 num [1:3, 1:6] 0 0 0 0 0 0 0 0 0 0 ...
~~~

~~~ {.r}
nrow(matrix_example)
~~~
~~~ {.output}
[1] 3
~~~

~~~ {.r}
ncol(matrix_example)
~~~
~~~ {.output}
[1] 6
~~~

> ### Challenge 4 {.challenge}
> 
> What do you think will be the result of
> `length(matrix_example)`?
> Try it.
> Were you right? Why / why not?
> 
> ###### Solution
> 
> What do you think will be the result of
> `length(matrix_example)`?
> 
> ~~~ {.r}
> matrix_example <- matrix(0, ncol=6, nrow=3)
> length(matrix_example)
> ~~~
> 
> Because a matrix is a vector with added dimension attributes, `length`
> gives you the total number of elements in the matrix.


> ### Challenge 5 {.challenge}
> 
> Make another matrix, this time containing the numbers 1:50,
> with 5 columns and 10 rows.
> Did the `matrix` function fill your matrix by column, or by
> row, as its default behaviour?
> See if you can figure out how to change this.
> (hint: read the documentation for `matrix`!)
> 
> ###### Solution
> 
> Make another matrix, this time containing the numbers 1:50,
> with 5 columns and 10 rows.
> Did the `matrix` function fill your matrix by column, or by
> row, as its default behaviour?
> See if you can figure out how to change this.
> (hint: read the documentation for `matrix`!)
> 
> ~~~ {.r}
> x <- matrix(1:50, ncol=5, nrow=10)
> x <- matrix(1:50, ncol=5, nrow=10, byrow = TRUE) # to fill by row
> ~~~


> ### Challenge 6 {.challenge}
> 
> Create a list of length two containing a character vector for each of the sections in this part of the workshop:
> 
> - Data types
> - Data structures
> 
> Populate each character vector with the names of the data types and data
> structures we've seen so far.
> 
> ###### Solution
> 
> ~~~ {.r}
> dataTypes <- c('double', 'complex', 'integer', 'character', 'logical')
> dataStructures <- c('data.frame', 'vector', 'list', 'matrix')
> answer <- list(dataTypes, dataStructures)
> ~~~
> 
> Note: it's nice to make a list in big writing on the board or taped to the wall
> listing all of these types and structures - leave it up for the rest of the workshop
> to remind people of the importance of these basics.


> ### Challenge 7 {.challenge}
> 
> Consider the R output of the matrix below:
> 
> ~~~ {.r}
> matrix(c(4, 1, 9, 5, 10, 7), ncol = 2, byrow = TRUE)
> ~~~
> 
> What was the correct command used to write this matrix? Examine
> each command and try to figure out the correct one before typing them.
> Think about what matrices the other commands will produce.
> 
> 1. `matrix(c(4, 1, 9, 5, 10, 7), nrow = 3)`
> 2. `matrix(c(4, 9, 10, 1, 5, 7), ncol = 2, byrow = TRUE)`
> 3. `matrix(c(4, 9, 10, 1, 5, 7), nrow = 2)`
> 4. `matrix(c(4, 1, 9, 5, 10, 7), ncol = 2, byrow = TRUE)`
> 
> ###### Solution
> 
> Consider the R output of the matrix below:
> 
> ~~~ {.r}
> matrix(c(4, 1, 9, 5, 10, 7), ncol = 2, byrow = TRUE)
> ~~~
> 
> What was the correct command used to write this matrix? Examine
> each command and try to figure out the correct one before typing them.
> Think about what matrices the other commands will produce.
> 
> ~~~ {.r}
> matrix(c(4, 1, 9, 5, 10, 7), ncol = 2, byrow = TRUE)
> ~~~


> ## Keypoints {.objectives}
> 
> * Use `read.csv` to read tabular data in R.
> * The basic data types in R are double, integer, complex, logical, and character.
> * Data structures such as data frames or matrices are built on top of lists and vectors, with some added attributes.
> 


