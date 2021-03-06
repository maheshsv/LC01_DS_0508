探索性数据分析 Exploratory Data Analysis
========================================

这个文档整理了[Coursera](https://www.coursera.org/)上数据系列课[Exploratory Data Analysis](https://www.coursera.org/course/exdata)的一些内容和笔记。

### 1. R中的绘图系统

R中有三大绘图系统，分别为 Base Plotting System，Lattice System 和 ggplot2 System。

#### Base Plotting System

Base Plotting System 是R自带的绘图系统。它的优点是使用非常方便，可以用一系列的R命令来绘图，
缺点是一旦生成图像之后就无法再改变，所以在绘图之前需要做一定的规划。

下面是一些例子


```r
library(datasets)
# 散点图
plot(cars$speed, cars$dist, xlab = "speed", ylab = "dist")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-11.png) 

```r
# 直方图
hist(cars$speed, xlab = "speed")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-12.png) 

```r
# 盒型图
boxplot(cars$speed, xlab = "speed")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-13.png) 


#### Lattice System

Lattice System 是R中的一个包，项目的主页在[这里](http://lattice.r-forge.r-project.org)。
它的优点是用一个函数调用就可以生成图像，而且生成的图的大小能够自动调整适应，特别适合在一张图下绘制多个图的情况。
它的缺点则是仅用一次函数调用来生成图像有时候会显得太过复杂，而且在多图情况下给各个图添加注释也很不方便。
和Base Plotting System一样，Lattice System在图像生成后就无法再对图像做改变。

Lattice System中的一些绘图函数

- xyplot 用于生成散点图
- bwplot 用于生成盒型图
- histogram 用于生成直方图
- stripplot 和bwplot类似，用的是散点的形式
- dotplot 用于生成克利夫兰点图
- splom 用于生成散点图阵列
- levelplot, contourplot 利用图像来生成图

下面是`xyplot`的一些例子，使用形式 `xyplot(y ~ x | f * g, data, ...)`


```r
library(lattice)
library(datasets)
xyplot(Ozone ~ Wind, data = airquality)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-21.png) 

```r
xyplot(Ozone ~ Wind | Month, data = airquality, layout = c(5, 1))
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-22.png) 


Lattice System中还可以使用Panel Function，针对每个子图添加额外的图形。


```r
set.seed(10)
x <- rnorm(100)
f <- rep(0:1, each = 50)
y <- x + f - f * x + rnorm(100, sd = 0.5)
f <- factor(f, labels = c("Group 1", "Group 2"))
# 无panel function
xyplot(y ~ x | f, layout = c(2, 1))  # 绘制两个子图
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-31.png) 

```r
# 自定义panel function
xyplot(y ~ x | f, panel = function(x, y, ...) {
    panel.xyplot(x, y, ...)  # 先调用默认的panel function
    panel.abline(h = median(y), lty = 2)  # 在中值处添加水平线
})
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-32.png) 

```r
# 自定义panel function
xyplot(y ~ x | f, panel = function(x, y, ...) {
    panel.xyplot(x, y, ...)  # 先调用默认的panel function
    panel.lmline(x, y, col = 2)  # 添加回归线
})
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-33.png) 


#### ggplot2 System

ggplot2 System 和 Lattice System 一样也是R中的其中的一个包，基于图形的语法，
试图结合 Base Plotting System 和 Lattice System 各自的优点，它的项目主页在[这里](http://ggplot2.org/)。
ggplot2 System 结合了 Base Plotting System 和 Lattice System 的特点，能够自动调节图像的大小来适应显示，
还能够很方便地给各个图添加注释，绘图函数的默认参数也给了绘图极大的方便，它还能够很轻松地实现复杂的多层图像。

qplot()的一些运用，qplot()可以很方便地绘制出漂亮的图形。


```r
library(ggplot2)  # 加载这个包
# 简单的散点图
qplot(displ, hwy, data = mpg)
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-41.png) 

```r
# 根据`drv`分类，用不同颜色显示的散点图
qplot(displ, hwy, data = mpg, color = drv)
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-42.png) 

```r
# 在散点图上额外添加图形
qplot(displ, hwy, data = mpg, geom = c("point", "smooth"))
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-43.png) 

```r
# 直方图
qplot(hwy, data = mpg, fill = drv)
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-44.png) 

```r
# 分类显示
qplot(displ, hwy, data = mpg, facets = . ~ drv)  # 横向显示
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-45.png) 

```r
qplot(hwy, data = mpg, facets = drv ~ .)  # 纵向显示
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-46.png) 


另外一些图形的绘制


```r
library(ggplot2)
library(datasets)
airquality$Month = factor(airquality$Month)
qplot(Ozone, data = airquality, fill = Month)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-51.png) 

```r
qplot(Ozone, data = airquality, geom = "density", color = Month)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-52.png) 

```r
qplot(Wind, Ozone, data = airquality, shape = Month)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-53.png) 

```r
qplot(Wind, Ozone, data = airquality, color = Month, geom = c("point", "smooth"), 
    method = "lm")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-54.png) 

```r
qplot(Wind, Ozone, data = airquality, geom = c("point", "smooth"), method = "lm", 
    facets = . ~ Month)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-55.png) 


qplot()与R内置绘图系统的plot()很相似，但是提供了更多的特性，能够绘制出更漂亮的图形。

ggplot()能够更加灵活地绘制图形，它的绘制方式是一层一层地叠加图形特性，最后生成整个图像。
ggplot()函数返回一个对象，然后我们再在这个图像上不停地添加一些显示特性，最后可以通过print()函数来生成
最终的图形，对于编程来说，也很方便。


```r
library(ggplot2)
library(datasets)
g <- ggplot(airquality, aes(Wind, Ozone))
# 以散点图方式显示
g + geom_point()
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-61.png) 

```r
# 添加回归线
g + geom_point() + geom_smooth(method = "lm")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-62.png) 

```r
# 分类显示
g + geom_point() + facet_grid(. ~ Month) + geom_smooth(method = "lm")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-63.png) 

```r
# 更改标签
g + geom_point(aes(color = Month)) + labs(title = "Ozone ~ Wind", x = "Wind", 
    y = "Ozone")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-64.png) 


### 2. 聚类分析

聚类分析将数据集划分成有意义或有用的组(簇)，它根据在数据的描述及数据之间的关系信息，对数据点进行分组。
分组后，组内的数据点相互之间是相似的，而不同组中的数据点是不相似的。
组内的相似性越大，组间的差别越大，聚类的效果就越好。

#### 距离的定义

距离定义了不同数据点之间的相似性，或是不同组之间的相似性。

- 欧几里得距离
- 曼哈顿距离
- 其他距离

#### 层次聚类(Hierarchical Clustering)

层次聚类是一种聚类方法。它通过生成一系列嵌套的聚类树来完成聚类。
单个数据聚类处在树的最底层，在树的顶层有一个根节点聚类。根节点聚类覆盖了全部的所有数据。
这种方法的特点是生成一个聚类树，而不单单是几个分类，我们可以通过聚类树的不同高度，得到不同的分类。

算法流程非常简单

1. 找到两个距离最近的组(单个数据点也可以当做一个组)
2. 将这两个组合并成一个组
3. 重复步骤1和步骤2，直到只剩下一个组

数据点之间的距离可以直接根据距离的定义进行计算，但组间的距离计算有不同的方法

- 平均法。用一个等效的数据点来代表整个组，用等效点之间的距离代表组间的距离
- 极值法。计算所有两个组之间数据点的距离，用最大值来代表组间的距离

利用这两种方法得到的聚类效果可能会很不一样，一般建议两者都尝试一下，进行比较。

下面是R中的一个例子


```r
# 产生数据
set.seed(1234)
x <- rnorm(12, mean = rep(1:3, each = 4), sd = 0.2)
y <- rnorm(12, mean = rep(c(1, 2, 1), each = 4), sd = 0.2)
plot(x, y, col = "blue", pch = 19, cex = 2)
text(x + 0.05, y + 0.05, labels = as.character(1:12))
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-71.png) 

```r
# 进行层次聚类
dataFrame <- data.frame(x = x, y = y)
distxy <- dist(dataFrame)
hClustering <- hclust(distxy)
plot(hClustering)
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-72.png) 


#### K-means聚类(K-means Clustering)

K-means是最为经典的聚类方法，是十大经典数据挖掘算法之一。
K-means算法的基本思想是以数据空间中的k个点为中心进行聚类，然后对数据集进行分类，每个数据点选择一个距离最近的中心点。
之后通过迭代的方法，逐次更新各个分类的中心值，直至得到最好的聚类效果。
它的特点是能将数据集分成k个不同的类别，而且分类效果也比较好。

算法流程也很简单

1. 选定k个中心点，表示要分成k类
2. 将每个数据点划分给各个中心点(划分依据可以是每个数据点选择距离自己最近的一个中心点)
3. 对产生的k个分类重新计算出k个中心点(可以采用平均的方法)
4. 重复步骤2和步骤3，直到聚类效果达到要求

一个R的例子


```r
# 产生数据
set.seed(1234)
x <- rnorm(12, mean = rep(1:3, each = 4), sd = 0.2)
y <- rnorm(12, mean = rep(c(1, 2, 1), each = 4), sd = 0.2)
plot(x, y, col = "blue", pch = 19, cex = 2)
text(x + 0.05, y + 0.05, labels = as.character(1:12))
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-81.png) 

```r
# 进行K-means聚类
dataFrame <- data.frame(x = x, y = y)
kmeansObj <- kmeans(dataFrame, centers = 3)
plot(x, y, col = kmeansObj$cluster, pch = 19, cex = 2)
points(kmeansObj$centers, col = 1:3, pch = 3, cex = 3, lwd = 3)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-82.png) 


### 3. Demo
