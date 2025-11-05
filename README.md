# Netflix-s_Titles-data-analysis-internship-project-

## 📘 Project Overview
This project analyzes Netflix's catalog of Movies and TV Shows using **Python** for data cleaning and **Power BI** for visualization.  
It explores **content trends, genres, audience ratings, and regional distribution** to provide insights into Netflix's global content strategy.

---

## 🧩 Tools & Technologies
- **Python (Pandas, NumPy)** – Data Cleaning & Preprocessing  
- **Power BI Desktop** – Dashboard & Visualization  
- **Power Query** – Data Transformation  
- **DAX Expressions** – KPI Calculations  

---

## 🧹 Data Cleaning (Python)
- Removed missing & duplicate records  
- Deleted invalid entries (like “William Wyler” in the *type* column)  
- Cleaned and standardized date & country columns  
- Exported as `netflix_cleaned.csv`

- import pandas as pd
import numpy as np

df = pd.read_csv("netflix_titles.csv")
print("Dataset Shape:", df.shape)
df.head()

# Remove duplicates
df.drop_duplicates(subset='show_id', inplace=True)

# Handle missing values
df['director'].fillna('Unknown', inplace=True)
df['cast'].fillna('Unknown', inplace=True)
df['country'].fillna('Unknown', inplace=True)
df['rating'].fillna('Not Rated', inplace=True)
df['duration'].fillna('Unknown', inplace=True)

# Convert 'date_added' to datetime
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce')

# Extract year and month
df['added_year'] = df['date_added'].dt.year
df['added_month'] = df['date_added'].dt.month

df[['duration_value', 'duration_unit']] = df['duration'].str.extract(r'(\d+)\s*(\w+)')
df['duration_value'] = pd.to_numeric(df['duration_value'], errors='coerce')

# Convert comma-separated genres and countries to lists
df['genre_list'] = df['listed_in'].str.split(',')
df['country_list'] = df['country'].str.split(',')

# Trim extra spaces
df['genre_list'] = df['genre_list'].apply(lambda x: [i.strip() for i in x] if isinstance(x, list) else [])
df['country_list'] = df['country_list'].apply(lambda x: [i.strip() for i in x] if isinstance(x, list) else [])


df['release_year'] = pd.to_numeric(df['release_year'], errors='coerce', downcast='integer')
df['title'] = df['title'].astype(str).str.strip()
df['type'] = df['type'].astype(str).str.strip()

clean_df = df[[
    'show_id','type','title','director','cast','country_list','date_added',
    'added_year','added_month','release_year','rating','duration_value',
    'duration_unit','genre_list','description'
]]

clean_df.to_csv("netflix_cleaned.csv", index=False)
print("✅ Cleaned data saved as 'netflix_cleaned.csv'")



---

## 📊 Power BI Dashboard
The dashboard includes:
- **KPIs:** Total Movies (6,127), Total TV Shows (2,578), Total Countries (746)
- **Visuals:**
  - Ratings Distribution (Donut Chart)
  - Content Trends (Line Chart)
  - Genre Popularity (Bar Chart)
  - Content Type Comparison (Donut Chart)
  - Regional Distribution (Shape Map using JSON)

---

## 💡 Key Insights
1. Movies dominate Netflix’s library (~71%)  
2. TV Shows form ~29% of total titles  
3. Stand-Up Comedy, Thrillers & Dramas are top genres  
4. TV-MA and TV-14 ratings dominate  
5. Sharp content growth post-2010  
6. Global reach across 746 countries

---

## ⚙️ Challenges Solved
| Problem | Solution |
|----------|-----------|
| Invalid rows in *type* column | Filtered out using Power Query |
| “Size” field missing in map | Used automatic bubble sizing |
| Map not rendering properly | Used Shape Map with custom JSON |
| 1000-row import limit | Loaded full dataset manually |

---

## 📚 Conclusion
This project demonstrates how **Power BI + Python** can be combined for end-to-end data analysis.  
It provides a visual and analytical understanding of Netflix’s evolving content strategy across genres and regions.

---

## 👨‍💻 Author
**[Mohit Sharma]**  
*Data Analyst Intern*  
📧 [danalyst.mohit@gmail.com]   


