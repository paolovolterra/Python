![](https://opencoesione.gov.it/static/images/header-logo-it.svg)
https://opencoesione.gov.it/it/opendata/#!basedati_section

```Python
import pandas as pd
import os
os.chdir('D:\\duckdb\\files\\opencoesione')
```


```Python
%%time
file = 'progetti_esteso' # https://opencoesione.gov.it/it/opendata/progetti_esteso.zip
date_column = ['OC_DATA_INIZIO_PROGETTO', 'OC_DATA_FINE_PROGETTO_PREVISTA', 'OC_DATA_FINE_PROGETTO_EFFETTIVA', 
               'DATA_INIZIO_PREV_STUDIO_FATT', 'DATA_INIZIO_EFF_STUDIO_FATT', 'DATA_FINE_PREV_STUDIO_FATT', 
               'DATA_FINE_EFF_STUDIO_FATT', 'DATA_INIZIO_PREV_PROG_PREL', 'DATA_INIZIO_EFF_PROG_PREL',
               'DATA_FINE_PREV_PROG_PREL', 'DATA_FINE_EFF_PROG_PREL', 'DATA_INIZIO_PREV_PROG_DEF', 
               'DATA_INIZIO_EFF_PROG_DEF', 'DATA_FINE_PREV_PROG_DEF', 'DATA_FINE_EFF_PROG_DEF', 'DATA_INIZIO_PREV_PROG_ESEC', 
               'DATA_INIZIO_EFF_PROG_ESEC', 'DATA_FINE_PREV_PROG_ESEC', 'DATA_FINE_EFF_PROG_ESEC', 'DATA_INIZIO_PREV_AGG_BANDO',
               'DATA_INIZIO_EFF_AGG_BANDO', 'DATA_FINE_PREV_AGG_BANDO', 'DATA_FINE_EFF_AGG_BANDO', 'DATA_INIZIO_PREV_STIP_ATTRIB',
               'DATA_INIZIO_EFF_STIP_ATTRIB', 'DATA_FINE_PREV_STIP_ATTRIB', 'DATA_FINE_EFF_STIP_ATTRIB', 'DATA_INIZIO_PREV_ESECUZIONE',
               'DATA_INIZIO_EFF_ESECUZIONE', 'DATA_FINE_PREV_ESECUZIONE', 'DATA_FINE_EFF_ESECUZIONE', 'DATA_INIZIO_PREV_COLLAUDO', 
               'DATA_INIZIO_EFF_COLLAUDO', 'DATA_FINE_PREV_COLLAUDO', 'DATA_FINE_EFF_COLLAUDO']
oc = pd.read_csv('https://opencoesione.gov.it/it/opendata/'+file+'.zip', compression='zip', sep=';', decimal=",", quotechar='"',
                parse_dates=date_column,dayfirst=False)

OC = oc[['OC_CODFISC_BENEFICIARIO','OC_DENOM_BENEFICIARIO','COD_LOCALE_PROGETTO', 'CUP', 'OC_TITOLO_PROGETTO', 'OC_SINTESI_PROGETTO', 'OC_LINK','DEN_REGIONE',
        'FINANZ_TOTALE_PUBBLICO','FINANZ_PRIVATO','TOT_PAGAMENTI','OC_DATA_INIZIO_PROGETTO',
         'OC_DATA_FINE_PROGETTO_PREVISTA','OC_STATO_FINANZIARIO','OC_STATO_PROGETTO','OC_STATO_PROCEDURALE']]

OC['OC_CODFISC_BENEFICIARIO'] = OC['OC_CODFISC_BENEFICIARIO'].astype(str)
OC['OC_DATA_INIZIO_PROGETTO'] =  pd.to_datetime(OC['OC_DATA_INIZIO_PROGETTO'], format='%Y%m%d')
OC['OC_DATA_FINE_PROGETTO_PREVISTA'] =  pd.to_datetime(OC['OC_DATA_FINE_PROGETTO_PREVISTA'], format='%Y%m%d')

OC.to_parquet('D:/duckdb/files/Opencoesione/Opencoesione.parquet')
```

## creare il DB

```Python
import duckdb
import os
os.chdir('D:')
conn = duckdb.connect(database='OPencoesione.duckdb', read_only=False)
conn.execute(f'''CREATE TABLE Opencoesione AS SELECT * FROM read_parquet('D:/duckdb/files/Opencoesione/Opencoesione.parquet');''').df()
```

## Contratti di Sviluppo¶

```Python
cds = OC.loc[OC["OC_TITOLO_PROGETTO"].str.startswith('CONTRATTO DI SVILUPPO', na=False)]

cds['TOT_progetto'] = cds['FINANZ_TOTALE_PUBBLICO']+cds['FINANZ_PRIVATO']
cds['copertura'] = cds['TOT_PAGAMENTI']/cds['FINANZ_TOTALE_PUBBLICO']

cds
```

