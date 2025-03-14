# SportPerformanceAnalysis_R
Football Performance Analysis Using Statistical Methods


# Arsenal Football Performance Analysis (2021 Season)

## 📌 Project Overview
This project conducts a statistical analysis of Arsenal Football Club's 2021 season performance, focusing on home vs. away match outcomes and seasonal trends. Using hypothesis testing and data visualization techniques, we aim to determine whether home advantage significantly impacts match results.

## 📊 Dataset
- **Matches Analyzed:** 23 games from the 2021 season.
- **Features:**  
  - `Date` – Match date  
  - `Opponent` – Team Arsenal played against  
  - `Score` – Final match score  
  - `Points` – Points earned (Win: 3, Draw: 1, Loss: 0)  
  - `HomeAdvantage` – Whether the match was played at home (1) or away (0)  
  - `TotalPoints` – Cumulative points earned over the season  

## 📈 Statistical Methods & Analysis
### **1️⃣ Hypothesis Testing**
- **Null Hypothesis:** Home advantage does not significantly affect match outcomes.  
- **Alternative Hypothesis:** Home advantage does influence results.  

- **Student t-test**: Evaluates the statistical difference between home and away performances.  
- **Fisher’s Exact Test**: Determines if there's a significant association between playing at home and winning.  
- **Likelihood Estimation**: Calculates Arsenal’s win probability at home vs. away games.  

### **2️⃣ Data Visualization**
- **Stacked Bar Plot**: Shows the proportion of wins/losses for home and away matches.
- **Scatter Plot**: Tracks Arsenal’s cumulative points over time.
- **Linear Regression**: Analyzes trends between total points and season progression.

## 🔍 Key Findings
- Arsenal performed **better away from home**, with a higher win probability (63.6%) compared to home (45.4%).
- Statistical tests found **no significant advantage** to playing at home.
- The dataset was small, indicating potential variability and the need for further analysis with larger samples.

## 🛠️ Technologies Used
- **R Programming**: Data manipulation, hypothesis testing, visualization
- **ggplot2**: Data visualization
- **Statistical Modeling**: t-tests, Fisher’s test, Likelihood Estimation, Linear Regression

## Repository Structure

📂 Arsenal-Analysis/ │-- 📄 portfolio.pdf # Full assignment report
│-- 📄 arsenal_analysis.R # R script for data analysis
│-- 📄 dataset.csv # Match performance dataset
│-- 📄 README.md # Project description


Future Enhancements
Expanding the dataset to include multiple seasons for a broader trend analysis.
Applying machine learning models to predict match outcomes based on historical data.
Exploring additional factors like player performance and in-game statistics.
