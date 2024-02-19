# Fuzzy Wuzzy Matching Project
## Introduction
The Fuzzy Wuzzy Matching Project aims to reconcile sales point records from two distinct databases, BASE A and BASE B, by employing fuzzy matching techniques. The objective is to link establishments from a survey dataset with those from a client visit dataset, even in the absence of exact matches.

## Datasets
BASE A: A CSV dataset containing 3,747 records from surveys conducted by Premise.
BASE B: An XLSX dataset with 6,039 records from client visits to various sales points.
Both datasets include demographic and geographic information such as the establishment's name, owner's name, address, city, and GPS coordinates.

## Methodology
### Data Preparation
Data Importation: Data is imported into Pandas DataFrames from local storage paths using Python's Pandas library.
Data Cleaning and Transformation: String data types are converted to lower case and stripped of leading/trailing spaces to ensure consistency. The columns of type 'object' are converted to 'string' for better processing.
Data Profiling: Utilization of the pandas_profiling library to generate comprehensive reports for both datasets, providing insights into the data quality and distributions.
### Database Setup
A connection to MySQL database is established using SQLAlchemy to enable SQL transactions for data manipulation.
### Matching Process
Fuzzy Matching Attempt: After an initial lack of exact matches, the fuzz.ratio function from the FuzzyWuzzy library is applied to match the "NombreEstablecimiento" field.
GPS Data Standardization: Custom functions are defined to clean and normalize the GPS coordinates for comparison.
Enhanced Matching: An enhanced fuzzy match function is created, taking into account multiple attributes such as "NombreEstablecimiento", "Ciudad", and "Direccion", using a weighted average of fuzzy match scores.
#### Results
A total of 1,601 high-quality matches are identified with minimal repetition.
The MySQL query confirms 1,656 matches, showcasing the effectiveness of the fuzzy matching process.
![Local Image](result2.jpg)
![Local Image](result1.jpg)
### Code Snippets
Here's an example of the data cleaning and transformation step:

```python
def limpiar_dataframe(df):
    for columna in df.columns:
        if df[columna].dtype == 'object':
            df[columna] = df[columna].astype(str).lower().strip()
            df[columna] = df[columna].astype("string")
    return df

base_a_cleaned = limpiar_dataframe(base_a.copy())
base_b_cleaned = limpiar_dataframe(base_b.copy())
```

Finally, the fuzzy match was conducted with "NombreEstablecimiento", "Ciudad", and "Direccion". A function was created that utilizes the `fuzz.ratio` function from the FuzzyWuzzy library and weights the match of these three attributes to enhance assurance. The results were collected in MySQL Workbench, yielding 1,601 matches of high quality with only one repetition. This function effectively solves the problem of matching two databases.

### Enhanced Fuzzy Matching Function

```python
def enhanced_fuzzy_match(row):
    best_match = None
    best_score = 0
    
    for _, match_row in base_b_df.iterrows():
        score1 = fuzz.ratio(row['NombreEstablecimiento'], match_row['NombreEstablecimiento'])
        score2 = fuzz.ratio(row['Ciudad'], match_row['Ciudad'])
        score3 = fuzz.ratio(row['Direccion'], match_row['Direccion'])
        
        weighted_score = (score1 + score2 + score3) / 3.0  # Updated the average calculation

        if weighted_score > best_score:
            best_score = weighted_score
            best_match = match_row['ID']
            
    if best_score >= 85:
        return best_match  
    return None
```

## Conclusion and Future Directions
The fuzzy matching process has proven successful in linking datasets that lack exact matches. Future work could explore machine learning models to predict matches or identify patterns that contribute to successful matches.

## Prerequisites
Python 3.x
Pandas library
FuzzyWuzzy library
SQLAlchemy
MySQL database
Pandas Profiling for data exploration
Before executing the scripts, ensure the MySQL server is running and accessible with the appropriate credentials.
