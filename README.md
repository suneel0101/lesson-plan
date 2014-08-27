## What is this class about?
By the end of the class you should have
- learn some more advanced Python techniques
- become comfortable with the Pandas data analysis library
- one of the most important things to know is that 90% of becoming an advanced programmer is being able to learn and use new libraries (rephrase?)

Anytime I've taken a workshop, I found the teacher lectured too much and I didn't get enough hands-on experience.
I'm a big believer in learning by doing, so we'll be coding a lot of the workshop.

## Gauge Python Levels
- How many have gone through Intro Python course/ Learn Python the Hard Way?

## Shell vs Scripting Python
Ask: What's the difference?

### Shell:

1. Open Terminal
2. run
```bash
$ python
>>> "Hello"
Hello
>>> 2 + 2
4
>>> exit()
$
```

### Scripting
Run:
```bash
$ mkdir ~/Documents/ga-python-data-science
$ cd ~/Documents/ga-python-data-science
$ touch test.py
```

In your editor, open up test.py and write the following line:
print "Hello, world!"
Save and close the file.
Then in the terminal, run
$ python test.py
Hello, world!

# Python Warm Up and Diagnostic
- Let's start with a few basic Python problems to warm up and also to get a sense of your Python level

## Basic Problems


1 - Write a function that prints all the even numbers between 1 and 10,000. This reviews FOR LOOPS, IF CONDITIONS, and PRINTING
```python
def print_even_numbers():
    for x in xrange(10001):
        if x % 2 == 0:
           print x
```
2 - Write a function that returns a list of the numbers between 1 and 10,000 that are divisible by 3.
```python
This reviews basic use of LISTS
def divisible_by_3():
    numbers = []
    for x in xrange(10001):
        if x % 3 == 0:
           numbers.append(x)
    return numbers
```
3 - The same as #2, but use Python list comprehensions. This reviews/tests LIST COMPREHENSIONS
```python
def divisible_by_3():
    return [x for x in xrange(10001) if x % 3 == 0]
```
4 - Write a function that takes a list of numbers and returns the max of those numbers, don't ues the max() function.
```python
def get_max(numbers):
    max_number = numbers[0]
    for number in numbers:
        if number > max_number:
           max_number = number
    return max_number
```
5 - Use the max() function to achieve the same functionality
```python
def get_max(numbers):
    return max(numbers)
```
6 - Write a function that returns True if a number is odd or divisble by 7 and False otherwise.
```python
def is_odd_or_div_by_7(number):
    return (number % 2 == 1 or number % 7 == 0)
```
7 - Use the function in #6 and list comprehensions to write a function that given a list of numbers returns a sublist of numbers
that are odd or divisible by 7.
```python
def get_sublist_of_numbers_odd_or_div_by_7(numbers):
    return [number for number in numbers if is_odd_or_div_by(number)]
```
8 - Given a list of food orders, e.g. ["burger", "fries", "burger", "tenders", "apple pie"], write a function that takes the list
and returns a dictionary with the different dishes as keys and the number of times they appear in the list as the values. For example,
Takes ["burger", "fries", "burger", "tenders", "apple pie"] and turns it into
{
   "burger": 2,
   "fries": 1,
   "tenders": 1,
   "apple pie": 1
}
```python
def aggregate_counts(order_list):
    orders_by_count = {}
    for order in order_list:
        if order in orders_by_count:
           orders_by_count[order] += 1
        else:
           orders_by_count[order] = 1
    return orders_by_count
```
9 - Use collections.Counter to achieve the same functionality.
```python
from collections import Counter

def aggregate_counts(order_list):
    return Counter(order_list)
```
10 - Write a function that takes the same kind of input as in #9 but instead of returning a dictionary with the counts, it just returns a tuple: the dish that appears the most in the list and the number of times it appears in the list. So the output given the example would be ("burger", 2)

```python
def get_most_popular_order_data(order_list):
    agg_counts = aggregate_counts(order_list)
    return max(agg_accounts.iteritems(), key=lambda: agg_count[1])
```

## Now let's get ready for Data Science. Install Anacondas
### Instructions for Mac: http://docs.continuum.io/anaconda/install.html#mac-install
### Instructions for Windows: http://docs.continuum.io/anaconda/install.html#windows-install
### Setting up your environment: `conda` command line tool:
```bash
conda create -n ga-python pandas
source activate ga-python
python
>>> import pandas
```

## Python Data Science Worksheet 1
## Objectives
- use csv library to read in data
- use pure Python techniques to extract insights about the data

### Exercises

1 - Using csv library, read in data from https://raw.githubusercontent.com/suneel0101/data/master/classic-rock/classic-rock-song-list.csv
HINT: Here's the relevant documentation on csv: https://docs.python.org/2/library/csv.html, use `DictReader`
```
>>> import csv
>>> csvfile = open('rock.csv', 'rb')
>>> reader = csv.DictReader(csvfile)
>>> dir(reader)
>>> reader.filenames
```
2 - How many songs are from 1981?
```
>>> rows = [row for row in reader]
>>> len([row for row in rows if row['Release Year'] == '1981'])
61
```
3 - How many songs are from before 1984
```
>>> rows = [row for row in reader]
>>> len([row for row in rows if row['Release Year'] < '1981'])
```
4 - What is the earliest release year in the data?
HINT: You might have to account for/clean up dirty data

#### First pass
```
>>> min([int(row['Release Year']) for row in rows if row['Release Year']])
ValueError: invalid literal for int() with base 10: 'SONGFACTS.COM'
```

#### Second pass
```
>>> def is_valid_year(value):
...     try:
...         val = int(value)
...     except ValueError:
...         pass
        else:
            return val
...

>>> release_years = [int(row['Release Year']) for row in rows if is_valid_year(row['Release Year'])]
>>> min(release_years)
1071
```

This doesn't make any sense! Exclude that!

#### Third pass
```
>>> def is_valid_year(value):
...     try:
...         val = int(value)
...     except ValueError:
...         pass
        else:
           if val > 1900:
              return val
>>> release_years = [int(row['Release Year']) for row in rows if is_valid_year(row['Release Year'])]
>>> min(release_years)
1955
```

That makes much more sense!

5 - What are the top 20 songs by play count
HINT: use builtin sorted() function
```
>>> top_20_rows_by_play_count = sorted(rows, key=lambda row: row['PlayCount'], reverse=True)[:20]
>>> top_20_play_count_song_names = [row['Song Clean'] for row in top_20_rows_by_play_count]
["(Don't Fear) The Reaper", 'Layla', 'Back In Black', 'All Right Now', 'Refugee', 'Bad Company', 'Gimme Shelter', "Runnin' Down a Dream", "Jamie's Cryin'", 'Sweet Home Alabama', 'Foreplay (Long Time)', 'Over the Hills and Far Away', 'Who Are You', 'Lights', 'In the Air Tonight', 'Come Sail Away', 'Highway To Hell', 'Rock and Roll', 'Comfortably Numb', "Rock 'n' Roll Fantasy"]
```

6 - Who are the top 10 most prolific artists in the data along with the number of their songs that appear in the data?
```
>>> artists = [row['ARTIST CLEAN'] for row in rows]
>>> from collections import Counter
>>> artists_by_play_count = Counter(artists)
>>> artists_ordered_by_play_count = sorted(artists_by_play_count.items(), key=lambda artist_and_count: artist_and_count[1], reverse=True)
>>> artists_ordered_by_play_count[:10]
[('The Beatles', 100), ('Led Zeppelin', 69), ('Rolling Stones', 55), ('Van Halen', 44), ('Pink Floyd', 39), ('Aerosmith', 31), ('The Who', 31), ('Tom Petty & The Heartbreakers', 29), ('AC/DC', 29), ('Bob Seger', 24)]
```

7 - How many different artists appear in the data?
```
>>> artists = [row['ARTIST CLEAN'] for row in rows]
>>> len(set(artists))
475
```

8 - How many songs does 'Rock'/'rock' appear in the title of?
```
>>> with_rock_in_title = [row for row in rows if 'rock' in row['Song Clean'].lower()]
>>> len(with_rock_in_title)
60
```

## Pandas Makes Life Easier
What are we going to learn?
- how to read in data from a csv
- how to filter and slice DataFrames
- how to get aggregate counts
- how to apply functions to entire columns
- string methods
- some data cleaning using `apply`

1 - How to read in csvs and find out how many songs from 1981
```python
>>> from pandas import read_csv
>>> rock_data = read_csv('rock.csv')
# rock data is a DataFrame, super powerful data structure that has some of the traits of lists but with a lot more functionality, as we'll find out
- I can slice
>>> rock_data[:20]
# I can limit it to just the columns I care about, still a DataFrame
>>> rock_data[['Song Clean', 'Release Year']]
# I can limit it to columns and slice
>>> rock_data[['Song Clean', 'Release Year']][:20]
>>> rock_data[:20][['Song Clean', 'Release Year']]
# Use .ix to index on multiple dimensions
# ix supports mixed integer and label based access
>>> rock_data.ix[0:20, ['Song Clean', 'Release Year']]
- I can just take a look at one column, this is a Series
>>> rock_data[['Song Clean']]
# VS
>>> rock_data['Song Clean']
- I can filter
# Think about the [] brackets like a WHERE clause
>>> release_years = release_years[release_years['Release Year']=='1981']
To get the count of the rows,
>>> len(release_years.index)
61, this includes the header
```
2 - Let's learn some more useful functions of DataFrames: value_counts
Top 10 most prolific artists?
- more filtering and slicing
```python
>>> rock_data['ARTIST CLEAN'].value_counts()
>>> rock_data['ARTIST CLEAN'].values_counts()[:10]
>>> rock_data['ARTIST CLEAN'].values_counts().head()
also tail()!
```
3 - How many songs contain the word 'Rock'/'rock'/'ROCK' in it?
- Explain lambda functions!
- Learn how to use DataFrame.apply
```python
# first lowercase the song titles
>>> lowercase_song_titles = rock_data['Song Clean'].apply(lambda x: x.lower())
# then get a True/False column of whether the title contains 'rock' or not
>>> contains_rock = lowercase_song_titles.str.contains('rock')
# filter down to just where contains_rock is True
>>> contains_rock[contains_rock == True]
>>> contains_rock[contains_rock]
```

### Some useful string methods
- startswith
- lower
- upper
- strip

e.g.:
```
>>> rock_data['Title'] = rock_data['Song Clean'].apply(lambda x: x.lower())
>>> starts_with_party = rock_data['Song Clean'].str.startswith('Party')
>>> uppercase_song_titles = rock_data['Song Clean'].str.upper()
```

4 - What is the earliest release year in the data?
- We need to clean the data. Let's use apply. Let's change any nonstring or below 1900 to n/a.
- Learn how to add columns to the DataFrame.
```python
>>> def is_number(year):
...     try:
...         int(year)
...     except ValueError:
...         return False
...     else:
...         return True
...
>>> def is_before_1900(year):
...     return int(year) < 1900
# removes string outliers and year outliers like 1071 and turns year into an int
>>> clean_year = lambda year: 9999 if not is_number(year) or is_before_1900(year) else int(year)
>>> rock_data["Release Year Clean"] = rock_data["Release Year"].apply(clean_year)
# Use .min() function of dataframes, veryuseful!
>>> rock_data["Release Year Clean"].min()
1955
```
