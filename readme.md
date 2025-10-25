Mind-Metrics: Sleep, Stress & Academic Performance Analysis

📘 Overview

This project analyzes the relationship between sleep quality, stress levels, and academic performance among students using a survey-based dataset.
It involves data preprocessing, visualization (EDA), and correlation interpretation to uncover behavioral insights.

📂 Dataset

Source: Student Insomnia and Educational Outcomes Dataset (Version 2)

Contains responses on sleep habits, stress levels, and academic outcomes.

Columns include: Sleep Hours, Sleep Quality, Stress Level, and Academic Performance.

⚙️ Preprocessing

Cleaned column names and standardized categories.

Mapped ordinal variables:

Sleep Quality → Very Poor → Very Good (1–5)

Stress Level → No Stress → Extremely High Stress (1–4)

Academic Performance → Poor → Excellent (1–5)

Converted sleep-hour bands to numeric midpoints (e.g., “7–8 hours” → 7.5).

Saved processed data as clean_dataset.csv.

📊 Exploratory Data Analysis

1️⃣ Distribution of Sleep Hours

Most students sleep between 7–8 hours, showing a right-skewed distribution with few extreme short sleepers.

2️⃣ Sleep Quality Distribution
Sleep quality varies widely — majority rated their sleep as Very Poor or Very Good, indicating polarization in responses.

3️⃣ Academic Performance Distribution
Most students report Poor or Below Average performance, showing an overall downward skew in academic outcomes.

4️⃣ Average Academic Performance by Sleep Quality
Students with Average or Good Sleep Quality achieve better academic scores on average compared to those with Very Poor or Very Good sleep (possible over/under-rest effect).

5️⃣ Impact of Stress on Sleep Quality
Higher stress levels generally correlate with lower and more variable sleep quality.

6️⃣ Distribution of Sleep Hours across Stress Levels
As stress increases, average sleep duration slightly decreases, and distribution becomes narrower, showing less variation.

🧠 Key Insights

Better sleep quality → higher academic performance.

High stress → poor sleep quality and shorter sleep duration.

Most students experience below-average academic performance, possibly linked to sleep inconsistency.