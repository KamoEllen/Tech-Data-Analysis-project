
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
    
