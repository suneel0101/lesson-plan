## What is this class about?
By the end of the class you should have
- learn some more advanced Python techniques
- become comfortable with the Pandas data analysis library
- one of the most important things to know is that 90% of becoming an advanced programmer is being able to learn and use new libraries (rephrase?)

Anytime I've taken a workshop, I found the teacher lectured too much and I didn't get enough hands-on experience.
I'm a big believer in learning by doing, so we'll be coding a lot of the workshop.

## Gauge Python Levels
- How many have gone through Intro Python course/ Learn Python the Hard Way?

## Shell vs Scripting Python [5]
Ask: What's the difference?

Shell:
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
3.
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
1. Write a function that prints all the even numbers between 1 and 10,000. [5]
This reviews FOR LOOPS, IF CONDITIONS, and PRINTING

```python
def print_even_numbers():
    for x in xrange(10001):
        if x % 2 == 0:
           print x
```

2. Write a function that returns a list of the numbers between 1 and 10,000 that are divisible by 3. [5]
```python
This reviews basic use of LISTS
def divisible_by_3():
    numbers = []
    for x in xrange(10001):
        if x % 3 == 0:
           numbers.append(x)
    return numbers
```

3. The same as 2a, but use Python list comprehensions. [5]
This reviews/tests LIST COMPREHENSIONS
```python
def divisible_by_3():
    return [x for x in xrange(10001) if x % 3 == 0]
```

4. Write a function that takes a list of numbers and returns the max of those numbers, don't ues the max() function. [5]
```python
def get_max(numbers):
    max_number = numbers[0]
    for number in numbers:
        if number > max_number:
           max_number = number
    return max_number
```

5. Use the max() function to achieve the same functionality
```python
def get_max(numbers):
    return max(numbers)
```

6. Write a function that returns True if a number is odd or divisble by 7 and False otherwise. [2]
```python
def is_odd_or_div_by_7(number):
    return (number % 2 == 1 or number % 7 == 0)
```

7. Use the function in 4 and list comprehensions to write a function that given a list of numbers returns a sublist of numbers
that are odd or divisible by 7. [2]
```python
def get_sublist_of_numbers_odd_or_div_by_7(numbers):
    return [number for number in numbers if is_odd_or_div_by(number)]
```

## Intermediate Problems
8. [5] Given a list of food orders, e.g. ["burger", "fries", "burger", "tenders", "apple pie"], write a function that takes the list
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


9. [2] Use collections.Counter to achieve the same functionality.
```python
from collections import Counter

def aggregate_counts(order_list):
    return Counter(order_list)
```

10. [5] Write a function that takes the same kind of input as in 1 but instead of returning a dictionary with the counts, it just returns a tuple: the dish that appears the most in the list and the number of times it appears in the list. So the output given the example would be ("burger", 2)

```python
def get_most_popular_order_data(order_list):
    agg_counts = aggregate_counts(order_list)
    return max(agg_accounts.iteritems(), key=lambda: agg_count[1])
```

# Now let's get ready for Data Science. Install Anacondas [10]
## Instructions for Mac: http://docs.continuum.io/anaconda/install.html#mac-install
## Instructions for Windows: http://docs.continuum.io/anaconda/install.html#windows-install
## Setting up your environment: `conda` command line tool:
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
1. Using csv library, read in data from https://raw.githubusercontent.com/suneel0101/data/master/classic-rock/classic-rock-song-list.csv
HINT: Here's the relevant documentation on csv: https://docs.python.org/2/library/csv.html, use `DictReader`
```
>>> import csv
>>> csvfile = open('rock.csv', 'rb')
>>> reader = csv.DictReader(csvfile)
>>> dir(reader)
>>> reader.filenames
```

2a. How many songs are from 1981?
```
>>> rows = [row for row in reader]
>>> len([row for row in rows if row['Release Year'] == '1981'])
61
```

2b. How many songs are from before 1984
```
>>> rows = [row for row in reader]
>>> len([row for row in rows if row['Release Year'] < '1981'])
```

3. What is the earliest release year in the data?
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

4. What are the top 20 songs by play count
HINT: use builtin sorted() function
```
>>> top_20_rows_by_play_count = sorted(rows, key=lambda row: row['PlayCount'], reverse=True)[:20]
>>> top_20_play_count_song_names = [row['Song Clean'] for row in top_20_rows_by_play_count]
["(Don't Fear) The Reaper", 'Layla', 'Back In Black', 'All Right Now', 'Refugee', 'Bad Company', 'Gimme Shelter', "Runnin' Down a Dream", "Jamie's Cryin'", 'Sweet Home Alabama', 'Foreplay (Long Time)', 'Over the Hills and Far Away', 'Who Are You', 'Lights', 'In the Air Tonight', 'Come Sail Away', 'Highway To Hell', 'Rock and Roll', 'Comfortably Numb', "Rock 'n' Roll Fantasy"]
```

5. Who are the top 10 most prolific artists in the data along with the number of their songs that appear in the data?
```
>>> artists = [row['ARTIST CLEAN'] for row in rows]
>>> from collections import Counter
>>> artists_by_play_count = Counter(artists)
>>> artists_ordered_by_play_count = sorted(artists_by_play_count.items(), key=lambda artist_and_count: artist_and_count[1], reverse=True)
>>> artists_ordered_by_play_count[:10]
[('The Beatles', 100), ('Led Zeppelin', 69), ('Rolling Stones', 55), ('Van Halen', 44), ('Pink Floyd', 39), ('Aerosmith', 31), ('The Who', 31), ('Tom Petty & The Heartbreakers', 29), ('AC/DC', 29), ('Bob Seger', 24)]
```

6. How many different artists appear in the data?
```
>>> artists = [row['ARTIST CLEAN'] for row in rows]
>>> len(set(artists))
475
```

7. How many songs does 'Rock'/'rock' appear in the title of?
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

1. How to read in csvs and find out how many songs from 1981
```python
>>> from pandas import read_csv
>>> rock_data = read_csv('rock.csv')
rock data is a DataFrame, super powerful data structure that has some of the traits of lists but with a lot more functionality, as we'll find out
>>> release_years = rock_data[['Release Year',]]
# Think about the [] brackets like a WHERE clause
>>> release_years = release_years[release_years['Release Year']=='1981']
To get the count of the rows,
>>> len(release_years.index)
61, this includes the header
```

2. Let's learn some more useful functions of DataFrames: value_counts
Top 10 most prolific artists?
- more filtering and slicing
```python
>>> rock_data['ARTIST CLEAN'].value_counts()
>>> rock_data['ARTIST CLEAN'].values_counts()[:10]
>>> rock_data['ARTIST CLEAN'].values_counts().head()
also tail()!
```

3. How many songs contain the word 'Rock'/'rock'/'ROCK' in it?
```python
# first lowercase
>>> lowercase_song_titles = rock_data['Song Clean'].apply(lambda x: x.lower())
>>> contains_rock = lowercase_song_titles.str.contains('rock')
>>> contains_rock[contains_rock]
```
Discuss and demonstrate [5]:
-startswith
-lower
-upper
-strip
- how to add columns to the DataFrame
e.g.
```
>>> rock_data['Title'] = rock_data['Song Clean'].apply(lambda x: x.lower())
```

4. What is the earliest release year in the data?
We need to clean the data. Let's use apply. Let's change any nonstring or below 1900 to n/a.
