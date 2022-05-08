---
title: Business intelligence con ATOTI e Python
---

# Atoti e FdG

```Python
	import atoti as tt
	import numpy as np
	import pandas as pd
	from sqlalchemy import create_engine

	username = ""
	password = ""
	port = 3306
	database = ""

	engine = create_engine('mysql+mysqldb://%s:%s@localhost:%i/%s'
						   %(username, password, port, database))

	sql = "SELECT * FROM fdg_abs;"
	df = pd.read_sql_query(sql, engine)

	df.index.set_names("indice", inplace=True)
	df.describe()


	session = tt.create_session(config={"user_content_storage": "./content", "port": 55707})
	## session= tt.create_session()

	data= session.read_pandas(df,
	table_name="Sales Data",
	keys=  ["regione","CF","processo"]
	)

	cube= session.create_cube(data,"FdG")

	cube

	cube.schema

	session.visualize()
```
