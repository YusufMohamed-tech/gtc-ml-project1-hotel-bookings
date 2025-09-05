# 🏨 Hotel Booking Demand Analysis

This project was completed as part of the **Machine Learning Internship** at **Genius Technology Center (GTC)**.  
The goal of this project is to analyze hotel booking data and identify key factors that influence **booking cancellations**.  

---

## 📌 Project Overview
In the world of tourism and hospitality, every cancellation is a potential loss.  
What if we could **predict which bookings are likely to be canceled before they actually happen**?  

This project uses a dataset of **119,390 hotel bookings** from both Resort Hotels and City Hotels.  
It contains details such as:  
- Number of days before arrival (`lead_time`)  
- Whether the booking was canceled (`is_canceled`)  
- Number of nights (weekend + weekday)  
- Guest nationality (`country`)  
- Meal type (`meal`)  
- Booking channel (`distribution_channel`)  
- Number of guests (`total_guests`)  
- Average daily rate (`adr`)  

The ultimate aim is to **understand customer behavior, reduce cancellations, and improve revenue management** for hotels.  

---

## 🛠 Steps Performed

### 1️⃣ Exploratory Data Analysis (EDA)
- Loaded and explored the dataset (`.head()`, `.info()`, `.describe()`).
- Checked missing values with `missingno` and heatmaps.
- Detected outliers in columns like `adr`, `lead_time`, and `stays_in_weekend_nights` using boxplots and the **IQR method**.

### 2️⃣ Data Cleaning
- **Missing Values**:  
  - `children` → filled with median.  
  - `country` → filled with `"unknown"`.  
  - `agent`, `company` → filled with `0`.  
- **Duplicates**: Removed exact duplicate rows.  
- **Outliers**:  
  - `adr` capped at 1000 (to prevent extreme skew).  
  - Removed negative values from numeric columns.  
- **Data Types**:  
  - Converted `reservation_status_date` to `datetime`.  
  - Converted categorical columns to `category` type for efficiency.  

### 3️⃣ Feature Engineering
Created new features to add more business insights:
- `total_guests = adults + children + babies`  
- `total_nights = stays_in_weekend_nights + stays_in_week_nights`  
- `is_family = Yes/No` depending on presence of children/babies  
- Dropped **data leakage columns**: `reservation_status` and `reservation_status_date`.  

### 4️⃣ Encoding Categorical Variables
- **Low-cardinality features** (`meal`, `market_segment`, `distribution_channel`) → One-Hot Encoding.  
- **High-cardinality feature** (`country`) → Frequency Encoding.  
- Converted `is_family` into numeric using `LabelEncoder`.  

### 5️⃣ Train/Test Split
Prepared the dataset for machine learning:  
```python
from sklearn.model_selection import train_test_split

X = df1.drop('is_canceled', axis=1)
y = df1['is_canceled']

X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
