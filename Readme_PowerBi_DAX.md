# 📊 **Power BI Dashboard – Sentiment Analysis (Part 2)**

This is the **Power BI component** of the complete **Sentiment Analysis Project**, built on top of the cleaned output from the Python NLP phase (📄 *Part 1*). It transforms sentiment-labeled tweets into an **interactive business dashboard** using **advanced DAX** and **visualization techniques**.

---

## 🚀 **Key Features**

* ✅ **Data Modeling**

  * **Primary Dataset:** `Twitter_Sentiment_Analyzed.csv` (from Python phase)
  * Created a **Calendar Table** using DAX for time-based slicing
  * Built a **1-to-many relationship** between `CalendarTable[Date]` and `Twitter_Sentiment_Analyzed[Date]`
  * Enabled **Time Intelligence**, custom date formats, and clean slicers

---

## 📅 **Calendar Table (DAX Fields)**

```dax
Month       = FORMAT(CalendarTable[Date], "MMMM")
MonthYear   = FORMAT(CalendarTable[Date], "MMM YYYY")
Weekday     = FORMAT(CalendarTable[Date], "dddd")
Year        = YEAR(CalendarTable[Date])
```

---

## 📈 **Dashboard Visual Highlights**

| 🔹 Section            | 💡 Visual Type    | 📄 Description                                               |
| --------------------- | ----------------- | ------------------------------------------------------------ |
| **Top KPIs**          | Card Visuals      | Total Tweets, % Positive, Avg Tweet Length, Avg Retweets     |
| **Trends**            | Line Chart        | Sentiment trends over time using `CalendarTable[Date]`       |
| **Sentiment Split**   | Donut Chart       | Overall sentiment distribution (Positive, Neutral, Negative) |
| **Airline Breakdown** | Stacked Bar       | Sentiment split per airline                                  |
| **Word Cloud**        | Word Cloud Visual | Frequently used words in tweets (Marketplace visual)         |
| **Details View**      | Table / Matrix    | Tweet-level breakdown for drill-down                         |
| **Filters**           | Slicers           | Filter by Sentiment, Airline, Date Range                     |

---

## 🧠 **Custom DAX Measures**

### ✅ **Tweet Sentiment KPIs**

```dax
Positive Tweets = 
CALCULATE(
    COUNTROWS(Twitter_Sentiment_Analyzed),
    Twitter_Sentiment_Analyzed[Polarity_Sentiment] = "positive"
)

Negative Tweets = 
CALCULATE(
    COUNTROWS(Twitter_Sentiment_Analyzed),
    Twitter_Sentiment_Analyzed[Polarity_Sentiment] = "negative"
)

Neutral Tweets = 
CALCULATE(
    COUNTROWS(Twitter_Sentiment_Analyzed),
    Twitter_Sentiment_Analyzed[Polarity_Sentiment] = "neutral"
)

PercentPositive = 
DIVIDE([Positive Tweets], COUNTROWS(Twitter_Sentiment_Analyzed))
```

### ✅ **Tweet Insights**

```dax
Tweet Length = 
LEN(Twitter_Sentiment_Analyzed[Clean_Tweet])

AverageTweetLength = 
AVERAGE(Twitter_Sentiment_Analyzed[Tweet Length])

AverageRetweets = 
AVERAGE(Twitter_Sentiment_Analyzed[Retweets])
```

---

## 🗓️ **Date Enhancements for Filtering**

```dax
FormattedDate = FORMAT(Twitter_Sentiment_Analyzed[Date], "DD-MM-YYYY")
Tweet Day     = FORMAT(Twitter_Sentiment_Analyzed[Date], "dddd")
```

---

## ✨ **Advanced Features Implemented**

| 🛠 Feature             | 🎯 Purpose                                              |
| ---------------------- | ------------------------------------------------------- |
| **Calendar Table**     | Enables smooth Month/Year/Weekday filtering             |
| **Drillthrough Pages** | Right-click on Airline → View tweet-level details       |
| **Tooltip Pages**      | Hover on charts to see Avg Tweet Length, Retweets, etc. |
| **Word Cloud Visual**  | Integrated from Power BI Marketplace                    |
| **Relationships**      | Optimized model for accurate cross-filtering            |

---

## 🔗 **Power BI Report Link**

👉 [Click here to view the Power BI Dashboard](https://app.powerbi.com/groups/me/reports/ccb8dca9-17dc-4f31-989e-aee9c885ae2d/02719f4635e2a51f1ae7?experience=power-bi)

---


## 👩‍💻 **Author**

**Indira Narayanareddygari**
📎 [LinkedIn](https://www.linkedin.com/in/indira-narayanareddygari-analyst061294/)
💼 *Data Analyst | Power BI Developer | SQL & Excel Specialist | Python Enthusiast*
