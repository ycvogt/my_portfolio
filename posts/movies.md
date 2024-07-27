[Back](https://ycvogt.github.io/my_portfolio/)

## Netflix Series and Movies Analysis ##

Dataset: The dataset was downloaded from Kaggle[1].
Libraries: pandas, numpy, matplotlib, bokeh, seaborn


# Setup and preliminary exloration
First, we load the dataset and the libraries. Next, we read in the file as a dataframe.

```
df = pd.read_csv('imdb_movies_shows.csv', encoding="UTF-8")
df.size #get number of datapoints, 63866
df.shape #get number of (rows, cols) #(5806, 11)
df.columns #get column names
len(df.columns) #number of columns 11
len(df) #number of rows 5806
df.isnull().sum() #show sum of missing values
df.isnull().sum() / len(df)
```

# Cleaning and preprocessing

```
columns_remove = ["age_certification", "seasons", "imdb_id", "imdb_votes"]
df = df.drop(columns=columns_remove)
#clean and make columns uniform, especially how they show empty values
df['imdb_score'] = df['imdb_score'].fillna(0)
imdb_score_filtered = df.loc[df["imdb_score"] > 0.0, ["title", "type", "release_year", "runtime", "genres", "production_countries", "imdb_score"]]
pcountries_filtered = imdb_score_filtered.loc[imdb_score_filtered["production_countries"] != "[]", ["title", "type", "release_year", "runtime", "genres", "production_countries", "imdb_score"]]
clean_df = pcountries_filtered.loc[pcountries_filtered["title"] != np.NaN, ["title", "type", "release_year", "runtime", "genres", "production_countries", "imdb_score"]]#0 in title, and [] in genres, thus same length
clean_df
```

# Analysis

1. Which year has the most titles?
```
clean_df['release_year'].value_counts(normalize=True)
```

```
sns.histplot(x='release_year', data=clean_df)
```
```
import plotly.express as px
fig = px.violin(clean_df, x="release_year")
fig.show()
```


References

[1] https://www.kaggle.com/datasets/maso0dahmed/netflix-movies-and-shows?resource=download
