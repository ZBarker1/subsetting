---
title: "Subsetting Practice"
author: "Ziqi Liu"
date: "May 16, 2018"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
    toc_depth: 2
editor_options:
  chunk_output_type: console
---

# 1.0 Subsetting
- 3 subsetting operators
- 6 types of subsetting

# 2.0 Selecting multiple elements
- use [] operator

## 2.1 Atomic vectors
- positive integers return elements at specified positions

```r
x <- c(2.1, 4.2, 3.3, 5.4)
x[c(3,1)]
```

```
## [1] 3.3 2.1
```

```r
x[order(x)]
```

```
## [1] 2.1 3.3 4.2 5.4
```

- duplicated indices yield duplicated values

```r
x[c(1, 1)]
```

```
## [1] 2.1 2.1
```

- real numbers are truncated to integers (round down)

```r
x[c(2.1, 2.9)]
```

```
## [1] 4.2 4.2
```

- negative integers omit elements at the specified positions

```r
x[-c(3, 1)]
```

```
## [1] 4.2 5.4
```

- you can't mix positive and negative integers in a single subset

```r
# x[c(-1, 2)]
```

- logical vectors select elements where the corresponding logical value is TRUE

```r
x[c(T, T, F, F)]
```

```
## [1] 2.1 4.2
```

```r
x[x>3]
```

```
## [1] 4.2 3.3 5.4
```

- if the logical vector is shorter than the vector being subsetted, it will be recycled to be the same length

```r
x[c(T, F)]
```

```
## [1] 2.1 3.3
```

```r
#same as
x[c(T, F, T, F)]
```

```
## [1] 2.1 3.3
```

- a missing value in the index yields a missing value in the output

```r
x[c(T, T, NA, F)]
```

```
## [1] 2.1 4.2  NA
```

- nothing returns the original vector/matrix/df/array

```r
x[]
```

```
## [1] 2.1 4.2 3.3 5.4
```

- if a vector is named you can also use character vectors to return elements with matching names (must be exact match)

```r
y <- setNames(x, letters[1:4])
y
```

```
##   a   b   c   d 
## 2.1 4.2 3.3 5.4
```

```r
y[c("d","c","a")]
```

```
##   d   c   a 
## 5.4 3.3 2.1
```

```r
#like integer indices you can repeat them
y[c("a","a","a")]
```

```
##   a   a   a 
## 2.1 2.1 2.1
```

## 2.2 Lists
- using [[]] and $ lets you pull out components of a list
- you can subset higher-dimensional structures with multiple vectors, a single vector, or a matrix

```r
a <- matrix(1:9, nrow = 3)
colnames(a) <- c("A", "B", "C")
a
```

```
##      A B C
## [1,] 1 4 7
## [2,] 2 5 8
## [3,] 3 6 9
```

```r
a[c(TRUE, FALSE, TRUE), c("B", "A")]
```

```
##      B A
## [1,] 4 1
## [2,] 6 3
```

```r
a[0, -2]
```

```
##      A C
```

## 2.3 Data frames
- dfs possess characteristics of both lists and matrices, if you subset with a single vector they behave like lists, if you subset with 2 vectors they behave like matrices

```r
df <- data.frame(x=1:3, y=3:1, z=letters[1:3])
df
```

```
##   x y z
## 1 1 3 a
## 2 2 2 b
## 3 3 1 c
```


```r
# to select rows
df[df$x == 2, ]
```

```
##   x y z
## 2 2 2 b
```

```r
df[c(1, 3),]
```

```
##   x y z
## 1 1 3 a
## 3 3 1 c
```

```r
# to select columns
# 1. like a list
df[c("x", "z")]
```

```
##   x z
## 1 1 a
## 2 2 b
## 3 3 c
```

```r
# 2. like a matrix
df[, c("x", "z")]
```

```
##   x z
## 1 1 a
## 2 2 b
## 3 3 c
```

## 2.4 Preserving dimensionality
- subsetting 2d data structures with a single number/char/logical vector will simplify the returned output 
- to preserve original dimensionality, use drop=F

```r
# for matrices and arrays, any dimensions with len=1 will be dropped
a <- matrix(1:4, nrow=2)

str(a[1, ]) #1D
```

```
##  int [1:2] 1 3
```

```r
str(a[1, , drop=F]) #2D
```

```
##  int [1, 1:2] 1 3
```

```r
# data frames with a single column will return just that column
df <- data.frame(a=1:2, b=1:2)

str(df[, "a"])
```

```
##  int [1:2] 1 2
```

```r
str(df[, "a", drop=F])
```

```
## 'data.frame':	2 obs. of  1 variable:
##  $ a: int  1 2
```

- ALWAYS ADD drop=F JUST IN CASE!!

## 2.5 Exercises

1)

```r
# mtcars[mtcars$cyl=4, ] will error need to use equality operator ==

mtcars[mtcars$cyl==4, ]
```

```
##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Datsun 710     22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Merc 240D      24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230       22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Fiat 128       32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic    30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona  21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Fiat X1-9      27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Volvo 142E     21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

2)

```r
# mtcars[-1:4, ] will error because you can't index over a negative and positive integer

mtcars[2:4, ] # this will drop the first row, and then go to 4
```

```
##                 mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4 Wag  21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710     22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
```

```r
mtcars[c(ncol(mtcars), 1:4), ] # this will get the last row and the first 4
```

```
##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Merc 280C      17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Mazda RX4      21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag  21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710     22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive 21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
```

3)

```r
# mtcars[mtcars$cyl <= 5] will error because we haven't specified rows or coluns

mtcars[mtcars$cyl <=5, ]
```

```
##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Datsun 710     22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Merc 240D      24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230       22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Fiat 128       32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic    30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona  21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Fiat X1-9      27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Volvo 142E     21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

4)

```r
# mtcars[mtcars$cyl == 4 | 6, ] 

mtcars[mtcars$cyl == 4 | mtcars$cyl == 6, ]
```

```
##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4      21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag  21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710     22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive 21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Valiant        18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Merc 240D      24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230       22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 280       19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Merc 280C      17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Fiat 128       32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic    30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona  21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Fiat X1-9      27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Ferrari Dino   19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
## Volvo 142E     21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

5) 
why does x <- 1:5; x[NA] yield five missing values? (Hint: why is it different from x[NA_real_]?)

6)
What does upper.tri() return? How does subsetting a matrix with it work? Do we need any additional subsetting rules to describe its behaviour?

```r
x <- outer(1:5, 1:5, FUN = "*") ; x
```

```
##      [,1] [,2] [,3] [,4] [,5]
## [1,]    1    2    3    4    5
## [2,]    2    4    6    8   10
## [3,]    3    6    9   12   15
## [4,]    4    8   12   16   20
## [5,]    5   10   15   20   25
```

```r
# this creates a 5 by 5 matrix where each value is its row*col

upper.tri(x)
```

```
##       [,1]  [,2]  [,3]  [,4]  [,5]
## [1,] FALSE  TRUE  TRUE  TRUE  TRUE
## [2,] FALSE FALSE  TRUE  TRUE  TRUE
## [3,] FALSE FALSE FALSE  TRUE  TRUE
## [4,] FALSE FALSE FALSE FALSE  TRUE
## [5,] FALSE FALSE FALSE FALSE FALSE
```

```r
# creates a matrix of T or F where T is the upper triangle of values

x[upper.tri(x)]
```

```
##  [1]  2  3  6  4  8 12  5 10 15 20
```

```r
# this subsets x with only the values that were true, so values in the upper triangle
```

7. Why does mtcars[1:20] return an error? How does it differ from the similar mtcars[1:20, ]?

```r
# mtcars[1:20] doesn't work because we didn't specify rows or cols
mtcars[1:20, ] #returns rows 1 to 20
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
```

8. Implement your own function that extracts the diagonal entries from a matrix (it should behave like diag(x) where x is a matrix).

```r
x <- matrix(LETTERS[1:25], ncol=5) ; x
```

```
##      [,1] [,2] [,3] [,4] [,5]
## [1,] "A"  "F"  "K"  "P"  "U" 
## [2,] "B"  "G"  "L"  "Q"  "V" 
## [3,] "C"  "H"  "M"  "R"  "W" 
## [4,] "D"  "I"  "N"  "S"  "X" 
## [5,] "E"  "J"  "O"  "T"  "Y"
```

```r
diag(x)
```

```
## [1] "A" "G" "M" "S" "Y"
```

9. What does df[is.na(df)] <- 0 do? How does it work?

```r
df <- data.frame("memes" = letters[1:9], "dreams" = 7:15, "123" = c(1, 69, NA, 23, 61, 2, 36, NA, 3045069)) ; df
```

```
##   memes dreams    X123
## 1     a      7       1
## 2     b      8      69
## 3     c      9      NA
## 4     d     10      23
## 5     e     11      61
## 6     f     12       2
## 7     g     13      36
## 8     h     14      NA
## 9     i     15 3045069
```

```r
# creates df with some NA

is.na(df)
```

```
##       memes dreams  X123
##  [1,] FALSE  FALSE FALSE
##  [2,] FALSE  FALSE FALSE
##  [3,] FALSE  FALSE  TRUE
##  [4,] FALSE  FALSE FALSE
##  [5,] FALSE  FALSE FALSE
##  [6,] FALSE  FALSE FALSE
##  [7,] FALSE  FALSE FALSE
##  [8,] FALSE  FALSE  TRUE
##  [9,] FALSE  FALSE FALSE
```

```r
# creates a matrix of logicals where T is NA

df[is.na(df)] <- 0 ; df
```

```
##   memes dreams    X123
## 1     a      7       1
## 2     b      8      69
## 3     c      9       0
## 4     d     10      23
## 5     e     11      61
## 6     f     12       2
## 7     g     13      36
## 8     h     14       0
## 9     i     15 3045069
```

```r
# subsets df looking for places where the value is NA and then turns that to 0
```

# 3.0 Selecting a single element
- use $ or [[]]
- [[]] is like a nested list

```r
mtcars[1] #returns the first column with all the rownames
```

```
##                      mpg
## Mazda RX4           21.0
## Mazda RX4 Wag       21.0
## Datsun 710          22.8
## Hornet 4 Drive      21.4
## Hornet Sportabout   18.7
## Valiant             18.1
## Duster 360          14.3
## Merc 240D           24.4
## Merc 230            22.8
## Merc 280            19.2
## Merc 280C           17.8
## Merc 450SE          16.4
## Merc 450SL          17.3
## Merc 450SLC         15.2
## Cadillac Fleetwood  10.4
## Lincoln Continental 10.4
## Chrysler Imperial   14.7
## Fiat 128            32.4
## Honda Civic         30.4
## Toyota Corolla      33.9
## Toyota Corona       21.5
## Dodge Challenger    15.5
## AMC Javelin         15.2
## Camaro Z28          13.3
## Pontiac Firebird    19.2
## Fiat X1-9           27.3
## Porsche 914-2       26.0
## Lotus Europa        30.4
## Ford Pantera L      15.8
## Ferrari Dino        19.7
## Maserati Bora       15.0
## Volvo 142E          21.4
```

```r
mtcars[[1]] #returns all the values of the first column
```

```
##  [1] 21.0 21.0 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 17.8 16.4 17.3 15.2
## [15] 10.4 10.4 14.7 32.4 30.4 33.9 21.5 15.5 15.2 13.3 19.2 27.3 26.0 30.4
## [29] 15.8 19.7 15.0 21.4
```

# 3.1 $
- x$y is ~= x[["y"]]
- where x is the tibble/df/matrix name and y is the name of a variable(column) in it

# 3.2 Exercises
1)
Come up with as many way as possible to extract the third value from the cyl variable in the mtcars dataset.


```r
mtcars[[2]][3]
```

```
## [1] 4
```

```r
mtcars$cyl[3]
```

```
## [1] 4
```

# 4.0 Subsetting and assignment
- all subsetting operators can be combined with assignment operators to modify selected values of input vector

```r
x <- 1:5 ; x[c(1, 2)] <- 2:3 ; x
```

```
## [1] 2 3 3 4 5
```

```r
x[-1] <- 4:1 ; x
```

```
## [1] 2 4 3 2 1
```

```r
# length(LHS) needs to = length(RHS) for assignment to work

x[c(1, 1)] <- 2:3 ; x
```

```
## [1] 3 4 3 2 1
```

```r
# duplicated indices go unchecked and may be problematic

# you can't combine integer indices with NA but you can combine logical indices with NA
```

- subsetting with nothing and then assignment preserves original object class and structure

```r
mtcars[] <- lapply(mtcars, as.integer) #mtcars stays as df
mtcars <- lapply(mtcars, as.integer) #mtcars becomes list
```

# 5.0 Applications

## 5.1 Lookup tables

```r
x <- c("m", "f", "u", "f", "f", "m", "m")
lookup <- c(m = "Male", f = "Female", u = NA)
lookup[x]
```

```
##        m        f        u        f        f        m        m 
##   "Male" "Female"       NA "Female" "Female"   "Male"   "Male"
```

```r
unname(lookup[x])
```

```
## [1] "Male"   "Female" NA       "Female" "Female" "Male"   "Male"
```

## 5.2 Matching or merging by hand (integer subsetting)


```r
grades <- c(1, 2, 2, 3, 1)
info <- data.frame(
  grade = 3:1,
  desc = c("Excellent", "Good", "Poor"),
  fail = c(F, F, T)
)
id <- match(grades, info$grade)
info[id, ]
```

```
##     grade      desc  fail
## 3       1      Poor  TRUE
## 2       2      Good FALSE
## 2.1     2      Good FALSE
## 1       3 Excellent FALSE
## 3.1     1      Poor  TRUE
```

## 5.3 Random samples/bootstrap (integer subsetting)

- you can use integer indices to perform random sampling or bootstrapping

```r
df <- data.frame(x = rep(1:3, each = 2), y = 6:1, z = letters[1:6]) ; df
```

```
##   x y z
## 1 1 6 a
## 2 1 5 b
## 3 2 4 c
## 4 2 3 d
## 5 3 2 e
## 6 3 1 f
```

```r
df[sample(nrow(df)), ] #reorder rows
```

```
##   x y z
## 5 3 2 e
## 6 3 1 f
## 1 1 6 a
## 3 2 4 c
## 2 1 5 b
## 4 2 3 d
```

```r
df[sample(nrow(df), 3), ] #select 3 random rows
```

```
##   x y z
## 3 2 4 c
## 5 3 2 e
## 2 1 5 b
```

## 5.4 Ordering (integer subsetting)

- order() takes a vector as input and returns an integer vector describing how the vector should be ordered

```r
x <- c("b", "c", "a")
order(x)
```

```
## [1] 3 1 2
```

```r
x[order(x)]
```

```
## [1] "a" "b" "c"
```

- you can order in desc order by using decreasing = T

```r
df
```

```
##   x y z
## 1 1 6 a
## 2 1 5 b
## 3 2 4 c
## 4 2 3 d
## 5 3 2 e
## 6 3 1 f
```

```r
df2 <- df[sample(nrow(df)), 3:1] #randomly reorder df rows, invert columns
df2[order(df2$x), ] #order values of col x
```

```
##   z y x
## 1 a 6 1
## 2 b 5 1
## 4 d 3 2
## 3 c 4 2
## 6 f 1 3
## 5 e 2 3
```

```r
df2[, order(names(df2))] #order by col names
```

```
##   x y z
## 4 2 3 d
## 6 3 1 f
## 3 2 4 c
## 1 1 6 a
## 5 3 2 e
## 2 1 5 b
```

## 5.5 Expanding aggregated counts (integer subsetting)

- sometimes you get a data frame where identical rows have been collapsed into one and a count column has been added
- rep() and integer subsetting make it easy to uncollapse the data by subsetting with a repeated row index

```r
df <- data.frame(x=c(2,4,1), y=c(9,11,6), n=c(3,5,1)) ; df
```

```
##   x  y n
## 1 2  9 3
## 2 4 11 5
## 3 1  6 1
```

```r
rep(1:nrow(df), df$n)
```

```
## [1] 1 1 1 2 2 2 2 2 3
```

```r
df[rep(1:nrow(df), df$n), ]
```

```
##     x  y n
## 1   2  9 3
## 1.1 2  9 3
## 1.2 2  9 3
## 2   4 11 5
## 2.1 4 11 5
## 2.2 4 11 5
## 2.3 4 11 5
## 2.4 4 11 5
## 3   1  6 1
```

## 5.6 Removing columns from data frames (character subsetting)

- 2 ways to remove columns from df
- you can set columns to NULL

```r
mtcars$cyl <- NULL ; head(mtcars)
```

```
## $mpg
##  [1] 21 21 22 21 18 18 14 24 22 19 17 16 17 15 10 10 14 32 30 33 21 15 15
## [24] 13 19 27 26 30 15 19 15 21
## 
## $disp
##  [1] 160 160 108 258 360 225 360 146 140 167 167 275 275 275 472 460 440
## [18]  78  75  71 120 318 304 350 400  79 120  95 351 145 301 121
## 
## $hp
##  [1] 110 110  93 110 175 105 245  62  95 123 123 180 180 180 205 215 230
## [18]  66  52  65  97 150 150 245 175  66  91 113 264 175 335 109
## 
## $drat
##  [1] 3 3 3 3 3 2 3 3 3 3 3 3 3 3 2 3 3 4 4 4 3 2 3 3 3 4 4 3 4 3 3 4
## 
## $wt
##  [1] 2 2 2 3 3 3 3 3 3 3 3 4 3 3 5 5 5 2 1 1 2 3 3 3 3 1 2 1 3 2 3 2
## 
## $qsec
##  [1] 16 17 18 19 17 20 15 20 22 18 18 17 17 18 17 17 17 19 18 19 20 16 17
## [24] 15 17 18 16 16 14 15 14 18
```

- or you can subset to return only the columns you want

```r
rm(mtcars)
mtcars[-2]
```

```
##                      mpg  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4           21.0 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710          22.8 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive      21.4 258.0 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout   18.7 360.0 175 3.15 3.440 17.02  0  0    3    2
## Valiant             18.1 225.0 105 2.76 3.460 20.22  1  0    3    1
## Duster 360          14.3 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 240D           24.4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230            22.8 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 280            19.2 167.6 123 3.92 3.440 18.30  1  0    4    4
## Merc 280C           17.8 167.6 123 3.92 3.440 18.90  1  0    4    4
## Merc 450SE          16.4 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 450SLC         15.2 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7 440.0 230 3.23 5.345 17.42  0  0    3    4
## Fiat 128            32.4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic         30.4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla      33.9  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona       21.5 120.1  97 3.70 2.465 20.01  1  0    3    1
## Dodge Challenger    15.5 318.0 150 2.76 3.520 16.87  0  0    3    2
## AMC Javelin         15.2 304.0 150 3.15 3.435 17.30  0  0    3    2
## Camaro Z28          13.3 350.0 245 3.73 3.840 15.41  0  0    3    4
## Pontiac Firebird    19.2 400.0 175 3.08 3.845 17.05  0  0    3    2
## Fiat X1-9           27.3  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2       26.0 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa        30.4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Ford Pantera L      15.8 351.0 264 4.22 3.170 14.50  0  1    5    4
## Ferrari Dino        19.7 145.0 175 3.62 2.770 15.50  0  1    5    6
## Maserati Bora       15.0 301.0 335 3.54 3.570 14.60  0  1    5    8
## Volvo 142E          21.4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

## 5.7 Selecting rows based on a condition (logical subsetting)


```r
mtcars[mtcars$gear == 5, ]
```

```
##                 mpg cyl  disp  hp drat    wt qsec vs am gear carb
## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.7  0  1    5    2
## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.9  1  1    5    2
## Ford Pantera L 15.8   8 351.0 264 4.22 3.170 14.5  0  1    5    4
## Ferrari Dino   19.7   6 145.0 175 3.62 2.770 15.5  0  1    5    6
## Maserati Bora  15.0   8 301.0 335 3.54 3.570 14.6  0  1    5    8
```

```r
mtcars[mtcars$gear == 5 & mtcars$cyl == 4, ]
```

```
##                mpg cyl  disp  hp drat    wt qsec vs am gear carb
## Porsche 914-2 26.0   4 120.3  91 4.43 2.140 16.7  0  1    5    2
## Lotus Europa  30.4   4  95.1 113 3.77 1.513 16.9  1  1    5    2
```

```r
# subset function also exists! these are equivalent to:

subset(mtcars, gear ==5)
```

```
##                 mpg cyl  disp  hp drat    wt qsec vs am gear carb
## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.7  0  1    5    2
## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.9  1  1    5    2
## Ford Pantera L 15.8   8 351.0 264 4.22 3.170 14.5  0  1    5    4
## Ferrari Dino   19.7   6 145.0 175 3.62 2.770 15.5  0  1    5    6
## Maserati Bora  15.0   8 301.0 335 3.54 3.570 14.6  0  1    5    8
```

```r
subset(mtcars, gear == 5 & cyl == 4)
```

```
##                mpg cyl  disp  hp drat    wt qsec vs am gear carb
## Porsche 914-2 26.0   4 120.3  91 4.43 2.140 16.7  0  1    5    2
## Lotus Europa  30.4   4  95.1 113 3.77 1.513 16.9  1  1    5    2
```

## 5.8 Boolean algebra vs sets (logical & integer subsetting)

- which() allows you to convert a boolean representation to an integer representation

```r
x <- sample(10) < 4 ; x
```

```
##  [1]  TRUE FALSE FALSE FALSE  TRUE  TRUE FALSE FALSE FALSE FALSE
```

```r
which(x)
```

```
## [1] 1 5 6
```

```r
(x1 <- 1:10 %% 2 == 0)
```

```
##  [1] FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE
```

```r
(y1 <- 1:10 %% 5 == 0)
```

```
##  [1] FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE  TRUE
```

```r
(x2 <- which(x1))
```

```
## [1]  2  4  6  8 10
```

```r
(y2 <- which(y1))
```

```
## [1]  5 10
```

```r
# X & Y <-> intersect(x,y)
x1 & y1
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE
```

```r
intersect(x2, y2)
```

```
## [1] 10
```

```r
# X | Y <-> union(x,y)
x1 | y1
```

```
##  [1] FALSE  TRUE FALSE  TRUE  TRUE  TRUE FALSE  TRUE FALSE  TRUE
```

```r
union(x2, y2)
```

```
## [1]  2  4  6  8 10  5
```

```r
# X & !Y <-> setdiff(x,y)
x1 & !y1
```

```
##  [1] FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE FALSE
```

```r
setdiff(x2, y2)
```

```
## [1] 2 4 6 8
```

## 5.9 Exercises
3)
How could you put the columns in a data frame in alphabetical order?

```r
mtcars[ , order(names(mtcars))]
```

```
##                     am carb cyl  disp drat gear  hp  mpg  qsec vs    wt
## Mazda RX4            1    4   6 160.0 3.90    4 110 21.0 16.46  0 2.620
## Mazda RX4 Wag        1    4   6 160.0 3.90    4 110 21.0 17.02  0 2.875
## Datsun 710           1    1   4 108.0 3.85    4  93 22.8 18.61  1 2.320
## Hornet 4 Drive       0    1   6 258.0 3.08    3 110 21.4 19.44  1 3.215
## Hornet Sportabout    0    2   8 360.0 3.15    3 175 18.7 17.02  0 3.440
## Valiant              0    1   6 225.0 2.76    3 105 18.1 20.22  1 3.460
## Duster 360           0    4   8 360.0 3.21    3 245 14.3 15.84  0 3.570
## Merc 240D            0    2   4 146.7 3.69    4  62 24.4 20.00  1 3.190
## Merc 230             0    2   4 140.8 3.92    4  95 22.8 22.90  1 3.150
## Merc 280             0    4   6 167.6 3.92    4 123 19.2 18.30  1 3.440
## Merc 280C            0    4   6 167.6 3.92    4 123 17.8 18.90  1 3.440
## Merc 450SE           0    3   8 275.8 3.07    3 180 16.4 17.40  0 4.070
## Merc 450SL           0    3   8 275.8 3.07    3 180 17.3 17.60  0 3.730
## Merc 450SLC          0    3   8 275.8 3.07    3 180 15.2 18.00  0 3.780
## Cadillac Fleetwood   0    4   8 472.0 2.93    3 205 10.4 17.98  0 5.250
## Lincoln Continental  0    4   8 460.0 3.00    3 215 10.4 17.82  0 5.424
## Chrysler Imperial    0    4   8 440.0 3.23    3 230 14.7 17.42  0 5.345
## Fiat 128             1    1   4  78.7 4.08    4  66 32.4 19.47  1 2.200
## Honda Civic          1    2   4  75.7 4.93    4  52 30.4 18.52  1 1.615
## Toyota Corolla       1    1   4  71.1 4.22    4  65 33.9 19.90  1 1.835
## Toyota Corona        0    1   4 120.1 3.70    3  97 21.5 20.01  1 2.465
## Dodge Challenger     0    2   8 318.0 2.76    3 150 15.5 16.87  0 3.520
## AMC Javelin          0    2   8 304.0 3.15    3 150 15.2 17.30  0 3.435
## Camaro Z28           0    4   8 350.0 3.73    3 245 13.3 15.41  0 3.840
## Pontiac Firebird     0    2   8 400.0 3.08    3 175 19.2 17.05  0 3.845
## Fiat X1-9            1    1   4  79.0 4.08    4  66 27.3 18.90  1 1.935
## Porsche 914-2        1    2   4 120.3 4.43    5  91 26.0 16.70  0 2.140
## Lotus Europa         1    2   4  95.1 3.77    5 113 30.4 16.90  1 1.513
## Ford Pantera L       1    4   8 351.0 4.22    5 264 15.8 14.50  0 3.170
## Ferrari Dino         1    6   6 145.0 3.62    5 175 19.7 15.50  0 2.770
## Maserati Bora        1    8   8 301.0 3.54    5 335 15.0 14.60  0 3.570
## Volvo 142E           1    2   4 121.0 4.11    4 109 21.4 18.60  1 2.780
```



