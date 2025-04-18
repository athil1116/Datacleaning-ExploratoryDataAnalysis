
# 🧹 SQL Data Cleaning Project

## 📋 Overview

This project focuses on cleaning and transforming raw data using SQL. The goal is to ensure the dataset is structured, consistent, and ready for analysis or visualization.


## 📁 Project Structure

```
sql-data-cleaning/
│
├── raw_data/                # Original, unprocessed datasets
├── cleaned_data/            # Output after cleaning operations
├── scripts/                 # SQL queries used for data cleaning
│   ├── step1_remove_nulls.sql
│   ├── step2_standardize_dates.sql
│   └── ...
├── documentation/           # Data dictionary, ERDs, etc.
└── README.md                # You're here!
```

---

## 🛠️ Tools & Technologies

- **SQL ( MySQL /)** – core language for data cleaning
- **DBMS**: [SQL]


---

## 🧽 Cleaning Techniques Applied

- Removing duplicate records
- Handling NULL/missing values
- Standardizing date formats
- Fixing inconsistent category values (e.g., "USA" vs "United States")
- Data type conversions
- Filtering out irrelevant data

---

## 🔄 How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/sql-data-cleaning.git
   cd sql-data-cleaning
   ```

2. Set up your database and import the raw data.

3. Run the SQL scripts in the `scripts/` directory in order:
   ```sql
   \i scripts/step1_remove_nulls.sql
   \i scripts/step2_standardize_dates.sql
   -- and so on...
   ```

4. Cleaned data will be available in your target schema/table or in exported CSV files.

---

## 🧠 Learnings

Through this project, I practiced:
- Writing efficient SQL queries
- Data profiling & diagnostics
- Understanding of data integrity and normalization

---

## 📬 Contact

For questions, suggestions, or collaborations:

**Your Name**  
[LinkedIn](https://linkedin.com/in/yourname) | [Email] athilchandramoahn@gamail.com


