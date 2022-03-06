

数据合并
HeBIngHou <- merge(数据框A,数据框B,by = c("证券代码","time"))

将短型数据转为Long Form长型面板数据
```
library(reshape2)
LongFormDF <- melt(短型面板数据框,
            id.vars = c('证券代码','保留不变的列'),
            na.rm = FALSE ,
            variable.name='time将矩阵列名（常为年份）整理为一个新变量',
            value.name='矩阵内所有数值的变量名')
```

# 《Data Mining with R：cases》
r包 `DMwR`是为此书设计的包，含函数和数据，数据和代码也在[主页](https://www.dcc.fc.up.pt/~ltorgo/DataMiningWithR/code2.html)

`read.table`读取表格。
参数|含义
-|-
header|将表格的第一行设置为列名
dec|目标文件中小数点的形状（decimal point）
`col.names = c('')` |列名（就是变量名）
row.names | 行名，默认就是从1开始排序


- 将画幅分割成一行两列，使接下来两幅图以左右布局呈现。`par(mrow=c(1,2))`,画完后记得`par(mrow=c(1,1))`恢复c（1，1）画幅。  


# 《R数据清洗及应用》
[配套网站](www.data-cleaning.org)

## 第五章 清洗文本数据
### 字符规范化
> 指让各个选项变得一致的过程.两种形式:**编码规范化**(文本的技术表示)和**符号规范化**(字符转换)   

#### 编码转换和Unicode规范化
baseR自带同各国iconv规范的转换编码的工具，使用`enc2utf8()`即可将某个字符向量的所有元素都转换为UTF-8格式.任何编码之间的转换都可以通过`iconv()`完成,使用`iconvlist()`可查询支持的编码规范:
```{r}
y <- iconv(x , to = "[encoding]" , from = "[encoding]")
```
另有`stringi`软件包,基于ICU库(ICU,2014),可以确保不受平台的限制.

### 正则表达式
#### 基本正则表达式
正则表达式，又称*规则表达式*，是一种文本模式，通常用来**检索、替换和控制文本**。主要包括`a `到 `z `的字母以及一些特殊的元字符。  
正则表达式的**作用**：
- 测试字符串内的模式。例如，可以测试输入字符串，以查看字符串内是否出现电话号码模式或信用卡号码模式。这称为数据验证。
- 替换文本。可以使用正则表达式来识别文档中的特定文本，完全删除该文本或者用其他文本替换它。
- 基于模式匹配从字符串中提取子字符串。可以查找文档内或输入域内特定的文本。







# 《Programming Skills for Data Science》
中译：数据科学之编程技术（使用r进行数据清理、分析和可视化）  

[实践代码](https://github.com/programming-for-data-science/in-action)  
[在线练习](https://github.com/programming-for-data-science)  
[git下载](git-scm.com/downloads)


第一章 设置计算机    
第二章 命令行  
第三章 git版本控制  
第四章 Markdown  
第五章 R语言


## 第六章 函数
数据科学中，将指令分组为可重复执行的函数很有帮助：
- **数据管理**。对加载和组织数据的指令进行分组，以便将其应用于多个数据集
- **数据分析**。储存计算感兴趣指标的步骤，以便对多个变量进行重复分析
- **数据可视化**。定义创建具有特定结构和样式的图形的过程，以便生成一致的报表。

R语言附带的内置函数（也称base R function,可在[quick R](statmethods.net/management/functions.html)查看).  
更多函数参考则参见[R函数汇总备忘单](cran.r-project.org/doc/contrib/Short-refcard.pdf).
### 加载函数
```
# stringr包用于处理字符串str_count()函数能够返回'子字符串'在单词中出现的次数
library(stringr)
str_count("Mississippi","i")
```
> 改进R代码格式的包:`lintr`包可检测违反tidyverse风格指南的代码.`styler`包将建议的格式用于代码.加载这些包后,通过`lint("文件名.r")`或者`style_file("文件名.r")`来规范代码格式
### 编写函数
- 函数的组成:
    - **参数**:分配给函数的值,使用`function(变量名称,变量名)`来表明正在创建函数,括号()中的单词是变量名称,用逗号分隔,这些变量将接收作为参数传入的值
    - **函数体**:介于大括号`{}`之间的代码块.用于指定函数要执行的所有指令(代码行).
    - **返回值**:一个函数将输出该函数的最后一个语句(行)中得到的任何值.也可以将希望返回的值传递给`return()`函数.出于格式简洁,如非要输出的值包含之前语句求出的值,否则直接返回最后一行的就行,可以省略return()

不显式地使用`else`子句,而是在不满足if条件的时候让函数"继续运行".这样避免else代码可以将if条件**视为只处理特殊情况**
```
add_title <- function(full_name,title){
    if(starsWith(full_name,title)){
        return(full_name)
    }

    name_with_title <- paste(title,full_name)
    name_with_title
}
```

## 第七章 向量 
向量是存储再单个变量中的一维数据的集合。向量中所有元素**必须属于同一类型**（如全部元素都是数字/字符/逻辑，不能有其中两类）
### 创建向量
- 用内置的`c()`函数,将值组合成向量.
- 用`seq()`函数,接受两个参数,在它们之间生成整数的向量,可用第三个参数指定相邻数字的间隔.
- 用冒号`a:b`生成一个序列,返回从a到b的向量,元素值以1递增.
### 向量化操作

向量的加减乘除运算都是按元素进行操作的.想将两向量简单拼接在一起可用`paste(v1,v2,sep="")`函数               
#### 循环
两个向量数目不等,进行操作时会发生循环,r将重复使用较短向量中的元素
#### 多数都是向量
R 将所有字符\数字\布尔值都存储为向量(单个的就是长度为1的向量)
> `length()`可获取向量长度.如果用length()来检查单个字符串,则返回向量长度1.要用`nchar()`才能获取字符串中的字符数
#### 向量化函数
所有基本数据类型都被存储为向量.所以很多函数都可以应用于向量,而不仅仅是作用于单个值.实际上,函数面对向量和面对单个值的工作方式相同,因为**单个值只是向量的一个实例**  
所以们可以在向量上使用任何函数,它将以**相同的向量化、元素级的方式运行**:函数将产生一个新的向量,其中已按顺序将函数作用于每个单独元素.  
- 由于各种操作在R中是向量化的,所以不需要显式地迭代向量,也就是几乎不需要使用循环.
### 向量索引,获取子集
通过索引index引用向量中的元素.需要注意: **R中索引从 1 开始**,这不同于其他语言的从 0 开始索引.

### 向量过滤
在索引的位置,方括号里面用逻辑值True\False也可实现过滤  
`smallshoes <- shoe_sizes < 6.5`可以按照shoe_sizes的顺序一组逻辑值(大于6.5则为True)  
`shoe_sizes[shoe_sizes < 6.5]`也可实现筛选

### 向量修改
将新值赋予向量中某个索引即可更新该索引位置的值`prices[3] <- 36`  
在向量中创建新元素(在向量最后加一个索引)`prices[length(prices)+1] <- 89`  
跟踪索引很困难,且索引会随数据改变而改变,所以最好的方法是将原向量与新元素组成一个新的向量`more_people <- c(people,"Johnson")`  
### 修改和过滤结合:
- 作用:常用于打包和清理数据,**允许识别和操作无效值或其他异常值**.  
`v1[v1 > 10] <- 10`  



## 第八章 列表
### list和vector的区别
- 列表也是一维的数据组合,但列表**可以储存不同类型的元素**,甚至可以包含向量和其他列表.  
- 另外,列表的元素可以**用名称进行标记**  
> [警告]R中列表list的定义不同于其他语言,如python中至于dict结构才能实现名称标记
> [注意]若要标记集合中的元素,使用列表!虽然可以用名称标记向量元素,但是不常见且访问元素时语法复杂.
- 向量使用单括号`[]`来按索引访问元素,但列表使用双括号`[[]]`来按索引访问元素.注意列表中使用单括号`[]`将会返回符合括号内要求的(子)列表,而非元素.

### 创建list和命名其中元素
用list()函数创建列表即可,同时要注意可以(也应该如此)为列表中每个元素指定名称,方法类似于为函数命名参数:
```{r}
person <- list(
    first_name = "Ada",
    job = "Programmer",
    salary = "78000",
    in_union= TRUE,
    favorites = list(
        music = "jazz",
        food = "pizza"
    )
)
```
- 标记list元素名称的好处:访问特定元素更容易\更不易出错,也易于理解,建议始终标记创建的列表  
- **可以使用names()函数获取列表中各项的名称**,这对理解其他数据源的变量结构很有用.
- 列表中包含列表的这种双层结构常见于表示JSON数据(第14章)

### 访问列表元素
由于通常情况下列表的元素具有标记名称,所以可以通过它们的标记名称而不是向量那样必须用索引号来访问.  
- 用`$`表示法来指定访问的**元素**名称($符号可看作英语中的所有格符号,意为xxx的)`person$salary`就代表person列表中的salary元素值,有嵌套列表的话可以用多个`$`来访问  
- 用`[[]]`双括号来访问指定**元素**
- 用`[]`单括号来访问指定的**子列表**

### 修改列表
直接向现有列表的元素分配新值就行.
- 技巧:将`NULL`分配给现有列表元素来删除列表元素.
> NULL表示"未定义",NA表示"缺失值".所以NULL可用于删除值.[参考](r-bloggers.com/r-na-vs-null)

### laapply()函数
r中大多数函数都是**向量化**的,因此向量可以直接作为参数传入函数中.但将函数应用于列表中的每个项目则需要借用lapply()函数的帮助.   
- 要注意lapply()中第一个参数是列表名称,第二个参数是函数名称,函数名不带括号.第三个参数及之后的位置上是函数的各种参数.
例如将转换大写字母的函数toupper()作用于列表people中的每一个元素时,可用:`people_upper <- lapply(people,toupper)`  
- lapply()不修改原列表,而是返回一个新列表.
- 通常将`lapply()`与自定义函数一起使用:
```{r}
greet <- function(item){
    paste("Hello",item)  #函数体的最后一行将被return
}

people <- c("sarah","han","zhang","wang")

greetings <- lapply(people,greet)
```
> **apply()**函数系列成员:sapply()作用于向量,返回的也是向量,考虑到大多数r函数都是向量化的,sapply()一般只用于自定义函数.


## 第九章 理解数据
开放的数据集:  
政府出版物:  
[美国政府公开数据](data.gov)  
[加拿大政府](open.canada.ca/en/open-data)  
[印度](data.gov.in)  
新闻数据:  
[纽约时报开发者网络](developer.nytimes.com)  
[数据政治博客FiveTirtyEight](data.fivethirtyeight.com)   
科学研究:  
[自然杂志推荐的数据仓库](nature.com/sdata/policies/repositories)   
社交网络:  
[推特开发平台](developer.twitter.com/en/docs)  
[Google API](developer.google.com/apis-explorer)  
在线社区:  
[Kaggle数据科学与机器学习之家](www.kaggle.com)   
[Socrata数据服务平台](opendata.socrata.com)   
[UCI机器学习库](archive.ics.uci.edu/ml/index.php)  
[r/dataset](reddit.com/r/datasets)  



## 第十章 数据框

### 数据框与列表与向量
数据框实际上是列表。这个列表中每一个元素都是长度相同的向量（数据框的列就是一个向量）。但数据框又有其特点，使其特别适合处理数据表。

### 使用数据框
**创建**：
- 用`data.frame()`来组合多个向量.该函数接受向量作为参数,并创建一个每列对应一个向量的表.
> data.frame()的参数可选:`stringsAsFactors = FALSE`告诉R将那个包含字符串的向量视为典型向量,而不是因子factor.

- 从外部加载数据集

**数据帧的结构**  
函数|描述
-|:-:
nrow(数据框)|返回数据框的行数
ncol(数据框)|返回数据框的列数
dim(数据框)|数据框的尺寸(行数\列数)
colnames(数据框)|数据框的列名称
rownames(数据框)|行名
head(数据框)|前几行(作为新的数据框)
tail(数据框)|后几行
View(数据框)|Rstudio中用查看器查看数据框

{注意:}使用`colnames(数据框) <- 新列名向量`可以为数据框分配一组新的列名  

**访问数据框**
- 数据框是**列表**-->用美元表示法(`my_dataframe$column_name`)或者双括号表示法(`my_df[["column_name]]`)来访问整个列.
- 数据框独特的访问法:单括号配合逗号-->`my_df["row_name" , "col_name"]`可访问指定行名和列名的元素(若逗号右边的列参数空缺,则访问整行);也可`my_df[5 , 6]`来访问第5行第6列的元素.注意:在指定行列名称或行列索引时,可以混合使用:`df[5,"height"]`来访问第5行且列名为height的元素.  
- 因为数据框的列实质上就是向量,所以实际上就是用名称或索引指定要提取的向量,这允许一次获取多个行列.
- 也可以使用布尔值的向量来指定感兴趣的索引:`my_df[my_df$height > 70 , ]`就能筛选数据框中height大于70的所有行.
> 按列名(字符串)而不是列号进行筛选更容易\更干净\更不易出错.应该用列名指定列,并使用过滤器指定感兴趣的行.  
> **技巧:** 使用`is.data.frame()`函数来确认数据对象是数据框.也可使用`as.data.frame()`函数来将对象转换为数据框.

### 使用CSV数据:  
CSV只存储数据,用逗号分隔值.用`read.csv("文件名.csv" , stringsAsFactors = FALSE )`函数来加载数据.用`write.csv(数据框名 , "新建表名.csv" , row.names = FALSE)`来写入csv文件.  
> 注意:如果数据中缺少元素,R将用逻辑值NA填充该单元格,代表"不可用".[一些处理方法](statmethods.net/input/missingdata.html)  
> [R软件包数据集](vincentarelbundock.github.io/Rdatasets/datasets.html)  

### 因子变量
> 注意:在加载或创建数据帧时,应该**始终包含`stringsAsFactors = FALSE`参数.  

因子是一种数据结构,用于优化由一组优先类别组成的变量(即类别变量).这将节省大量内存.factor将每个字符串转存为一个数字(level),并记住数字与其标签(原始数据字符串)之间的关系.  
**因子不是向量**,这意味着在向量上使用的大多数操作和函数都无法对因子起效.  
> 如果创建一个以字符串向量作为列的数据框,如使用read.csv()函数时,字符串向量将被自动视为一个因子,**除非明确告诉它不是!**.   


## 第十一章 使用dplyr操作数据
### 操作数据语法
dplyr包提供了一组**动词(函数)**来描述和执行常见的数据准备工作:   

- **Select**:选择,从数据集中选择感兴趣的特征值(列);
- **Filter**:过滤,过滤掉不相关的数据,只保留感兴趣的观察值(行);
- **Mutate**:修改,向数据集添加更多特征(列);
- **Arrange**:排序,按特定顺序排列观察结果(行);
- **Summarize**:汇总,根据集合汇总数据;
- **Join**:连接,将多个数据集连接到一个数据框中.  

### 核心dplyr函数
#### Select()函数|提取【列】  
select()函数的参数列表中,指定数据框的列时没有引号,也就是**将列名称作为变量名**,而不是字符串,这被称为非标准评估(NSE).这使得dplyr更易编写和读取,但在使用存储在变量中的列名时,有可能遇到错误.若遇到错误,则改用baseR语法(如美元符号和括号符号)    
**辨别:**selcet()选择单个列也会返回数据框,(对比之下,用括号表示法选择单列则会返回向量).如果要从数据框选择特定的列或值,可以用baseR语法或者用dplyr中的`pull()`函数.**通常,使用dplyr操作数据框,使用baseR引用数据框中的特定值**.  

```{r}
library(pscl)
names(presidentialElections) #1097 obs , of 4 variables
votes <- select(presidentialElections,year,demVote)  # 列名year\demVote不带引号!
votes <- presidentialElections[,c("year","demVote")]  # baseR语法可以实现等价操作!

select(presidentialElections,state:year)
select(presidentialElections, -demVote)
```

#### Fliter()函数|提取【行】
**注意：**如果使用的数据框有行名称，**执行dplyr函数时会删除行名称**。如果想要保留这些行名称，可以考虑将其作为特征值存储于数据框的一列，以便能够在清理和分析中包含这些名称。可以使用`mutate()`函数将行名添加为列.
```{r}
votes_2008 <- filter(presidentialElections,year == 2008)
votes_2008 <- presidentialElections[presidentialElections$year == 2008, ] #与上句等价
votes <- colorado_2008 <- filter(presidentialElections,year == 2008,state == "Colorado")
```

#### mutate()函数|修改【列】
mutate()函数实际上没有更改数据框，只是返回一个添加了额外列的**新数据框**，通常我们用新数据框顶替了旧数据框。
```{r}
#将行名rownames存储为dataframe中的一列。因为dplyr操作之后，行名会丢失，所以要备份。
presidentialElections <- mutate(presidentialElections,row_names = rownames(presidentialElections))

presidentialElections <- mutate(
  presidentialElections,
  other_parties_vote = 100-demVote,
  abs_vote_diffrence = abs(demVote - other_parties_vote)
)
```
**重命名列：**使用dplyr包提供的**rename()**函数,rename函数实际上是将命名参数传递给select()函数以选择要重命名的列.

#### arrange()函数|排序【行】
arrange()函数允许**根据一些特征值(列值),对数据框中的[行]进行排序**.默认按照**递增的顺序**进行排序.若想要递减排序,则使用`desc()`函数或在列名前面加负号`-`.
```{r}
#按年份递减（加负号）；按demVote递增排列
presidentialElections <- arrange(presidentialElections, -year , demVote)
```

#### summarize()函数|汇总
summarize()函数的作用:生成一个包含汇总值的数据框,可以使用baseR或pull()进行**引用**.  
summarize()可以接受"任何以向量为阐述,返回值为单个值的函数"以实现对列的聚合.例如mean()\max()\median()等,也可自己编写汇总函数.
```{r}
average_votes <- summarize(presidentialElections,
                           mean_dem_vote = mean(demVote),
                           mean_other_parties = mean(other_parties_vote))

#构建函数,在向量中查找距离50最远的元素.
furthest_from_50 <- function(vec){
  adjusted_value <- vec-50
  vec[abs(adjusted_value)==max(abs(adjusted_value))]
}
summarize(presidentialElections,
          biggest_landslide = furthest_from_50(demVote))
#结果:查找投票支持率距离50%最远的选举.
```

### 管道|执行顺序操作
目标:使用一个函数的结果,作为下一个函数的参数.  
> 不好的方法:(1)产生很多中间变量;(2)匿名变量(函数结果不分配变量,而是直接作用于其他函数的参数)  

管道运算符`%>%`(快捷键Ctrl+Shift+M)可以实现这个目标!为了便于调试,最好把管道序列的每个步骤单独放在自己的行上.
```{r}
most_dem_state <- presidentialElections %>% 
  filter(year == 2008) %>% 
  filter(demVote == max(demVote)) %>% 
  select(state)
```
> 管道符向右传递参数!presidentialElections数据框作为filter函数的第一个参数,直接被传入第一个filter函数中,然后依次进行运算并向后传递.  


### 按组分析
`group_by()`函数允许在数据框中的行组之间创建关联.不会显式地将数据分成不同的块或箱,就可以对数组进行操作,函数返回一个tibble,这是tidyverse使用的数据框版本,它能跟踪同一变量中的"子集(组)".使用`group_by()`对数据框进行分组后,可以将其他函数(如summarize()\filter())应用于该tibble,它们将**自动应用于每个组(就像按组分成一些数据框一样)**.
```{r}
grouped <- group_by(presidentialElections , state)
# presidentialElections的类型是tbl_df ,而grouped的类型是grouped_df

state_voting_summary <- presidentialElections %>% 
  group_by(state) %>% 
  summarize(mean_dem_vote=mean(demVote),mean_other_parties=mean(other_parties_vote))
```
意义：这种分组形式允许快速比较不同的数据子集，重新定义了数据分析单元，分组允许根据观测组而不是单个观测结果来分析问题。使询问和回答有关数据的复杂问题更容易了。  

### 连接数据框  
当我们需要访问两个数据集中的信息，同时引用两个数据框中的值，这叫做组合数据框。将两个数据框连接后，可以识别两个表中的列，并用这些列相互匹配对应的行。这些列值作为确定每个表中哪些行相互对应的标识符，进而将对应行组合成连接表中的一行。  
左连接（`left_join()`)使最常见的连接函数之一.实际上就是合并表(返回新的数据框包括第一个(左)数据框的全部列,再加上第二个(右)数据框的其他列).   
[dplyr连接函数备忘单](stat545.com/bit001_dplyr-cheatsheet.html)

### 【章末实操】dplyr in Action: Analyzing Flight Data
```{r}
library("nycflights13")          # in each relevant script
library("dplyr")                 # load the dplyr library
library("ggplot2")               # for plotting

?flights          # read the available documentation
dim(flights)      # check the number of rows/columns
colnames(flights) # inspect the column names

# Q1：哪家公司延迟起飞的次数最多？
# Identify the airline (`carrier`) that has the highest number of delayed flights
has_most_delays <- flights %>%            # start with the flights
  group_by(carrier) %>%                   # group by airline (carrier)
  filter(dep_delay > 0) %>%               # find only the delays
  summarize(num_delay = n()) %>%          # count the observations
  filter(num_delay == max(num_delay)) %>% # find most delayed
  select(carrier)                         # select the airline

#上面得到的是公司简写“UA”，现在从airlines数据框里获取公司全称
most_delayed_name <- has_most_delays %>%  # start with the previous answer
  left_join(airlines, by = "carrier") %>% # join on airline ID
  select(name)                            # select the airline name
print(most_delayed_name$name) # access the value from the tibble



#Q2：平均来看，航班最早到达哪个机场？【建议：逐句检查，看其中是否有问题，不要一口气跑到最后】
# Calculate the average arrival delay (`arr_delay`) for each destination (`dest`)
most_early <- flights %>%
  group_by(dest) %>% # group by destination 按机场destination进行分组
  summarize(delay = mean(arr_delay)) # compute mean delay 分组计算各组达到延迟的平均值

# 【对上句进行调整：移除NA值】Compute the average delay by destination airport, omitting NA results
most_early <- flights %>%
  group_by(dest) %>% # group by destination
  summarize(delay = mean(arr_delay, na.rm = TRUE)) # compute mean delay

# Identify the destination where flights, on average, arrive most early
most_early <- flights %>%
  group_by(dest) %>% # group by destination
  summarize(delay = mean(arr_delay, na.rm = TRUE)) %>% # compute mean delay, ignore NA
  filter(delay == min(delay, na.rm = TRUE)) %>% # filter for the *least* delayed
  select(dest, delay) %>% # select the destination (and delay to store it)
  left_join(airports, by = c("dest" = "faa")) %>% # join on `airports`data frame
  select(dest, name, delay) # select output variables of interest

print(most_early)



# Q3:航班在哪个月延迟最久？
# 【不转存数据，最后直接传到print（）函数打印出来】Identify the month in which flights tend to have the longest delays
flights %>%
  group_by(month) %>% # group by selected feature
  summarize(delay = mean(arr_delay, na.rm = TRUE)) %>% # summarize value of interest
  filter(delay == max(delay)) %>% # filter for the record of interest
  select(month) %>% # select the column that answers the question
  print() # print the tibble out directly

# Compute delay by month, adding month names for visual display
# Note, `month.name` is a variable build into R
delay_by_month <- flights %>%
  group_by(month) %>%
  summarize(delay = mean(arr_delay, na.rm = TRUE)) %>%
  select(delay) %>%
  mutate(month = month.name)

# Create a plot using the ggplot2 package (described in Chapter 17)
ggplot(data = delay_by_month) +
  geom_point(
    mapping = aes(x = delay, y = month), 
    color = "blue",
    alpha = .4, 
    size = 3
  ) +
  geom_vline(xintercept = 0, size = .25) +
  xlim(c(-20, 20)) +
  scale_y_discrete(limits = rev(month.name)) +
  labs(title = "Average Delay by Month", y = "", x = "Delay (minutes)")
```


## 第十二章 使用tidyr重塑数据 
### 数据整洁之道  
tidyr包构建和使用数据框遵循三个整洁数据的规则：  
- 每个变量都在一列中
- 每次观测结果对应一行
- 每个值对应一个单元格  
### gather()|由宽转为长格式  
```
band_data_long <- gather(
  band_data_wide,
  key = band,      #宽型数据的带合并那几列的列名(作为新建band列里面的值)
  value = price,   #将宽型数据待整合的矩形数值(value)整理到新的长形数据框的新建列price作为列内的值
  -city            #指定了宽型数据中哪几列属于"待整合的数据",语法类似于select()函数
)
```
注意:gather的参数比较难记忆,可以尝试跟踪表中每个值移动的位置.  
> gather的第一个参数为待处理的数据框;第二个参数为key
### spread()|由长转为宽格式 
spread()函数可以根据现有的**两列**创建多个特征(列).函数的作用:为所提供的key列中的每一个唯一值创建一个新列,其中的值取自value列.
```
price_by_band <- spread(
  band_data_long,
  key = city,      #与上例不同,上例的"宽型"待处理列名为band,这里则想要将长型的city列里的值作为宽型的列名.
  value = price
)
```


**unite()**  
  将多个列合并为一个列。    
**separate()**  

### 实战|探索教育统计
- 数据:[世界银行浏览器](data.worldbank.org/education)下载的.
- 数据集的缺陷:(1).csv文件顶部不必要的行;(2)大量缺失数据;(3)带有特殊字符的长列名  
载入数据:  
```
# 跳过前四行废行。字符串不要读取为factor。
#注意：表格中列名只有数字的话，会自动加`X`字母前缀
wb_data <- read.csv(
  "data/world_bank_data.csv",
  stringsAsFactors = F,
  skip = 4
)
```




## 第十三章 访问数据库
### 关系数据库
**数据库**是用于保存、组织和访问信息的专门应用程序。直接从数据库访问数据避免了将所有数据放入内存的无所适从。计算机将不用同时保存对所有数据的引用，而是能够将数据操作（选择和过滤数据）应用于计算机硬盘上的数据。   
**关系数据库**：将数据组织到在概念和结构上与数据框类似的表中。表中每一行（也叫“记录”）表示单个“项”或观测结果；每一列（也叫“字段”）表示该项的单个数据属性。通过这种方式，数据框反映了R中的数据框，某种程度上，它们是等价的。但是关系数据库有很多不同的表组成，每个表都表示数据的不同方面（如一个表存储单只歌曲信息，一个表存储艺术家的信息）。  
**主键**：关系数据库特别之处就在于如何指定这些表之间的关系，表中每个记录（行）都被赋予一个被成为主键的字段（列）。主键是每一行的唯一值，因此可以通过主键来引用特定记录（行）。主键可以是任何唯一的识别符，但它们**几乎总是数字**，并且经常由数据库自动生成和分配。数据库不能只使用“行号”作为主键，因为记录（行）可能被添加删除或重新排序。   
**外键**：可以将外键看作是为`join()`函数的`by`参数定义一个匹配列的固定方法.使用外键可以将表连接在一起。   
**桥接表**（bridge table）数据库中只包含与其他表相连接的外键，其记录表示两外两个表之间的关联，可以将此视为“关联其他表的表”。  
**软件** 几个流行的关系数据库管理系统（RDMS）：
- SQLite：最简单的SQL数据库系统。工程实践中少用，常用于测试和开发。[SQL下载](sqlite.org/index.html);SQLite的[DB浏览器](sqlitebrowser.org)  
- PostgreSQL:缩写作[Postgres](postgresql.org)是免费开源的RDMS,更完整全面.计算机上将安装管理器,并提供一个图形化的应用程序pgAdmin来管理数据库.  
- MySQL:免费但不开源的RDMS,比上一个产不多,但更受欢迎.从[官网](mysql.com)下载并安装`Community Server Edition`就行,不需要注册账户.  

## SQL语句
SQL(结构化查询语言)是一种用于查询(访问)数据库的结构化编程语言,专门用于管理关系数据库中的数据.完全可以**不使用SQL而通过R访问和操作数据库**．但本节就是要了解R操作的底层命令，并且SQL有助于项目合作．　　
> w3schools为新搜提供了友好的[SQL语法和用法教程](w3schools.com/sql/default.asp)  

SQL中注释单独占一行用`/*     */`囊括在中间.  
`SELECT columnname FROM tablename`  左边的SQL相当于R中的  `select(tablename,columnname)`
(SQL不区分大小写,习惯性地将诸如SELECT这样的关键字所有字母都大写,而列名和表名则小写.)  
> SQL中,可以用逗号隔开列名,达到选择多个列的目的.  

使用`*`号表示"所有内容"(**通配符**):`SELECT * FROM tablename`将返回表中的所有列,即加载整个表.   

使用`AS`作为关键词,**为结果列指定一个新名称**(类似于mutate操作).它不改变表,只改变查询结果"子表"的列标签.
`SELECT id AS song_id FROM songstable` 表示从songstable表中取出id列,并命名其为song_id.   

使用`WHERE`子句进行**筛选**操作:`SELECT columnname FROM tablename WHERE artist_id = 11` ,筛选条件将应用于整个表,而不仅仅是所选列.SQL中筛选发生在所选内容之前.    
相当于r中的dplyr语句`filter(tablename,artist_id == 11)%>%select(columnname)`    

使用`JOIN`子句实现**跨多个表存储和查询数据(内连接)**:`SELECT columns FROM artists JOIN songs ON songs.artist_id = artists.id`  
JOIN子句类似于dplyr中的**内连接**操作,即只返回两个表中匹配成功的行,连接后的表中将没有不匹配的行.也可以指定执行左连接(LEFT JOIN);右连接(RIGHT JOIN);外连接(OUTER JOIN)也叫完全连接.    

## 从R访问数据库
使用R包可以直接连接和查询数据库,而不需要通过RDMS提供的解释器来执行SQL.  
最简单的r包是`dbplyr`包,作为tidyverse的一部分,允许使用dplyr函数查询关系数据库,避免了使用外部应用程序.dbplyr是一个外部包,但实际上是dplyr的"**后端**"(提供了dplyr处理数据库的后台代码),而实际使用的就是dplyr中的函数,因此安装后不需加载,只要**加载dplyr即可.还需要加载DBI包**,该包与dbplyr一起安装,允许我们连接到数据库.    
```{r}
library(dplyr)
library(DBI)
```
还需要根据带访问的**数据库类型**再安装一个附加包,这些包提供了跨多个数据库格式的公共接口(函数集)并允许使用相同的R函数访问SQLite数据库和Postgres数据库.
```{r}
library(RSQLite)
library(RPostgreSQL)  
```
数据库是通过RDMS管理和访问的,RDMS是独立于R解释器的程序,因此需要"连接"到外部RDMS程序,并使用R通过它发出的语句,才能通过R访问数据库.可以使用`DBI`包提供的`dbConnect()`函数连接到外部数据库,记住用完数据库后要断开连接:  
```{r}
db_connection <- dbConnect(SQLite(),dbname = "path/to/database.sqlite")

dbDisconnect(db_connection)
```
> dbConnect()函数介绍:第一个参数是"数据库连接包如RSQLite提供的'连接'接口";其余参数指定数据库位置并且取决于它是那种类型的数据库以及数据库在哪里.如`dbname`指定本地SQLite数据库文件的路径;`host`,`user`,`password`指定连接到远程计算机上的数据库.不要将数据库密码包含在R脚本中:通过RStudio使用`rstudioapi`包中的**askForPassword()**函数提示用户输入密码(将弹出一个窗口,供用户键入密码).参见[dbplyr简介](cran.r-project.org/web/packages/dbplyr/vignettes/dbplyr.html)  

连接到数据库后,使用`dbListTables()`函数获取包含所有表名的向量,这对检查是否已连接到数据库以及查看哪些数据可用很有帮助.  
因为所有SQL查询都是访问特定表中的数据,因此需要先为该表创建变量形式的引用.可以使用`dplyr`提供的**tbl()**函数,此函数参数包括与数据库的连接以及要引用的表的名称.例如查询songs表:`songs_table <- tbl(db_connection, "songs")`此时打印songs_tabe表就像是普通的数据框,接下来就可以使用dplyr函数进行操作,只需使用表代替数据框进行操作.  
dbplyr包将自动将dplyr函数转换为等效的SQL语句,而无须编写任何SQL!
```{r}
queen_songs_query <- songs_table %>% filter(artist_id == 11) %>% select(title) 
```
使用`show_query()`可以看到上面这句代码所暗地里生成的SQL语句.   
另外,在表上使用dplyr方法不会返回数据框,只是显示所请求数据的小预览.因为dbplyr使用**惰性求值**,只有在被明确告知需要查询时,才对数据库执行查询操作.打印queen_songs_query时显示的内容只是数据的一个子集.惰性求值防止意外地进行大量的查询和下载大量的数据.   

使用`collect()`函数**实际查询数据库并将结果作为可操作的R值加载到内存中**.通常将collect()函数的调用添加为dplyr调用管道中的最后一步.    

**总结:通过R从数据库中查询数据的步骤**  
```{r}
db_connection <- dbConnect(SQLite(),dbname="path/to/database.sqlite")

some_table <- tbl(db_connection, "TABLE_NAME")

db_query <- some_table %>% filter(some_column == some_value)

results <- collect(db_query)

dbDisconnect(db_connection)
```
(1)创建一个指向RDMS的连接;  
(2)为数据库创建变量形式的引用;  
(3)使用dplyr语法对引用的表进行操作;  
(4)让操作实际发生;  
(5)断开数据库连接  


## 第十四章 访问Web API
R脚本通过Web服务提供的应用程序编程接口(API)访问数据。许多Web服务遵循名为表述性状态传递（REST）的特定样式，本章介绍了如何通过RESTful API访问和使用数据。  
Interface（可以调用以访问数据的“函数集”）采用HTTP请求（即遵循超文本传输协议发送数据的请求）的形式使用Web服务。   

**URI**：统一资源标识符  
**URL**：统一资源定位器  



