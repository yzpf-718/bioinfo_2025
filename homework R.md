#### （1）iris数据集有几列？每列的数据类型是什么?
```
> ncol(iris)
[1] 5
> colnames(iris)
[1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width"  "Species"   
> class(iris$Sepal.Length)
[1] "numeric"
> class(iris$Sepal.Width)
[1] "numeric"
> class(iris$Petal.Length)
[1] "numeric"
> class(iris$Petal.Width)
[1] "numeric"
> class(iris$Species)
[1] "factor"
```
因此，iris数据集有5列，前四列的数据类型是数值变量，最后一列的数据类型是因子。
#### （2）按Species列将数据分成3组，分别计算Sepal.Length的均值和标准差，保存为一个csv文件，提供代码和csv文件的内容。
```
> mean=aggregate(Sepal.Length~Species,iris,mean)
> sd=aggregate(Sepal.Length~Species,iris,sd)
> mean_sd <- rbind(mean, sd)
```
#### （3）对不同Species的Sepal.Width进行One way ANOVA分析，提供代码和输出的结果。 
```
> summary(aov(Sepal.Width~Species,data=iris))
             Df Sum Sq Mean Sq F value Pr(>F)    
Species       2  11.35   5.672   49.16 <2e-16 ***
Residuals   147  16.96   0.115                   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```