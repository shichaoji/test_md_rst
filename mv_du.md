

```python
import pandas as pd
import shutil
import os
```


```python
path = '/home/soyoung/face/newFlask/static/phos/'
# path = '/home/soyoung/face/Handsome_Pics/handsome/ori/keep/'
path = 'boy/'
```


```python
# %%time
# df = pd.read_csv('./31_Oct_50W_V5.csv', usecols=['name'])
# print df.shape
# df.head(3)
```


```python
# df['identify']=df['name'].apply(lambda x: ''.join(x.split('/')[-1].split('.')[:-1]))
```


```python
# df['name'].apply(lambda x: x.count('.')).value_counts()
```


```python
# df[df['name'].apply(lambda x: x.count('.'))>1]
```


```python
# df[df['name'].apply(lambda x: x.find('..jpg')>0)]
```


```python
# df['identify'].apply(lambda x: x.count('.')).value_counts()
```


```python
# df.set_index('identify').head()
```


```python
f = pd.DataFrame(os.listdir(path), columns=['name'])
f.shape
```




    (337905, 1)




```python
f['C']=f['name'].apply(lambda x:x.count('.'))

```


```python
f['C'].value_counts()
```




    1    330022
    2      5960
    3       907
    0       831
    4       146
    7        24
    5        15
    Name: C, dtype: int64




```python
# f[f['C']==0]['name']
```


```python

```




    ascii    337905
    Name: chk, dtype: int64




```python
%%time
fail=[]
for i in f[f['C']==0]['name']:
    try:
        os.remove(path+i)
    except:
        fail.append(i)
print len(fail)
```

    0
    Wall time: 149 ms



```python
# %%time
# import chardet
# f['chk'] = f['name'].apply(lambda x: chardet.detect(x)['encoding'])
# print f['chk'].value_counts()

# print f[f['chk']!='ascii']['name'].shape
# %%time
# fail=[]
# for i in f[f['chk']!='ascii']['name']:
#     try:
#         os.remove(path+i)
#     except:
#         fail.append(i)
# print len(fail)
```


```python
path
```




    'boy/'




```python
os.path.exists(path+'$_1(1).jpg')
```




    True




```python
one = f[f['name'].apply(lambda x:x[-7:]=='(1).jpg')]

```


```python
%%time
one.loc[:,('dup')]=one['name'].apply(lambda x: os.path.exists(path+x[:-7]+'.jpg'))
```

    Wall time: 2.03 s



```python
one['dup'].sum()
```




    33260




```python
%%time
fail=[]
for i in one[one['dup']]['name']:
    try:
        os.remove(path+i)
    except:
        fail.append(i)
print len(fail)
```

    0
    Wall time: 1min 15s



```python

```


```python

```


```python

```


```python
# f[f['C']==0]
```


```python
f['A']=f['name'].apply(lambda x:x[-5:]=='..jpg')
f['B']=f['name'].apply(lambda x:x[-6:]=='...jpg')
```


```python
try:
    os.mkdir('delete_boy')
except:
    print 'X'
```

    X



```python
f[f['A']|f['B']].head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>C</th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>UTF-8' '20160214234411_sjVXz...jpg</td>
      <td>3</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>UTF-8' '20160214234411_sjVXz..jpg</td>
      <td>2</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>UTF-8' '5735405fbb5f429935b70d74c04c8e6b(1).....</td>
      <td>3</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>UTF-8' '5735405fbb5f429935b70d74c04c8e6b(1)..jpg</td>
      <td>2</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>6</th>
      <td>UTF-8' '5735405fbb5f429935b70d74c04c8e6b...jpg</td>
      <td>3</td>
      <td>True</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




```python
f[f['A']|f['B']].shape
```




    (193934, 4)




```python
for i in f[f['A']|f['B']]['name']:
    print i
    break
```

     UTF-8' '20160214234411_sjVXz...jpg



```python
# %%time
# fail=[]
# for i in f[f['A']|f['B']]['name']:
#     try:
#         shutil.move(path+i,'delete_boy/')
#     except:
#         fail.append(i)
# print len(fail)
```


```python
%%time
fail=[]
for i in f[f['A']|f['B']]['name']:
    try:
        os.remove(path+i)
    except:
        fail.append(i)
print len(fail)
```

    0
    Wall time: 3min 54s



```python
fail[0]
```




    " UTF-8' '20160214234411_sjVXz...jpg"




```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python
a='123456'
```


```python
a[:-4]
```




    '12'




```python
a[-3:]
```




    '456'




```python
os.rename?
```


```python
import pandas as pd
```


```python
pd.DataFrame([(1,2),[3,4],[5,6]], columns=['x','y'])
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
a=[]
a.append([1,2])
a
```




    [[1, 2]]




```python
df = pd.read_csv("./duplicate_['boy'].csv")
df.shape
```




    (43657, 3)




```python
df['file'].value_counts().head()
```




    boy\4354913_906651.jpg                              1
    boy\9021746096411051035.jpg                         1
    boy\74b1OOOPIC2c..jpg                               1
    boy\e61190ef76c6a7ef189a306af4faaf51f3de663a.jpg    1
    boy\9c16fdfaaf51f3def199c02394eef01f3a297949.jpg    1
    Name: file, dtype: int64




```python
# df['duplicate'].value_counts()
```


```python
# df['file'].value_counts()
```


```python
import shutil
import os
```


```python
%%time
fail=[]
for i in df['file']:
    try:
        os.remove(i)
    except:
        fail.append(i)
print len(fail)
```

    0
    Wall time: 2min 21s



```python

```
