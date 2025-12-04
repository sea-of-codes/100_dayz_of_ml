# UNDERSTANDING DATA

A structured workflow used before modeling.  
The goal is to explore, question, validate, and summarize the dataset so that modeling decisions become correct and efficient.

---

## 1. Asking Basic Questions  
Before touching code, clarify what the dataset contains and what you want from it.

### Key Questions
- What is the objective of the project? (classification, regression, clustering)
- How many rows and columns exist?
- What types of variables exist?
  - numeric (continuous/discrete)
  - categorical (nominal/ordinal)
  - datetime
  - text
- What is the target variable and what features impact it?
- How much missing data exists?
- Are there duplicate rows?
- Are there obvious data quality issues?

### Code
```python
df.shape
df.info()
df.head()
df.describe(include='all')
df.isnull().sum()
df.duplicated().sum()
```

---

## 2. EDA – Univariate Analysis  
Analyze each feature independently.  
The goal is to understand distribution, range, skewness, and anomalies.

### What to Check
- Distribution (normal, skewed, multimodal)
- Min, max, percentiles
- Outliers
- Missing values
- Cardinality of categorical variables

### Common Plots
- Histogram  
- Boxplot  
- Countplot (for categorical features)  

### Code
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Numeric histogram
sns.histplot(df['col'], kde=True)
plt.show()

# Boxplot for outliers
sns.boxplot(x=df['col'])
plt.show()

# Categorical countplot
sns.countplot(x=df['category_col'])
plt.show()
```

---

## 3. EDA – Multivariate Analysis  
Explores *relationships* between multiple variables.  
The purpose is to detect correlation, interactions, and dependencies.

### What to Check
- Numeric vs Numeric: correlation, scatterplots  
- Categorical vs Numeric: boxplots, violin plots  
- Categorical vs Categorical: cross-tabulation, heatmaps  
- Target relationships: which features influence the target?

### Common Plots
- Heatmap (correlation matrix)
- Pairplot
- Scatterplot
- Group-wise summaries

### Code
```python
# Correlation matrix
plt.figure(figsize=(12,6))
sns.heatmap(df.corr(), annot=False, cmap='viridis')
plt.show()

# Scatterplot
sns.scatterplot(x=df['feature1'], y=df['feature2'])
plt.show()

# Grouped statistics
df.groupby('category')['numeric_col'].mean()
```

---

## 4. Pandas Profiling (YData Profiling)  
Generates an automated EDA report with distributions, correlations, missing data patterns, and warnings.

### Installation (Colab)
```python
!pip install ydata-profiling
```

### Generate Profile Report
```python
from ydata_profiling import ProfileReport

profile = ProfileReport(df, title="Pandas Profiling Report", explorative=True)
profile
```

### Export to HTML
```python
profile.to_file("profile_report.html")
```

---

## Notes
- Automated profiling tools are helpful but should never replace manual analysis.  
- Use univariate + multivariate EDA to decide feature engineering, transformations, imputation strategy, and model selection.

