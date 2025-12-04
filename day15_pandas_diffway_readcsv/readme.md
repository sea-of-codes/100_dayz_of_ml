# DATA LOADING BASICS IN PANDAS

A complete reference for reading CSV files in Python using Pandas.  
Covers essential parameters and techniques required for real-world datasets.

---

## 1. Importing Pandas
```python
import pandas as pd
```

---

## 2. Opening a Local CSV File
```python
df = pd.read_csv('file.csv')
df.head()
```

Use correct path if file is inside folders:
```python
df = pd.read_csv('/content/data/myfile.csv')
```

---

## 3. Opening a CSV File from a URL
```python
url = "https://raw.githubusercontent.com/datasciencedojo/datasets/master/titanic.csv"
df = pd.read_csv(url)
df.head()
```

---

## 4. `sep` Parameter  
Defines the column separator used in the file.

### Common separators  
- `,` → CSV  
- `\t` → TSV  
- `;` → European datasets  
- `|` → Pipe-separated  

```python
df = pd.read_csv('data.csv', sep=',')
df = pd.read_csv('data.tsv', sep='\t')
df = pd.read_csv('data_pipe.txt', sep='|')
```

---

## 5. `index_col` Parameter  
Set a column as the DataFrame index.

```python
df = pd.read_csv('sales.csv', index_col='order_id')
```

Using index position:
```python
df = pd.read_csv('sales.csv', index_col=0)
```

---

## 6. `header` Parameter  
Controls which row is used as column names.

```python
df = pd.read_csv('data.csv', header=0)   # default
df = pd.read_csv('data.csv', header=1)   # second row becomes header
df = pd.read_csv('data.csv', header=None)  # no header, pandas assigns 0,1,2...
```

---

## 7. `usecols` Parameter  
Load only selected columns.

```python
df = pd.read_csv('bigdata.csv', usecols=['name', 'age', 'salary'])
```

Using index numbers:
```python
df = pd.read_csv('bigdata.csv', usecols=[0, 2, 5])
```

---

## 8. `squeeze` Parameter (Removed in Pandas 2.0)  
Old behavior: returned a Series if CSV had only one column.

Use instead:
```python
df = pd.read_csv('onecol.csv')
s = df.iloc[:, 0]    # equivalent of squeeze
```

---

## 9. `skiprows` / `nrows` Parameters  
### `skiprows`: Skip lines while reading  
```python
df = pd.read_csv('data.csv', skiprows=3)
```

Skip specific rows:
```python
df = pd.read_csv('data.csv', skiprows=[0, 2, 5])
```

### `nrows`: Load only first N rows  
```python
df = pd.read_csv('data.csv', nrows=1000)
```

---

## 10. `encoding` Parameter  
Important for non-English text.

### Common Encodings  
- `"utf-8"`  
- `"latin1"`  
- `"ISO-8859-1"`  
- `"cp1252"`  

```python
df = pd.read_csv('data.csv', encoding='latin1')
```

---

## 11. Skip Bad Lines  
Handles rows with unexpected column counts.

```python
df = pd.read_csv('data.csv', on_bad_lines='skip')
```

Raise error:
```python
df = pd.read_csv('data.csv', on_bad_lines='error')
```

---

## 12. `dtype` Parameter  
Force a specific data type for columns.

```python
df = pd.read_csv('data.csv', dtype={'id': str, 'age': float})
```

---

## 13. Handling Dates  
Convert date columns automatically:

```python
df = pd.read_csv('sales.csv', parse_dates=['date'])
```

Multiple date columns:
```python
df = pd.read_csv('sales.csv', parse_dates=['order_date', 'ship_date'])
```

Define format:
```python
df['date'] = pd.to_datetime(df['date'], format='%Y-%m-%d')
```

---

## 14. `converters` Parameter  
Apply custom transformations while loading.

Example: convert "Yes"/"No" to boolean.

```python
df = pd.read_csv('data.csv', converters={'active': lambda x: 1 if x=="Yes" else 0})
```

Example: clean currency
```python
df = pd.read_csv('sales.csv', converters={'price': lambda x: float(x.replace('$',''))})
```

---

## 15. `na_values` Parameter  
Define additional strings that should be treated as missing values.

```python
df = pd.read_csv('data.csv', na_values=['NA', 'n/a', '-', 'Missing'])
```

---

## 16. Loading a Huge Dataset in Chunks  
Used when dataset is too large for RAM.

```python
chunks = pd.read_csv('huge.csv', chunksize=50000)

for chunk in chunks:
    # process each chunk
    print(chunk.shape)
```

Example: sum a column across chunks

```python
total_sales = 0

for chunk in pd.read_csv('huge.csv', chunksize=50000):
    total_sales += chunk['sales'].sum()

total_sales
```

---

# Notes
- Always inspect the file format before loading.  
- Use `usecols`, `dtype`, `nrows`, and `chunksize` for large datasets.  
- Use `parse_dates` for any timestamp column.  
- Use `on_bad_lines='skip'` for messy data sources.

