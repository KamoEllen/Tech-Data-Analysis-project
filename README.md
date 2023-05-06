### This project analyzes the prices of mobile phones and laptops, compares brands, calculates averages, and examines price distributions. It aims to understand price variations, identify influential factors, and gain insights into brand pricing strategies.


```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```


```python
from scipy.stats import ttest_ind
```


```python
from scipy.stats import chi2_contingency

```


```python
mobile_phone_price = pd.read_csv('/home/kamogelo/Downloads/archive (28)/Mobile phone price.csv')

```


```python
mobile_phone_price.head()
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
      <th>Brand</th>
      <th>Model</th>
      <th>Storage</th>
      <th>RAM</th>
      <th>Screen Size (inches)</th>
      <th>Camera (MP)</th>
      <th>Battery Capacity (mAh)</th>
      <th>Price ($)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Apple</td>
      <td>iPhone 13 Pro</td>
      <td>128 GB</td>
      <td>6 GB</td>
      <td>6.1</td>
      <td>12 + 12 + 12</td>
      <td>3095</td>
      <td>999</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Samsung</td>
      <td>Galaxy S21 Ultra</td>
      <td>256 GB</td>
      <td>12 GB</td>
      <td>6.8</td>
      <td>108 + 10 + 10 + 12</td>
      <td>5000</td>
      <td>1199</td>
    </tr>
    <tr>
      <th>2</th>
      <td>OnePlus</td>
      <td>9 Pro</td>
      <td>128 GB</td>
      <td>8 GB</td>
      <td>6.7</td>
      <td>48 + 50 + 8 + 2</td>
      <td>4500</td>
      <td>899</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Xiaomi</td>
      <td>Redmi Note 10 Pro</td>
      <td>128 GB</td>
      <td>6 GB</td>
      <td>6.67</td>
      <td>64 + 8 + 5 + 2</td>
      <td>5020</td>
      <td>279</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Google</td>
      <td>Pixel 6</td>
      <td>128 GB</td>
      <td>8 GB</td>
      <td>6.4</td>
      <td>50 + 12.2</td>
      <td>4614</td>
      <td>799</td>
    </tr>
  </tbody>
</table>
</div>




```python
mobile_phone_price.tail()
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
      <th>Brand</th>
      <th>Model</th>
      <th>Storage</th>
      <th>RAM</th>
      <th>Screen Size (inches)</th>
      <th>Camera (MP)</th>
      <th>Battery Capacity (mAh)</th>
      <th>Price ($)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>402</th>
      <td>Samsung</td>
      <td>Galaxy Note20 5G</td>
      <td>128</td>
      <td>8</td>
      <td>6.7</td>
      <td>12+64+12</td>
      <td>4300</td>
      <td>1049</td>
    </tr>
    <tr>
      <th>403</th>
      <td>Xiaomi</td>
      <td>Mi 10 Lite 5G</td>
      <td>128</td>
      <td>6</td>
      <td>6.57</td>
      <td>48+8+2+2</td>
      <td>4160</td>
      <td>349</td>
    </tr>
    <tr>
      <th>404</th>
      <td>Apple</td>
      <td>iPhone 12 Pro Max</td>
      <td>128</td>
      <td>6</td>
      <td>6.7</td>
      <td>12+12+12</td>
      <td>3687</td>
      <td>1099</td>
    </tr>
    <tr>
      <th>405</th>
      <td>Oppo</td>
      <td>Reno3</td>
      <td>128</td>
      <td>8</td>
      <td>6.4</td>
      <td>48+13+8+2</td>
      <td>4025</td>
      <td>429</td>
    </tr>
    <tr>
      <th>406</th>
      <td>Samsung</td>
      <td>Galaxy S10 Lite</td>
      <td>128</td>
      <td>6</td>
      <td>6.7</td>
      <td>48+12+5</td>
      <td>4500</td>
      <td>649</td>
    </tr>
  </tbody>
</table>
</div>




```python
mobile_phone_price.shape
```




    (407, 8)




```python
mobile_phone_price.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 407 entries, 0 to 406
    Data columns (total 8 columns):
     #   Column                  Non-Null Count  Dtype 
    ---  ------                  --------------  ----- 
     0   Brand                   407 non-null    object
     1   Model                   407 non-null    object
     2   Storage                 407 non-null    object
     3   RAM                     407 non-null    object
     4   Screen Size (inches)    407 non-null    object
     5   Camera (MP)             407 non-null    object
     6   Battery Capacity (mAh)  407 non-null    int64 
     7   Price ($)               407 non-null    object
    dtypes: int64(1), object(7)
    memory usage: 25.6+ KB



```python
mobile_phone_price.isnull().sum()
```




    Brand                     0
    Model                     0
    Storage                   0
    RAM                       0
    Screen Size (inches)      0
    Camera (MP)               0
    Battery Capacity (mAh)    0
    Price ($)                 0
    dtype: int64




```python
mobile_phone_price.describe()
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
      <th>Battery Capacity (mAh)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>407.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>4676.476658</td>
    </tr>
    <tr>
      <th>std</th>
      <td>797.193713</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1821.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>4300.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>5000.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>5000.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>7000.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
laptop_price = pd.read_csv('/home/kamogelo/Downloads/archive (29)/laptop_price.csv', encoding='latin1')
# The default encoding used by read_csv() is 'utf-8', but it seems that the file contains
# characters that cannot be decoded using 'utf-8'. So, we specify a different encoding.
# 'latin1' is a common encoding that can handle a wide range of characters.
```


```python
laptop_price.head()
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
      <th>laptop_ID</th>
      <th>Company</th>
      <th>Product</th>
      <th>TypeName</th>
      <th>Inches</th>
      <th>ScreenResolution</th>
      <th>Cpu</th>
      <th>Ram</th>
      <th>Memory</th>
      <th>Gpu</th>
      <th>OpSys</th>
      <th>Weight</th>
      <th>Price_euros</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Apple</td>
      <td>MacBook Pro</td>
      <td>Ultrabook</td>
      <td>13.3</td>
      <td>IPS Panel Retina Display 2560x1600</td>
      <td>Intel Core i5 2.3GHz</td>
      <td>8GB</td>
      <td>128GB SSD</td>
      <td>Intel Iris Plus Graphics 640</td>
      <td>macOS</td>
      <td>1.37kg</td>
      <td>1339.69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Apple</td>
      <td>Macbook Air</td>
      <td>Ultrabook</td>
      <td>13.3</td>
      <td>1440x900</td>
      <td>Intel Core i5 1.8GHz</td>
      <td>8GB</td>
      <td>128GB Flash Storage</td>
      <td>Intel HD Graphics 6000</td>
      <td>macOS</td>
      <td>1.34kg</td>
      <td>898.94</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>HP</td>
      <td>250 G6</td>
      <td>Notebook</td>
      <td>15.6</td>
      <td>Full HD 1920x1080</td>
      <td>Intel Core i5 7200U 2.5GHz</td>
      <td>8GB</td>
      <td>256GB SSD</td>
      <td>Intel HD Graphics 620</td>
      <td>No OS</td>
      <td>1.86kg</td>
      <td>575.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Apple</td>
      <td>MacBook Pro</td>
      <td>Ultrabook</td>
      <td>15.4</td>
      <td>IPS Panel Retina Display 2880x1800</td>
      <td>Intel Core i7 2.7GHz</td>
      <td>16GB</td>
      <td>512GB SSD</td>
      <td>AMD Radeon Pro 455</td>
      <td>macOS</td>
      <td>1.83kg</td>
      <td>2537.45</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Apple</td>
      <td>MacBook Pro</td>
      <td>Ultrabook</td>
      <td>13.3</td>
      <td>IPS Panel Retina Display 2560x1600</td>
      <td>Intel Core i5 3.1GHz</td>
      <td>8GB</td>
      <td>256GB SSD</td>
      <td>Intel Iris Plus Graphics 650</td>
      <td>macOS</td>
      <td>1.37kg</td>
      <td>1803.60</td>
    </tr>
  </tbody>
</table>
</div>




```python
laptop_price.tail()
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
      <th>laptop_ID</th>
      <th>Company</th>
      <th>Product</th>
      <th>TypeName</th>
      <th>Inches</th>
      <th>ScreenResolution</th>
      <th>Cpu</th>
      <th>Ram</th>
      <th>Memory</th>
      <th>Gpu</th>
      <th>OpSys</th>
      <th>Weight</th>
      <th>Price_euros</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1298</th>
      <td>1316</td>
      <td>Lenovo</td>
      <td>Yoga 500-14ISK</td>
      <td>2 in 1 Convertible</td>
      <td>14.0</td>
      <td>IPS Panel Full HD / Touchscreen 1920x1080</td>
      <td>Intel Core i7 6500U 2.5GHz</td>
      <td>4GB</td>
      <td>128GB SSD</td>
      <td>Intel HD Graphics 520</td>
      <td>Windows 10</td>
      <td>1.8kg</td>
      <td>638.0</td>
    </tr>
    <tr>
      <th>1299</th>
      <td>1317</td>
      <td>Lenovo</td>
      <td>Yoga 900-13ISK</td>
      <td>2 in 1 Convertible</td>
      <td>13.3</td>
      <td>IPS Panel Quad HD+ / Touchscreen 3200x1800</td>
      <td>Intel Core i7 6500U 2.5GHz</td>
      <td>16GB</td>
      <td>512GB SSD</td>
      <td>Intel HD Graphics 520</td>
      <td>Windows 10</td>
      <td>1.3kg</td>
      <td>1499.0</td>
    </tr>
    <tr>
      <th>1300</th>
      <td>1318</td>
      <td>Lenovo</td>
      <td>IdeaPad 100S-14IBR</td>
      <td>Notebook</td>
      <td>14.0</td>
      <td>1366x768</td>
      <td>Intel Celeron Dual Core N3050 1.6GHz</td>
      <td>2GB</td>
      <td>64GB Flash Storage</td>
      <td>Intel HD Graphics</td>
      <td>Windows 10</td>
      <td>1.5kg</td>
      <td>229.0</td>
    </tr>
    <tr>
      <th>1301</th>
      <td>1319</td>
      <td>HP</td>
      <td>15-AC110nv (i7-6500U/6GB/1TB/Radeon</td>
      <td>Notebook</td>
      <td>15.6</td>
      <td>1366x768</td>
      <td>Intel Core i7 6500U 2.5GHz</td>
      <td>6GB</td>
      <td>1TB HDD</td>
      <td>AMD Radeon R5 M330</td>
      <td>Windows 10</td>
      <td>2.19kg</td>
      <td>764.0</td>
    </tr>
    <tr>
      <th>1302</th>
      <td>1320</td>
      <td>Asus</td>
      <td>X553SA-XX031T (N3050/4GB/500GB/W10)</td>
      <td>Notebook</td>
      <td>15.6</td>
      <td>1366x768</td>
      <td>Intel Celeron Dual Core N3050 1.6GHz</td>
      <td>4GB</td>
      <td>500GB HDD</td>
      <td>Intel HD Graphics</td>
      <td>Windows 10</td>
      <td>2.2kg</td>
      <td>369.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
laptop_price.shape
```




    (1303, 13)




```python
laptop_price.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1303 entries, 0 to 1302
    Data columns (total 13 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   laptop_ID         1303 non-null   int64  
     1   Company           1303 non-null   object 
     2   Product           1303 non-null   object 
     3   TypeName          1303 non-null   object 
     4   Inches            1303 non-null   float64
     5   ScreenResolution  1303 non-null   object 
     6   Cpu               1303 non-null   object 
     7   Ram               1303 non-null   object 
     8   Memory            1303 non-null   object 
     9   Gpu               1303 non-null   object 
     10  OpSys             1303 non-null   object 
     11  Weight            1303 non-null   object 
     12  Price_euros       1303 non-null   float64
    dtypes: float64(2), int64(1), object(10)
    memory usage: 132.5+ KB



```python
laptop_price.isnull().sum()
```




    laptop_ID           0
    Company             0
    Product             0
    TypeName            0
    Inches              0
    ScreenResolution    0
    Cpu                 0
    Ram                 0
    Memory              0
    Gpu                 0
    OpSys               0
    Weight              0
    Price_euros         0
    dtype: int64




```python
laptop_price.describe().mean()
```




    laptop_ID       705.790987
    Inches          174.142937
    Price_euros    1557.822004
    dtype: float64




```python
least_phone_price = mobile_phone_price['Price ($)'].min()
print(least_phone_price)
```

    $1,199 



```python
mobile_phone_price['Price ($)'] = mobile_phone_price['Price ($)'].astype(str)

mobile_phone_price['Price ($)'] = mobile_phone_price['Price ($)'].str.replace('$', '', regex=False)  # Remove '$' symbol
mobile_phone_price['Price ($)'] = mobile_phone_price['Price ($)'].str.replace(',', '', regex=False)  # Remove comma separator
mobile_phone_price['Price ($)'] = mobile_phone_price['Price ($)'].str.extract('(\d+)', expand=False)  # Extract numeric values
mobile_phone_price['Price ($)'] = pd.to_numeric(mobile_phone_price['Price ($)'])  # Convert to numeric

average_phone_price = mobile_phone_price['Price ($)'].mean()
print(average_phone_price)
#converted to numeric before checking mean value
```

    408.3144963144963



```python
most_expensive_phone_price = mobile_phone_price['Price ($)'].max()
print(most_expensive_phone_price)
```

    1999



```python
least_laptop_price = laptop_price['Price_euros'].min()
print(least_laptop_price)

```

    174.0



```python
average_laptop_price = laptop_price['Price_euros'].mean()
print(average_laptop_price)

```

    1123.6869915579432



```python
most_expensive_laptop_price = laptop_price['Price_euros'].max()
print(most_expensive_laptop_price)
```

    6099.0



```python
mobile_phone_price.isnull().sum()

```




    Brand                     0
    Model                     0
    Storage                   0
    RAM                       0
    Screen Size (inches)      0
    Camera (MP)               0
    Battery Capacity (mAh)    0
    Price ($)                 0
    dtype: int64




```python
mobile_phone_price.dropna()
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
      <th>Brand</th>
      <th>Model</th>
      <th>Storage</th>
      <th>RAM</th>
      <th>Screen Size (inches)</th>
      <th>Camera (MP)</th>
      <th>Battery Capacity (mAh)</th>
      <th>Price ($)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Apple</td>
      <td>iPhone 13 Pro</td>
      <td>128 GB</td>
      <td>6 GB</td>
      <td>6.1</td>
      <td>12 + 12 + 12</td>
      <td>3095</td>
      <td>999</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Samsung</td>
      <td>Galaxy S21 Ultra</td>
      <td>256 GB</td>
      <td>12 GB</td>
      <td>6.8</td>
      <td>108 + 10 + 10 + 12</td>
      <td>5000</td>
      <td>1199</td>
    </tr>
    <tr>
      <th>2</th>
      <td>OnePlus</td>
      <td>9 Pro</td>
      <td>128 GB</td>
      <td>8 GB</td>
      <td>6.7</td>
      <td>48 + 50 + 8 + 2</td>
      <td>4500</td>
      <td>899</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Xiaomi</td>
      <td>Redmi Note 10 Pro</td>
      <td>128 GB</td>
      <td>6 GB</td>
      <td>6.67</td>
      <td>64 + 8 + 5 + 2</td>
      <td>5020</td>
      <td>279</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Google</td>
      <td>Pixel 6</td>
      <td>128 GB</td>
      <td>8 GB</td>
      <td>6.4</td>
      <td>50 + 12.2</td>
      <td>4614</td>
      <td>799</td>
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
    </tr>
    <tr>
      <th>402</th>
      <td>Samsung</td>
      <td>Galaxy Note20 5G</td>
      <td>128</td>
      <td>8</td>
      <td>6.7</td>
      <td>12+64+12</td>
      <td>4300</td>
      <td>1049</td>
    </tr>
    <tr>
      <th>403</th>
      <td>Xiaomi</td>
      <td>Mi 10 Lite 5G</td>
      <td>128</td>
      <td>6</td>
      <td>6.57</td>
      <td>48+8+2+2</td>
      <td>4160</td>
      <td>349</td>
    </tr>
    <tr>
      <th>404</th>
      <td>Apple</td>
      <td>iPhone 12 Pro Max</td>
      <td>128</td>
      <td>6</td>
      <td>6.7</td>
      <td>12+12+12</td>
      <td>3687</td>
      <td>1099</td>
    </tr>
    <tr>
      <th>405</th>
      <td>Oppo</td>
      <td>Reno3</td>
      <td>128</td>
      <td>8</td>
      <td>6.4</td>
      <td>48+13+8+2</td>
      <td>4025</td>
      <td>429</td>
    </tr>
    <tr>
      <th>406</th>
      <td>Samsung</td>
      <td>Galaxy S10 Lite</td>
      <td>128</td>
      <td>6</td>
      <td>6.7</td>
      <td>48+12+5</td>
      <td>4500</td>
      <td>649</td>
    </tr>
  </tbody>
</table>
<p>407 rows × 8 columns</p>
</div>




```python
mobile_phone_price['Price ($)'].fillna(mobile_phone_price['Price ($)'].mean(), inplace=True)
```


```python
max_price_by_brand = mobile_phone_price.groupby('Brand')['Price ($)'].max()
most_expensive_brand = max_price_by_brand.idxmax()
print("The brand with the most expensive phones is:", most_expensive_brand)
#brand with the highest maximum price
```

    The brand with the most expensive phones is: Samsung



```python
min_price_by_brand = mobile_phone_price.groupby('Brand')['Price ($)'].min()
cheapest_brand = min_price_by_brand.idxmin()
print("The brand with the cheapest phones is:", cheapest_brand)
#brand with the lowest minimum price

```

    The brand with the cheapest phones is: Motorola



```python
unique_brands = mobile_phone_price['Brand'].unique()
print("List of unique brands:")
for brand in unique_brands:
    print(brand)

```

    List of unique brands:
    Apple
    Samsung
    OnePlus
    Xiaomi
    Google
    Oppo
    Vivo
    Realme
    Motorola
    Nokia
    Sony
    LG
    Asus
    Blackberry
    CAT
    Huawei



```python
brand_average_price = mobile_phone_price.groupby('Brand')['Price ($)'].mean()
sorted_brands = brand_average_price.sort_values(ascending=False)
print("Brands from most expensive to least expensive:")
for brand in sorted_brands.index:
    print(brand)
#sort by average then listing from most expensive to least expensive
```

    Brands from most expensive to least expensive:
    Sony
    Asus
    Huawei
    Apple
    Google
    OnePlus
    LG
    Blackberry
    Samsung
    Oppo
    Vivo
    CAT
    Xiaomi
    Motorola
    Nokia
    Realme



```python
laptop_price['Price_euros'].fillna(laptop_price['Price_euros'].mean(), inplace=True)

```


```python
brand_average_price = laptop_price.groupby('Company')['Price_euros'].mean()
sorted_brands = brand_average_price.sort_values(ascending=False)
print("Brands from most expensive to least expensive:")
for brand in sorted_brands.index:
    print(brand)

```

    Brands from most expensive to least expensive:
    Razer
    LG
    MSI
    Google
    Microsoft
    Apple
    Huawei
    Samsung
    Toshiba
    Dell
    Xiaomi
    Asus
    Lenovo
    HP
    Fujitsu
    Acer
    Chuwi
    Mediacom
    Vero



```python
unique_brands = laptop_price['Company'].unique()
print("List of unique brands:")
for brand in unique_brands:
    print(brand)

```

    List of unique brands:
    Apple
    HP
    Acer
    Asus
    Dell
    Lenovo
    Chuwi
    MSI
    Microsoft
    Toshiba
    Huawei
    Xiaomi
    Vero
    Razer
    Mediacom
    Samsung
    Google
    Fujitsu
    LG



```python
min_price_by_brand = laptop_price.groupby('Company')['Price_euros'].min()
cheapest_brand = min_price_by_brand.idxmin()
print("The brand with the cheapest laptops is:", cheapest_brand)

```

    The brand with the cheapest laptops is: Acer



```python
max_price_by_brand = laptop_price.groupby('Company')['Price_euros'].max()
most_expensive_brand = max_price_by_brand.idxmax()
print("The brand with the most expensive laptops is:", most_expensive_brand)

```

    The brand with the most expensive laptops is: Razer



```python
mobile_brands = set(mobile_phone_price['Brand'].unique())
laptop_brands = set(laptop_price['Company'].unique())

common_brands = mobile_brands.intersection(laptop_brands)

print("Common brands between mobile phones and laptops:")
for brand in common_brands:
    print(brand)

```

    Common brands between mobile phones and laptops:
    Google
    Samsung
    LG
    Asus
    Apple
    Huawei
    Xiaomi



```python
mean_price = data['Price ($)'].mean()
print(mean_price)
```

    /tmp/ipykernel_14434/3377935478.py:1: FutureWarning: The default value of numeric_only in DataFrame.mean is deprecated. In a future version, it will default to False. In addition, specifying 'numeric_only=None' is deprecated. Select only valid columns or specify the value of numeric_only to silence this warning.
      mobile_phone_price.mean()





    Battery Capacity (mAh)    4676.476658
    Price ($)                  408.314496
    dtype: float64




```python
mean_laptop_price = laptop_price['Price_euros'].mean()
print(mean_laptop_price)
```

    1123.6869915579432



```python
mean_mobile_phone_price = mobile_phone_price['Price ($)'].mean()
print(mean_mobile_phone_price)
```

    408.3144963144963


### The t-statistic of 20.075055808267276 indicates a large difference between the mean prices of laptops and mobile phones. The p-value of 1.1870133638759013e-80 is extremely small, which suggests strong evidence against the null hypothesis that the mean prices of laptops and mobile phones are equal. In other words, there is a significant difference in the mean prices between laptops and mobile phones.

#### Therefore, based on the t-test results, we can conclude that there is a statistically significant difference between the average prices of laptops and mobile phones.








```python
# Perform the t-test
t_stat, p_value = ttest_ind(laptop_price['Price_euros'], mobile_phone_price['Price ($)'])

# Print the mean prices
print("Mean laptop price:", mean_laptop_price)
print("Mean mobile phone price:", mean_mobile_phone_price)

# Print the t-test results
print("T-statistic:", t_stat)
print("P-value:", p_value)
```

    Mean laptop price: 1123.6869915579432
    Mean mobile phone price: 408.3144963144963
    T-statistic: 20.075055808267276
    P-value: 1.1870133638759013e-80


###  contingency table that shows the counts or frequencies of the different combinations of brand and storage capacity.


```python
#fixed TypeError: unsupported operand type(s) for /: 'str' and 'int' by using pd.to_numeric()
mobile_phone_price['Screen Size (inches)'] = pd.to_numeric(mobile_phone_price['Screen Size (inches)'], errors='coerce')
mobile_phone_price['Price ($)'] = pd.to_numeric(mobile_phone_price['Price ($)'], errors='coerce')

correlation = mobile_phone_price['Screen Size (inches)'].corr(mobile_phone_price['Price ($)'])

print("Correlation coefficient:", correlation)

```

    Correlation coefficient: -0.005164712625142225



```python
# Assuming you have a DataFrame named 'mobile_phone_price' with columns 'Brand' and 'Storage '
contingency_table = pd.crosstab(mobile_phone_price['Brand'], mobile_phone_price['Storage '])

# Perform the chi-square test
chi2, p_value = stats.chisquare(contingency_table)

print("Chi-square statistic:", chi2)
print("P-value:", p_value)
```

    Chi-square statistic: [102.43243243  77.31372549 174.392       47.26086957  22.71428571
      15.6         15.          13.          53.          13.
      53.11111111  50.          71.48979592]
    P-value: [4.50500661e-15 2.15670486e-10 3.20313766e-29 3.34393340e-05
     9.03973750e-02 4.09122032e-01 4.51417211e-01 6.02297939e-01
     3.85598295e-06 6.02297939e-01 3.69535758e-06 1.20411986e-05
     2.42138564e-09]


#### The low p-value suggests that the association between these two variables is not due to chance, but rather a significant and meaningful connection exists.


```python
contingency_table = pd.crosstab(mobile_phone_price['Brand'], mobile_phone_price['Storage '])
# Perform the chi-square test
chi2, p_value, _, _ = stats.chi2_contingency(contingency_table)
print("Chi-square statistic:", chi2)
print("P-value:", p_value)
```

    Chi-square statistic: 375.9617609243807
    P-value: 6.518382675946527e-16



```python
mobile_phone_price_columns = mobile_phone_price.columns
print(mobile_phone_price_columns)
```

    Index(['Brand', 'Model', 'Storage ', 'RAM ', 'Screen Size (inches)',
           'Camera (MP)', 'Battery Capacity (mAh)', 'Price ($)'],
          dtype='object')



```python
###Price Distribution by Brand
```


```python
plt.figure(figsize=(12, 6))
mobile_phone_price.boxplot(column='Price ($)', by='Brand')
plt.xticks(rotation=45)
plt.xlabel('Brand')
plt.ylabel('Price ($)')
plt.title('Price Distribution by Brand')
plt.show()
```


    <Figure size 1200x600 with 0 Axes>



    
![png](output_49_1.png)
    



```python
###Price Comparison by Storage and RAM 
```


```python
grouped_df = mobile_phone_price.groupby(['Storage ', 'RAM ']).mean(numeric_only=True)['Price ($)'].unstack()

# Plotting the grouped bar plot
fig, ax = plt.subplots(figsize=(12, 6))
grouped_df.plot(kind='bar', ax=ax)
ax.set_xlabel('Storage')
ax.set_ylabel('Average Price')
ax.set_title('Price Comparison by Storage and RAM')
plt.xticks(rotation=0)
plt.legend(title='RAM')
plt.show()
```


    
![png](output_51_0.png)
    



```python
### Correlation Analysis (Heatmap)
```


```python
corr_matrix = mobile_phone_price.corr()

plt.figure(figsize=(10, 8))
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm')
plt.title('Correlation Matrix')
plt.show()
```

    /tmp/ipykernel_14434/1326466750.py:1: FutureWarning: The default value of numeric_only in DataFrame.corr is deprecated. In a future version, it will default to False. Select only valid columns or specify the value of numeric_only to silence this warning.
      corr_matrix = mobile_phone_price.corr()



    
![png](output_53_1.png)
    



```python
### Competitive Analysis
```


```python
grouped_df = mobile_phone_price.groupby('Brand').mean()['Price ($)'].sort_values(ascending=False)

# Plotting the grouped bar plot
fig, ax = plt.subplots(figsize=(12, 6))
grouped_df.plot(kind='bar', ax=ax)
ax.set_xlabel('Brand')
ax.set_ylabel('Average Price')
ax.set_title('Competitive Analysis')
plt.xticks(rotation=45)
plt.show()
```

    /tmp/ipykernel_14434/3724624031.py:1: FutureWarning: The default value of numeric_only in DataFrameGroupBy.mean is deprecated. In a future version, numeric_only will default to False. Either specify numeric_only or select only columns which should be valid for the function.
      grouped_df = mobile_phone_price.groupby('Brand').mean()['Price ($)'].sort_values(ascending=False)



    
![png](output_55_1.png)
    
