# 📚 Books Web Scraping & Data Analysis (Python + PySpark)

## 🧩 Overview

This project demonstrates **web scraping, data cleaning, and analysis** using both **Python** and **Apache PySpark**.
The data is collected from the sample website [Books to Scrape](https://books.toscrape.com/), which provides structured book data for practice.

---

## 🚀 Features

* Scrapes book data (title, price, rating, availability, genre) from multiple pages
* Cleans and stores data in a **CSV file**
* Performs **data transformation and analysis using PySpark**
* Demonstrates **ETL (Extract, Transform, Load)** workflow
* Shows conversion of **rating text → numeric values** and **price cleaning**

---

## ⚙️ Tools & Technologies

* **Python 3**
* **Libraries:** `requests`, `BeautifulSoup`, `pandas`, `pyspark`
* **Concepts:** Web Scraping, Data Cleaning, ETL, PySpark DataFrame Operations

---

## 📥 Steps Performed

### **1. Web Scraping (Python + BeautifulSoup)**

* Extracted data from `https://books.toscrape.com/` page by page.

* Collected key fields:

  * `Title`
  * `Price`
  * `Rating`
  * `Availability`
  * `Genre` (retrieved from individual book pages)

* Combined all pages into a single dataset and saved it to **`books.csv`**.

```python
books_data = scrape_all_books()
df = pd.DataFrame(books_data)
df.to_csv('books.csv', index=False)
```

---

### **2. Data Analysis (PySpark)**

Used PySpark for distributed processing and transformation of scraped data.

#### ✅ Loading Data

```python
spark = SparkSession.builder.appName("Books Data Analysis").getOrCreate()
df = spark.read.option("header", True).csv("books.csv")
```

#### ✅ Data Cleaning

* Removed “£” from prices and converted them to `float`
* Converted rating words (`One`, `Two`, `Three`, etc.) into numeric values

```python
df = df.withColumn("Price", regexp_replace("Price", "£", "").cast("float"))
df = df.withColumn("RatingNum",
    when(col("Rating") == "One", 1)
    .when(col("Rating") == "Two", 2)
    .when(col("Rating") == "Three", 3)
    .when(col("Rating") == "Four", 4)
    .when(col("Rating") == "Five", 5)
)
```

#### ✅ Exploratory Analysis

* Displayed books with price > £20
* Filtered high-rated books (`RatingNum >= 4`)
* Used `describe()` for summary statistics

---

## 📊 Insights

* Extracted **500+ book records** across multiple categories.
* Most books are priced under **£30**, with only a few premium ones above £50.
* Ratings are skewed towards **4 or 5 stars**, indicating positive reviews.
* PySpark enabled scalable transformation and filtering on the dataset.

---

## 🧠 Key Learnings

* How to automate **data collection** from a website using BeautifulSoup.
* How to perform **data cleaning and transformation** using PySpark.
* Demonstrated a mini **ETL pipeline** from web to CSV to analytics.

---

## 🗂️ Project Structure

```
├── Scraping.txt             # Python script with scraping and analysis code
├── books.csv                # Output dataset
├── README.md                # Project documentation
```

---

## 💡 Future Improvements

* Add visualization (Matplotlib or Seaborn) for rating and price distributions
* Store data in a SQL/NoSQL database instead of CSV
* Automate scraping via **Airflow** or **cron job**
* Create a **dashboard** in Power BI or Streamlit

---

## 🧢 Author

**Nasih J**
*Data Analyst | Aspiring Data Engineer*
📍 [Books to Scrape](https://books.toscrape.com/) dataset used for educational purposes only.

