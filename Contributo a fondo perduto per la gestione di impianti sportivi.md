# Contributo a fondo perduto per la gestione di impianti sportivi

![](https://www.figc.it/media/127207/logo-dipartimento-per-lo-sport.png?anchor=center&mode=crop&width=730&height=358&rnd=132682855780000000)


Oggi 2023-12-22 il Dipartimento per lo Sport della Presidenza del Consiglio dei Ministri ha pubblicato l'elenco dei [Contributi a fondo perduto: per le ASD/SSD beneficiarie](https://www.sport.governo.it/it/emergenza-covid-19/contributi-a-fondo-perduto-in-favore-delle-societa-e-associazioni-sportive-dilettantistiche/contributi-2023/contributi-a-fondo-perduto-pubblicato-lelenco-delle-asdssd-beneficiarie/)

Si tratta di un [PDF](https://www.sport.governo.it/media/jnak00r3/elenco-beneficiari-impianti-sportivi.pdf) di 208 pagine per 10.126 righe  

Qui la conversione in un csv, dopo un #ETL:  

```Python
import camelot
import pandas as pd

tables = camelot.read_pdf(pdf, pages='all')
df = pd.concat([tab.df for tab in tables], ignore_index=True)

df.columns = df.iloc[0] # Set the First Row as Column Headers
df.drop(df.head(1).index,inplace=True) # drop first row
df.drop(df.tail(1).index,inplace=True) # drop last row
df['Importo contributo'] = df['Importo contributo'].str.replace('.', '').str.replace(',', '.').str.replace('€', '')
df['Importo contributo'] = df['Importo contributo'].astype(float)
df.to_csv('D:/elenco-beneficiari-impianti-sportivi.csv', index = False,sep='|')

```

Nella prima riga del CSV, infine,ho inserito pigramente e a mano lo zero mancante al codice fiscale  
Nella cartella "Dati", il csv già pronto

#opendata