# Stata的统计应用——金融计量学第一次作业

## 基本目标

- 认识Stata MP 16，了解Stata在计量经济学中的运用
- 学会在Stata中导入和导出数据
- 学会在Stata中查看和审视数据和变量，学会数据的选择、排序等基本操作
- 学会利用Stata画散点图、直方图和折线图
- 学会用Stata做描述性统计、变量相关性分析等基本操作
- 学会用Stata估计概率密度函数（核密度估计）
- 学会用Stata做OLS多元回归分析

## 实验环境和数据

所用的Stata版本为Stata MP16

数据采用课件数据

## 实验内容

### 导入和导出数据

通过工具栏导入数据(推荐)或使用代码：

```shell
import excel "C:\Users\Explorer\Desktop\学科文档\计量金融学\课件数据\数据\grilic_small.xls", sheet("Sheet1") firstrow
```

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301160834994.png" alt="image-20200301160834994" style="zoom:67%;" />

导出数据也可以使用工具栏UI操作：

比如我们将数据（所有变量）导出到桌面example.csv:

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301161201906.png" alt="image-20200301161201906" style="zoom:50%;" />

同样也可以使用代码进行导出：

```shell
export delimited using "C:\Users\Explorer\Desktop\example.csv", replace
```

### 查看和审视数据和变量

数据可以在数据编辑器内进行浏览和编辑：

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301161904690.png" alt="image-20200301161904690" style="zoom:50%;" />

变量窗口展示了变量的基本信息（如标签）：

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301162025952.png" alt="image-20200301162025952" style="zoom:50%;" />

如果想要更改变量的名称、标签、属性，在变量窗口下方的属性窗口进行更改，更改前要先将变量解锁。

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301162953231.png" alt="image-20200301162953231" style="zoom:50%;" />

变量名称的更改也可以用$rename$`语句`：

```shell
rename expr expr可以改名称
```

审视数据可用$discribe$或者$d$：

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301163332800.png" alt="image-20200301163332800" style="zoom:67%;" />

$list$语句可以显示变量的具体数据：

```R
list s expr lnw
```

后缀in 1/10可只显示第一行到第十行数据，如：

```R
list s expr lnw in 1/10
```

后缀if语句可通过逻辑关系定义数据集的子集：

```R
list s ln if s>=18
```

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301164206398.png" alt="image-20200301164206398" style="zoom: 50%;" />

若要删除满足$s>=18$条件的的观测值，可使用$drop$语句；若要仅保留$s>=18$的观测值，可使用$keep$语句

```R
drop if s>=16
keep if s>=16
```

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301164517711.png" alt="image-20200301164517711" style="zoom:50%;" />

数据排序(升序)则使用$sort$语句：

```R
sort s
```

若要使用降序排列，可用$gsort$语句，记得在变量前加负号：

```R
gsort -s
```

### 绘图

#### 直方图（histogram, 简写为hist）

直方图使用$histogram$命令：

```R
histogram s, width(1) frequency
```

![image-20200301165533271](Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301165533271.png)

可简写为

```R
hist s,width(1) freq
```

#### 散点图（scatter，简写为sc）

```R
sc lnw s
```

![image-20200301165918965](Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301165918965.png)

若要在散点图中标注每一个点观测值，则

```
gen n=_n
scatter lnw s,mlabel(n)
```

#### 折线图（line）

折线图使用line语句

```
line s lnw
```

![image-20200301170619228](Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301170619228.png)

### 描述性统计

若要看单个变量的统计统计特征，可用$summarize$语句（可简写为su），若想要显示所有变量的统计特征，则使用$sum$语句：

```
su lnw
sum
```

![image-20200301171151268](Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301171151268.png)

若想看到lnw更多的统计指标，如峰度、偏度，可加选择项detail

```
sum lnw,detail
```

若要显示变量s的经验累积分布函数，可用$tabulate$语句，可简写为ta

```
ta s
```

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301171327819.png" alt="image-20200301171327819" style="zoom:50%;" />

变量的相关系数可由$pwcorr$语句得到：

```
pwcorr lnw s expr, sig star(0,05)
```

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301171451347.png" alt="image-20200301171451347" style="zoom: 67%;" />

（其中，$*$代表显著性水平小于或等于0.05）

使用$tabstat$语句也可得到变量的基本描述性统计信息：

```
tabstat lnw s expr, stat(max min mean p20 sd n)
```

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301172155188.png" alt="image-20200301172155188" style="zoom:67%;" />

## 案例：以grilic.dta为例

这次使用更完整的数据集进行统计分析

导入数据并看一看数据的基本情况：

```
use "C:\Users\Explorer\Desktop\学科文档\计量金融学\课件数据\数据\grilic.dta"
describe
sum
```

做lnw的直方图

```
hist lnw,width(0.1)
```

<img src="Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301174002389.png" alt="image-20200301174002389" style="zoom:67%;" />

可用$kdensity$函数做估计变量的概率密度函数

```
kdensity lnw
```

![image-20200301174233561](Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301174233561.png)

加上正态估计函数作为对比

```
kdensity lnw, normal normop(lpattern(dash))
```

![image-20200301174514908](Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301174514908.png)

看一看工资水平本身的分布：

```
g wage=exp(lnw)
kdensity wage
```

![image-20200301174713042](Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301174713042.png)

观察$s=16$的工资对数分布：

```
kdensity lnw if s==16
```

![image-20200301174854915](Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301174854915.png)

做一个对比

```
twoway kdensity lnw || kdensity lnw if s==16,lpattern(dash)
```

![image-20200301175018370](Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301175018370-1583056989567.png)



## 多元回归分析

可使用regress语句（简写为reg）实现多元回归分析：

```
reg lnw s expr age iq tenure smsa
```

![image-20200301175430821](Stata%E7%9A%84%E7%BB%9F%E8%AE%A1%E5%BA%94%E7%94%A8%E2%80%94%E2%80%94%E9%87%91%E8%9E%8D%E8%AE%A1%E9%87%8F%E5%AD%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%9C%E4%B8%9A.assets/image-20200301175430821.png)