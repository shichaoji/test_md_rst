
.. code:: ipython2

    import pandas as pd
    import os
    import natsort

.. code:: ipython2

    !pwd


.. parsed-literal::

    /home/shj16110/notebook/27Dec2017


.. code:: ipython2

    os.chdir('../DeepGenderPred/predict/')

.. code:: ipython2

    files = os.listdir('./')
    files = natsort.natsorted(files)
    len(files)




.. parsed-literal::

    367



.. code:: ipython2

    files[0:2]+files[-2:]




.. parsed-literal::

    ['predict_1.csv', 'predict_2.csv', 'predict_366.csv', 'predict_367.csv']



.. code:: ipython2

    %%time
    df = pd.concat([pd.read_csv(i) for i in files])
    print df.shape


.. parsed-literal::

    (109417, 3)
    CPU times: user 452 ms, sys: 27 ms, total: 479 ms
    Wall time: 6.53 s


.. code:: ipython2

    df = df.reset_index(drop=True)
    df.tail()




.. raw:: html

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
          <th>file</th>
          <th>label</th>
          <th>score</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>109412</th>
          <td>/home/shj16110/notebook/DeepGenderPred/AllPics...</td>
          <td>M</td>
          <td>0.94</td>
        </tr>
        <tr>
          <th>109413</th>
          <td>/home/shj16110/notebook/DeepGenderPred/AllPics...</td>
          <td>M</td>
          <td>0.69</td>
        </tr>
        <tr>
          <th>109414</th>
          <td>/home/shj16110/notebook/DeepGenderPred/AllPics...</td>
          <td>M</td>
          <td>0.99</td>
        </tr>
        <tr>
          <th>109415</th>
          <td>/home/shj16110/notebook/DeepGenderPred/AllPics...</td>
          <td>M</td>
          <td>0.93</td>
        </tr>
        <tr>
          <th>109416</th>
          <td>/home/shj16110/notebook/DeepGenderPred/AllPics...</td>
          <td>F</td>
          <td>0.65</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython2

    df['user_id'] = df['file'].str.split('/').apply(lambda x: x[-1]).str.split('_').apply(lambda x: int(x[0]))
    df.shape




.. parsed-literal::

    (109417, 4)



.. code:: ipython2

    df1 = df[['user_id','label','score']]
    df1.shape




.. parsed-literal::

    (109417, 3)



.. code:: ipython2

    df2 = df1.sort_values('user_id')
    df2 = df2.reset_index(drop=True)
    df2.shape




.. parsed-literal::

    (109417, 3)



.. code:: ipython2

    df2.to_csv('gpu_results.csv', index=False)

.. code:: ipython2

    os.chdir('../../27Dec2017/')

.. code:: ipython2

    df9 = pd.read_csv('./Training_set.csv')
    df9.shape




.. parsed-literal::

    (108011, 3)



.. code:: ipython2

    df2.head(1)




.. raw:: html

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
          <th>user_id</th>
          <th>label</th>
          <th>score</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>215</td>
          <td>F</td>
          <td>0.94</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython2

    df9.head(1)




.. raw:: html

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
          <th>index</th>
          <th>label</th>
          <th>user_id</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>0</td>
          <td>1</td>
          <td>5</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython2

    df_c = df2.set_index('user_id').join(df9.set_index('user_id'), how='inner', lsuffix='_pic')
    df_c.shape




.. parsed-literal::

    (62527, 4)



.. code:: ipython2

    df2['label'].value_counts()




.. parsed-literal::

    M    72498
    F    36919
    Name: label, dtype: int64



.. code:: ipython2

    df9['label'].value_counts()




.. parsed-literal::

    1    89306
    0    18705
    Name: label, dtype: int64



.. code:: ipython2

    df_c['label2'] = df_c['label_pic'].map({'M':1,'F':0})
    df_c.head()




.. raw:: html

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
          <th>label_pic</th>
          <th>score</th>
          <th>index</th>
          <th>label</th>
          <th>label2</th>
        </tr>
        <tr>
          <th>user_id</th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>215</th>
          <td>F</td>
          <td>0.94</td>
          <td>4</td>
          <td>1</td>
          <td>0</td>
        </tr>
        <tr>
          <th>220</th>
          <td>M</td>
          <td>1.00</td>
          <td>5</td>
          <td>1</td>
          <td>1</td>
        </tr>
        <tr>
          <th>224</th>
          <td>M</td>
          <td>0.94</td>
          <td>6</td>
          <td>1</td>
          <td>1</td>
        </tr>
        <tr>
          <th>274</th>
          <td>F</td>
          <td>0.89</td>
          <td>7</td>
          <td>1</td>
          <td>0</td>
        </tr>
        <tr>
          <th>470</th>
          <td>M</td>
          <td>1.00</td>
          <td>9</td>
          <td>1</td>
          <td>1</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython2

    df_c[df_c['label']!=df_c['label2']].shape




.. parsed-literal::

    (17761, 5)



.. code:: ipython2

    df_c.shape




.. parsed-literal::

    (62527, 5)



.. code:: ipython2

    (62527-17761)/62527.0




.. parsed-literal::

    0.715946711020839


