
# 🧹 SQL Data Cleaning & Exploratory Data Analysis (EDA)

## 📋 Overview

This project focuses on cleaning and transforming raw data using SQL. The goal is to ensure the dataset is structured, consistent, and ready for analysis or visualization.
Exploratory Data Analysis (EDA), 
is the process of analyzing and summarizing the main characteristics of a dataset, often using visual methods and descriptive statistics. It serves as a crucial first step in the data analysis pipeline, enabling data scientists and analysts to better understand the structure, patterns, and relationships within the data before applying any formal modeling techniques.

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
# 📊 Exploratory Data Analysis (EDA) – [Layoffs staging 2]

Welcome to the Exploratory Data Analysis (EDA) for the **[Layoffs staging 2]** dataset. This project focuses on understanding the structure, patterns, and relationships within the dataset through visualization and statistical summaries.

## 🔍 Project Overview

The primary goal of this EDA is to:
- Understand the data distribution
- Identify and handle missing or inconsistent values
- Detect outliers or anomalies
- Explore relationships between features
- Prepare insights for potential modeling or further analysis

## 📁 Repository Structure

SELECT * 
FROM layoffs_staging2;

SELECT MAX(total_laid_off), MAX(percentage_laid_off)                                         ##the maximum values of total_laid_off and percentage_laid_off from the layoffs_staging2
FROM  layoffs_staging2;                                                                          

SELECT * 
FROM layoffs_staging2
WHERE percentage_laid_off = 1
ORDER BY funds_raised_millions DESC;


SELECT company, SUM(total_laid_off)
FROM layoffs_staging2                                                               ###the total number of layoffs per company and ordering the results from highest to lowest
GROUP BY company
ORDER BY 2 DESC;

SELECT MIN(`date`), MAX(`date`)                                                   ##the earliest and latest dates in the layoffs_staging2
FROM layoffs_staging2;

SELECT industry, SUM(total_laid_off)                                              ## total layoffs by industry
FROM layoffs_staging2
GROUP BY industry
ORDER BY 2 DESC;

SELECT *
FROM layoffs_staging2;

SELECT country, SUM(total_laid_off)
FROM layoffs_staging                                                            ## Total layoffs by the country
GROUP BY country                                                                         
ORDER BY 2 DESC;

SELECT YEAR(`date`), SUM(total_laid_off)                                       ##total layoffs per year, ordered from most to least
FROM layoffs_staging2
GROUP BY YEAR(`date`)
ORDER BY 2 DESC;



SELECT stage, AVG(percentage_laid_off)                                     ##the average percentage of layoffs
FROM layoffs_staging2
GROUP BY stage
ORDER BY 2 DESC;

SELECT company,AVG(total_laid_off)
FROM layoffs_staging2
GROUP BY company
ORDER BY 2 DESC;



SELECT * 
FROM layoffs_staging2;

SELECT SUBSTRING(`date` ,1,7) AS `MONTH` , SUM(total_laid_off)                           
FROM layoffs_staging2
WHERE  SUBSTRING(`date` ,1,7) IS NOT NULL
GROUP BY `MONTH`
ORDER BY 1 ASC
;

WITH Rolling_Total AS
(
SELECT SUBSTRING(`date` ,1,7) AS `MONTH` , SUM(total_laid_off) AS total_off                   ##CTE,Aggregates total layoffs per month
FROM layoffs_staging2
WHERE  SUBSTRING(`date` ,1,7) IS NOT NULL
GROUP BY `MONTH`
ORDER BY 1 ASC
)
SELECT `MONTH` , total_off
,SUM(total_off) OVER(ORDER BY `MONTH`) AS rolling_total                                                       
FROM Rolling_Total;

SELECT company, YEAR(`date`), SUM(total_laid_off)
FROM layoffs_staging2
GROUP BY company, YEAR(`date`)
ORDER BY 3 DESC;

WITH  Company_Year (company, years,  total_laid_off) AS
(
SELECT company, YEAR(`date`), SUM(total_laid_off)                                               ##Aggregates layoffs by company & year
FROM layoffs_staging2                                                                           ##Ranks companies within each year based on layoffs using DENSE_RANK
GROUP BY company, YEAR(`date`)                                                                  ##Filters to the top 5 companies per year
), Company_Year_Rank AS
(SELECT *, DENSE_RANK() OVER (PARTITION BY years ORDER BY total_laid_off DESC) AS Ranking
FROM company_year
WHERE years IS NOT NULL
)
SELECT *
FROM Company_Year_Rank
WHERE Ranking <= 5



  





