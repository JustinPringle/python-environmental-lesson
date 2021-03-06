---
title: Starting With Data
teaching: 40
exercises: 30
questions:
    - " How can I import data in Python?"
    - " What is Pandas?"
    - " Why should I use Pandas to work with data?"
objectives:
    - "Navigate the workshop directory and download a dataset."
    - "Explain what a library is and what libraries are used for."
    - "Describe what the Python Data Analysis Library (Pandas) is."
    - "Load the Python Data Analysis Library (Pandas)."
    - "Use `read_csv` to read tabular data into Python."
    - "Describe what a DataFrame is in Python."
    - "Access and summarize data stored in a DataFrame."
    - "Define indexing as it relates to data structures."
    - "Perform basic mathematical operations and summary statistics on data in a Pandas DataFrame."
    - "Create simple plots."
keypoints:
- pd.read_csv() is used to import tabular data into python
- methods to inspect dataframes are .dtypes(), .shape(), .head(), .tail()
- individual columns from a dataframe can be chosen using ['ColumnName']
- methods for basic statistics on dataframes are .describe(), .min(), .max(), .mean(), ... 
- .groupby() can be used to group a dataframe by categories
---


# Working With Pandas DataFrames in Python

We can automate the process of performing data manipulations in Python. It's efficient to spend time
building the code to perform these tasks because once it's built, we can use it
over and over on different datasets that use a similar format. This makes our
methods easily reproducible. We can also easily share our code with colleagues
and they can replicate the same analysis.

## Starting in the same spot

To help the lesson run smoothly, let's ensure everyone is in the same directory. This should help us avoid path and file name issues. After launching Spyder (remember you can launch Spyder from the Anaconda Navigator), open the project you created in the previous session. You can do this under the _Projects_ tab. 

If you are working in IPython Notebook be sure
that you start your notebook in the workshop directory.

A quick aside that there are Python libraries like [OS
Library](https://docs.python.org/3/library/os.html) that can work with our
directory structure, however, that is not our focus today.

### Our Data

For this lesson, we will be using the Portal Teaching data, a subset of the data
from Ernst et al
[Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA](http://www.esapubs.org/archive/ecol/E090/118/default.htm)

We will be using files from the [Portal Project Teaching Database](https://figshare.com/articles/Portal_Project_Teaching_Database/1314459).
This section will use the `surveys.csv` file that can be downloaded here:
[https://ndownloader.figshare.com/files/2292172](https://ndownloader.figshare.com/files/2292172)
After downloading the file move it to your project/data folder. Remember, this is the folder that should keep the raw data.

We are studying the species and weight of animals caught in plots in our study
area. The dataset is stored as a `.csv` file: each row holds information for a
single animal, and the columns represent:


| Column           | Description                        |
|------------------|------------------------------------|
| record_id        | Unique id for the observation      |
| month            | month of observation               |
| day              | day of observation                 |
| year             | year of observation                |
| plot_id          | ID of a particular plot            |
| species_id       | 2-letter code                      |
| sex              | sex of animal ("M", "F")           |
| hindfoot_length  | length of the hindfoot in mm       |
| weight           | weight of the animal in grams      |

We can open the `surveys.csv` file in a text editor.
The first few rows of our first file look like this:

```
record_id,month,day,year,plot_id,species_id,sex,hindfoot_length,weight
1,7,16,1977,2,NL,M,32,
2,7,16,1977,3,NL,M,33,
3,7,16,1977,2,DM,F,37,
4,7,16,1977,7,DM,M,36,
5,7,16,1977,3,DM,M,35,
6,7,16,1977,1,PF,M,14,
7,7,16,1977,2,PE,F,,
8,7,16,1977,1,DM,M,37,
9,7,16,1977,1,DM,F,34,
```
---
## Pandas in Python
One of the best options for working with tabular data in Python is to use the
[Python Data Analysis Library](http://pandas.pydata.org/) (a.k.a. Pandas). The
Pandas library provides data structures, produces high quality plots with
[matplotlib](http://matplotlib.org/) and integrates nicely with other libraries
that use [NumPy](http://www.numpy.org/) (which is another Python library) arrays.

Remember we can load a library with the `import` statement and give it a nickname. For example we can import the pandas library using the common nickname `pd`:

```python
import pandas as pd
```

To call a function from the pandas library we use `pd.FunctionName`. 


## Reading CSV Data Using Pandas

We will begin by locating and reading our survey data which are in CSV format.
We can use Pandas' `read_csv` function to pull the file directly into a
[DataFrame](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dataframe).

### So What's a DataFrame?

A DataFrame is a 2-dimensional data structure that can store data of different
types (including characters, integers, floating point values, factors and more)
in columns. It is similar to a spreadsheet or an SQL table or the `data.frame` in
R. It has columns, with column headers and rows representing observations. A column needs to be of the same data type, i.e. either numeric or character, but different columns can differ in their data type. That means, pandas DataFrames are ideal to hold observational data tables imported into python.
A DataFrame always has an index (0-based). An index refers to the position of
an element in the data structure.

```python
# note that pd.read_csv is used because we imported pandas as pd
# the 'data/' stands for the subfolder data in our working directory
pd.read_csv("data/surveys.csv")
```

The above command yields the **output** below:

```
record_id  month  day  year  plot_id species_id sex  hindfoot_length  weight
0          1      7   16  1977        2         NL   M               32   NaN
1          2      7   16  1977        3         NL   M               33   NaN
2          3      7   16  1977        2         DM   F               37   NaN
3          4      7   16  1977        7         DM   M               36   NaN
4          5      7   16  1977        3         DM   M               35   NaN
...
35544      35545     12   31  2002       15     AH  NaN              NaN  NaN
35545      35546     12   31  2002       15     AH  NaN              NaN  NaN
35546      35547     12   31  2002       10     RM    F               15   14
35547      35548     12   31  2002        7     DO    M               36   51
35548      35549     12   31  2002        5     NaN  NaN             NaN  NaN

[35549 rows x 9 columns]
```

We can see that there were 33,549 rows parsed. Each row has 9
columns. The first column is the index of the DataFrame. The index is used to
identify the position of the data, but it is not an actual column of the DataFrame.
It looks like  the `read_csv` function in Pandas  read our file properly. However,
we haven't saved any data to memory so we can work with it. We need to assign the
DataFrame to a variable. Remember that a variable is a name for a value, such as `x`,
or  `data`. We can create a new  object with a variable name by assigning a value to it using `=`.

Let's call the imported survey data `surveys_df`:

```python
surveys_df = pd.read_csv("data/surveys.csv")
```

Notice when you assign the imported DataFrame to a variable, Python does not
produce any output on the screen. We can view the value of the `surveys_df`
object by typing its name into the Python command prompt

```python
surveys_df
```

which prints contents like above.
(We can also look at it in the _Variable Explorer_ tab in top right window.)

## Exploring Our Species Survey Data

Again, we can use the `type` function to see what kind of thing `surveys_df` is:

```python
>>> type(surveys_df)
<class 'pandas.core.frame.DataFrame'>
```

As expected, it's a DataFrame (or, to use the full name that Python uses to refer
to it internally, a `pandas.core.frame.DataFrame`).

What kind of things does `surveys_df` contain? DataFrames have an attribute
called `dtypes` that answers this:

```python
>>> surveys_df.dtypes
record_id            int64
month                int64
day                  int64
year                 int64
plot_id              int64
species_id          object
sex                 object
hindfoot_length    float64
weight             float64
dtype: object
```

All the values in a column have the same type. For example, months have type
`int64`, which is a kind of integer. Cells in the month column cannot have
fractional values, but the weight and hindfoot_length columns can, because they
have type `float64`. The `object` type doesn't have a very helpful name, but in
this case it represents strings (such as 'M' and 'F' in the case of sex).

### Useful Ways to View DataFrame objects in Python

There are many ways to summarize and access the data stored in DataFrames,
using attributes and methods provided by the DataFrame object.

To access an attribute, use the DataFrame object name followed by the attribute
name `df_object.attribute`. Using the DataFrame `surveys_df` and attribute
`columns`, an index of all the column names in the DataFrame can be accessed
with `surveys_df.columns`.

Methods are called in a similar fashion using the syntax `df_object.method()`.
As an example, `surveys_df.head()` gets the first few rows in the DataFrame
`surveys_df` using **the `head()` method**. With a method, we can supply extra information in the parentheses to control behaviour.

Let's look at the data using these.

> ## Challenge - DataFrames
>
> Using our DataFrame `surveys_df`, try out the following attributes and methods to see what they return:
>  `.columns`, `.axes`, `.ndim`, `.size`, `.shape`, `.head()`, `.tail()`, `.describe`
> 
> (Remember you can use `?`, e.g. `?surveys_df.head()`, to get some information about an attribute in the console or check the [pandas documentation](http://pandas.pydata.org/pandas-docs/stable/index.html))
> 
> Can you answer the following questions:
> 1. How many rows and how many columns are in `surveys_df`?
> 
> 2. What is the value of  `hindfoot_length` in row 11 and what in the second last row?
> 
> 3. What is the mean value and what the median of `weight`?
> 
> 4. Take note of the output of `.shape` - what format does it
>    return the shape of the DataFrame in?
>    (HINT: More on tuples, [here](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences))
{: .challenge}


## Calculating Statistics From Data In A Pandas DataFrame

We've read our data into Python. Next, let's perform some quick summary
statistics to learn more about the data that we're working with. We might want
to know how many animals were collected in each plot, or how many of each
species were caught. We can perform summary stats quickly using groups. But
first we need to figure out what we want to group by.

Let's begin by exploring our data:

```python
# Look at the column names
surveys_df.columns
```

which **returns**:

```
Index(['record_id', 'month', 'day', 'year', 'plot_id', 'species_id', 'sex',
       'hindfoot_length', 'weight'],
      dtype='object')
```
We can access individual columns of `surveys_df` by giving the column name in `[]`, e.g. `surveys_df['year']` only shows the content of column `year`.

Let's get a list of all the species in our dataset. The `pd.unique` function tells us all of the unique values in the `species_id` column.

```python
pd.unique(surveys_df['species_id'])
```

which **returns**:

```python
array(['NL', 'DM', 'PF', 'PE', 'DS', 'PP', 'SH', 'OT', 'DO', 'OX', 'SS',
       'OL', 'RM', nan, 'SA', 'PM', 'AH', 'DX', 'AB', 'CB', 'CM', 'CQ',
       'RF', 'PC', 'PG', 'PH', 'PU', 'CV', 'UR', 'UP', 'ZL', 'UL', 'CS',
       'SC', 'BA', 'SF', 'RO', 'AS', 'SO', 'PI', 'ST', 'CU', 'SU', 'RX',
       'PB', 'PL', 'PX', 'CT', 'US'], dtype=object)
```

> ## Challenge - Statistics
>
> 1. Create a list of unique plot ID's found in the surveys data. Call it
>   `plot_names`. How many unique plots are there in the data? How many unique
>   species are in the data?
>
> 2. What is the difference between `len(plot_names)` and `surveys_df['plot_id'].nunique()`?
{: .challenge}

## Groups in Pandas

We often want to calculate summary statistics grouped by subsets or attributes
within fields of our data. For example, we might want to calculate the average
weight of all individuals per plot.

We can calculate basic statistics for all records in a single column using the
syntax below:

```python
surveys_df['weight'].describe()
```
gives **output**

```python
count    32283.000000
mean        42.672428
std         36.631259
min          4.000000
25%         20.000000
50%         37.000000
75%         48.000000
max        280.000000
Name: weight, dtype: float64
```

We can also extract one specific metric if we wish:

```python
surveys_df['weight'].min()
surveys_df['weight'].max()
surveys_df['weight'].mean()
surveys_df['weight'].std()
surveys_df['weight'].count()
```

But if we want to summarize by one or more variables, for example sex, we can
use **Pandas' `.groupby` method**. Once we've created a groupby DataFrame, we
can quickly calculate summary statistics by a group of our choice.

```python
# Group data by sex
grouped_data = surveys_df.groupby('sex')
```

The **pandas function `describe`** will return descriptive stats including: mean,
median, max, min, std and count for a particular column in the data. Pandas'
`describe` function will only return summary values for columns containing
numeric data.

```python
# summary statistics for all numeric columns by sex
grouped_data.describe()
# provide the mean for each numeric column by sex
grouped_data.mean()
```

`grouped_data.mean()` **OUTPUT:**

```python
        record_id     month        day         year    plot_id  \
sex                                                              
F    18036.412046  6.583047  16.007138  1990.644997  11.440854   
M    17754.835601  6.392668  16.184286  1990.480401  11.098282   

     hindfoot_length     weight  
sex                              
F          28.836780  42.170555  
M          29.709578  42.995379  

```

The `groupby` command is powerful in that it allows us to quickly generate
summary stats.

> ## Challenge - Summary Data
>
> 1. How many recorded individuals are female `F` and how many male `M`
> 2. What happens when you group by two columns using the following syntax and
>    then grab mean values:
>	- `grouped_data2 = surveys_df.groupby(['plot_id','sex'])`
>	- `grouped_data2.mean()`
> 3. Summarize weight values for each plot in your data. HINT: you can use the
>   following syntax to only create summary statistics for one column in your data
>   `by_plot['weight'].describe()`
>
>
>> ## Did you get #3 right?
>> **A Snippet of the Output from challenge 3 looks like:**
>>
>> ```
>>	plot
>>	1     count    1903.000000
>>	      mean       51.822911
>>	      std        38.176670
>>	      min         4.000000
>>	      25%        30.000000
>>	      50%        44.000000
>>	      75%        53.000000
>>	      max       231.000000
>>          ...
>> ```
> {: .solution}
{: .challenge}

### Quickly Creating Summary Counts in Pandas

Let's next count the number of samples for each species. We can do this in a few
ways, but we'll use `groupby` combined with **a `count()` method**.


```python
# count the number of samples by species
species_counts = surveys_df.groupby('species_id')['record_id'].count()
print(species_counts)
```
Did you notice how we combined all the methods into one statement?

Or, we can also count just the rows that have the species "DO":

```python
surveys_df.groupby('species_id')['record_id'].count()['DO']
```

> ## Challenge - Make a list
>
>  What's another way to create a list of species and associated `count` of the
>  records in the data? Hint: you can perform `count`, `min`, etc functions on
>  groupby DataFrames in the same way you can perform them on regular DataFrames.
{: .challenge}

## Mutating columns

If we wanted to, we could perform math on an entire column of our data. For example let's multiply all weight values by 2 or take the `log` of weight. Remember if we want to apply a function to all elements in an array like object we can use functions from `numpy`, e.g. `np.log()`.

	# multiply all weight values by 2
	surveys_df['weight']*2
	# take the log of all weight values
	np.log(surveys_df['weight'])

We can create a new variable for the 'mutated' weight or adding it as a new column to our dataframe `surveys_df`:
	
	# adding the column `ln_weight` to `surveys_df`
	surveys_df['ln_weight'] = np.log(surveys_df['weight'])
	surveys_df.head()
		
A more practical use of this might be to normalize the data according to a mean, area, or some other value calculated from our data.

## A few words on dates
Dates in python are represented by `date`, `time` and `datetime` objects from the 'datetime' module. 

```python
import datetime as dt
# a date
dt.date(year = 2017, month = 12, day = 17)
# a time
dt.time(hour = 9, minute=30, second=15)
# a datetime
dt.datetime(2017, 12, 17, 9, 30, 15)
```
A defined format for date is important when doing e.g. differences etc.

Let's try:
```python
dt1 = dt.datetime(2017, 12, 17, 9, 30, 00)
dt2 = dt.datetime(2017, 12, 17, 10, 00, 00)
dt2 - dt1
datetime.timedelta(0, 1800)
```

This yields a `timedelta` object with a value of 1800. `datetime` objects are represented in seconds, so the difference is given in seconds, and 1800 sec = 30 min.


Now back to our surveys data. The date is represented by three columns, year, month and day. To combine those columns into one datetime column, Pandas has the function `to_datetime`:

```python
pd.to_datetime(surveys_df[['year', 'month', 'day']])
```
**Error**: 

```
ValueError: cannot assemble the datetimes: day is out of range for month
```
This throws an interesting error: there seems to be a month with a day outside the range 1-30(31)? After a bit of searching we can find that in the year 2000 there is a 31st April in the data, perhaps that should be the 1st of May, so lets replace these wrong dates.

To replace wrong month and year we can use the method `.replace`: 

```python
sy2000=surveys_df[surveys_df.year==2000].replace({'month':{4:5},'day':{31:1}})
```
This creates the dataframe sy2000 which only holds data of the year 2000, but with the correct values for `month` and `day`. To replace the wrong `month` and `day` in `surveys_df` we will use `.where(cond, other)`. 
Entries where `cond` is False are replaced with corresponding value from `other`. Our False condition is that `year` is not 2000, and our `other` is `sy2000`.

```
surveys_df=surveys_df.where(surveys_df.year!=2000, sy2000)
```
Now we can apply the `pd.to_datetime` function again:

```python
# now change date
surveys_dfc['date'] = pd.to_datetime(surveys_dfc[['year','month','day']]
```


