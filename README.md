# 🦠 COVID-19 Data Exploration using SQL
![COVID-19-Data-Exploration-using-SQL](covid19.jpg)

## 📌 Project Overview

This project focuses on **exploratory data analysis (EDA) using SQL** on global COVID-19 datasets. The goal was to uncover insights into cases, deaths, and vaccination progress across different countries and regions.

## 🎯 Objectives

* Explore the relationship between **total cases** and **total deaths**.
* Identify countries with the **highest infection rates** relative to population.
* Compare **COVID-19 impact across continents**.
* Track **vaccination progress** globally and by country.
* Perform **rolling calculations** to analyze cumulative trends.

## 🛠️ Tools & Technologies

* **SQL Server** (T-SQL)
* **COVID-19 Dataset** (cases, deaths, vaccinations)

## 🔑 Key Steps & Queries

1. **Case Fatality Rate Analysis**

   ```sql
   SELECT location, date, total_cases, total_deaths,
          (total_deaths * 1.0 / total_cases) * 100 AS DeathPercentage
   FROM PortfolioProject..CovidDeaths
   WHERE location LIKE '%Nigeria%'
   ORDER BY date;
   ```

2. **Countries with Highest Infection Rates**

   ```sql
   SELECT location, population, MAX(total_cases) AS HighestInfectionCount,
          MAX((total_cases * 1.0 / population)) * 100 AS PercentPopulationInfected
   FROM PortfolioProject..CovidDeaths
   GROUP BY location, population
   ORDER BY PercentPopulationInfected DESC;
   ```

3. **Global Death Percentage**

   ```sql
   SELECT SUM(new_cases) AS total_cases, SUM(new_deaths) AS total_deaths,
          (SUM(new_deaths) * 1.0 / SUM(new_cases)) * 100 AS DeathPercentage
   FROM PortfolioProject..CovidDeaths
   WHERE continent IS NOT NULL;
   ```

4. **Vaccination Progress (Rolling Count)**

   ```sql
   SELECT dea.continent, dea.location, dea.date, dea.population,
          vac.new_vaccinations,
          SUM(CONVERT(BIGINT,vac.new_vaccinations)) OVER
          (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS RollingPeopleVaccinated
   FROM PortfolioProject..CovidDeaths dea
   JOIN PortfolioProject..CovidVaccinations vac
        ON dea.location = vac.location
       AND dea.date = vac.date
   WHERE dea.continent IS NOT NULL
   ORDER BY dea.location, dea.date;
   ```

## ✅ Results & Insights

* Case fatality rates varied significantly across countries.
* Some nations had **infection rates exceeding 20%** of their population.
* Vaccination campaigns had a visible effect in reducing severe cases and deaths.
* Rolling vaccination analysis highlighted disparities between developed and developing countries.

## 📂 Dataset Source

* COVID-19 data sourced from [Our world in data](https://ourworldindata.org/covid-deaths)
