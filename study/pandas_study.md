## jupyter的安装与运行
```bash
# 安装jupyter

pip install jupyter
# 安装提示插件
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install --user --skip-running-check

#启动notebook server
jupyter notebook
```

##pandas基本使用


```python
import pandas as pd
df = pd.DataFrame({'gz':[5000,7000,9000,8500], 
                   'jxf':[60,84,98,91], 
                   'bz':['pass', 'good', 'excel', 'good']}, 
                  index = ['lw', 'xl', 'xz', 'lg'])
print(df.head())
```

          gz  jxf     bz
    lw  5000   60   pass
    xl  7000   84   good
    xz  9000   98  excel
    lg  8500   91   good
    

##pandas值的操作



```python
import pandas as pd
df = pd.DataFrame({'gz':[5000,7000,9000,8500], 
                   'jxf':[60,84,98,91], 
                   'bz':['pass', 'good', 'excel', 'good']}, 
                  index = ['lw', 'xl', 'xz', 'lg'])
df['jxf'] += 100;#列增加100
df['gz'][0] += 1000;
df['bz'][0] += 'caibi';

print(df.head())
```

          gz  jxf         bz
    lw  6000  160  passcaibi
    xl  7000  184       good
    xz  9000  198      excel
    lg  8500  191       good
    

    <ipython-input-4-92e0ce9d4dfa>:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['gz'][0] += 1000;
    <ipython-input-4-92e0ce9d4dfa>:8: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['bz'][0] += 'caibi';
    

##pandas导入导出文件




```python
#导出到excel
import pandas as pd
df = pd.DataFrame({'gz':[5000,7000,9000,8500], 
                   'jxf':[60,84,98,91], 
                   'bz':['pass', 'good', 'excel', 'good']}, 
                  index = ['lw', 'xl', 'xz', 'lg'])
df.to_excel("pandasTest/hellworld.xls");
print("success")


```

    success
    


```python
#导出到csv
import pandas as pd
df = pd.DataFrame({'gz':[5000,7000,9000,8500], 
                   'jxf':[60,84,98,91], 
                   'bz':['pass', 'good', 'excel', 'good']}, 
                  index = ['lw', 'xl', 'xz', 'lg'])
df.to_csv("pandasTest/hellworld.csv");
print("success")
```

    success
    


```python
#导出到json
import pandas as pd
df = pd.DataFrame({'gz':[5000,7000,9000,8500], 
                   'jxf':[60,84,98,91], 
                   'bz':['pass', 'good', 'excel', 'good']})
df.to_json("pandasTest/hellworld.json");
print("success")

```

    success
    


```python
#从json文件导入

import pandas as pd
df = pd.read_json("pandasTest/hellworld.json");
print(df.describe())
print("\n")
print(df.info())
print(df.head());

```

                    gz       jxf
    count     4.000000   4.00000
    mean   7375.000000  83.25000
    std    1796.988221  16.52019
    min    5000.000000  60.00000
    25%    6500.000000  78.00000
    50%    7750.000000  87.50000
    75%    8625.000000  92.75000
    max    9000.000000  98.00000
    
    
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 4 entries, 0 to 3
    Data columns (total 3 columns):
     #   Column  Non-Null Count  Dtype 
    ---  ------  --------------  ----- 
     0   gz      4 non-null      int64 
     1   jxf     4 non-null      int64 
     2   bz      4 non-null      object
    dtypes: int64(2), object(1)
    memory usage: 112.0+ bytes
    None
         gz  jxf     bz
    0  5000   60   pass
    1  7000   84   good
    2  9000   98  excel
    3  8500   91   good
    


```python
#从excel文件导入
import pandas as pd
df = pd.read_excel("pandasTest/hellworld.xls");
print(df.describe())
print("\n")
print(df.info())
print(df.head());
```

                    gz       jxf
    count     4.000000   4.00000
    mean   7375.000000  83.25000
    std    1796.988221  16.52019
    min    5000.000000  60.00000
    25%    6500.000000  78.00000
    50%    7750.000000  87.50000
    75%    8625.000000  92.75000
    max    9000.000000  98.00000
    
    
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4 entries, 0 to 3
    Data columns (total 4 columns):
     #   Column      Non-Null Count  Dtype 
    ---  ------      --------------  ----- 
     0   Unnamed: 0  4 non-null      object
     1   gz          4 non-null      int64 
     2   jxf         4 non-null      int64 
     3   bz          4 non-null      object
    dtypes: int64(2), object(2)
    memory usage: 160.0+ bytes
    None
      Unnamed: 0    gz  jxf     bz
    0         lw  5000   60   pass
    1         xl  7000   84   good
    2         xz  9000   98  excel
    3         lg  8500   91   good
    


```python
#从csv文件导入
import pandas as pd
df = pd.read_csv("pandasTest/hellworld.csv");

df['高于平均工资'] = df['gz'] > df['gz'].mean()
print(df['gz'].mean())
print(df.describe())
print("\n")
print(df.info())
print(df.head());

```

    7375.0
                    gz       jxf
    count     4.000000   4.00000
    mean   7375.000000  83.25000
    std    1796.988221  16.52019
    min    5000.000000  60.00000
    25%    6500.000000  78.00000
    50%    7750.000000  87.50000
    75%    8625.000000  92.75000
    max    9000.000000  98.00000
    
    
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4 entries, 0 to 3
    Data columns (total 5 columns):
     #   Column      Non-Null Count  Dtype 
    ---  ------      --------------  ----- 
     0   Unnamed: 0  4 non-null      object
     1   gz          4 non-null      int64 
     2   jxf         4 non-null      int64 
     3   bz          4 non-null      object
     4   高于平均工资      4 non-null      bool  
    dtypes: bool(1), int64(2), object(2)
    memory usage: 164.0+ bytes
    None
      Unnamed: 0    gz  jxf     bz  高于平均工资
    0         lw  5000   60   pass   False
    1         xl  7000   84   good   False
    2         xz  9000   98  excel    True
    3         lg  8500   91   good    True
    


```python
#遍历DataFrame
import pandas as pd
df = pd.read_csv("pandasTest/hellworld.csv");

print(df.head())
for col in df:
    print(col, end="\t")
    for item in df[col]:
        print(item, end="\t")
    print()
```

      Unnamed: 0    gz  jxf     bz
    0         lw  5000   60   pass
    1         xl  7000   84   good
    2         xz  9000   98  excel
    3         lg  8500   91   good
    Unnamed: 0	lw	xl	xz	lg	
    gz	5000	7000	9000	8500	
    jxf	60	84	98	91	
    bz	pass	good	excel	good	
    

##索引


```python
# 行选择
# [:,:]，第一个分号表示行，第二个分号表示列，两个分号支持[,,]用来单选，或者用[:]，前开后闭用来连续
# 选择
import pandas as pd
df = pd.read_csv("pandasTest/hellworld.csv");

print(df.iloc[:1,:].head())
```

      Unnamed: 0    gz  jxf    bz
    0         lw  5000   60  pass
    


```python
# 设置index，设置某一行为index
import pandas as pd
df = pd.read_csv("pandasTest/hellworld.csv");
df.set_index(["Unnamed: 0"], inplace=True)

print(df.head())
print(df.to_json())
df1 = pd.read_json(df.to_json())
print(df1.head())
```

                  gz  jxf     bz
    Unnamed: 0                  
    lw          5000   60   pass
    xl          7000   84   good
    xz          9000   98  excel
    lg          8500   91   good
    {"gz":{"lw":5000,"xl":7000,"xz":9000,"lg":8500},"jxf":{"lw":60,"xl":84,"xz":98,"lg":91},"bz":{"lw":"pass","xl":"good","xz":"excel","lg":"good"}}
          gz  jxf     bz
    lw  5000   60   pass
    xl  7000   84   good
    xz  9000   98  excel
    lg  8500   91   good
    


```python
# DataFrame.loc[:,:],行列条件选择第一个分号和第二个分号可以变成boolean表达式

import pandas as pd
df = pd.read_csv("pandasTest/hellworld.csv");
df.set_index(["Unnamed: 0"], inplace=True)
df = pd.read_json(df.to_json())
df.loc[df['gz'] > df['gz'].mean(), :]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gz</th>
      <th>jxf</th>
      <th>bz</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>xz</th>
      <td>9000</td>
      <td>98</td>
      <td>excel</td>
    </tr>
    <tr>
      <th>lg</th>
      <td>8500</td>
      <td>91</td>
      <td>good</td>
    </tr>
  </tbody>
</table>
</div>


