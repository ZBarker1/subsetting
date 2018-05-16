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


