
# Account Transactions with Plaid

In this section, you will use the Plaid Python SDK to connect to the Developer Sandbox account and grab a list of transactions. You will need to complete the following steps:


1. Use the access token to fetch the transactions for the last 90 days
2. Print the categories for each transaction type
3. Create a new DataFrame using the following fields from the JSON transaction data: `date, name, amount, category`. (For categories with more than one label, just use the first category label in the list)
4. Convert the data types to the appropriate types (i.e. datetimeindex for the date and float for the amount)

### 1. Fetch the Transactions for the last 90 days


```python
start_date = '{:%Y-%m-%d}'.format(datetime.datetime.now() + datetime.timedelta(-90))
end_date = '{:%Y-%m-%d}'.format(datetime.datetime.now()) 
transactions_response = client.Transactions.get(access_token, start_date, end_date)
```

### 2. Print the categories for each transaction


```python
for transactions in transactions_response['transactions']:
    print(transactions['category'])  
```

    ['Payment']
    ['Food and Drink', 'Restaurants', 'Fast Food']
    ['Shops', 'Sporting Goods']
    ['Payment', 'Credit Card']
    ['Travel', 'Taxi']
    ['Transfer', 'Debit']
    ['Transfer', 'Deposit']
    ['Recreation', 'Gyms and Fitness Centers']
    ['Travel', 'Airlines and Aviation Services']
    ['Food and Drink', 'Restaurants', 'Fast Food']
    ['Food and Drink', 'Restaurants', 'Coffee Shop']
    ['Food and Drink', 'Restaurants']
    ['Transfer', 'Credit']
    ['Travel', 'Airlines and Aviation Services']
    ['Travel', 'Taxi']
    ['Food and Drink', 'Restaurants']
    ['Payment']
    ['Food and Drink', 'Restaurants', 'Fast Food']
    ['Shops', 'Sporting Goods']
    ['Payment', 'Credit Card']
    ['Travel', 'Taxi']
    ['Transfer', 'Debit']
    ['Transfer', 'Deposit']
    ['Recreation', 'Gyms and Fitness Centers']
    ['Travel', 'Airlines and Aviation Services']
    ['Food and Drink', 'Restaurants', 'Fast Food']
    ['Food and Drink', 'Restaurants', 'Coffee Shop']
    ['Food and Drink', 'Restaurants']
    ['Transfer', 'Credit']
    ['Travel', 'Airlines and Aviation Services']
    ['Travel', 'Taxi']
    ['Food and Drink', 'Restaurants']
    ['Payment']
    ['Food and Drink', 'Restaurants', 'Fast Food']
    ['Shops', 'Sporting Goods']
    ['Payment', 'Credit Card']
    ['Travel', 'Taxi']
    ['Transfer', 'Debit']
    ['Transfer', 'Deposit']
    ['Recreation', 'Gyms and Fitness Centers']
    ['Travel', 'Airlines and Aviation Services']
    ['Food and Drink', 'Restaurants', 'Fast Food']
    ['Food and Drink', 'Restaurants', 'Coffee Shop']
    ['Food and Drink', 'Restaurants']
    ['Transfer', 'Credit']
    ['Travel', 'Airlines and Aviation Services']
    ['Travel', 'Taxi']
    ['Food and Drink', 'Restaurants']
    ['Payment']
    ['Food and Drink', 'Restaurants', 'Fast Food']
    ['Shops', 'Sporting Goods']
    

### 3. Create a new DataFrame using the following fields from the JSON transaction data: date, name, amount, category. 

(For categories with more than one label, just use the first category label in the list)


```python
# A dataframe can be defined as a list of lists:
transactions_lst = []
for transactions in transactions_response['transactions']:
    transactions_date = transactions['date']
    transactions_name = transactions['name']
    transactions_amount = transactions['amount']
    transactions_category = transactions['category']
    transactions_lst.append([transactions_date,transactions_name,transactions_amount,transactions_category[0]])     
```


```python
# Once we have our list of lists we just use DataFrame panda method
transactions_df = pd.DataFrame(transactions_lst)
transactions_df.columns =['Date','Name','Amount','Category']
transactions_df.head()
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
      <th>Date</th>
      <th>Name</th>
      <th>Amount</th>
      <th>Category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-04-24</td>
      <td>AUTOMATIC PAYMENT - THANK</td>
      <td>2078.5</td>
      <td>Payment</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-04-24</td>
      <td>KFC</td>
      <td>500.0</td>
      <td>Food and Drink</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-04-24</td>
      <td>Madison Bicycle Shop</td>
      <td>500.0</td>
      <td>Shops</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-04-15</td>
      <td>CREDIT CARD 3333 PAYMENT *//</td>
      <td>25.0</td>
      <td>Payment</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-04-15</td>
      <td>Uber</td>
      <td>5.4</td>
      <td>Travel</td>
    </tr>
  </tbody>
</table>
</div>



### 4. Convert the data types to the appropriate types 

(i.e. datetimeindex for the date and float for the amount)


```python
transactions_df['Amount'] = transactions_df['Amount'].astype(float)
transactions_df.set_index('Date', inplace = True)
transactions_df.head()
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
      <th>Name</th>
      <th>Amount</th>
      <th>Category</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-04-24</th>
      <td>AUTOMATIC PAYMENT - THANK</td>
      <td>2078.5</td>
      <td>Payment</td>
    </tr>
    <tr>
      <th>2020-04-24</th>
      <td>KFC</td>
      <td>500.0</td>
      <td>Food and Drink</td>
    </tr>
    <tr>
      <th>2020-04-24</th>
      <td>Madison Bicycle Shop</td>
      <td>500.0</td>
      <td>Shops</td>
    </tr>
    <tr>
      <th>2020-04-15</th>
      <td>CREDIT CARD 3333 PAYMENT *//</td>
      <td>25.0</td>
      <td>Payment</td>
    </tr>
    <tr>
      <th>2020-04-15</th>
      <td>Uber</td>
      <td>5.4</td>
      <td>Travel</td>
    </tr>
  </tbody>
</table>
</div>



---

# Income Analysis with Plaid

In this section, you will use the Plaid Sandbox to complete the following:
1. Determine the previous year's gross income and print the results
2. Determine the current monthly income and print the results
3. Determine the projected yearly income and print the results


```python
start_date = '{:%Y-%m-%d}'.format(datetime.datetime.now() + datetime.timedelta(-90))
end_date = '{:%Y-%m-%d}'.format(datetime.datetime.now()) 
transactions_response = client.Transactions.get(access_token, start_date, end_date)
```

# Budget Analysis
In this section, you will use the transactions DataFrame to analyze the customer's budget

1. Calculate the total spending per category and print the results (Hint: groupby or count transactions per category)
2. Generate a bar chart with the number of transactions for each category 
3. Calculate the expenses per month
4. Plot the total expenses per month

### Calculate the expenses per category


```python
transactions_df.groupby(['Category']).sum()
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
      <th>Amount</th>
    </tr>
    <tr>
      <th>Category</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Food and Drink</th>
      <td>3317.19</td>
    </tr>
    <tr>
      <th>Payment</th>
      <td>6310.50</td>
    </tr>
    <tr>
      <th>Recreation</th>
      <td>235.50</td>
    </tr>
    <tr>
      <th>Shops</th>
      <td>1500.00</td>
    </tr>
    <tr>
      <th>Transfer</th>
      <td>20537.34</td>
    </tr>
    <tr>
      <th>Travel</th>
      <td>35.19</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Spending Categories Pie Chart
transactions_df.groupby(['Category']).sum().plot(subplots = True ,legend = False,kind = 'pie', title = 'Spending per category (last 90 days)')
```




    array([<matplotlib.axes._subplots.AxesSubplot object at 0x000001B08E011B00>],
          dtype=object)




![png](output_31_1.png)



```python
# bar chart with the number of transactions for each category
transactions_df.groupby(['Category']).count().plot(legend = False,kind = 'bar', color = 'DarkBlue', title = 'Number of transactions per category (last 90 days)')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1b08f16c898>




![png](output_32_1.png)


### Calculate the expenses per month


```python
# create a new column with only year and month from 'Date'
transactions_df.reset_index(inplace = True)
transactions_df['Date_Y_M']= transactions_df['Date'].str.slice(0, -3)
expenses_month_df = transactions_df.groupby(['Date_Y_M']).sum()
expenses_month_df
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
      <th>Amount</th>
    </tr>
    <tr>
      <th>Date_Y_M</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-01</th>
      <td>4084.83</td>
    </tr>
    <tr>
      <th>2020-02</th>
      <td>10145.24</td>
    </tr>
    <tr>
      <th>2020-03</th>
      <td>11145.24</td>
    </tr>
    <tr>
      <th>2020-04</th>
      <td>6560.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# plot expenses per month in a bar plot
expenses_month_df.plot(kind = 'bar', title = 'Expenses per month (last 90 days)', color = 'DarkBlue')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1b08f1e9a20>




![png](output_35_1.png)


### Use the API to fetch income data from the sandbox and print the following:

#### Last Year's Income Before Tax


```python
# We need to fetch the income data from the sandbox
income_response = client.Income.get(access_token)
income_response
```




    {'income': {'income_streams': [{'confidence': 0.99,
        'days': 720,
        'monthly_income': 500,
        'name': 'UNITED AIRLINES'}],
      'last_year_income': 6000,
      'last_year_income_before_tax': 7285,
      'max_number_of_overlapping_income_streams': 1,
      'number_of_income_streams': 1,
      'projected_yearly_income': 6085,
      'projected_yearly_income_before_tax': 7389},
     'request_id': '2ye0cqFDbcNnyf6'}




```python
last_year_incomeBT =income_response['income']['last_year_income_before_tax']
print (f"Last year's income before tax was: ${last_year_incomeBT}")
```

    Last year's income before tax was: $7285
    

#### Current Monthly Income


```python
# we just query the 'income' dictionary with the info
income_streams =income_response['income']['income_streams']
print (f"Current monthly income is: ${income_streams[0]['monthly_income']}")
```

    Current monthly income is: $500
    

#### Projected Year's Income Before Tax


```python
# we just query the 'income' dictionary with the info
projected_year_incomeBT =income_response['income']['projected_yearly_income_before_tax']
print (f"Projected Year's Income Before Tax is: ${projected_year_incomeBT}")
```

    Projected Year's Income Before Tax is: $7389
    
