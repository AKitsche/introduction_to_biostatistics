Introduction to R (2nd part)
================
Andreas Kitsche

Data input
----------

Data sets frequently consist of more than one column of data, where each column represents measurements of a single variable. Each row usually represents measurements of a single subject. You can find a manual "R Data Import/Export" on <http://www.r-project.org/> for more details on data input.

### Data sets from .txt files

The function `read.table()` is the most convenient way to read in a rectangular grid of data. Using the primary interface to the help systems allows us to get more informations about this function:

``` chunk1
help(read.table) #equals ?read.table
```

We can import the `maize.txt` file from `https://github.com/AKitsche/introduction_to_biostatistics` in the following way

``` chunk2
maizeTXT <- read.table(file="maize.txt", 
                       header=TRUE, 
                       sep="", 
                       dec=".")
head(maizeTXT) #head() returns the first parts of a data frame
```

A common field separator to use in files is a comma. Such files are known as CSV (comma separated values) files. Convenience functions `read.csv` and `read.delim` provide arguments appropriate for CSV and tab-delimited files exported from spreadsheets in english-speaking locales. We can import the file `maize.csv` from `https://github.com/AKitsche/introduction_to_biostatistics` with the commmand

``` chunk3
maizeCSV1 <- read.csv(file="maize.csv", 
                      header=TRUE, 
                      sep = ";" ,
                      dec = ".")
maizeCSV2 <- read.delim(file="maize.csv", 
                        header=TRUE, 
                        sep = ";",
                        dec = ".")
```

Note that R is a case sensitive expression language, so `A` and `a` are different symbols. Most errors occur on erroneous typeseetings, i.e.

``` chunk6
maize
```

Another common mistake often occurs on setting the brackets. Forgetting a comma is also an often performed failure. Most mistakes can be fixed by reading distinctly the error messages!

Some general remarks:
After importing or loading a data set into R you can get an overview using some summary functions:

``` chunk9
head(maizeTXT)
names(maizeTXT)
dim(maizeTXT)
str(maizeTXT)
summary(maizeTXT)
```

Per default the ordering of factor variables in a data frame in alphanumerically. You can get the factor levels with the `levels()` function:

``` chunk10
levels(maizeTXT$hybrid)
```

You can reorder the level of the factor with the `levels` argument in the `factor()` function:

``` chunk11
maizeTXT$hybrid <- factor(maizeTXT$hybrid, levels=c("P3732", "P3747", "LH74", "Mol17", "A632"))
levels(maizeTXT$hybrid)
```

When we want to find all rows where the yield is greater than 155 we can compute:

``` chunk12
maizeTXT$yield > 155
maizeTXT[maizeTXT$yield > 155,]
which(maizeTXT$yield > 155)
maizeTXT[which(maizeTXT$yield > 155),]
```

This situation is so common that R provides the `subset()` function which accepts a data frame, matrix or vector, and a logical expression as its first two arguments, and which returns a similar object containing only those elements that meet the condition of the logical expression. So `subset()` could be used for the previous example as follows:

``` chunk13
subset(maizeTXT, yield>155)
```

A further convenience is offered by the `select=` argument which will extract only the specified columns from the data frame passed as the first argument. The argument to `select=` is a vector of integers or variable names which correspond to the columns that are to be extracted.

``` chunk14
subset(maizeTXT, yield>155, select=c(yield, hybrid))
```

R also provides a wide array of functions to aid in aggregating data. A very common task is to operate on subsets of data based on grouping of variables. The function `aggregate()` is the most likely choice is this cases. Assume you want to calculate the mean of yield for each hybrid from the `maizeTXT` data frame:

``` chunk15
help(aggregate)#getting the help page
aggregate(x=maizeTXT$yield, by=list(maizeTXT$hybrid), FUN=mean)
#method for class data.frame
aggregate(yield ~ hybrid,FUN=mean, data=maizeTXT)
#formula notation
```

It is also possible to calculate the mean yield for each factor combination (hybrid x nitrogen):

``` chunk16
aggregate(x=maizeTXT$yield, by=list(maizeTXT$hybrid, maizeTXT$nitrogen), FUN=mean)
#method for class data.frame
aggregate(yield ~ hybrid+nitrogen,FUN=mean, data=maizeTXT)
#formula notation
```
