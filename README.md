
# 🧹 SQL Data Cleaning Project

## 📋 Overview

This project focuses on cleaning and transforming raw data using SQL. The goal is to ensure the dataset is structured, consistent, and ready for analysis or visualization.


## 📁 Project Structure

```
sql-data-cleaning/
│
├── raw_data/      Raw layoffs.csv
├── cleaned_data/           Layoffs modifed.csv
├── scripts/                 # SQL queries used for data cleaning
│
SELECT *
FROM layoffs;

CREATE TABLE layoffs_staging        #Creating new table to not loss the Raw data set
LIKE layoffs;                      

SELECT * 
FROM layoffs_staging;

INSERT layoffs_staging
SELECT *
FROM layoffs;

WITH duplicate_cte AS 
(
SELECT *, 
ROW_NUMBER() OVER(
PARTITION BY company, location, industry, total_laid_off,percentage_laid_off, 'date', stage ,country, funds_raised_millions) AS row_num
FROM layoffs_staging
)
SELECT *
FROM duplicate_cte                                                                                  # identifing duplicates in Dataset and setting Row numbers 
WHERE row_num > 1;


SELECT *
FROM layoffs_staging
WHERE company ='casper';



WITH duplicate_cte AS 
(
SELECT *, 
ROW_NUMBER() OVER(
PARTITION BY company, location, industry, total_laid_off,percentage_laid_off, 'date', stage ,country, funds_raised_millions) AS row_num
FROM layoffs_staging
)
DELETE 
FROM duplicate_cte
WHERE row_num > 1;


CREATE TABLE `layoffs_staging2` (
  `company` text,
  `location` text,
  `industry` text,
  `total_laid_off` int DEFAULT NULL,
  `percentage_laid_off` text,
  `date` text,
  `stage` text,
  `country` text,
  `funds_raised_millions` int DEFAULT NULL,
  `row_num` INT 
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

SELECT *
FROM layoffs_staging2
WHERE row_num > 1;

                                                                                                             ## Adding New table and importing the all dataset to newone

INSERT INTO layoffs_staging2
SELECT *, 
ROW_NUMBER() OVER(
PARTITION BY company, location, industry, total_laid_off,percentage_laid_off, 'date', stage ,country, funds_raised_millions) AS row_num
FROM layoffs_staging;


DELETE                                                                                             ## Delete the duplicates that notneed in Dataset
FROM layoffs_staging2
WHERE row_num > 1;

SELECT*
FROM layoffs_staging2;

##Standardizing data 

SELECT company, TRIM(company)
FROM layoffs_staging2;

UPDATE layoffs_staging2
SET company =TRIM(company);

SELECT DISTINCT industry
FROM layoffs_staging2
ORDER BY 1;
                                                                                                            ## Standardizing date formats
SELECT *
FROM layoffs_staging2 
WHERE industry LIKE 'crypto%';

UPDATE layoffs_staging2
SET industry = 'crypto'
WHERE industry LIKE  'crypto%';

SELECT DISTINCT country, TRIM(TRAILING '.' FROM country)
FROM layoffs_staging2
ORDER BY 1;

UPDATE layoffs_staging2
SET country = TRIM(TRAILING '.' FROM country)
WHERE country LIKE 'united states%';

SELECT 'date',
STR_TO_DATE('date', '%M/%d/%y')
FROM layoffs_staging2;

UPDATE layoffs_staging2
SET `date` = STR_TO_DATE(`date`, '%m/%d/%Y');

ALTER TABLE layoffs_staging2
MODIFY COLUMN `date` DATE;

SELECT *
FROM layoffs_staging2
WHERE total_laid_off IS NULL
AND percentage_laid_off IS NULL;

SELECT DISTINCT industry
FROM layoffs_staging2
WHERE industry IS NULL
OR industry = '';

SELECT *
FROM layoffs_staging2
WHERE company ='airbnb';

SELECT * 
FROM layoffs_staging2 t1
JOIN layoffs_staging2 t2
ON t1.company = t2.company
AND t1.location = t2.location
WHERE (t1.industry IS NULL OR t1.industry='')
AND t2.industry IS NOT NULL;

UPDATE layoffs_staging2 t1
JOIN layoffs_staging2 t2
ON t1.company = t2.company
SET t1.company = t2.industry
WHERE (t1.industry IS NULL OR t1.industry = '')
AND t2.industry is NOT NULL;


SELECT *
FROM layoffs_staging2;

SELECT *
FROM layoffs_staging2
WHERE total_laid_off IS NULL
AND percentage_laid_off IS NULL;


DELETE
FROM layoffs_staging2
WHERE total_laid_off IS NULL
AND percentage_laid_off IS NULL;


SELECT *
FROM layoffs_staging2;


└── README.md               

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


