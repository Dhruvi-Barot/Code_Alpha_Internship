# Code_Alpha_Internship

# Task A Web Scrapping 


import requests
from bs4 import BeautifulSoup
import pandas as pd
import matplotlib.pyplot as plt

url = "http://books.toscrape.com/"
res = requests.get(url)
soup = BeautifulSoup(res.content, 'html.parser')

books = soup.find_all('article', class_='product_pod')

book_data = []
for book in books:
    title = book.h3.a['title']
    price = book.find('p', class_='price_color').text
    availability = book.find('p', class_='instock availability').text.strip()
    rating_tag = book.find('p', class_='star-rating')
    rating = rating_tag['class'][1]  # The second class is the actual rating
    book_data.append({
        'Title': title,
        'Price': price,
        'Availability': availability,
        'Rating': rating
    })

df = pd.DataFrame(book_data)
print(df.head())


# Task B Exploratory Data Analysis


df.info() 
df.describe() 
df.isnull().sum() 
df.duplicated().sum()
print(df.columns)
rating_map = {
    'One': 1,
    'Two': 2,
    'Three': 3,
    'Four': 4,
    'Five': 5
}

df['Rating'] = df['Rating'].map(rating_map)df['Rating'].map(rating_map)
df.head()



# Task C Data Visulization 



df['Rating'].value_counts().sort_index().plot(kind='bar')
plt.title('Book Ratings Distribution')
plt.xlabel('Rating')
plt.ylabel('Count')
plt.show()
