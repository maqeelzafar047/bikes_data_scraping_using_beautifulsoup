# **Bikes Data Scraping from Bikez.com**

A year-wise motorcycle data scraping project using Python and BeautifulSoup.

---

**Developed by:** [**M Aqeel Zafar**](https://github.com/maqeelzafar047)

---

## 📊 **Overview**

This project collects motorcycle model data from **Bikez.com** for each available year using a custom-built web scraper. The data includes motorcycle brand, model, and related details, which is compiled into a single CSV file for use in analysis or machine learning applications.

---

## ⚙️ **Tools & Technologies Used**

- **Python**
- **BeautifulSoup (bs4)** – for HTML parsing
- **Requests** – for sending HTTP requests
- **Pandas** – for working with structured data
- **Jupyter Notebook** – for writing and testing code

---

## 🔄 **Workflow Breakdown**

### 1. **Import Required Libraries**

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
```

These libraries are used to fetch web content (`requests`), parse HTML (`BeautifulSoup`), and store the data in a structured format (`pandas`).

---

### 2\*\*. Fetch All Available Years\*\*

```python
base_url = "https://www.bikez.com"
years_url = "https://www.bikez.com/year/index.php"
html = requests.get(years_url).text
soup = BeautifulSoup(html, 'html.parser')
year_links = soup.select(".col-md-6 li a")
```

The scraper visits the `year/index.php` page and extracts all links corresponding to available years for motorcycle models.

---

### 3. **Loop Through Each Year and Scrape Models**

```python
models = pd.DataFrame()
limit = 50

for year_link in year_links:
    year = year_link.text.strip()
    year_href = year_link['href']
    url = base_url + '/' + year_href
    html = requests.get(url).text
    soup = BeautifulSoup(html, 'html.parser')

    rows = soup.select(".list div")[:limit]
    for row in rows:
        model = row.text.strip()
        models = pd.concat([models, pd.DataFrame({"year": [year], "model": [model]})], ignore_index=True)
```

This part collects bike model data for each year. It limits the number of models per year (for testing or throttling reasons).

---

### 4. **Preview and Display Data**

```python
models.head()
```

This previews the first few rows of the collected data to validate the scraping process.

---

### 5. **Export Data to CSV**

```python
models.to_csv("bikes_data.csv", index=False)
```

The structured data is saved as a CSV file named `bikes_data.csv`, ready for further analysis.

---

## 📆 Output Example

Sample output:

```
year      model
2023      Yamaha R1
2023      Honda CBR1000RR
...
```

---

## 📊 **Use Cases**

- Building a motorcycle model recommender system
- Trend analysis in motorcycle manufacturing
- Historical motorcycle data exploration
- Brand popularity comparison
---

## **Disclaimer**

This project is created by M Aqeel Zafar and is intended for educational purposes only. It is not affiliated with or endorsed by bikez.com. Do not use this script for any commercial or abusive activities. Always respect websites' terms of service.

---
