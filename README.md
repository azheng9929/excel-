# excel-
基于python的csv处理与可视化分析

# 一、数据初步了解与分析
-----  
## 1.读取数据  
```python
filename = '../0.data_files/data_house_bj_sh.csv'
df = pd.read_csv(filename,header=1)
```
## 2.查看数据类型
 `df.info()`
> RangeIndex: 6000 entries, 0 to 5999
> Data columns (total 18 columns):
>>  #   Column                             Non-Null Count  Dtype  
>> ---  ------                             --------------  -----  
>>  0   _id                                6000 non-null   object 
>>  1   bathroom_num                       6000 non-null   int64  
>>  2   bedroom_num                        6000 non-null   int64  
>>  3   bizcircle_name                     6000 non-null   object 
>>  4   city                               6000 non-null   object 
>>  5   dist                               6000 non-null   object 
>>  6   distance                           3956 non-null   float64
>>  7   frame_orientation                  6000 non-null   object 
>>  8   hall_num                           6000 non-null   int64  
>>  9   house_tag                          5163 non-null   object 
>>  10  house_title                        6000 non-null   object 
>>  11  latitude                           5988 non-null   float64
>>  12  longitude                          5988 non-null   float64
>>  13  layout                             6000 non-null   object 
>>  14  rent_area                          6000 non-null   int64  
>>  15  rent_price_listingrent_price_unit  6000 non-null   object 
>>  16  resblock_name                      6000 non-null   object 
>>  17  type                               6000 non-null   object 
> dtypes: float64(3), int64(4), object(11)
> memory usage: 586.0+ KB
> None
## 3.查看数据量
`df.shape`
> (6000, 18)  
## 4.查看各数字类型特征的统计量
`df.describe()`
>        bathroom_num  bedroom_num  ...    longitude    rent_area
> count   6000.000000  6000.000000  ...  5988.000000  6000.000000
> mean       1.379167     2.210000  ...   118.927565    98.889667
> std        0.736313     0.993343  ...     2.516650    64.434865
> min        0.000000     1.000000  ...   116.068850     6.000000
> 25%        1.000000     2.000000  ...   116.420712    58.000000
> 50%        1.000000     2.000000  ...   116.722221    85.000000
> 75%        2.000000     3.000000  ...   121.446008   118.000000
> max        9.000000     9.000000  ...   121.923456   949.000000
>
> [8 rows x 7 columns]  

由此可知：
* 数据一共6000行，18列
* 占用内存586kb
* 数据类型上：3列float64类型，4列int64类型,11列objest类型
* 数据项有缺失，但除distance外缺失不多，distance项可舍去
* rent_area项存在明显偏移值，可舍去
总体而言，数据的质量还是不错的，无需特别清洗
-----
# 二、数据清洗
## 1.去重
```python
data = df.drop_duplicates(inplace=False) #去重
data.reset_index(inplace=True,drop = True)
print(df.shape)
```
> (6000, 18)
## 2.缺省值处理
```python
df.distance=df.distance.fillna(-1)#距离地铁 缺省值置-1
df.house_tag=df.house_tag.fillna("无")#房屋标签 缺省值置”无“
df=df.fillna(-1)#剩余经纬度缺省值置-1
```
> (6000, 18)
> _id                                  False
> bathroom_num                         False
> bedroom_num                          False
> bizcircle_name                       False
> city                                 False
> dist                                 False
> distance                             False
> frame_orientation                    False
> hall_num                             False
> house_tag                            False
> house_title                          False
> latitude                             False
> longitude                            False
> layout                               False
> rent_area                            False
> rent_price_listingrent_price_unit    False
> resblock_name                        False
> type                                 False
> dtype: bool
> 
> Process finished with exit code 0
可以看出，经处理后，已经没有缺省值
## 3.数据格式化
```python
df.rent_price_listingrent_price_unit=df['rent_price_listingrent_price_unit'].apply(lambda x:x[:-3])#去除房租单位
df.rent_price_listingrent_price_unit=df['rent_price_listingrent_price_unit'].astype('int64')#将房租列格式转换为int64 便于计算
```
-----
# 三、数据可视化分析
## 1.输入数据 调用数据处理包
```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
filename = '../0.data_files/cleaned_data_house_bj_sh.csv'
df = pd.read_csv(filename,skiprows=[0],names=names,na_values=miss_value)
```
## 2.可视化
```python
plt.savefig('../5.results/北京各区域房租箱线图.png')#存储图片
plt.show()#展示图片
```
### 2.1 北京上海各区域房租价格可视化
```python
"""各区域出租房平均单价"""
#数据分组、数据运算和聚合
groups_unitprice_area = df["rent_price_listingrent_price_unit"].groupby(df[df.city=='上海'].dist)
mean_unitprice = groups_unitprice_area.mean()
mean_unitprice.index.name = "各区域名称"

fig = plt.figure(figsize=(16,9))
ax = fig.add_subplot(111)
ax.set_ylabel("单价(元/月)")
ax.set_title("上海各区域平均房租价格")
mean_unitprice.plot(kind="bar")
```
![北京各区域平均房租价格](/5.results/北京各区域平均房租价格.png)

# 一、数据初步了解与分析
-----  
## 1.读取数据  
```python
filename = '../0.data_files/data_house_bj_sh.csv'
df = pd.read_csv(filename,header=1)
```
## 2.查看数据类型
 `df.info()`
> RangeIndex: 6000 entries, 0 to 5999
> Data columns (total 18 columns):
>>  #   Column                             Non-Null Count  Dtype  
>> ---  ------                             --------------  -----  
>>  0   _id                                6000 non-null   object 
>>  1   bathroom_num                       6000 non-null   int64  
>>  2   bedroom_num                        6000 non-null   int64  
>>  3   bizcircle_name                     6000 non-null   object 
>>  4   city                               6000 non-null   object 
>>  5   dist                               6000 non-null   object 
>>  6   distance                           3956 non-null   float64
>>  7   frame_orientation                  6000 non-null   object 
>>  8   hall_num                           6000 non-null   int64  
>>  9   house_tag                          5163 non-null   object 
>>  10  house_title                        6000 non-null   object 
>>  11  latitude                           5988 non-null   float64
>>  12  longitude                          5988 non-null   float64
>>  13  layout                             6000 non-null   object 
>>  14  rent_area                          6000 non-null   int64  
>>  15  rent_price_listingrent_price_unit  6000 non-null   object 
>>  16  resblock_name                      6000 non-null   object 
>>  17  type                               6000 non-null   object 
> dtypes: float64(3), int64(4), object(11)
> memory usage: 586.0+ KB
> None
## 3.查看数据量
`df.shape`
> (6000, 18)  
## 4.查看各数字类型特征的统计量
`df.describe()`
>        bathroom_num  bedroom_num  ...    longitude    rent_area
> count   6000.000000  6000.000000  ...  5988.000000  6000.000000
> mean       1.379167     2.210000  ...   118.927565    98.889667
> std        0.736313     0.993343  ...     2.516650    64.434865
> min        0.000000     1.000000  ...   116.068850     6.000000
> 25%        1.000000     2.000000  ...   116.420712    58.000000
> 50%        1.000000     2.000000  ...   116.722221    85.000000
> 75%        2.000000     3.000000  ...   121.446008   118.000000
> max        9.000000     9.000000  ...   121.923456   949.000000
>
> [8 rows x 7 columns]  

由此可知：
* 数据一共6000行，18列
* 占用内存586kb
* 数据类型上：3列float64类型，4列int64类型,11列objest类型
* 数据项有缺失，但除distance外缺失不多，distance项可舍去
* rent_area项存在明显偏移值，可舍去
总体而言，数据的质量还是不错的，无需特别清洗
-----
# 二、数据清洗
## 1.去重
```python
data = df.drop_duplicates(inplace=False) #去重
data.reset_index(inplace=True,drop = True)
print(df.shape)
```
> (6000, 18)
## 2.缺省值处理
```python
df.distance=df.distance.fillna(-1)#距离地铁 缺省值置-1
df.house_tag=df.house_tag.fillna("无")#房屋标签 缺省值置”无“
df=df.fillna(-1)#剩余经纬度缺省值置-1
```
> (6000, 18)
> _id                                  False
> bathroom_num                         False
> bedroom_num                          False
> bizcircle_name                       False
> city                                 False
> dist                                 False
> distance                             False
> frame_orientation                    False
> hall_num                             False
> house_tag                            False
> house_title                          False
> latitude                             False
> longitude                            False
> layout                               False
> rent_area                            False
> rent_price_listingrent_price_unit    False
> resblock_name                        False
> type                                 False
> dtype: bool
> 
> Process finished with exit code 0
可以看出，经处理后，已经没有缺省值
## 3.数据格式化
```python
df.rent_price_listingrent_price_unit=df['rent_price_listingrent_price_unit'].apply(lambda x:x[:-3])#去除房租单位
df.rent_price_listingrent_price_unit=df['rent_price_listingrent_price_unit'].astype('int64')#将房租列格式转换为int64 便于计算
```
-----
# 三、数据可视化分析
## 1.输入数据 调用数据处理包
```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
filename = '../0.data_files/cleaned_data_house_bj_sh.csv'
df = pd.read_csv(filename,skiprows=[0],names=names,na_values=miss_value)
```
## 2.可视化
```python
plt.savefig('../5.results/北京各区域房租箱线图.png')#存储图片
plt.show()#展示图片
```
### 2.1 北京上海各区域房租价格可视化
```python
"""各区域出租房平均单价"""
#数据分组、数据运算和聚合
groups_unitprice_area = df["rent_price_listingrent_price_unit"].groupby(df[df.city=='上海'].dist)
mean_unitprice = groups_unitprice_area.mean()
mean_unitprice.index.name = "各区域名称"

fig = plt.figure(figsize=(16,9))
ax = fig.add_subplot(111)
ax.set_ylabel("单价(元/月)")
ax.set_title("上海各区域平均房租价格")
mean_unitprice.plot(kind="bar")
```
![北京各区域平均房租价格](/5.results/北京各区域平均房租价格.png)
![上海各区域平均房租价格](/5.results/上海各区域平均房租价格.png)
可以看出北京海淀、朝阳以及西城区房租较高，月租金均在一万以上，上海平均月租金超过一万的区域更多，黄埔平均月租金更是超过了17500
### 2.2北京上海各区域出租房平均建筑面积
```python
#数据运算
groups_area_jzmj = df["rent_area"].groupby(df[df.city=='上海'].dist)
mean_jzmj = groups_area_jzmj.mean()
mean_jzmj.index.name = "各区域名称"

#数据可视化
fig = plt.figure(figsize=(16,9))
ax = fig.add_subplot(111)
ax.set_ylabel("出租房面积(㎡)")
ax.set_title("上海各区域出租房平均建筑面积")
mean_jzmj.plot(kind="bar")
```
![北京各区域出租房平均建筑面积](/5.results/北京各区域出租房平均建筑面积.png)
![上海各区域出租房平均建筑面积](/5.results/上海各区域出租房平均建筑面积.png)
### 2.3各区域平均月租金和平均建筑面积
```python
groups_unitprice_area = df["rent_price_listingrent_price_unit"].groupby(df[df.city=='上海'].dist)
mean_unitprice = groups_unitprice_area.mean()
mean_unitprice.index.name = ""

groups_area_jzmj = df["rent_area"].groupby(df[df.city=='上海'].dist)
mean_jzmj = groups_area_jzmj.mean()
mean_jzmj.index.name = "各区域名称"

fig = plt.figure()
ax1 = fig.add_subplot(2,1,1)
ax1.set_ylabel("单价(元/平米)")
ax1.set_title("上海各区域平均月房租")
ax2 = fig.add_subplot(2,1,2)
ax2.set_ylabel("建筑面积(㎡)")
ax2.set_title("上海各区域出租房平均建筑面积")
plt.subplots_adjust(hspace=0.4)

mean_unitprice.plot(kind="bar",ax=ax1)
mean_jzmj.plot(kind="bar",ax=ax2)
```
![北京各区域平均月房租和平均出租房面积](/5.results/北京各区域平均月房租和平均出租房面积.png)
![上海各区域平均月房租和平均出租房面积](/5.results/上海各区域平均月房租和平均出租房面积.png)

### 2.4各区域出租房房源数量
```python
groups_area = df["_id"].groupby(df[df.city=='上海'].dist)
count_area = groups_area.count()
count_area.index.name = "各区域名称"

fig = plt.figure(figsize=(12,7))
ax = fig.add_subplot(111)
ax.set_ylabel("房源数量(套)")
ax.set_title("上海各区域出租房房源数量")
count_area.plot(kind="bar")
```
![上海各区域出租房房源数量](/5.results/上海各区域二手房房源数量.png)
![北京各区域出租房房源数量](/5.results/北京各区域二手房房源数量.png)

### 2.5出租房单价最高top10
```python
unitprice_top = df[df.city=='北京'].sort_values(by="rent_price_listingrent_price_unit",ascending=False)[:10]
unitprice_top.set_index(unitprice_top["resblock_name"],inplace=True)
unitprice_top.index.name = ""

fig = plt.figure(figsize=(12,7))
ax = fig.add_subplot(111)
ax.set_ylabel("单价(元/月)")
ax.set_title("北京出租房房租最高Top10")
unitprice_top["rent_price_listingrent_price_unit"].plot(kind="bar")
```
![上海出租房单价最高top10](/5.results/上海出租房单价最高top10.png)
![北京出租房单价最高top10](/5.results/北京出租房单价最高top10.png)
高达17万一个月
### 2.6两地出租房房屋户型占比情况
```python
count_fwhx = df[df.city=='上海'].layout.value_counts()[:10]
count_other_fwhx = pd.Series({"其他":df['layout'].value_counts()[10:].count()})
count_fwhx = count_fwhx.append(count_other_fwhx)
count_fwhx.index.name = ""
count_fwhx.name = ""

fig = plt.figure(figsize=(8,8))
ax = fig.add_subplot(111)
ax.set_title("上海出租房房屋户型占比情况")
count_fwhx.plot(kind="pie",cmap=plt.cm.rainbow,autopct="%3.1f%%")
```
![上海出租房房屋户型占比情况](/5.results/上海出租房房屋户型占比情况.png)
![北京出租房房屋户型占比情况](/5.results/北京出租房房屋户型占比情况.png)
### 2.7 出租房房屋朝向分布情况
```python
count_fwcx = df[df.city=='上海'].frame_orientation.value_counts()[:15]
count_other_fwcx = pd.Series({"其他":df['frame_orientation'].value_counts()[15:].count()})
count_fwcx = count_fwcx.append(count_other_fwcx)

fig = plt.figure(figsize=(12,7))
ax = fig.add_subplot(111)
ax.set_title("上海出租房屋朝向")
count_fwcx.plot(kind="bar")
```
![上海出租房房屋朝向分布情况](/5.results/上海出租房屋朝向.png)
![北京出租房房屋朝向分布情况](/5.results/北京出租房屋朝向.png)

### 2.8上海出租房面积分布区间
```python
area_level = [0, 50, 100, 150, 200, 250, 300, 500,1000]
label_level = ['小于50', '50-100', '100-150', '150-200', '200-250', '250-300', '300-500','500-1000']
jzmj_cut = pd.cut(df[df.city=='上海'].rent_area, area_level, labels=label_level)
jzmj_result = jzmj_cut.value_counts()
#jzmj_result = jzmj_result.sort_values()

fig = plt.figure(figsize=(12,7))
ax = fig.add_subplot(111)
ax.set_ylabel("出租房面积",fontsize=14)
ax.set_title("上海出租房面积分布区间",fontsize=18)
jzmj_result.plot(kind="bar",fontsize=12)
```
![上海出租房面积分布区间](/5.results/上海出租房面积分布区间.png)
![北京出租房面积分布区间](/5.results/北京出租房面积分布区间.png)

### 2.9房租与建筑面积散点图
```python
fig = plt.figure(figsize=(16,9))
ax = fig.add_subplot(111)
ax.set_title("上海出租房总价与建筑面积散点图",fontsize=18)
df[df.city=='上海'].plot(x="rent_area", y="rent_price_listingrent_price_unit", kind="scatter",fontsize=10,ax=ax,alpha=0.4,xticks=[0,50,100,150,200,250,300,400,500,600],xlim=[0,600])
ax.set_xlabel("出租面积（平方米）",fontsize=14)
ax.set_ylabel("房租价格（平/月）",fontsize=14)
```
![上海房租与建筑面积散点图](/5.results/上海出租房房租与建筑面积散点图.png)
![北京房租与建筑面积散点图](/5.results/北京出租房房租与建筑面积散点图.png)

### 2.10 各区域房租箱线图
```python
#数据分组、数据运算和聚合
box_unitprice_area = df[df.city == '北京'].rent_price_listingrent_price_unit.groupby(df["dist"])
flag = True
print(df.head())
box_data = pd.DataFrame(list(range(6000)),columns=["start"])
for name,group in box_unitprice_area:
    box_data[name] = group
del box_data["start"]

fig = plt.figure(figsize=(12,7))
ax = fig.add_subplot(111)
ax.set_ylabel("房租（元/月）",fontsize=14)
ax.set_title("北京各区域房租箱线图",fontsize=18)
box_data.plot(kind="box",fontsize=12,sym='r+',grid=True,ax=ax,yticks=[1000,2000,3000,5000,7000,9000,11000,14000,19000,25000,40000,60000,90000])
```
![上海各区域房租箱线图](/5.results/上海各区域房租箱线图.png)
![北京各区域房租箱线图](/5.results/北京各区域房租箱线图.png)
