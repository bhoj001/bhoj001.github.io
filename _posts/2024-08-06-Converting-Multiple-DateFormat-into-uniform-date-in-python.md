---
layout: post
title: "Converting Multiple DateTime formate into Single Uniform format using Python and Pandas"
description: "Convert multiple datetime formate into single uniform format"
tags: [python, pandas]
# link: http://mademistakes.com  
published: True
authors: 'Bhoj Bahadur Karki'
name: 'Bhoj Bahadur Karki'
share: True
---
Problem Statement:

Imagine you are receiving data for an ETL pipeline from multiple sources, each with a different datetime format for when the items are created. Through the ETL process, this data is collected into a single Excel file where you must convert the datetime values to a standard format before inserting them into the data warehouse.

Assume you have datetime values in five different formats:

```python
'2024-02/05',
'02/01/2024',
'02/16-2024 10:01:35',
'02 22 2024'
```

Your output should be a standard format such as:

```
2024-02-22 00:00:00
2024-02-16 10:01:35
2024-02-05 00:00:00
2024-02-01 00:00:00
```

Solution:

We can use python pandas to convert the list of dates into data frame and then use apply function to apply parse date function. The parse date function uses python datetime.strptime() function.


We can use Python Pandas to convert the list of dates into a DataFrame and then use the `apply` function to apply the`parse_date`function. The`parse_date`function utilizes Python's`datetime.strptime()` function.


About the `datetime.strptime()` class method:
It parses a string into a datetime object given a corresponding format



About datetime.strptime() class method:
It parses a string into a datetime object given a corresponding format

Syntax:

```python
strptime(date_string, format)
```

Code:

```python
import pandas as pd
from datetime import datetime


# Step 1: given dates from questions as input
given_date =[
        '2024-02/05',
        '02/01/2024',
        '02/16-2024 10:01:35',
        '02 22 2024']

given_date_df = pd.DataFrame({'date': given_date})

# Step 2: Define a function to parse and standardize the date format
def parse_date(date_str:str):
    date_formats = [
        '%Y-%m/%d',
        '%m/%d/%Y',
        '%m/%d-%Y %H:%M:%S',
        '%m %d %Y'
    ]
    for fmt in date_formats:
        try:
            return datetime.strptime(date_str, fmt)
        except ValueError:
            pass
    raise ValueError(f'Invalid format {date_str}')


given_date_df['parsed_date'] = given_date_df['date'].apply(parse_date)

print(given_date_df)
```

Output:

```python
                  date         parsed_date
0           2024-02/05 2024-02-05 00:00:00
1           02/01/2024 2024-02-01 00:00:00
2  02/16-2024 10:01:35 2024-02-16 10:01:35
3           02 22 2024 2024-02-22 00:00:00

```

# Sorting the given values in latest order

Now if you want to sort the values in descending order to get the most recent ones in the top you can do following:

```python
# Step 4: Sorting the dataframe by the parsed date in descending order
sorted_date_df = given_date_df.sort_values(by='parsed_date', ascending=False).reset_index(drop=True)

# Step 5: Display the sorted dataframe
sorted_date_df[['date', 'parsed_date']]
```

Output:

```python
date	parsed_date
0	02 22 2024	2024-02-22 00:00:00
1	02/16-2024 10:01:35	2024-02-16 10:01:35
2	2024-02/05	2024-02-05 00:00:00
3	02/01/2024	2024-02-01 00:00:00
```
