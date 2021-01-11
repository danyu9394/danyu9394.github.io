---
title: python pandas cheatsheet
date: 2021-01-01 13:20:06
tags: [pyhth]
cover: /img/python_panda.png
---
Pandas基于两种数据类型：`series`与`dataframe`。

一个`series`是一个一维的数据类型，其中每一个元素都有一个标签。`series`类似于`Numpy`中元素带标签的数组。其中，标签可以是数字或者字符串。

使用`dataframe[column_name]`，返回`series`格式数据。 `series`序列数据类似于`list`，你可以近似等同于list。 只不过返回数据中会_多一列index索引_。

一个`dataframe`是一个二维的表结构。Pandas的dataframe可以存储许多种不同的数据类型，并且每一个坐标轴都有自己的标签。你可以把它想象成一个series的字典项。


### 1. <a name='dataframe'></a>查看data frame
```py
import pandas as pd
filepath = r'C:/Users/lenovo/Desktop/20180108-百度地图/20180108-百度地图/data.csv'
df = pd.read_csv(filepath)
print(df)
#############################################
#检测下数据格式是否为DataFrame
print(type(df))
#输出class 'pandas.core.frame.DataFrame
#############################################
df.values  # 查看所有元素
df.index   # 查看索引
df.columns # 查看所有列名
df.dtype   # 查看字段类型
df.size    # 元素总数
df.ndim    # 表的维度数
df.shape   # 返回表的行数与列数
df.info    # DataFrame的详细内容
df.T       # 表转置
#############################################
#展示列名
col_names = df.columns
print(col_names)
#查看下col_names格式
type(col_names)
#将col_names转化为list
col_list = col_names.tolist()

```
### 2. <a name='DataFrame'></a>查看访问DataFrame中的数据
##### 基本查看方式:
```py
##################################
# 取列数据
df['col1']  # 单列数据
df['col1'][2:7] # 单列多行
#这里我们一列，如取Name列数据前五行
df['Name'][:5]

df[['col1','col2']][2:7] #多列多行
df[:][2:7] #多行数据


#这里返回的数据还是dataframe格式，为了方便也只显示前几条记录
cols = ['name', 'province_name', 'city_name', 'city_code', 'area', 'addr']
df[cols]
##################################
## 取行数据
## ix[row, col] 中括号中第一个参数row是行参数，你想选择的数据行数。 第二个参数col是列参数，选择你想要的列数据项
#第一行所有数据
df.ix[0, :]

#第一行的某几列数据
col = ['Survived', 'Pclass', 'Sex']
df.ix[0, col]

#取多行数据，所有列。选择前5行，所有列.
df.ix[:5, :]

#取多行，某几列
col = ['Survived', 'Pclass', 'Sex']
df.ix[:5, col]

# 取第一行第一列
df.ix[0,0] 
# 第三行第七列
df.ix[2,6]

df.head() #前几行
df.tail() #后几行

#展示前两条记录（根据需要显示条数）
df.head(2)

#展示后三条记录
df.tail(3)
```
##### 缺失值处理
缺失值一般标记为NaN,处理办法如下
```py
df.dropna(axis)
  # 默认直接使用df.dropna()
  # axis=1,按照行进行缺失值处理
  # axis=0，按照列进行缺失值处理
df.dropna(axis=0,subset)
  # axis=0,按照列方向处理subset中的列缺失值
  # subset=[column]   subset含有一个或多个列名的的list

#按照列处理缺失值(为显示方便，只显示前5行)
df.dropna(axis=0)

#对指定列进行缺失值处理
df.dropna(axis=0,subset=['Sex','Age'])
```
##### 排序
```py
df.sort_values(col,inplace,ascending)
#col          对col列进行排序
#inplace      布尔型值，是否原地操作。
#             True时，操作结果覆盖掉原数据，原数据被修改
#             False时，新建一个新数据，原数据未被修改
#ascending    布尔型值。升序降序。 False降序，True升序

#对Age列进行降序操作，不修改原始数据
df.sort_values('Age',inplace=False,ascending=False)

```
##### 求平均值
###### 所有列的平均值信息

```py
df.mean()
```
###### 单个列的平均值
```py
df['Age'].mean()
```
##### 矢量化操作（批量操作）
一般对如list样式的数据批量操作，需要写循环，但是这样费时费力。 pandas基于numpy，可进行矢量化操作，一行就能完成复杂的循环语句，而且运行效率还很高。
```py
#对Age列批量加10
df['Age']+10).head

#对Age列批量减20
df['Age']-10
```
##### `loc`，`iloc`的查看方式（大多数时候建议用`loc`）
```oy
# loc[行索引名称或条件，列索引名称]
# iloc[行索引位置，列索引位置]
# 单列切片：
df.loc[:,'col1']
df.iloc[:,3]
# 多列切片：
df.loc[:,['col1','col2']]
df.iloc[:,[1,3]]
花式切片：
df.loc[2:5,['col1','col2']]
df.iloc[2:5,[1,3]]
条件切片：
df.loc[df['col1']=='245',['col1','col2']]
df.iloc[(df['col1']=='245').values,[1,5]]

```
### 3. <a name=''></a>更改某个字段的数据
```py
df.loc[df['col1']=='258','col1']=214
# 注意：数据更改的操作无法撤销，更改前最好对条件进行确认或者备份数据
```
### 4. <a name='-1'></a>增加一列数据
```py
df['col2'] = 计算公式/常量
```
### 5. <a name='-1'></a>删除数据
```py
# 删除某几行数据,inplace为True时在源数据上删除，False时需要新增数据集
df.drop(labels=range(1,11),axis=0,inplace=True)
# 删除某几列数据
df.drop(labels=['col1','col2'],axis=1,inplace=True)
```

### 6. <a name='-1'></a>创建透视表与交叉表
##### 使用pivot_table创建透视表
围绕index参数列，分析各个col2，aggfunc是np函数，当然这里的aggfunc也可以是自定义函数。
```py
pd.pivot_table(data,index='',columns='',values='',aggfunc='mean',
               fill_value=None,margins=True,dropna=False)
# data为表，index为行分组键，columns为列分组键，values为要聚合的字段，aggfunc为聚合
#  函数，默认为mean，fill_value为指定填充缺失值，margins为是否显示汇总，默认为True
#  dropna为是否删除全部为NaN的列，默认False
```
##### 使用crosstab创建交叉表
```py
dfcross = pd.pivot_table(index=df['col1'],columns=df['col2'],values=df['col3'],
          aggfunc='mean',colnames=None,rownames=None,margins=True,dropna=False,
          normalize=False)
# colnames表示列分组键名，无默认   rownames表示行分组键名，无默认
# normalize表示是否对数据进行标准化,默认False
```

### 7. <a name='-1'></a>导入数据
```py
pd.read_csv(filename) #从csv导入
pd.read_table(filename) #从分隔的文本文件导入
pd.read_excel(filename) #从excel文件导入
pd.read_sql(query, connection_object) #从SQL数据库读取
pd.read_json(json_string) #读取json格式的字符串、URL或文件
pd.read_html(url) #解析html的url，字符串或者文件，从一系列的dataframes提取table
pd.read_clipboard() #获取剪切板的内容，将其传递给read_table
pd.DataFrame(dict) #从dict获取DataFrame，键名为栏目名，值为一系列的列表
```
### 8. <a name='-1'></a>导出数据
```py
df.to_csv(filename) #写入csv文件
df.to_excel(filename) #写入excel文件
df.to_sql(table_name, connection_object) #写入SQL数据库(表)
df.to_json(filename) #以json文件的形式写入
df.to_html(filename) #保存成html格式
df.to_clipboard() #写进剪贴板
```
### 9. <a name='-1'></a>创建测试对象
```py
pd.DataFrame(np.random.rand(20,5)) # 生成5列20行的随机浮点数
pd.Series(my_list) # 用可迭代列表创造一列数据
df.index = pd.date_range('1900/1/30',periods=df.shape[0]) #增加一个日期索引
```
### 10. <a name='-1'></a>数学统计
```py
df.describe() #显示总体统计的汇总状况
df.mean() #返回所有列的平均值
df.corr() #返回Dataframe列之间的相关关系
df.count() #返回Dataframe列中的非空值数量
df.max() #返回Dataframe列中的最大值
df.min() #返回Dataframe列中的最低值
df.median() #返回Dataframe每列的中位数
df.std() #返回Dataframe每列的标准差

```
### 11. <a name='-1'></a>数据连接
```py
df1.append(df2) #将df1的数据添加在df2下方(列必须相同)
df.concat([df1,df2],axis=1) #将df2的数据加载df1右侧(行必须相同)
df1.join(df2,on=col1,how='inner') # SQL的方式加入列df1与列在df2其中对于行col具有相同的值。how参数可以为'left'，'right'，'outer'，'inner'

```
### 12. <a name='-1'></a>过滤，排序和分组
```py
df[df[col] > 0.5] #col列值大于0.5的行
df[(df[col] >0.5) & (df[col] <0.7)] #col列值大于0.5小于0.7的行
df.sort_values(col1) #按照col1进行升序进行排列
df.sort_values(col2,ascending=False) #根据col2进行降序排列
df.sort_values([col1,col2],ascending=[True,False]) #根据col1升序col2降序联合排列
df.groupby(col) #根据某列的值返回分组对象
df.groupby([col1,col2]) #根据多列的值返回分组对象
df.groupby(col1)[col2].mean() #根据col1值返回分组对象，求col2列的平均值
df.pivot_table(index=col1,values=[col2,col3],aggfunc=mean) 
#创建一个按col1分组的数据透视表，并计算col2和col3的平均值
df.groupby(col1).agg(np.mean) #查找每个唯一col1组的所有列的平均值
df.apply(np.mean) #给每一列都计算平均值
df.apply(np.max, axis=1) #找出每一行的最大值
```
### 13. <a name='-1'></a>数据清洗
```py
df.columns = ['a','b','c'] #重命名列
pd.isnull() #确认是否为空值，返回布尔数组
pd.notnull() #与上面相反
df.dropna() #删除所有包含null值的行记录
df.dropna(axis=1) #删除所有包含null值的列记录
df.dropna(axis=1,thresh=n) #删除所有包含少于n个非空值的行
df.fillna(s.mean()) #用平均值替换掉所有空值
s.astype(float) #将某series的数据转换成float的数据类型　
s.replace(1,'one') #将所有值等于１的替换为one
s.replace([1,3], ['one','three']) #将所有值等于１的替换为one,3替换为three
df.rename(columns=lambda x:x+1)　#取上一般性的标题名
df.rename(columns={'old_name':'new_name'}) #指定列名重命名
df.set_index('column_one')　#修改索引
```

### 14. <a name='-1'></a>常见函数
##### `shift()`

```py
DataFrame.shift(periods=1, freq=None, axis=0)
## periods：类型为int，表示移动的幅度，可以是正数，也可以是负数，
## 默认值是1,1就表示移动一次，注意这里移动的都是数据，而索引是不移动的，移动之后没有对应值的，就赋值为NaN。
## orig df
# index	value1
# A		0
# B		1
# C		2
# D		3

df.shift()
# index	value1
# A		NaN
# B		0
# C		1
# D		2

df.shift(2)
# index	value1
# A		NaN
# B		NaN
# C		0
# D		1

df.shift(-1)
# index	value1
# A		1
# B		2
# C		3
# D		NaN

```
##### `diff()`
diff函数是用来将数据进行某种移动之后与原数据进行比较得出的差异数据
```py
## orig df
# index	value1
# A		0
# B		1
# C		2
# D		3

df.diff()
# index	value1
# A		NaN
# B		1
# C		1
# D		1

df.diff(2)
# index	value1
# A		NaN
# B		NaN
# C		2
# D		2
df.diff(-1)
# index	value1
# A		-1
# B		-1
# C		-1
# D		NaN

## 怎样得到
# 1.首先会执行 						df.shift()
# 2. 然后再将该数据与原数据做差 	 df.shift()-df
```
##### `sort_values()`
`pandas`中的`sort_values()`函数原理类似于`SQL`中的`order by`，可以将数据集依照某个字段中的数据进行排序，该函数即可根据指定列数据也可根据指定行的数据排序。

|     参数    |                                                     说明                                                     |
|:-----------:|:------------------------------------------------------------------------------------------------------------:|
|      `by`     |                             指定列名(axis=0或’index’)或索引值(axis=1或’columns’)                             |
|     `axis`    | 若`axis=0`或’index’，则按照指定列中数据大小排序；若axis=1或’columns’，则按照指定索引中数据大小排序，默认axis=0 |
|  `ascending`  |                              是否按指定列的数组升序排列，默认为True，即升序排列                              |
|   `inplace`   |                           是否用排序后的数据集替换原来的数据，默认为False，即不替换                          |
| `na_position` |                                   `{‘first’,‘last’}`，设定缺失值的显示位置                                    |

```py
DataFrame.sort_values(by='##',axis=0,ascending=True, inplace=False, na_position='last')

#利用字典dict创建数据框
import numpy as np
import pandas as pd
df=pd.DataFrame({'col1':['A','A','B',np.nan,'D','C'],
                 'col2':[2,1,9,8,7,7],
                 'col3':[0,1,9,4,2,8]
})
print(df)

# >>>
#   col1  col2  col3
# 0    A     2     0
# 1    A     1     1
# 2    B     9     9
# 3  NaN     8     4
# 4    D     7     2
# 5    C     7     8

#依据第一列排序，并将该列空值放在首位
print(df.sort_values(by=['col1'],na_position='first'))
# >>>
#   col1  col2  col3
# 3  NaN     8     4
# 0    A     2     0
# 1    A     1     1
# 2    B     9     9
# 5    C     7     8
# 4    D     7     2

#依据第二、三列，数值降序排序
print(df.sort_values(by=['col2','col3'],ascending=False))
# >>>
#   col1  col2  col3
# 2    B     9     9
# 3  NaN     8     4
# 5    C     7     8
# 4    D     7     2
# 0    A     2     0
# 1    A     1     1

#根据第一列中数值排序，按降序排列，并替换原数据
df.sort_values(by=['col1'],ascending=False,inplace=True,
                     na_position='first')
print(df)
# >>>
#   col1  col2  col3
# 3  NaN     8     4
# 4    D     7     2
# 5    C     7     8
# 2    B     9     9
# 1    A     1     1
# 0    A     2     0

x = pd.DataFrame({'x1':[1,2,2,3],'x2':[4,3,2,1],'x3':[3,2,4,1]}) 
print(x)
# >>>
#    x1  x2  x3
# 0   1   4   3
# 1   2   3   2
# 2   2   2   4
# 3   3   1   1
#按照索引值为0的行，即第一行的值来降序排序
print(x.sort_values(by =0,ascending=False,axis=1))
# >>>
#    x2  x3  x1
# 0   4   3   1
# 1   3   2   2
# 2   2   4   2
# 3   1   1   3

```
### 15. <a name='-1'></a>遍历
##### `DataFrame.iterrows()`
将DataFrame的每一行迭代为`{索引，Series}`对，对`DataFrame`的列，用`row['cols']`读取元素
```py
# iterrows()返回值为tuple,(index,row)
for index,row in otu.iterrows():
  print index
  print row

for index, row in df.iterrows():
    print(index,row['mas'],row['num'])

#就是整行
for row in otu.iterrows():
  print row

```  
##### `DataFrame.itertuples()`
将`DataFrame`的每一行迭代为元祖，可以通过`row['cols']`对元素进行访问，方法一效率高
```py
for row in df.itertuples():
    print(getattr(row, 'mas'), getattr(row, 'num')) # 输出每一行
```
##### `DataFrame.iteritems()`
这种方法和上面两种不同，这个是按列遍历，将DataFrame的每一列迭代为`(列名, Series)`对，可以通过`row['cols']`对元素进行访问。
```py
for index, row in df.iteritems():
    print(index,row[0],row[1],row[2])
```