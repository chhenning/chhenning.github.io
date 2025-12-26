---
date:
  created: 2025-12-23
  updated: 2025-12-24

draft: False

categories:
  - Data Analysis

tags:
  - python
  - pandas
  - cheat sheet

links:
  - pandas cheats heet: https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf
  - Aurélien Géron pandas cheat sheet: https://github.com/ageron/handson-mlp/blob/main/tools_pandas.ipynb
  - pandarallel: https://github.com/nalepae/pandarallel
---

# pandas cheat sheet

You haven't touched `pandas` for awhile and feeling a bit rusty?

Here you go!

<!-- more -->

```py
import numpy as np

import pandas as pd

np.set_printoptions(precision=4, suppress=True)

pd.options.display.max_columns = 20
pd.options.display.max_rows = 20
pd.options.display.max_colwidth = 80
```

## Dataframe

### Read csv and summarize data

```py
df = pd.read_csv("data/toy-dataset.zip")  # or read with

# if Number exists override the default index.
df.set_index("Number", inplace=True)

print(" --- Info ---")
print(df.info())
print("")
print(" --- First Five Rows ---")
print(df.head(n=5))
print("")
print(" --- Last Two Rows ---")
print(df.tail(n=2))
print("")

print(" --- Describe ---")
# show basic stats for numerical columns
df.describe()
```

### Basic operations

```py
a = pd.DataFrame(
    [[1.4, np.nan], [7.1, -4.5], [np.nan, np.nan], [0.75, -1.3]],
    index=list("abcd"),
    columns=["one", "two"],
)

print("--- print dataframe ----")
print(a)
print("")
print("--- column \"one\" ----")
print(a.one)
print("")
print("--- sum cols ----")
print(a.sum(axis=0))  # sum up each column
print("")
print("--- sum rows ----")
print(a.sum(axis=1))  # sum up each row (across all columns)
print("")
print("--- mean by rows ----")
print(a.mean(axis="columns", skipna=False)) # better to use axis=1
print("")
print("--- INDICES for max value by columns ----")
print(a.idxmax(axis=0)) # note that indices in this dataframe are letters
print("")
print("--- rows that contain the max values by column ----")
print(a.loc[a.idxmax(axis=0)]) # a[[b,d]]
print("")
print("--- INTEGER INDICES where smallest value ----")
print(a.one.argmin())  # take the Series ("one" column) and return int index of smallest value
```

```
--- print dataframe ----
    one  two
a  1.40  NaN
b  7.10 -4.5
c   NaN  NaN
d  0.75 -1.3

--- column "one" ----
a    1.40
b    7.10
c     NaN
d    0.75
Name: one, dtype: float64

--- sum cols ----
one    9.25
two   -5.80
dtype: float64

--- sum rows ----
a    1.40
b    2.60
c    0.00
d   -0.55
dtype: float64

--- mean by rows ----
a      NaN
b    1.300
c      NaN
d   -0.275
dtype: float64

--- INDICES for max value by columns ----
one    b
two    d
dtype: object

--- rows that contain the max values by column ----
    one  two
b  7.10 -4.5
d  0.75 -1.3

--- INTEGER INDICES where smallest value ----
3
```

### Create dataframe with some random data

```py
import pandas as pd

import numpy as np

np.set_printoptions(precision=4, suppress=True)

np.random.seed(42)

num_rows = 20
df = pd.DataFrame(
    {
        "date": np.random.choice(
            pd.date_range("2018-01-01", "2018-06-18", freq="D"), num_rows
        ),
        "analysis_tool": np.random.choice(
            ["pandas", "r", "julia", "sas", "stata", "spss"], num_rows
        ),
        "database": np.random.choice(
            ["postgres", "mysql", "sqlite", "oracle", "sql server", "db2"], num_rows
        ),
        "os": np.random.choice(
            ["windows 10", "ubuntu", "mac os", "android", "ios", "windows 7", "debian"],
            num_rows,
        ),
        "num1": np.random.randn(num_rows) * 100,
        "num2": np.random.uniform(0, 1, num_rows),
        "num3": np.random.randint(100, size=num_rows),
        "bool": np.random.choice([True, False], num_rows),
    },
    columns=["date", "analysis_tool", "num1", "database", "num2", "os", "num3", "bool"],
)
```

### Create dataframe from list of lists

```py
data = [
    ["DS", "Linked_list", 10],
    ["DS", "Stack", 9],
    ["DS", "Queue", 7],
    ["Algo", "Greedy", 8],
    ["Algo", "DP", 6],
    ["Algo", "BackTrack", 5],
]

# Create the pandas DataFrame
df = pd.DataFrame(data, columns=["Category", "Name", "Marks"])
```

### loc, iloc and slices

```py
# note that order_id is index
df = pd.DataFrame({
    "order_id": [101, 102, 103, 104, 105],
    "table": [1, 1, 2, 3, 3],
    "item": ["Beer", "Wine", "Burger", "Beer", "Dessert"],
    "price": [6.0, 9.0, 14.0, 6.0, 7.5]
}).set_index("order_id")

#############
# loc - label-based (names, conditions)
# Note: loc slicing is end-inclusive!
#############

df.loc[[101, 104], ["item", "price"]]

df.loc[df["price"] > 7, ["item", "price"]]

#############
# iloc - position-based (row/column numbers)
# Note: iloc slicing is end-exclusive!
#############

# get first row
df.iloc[0]

# first 3 rows, and last two columns
df.iloc[0:3, -2:]

# single cell
df.iloc[2, 1]
```

### convert columns to specific data type

```py
import pandas as pd

df = pd.DataFrame({
    "order_id": ["1001", "1002", "1003"],
    "order_date": ["2025-01-05", "2025-01-06", "2025-01-06"],
    "price": ["12.50", "9.00", "15.75"],
    "is_refund": ["False", "True", "False"]
})

# all column types are "object"
print(df.dtypes)
print("")

df["order_id"] = df["order_id"].astype("int64", errors="raise")
df["order_date"] = pd.to_datetime(df["order_date"], errors="raise")
df["price"] = pd.to_numeric(df["price"], errors="raise")
df["is_refund"] = df["is_refund"].map({"True": True, "False": False})

print(df.dtypes)
```

```
order_id      object
order_date    object
price         object
is_refund     object
dtype: object

order_id               int64
order_date    datetime64[ns]
price                float64
is_refund               bool
dtype: object
```

### Duplicates

```py
df = pd.DataFrame({
    "order_id": [101, 101, 102, 103, 103, 103],
    "item": ["IPA", "IPA", "Lager", "IPA", "IPA", "Stout"],
    "price": [8, 8, 7, 8, 8, 9]
})

# returns a series of False and True. True means this row is duplicated
# False -> First occurrence
# True  -> All other occurrences
df.duplicated()

# get the rows that are duplicated
df[df.duplicated()]

# only consider a few columns
df[df.duplicated(subset=["order_id", "item"])]

# drop duplicates
df.drop_duplicates(subset=["order_id", "item"], keep="first") # or "last"

# see rows what rows have duplicates. `keep` indicates that we don't care if a row is the first or any other occurrence
df[df.duplicated(subset=["order_id", "item"], keep=False)]

# get all unique rows and count its occurrence
df.groupby(["order_id", "item"]).size().reset_index(name="count")
```

### Grouping

```py
import pandas as pd

data = [
    ("2025-12-20", "Beer", "IPA", 8.00),
    ("2025-12-20", "Beer", "Lager", 7.00),
    ("2025-12-20", "Wine", "Red", 12.00),
    ("2025-12-21", "Beer", "IPA", 8.00),
    ("2025-12-21", "Wine", "White", 11.00),
    ("2025-12-21", "Cocktail", "Margarita", 14.00),
    ("2025-12-21", "Beer", "IPA", 8.00),
]

df = pd.DataFrame(
    data,
    columns=["date", "category", "item", "price"]
)

df["date"] = pd.to_datetime(df["date"])

# groupby and sum will create a series, where the values are prices
revenue_by_category = (
    df
    .groupby("category")["price"]
    .sum()
    .reset_index(name="total_revenue") # rename the price column to "total_revenue"
)

print(revenue_by_category)
print("")

avg_price = (
    df
    .groupby(["category", "item"])["price"]
    .mean()
    .reset_index()
)

print(avg_price)
```

```
   category  total_revenue
0      Beer           31.0
1  Cocktail           14.0
2      Wine           23.0

   category       item  price
0      Beer        IPA    8.0
1      Beer      Lager    7.0
2  Cocktail  Margarita   14.0
3      Wine        Red   12.0
4      Wine      White   11.0 
```

### Reindex

Use `reindex()` when the shape of an operation is correct, but the index is incomplete.

Operations that create Series are usually `group_by`, `value_counts`, `resample`, `pivot_table`, etc

```py
df = pd.DataFrame({
    "day": ["Mon", "Mon", "Wed", "Wed", "Wed"], # only Monday and Wednesday
    "sales": [100, 200, 50, 60, 70]
})

print(df)
print("")

# aggregate (this will create a Series)
daily = df.groupby("day")["sales"].sum()
print(daily)
print("")

# make sure all weekdays are in the data
all_days = ["Mon", "Tue", "Wed", "Thu", "Fri"]
daily = daily.reindex(all_days, fill_value=0)
print(daily)
print("")
```

### Reset_Index

After aggregating operations like `group_by` some of the columns are now in the index.
If, for example, you want to plot the data a `reset_index` will put columns back and create a new numerical index.

```py
df = pd.DataFrame({
    "country": ["US", "US", "DE", "DE", "FR"],
    "year": [2023, 2024, 2023, 2024, 2024],
    "revenue": [10, 12, 7, 9, 5]
})

print(df)

g = df.groupby(["country", "year"])["revenue"].sum()

print(g)

# Key Error!
# g["country"]

# need reset index (make index into columns)
g = g.reset_index()
print(g)
```

```
  country  year  revenue
0      US  2023       10
1      US  2024       12
2      DE  2023        7
3      DE  2024        9
4      FR  2024        5
country  year
DE       2023     7
         2024     9
FR       2024     5
US       2023    10
         2024    12
Name: revenue, dtype: int64
  country  year  revenue
0      DE  2023        7
1      DE  2024        9
2      FR  2024        5
3      US  2023       10
4      US  2024       12
```

### Drop Columns

```py
df = load_flicker_data()
print(df.info())

drop_columns = [
    "Edition Statement",
    "Corporate Author",
    "Corporate Contributors",
    "Former owner",
    "Engraver",
    "Contributors",
    "Issuance type",
    "Shelfmarks",
]

df.drop(drop_columns, inplace=True, axis=1)
df.info()
```

### Convert Columns

```py
df["column"] = df["column"].astype("int")
```

### Factorize

```py
# turn messy categories into numeric IDs
df = pd.DataFrame({
    "beer_style": [
        "IPA", "Stout", "IPA", "Pilsner",
        "Stout", "IPA", "Lager"
    ]
})

# uniques are not needed here
codes, uniques = pd.factorize(df["beer_style"])
df['style_id'] = codes
```

```
  beer_style  style_id
0        IPA         0
1      Stout         1
2        IPA         0
3    Pilsner         2
4      Stout         1
5        IPA         0
6      Lager         3
```

### Get_Dummies

Mostly used in Machine learning when feeding data into a model. A model cannot understand a string like "Urban".

So, we need to perform a technique called `one hot encoding`. `get_dummies()` creates for each store type a yes/no column.

Let's take this example:

```py
df = pd.DataFrame({
    "store_type": ["Urban", "Suburban", "Urban", "Rural"],
    "revenue": [120, 80, 150, 60]
})

print(df)
print("")

X = pd.get_dummies(df["store_type"])

print(X)
print("")

# one liner
print(pd.get_dummies(df, columns=["store_type"], prefix="key"))
```

```
  store_type  revenue
0      Urban      120
1   Suburban       80
2      Urban      150
3      Rural       60

   Rural  Suburban  Urban
0  False     False   True
1  False      True  False
2  False     False   True
3   True     False  False

   revenue  key_Rural  key_Suburban  key_Urban
0      120      False         False       True
1       80      False          True      False
2      150      False         False       True
3       60       True         False      False
```

### Cut

Create a bunch of equal sized bins to categories a numerical column.

```py
df = pd.DataFrame({
    "customer_id": range(1, 13),
    "total_spend": [
        12, 35, 48, 52, 67, 80,
        120, 150, 220, 310, 450, 900
    ]
})

num_bins = 3

# get the bin's intervals
_, bins = pd.cut(df['total_spend'], num_bins=3, retbins=True)


df['spending_category'] = pd.cut(df['total_spend'], num_bins=3, labels=[0,1,2])

print(df)
```

```
    customer_id  total_spend spending_category
0             1           12                 0
1             2           35                 0
2             3           48                 0
3             4           52                 0
4             5           67                 0
5             6           80                 0
6             7          120                 0
7             8          150                 0
8             9          220                 0
9            10          310                 1
10           11          450                 1
11           12          900                 2
```

### Mapping

```py
df = pd.DataFrame({
    "beer_style_raw": [
        "IPA", "India Pale Ale", "I.P.A.",
        "Stout", "Imperial Stout", "Pils"
    ]
})

style_map = {
    "IPA": "IPA",
    "India Pale Ale": "IPA",
    "I.P.A.": "IPA",
    "Stout": "Stout",
    "Imperial Stout": "Stout",
    "Pils": "Pilsner"
}

df['beer_style'] = df.beer_style_raw.map(style_map)

print(df)
```

```
   beer_style_raw beer_style
0             IPA        IPA
1  India Pale Ale        IPA
2          I.P.A.        IPA
3           Stout      Stout
4  Imperial Stout      Stout
5            Pils    Pilsner
```

### Searching/Filtering

The searching or filtering syntax always trips me up. Hopefully this
section will be last one I will write.

Let's make the dataframe example a bit larger.

```py
import pandas as pd

df = pd.DataFrame([
    # menu_id, venue, city, state, category, item, producer, price, currency, abv, rating, in_stock, on_tap, updated_at
    (101, "The Fox & Hound", "New York", "NY", "Beer",  "Guinness Draught",          "Guinness",   9.00, "USD", 4.2, 4.7, True,  True,  "2025-12-20 18:15"),
    (101, "The Fox & Hound", "New York", "NY", "Beer",  "Heineken",                  "Heineken",   8.00, "USD", 5.0, 4.0, True,  False, "2025-12-20 18:15"),
    (102, "Sunset Cantina",  "Austin",   "TX", "Spirit","Margarita (House)",         "Tequila",   14.00, "USD", None, 4.4, True,  False, "2025-12-21 20:05"),
    (102, "Sunset Cantina",  "Austin",   "TX", "Spirit","Old Fashioned",             "Bourbon",   16.00, "USD", None, 4.6, True,  False, "2025-12-21 20:05"),
    (103, "Harbor Grill",    "Miami",    "FL", "Wine",  "Sauvignon Blanc (Glass)",   "Kim Crawford",13.00,"USD", None, 4.1, False, False, "2025-12-22 12:30"),
    (103, "Harbor Grill",    "Miami",    "FL", "Wine",  "Cabernet Sauvignon (Glass)","Josh Cellars",14.00,"USD", None, 4.2, True,  False, "2025-12-22 12:30"),
    (104, "Brew Lab",        "San Diego","CA", "Beer",  "Pliny the Elder",           "Russian River",10.00,"USD", 8.0, 4.8, True,  True,  "2025-12-23 19:10"),
    (104, "Brew Lab",        "San Diego","CA", "Beer",  "Hazy IPA",                  "Local",      9.50, "USD", 6.5, 4.3, True,  True,  "2025-12-23 19:10"),
    (105, "Green Room",      "Seattle",  "WA", "Beer",  "Non-Alcoholic IPA",         "Athletic",   7.00, "USD", 0.5, 4.0, True,  False, "2025-12-23 16:40"),
    (106, "Pasta House",     "Boston",   "MA", "Wine",  "Chianti (Bottle)",          "Ruffino",   42.00, "USD", None, 4.0, True,  False, "2025-12-19 21:00"),
], columns=[
    "menu_id","venue","city","state","category","item","producer",
    "price","currency","abv","rating","in_stock","on_tap","updated_at"
])

# convert to datetime type
df["updated_at"] = pd.to_datetime(df["updated_at"])
# df.info()

##########################
# simple filtering
##########################
df[(df["category"] == "Beer") & (df["state"] == "NY")]

df[df["state"].isin(["NY", "CA", "TX"])]
df[~df["state"].isin(["NY", "CA", "TX"])]

##########################
# string search
##########################
df[df["item"].str.contains("ipa", case=False, na=False)]

df[df["producer"].str.startswith("Ru", na=False)]


##########################
# Numeric filters: between, missing values, top-N
##########################
df[df["price"].between(9, 14)]

df[df["abv"].notna()]

df.nlargest(3, "rating")[["venue","item","rating"]]

##########################
# Datetime range filters
##########################
df[df["updated_at"] >= "2025-12-22"]

df[df["updated_at"].between("2025-12-21", "2025-12-23 18:00")]

##########################
# compound conditions plus sorting
##########################

recommend = df[
    (df["in_stock"]) &
    ((df["category"] == "Beer") | (df["category"] == "Wine")) &
    (df["rating"] >= 4.2)
].sort_values(["rating","price"], ascending=[False, True])

recommend[["venue","category","item","price","rating"]]

##########################
# “Group-wise” filtering
##########################

beer_counts = df[df["category"] == "Beer"].groupby("venue").size()
venues_with_2_beers = beer_counts[beer_counts >= 2].index

df[(df["category"] == "Beer") & (df["venue"].isin(venues_with_2_beers))]
```

#### query

`Query()` gives an easy way to specify conditions. But be aware that it's NOT sql. 

Things that should work are: `and`, `or`, `not`, `in`, comparisons (e.g `==`, `>`, `<=`), parentheses 

Things that might cause troubles: `like`, `between`, subqueries

```py
df.query("category == 'Beer' and in_stock == True and price >= 9")
```


### Copy or Drop

When you filter a dataframe and need to mutate the result use `copy()`.

```py
df = pd.DataFrame({
    "venue": ["A", "B", "C", "D"],
    "category": ["Beer", "Wine", "Beer", "Spirit"],
    "price": [8, 15, 12, 18],
    "rating": [4.0, 4.5, 3.8, 4.7]
})

# most recommended
df_filtered = df[df["price"] >= 10].copy()

# if you really must drop
df_dropped = df.drop(df[df["category"] == "Beer"].index)
```

### Apply

create new data from dataframe as a new column(s). This can be pretty slow sometimes. Have a look here
to speed it up: [pandarallel](https://github.com/nalepae/pandarallel).

```py
df = pd.DataFrame({
    "order_id": [1, 2, 3],
    "subtotal": [42.50, 18.00, 64.25],
    "is_alcohol": [True, False, True]
})

def compute_costs(row):
    tax_rate = 0.10 if row["is_alcohol"] else 0.07
    tax = row["subtotal"] * tax_rate
    tip = row["subtotal"] * 0.20
    total = row["subtotal"] + tax + tip
    return pd.Series([tax, tip, total])

df[["tax", "tip", "total"]] = df.apply(compute_costs, axis=1)
```

### Pivots

Pandas clearly wins in terms of pivoting data to SQL! I sometimes download data from a database, pivot, and then reupload back to the database.

```py
import pandas as pd

df = pd.DataFrame([
    # NYC
    {"date":"2025-12-01", "store":"NYC", "channel":"dine_in",  "category":"Beer",  "revenue":120},
    {"date":"2025-12-01", "store":"NYC", "channel":"takeout",  "category":"Beer",  "revenue":60},
    {"date":"2025-12-02", "store":"NYC", "channel":"dine_in",  "category":"Wine",  "revenue":90},
    {"date":"2025-12-02", "store":"NYC", "channel":"delivery", "category":"Beer",  "revenue":40},
    {"date":"2025-12-03", "store":"NYC", "channel":"delivery", "category":"Wine",  "revenue":35},
    {"date":"2025-12-04", "store":"NYC", "channel":"takeout",  "category":"Wine",  "revenue":50},

    # SF
    {"date":"2025-12-03", "store":"SF",  "channel":"dine_in",  "category":"Beer",  "revenue":110},
    {"date":"2025-12-03", "store":"SF",  "channel":"delivery", "category":"Wine",  "revenue":55},
    {"date":"2025-12-05", "store":"SF",  "channel":"delivery", "category":"Beer",  "revenue":45},
    {"date":"2025-12-05", "store":"SF",  "channel":"takeout",  "category":"Wine",  "revenue":40},
])

df["date"] = pd.to_datetime(df["date"])
df["week"] = df["date"].dt.to_period("W").astype(str)

#######################
# Pivot: revenue by (store, week) x channel
#######################

# aggfuncs
# numeric aggfunc: size, mean, median, min, max, std, var
# counting aggfunc: count (number of non-null rows), size (number of all rows), nunique (number of unique rows)

rev_pivot = df.pivot_table(
    index=["store", "week"],
    columns="channel",
    values="revenue",
    aggfunc="sum", # or ["sum", "mean", "size"],
    fill_value=0,
)


##################
# Normalization: convert each row to shares (each row sums to 1.0)
##################

rev_share = rev_pivot.div(rev_pivot.sum(axis=1), axis=0)

print("Revenue pivot:\n", rev_pivot, "\n")

# the sum per row is 1
print("Row-normalized shares:\n", rev_share.round(3))
```

```
Revenue pivot:
 channel                      delivery  dine_in  takeout
store week                                             
NYC   2025-12-01/2025-12-07        75      210      110
SF    2025-12-01/2025-12-07       100      110       40 

Row-normalized shares:
 channel                      delivery  dine_in  takeout
store week                                             
NYC   2025-12-01/2025-12-07      0.19    0.532    0.278
SF    2025-12-01/2025-12-07      0.40    0.440    0.160
```


### Crosstab

`crosstab()` is a nice quick shortcut to pivoting (when counting) and have normalizing done at the same time


```py
visits = pd.DataFrame([
    {"customer":"c1", "daypart":"Lunch",  "drink":"Beer"},
    {"customer":"c2", "daypart":"Lunch",  "drink":"Beer"},
    {"customer":"c3", "daypart":"Lunch",  "drink":"Wine"},
    {"customer":"c4", "daypart":"Dinner", "drink":"Wine"},
    {"customer":"c5", "daypart":"Dinner", "drink":"Beer"},
    {"customer":"c6", "daypart":"Dinner", "drink":"Cocktail"},
    {"customer":"c7", "daypart":"Late",   "drink":"Beer"},
    {"customer":"c8", "daypart":"Late",   "drink":"Cocktail"},
    {"customer":"c9", "daypart":"Late",   "drink":"Cocktail"},
])

# Counts table
counts = pd.crosstab(index=visits["daypart"], columns=visits["drink"])

# Normalization options
row_pct = pd.crosstab(visits["daypart"], visits["drink"], normalize="index")   # per-daypart distribution
col_pct = pd.crosstab(visits["daypart"], visits["drink"], normalize="columns") # for each drink, where it happens
overall = pd.crosstab(visits["daypart"], visits["drink"], normalize=True)      # overall %

print("Counts:\n", counts, "\n")
print("Row % (normalize='index'):\n", row_pct.round(3), "\n")
print("Col % (normalize='columns'):\n", col_pct.round(3), "\n")
print("Overall % (normalize=True):\n", overall.round(3))
```

```
Counts:
 drink    Beer  Cocktail  Wine
daypart                      
Dinner      1         1     1
Late        1         2     0
Lunch       2         0     1 

Row % (normalize='index'):
 drink     Beer  Cocktail   Wine
daypart                        
Dinner   0.333     0.333  0.333
Late     0.333     0.667  0.000
Lunch    0.667     0.000  0.333 

Col % (normalize='columns'):
 drink    Beer  Cocktail  Wine
daypart                      
Dinner   0.25     0.333   0.5
Late     0.25     0.667   0.0
Lunch    0.50     0.000   0.5 

Overall % (normalize=True):
 drink     Beer  Cocktail   Wine
daypart                        
Dinner   0.111     0.111  0.111
Late     0.111     0.222  0.000
Lunch    0.222     0.000  0.111
```

### Merge, Stack, Concat

```py
import pandas as pd

orders = pd.DataFrame({
    "order_id": [1, 2, 3],
    "customer_id": [10, 20, 10],
    "total": [45.0, 23.5, 67.0]
})

customers = pd.DataFrame({
    "customer_id": [10, 20, 30],
    "name": ["Alice", "Bob", "Charlie"],
    "city": ["NYC", "LA", "SF"]
})

items = pd.DataFrame({
    "order_id": [1, 1, 2, 3, 3],
    "item": ["Beer", "Burger", "Beer", "Wine", "Cheese"],
    "price": [8, 15, 8, 12, 10]
})

######################
# Merge
######################
orders_with_customers = orders.merge(
    customers,
    on="customer_id",
    how="left" # inner, right, outer
)

print(orders_with_customers)
print("")

######################
# Vertical stacking with concat
######################
orders_day_1 = orders.iloc[:2]
orders_day_2 = orders.iloc[2:]

all_orders = pd.concat([orders_day_1, orders_day_2])
print(all_orders)
print("")

######################
# Horizontal stacking with concat
######################
order_totals = orders[["order_id", "total"]]
order_flags = pd.DataFrame({
    "vip": [True, False, True]
})

combined = pd.concat([order_totals, order_flags], axis=1)
print(combined)
print("")

######################
# Stacking - converting columns to rows
######################

# create df with cities as index and two product columns with sales data
sales = pd.DataFrame({
    "Beer": [120, 90],
    "Wine": [80, 110]
}, index=["NYC", "LA"])

# actually name the index and columns
sales.index.name = "city"
sales.columns.name = "product"
print(sales)
print("")

# only one column
print(sales.stack().reset_index(name="sales"))
print("")

# in one line. long means that we are reformatting from columns to rows (wide -> long)
long = (
    sales.rename_axis(index="city", columns="product")
         .stack()
         .reset_index(name="sales")
)
```

```
   order_id  customer_id  total   name city
0         1           10   45.0  Alice  NYC
1         2           20   23.5    Bob   LA
2         3           10   67.0  Alice  NYC

   order_id  customer_id  total
0         1           10   45.0
1         2           20   23.5
2         3           10   67.0

   order_id  total    vip
0         1   45.0   True
1         2   23.5  False
2         3   67.0   True

product  Beer  Wine
city               
NYC       120    80
LA         90   110

  city product  sales
0  NYC    Beer    120
1  NYC    Wine     80
2   LA    Beer     90
3   LA    Wine    110

```

## Series

### Reindex

Use `reindex()` when the shape of an operation is correct, but the index is incomplete.

Operations that create Series are usually `group_by`, `value_counts`, `resample`, `pivot_table`, etc

```py
# The index are dates
ts = pd.Series(
    [10, 12, 15],
    index=pd.to_datetime(["2025-01-01", "2025-01-03", "2025-01-06"])
)

print(ts)

# fill in the missing dates 
full_index = pd.date_range("2025-01-01", "2025-01-06", freq="D")

ts = ts.reindex(full_index, fill_value=0)
print(ts)
```

### MultiIndex

```py
index = pd.MultiIndex.from_tuples(
    [("one", "a"), ("one", "b"), ("two", "a"), ("two", "b")]
)
s = pd.Series(np.arange(1.0, 5.0), index=index)
```

## Bonus

### Download tables from wikipedia

```py
from io import StringIO

import pandas as pd

import requests

url = "https://en.wikipedia.org/wiki/List_of_countries_by_GDP_(nominal)"

headers = {
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) "
                  "AppleWebKit/537.36 (KHTML, like Gecko) "
                  "Chrome/121.0 Safari/537.36",
    "Accept-Language": "en-US,en;q=0.9",
}

resp = requests.get(url, headers=headers, timeout=30)
resp.raise_for_status()

html = StringIO(resp.text)
# note the flavor! pandas prefer lxml for some reason.
tables = pd.read_html(html, flavor="bs4")

print("Num Tables:", len(tables))
for i, t in enumerate(tables):
    print(i, t.shape)
```