##Outlier Detection and Removal using the Z-Score Method##
This project demonstrates a fundamental data preprocessing technique: outlier detection and handling using the Z-Score method, also known as the 3-Sigma Rule, on a numerical feature. This method is especially effective for features that follow a Gaussian (normal) or near-Gaussian distribution.

Overview
The Jupyter notebook walks through the steps to identify and manage outliers in the income column of a customer dataset. The key steps are:

Data Loading and Initial Check: Load the data and inspect the size and initial distribution of the target numerical column (income).

Visualization and Skewness: Use histograms to visualize the distribution and calculate skewness to confirm that the data is approximately normally distributed, or at least symmetric enough, for the Z-score method.

Z-Score Calculation: Calculate the mean and standard deviation of the income column.

Outlier Boundaries (3-Sigma Rule): Define the upper and lower bounds for outliers using the formula:

Upper Limit =μ+3σ

Lower Limit =μ−3σ

Outlier Handling Techniques:

Trimming (Removal): Filter the DataFrame to remove rows where the income falls outside the calculated bounds.

Capping (Winsorization): Replace outlier values with the respective upper or lower limit, thus preserving the row but minimizing the impact of the extreme values.

Final Verification: Use descriptive statistics (.describe()) to confirm the effect of capping on the maximum and minimum values of the income column.

Installation
To run this notebook, you'll need the following Python libraries installed. You can install them using pip:

Bash

pip install pandas numpy seaborn matplotlib
Dataset
This project uses a hypothetical customer dataset named Outlier.csv. The relevant column for outlier detection in this notebook is:

income: A numerical feature representing customer income, which is treated as the primary column for analysis.

Other columns in the dataset include age, gender, days_on_platform, and purchases, which provide context but are not directly modified for outlier treatment in this analysis.

Z-Score Method Explained
The Z-score measures how many standard deviations (σ) a data point is from the mean (μ) of the dataset.

Z-Score= 
σ
(x−μ)
​
 
According to the Empirical Rule (or 3-Sigma Rule), for a normal distribution:

Approximately 68% of the data falls within 1 standard deviation.

Approximately 95% of the data falls within 2 standard deviations.

Approximately 99.7% of the data falls within 3 standard deviations.

Data points with a Z-score greater than +3 or less than −3 are typically considered rare or extreme and are flagged as outliers.

Code Highlights and Steps
1. Initial Analysis & Visualization
The notebook first plots the distribution of income and days_on_platform and calculates the skewness of income (which is around 1.027), indicating a right-skewed distribution.

2. Boundary Calculation
The code explicitly calculates the 3-sigma boundaries for the income column:

Python

upper_limit = df['income'].mean() + 3 * df['income'].std()
lower_limit = df['income'].mean() - 3 * df['income'].std()
3. Outlier Trimming
This approach filters the dataset to keep only the non-outlier rows:

Python

df_changed = df[(df['income'] < upper_limit) & (df['income'] > lower_limit)]
# This results in a DataFrame with fewer rows (e.g., 4959 rows from 5000).
4. Outlier Capping (Winsorization)
The final and most common approach demonstrated is capping, which modifies the outlier values to the boundary limits. This avoids data loss and is often preferred in model training:

Python

df['income'] = np.where(
    df['income'] > upper_limit,
    upper_limit,
    np.where(
        df['income'] < lower_limit,
        lower_limit,
        df['income']
    )
)
The final df['income'].describe() confirms that the maximum value has been successfully reduced to the calculated upper_limit after capping, effectively neutralizing the most extreme outliers' influence.
