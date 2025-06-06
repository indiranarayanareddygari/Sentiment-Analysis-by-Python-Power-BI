# **✈️ Sentiment Analysis of Airline Tweets - Part 1 – Powered by Python**

Analyze airline customer sentiment using tweet data and natural language processing techniques with Python.

---

## 📁 **1. Dataset Loading and Cleaning**

```python
import pandas as pd
import numpy as np

# Load the dataset
df = pd.read_csv("Tweets.csv")

# Display basic info
df.info()
df.head()

# Select and rename relevant columns
df = df[['airline_sentiment', 'airline', 'text', 'tweet_created', 'retweet_count']]
df.rename(columns={
    'airline_sentiment': 'Sentiment',
    'airline': 'Airline',
    'text': 'Tweet',
    'tweet_created': 'Date',
    'retweet_count': 'Retweets'
}, inplace=True)

# Handle missing values
df.isnull().sum()
df.dropna(inplace=True)

# Convert data types
df['Date'] = pd.to_datetime(df['Date'])
df['Sentiment'] = df['Sentiment'].astype('category')
df['Airline'] = df['Airline'].astype('category')

# Save cleaned data
df.to_csv("Cleaned_Twitter_Sentiment.csv", index=False)
df.info()
```

---

## 🧼 **2. Text Preprocessing**

```python
import re
import string
import nltk
from nltk.corpus import stopwords
nltk.download('stopwords')
nltk.download('punkt')

stop_words = set(stopwords.words('english'))

def clean_text(text):
    text = text.lower()
    text = re.sub(r"http\S+|www\S+|https\S+", '', text)
    text = re.sub(r'\@w+|\#','', text)
    text = re.sub(r'[%s]' % re.escape(string.punctuation), '', text)
    text = re.sub(r'\d+', '', text)
    text = ' '.join([word for word in text.split() if word not in stop_words])
    return text

df['Clean_Tweet'] = df['Tweet'].apply(clean_text)
```

---

## 🧠 **3. Sentiment Scoring with TextBlob**

```python
from textblob import TextBlob

def get_polarity(text):
    return TextBlob(text).sentiment.polarity

def get_subjectivity(text):
    return TextBlob(text).sentiment.subjectivity

df['Polarity'] = df['Clean_Tweet'].apply(get_polarity)
df['Subjectivity'] = df['Clean_Tweet'].apply(get_subjectivity)

def polarity_to_sentiment(p):
    if p > 0:
        return 'positive'
    elif p < 0:
        return 'negative'
    else:
        return 'neutral'

df['Polarity_Sentiment'] = df['Polarity'].apply(polarity_to_sentiment)

# Save to CSV
df.to_csv("Twitter_Sentiment_Analyzed.csv", index=False)
```

---

## 📊 **4. Exploratory Data Analysis (EDA)**

```python
# Dataset overview
print("Shape:", df.shape)
print(df.info())
print(df['Polarity_Sentiment'].value_counts())
```

---

## 📈 **5. Sentiment Distribution Plot**

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.countplot(data=df, x='Polarity_Sentiment', palette='Set2')
plt.title('Tweet Sentiment Distribution')
plt.xlabel('Sentiment')
plt.ylabel('Count')
plt.savefig('Tweet Sentiment Distribution.png', dpi=300, bbox_inches='tight')
plt.show()
```

---

## ☁️ **6. WordClouds by Sentiment**

```python
from wordcloud import WordCloud

positive = ' '.join(df[df['Polarity_Sentiment'] == 'positive']['Clean_Tweet'])
neutral = ' '.join(df[df['Polarity_Sentiment'] == 'neutral']['Clean_Tweet'])
negative = ' '.join(df[df['Polarity_Sentiment'] == 'negative']['Clean_Tweet'])

plt.figure(figsize=(15, 5))

plt.subplot(1, 3, 1)
plt.imshow(WordCloud(background_color='white').generate(positive))
plt.title('Positive')

plt.subplot(1, 3, 2)
plt.imshow(WordCloud(background_color='white').generate(neutral))
plt.title('Neutral')

plt.subplot(1, 3, 3)
plt.imshow(WordCloud(background_color='white').generate(negative))
plt.title('Negative')

plt.tight_layout()
plt.savefig('sentiment_wordclouds.png', dpi=300, bbox_inches='tight')
plt.show()
```

---

## 📉 **7. Polarity & Subjectivity Distribution**

```python
plt.figure(figsize=(12, 5))

plt.subplot(1, 2, 1)
sns.histplot(df['Polarity'], bins=30, color='purple')
plt.title('Polarity Score Distribution')

plt.subplot(1, 2, 2)
sns.histplot(df['Subjectivity'], bins=30, color='orange')
plt.title('Subjectivity Score Distribution')

plt.tight_layout()
plt.savefig('polarity_subjectivity_distribution.png', dpi=300, bbox_inches='tight')
plt.show()
```

---

## 🔍 **8. Scatter Plot: Polarity vs Subjectivity**

```python
sns.scatterplot(data=df, x='Polarity', y='Subjectivity', hue='Polarity_Sentiment', palette='Set2')
plt.title('Polarity vs Subjectivity by Sentiment')
plt.savefig('polarity_vs_subjectivity_scatter.png', dpi=300, bbox_inches='tight')
plt.show()
```

---

## 👩‍💻 **Author**

**Indira Narayanareddygari**
📎 [LinkedIn](https://www.linkedin.com/in/indira-narayanareddygari-analyst061294/)
💼 *Data Analyst | Power BI Developer | SQL & Excel Specialist | Python Enthusiast*

