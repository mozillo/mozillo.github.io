---
layout: post
title:  "R lang初步学习记录(1)"
date:   2017-11-08 22:21:49
categories: java
tags: R lang
---
#### R的类型

{% highlight R %}

num <- 10
class(num)
# [1] "numeric"
int <- 10L
class(int)
# [1] "integer"
string <- "My first R code"
class(string)
# [1] "character"
complex <- 3+2i
class(complex)
# [1] "complex"
boolean <- TRUE
class(boolean)
# [1] "logical"


# 创建向量vector
x <- vector("numeric", length=4)
# 创建向量vector 方法1
x1 <- 1:4
# 创建向量vector 方法2
x2 <- c(1L,2L,3L,4L)
# 创建向量vector 方法3
x3 <- c("a", "b", "c", "d")

#矩阵类型统一
c = matrix(c("a", 1L, "b", TRUE), nrow = 2, ncol = 2)
# Output
#      [,1] [,2]  
# [1,] "a"  "b"   
# [2,] "1"  "TRUE"
c1 = matrix(c(1, 1L, 2L, TRUE), nrow = 2, ncol = 2)
c1
# Output
#         [,1] [,2]
# [1,]    1    2
# [2,]    1    1

c1 = matrix(c(1, TRUE, 2.5, 3.2, TRUE, 20L, 5.0, 6, 7), nrow = 3, ncol = 3)
c1
# Output
#       [,1] [,2]  [,3]
# [1,]  1.0  3.2   5
# [2,]  1.0  1.0   6
# [3,]  2.5  20.0  7
# 按列转化: 整数(非L)有精度的会 同一列变成浮点数
# 转化优先级: 实数 > 浮点数 > 整数(L) > 字符串 > Logical

class(x)
#[1] "numeric"
class(x1)
#[1] "integer"
class(x2)
#[1] "integer"
class(x3)
#[1] "character"

m <- matrix(1:256, nrow=16, ncol=16)
# 创建16 x 16的矩阵
{% endhighlight %}
***matrix 16x16如图所示***
![matrix 16x16]({{ "/assets/2017-11-30/shot4.png" | absolute_url }})


{% highlight R %}
class(x1)
names(x1) <- c("a", "b", "c", "d")

x <- matrix(1:6, nrow = 2, ncol = 3)
dim(x)
attributes(x)

y <- 6:1
dim(y) <- c(2,3)
cbind(x, y)

x5 <- array(1:24, c(4,6))
x5

l <- list("a", 2, 10L, TRUE, 3+5i)
l2 <- list(a=1, b=2, c=3)
names(l2)

# l <- list("a", 2, 10L, 3+4i, TRUE)
# l2 <- list(a=1, b=2, c=3)
# l3 <- list(c(1,2,3), c(4,5,6,7))
# x <- matrix(1:6, nrow = 2, ncol = 3)
# dimnames(x) <- list(c("a", "b"), c("c", "d", "e"))
x <- character(26)

a = NaN
b=  NA

a1 <- c(1, NA, 2, NA, 3)
is.na(a)
is.nan(a)
# b

df <- data.frame(id = c(1,2,3,4), name = c("Mot","Michael","Sunny", "Yu"), gender = c(TRUE, TRUE, FALSE, TRUE))
df
data.matrix(df)
{% endhighlight %}

![My helpful screenshot]({{ "/assets/2017-11-30/shot1.png" | absolute_url }})

{% highlight R %}
now <- date()
# class(now) : character
now2 <- Sys.Date()
# class(now2) : Date
now3 <- as.Date("2014-11-30")
#class(now3) : Date

weekdays(now3)
# Output:
# 星期四
quarters(now3)
# Output:
# Q4
julian(now3)
# Output:
# [1] 16404
# attr(,"origin")
# [1] "1970-01-01"
now2 - now3
# Output:
# Time difference of 1096 days
{% endhighlight %}

---

**POSIXct与POSIXlt的区别**

>Well, the functions do different things. <br/>
First, there are two internal implementations of date/time: POSIXct, which stores seconds since UNIX epoch (+some other data), and POSIXlt, which stores a list of day, month, year, hour, minute, second, etc.
strptime is a function to directly convert character vectors (of a variety of formats) to POSIXlt format.
as.POSIXlt converts a variety of data types to POSIXlt. It tries to be intelligent and do the sensible thing - in the case of character, it acts as a wrapper to strptime.
as.POSIXct converts a variety of data types to POSIXct. It also tries to be intelligent and do the sensible thing - in the case of character, it runs strptime first, then does the conversion from POSIXlt to POSIXct.
It makes sense that strptime is faster, because strptime only handles character input whilst the others try to determine which method to use from input type. It should also be a bit safer in that being handed unexpected data would just give an error, instead of trying to do the intelligent thing that might not be what you want.

![My helpful screenshot]({{ "/assets/2017-11-30/shot3.png" | absolute_url }})
