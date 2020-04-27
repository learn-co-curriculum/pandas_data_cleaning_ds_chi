
# Data Cleaning with Pandas

## Scenario

As data scientists, we want to build a model to predict the sale price of a house in Seattle in 2019, based on its square footage. We know that the King County Department of Assessments has comprehensive data available on real property sales in the Seattle area. We need to prepare the data.


```python
import pandas as pd
pd.set_option('display.max_columns', None)
```

### Learning Goals:

- practice cleaning a real dataset
- practice using `py` files and the terminal in conjunction with jupyter notebooks
- run a `py` script through the terminal


```python
!pwd
```

    /Users/johnmaxbarry/Documents/012720/Chicago-ds-012720/module_1/week_1/day5_more_pandas/pandas_data_cleaning


### First, get the data!

When working on a project involving data that can fit on our computer, we store it in a `data` directory.

```bash
cd <project_directory>  # example: cd ~/flatiron_ds/pandas-3
mkdir data
cd data
```

Note that `<project_directory>` in angle brackets is a _placeholder_. You should type the path to the actual location on your computer where you're working on this project. Do not literally type `<project_directory>` and _do not type the angle brackets_. You can see an example in the _comment_ to the right of the command above.

![terminal](https://media3.giphy.com/media/yR4xZagT71AAM/giphy.gif?cid=790b76115d3620444553533759086a54&rid=giphy.gif)

Now, we'll need to download the two data files that we need. We can do this at the command line:

```bash
wget https://aqua.kingcounty.gov/extranet/assessor/Real%20Property%20Sales.zip
wget https://aqua.kingcounty.gov/extranet/assessor/Residential%20Building.zip
```

*Note:* If you do not have the `wget` command yet, you can install it with `brew install wget`, or use `curl <url> -o <filename>`.

Note that `%20` in a URL translates into a space. Even though you will *never put spaces in filenames*, you may need to deal with spaces that _other_ people have used in filenames.

There are two ways to handle the spaces in these filenames when referencing them at the command line.


![internetgif](https://media2.giphy.com/media/QWkuGmMgphvmE/giphy.gif?cid=790b76115d361f42304a6850369f37ea&rid=giphy.gif)


```python
!wget https://aqua.kingcounty.gov/extranet/assessor/Real%20Property%20Sales.zip
```

    --2020-02-03 13:21:40--  https://aqua.kingcounty.gov/extranet/assessor/Real%20Property%20Sales.zip
    Resolving aqua.kingcounty.gov... 146.129.240.28
    Connecting to aqua.kingcounty.gov|146.129.240.28|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 126877142 (121M) [application/x-zip-compressed]
    Saving to: 'Real Property Sales.zip'
    
    Real Property Sales 100%[===================>] 121.00M   900KB/s    in 2m 5s   
    
    2020-02-03 13:23:45 (993 KB/s) - 'Real Property Sales.zip' saved [126877142/126877142]
    



```python
!wget https://aqua.kingcounty.gov/extranet/assessor/Residential%20Building.zip
```

    --2020-02-03 13:23:49--  https://aqua.kingcounty.gov/extranet/assessor/Residential%20Building.zip
    Resolving aqua.kingcounty.gov... 146.129.240.28
    Connecting to aqua.kingcounty.gov|146.129.240.28|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 24686735 (24M) [application/x-zip-compressed]
    Saving to: 'Residential Building.zip'
    
    Residential Buildin 100%[===================>]  23.54M  1.01MB/s    in 24s     
    
    2020-02-03 13:24:13 (1015 KB/s) - 'Residential Building.zip' saved [24686735/24686735]
    


#### 1. You can _escape_ the spaces by putting a backslash (`\`, remember _backslash is next to backspace_) before each one:

`unzip Real\ Property\ Sales.zip`

This is what happens if you tab-complete the filename in the terminal. Tab completion is your friend!

#### 2. You can put the entire filename in quotes:

`unzip "Real Property Sales.zip"`

Try unzipping these files with the `unzip` command. The `unzip` command takes one argument, the name of the file that you want to unzip.


```python
# ! unzip 'data/Real Property Sales.zip'
! unzip 'data/Residential Building.zip'
```

    Archive:  data/Residential Building.zip
      inflating: EXTR_ResBldg.csv        



```python
!pwd
```

    /Users/johnmaxbarry/Documents/012720/Chicago-ds-012720/module_1/week_1/day5_more_pandas/pandas_data_cleaning



```python
sales_df = pd.read_csv('data/EXTR_RPSale.csv')
```

    /Users/johnmaxbarry/anaconda3/lib/python3.7/site-packages/IPython/core/interactiveshell.py:3058: DtypeWarning: Columns (1,2) have mixed types. Specify dtype option on import or set low_memory=False.
      interactivity=interactivity, compiler=compiler, result=result)


### Seeing pink? Warnings are useful!

Note the warning above: `DtypeWarning: Columns (1, 2) have mixed types.` Because we start with an index of zero, the columns that we're being warned about are actually the _second_ and _third_ columns, `sales_df['Major']` and `sales_df['Minor']`.


```python
sales_df.head()
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
      <th>ExciseTaxNbr</th>
      <th>Major</th>
      <th>Minor</th>
      <th>DocumentDate</th>
      <th>SalePrice</th>
      <th>RecordingNbr</th>
      <th>Volume</th>
      <th>Page</th>
      <th>PlatNbr</th>
      <th>PlatType</th>
      <th>PlatLot</th>
      <th>PlatBlock</th>
      <th>SellerName</th>
      <th>BuyerName</th>
      <th>PropertyType</th>
      <th>PrincipalUse</th>
      <th>SaleInstrument</th>
      <th>AFForestLand</th>
      <th>AFCurrentUseLand</th>
      <th>AFNonProfitUse</th>
      <th>AFHistoricProperty</th>
      <th>SaleReason</th>
      <th>PropertyClass</th>
      <th>SaleWarning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1600768</td>
      <td>330405.0</td>
      <td>100.0</td>
      <td>03/19/1998</td>
      <td>215000</td>
      <td>199803251689</td>
      <td>145</td>
      <td>039</td>
      <td>330405</td>
      <td>C</td>
      <td>8722</td>
      <td>3</td>
      <td>ROSEHILL L L C                                ...</td>
      <td>MEYERS R STEPHEN+MEYERS PEGGY S               ...</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>1</td>
      <td>3</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>2413752</td>
      <td>868146.0</td>
      <td>30.0</td>
      <td>09/11/2009</td>
      <td>0</td>
      <td>20091022001461</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>DORN DEBRA A                                  ...</td>
      <td>DORN MICHAEL A                                ...</td>
      <td>3</td>
      <td>2</td>
      <td>15</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>11</td>
      <td>3</td>
      <td>18 31 38</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1939480</td>
      <td>258190.0</td>
      <td>265.0</td>
      <td>02/06/2003</td>
      <td>0</td>
      <td>20030214003390</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>MAHNEL JUDITH                                 ...</td>
      <td>HAHNEL GREG B                                 ...</td>
      <td>3</td>
      <td>6</td>
      <td>15</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>10</td>
      <td>8</td>
      <td>31 51</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2999169</td>
      <td>919715.0</td>
      <td>200.0</td>
      <td>07/08/2019</td>
      <td>192000</td>
      <td>20190712001080</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>WAGNERESTATES LLC                             ...</td>
      <td>SCHAFFER CORBIN                               ...</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>1</td>
      <td>3</td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>2220242</td>
      <td>334330.0</td>
      <td>1343.0</td>
      <td>05/30/2006</td>
      <td>0</td>
      <td>20060706002163</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>TRYON CONCEPTS L L C                          ...</td>
      <td>LARKERIDGE DEVELOPMENT INC                    ...</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>18</td>
      <td>7</td>
      <td>11 31</td>
    </tr>
  </tbody>
</table>
</div>




```python
bldg_df.head()
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
      <th>Major</th>
      <th>Minor</th>
      <th>BldgNbr</th>
      <th>NbrLivingUnits</th>
      <th>Address</th>
      <th>BuildingNumber</th>
      <th>Fraction</th>
      <th>DirectionPrefix</th>
      <th>StreetName</th>
      <th>StreetType</th>
      <th>DirectionSuffix</th>
      <th>ZipCode</th>
      <th>Stories</th>
      <th>BldgGrade</th>
      <th>BldgGradeVar</th>
      <th>SqFt1stFloor</th>
      <th>SqFtHalfFloor</th>
      <th>SqFt2ndFloor</th>
      <th>SqFtUpperFloor</th>
      <th>SqFtUnfinFull</th>
      <th>SqFtUnfinHalf</th>
      <th>SqFtTotLiving</th>
      <th>SqFtTotBasement</th>
      <th>SqFtFinBasement</th>
      <th>FinBasementGrade</th>
      <th>SqFtGarageBasement</th>
      <th>SqFtGarageAttached</th>
      <th>DaylightBasement</th>
      <th>SqFtOpenPorch</th>
      <th>SqFtEnclosedPorch</th>
      <th>SqFtDeck</th>
      <th>HeatSystem</th>
      <th>HeatSource</th>
      <th>BrickStone</th>
      <th>ViewUtilization</th>
      <th>Bedrooms</th>
      <th>BathHalfCount</th>
      <th>Bath3qtrCount</th>
      <th>BathFullCount</th>
      <th>FpSingleStory</th>
      <th>FpMultiStory</th>
      <th>FpFreestanding</th>
      <th>FpAdditional</th>
      <th>YrBuilt</th>
      <th>YrRenovated</th>
      <th>PcntComplete</th>
      <th>Obsolescence</th>
      <th>PcntNetCondition</th>
      <th>Condition</th>
      <th>AddnlCost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>42450</td>
      <td>95</td>
      <td>1</td>
      <td>1</td>
      <td>3322  NE 8TH ST   98056</td>
      <td>3322</td>
      <td></td>
      <td>NE</td>
      <td>8TH</td>
      <td>ST</td>
      <td></td>
      <td>98056</td>
      <td>1.0</td>
      <td>6</td>
      <td>0</td>
      <td>1060</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1060</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>340</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>0</td>
      <td>N</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1940</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>42500</td>
      <td>210</td>
      <td>1</td>
      <td>1</td>
      <td>3518  NE 9TH ST   98056</td>
      <td>3518</td>
      <td></td>
      <td>NE</td>
      <td>9TH</td>
      <td>ST</td>
      <td></td>
      <td>98056</td>
      <td>1.0</td>
      <td>6</td>
      <td>0</td>
      <td>1080</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1080</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
      <td>0</td>
      <td>N</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1954</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>42504</td>
      <td>9051</td>
      <td>1</td>
      <td>1</td>
      <td>2703  NE 68TH ST   98115</td>
      <td>2703</td>
      <td></td>
      <td>NE</td>
      <td>68TH</td>
      <td>ST</td>
      <td></td>
      <td>98115</td>
      <td>1.5</td>
      <td>7</td>
      <td>0</td>
      <td>1040</td>
      <td>480</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1920</td>
      <td>830</td>
      <td>400</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td></td>
      <td>50</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td></td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1918</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>42504</td>
      <td>9058</td>
      <td>1</td>
      <td>1</td>
      <td>6524   26TH AVE NE  98115</td>
      <td>6524</td>
      <td></td>
      <td></td>
      <td>26TH</td>
      <td>AVE</td>
      <td>NE</td>
      <td>98115</td>
      <td>1.0</td>
      <td>7</td>
      <td>0</td>
      <td>1270</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2270</td>
      <td>1270</td>
      <td>1000</td>
      <td>6</td>
      <td>240</td>
      <td>0</td>
      <td></td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>2</td>
      <td>0</td>
      <td></td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1958</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>42504</td>
      <td>9074</td>
      <td>1</td>
      <td>1</td>
      <td>6554   29TH AVE NE  98115</td>
      <td>6554</td>
      <td></td>
      <td></td>
      <td>29TH</td>
      <td>AVE</td>
      <td>NE</td>
      <td>98115</td>
      <td>1.0</td>
      <td>8</td>
      <td>0</td>
      <td>1570</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3140</td>
      <td>1570</td>
      <td>1570</td>
      <td>7</td>
      <td>0</td>
      <td>460</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>100</td>
      <td>N</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1952</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### Data overload?

That's a lot of columns. We're only interested in identifying the date, sale price, and square footage of each specific property. What can we do?


```python
sales_df = sales_df.loc[:,['Date', 'SalePrice', 'sq_feet']]
```

    /Users/johnmaxbarry/anaconda3/lib/python3.7/site-packages/pandas/core/indexing.py:1418: FutureWarning: 
    Passing list-likes to .loc or [] with any missing label will raise
    KeyError in the future, you can use .reindex() as an alternative.
    
    See the documentation here:
    https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#deprecate-loc-reindex-listlike
      return self._getitem_tuple(key)



```python

```




    Series([], Name: Date, dtype: int64)




```python
bldg_df = pd.read_csv('data/EXTR_ResBldg.csv')
```

### Another warning! Which column has index 11?


```python
bldg_df['ZipCode']
```




    0         98056
    1         98056
    2         98115
    3         98115
    4         98115
              ...  
    514559    98042
    514560    98177
    514561    98177
    514562    98053
    514563    98045
    Name: ZipCode, Length: 514564, dtype: object



`ZipCode` seems like a potentially useful column. We'll need it to determine which house sales took place in Seattle.

### So many features!

As data scientists, we should be _very_ cautious about discarding potentially useful data. But, today, we're interested in _only_ the total square footage of each property. What can we do?



```python
bldg_df.head()
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
      <th>Major</th>
      <th>Minor</th>
      <th>BldgNbr</th>
      <th>NbrLivingUnits</th>
      <th>Address</th>
      <th>BuildingNumber</th>
      <th>Fraction</th>
      <th>DirectionPrefix</th>
      <th>StreetName</th>
      <th>StreetType</th>
      <th>DirectionSuffix</th>
      <th>ZipCode</th>
      <th>Stories</th>
      <th>BldgGrade</th>
      <th>BldgGradeVar</th>
      <th>SqFt1stFloor</th>
      <th>SqFtHalfFloor</th>
      <th>SqFt2ndFloor</th>
      <th>SqFtUpperFloor</th>
      <th>SqFtUnfinFull</th>
      <th>SqFtUnfinHalf</th>
      <th>SqFtTotLiving</th>
      <th>SqFtTotBasement</th>
      <th>SqFtFinBasement</th>
      <th>FinBasementGrade</th>
      <th>SqFtGarageBasement</th>
      <th>SqFtGarageAttached</th>
      <th>DaylightBasement</th>
      <th>SqFtOpenPorch</th>
      <th>SqFtEnclosedPorch</th>
      <th>SqFtDeck</th>
      <th>HeatSystem</th>
      <th>HeatSource</th>
      <th>BrickStone</th>
      <th>ViewUtilization</th>
      <th>Bedrooms</th>
      <th>BathHalfCount</th>
      <th>Bath3qtrCount</th>
      <th>BathFullCount</th>
      <th>FpSingleStory</th>
      <th>FpMultiStory</th>
      <th>FpFreestanding</th>
      <th>FpAdditional</th>
      <th>YrBuilt</th>
      <th>YrRenovated</th>
      <th>PcntComplete</th>
      <th>Obsolescence</th>
      <th>PcntNetCondition</th>
      <th>Condition</th>
      <th>AddnlCost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>42450</td>
      <td>95</td>
      <td>1</td>
      <td>1</td>
      <td>3322  NE 8TH ST   98056</td>
      <td>3322</td>
      <td></td>
      <td>NE</td>
      <td>8TH</td>
      <td>ST</td>
      <td></td>
      <td>98056</td>
      <td>1.0</td>
      <td>6</td>
      <td>0</td>
      <td>1060</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1060</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>340</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>0</td>
      <td>N</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1940</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>42500</td>
      <td>210</td>
      <td>1</td>
      <td>1</td>
      <td>3518  NE 9TH ST   98056</td>
      <td>3518</td>
      <td></td>
      <td>NE</td>
      <td>9TH</td>
      <td>ST</td>
      <td></td>
      <td>98056</td>
      <td>1.0</td>
      <td>6</td>
      <td>0</td>
      <td>1080</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1080</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
      <td>0</td>
      <td>N</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1954</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>42504</td>
      <td>9051</td>
      <td>1</td>
      <td>1</td>
      <td>2703  NE 68TH ST   98115</td>
      <td>2703</td>
      <td></td>
      <td>NE</td>
      <td>68TH</td>
      <td>ST</td>
      <td></td>
      <td>98115</td>
      <td>1.5</td>
      <td>7</td>
      <td>0</td>
      <td>1040</td>
      <td>480</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1920</td>
      <td>830</td>
      <td>400</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td></td>
      <td>50</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td></td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1918</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>42504</td>
      <td>9058</td>
      <td>1</td>
      <td>1</td>
      <td>6524   26TH AVE NE  98115</td>
      <td>6524</td>
      <td></td>
      <td></td>
      <td>26TH</td>
      <td>AVE</td>
      <td>NE</td>
      <td>98115</td>
      <td>1.0</td>
      <td>7</td>
      <td>0</td>
      <td>1270</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2270</td>
      <td>1270</td>
      <td>1000</td>
      <td>6</td>
      <td>240</td>
      <td>0</td>
      <td></td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>2</td>
      <td>0</td>
      <td></td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1958</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>42504</td>
      <td>9074</td>
      <td>1</td>
      <td>1</td>
      <td>6554   29TH AVE NE  98115</td>
      <td>6554</td>
      <td></td>
      <td></td>
      <td>29TH</td>
      <td>AVE</td>
      <td>NE</td>
      <td>98115</td>
      <td>1.0</td>
      <td>8</td>
      <td>0</td>
      <td>1570</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3140</td>
      <td>1570</td>
      <td>1570</td>
      <td>7</td>
      <td>0</td>
      <td>460</td>
      <td>N</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>100</td>
      <td>N</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1952</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
sq_feet = []
for feature in bldg_df.columns:
    if 'Sq' in feature:
        sq_feet.append(feature)
sq_feet.append('Major')

        
df_sq = bldg_df[sq_feet]
df = pd.DataFrame(df_sq.sum(axis=1))
df['Major_b'] = bldg_df['Major']
df['Minor_b'] = bldg_df['Minor']
df.head()
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
      <th>0</th>
      <th>Major_b</th>
      <th>Minor_b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>44910</td>
      <td>42450</td>
      <td>95</td>
    </tr>
    <tr>
      <th>1</th>
      <td>44660</td>
      <td>42500</td>
      <td>210</td>
    </tr>
    <tr>
      <th>2</th>
      <td>47224</td>
      <td>42504</td>
      <td>9051</td>
    </tr>
    <tr>
      <th>3</th>
      <td>48554</td>
      <td>42504</td>
      <td>9058</td>
    </tr>
    <tr>
      <th>4</th>
      <td>50814</td>
      <td>42504</td>
      <td>9074</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.shape
```




    (514564, 3)




```python
sales_df.shape
```




    (2066905, 24)




```python

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
      <th>0</th>
      <th>Major_b</th>
      <th>Minor_b</th>
      <th>ExciseTaxNbr</th>
      <th>Major</th>
      <th>Minor</th>
      <th>DocumentDate</th>
      <th>SalePrice</th>
      <th>RecordingNbr</th>
      <th>Volume</th>
      <th>Page</th>
      <th>PlatNbr</th>
      <th>PlatType</th>
      <th>PlatLot</th>
      <th>PlatBlock</th>
      <th>SellerName</th>
      <th>BuyerName</th>
      <th>PropertyType</th>
      <th>PrincipalUse</th>
      <th>SaleInstrument</th>
      <th>AFForestLand</th>
      <th>AFCurrentUseLand</th>
      <th>AFNonProfitUse</th>
      <th>AFHistoricProperty</th>
      <th>SaleReason</th>
      <th>PropertyClass</th>
      <th>SaleWarning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>44910</td>
      <td>42450</td>
      <td>95</td>
      <td>1600768</td>
      <td>330405</td>
      <td>100</td>
      <td>03/19/1998</td>
      <td>215000</td>
      <td>199803251689</td>
      <td>145</td>
      <td>039</td>
      <td>330405</td>
      <td>C</td>
      <td>8722</td>
      <td>3</td>
      <td>ROSEHILL L L C                                ...</td>
      <td>MEYERS R STEPHEN+MEYERS PEGGY S               ...</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>1</td>
      <td>3</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>44660</td>
      <td>42500</td>
      <td>210</td>
      <td>2413752</td>
      <td>868146</td>
      <td>30</td>
      <td>09/11/2009</td>
      <td>0</td>
      <td>20091022001461</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>DORN DEBRA A                                  ...</td>
      <td>DORN MICHAEL A                                ...</td>
      <td>3</td>
      <td>2</td>
      <td>15</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>11</td>
      <td>3</td>
      <td>18 31 38</td>
    </tr>
    <tr>
      <th>2</th>
      <td>47224</td>
      <td>42504</td>
      <td>9051</td>
      <td>1939480</td>
      <td>258190</td>
      <td>265</td>
      <td>02/06/2003</td>
      <td>0</td>
      <td>20030214003390</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>MAHNEL JUDITH                                 ...</td>
      <td>HAHNEL GREG B                                 ...</td>
      <td>3</td>
      <td>6</td>
      <td>15</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>10</td>
      <td>8</td>
      <td>31 51</td>
    </tr>
    <tr>
      <th>3</th>
      <td>48554</td>
      <td>42504</td>
      <td>9058</td>
      <td>2999169</td>
      <td>919715</td>
      <td>200</td>
      <td>07/08/2019</td>
      <td>192000</td>
      <td>20190712001080</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>WAGNERESTATES LLC                             ...</td>
      <td>SCHAFFER CORBIN                               ...</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>1</td>
      <td>3</td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>50814</td>
      <td>42504</td>
      <td>9074</td>
      <td>2220242</td>
      <td>334330</td>
      <td>1343</td>
      <td>05/30/2006</td>
      <td>0</td>
      <td>20060706002163</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>TRYON CONCEPTS L L C                          ...</td>
      <td>LARKERIDGE DEVELOPMENT INC                    ...</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>18</td>
      <td>7</td>
      <td>11 31</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>514559</th>
      <td>958080</td>
      <td>950720</td>
      <td>590</td>
      <td>2419925</td>
      <td>285610</td>
      <td>925</td>
      <td>11/30/2009</td>
      <td>0</td>
      <td>20091202000340</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>MOUNTAIN SHADOW CONSTRUCTION INC              ...</td>
      <td>MOUNTAIN PACIFIC BANK                         ...</td>
      <td>3</td>
      <td>6</td>
      <td>27</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>4</td>
      <td>8</td>
      <td>31</td>
    </tr>
    <tr>
      <th>514560</th>
      <td>955250</td>
      <td>950810</td>
      <td>35</td>
      <td>1411920</td>
      <td>351000</td>
      <td>20</td>
      <td>01/05/1995</td>
      <td>177000</td>
      <td>199501061216</td>
      <td>072</td>
      <td>021</td>
      <td>351000</td>
      <td>P</td>
      <td>2</td>
      <td></td>
      <td>POWERS AARON B+RUTH A                         ...</td>
      <td>HODSON DONALD L+ESTHER E                      ...</td>
      <td>3</td>
      <td>6</td>
      <td>2</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>1</td>
      <td>8</td>
      <td></td>
    </tr>
    <tr>
      <th>514561</th>
      <td>954280</td>
      <td>950850</td>
      <td>15</td>
      <td>2834065</td>
      <td>223130</td>
      <td>620</td>
      <td>11/08/2016</td>
      <td>195000</td>
      <td>20161115000964</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>MELCHIORRE ARMANDO                            ...</td>
      <td>SERWOLD MADISON                               ...</td>
      <td>14</td>
      <td>2</td>
      <td>3</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>1</td>
      <td>3</td>
      <td></td>
    </tr>
    <tr>
      <th>514562</th>
      <td>957235</td>
      <td>950885</td>
      <td>170</td>
      <td>1608199</td>
      <td>666918</td>
      <td>150</td>
      <td>04/21/1998</td>
      <td>152600</td>
      <td>199804282180</td>
      <td>109</td>
      <td>016</td>
      <td>666918</td>
      <td>C</td>
      <td>E-101</td>
      <td></td>
      <td>ESTABROOK HAZEL L                             ...</td>
      <td>COCHRAN CHARLES L                             ...</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>1</td>
      <td>3</td>
      <td></td>
    </tr>
    <tr>
      <th>514563</th>
      <td>956240</td>
      <td>951030</td>
      <td>80</td>
      <td>1358636</td>
      <td>242302</td>
      <td>9122</td>
      <td>02/08/1994</td>
      <td>0</td>
      <td>199402161342</td>
      <td>000</td>
      <td>000</td>
      <td>000000</td>
      <td></td>
      <td></td>
      <td></td>
      <td>GIBSON STEPHEN GALE                           ...</td>
      <td>TWOHIG KATHLEEN JERI                          ...</td>
      <td>3</td>
      <td>0</td>
      <td>15</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>9</td>
      <td>8</td>
      <td>32</td>
    </tr>
  </tbody>
</table>
<p>514564 rows × 27 columns</p>
</div>



### Error!

Why are we seeing an error when we try to join the dataframes?

<table>
    <tr>
        <td style="text-align:left"><pre>
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2013160 entries, 0 to 2013159
Data columns (total 4 columns):
Major           object
Minor           object
DocumentDate    object
SalePrice       int64
dtypes: int64(1), object(3)
memory usage: 61.4+ MB</pre></td>
        <td style="text-align:left"><pre>
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 511359 entries, 0 to 511358
Data columns (total 4 columns):
Major            511359 non-null int64
Minor            511359 non-null int64
SqFtTotLiving    511359 non-null int64
ZipCode          468345 non-null object
dtypes: int64(3), object(1)
memory usage: 15.6+ MB
</pre></td>
    </tr>
</table>

Review the error message in light of the above:

* `ValueError: You are trying to merge on object and int64 columns.`


```python

```

### Error!

Note the useful error message above:

`ValueError: Unable to parse string "      " at position 936643`

In this case, we want to treat non-numeric values as missing values. Let's see if there's a way to change how the `pd.to_numeric` function handles errors.


```python
# The single question mark means "show me the docstring"
pd.to_numeric?
```

Here's the part that we're looking for:
```
errors : {'ignore', 'raise', 'coerce'}, default 'raise'
    - If 'raise', then invalid parsing will raise an exception
    - If 'coerce', then invalid parsing will be set as NaN
    - If 'ignore', then invalid parsing will return the input
```

Let's try setting the `errors` parameter to `'coerce'`.


```python
sales_df['Major'] = pd.to_numeric(sales_df['Major'], errors='coerce')
```

Did it work?


```python
sales_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2066905 entries, 0 to 2066904
    Data columns (total 24 columns):
    ExciseTaxNbr          int64
    Major                 float64
    Minor                 object
    DocumentDate          object
    SalePrice             int64
    RecordingNbr          object
    Volume                object
    Page                  object
    PlatNbr               object
    PlatType              object
    PlatLot               object
    PlatBlock             object
    SellerName            object
    BuyerName             object
    PropertyType          int64
    PrincipalUse          int64
    SaleInstrument        int64
    AFForestLand          object
    AFCurrentUseLand      object
    AFNonProfitUse        object
    AFHistoricProperty    object
    SaleReason            int64
    PropertyClass         int64
    SaleWarning           object
    dtypes: float64(1), int64(7), object(16)
    memory usage: 378.5+ MB


It worked! Let's do the same thing with the `Minor` parcel number.


```python
sales_df['Minor'] = pd.to_numeric(sales_df['Minor'], errors='coerce')
```


```python
sales_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2066905 entries, 0 to 2066904
    Data columns (total 24 columns):
    ExciseTaxNbr          int64
    Major                 float64
    Minor                 float64
    DocumentDate          object
    SalePrice             int64
    RecordingNbr          object
    Volume                object
    Page                  object
    PlatNbr               object
    PlatType              object
    PlatLot               object
    PlatBlock             object
    SellerName            object
    BuyerName             object
    PropertyType          int64
    PrincipalUse          int64
    SaleInstrument        int64
    AFForestLand          object
    AFCurrentUseLand      object
    AFNonProfitUse        object
    AFHistoricProperty    object
    SaleReason            int64
    PropertyClass         int64
    SaleWarning           object
    dtypes: float64(2), int64(7), object(15)
    memory usage: 378.5+ MB


Now, let's try our join again.


```python
sales_data = pd.merge(sales_df, bldg_df, on=['Major', 'Minor'])
```


```python
sales_data.info()
```

We can see right away that we're missing zip codes for many of the sales transactions. (1321536 non-null entries for ZipCode is fewer than the 1436772 entries in the dataframe.) 


```python
sales_data.loc[sales_data['ZipCode'].isna()].head()
```

Because we are interested in finding houses in Seattle zip codes, we will need to drop the rows with missing zip codes.


```python


# sales_data.head()
```


```python
# 1334823 homes

```

# Your turn: Data Cleaning with Pandas
![turtletype](https://media3.giphy.com/media/cFdHXXm5GhJsc/giphy.gif?cid=790b76115d3627d8354c7179366b0672&rid=giphy.gif)

# 311 Service Requests received by the City of Chicago
https://data.cityofchicago.org/Service-Requests/311-Service-Requests/v6vf-nfxy



###  Read and Investigate data
Use multiple notebook cells to accomplish this! Press `[esc]` then `B` to create a new cell below the current cell. Press `[return]` to start typing in the new cell.


```python
df = pd.read_csv('311_Service_Requests.csv')
```


    ---------------------------------------------------------------------------

    FileNotFoundError                         Traceback (most recent call last)

    <ipython-input-71-02da90961c2f> in <module>
    ----> 1 df = pd.read_csv('311_Service_Requests.csv')
    

    ~/anaconda3/lib/python3.7/site-packages/pandas/io/parsers.py in parser_f(filepath_or_buffer, sep, delimiter, header, names, index_col, usecols, squeeze, prefix, mangle_dupe_cols, dtype, engine, converters, true_values, false_values, skipinitialspace, skiprows, skipfooter, nrows, na_values, keep_default_na, na_filter, verbose, skip_blank_lines, parse_dates, infer_datetime_format, keep_date_col, date_parser, dayfirst, cache_dates, iterator, chunksize, compression, thousands, decimal, lineterminator, quotechar, quoting, doublequote, escapechar, comment, encoding, dialect, error_bad_lines, warn_bad_lines, delim_whitespace, low_memory, memory_map, float_precision)
        683         )
        684 
    --> 685         return _read(filepath_or_buffer, kwds)
        686 
        687     parser_f.__name__ = name


    ~/anaconda3/lib/python3.7/site-packages/pandas/io/parsers.py in _read(filepath_or_buffer, kwds)
        455 
        456     # Create the parser.
    --> 457     parser = TextFileReader(fp_or_buf, **kwds)
        458 
        459     if chunksize or iterator:


    ~/anaconda3/lib/python3.7/site-packages/pandas/io/parsers.py in __init__(self, f, engine, **kwds)
        893             self.options["has_index_names"] = kwds["has_index_names"]
        894 
    --> 895         self._make_engine(self.engine)
        896 
        897     def close(self):


    ~/anaconda3/lib/python3.7/site-packages/pandas/io/parsers.py in _make_engine(self, engine)
       1133     def _make_engine(self, engine="c"):
       1134         if engine == "c":
    -> 1135             self._engine = CParserWrapper(self.f, **self.options)
       1136         else:
       1137             if engine == "python":


    ~/anaconda3/lib/python3.7/site-packages/pandas/io/parsers.py in __init__(self, src, **kwds)
       1915         kwds["usecols"] = self.usecols
       1916 
    -> 1917         self._reader = parsers.TextReader(src, **kwds)
       1918         self.unnamed_cols = self._reader.unnamed_cols
       1919 


    pandas/_libs/parsers.pyx in pandas._libs.parsers.TextReader.__cinit__()


    pandas/_libs/parsers.pyx in pandas._libs.parsers.TextReader._setup_parser_source()


    FileNotFoundError: [Errno 2] File b'311_Service_Requests.csv' does not exist: b'311_Service_Requests.csv'



```python
df.head()
```


```python

```

##### Create new df with the following columns:
    'SR_NUMBER', 'SR_TYPE', 'SR_SHORT_CODE', 'OWNER_DEPARTMENT', 'STATUS',
       'CREATED_DATE', 'LAST_MODIFIED_DATE', 'LOCATION','CITY', 'LATITUDE']]

### Investigate and handle missing values

What's the right thing to do with missing values?


```python

```


```python

```


```python

```

### Find out top 3 service types


```python

```

## Pair Programming:
    
For these exercises, we will be practicing pair programming. 
While we work through these exercises, choose who will code and who will supervise.
I.E., one person types, and the other suggests the appropriate direction to head in.


### In-class Exercise: 
Think about 2 questions each person and use city_df to answer your questions


## After class : 
### Turning code into a script

#### make a new .py file
- open a new `.py` file _or_ open a new jupyter notbook and export as a `.py` file so we can start to edit the `.py` file directly
- save the file as `mean_ppsf_seattle.py`
- look at all your code between `sales_df = pd.read_csv('data/Real Property Sales.zip')` and question `number 6` above

#### review & organize your code
- _organize_ your code in the `mean_ppsf_seattle.py` to start with `sales_df = pd.read_csv('data/Real Property Sales.zip')` and end with printing out the mean price per square foot for a house sold in seattle in 2019
- the code should be able to run without throwing any errors
- remember to include `import pandas as pd` and any other necessary statements at the start of your script

#### test your script
- go to the terminal
- make sure you are in the same directory path as your jupyter notebook and the `.py` file
- in the terminal type and then run `python mean_ppsf_seattle.py`
- confirm the script returns in terminal what you wanted it to return


![anykey](https://media2.giphy.com/media/26BGIqWh2R1fi6JDa/giphy.gif?cid=790b76115d3627d8354c7179366b0672&rid=giphy.gif)


```python

```
