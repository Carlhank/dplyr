dplyr
=====

### dplyr main verb(function)

+ filter    — 選取一個資料框架列(row)的子集。
+ arrange   — 對資料框架中變量的內容進行重新排序。
+ select    — 選取需要資料框架中所需的變量(columns)。
+ mutate    — 添加變數(columns)到已存在的資料框架中。
+ summarize — summarize values.
+ group_by  — 描述如何將資料框架分割成幾個群組。

※ chain operator：`%.%`, 結合(combine)上述函數以快速方便操作資料。


### Install and library

Install：
````r
install.packages("dplyr")
````

Library：
````r
library(dplyr)
````

### ToothGrowth

Here we will use a data set that comes with R called `ToothGrowth`.

````r
head(ToothGrowth)
````
|  len|supp | dose|
|----:|:----|----:|
|  4.2|VC   |  0.5|
| 11.5|VC   |  0.5|
|  7.3|VC   |  0.5|
|  5.8|VC   |  0.5|
|  6.4|VC   |  0.5|
| 10.0|VC   |  0.5|

````r
str(ToothGrowth)
````
`
'data.frame':60 obs. of  3 variables:
 $ len : num  4.2 11.5 7.3  5.8 6.4 10 11.2 11.2 5.2 7 ...
 $ supp: Factor w/ 2 levels "OJ","VC": 2 2 2 2 2 2 2 2 2 2 ...
 $ dose: num  0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 ...
`

Data description
- len: the length of teeth in each of 10 guinea pigs
- supp: two delivery methods (orange juice or ascorbic acid)
- dose: 維他命C劑量濃度(0.5, 1 and 2 mg)

### Basic usage

**1.** `filter()`
````r
filter(ToothGrowth, supp == "VC", dose == 1.0)
````
|  len|supp | dose|
|----:|:----|----:|
| 16.5|VC   |    1|
| 16.5|VC   |    1|
| 15.2|VC   |    1|
| 17.3|VC   |    1|
| 22.5|VC   |    1|
| 17.3|VC   |    1|

This is equivalent to R basic indicator method:

````r
ToothGrowth[ToothGrowth$supp == "VC" & ToothGrowth$dose == 1.0, ]
````

Or you can use `subset` function:

````r
subset(ToothGrowth, supp == "VC" & dose == 1.0, drop = TRUE)
````

**2.** `arrange()`

EX1. `ToothGrowth` data
````r
arrange(ToothGrowth, len)
arrange(ToothGrowth, desc(len))
````
| len|supp | dose|
|---:|:----|----:|
| 4.2|VC   |  0.5|
| 5.2|VC   |  0.5|
| 5.8|VC   |  0.5|
| 6.4|VC   |  0.5|
| 7.0|VC   |  0.5|
| 7.3|VC   |  0.5|

|  len|supp | dose|
|----:|:----|----:|
| 33.9|VC   |    2|
| 32.5|VC   |    2|
| 30.9|OJ   |    2|
| 29.5|VC   |    2|
| 29.4|OJ   |    2|
| 27.3|OJ   |    1|

````r
head(iris)
````
| Sepal.Length| Sepal.Width| Petal.Length| Petal.Width|Species |
|------------:|-----------:|------------:|-----------:|:-------|
|          5.1|         3.5|          1.4|         0.2|setosa  |
|          4.9|         3.0|          1.4|         0.2|setosa  |
|          4.7|         3.2|          1.3|         0.2|setosa  |
|          4.6|         3.1|          1.5|         0.2|setosa  |
|          5.0|         3.6|          1.4|         0.2|setosa  |
|          5.4|         3.9|          1.7|         0.4|setosa  |

````r
arrange(iris, Sepal.Length, desc(Sepal.Width))    ## increase Sepal.Length and descrease Sepal.Width
````
| Sepal.Length| Sepal.Width| Petal.Length| Petal.Width|Species |
|------------:|-----------:|------------:|-----------:|:-------|
|          4.3|         3.0|          1.1|         0.1|setosa  |
|          4.4|         3.2|          1.3|         0.2|setosa  |
|          4.4|         3.0|          1.3|         0.2|setosa  |
|          4.4|         2.9|          1.4|         0.2|setosa  |
|          4.5|         2.3|          1.3|         0.3|setosa  |
|          4.6|         3.6|          1.0|         0.2|setosa  |

**3.**`select()`

EX1. `ToothGrowth` data
````r
select(ToothGrowth, len, supp)
select(ToothGrowth, supp, dose)
````
|  len|supp |
|----:|:----|
|  4.2|VC   |
| 11.5|VC   |
|  7.3|VC   |
|  5.8|VC   |
|  6.4|VC   |
| 10.0|VC   |

|supp | dose|
|:----|----:|
|VC   |  0.5|
|VC   |  0.5|
|VC   |  0.5|
|VC   |  0.5|
|VC   |  0.5|
|VC   |  0.5|

EX2. `iris` data
````r
select(iris, Sepal.Width, Petal.Width, Species)
select(iris, -c(Sepal.Length, Petal.Length))
````
| Sepal.Width| Petal.Width|Species |
|-----------:|-----------:|:-------|
|         3.5|         0.2|setosa  |
|         3.0|         0.2|setosa  |
|         3.2|         0.2|setosa  |
|         3.1|         0.2|setosa  |
|         3.6|         0.2|setosa  |
|         3.9|         0.4|setosa  |

| Sepal.Width| Petal.Width|Species |
|-----------:|-----------:|:-------|
|         3.5|         0.2|setosa  |
|         3.0|         0.2|setosa  |
|         3.2|         0.2|setosa  |
|         3.1|         0.2|setosa  |
|         3.6|         0.2|setosa  |
|         3.9|         0.4|setosa  |

**4.** `mutate()`
````r
mutate(ToothGrowth, meanLen = mean(len), sdLen = sd(len))
````
|  len|supp | dose|  meanLen|    sdLen|
|----:|:----|----:|--------:|--------:|
|  4.2|VC   |  0.5| 18.81333| 7.649315|
| 11.5|VC   |  0.5| 18.81333| 7.649315|
|  7.3|VC   |  0.5| 18.81333| 7.649315|
|  5.8|VC   |  0.5| 18.81333| 7.649315|
|  6.4|VC   |  0.5| 18.81333| 7.649315|
| 10.0|VC   |  0.5| 18.81333| 7.649315|

This is equivalent to R basic `transform` function:

````r
transform(ToothGrowth, meanLen = mean(len), sdLen = sd(len))
````

**5.** `summarize()` or `summarise()`
````r
summarize(ToothGrowth, meanLen = mean(len))
summarise(ToothGrowth, sdLen = sd(len))
````

### Chain operator `%.%` usage

**1.** Calculate the mean length of teeth (len) for each delivery method (supp)
````r
ToothGrowth %.%                      ## data
group_by(supp) %.%                   ## group
summarize(meanLen = mean(len))       ## calculate mean
````
````
Source: local data frame [2 x 2]

   supp  meanLen
 1   OJ 20.66333
 2   VC 16.96333
````

**2.** Calculate the mean teeth length by delivery method and dose level?
````r
ToothGrowth %.%
group_by(supp, dose) %.%
summarise(meanLen = mean(len), sdLen = sd(len))
````
````
Source: local data frame [6 x 4]
Groups: supp

    supp dose meanLen    sdLen
  1   OJ  0.5   13.23 4.459709
  2   OJ  1.0   22.70 3.910953
  3   OJ  2.0   26.06 2.655058
  4   VC  0.5    7.98 2.746634
  5   VC  1.0   16.77 2.515309
  6   VC  2.0   26.14 4.797731
````

#####*Now please compare the following 3. and 4. results：*

**3.** Create new variable
````r
ToothGrowth %.%
mutate(StdLen = (len - mean(len)) / sd(len))
````
|  len|supp | dose|     StdLen|
|----:|:----|----:|----------:|
|  4.2|VC   |  0.5| -1.9104107|
| 11.5|VC   |  0.5| -0.9560769|
|  7.3|VC   |  0.5| -1.5051456|
|  5.8|VC   |  0.5| -1.7012416|
|  6.4|VC   |  0.5| -1.6228032|
| 10.0|VC   |  0.5| -1.1521729|

**4.** Create new variable, which is the standard value of the tooth length in the six supp x dose groups.

````r
ToothGrowth %.%
group_by(supp, dose) %.%
mutate(StdLen = (len - mean(len)) / sd(len))
````
|  len|supp | dose|     StdLen|
|----:|:----|----:|----------:|
|  4.2|VC   |  0.5| -1.3762298|
| 11.5|VC   |  0.5|  1.2815685|
|  7.3|VC   |  0.5| -0.2475757|
|  5.8|VC   |  0.5| -0.7936987|
|  6.4|VC   |  0.5| -0.5752495|
| 10.0|VC   |  0.5|  0.7354456|

##### Practice

**[practice 1]**
````r
ToothGrowth %.%
filter(dose == 1) %.%
select(len, supp) %.%
arrange(len)
````
|  len|supp |
|----:|:----|
| 13.6|VC   |
| 14.5|VC   |
| 14.5|OJ   |
| 15.2|VC   |
| 15.5|VC   |
| 16.5|VC   |

**[practice 2]**
````r
TG05 <- ToothGrowth %.%
filter(dose == 0.5) %.%
select(len, supp) %.%
arrange(desc(len)) %.%
head(n = 5)
TG05
````
|  len|supp |
|----:|:----|
| 21.5|OJ   |
| 17.6|OJ   |
| 16.5|OJ   |
| 15.2|OJ   |
| 14.5|OJ   |
