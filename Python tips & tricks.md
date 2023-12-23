# Python tips & tricks.md

```Python

df.columns = df.iloc[0] # Set the First Row as Column Headers

```

```Python
df.drop(df.head(1).index,inplace=True) # drop first row
df.drop(df.tail(1).index,inplace=True) # drop last row

```

```Python
df['Importo contributo'] = df['Importo contributo'].str.replace('.', '').str.replace(',', '.').str.replace('€', '')
```

```Python
df['Importo contributo'] = df['Importo contributo'].astype(float)


```
