🌍 World Population SQL Project – README

This project analyzes global population trends using SQL queries on a dataset named WORLD_POPULATION_DATA_3 within the KISHOR_BANKAR database. It explores population growth across time, continents, and individual countries with insights from 1970 to 2023.

📁 Database & Table Setup

CREATE DATABASE KISHOR_BANKAR;
USE KISHOR_BANKAR;
SELECT * FROM WORLD_POPULATION_DATA_3;

🔍 Queries and Descriptions

1. World Population in 2023

Retrieves the total global population as of 2023.

SELECT SUM(POPULATION_2023) FROM WORLD_POPULATION_DATA_3;

2. World Population Growth (2000–2023)

Calculates the total increase in global population between 2000 and 2023.

SELECT SUM(POPULATION_2023) - SUM(POPULATION_2000) AS WORLD_POPULATION_GROWTH FROM WORLD_POPULATION_DATA_3;

3. Continent-wise Population (2023)

Shows the total population per continent in 2023.

SELECT CONTINENT, SUM(POPULATION_2023) AS POPULATION_2023
FROM WORLD_POPULATION_DATA_3
GROUP BY CONTINENT
ORDER BY CONTINENT ASC;

4. Top 5 Populated Countries in Asia (2023)

Lists the five most populated Asian countries in 2023.

SELECT COUNTRY, SUM(POPULATION_2023)
FROM WORLD_POPULATION_DATA_3
WHERE CONTINENT = 'ASIA'
GROUP BY COUNTRY
ORDER BY SUM(POPULATION_2023) DESC
LIMIT 5;

5. India’s Population Growth by Census (1970–2020)

Displays India’s population growth across each census decade.

SELECT COUNTRY,
       (POPULATION_2020 - POPULATION_2010) AS '10-20 growth',
       (POPULATION_2010 - POPULATION_2000) AS '00-10 growth',
       (POPULATION_2000 - POPULATION_1990) AS '90-00 growth',
       (POPULATION_1990 - POPULATION_1980) AS '80-90 growth',
       (POPULATION_1980 - POPULATION_1970) AS '70-80 growth'
FROM WORLD_POPULATION_DATA_3
WHERE COUNTRY = 'India';

6. Top 5 Populated Countries by Continent (2023)

Uses window function to rank top 5 populated countries per continent.

SELECT * FROM (
    SELECT DENSE_RANK() OVER (PARTITION BY CONTINENT ORDER BY POPULATION_2023 DESC) AS CONTINENT_RANK,
           CONTINENT, COUNTRY, POPULATION_2023, WORLD_PERCENTAGE
    FROM WORLD_POPULATION_DATA_3
) AS T
WHERE CONTINENT_RANK <= 5;

7. 10 Countries with the Least Population (2023)

Ranks and lists the 10 least populated countries in 2023.

SELECT DENSE_RANK() OVER (ORDER BY POPULATION_2023 ASC) AS DENSE_RANK_,
       CONTINENT, COUNTRY, POPULATION_2023
FROM WORLD_POPULATION_DATA_3
ORDER BY POPULATION_2023 ASC
LIMIT 10;

8. Population Growth in 50 Years (1970–2020)

Calculates 50-year population growth per country in millions.

SELECT CONTINENT, COUNTRY,
       (POPULATION_2020 - POPULATION_1970)/1000000 AS growth_50years_IN_MILLIONS
FROM WORLD_POPULATION_DATA_3;

🧰 Tools Used

SQL (Window functions, Aggregates, Filtering)

MySQL Workbench / SQL Server

Dataset: WORLD_POPULATION_DATA_3 (custom structured table)

📌 Project Goal

To generate data-driven insights on population trends across countries and continents using SQL queries, enabling informed understanding of global demographic shifts over time.

Created by: Kishor BankarDate: April 2025

