# Course Enrollment Data Mining

<!-- Project Shields -->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]

<br />
<div align="center">
  <h3 align="center">Course Enrollment Data Mining</h3>

  <p align="center">
    A data mining and predictive analytics project that analyzes university course enrollment behavior, forecasts seat fill rates, and ranks course popularity using historical registration patterns.
    <br />
    The project combines data cleaning, time-series feature engineering, machine learning, and pattern mining to support better course planning for students, advisors, and academic departments.
  </p>
</div>

---

## Table of Contents

- [About The Project](#about-the-project)
- [Project Goals](#project-goals)
- [Dataset](#dataset)
- [Why This Project Matters](#why-this-project-matters)
- [Tech Stack](#tech-stack)
- [Why Each Tool Was Used](#why-each-tool-was-used)
- [Methodology](#methodology)
- [Modeling and Mining Tasks](#modeling-and-mining-tasks)
- [Key Findings](#key-findings)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [Future Improvements](#future-improvements)
- [Acknowledgments](#acknowledgments)

---

## About The Project

This project studies how university courses fill over time during registration and uses that behavior to answer three practical questions:

1. **How can we forecast future enrollment for a course?**
2. **Which courses are most popular, and how should that popularity be measured?**
3. **Are there recurring patterns linking subjects, course levels, instructors, and popularity?**

The analysis was built around course section metadata and time-stamped enrollment logs exported from a university course information system. The workflow merges static course information with dynamic seat-availability snapshots, engineers time-aware features, trains forecasting models, and produces a popularity ranking for downstream decision-making.

This makes the project useful not only as a data mining assignment, but also as a portfolio piece that demonstrates an end-to-end workflow: **data cleaning, feature engineering, time-series modeling, pattern mining, and business interpretation**.

---

## Project Goals

The main goals of this project were to:

- analyze how enrollment changes over time for individual course sections,
- predict fill percentage **3, 7, and 14 days ahead**,
- identify which features best explain future enrollment behavior,
- create a composite **course popularity score**,
- mine frequent patterns related to subject, level, and instructor information,
- and translate technical results into insights that could help **students, academic advisors, and departments plan more effectively**.

---

## Dataset

The project uses university course registration data collected from a student information system and exported from a MongoDB-backed time-series pipeline.

### Data sources used
- **Static course sections data**: one row per course section with metadata such as subject, catalog number, section type, capacity, classroom, instructor fields, and schedule details.
- **Time-series enrollment data**: repeated snapshots of each course section over time, tracking values such as available seats, status, and timestamp of retrieval.

### Dataset overview
- **Initial static course sections:** 7,513 unique sections
- **Initial time-series observations:** 1,060,659
- **Cleaned active courses used for modeling:** 5,612
- **Cleaned time-series observations used for modeling:** 798,136
- **Courses with at least 10 observations after filtering:** 5,606
- **Popularity-score output records:** 11,185 total rows in the exported popularity file
- **Subjects represented:** 130

### What the data captures
The dataset lets you observe how a class changes during registration instead of seeing only the final enrollment outcome. That makes it possible to model:

- fill percentage over time,
- seat-taking velocity,
- waitlist pressure,
- early registration rush,
- time-to-fill,
- subject- and level-based demand,
- and whether a course is online, early morning, evening, undergraduate, or graduate.

### Data preparation highlights
- restricted analysis to **Spring 2025**,
- fixed schema inconsistencies such as misspelled fields,
- removed **31 fully empty columns**,
- dropped cancelled/ghost/low-quality records,
- validated key fields like `classNumber`, `semester`, `year`, and `academicCareer`,
- sorted each course’s observation history chronologically,
- and merged static and dynamic data on **classNumber + semester + year**.

---

## Why This Project Matters

Course registration creates stress for students and administrative burden for advisors. When students cannot tell whether a course is likely to fill soon, they often make inefficient decisions such as overloading backup schedules, delaying major requirements, or repeatedly checking course systems manually.

This project shows how data mining can reduce that uncertainty.

With accurate enrollment forecasting and a well-designed popularity score, institutions could:

- flag courses that are likely to fill quickly,
- identify classes that may need additional sections,
- spot under-enrolled offerings earlier,
- help students plan around demand,
- and give advisors more evidence-based guidance.

In other words, this project turns registration logs into something operationally useful.

---

## Tech Stack

- **Python**
- **Jupyter Notebook**
- **pandas**
- **NumPy**
- **scikit-learn**
- **XGBoost**
- **mlxtend**
- **Matplotlib**
- **Seaborn**
- **CSV / tabular data pipeline**
- **PDF / presentation deliverables**

---

## Why Each Tool Was Used

### Python
Python was the main language because it supports the full pipeline in one ecosystem: data cleaning, feature engineering, machine learning, pattern mining, and visualization.

### Jupyter Notebook
The notebook format was used to organize the project as a reproducible analysis workflow. It allowed preprocessing, modeling, and visualization to stay in one place and made the project easy to present in a class setting.

### pandas
`pandas` was used for loading CSV files, cleaning messy schema fields, merging datasets, reshaping time-series data, grouping by course, and building the final modeling tables.

### NumPy
`NumPy` supported numerical operations such as array handling, missing-value cleanup, and feature calculations used in modeling and scoring.

### scikit-learn
`scikit-learn` was used for preprocessing and evaluation tasks, including:
- one-hot encoding categorical variables,
- computing metrics such as **MAE**, **RMSE**, and **R²**,
- and supporting time-aware validation logic.

### XGBoost
`XGBoost` was chosen for the enrollment forecasting model because course fill behavior is highly non-linear. XGBoost performs well on structured tabular data, handles complex interactions, and is robust even when the data has sparsity or mixed feature types.

### mlxtend
`mlxtend` was used for **Apriori frequent pattern mining** and **association rule generation**, allowing the project to explore whether certain instructor, subject, or course-level combinations repeatedly align with high or low popularity.

### Matplotlib
`Matplotlib` was used to produce the core plots for model performance, rankings, and pattern summaries. It gives precise control over chart layout and export quality.

### Seaborn
`Seaborn` was used to improve the aesthetics and readability of statistical plots and summary visualizations.

### CSV-based pipeline
CSV exports made the workflow easy to inspect, save, and reuse between notebook stages. Intermediate files such as cleaned datasets and computed popularity scores helped keep the pipeline modular.

### PDF / presentation outputs
The final presentation and written project materials were used to turn technical work into a stakeholder-friendly explanation of the problem, methods, and results.

---

## Methodology

### 1. Data Cleaning and Validation
The raw section and time-series datasets were profiled for duplicates, missingness, invalid columns, and schema inconsistencies. The project identified many empty or low-value columns and removed them to reduce noise.

### 2. Cohort Selection
To keep the analysis focused and comparable, the workflow restricted the data to **Spring 2025** and removed courses that were inactive, cancelled, or otherwise unsuitable for modeling.

### 3. Static + Time-Series Merge
Static course metadata was combined with dynamic enrollment snapshots using an intersection merge on shared keys. This created a unified course history where each course could be studied both as a course offering and as a time-evolving enrollment process.

### 4. Time-Series Expansion
Nested or grouped time-series histories were expanded so the model could learn from one observation at a time. This enabled lag calculations, rolling statistics, and future-target construction.

### 5. Feature Engineering
The notebook created temporal, behavioral, and course-level features such as:
- `days_since_start`
- `observation_sequence`
- cyclic encodings for hour/day (`hour_sin`, `hour_cos`, `dow_sin`, `dow_cos`)
- `fill_pct`
- `fill_velocity_per_day`
- `fill_acceleration`
- lagged fill and velocity variables
- rolling means and rolling standard deviations
- `days_since_last_change`
- `course_level`
- `is_online`
- `early_morning`
- `evening`

### 6. Leakage Prevention
The project explicitly removed current-state variables that would leak the answer to the model, including features such as current seats taken and current seats available. Instead, it relied on lagged and historical behavior so that forecasts would be realistic.

### 7. Forecast Modeling
Enrollment forecasting was framed as a supervised learning problem where the target was future fill percentage at different horizons. Separate models were built for **3-day**, **7-day**, and **14-day** forecasts.

### 8. Popularity Scoring
A composite popularity index was built using multiple demand signals rather than relying on final enrollment alone.

The exported score combines:
- **Final Fill Rate**
- **Fill Velocity Score**
- **Time-to-Fill Score**
- **Waitlist Demand Score**
- **Early Rush Score**

The notebook weights them as:

```text
Popularity Score =
  0.30 * Final Fill Rate
+ 0.25 * Fill Velocity Score
+ 0.20 * Time-to-Fill Score
+ 0.15 * Waitlist Demand Score
+ 0.10 * Early Rush Score
```

### 9. Frequent Pattern Mining
The project then converted course/instructor/popularity information into transaction-style data and applied Apriori + association rule mining to look for recurring relationships.

---

## Modeling and Mining Tasks

### Enrollment Forecasting
The forecasting module predicts future enrollment percentage using historical enrollment behavior and static course characteristics.

From the notebook output:
- **798,136 observations** were loaded from **5,612 courses**
- **152 features** were used in the model
- leaky current-state variables were explicitly removed
- training/testing respected timeline order rather than random splitting

For the **3-day forecast**, the notebook reports:
- **Test MAE:** 0.0046 (**0.46%**)
- **Test RMSE:** 0.0161 (**1.61%**)
- **Test R²:** 0.9973
- **Within ±5% tolerance:** 99.34%
- **Within ±10% tolerance:** 99.79%

The notebook also states that the **7-day model** met proposal targets for MAE, RMSE, and R², showing that the forecasting approach remained strong beyond the shortest horizon.

### Course Popularity Ranking
The popularity scoring component generated a structured ranking dataset and reported:
- **11,185 course popularity records** in the exported CSV
- **11,203 total courses analyzed** in the popularity summary output
- **Median popularity score:** 0.154

Example ranking slices shown in the notebook include:
- top **CSC 200-level** courses,
- top **CSC 300-level** courses,
- top **ECE 300-level** courses,
- and top **ECE 400-level** courses.

### Frequent Pattern Mining
The pattern mining stage analyzed relationships among popularity class, subject, level, and instructors.

Reported results include:
- **9,189 transactions** for course-pattern analysis
- **1,947 unique items** in the transaction matrix
- **30 frequent itemsets** at support ≥ 5%
- **45 association rules** at confidence ≥ 50%

The notebook found general popularity/subject/level patterns, but no strong instructor-specific high-popularity association rules at the chosen thresholds.

---

## Key Findings

### 1. Enrollment behavior is predictable from historical momentum
The forecasting stage showed that short-horizon enrollment prediction is highly accurate. This suggests that recent enrollment movement, lagged fill percentage, and velocity-based features capture meaningful momentum in course demand.

### 2. Popularity should not be measured using final fill alone
A course can end nearly full without necessarily filling quickly, and a fast-rising course may matter operationally even before it reaches capacity. The composite score improves on single-metric ranking by blending fill rate, speed, time-to-fill, waitlist demand, and early rush.

### 3. Demand patterns vary by subject and level
The notebook’s ranking outputs showed strong differences across subject codes and course levels. This supports the idea that scheduling and advising strategies should be tailored to department-level behavior rather than treated as uniform across all courses.

### 4. Online and behavioral timing features are useful predictors
The project notes that online-course indicators and momentum-related features contribute meaningfully to prediction. That suggests demand is shaped not just by course identity, but also by how and when students react during the registration window.

### 5. Pattern mining added descriptive value, even when instructor rules were weak
The Apriori analysis did not uncover strong instructor-to-high-popularity rules at the selected thresholds, but it still helped profile overall distributions of popularity by subject and level. That is useful because not every mining task produces strong rules, and recognizing that honestly is part of good analysis.

---

## Project Structure

```text
.
├── courses_data_mining.ipynb          # main notebook with cleaning, modeling, and pattern mining
├── course_popularity_scores.csv       # exported popularity metrics and composite scores
├── Dataset-Report.txt                 # data quality profiling / validation report
├── DSC440_ Data Mining Project.pdf    # project write-up / presentation-style summary
└── README.md                          # project overview for GitHub
```

---

## How to Run

### 1. Clone the repository
```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
```

### 2. Create and activate a Python environment
```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows:
```bash
.venv\Scripts\activate
```

### 3. Install dependencies
```bash
pip install pandas numpy scikit-learn xgboost mlxtend matplotlib seaborn jupyter
```

### 4. Add the source datasets
Place the raw course section and time-series CSV files in the paths expected by the notebook, such as:

```text
Data/Course_data/courses.sections.csv
Data/Course_data/courses.sectionsTS.csv
```

### 5. Launch Jupyter
```bash
jupyter notebook
```

### 6. Open and run the notebook
Run:
```text
courses_data_mining.ipynb
```

The notebook handles:
- data cleaning,
- trimmed dataset generation,
- feature engineering,
- forecasting,
- popularity-score export,
- and pattern mining visualizations.

---

## Future Improvements

There are several ways this project could be extended:

- build a dashboard for students/advisors to monitor likely fill outcomes in real time,
- add semester-over-semester history for stronger generalization,
- include prerequisite pathways and degree requirements as features,
- experiment with more formal time-series or survival-style models,
- add anomaly detection for unusually slow- or fast-filling courses,
- and use the popularity score to recommend section expansion, waitlist alerts, or targeted outreach.

---

## Acknowledgments

This project was developed as part of a **data mining course project** and focuses on applying practical machine learning and pattern mining methods to a real institutional planning problem.

It highlights how course-registration data can be transformed into insights that support:
- better academic advising,
- smarter scheduling,
- and less uncertainty for students during enrollment.

---

<!-- MARKDOWN LINKS & IMAGES -->
[contributors-shield]: https://img.shields.io/github/contributors/your-username/your-repo-name.svg?style=for-the-badge
[contributors-url]: https://github.com/your-username/your-repo-name/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/your-username/your-repo-name.svg?style=for-the-badge
[forks-url]: https://github.com/your-username/your-repo-name/network/members
[issues-shield]: https://img.shields.io/github/issues/your-username/your-repo-name.svg?style=for-the-badge
[issues-url]: https://github.com/your-username/your-repo-name/issues
[license-shield]: https://img.shields.io/github/license/your-username/your-repo-name.svg?style=for-the-badge
[license-url]: https://github.com/your-username/your-repo-name/blob/main/LICENSE
