# ✈️ **Airline Tweet Sentiment Analysis – End-to-End Project**

This is a **two-part project** that combines **Natural Language Processing (NLP) in Python** with **Interactive Dashboards in Power BI**.
The goal is to extract and visualize customer sentiment from real airline tweets.

---

## 🧩 **Project Overview**

| Part | Tool     | Description                                                         |
| ---- | -------- | ------------------------------------------------------------------- |
| 1️⃣  | Python   | Clean, preprocess, and perform sentiment analysis on raw tweet data |
| 2️⃣  | Power BI | Build an interactive dashboard from the sentiment-labeled output    |

---

## 📁 **Part 1 – Python: Sentiment Analysis of Airline Tweets**

### 🔧 **1. Dataset Loading & Cleaning**

```python
import pandas as pd

df = pd.read_csv("Tweets.csv")

df = df[['airline_sentiment', 'airline', 'text', 'tweet_created', 'retweet_count']]
df.rename(columns={
    'airline_sentiment': 'Sentiment',
    'airline': 'Airline',
    'text': 'Tweet',
    'tweet_created': 'Date',
    'retweet_count': 'Retweets'
}, inplace=True)

df.dropna(inplace=True)
df['Date'] = pd.to_datetime(df['Date'])
df['Sentiment'] = df['Sentiment'].astype('category')
df['Airline'] = df['Airline'].astype('category')

df.to_csv("Cleaned_Twitter_Sentiment.csv", index=False)
```

---

### 🧼 **2. Text Preprocessing**

```python
import re, string, nltk
from nltk.corpus import stopwords

nltk.download('stopwords')
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

### 🧠 **3. Sentiment Scoring with TextBlob**

```python
from textblob import TextBlob

df['Polarity'] = df['Clean_Tweet'].apply(lambda t: TextBlob(t).sentiment.polarity)
df['Subjectivity'] = df['Clean_Tweet'].apply(lambda t: TextBlob(t).sentiment.subjectivity)

def classify_sentiment(score):
    if score > 0: return 'positive'
    elif score < 0: return 'negative'
    return 'neutral'

df['Polarity_Sentiment'] = df['Polarity'].apply(classify_sentiment)

df.to_csv("Twitter_Sentiment_Analyzed.csv", index=False)
```

---

### 📊 **4. Exploratory Data Analysis (EDA)**

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.countplot(data=df, x='Polarity_Sentiment', palette='Set2')
plt.title('Tweet Sentiment Distribution')
plt.savefig('Tweet Sentiment Distribution.png')
plt.show()
```

---

### ☁️ **5. Word Cloud by Sentiment**

```python
from wordcloud import WordCloud

for sentiment in ['positive', 'neutral', 'negative']:
    text = ' '.join(df[df['Polarity_Sentiment'] == sentiment]['Clean_Tweet'])
    wc = WordCloud(background_color='white').generate(text)
    plt.imshow(wc, interpolation='bilinear')
    plt.title(f'{sentiment.capitalize()} Word Cloud')
    plt.axis('off')
    plt.show()
```

---

## 📊 **Part 2 – Power BI Dashboard: Visual Sentiment Insights**

This phase uses `Twitter_Sentiment_Analyzed.csv` as the foundation for dynamic reporting in Power BI.

---

### 🚀 **Key Features**

* ✅ Full Data Model with Calendar Table
* ✅ Cleaned Relationships and Filtering Logic
* ✅ Drillthrough & Tooltip Pages
* ✅ Word Cloud via Marketplace Visual
* ✅ Metrics for Tweet Length, Retweets, Sentiment %

---

### 📅 **Calendar Table (DAX)**

```dax
Month      = FORMAT(CalendarTable[Date], "MMMM")
MonthYear  = FORMAT(CalendarTable[Date], "MMM YYYY")
Weekday    = FORMAT(CalendarTable[Date], "dddd")
Year       = YEAR(CalendarTable[Date])
```

---

### 📈 **Dashboard Visuals**

| Visual Type      | Section               | Description                                       |
| ---------------- | --------------------- | ------------------------------------------------- |
| **Card**         | Top KPIs              | Tweet count, % Positive, Avg Length, Avg Retweets |
| **Line Chart**   | Trends                | Sentiment trends by date                          |
| **Donut Chart**  | Sentiment Split       | Distribution of positive, neutral, negative       |
| **Stacked Bar**  | Airline Breakdown     | Sentiment distribution per airline                |
| **Word Cloud**   | Frequently Used Words | Common terms by sentiment                         |
| **Table/Matrix** | Details View          | Drill-down to individual tweets                   |
| **Slicers**      | Filters               | Airline, Sentiment, and Date filters              |

---

### 🧠 **Custom DAX Measures**

#### ✅ Sentiment KPIs

```dax
Positive Tweets = CALCULATE(COUNTROWS(Twitter_Sentiment_Analyzed), Twitter_Sentiment_Analyzed[Polarity_Sentiment] = "positive")

Negative Tweets = CALCULATE(COUNTROWS(Twitter_Sentiment_Analyzed), Twitter_Sentiment_Analyzed[Polarity_Sentiment] = "negative")

Neutral Tweets = CALCULATE(COUNTROWS(Twitter_Sentiment_Analyzed), Twitter_Sentiment_Analyzed[Polarity_Sentiment] = "neutral")

PercentPositive = DIVIDE([Positive Tweets], COUNTROWS(Twitter_Sentiment_Analyzed))
```

#### ✅ Tweet Insights

```dax
Tweet Length = LEN(Twitter_Sentiment_Analyzed[Clean_Tweet])
AverageTweetLength = AVERAGE(Twitter_Sentiment_Analyzed[Tweet Length])
AverageRetweets = AVERAGE(Twitter_Sentiment_Analyzed[Retweets])
```

---

### ✨ **Advanced Features**

| Feature                | Purpose                                                    |
| ---------------------- | ---------------------------------------------------------- |
| 📅 Calendar Table      | Enables month/year-based filtering                         |
| 📌 Drillthrough        | Navigate to tweet-level details by clicking an airline     |
| 💡 Tooltip Pages       | Hover over charts to see Tweet Length, Retweet Count, etc. |
| 🧩 Word Cloud Visual   | Visualizes frequent terms using Marketplace visual         |
| 🔗 Clean Relationships | Ensures cross-filtering and accurate aggregations          |

---

## 🔗 **Live Power BI Report**

👉 [Click here to view the Power BI dashboard](https://app.powerbi.com/groups/me/reports/ccb8dca9-17dc-4f31-989e-aee9c885ae2d/02719f4635e2a51f1ae7?experience=power-bi)

---

## 👩‍💻 **Author**

**Indira Narayanareddygari**
📎 [LinkedIn](https://www.linkedin.com/in/indira-narayanareddygari-analyst061294/)
💼 *Data Analyst | Power BI Developer | SQL & Excel Specialist | Python Enthusiast*

