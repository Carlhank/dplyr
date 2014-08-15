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
````r
str(ToothGrowth)
````

Data description
- len: the length of teeth in each of 10 guinea pigs
- supp: two delivery methods (orange juice or ascorbic acid)
- dose: 維他命C劑量濃度(0.5, 1 and 2 mg)

### Basic usage

**1.** `filter()`
````r
filter(ToothGrowth, supp == "VC", dose == 1.0)
````
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

EX2. `iris` data
````r
head(iris)
arrange(iris, Sepal.Length, desc(Sepal.Width))  ## increase Sepal.Length and descrease Sepal.Width
````

**3.**`select()`

EX1. `ToothGrowth` data
````r
select(ToothGrowth, len, supp)
select(ToothGrowth, supp, dose)
````

EX2. `iris` data
````r
select(iris, Sepal.Width, Petal.Width, Species)
select(iris, -c(Sepal.Length, Petal.Length))
````

**4.** `mutate()`
````r
mutate(ToothGrowth, meanLen = mean(len), sdLen = sd(len))
````

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

**2.** Calculate the mean teeth length by delivery method and dose level?
````r
ToothGrowth %.%
group_by(supp, dose) %.%
summarise(meanLen = mean(len), sdLen = sd(len))
````

#####*Now please compare the following 3. and 4. results：*

**3.** Create new variable
````r
ToothGrowth %.%
mutate(StdLen = (len - mean(len)) / sd(len))
````

**4.** Create new variable, which is the standard value of the tooth length in the six supp x dose groups.

````r
ToothGrowth %.%
group_by(supp, dose) %.%
mutate(StdLen = (len - mean(len)) / sd(len))
````

##### Practice

**[practice 1]**
````r
ToothGrowth %.%
filter(dose == 1) %.%
select(len, supp) %.%
arrange(len)
````

**[practice 2]**
````r
TG05 <- ToothGrowth %.%
filter(dose == 0.5) %.%
select(len, supp) %.%
arrange(desc(len)) %.%
head(n = 5)
TG05
````
