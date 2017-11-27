# Web-scraping
## What is Web Scraping ?

[Web Scraping](https://en.wikipedia.org/wiki/Web_scraping) (also termed Screen Scraping, Web Data Extraction, Web Harvesting etc.) is a technique employed to extract large amounts of data from websites whereby the data is extracted and saved to a local file in your computer or to a database in table (spreadsheet) format.
Data displayed by most websites can only be viewed using a web browser. They do not offer the functionality to save a copy of this data for personal use. The only option then is to manually copy and paste the data - a very tedious job which can take many hours or sometimes days to complete. Web Scraping is the technique of automating this process, so that instead of manually copying the data from websites, the Web Scraping software will perform the same task within a fraction of the time.

## Motivation:

On July 21, 2017, the New York Times updated an opinion article called [Trump's Lies](https://www.nytimes.com/interactive/2017/06/23/opinion/trumps-lies.html), detailing every public lie the President has told since taking office. Because this is a newspaper, the information was (of course) published as a block of text.This is a great format for human consumption, but it can't easily be understood by a computer. In this tutorial, we'll extract the President's lies from the New York Times article and store them in a structured dataset.

## Outline of the tutorial:

- What is web scraping?
- Examining the New York Times article
     - Examining the HTML
     - Fact 1: HTML consists of tags
     - Fact 2: Tags can have attributes
     - Fact 3: Tags can be nested
- Reading the web page into Python
- Parsing the HTML using Beautiful Soup
     - Collecting all of the records
     - Extracting the date
     - Extracting the lie
     - Extracting the explanation
     - Extracting the URL
     - Recap: Beautiful Soup methods and attributes
- Building the dataset
     - Applying a tabular data structure
     - Exporting the dataset to a CSV file
- Summary: 16 lines of Python code
## 16 lines of Python code:
Just want to see the code? Here it is:
```python
import requests  
r = requests.get('https://www.nytimes.com/interactive/2017/06/23/opinion/trumps-lies.html')

from bs4 import BeautifulSoup  
soup = BeautifulSoup(r.text, 'html.parser')  
results = soup.find_all('span', attrs={'class':'short-desc'})

records = []  
for result in results:  
    date = result.find('strong').text[0:-1] + ', 2017'
    lie = result.contents[1][1:-2]
    explanation = result.find('a').text[1:-1]
    url = result.find('a')['href']
    records.append((date, lie, explanation, url))

import pandas as pd  
df = pd.DataFrame(records, columns=['date', 'lie', 'explanation', 'url'])  
df['date'] = pd.to_datetime(df['date'])  
df.to_csv('Trump lies.csv', index=False, encoding='utf-8') 
```
Used libraries:
* [BeautifulSoup(bs4)](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
* [Pandas](https://pandas.pydata.org/)

