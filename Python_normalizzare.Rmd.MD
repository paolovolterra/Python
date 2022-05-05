---
title: Normalizzazione
---

# [Normalizzazione, standardizzazione e distribuzione normale](https://gustavorsantos.medium.com/normalization-standardization-and-normal-distribution-bfbe14e12df0)

![](https://cdn-images-1.medium.com/max/744/1*GAcEj37smCOCZMrqp-rjjA.png)
“**Riscalare**” un vettore significa aggiungere o sottrarre una costante e poi moltiplicare o dividere per una costante, come si farebbe per cambiare le unità di misura dei dati, ad esempio per convertire una temperatura da Celsius a Fahrenheit.

"**Normalizzare**" un vettore molto spesso significa dividere per una norma del vettore.  
Si fa spesso riferimento anche al ridimensionamento del minimo e dell'intervallo del vettore, per far sì che tutti gli elementi si trovino tra 0 e 1 portando così tutti i valori delle colonne numeriche nel set dati di a una comune scala.  
La normalizzazione è una buona tecnica da usare quando non conosci la distribuzione dei tuoi dati o quando sai che la distribuzione non è gaussiana (una curva a campana).   
La normalizzazione è utile quando i tuoi dati hanno scale variabili e l' algoritmo che stai utilizzando non fa ipotesi sulla distribuzione dei tuoi dati, come i k-vicini più vicini e le reti neurali artificiali .

"**standardizzare**" un vettore molto spesso significa sottrarre una misura di posizione e dividere per una misura di scala .   
Ad esempio, se il vettore contiene valori casuali con una distribuzione gaussiana, è possibile sottrarre la media e dividere per la deviazione standard, ottenendo così una "normale standard" variabile casuale con media 0 e deviazione standard 1. 
La standardizzazione presuppone che i dati abbiano una distribuzione gaussiana (curva a campana).  
Questo non deve essere strettamente vero, ma la tecnica è più efficace se la distribuzione degli attributi è gaussiana.  
La standardizzazione è utile quando i tuoi dati hanno scale variabili e l' algoritmo che stai utilizzando fa supposizioni sui tuoi dati con una distribuzione gaussiana, come la regressione lineare , la regressione logistica e l'analisi discriminante lineare. 

la normalizzazione e la standardizzazione non cambiano la distribuzione dei dati.  
In altre parole, se la tua variabile non è distribuita normalmente, non sarà trasformata in una con il normalize.  
normalize()o StandardScaler()a partire dal sklearn non cambieranno la forma dei dati.

## Standardizzazione
Standardizzare le caratteristiche intorno al centro e 0 con una deviazione standard di 1 è importante quando confrontiamo misurazioni che hanno unità diverse. 
Le variabili misurate su scale diverse non contribuiscono allo stesso modo all'analisi e potrebbero finire per creare una base.
Ad esempio, una variabile che varia tra 0 e 1000 supererà una variabile che varia tra 0 e 1. 
L'utilizzo di queste variabili senza standardizzazione darà la variabile con il peso dell'intervallo più ampio di 1000 nell'analisi. 
Trasformare i dati in scale comparabili può prevenire questo problema. Le procedure tipiche di standardizzazione dei dati equalizzano l'intervallo e/o la variabilità dei dati. 
La standardizzazione può essere eseguita utilizzando sklearn.preprocessing.StandardScalermodulo.  
Quello che fa alla tua variabile è centrare i dati su una media di 0 e una deviazione standard di 1.
E' importante mettere i dati nella stessa scala. 
A volte lavori con molte variabili di scale diverse. 
Ad esempio, supponiamo che tu stia lavorando su un progetto di regressione lineare che ha variabili come anni di studio e stipendio: gli anni di studio oscilleranno tra 1 e 30 e la variabile salariale sarà all'interno dell'intervallo delle decine di migliaia di €  
Una volta che l'algoritmo di regressione lineare calcolerà i coefficienti, naturalmente darà allo stipendio un numero maggiore rispetto agli anni di studio.  
Ma **sappiamo che non vogliamo che il modello faccia quella differenziazione**: possiamo standardizzare i dati per metterli nella stessa scala.

	import pandas as pd
	import seaborn as sns
	from sklearn.preprocessing import StandardScaler, normalize
	import scipy.stats as scs# Pull a dataset
	df = sns.load_dataset('tips')# Histogram of tip variable
	sns.histoplot(data=df, x='tip');

![Istogramma della variabile 'tip'](https://miro.medium.com/max/476/1*BxPHj3RtbdZ0Opdo3_wKgw.png)

	scaler = StandardScaler()
	scaled = scaler.fit_transform(df[['tip']])# Mean and Std of standardized data
	print(f'Mean: {scaled.mean().round()} | Std: {scaled.std().round()}')[OUT]: Mean: 0.0 | Std: 1.0# Histplot
	sns.histplot(scaled);

!["Suggerimento" standardizzato](https://miro.medium.com/max/487/1*QKsb0XFFimaZjGLjB3bFag.png)

La forma è la stessa. Non era normale prima. Non è normale ora. E possiamo fare un test di Shapiro per le distribuzioni normali prima e dopo per confermare. Il p-Value è il secondo numero tra parentesi (numero del test statistico, p-Value) e se minore di 0,05, significa distribuzione non normale.

	# Normal test original data
	scs.shapiro(df.tip)
	[OUT]: (0.897811233997345, 8.20057563521992e-12)
	# Normal test scaled data
	scs.shapiro(scaled)
	[OUT]: (0.8978115916252136, 8.201060490431455e-12)

## Normalizzazione

l'obiettivo della normalizzazione è modificare i valori delle colonne numeriche nel set dati di in una scala comune, senza distorcere le differenze negli intervalli di valori. Per l'apprendimento automatico , ogni set di dati non richiede la normalizzazione. È richiesto solo quando le caratteristiche hanno intervalli diversi.

![](https://cdn-images-1.medium.com/max/253/1*Dl3P3Rrzto258X0Ales9Xw.png)
Ad esempio, considera un set di dati contenente due caratteristiche, età e reddito (x2). Dove l'età varia da 0 a 100 anni, mentre il reddito varia da 0 a 100.000 e oltre. Il reddito è circa 1.000 volte superiore all'età. Quindi, queste due caratteristiche sono in intervalli molto diversi. Quando eseguiamo ulteriori analisi, come multivariata la regressione lineare , ad esempio, il reddito attribuito influenzerà intrinsecamente il risultato maggiormente a causa del suo valore maggiore. Ma questo non significa necessariamente che sia più importante come predittore. Quindi normalizziamo i dati per portare tutte le variabili nello stesso intervallo. 

La normalizzazione può essere eseguita in Python con normalize()a partire dal sklearne non cambierà anche la forma dei tuoi dati. Porta anche i dati alla stessa scala, ma la differenza principale qui è che presenterà numeri tra 0 e 1 (ma non centrerà i dati sulla media 0 e std =1).

Uno dei modi più comuni per normalizzare è la normalizzazione [Min Max](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html) , che sostanzialmente rende il valore massimo uguale a 1 e il minimo uguale a 0. Tutto ciò che sta nel mezzo sarà una percentuale di quello, o un numero compreso tra 0 e 1. Tuttavia, in questo esempio stiamo usando la normalizzazione funzione di di sklearn.

	# normalize
	normalized = normalize(df[['tip']], axis=0)
	# Normalized, but NOT Normal distribution. p-Value < 0.05
	scs.shapiro(normalized)
	[OUT]: (0.897811233997345, 8.20057563521992e-12) 

![Punta variabile normalizzata: stessa forma](https://miro.medium.com/max/474/1*hS3UVULg7WZGlPQNkWi0oA.png)

Ancora una volta, la nostra forma rimane la stessa. I dati non sono ancora distribuiti normalmente.  
Allora perché eseguire quelle operazioni? 

**Standardizzazione e Normalizzazione sono importanti per mettere tutte le caratteristiche nella stessa scala.**

Algoritmi come la regressione lineare sono chiamati deterministici e quello che fanno è trovare i numeri migliori per risolvere un'equazione matematica, o meglio, un'equazione lineare se stiamo parlando di regressione lineare.

Quindi il modello testerà molti valori da mettere come coefficienti di ciascuna variabile. 
I numeri saranno proporzionali alla grandezza delle variabili. 
Detto questo, possiamo capire che le variabili che fluttuano sulle decine di migliaia avranno coefficienti più alti di quelli nell'intervallo delle unità. Seguirà l'importanza data a ciascuno.
Includere molto grandi e molto piccoli numeri in una regressione può portare a problemi di calcolo. Quando si normalizza o si standardizza, si mitiga il problema. 

## Modificare la forma dei dati

C'è una trasformazione che può cambiare la forma dei tuoi dati e renderli approssimativi a una distribuzione normale.  
Questa è la **trasformazione logaritmica**

	# Log transform and Normality 
	scs.shapiro(df.tip.apply(np.log))
	[OUT]: (0.9888471961021423, 0.05621703341603279)
	p-Value > 0.05 : Data is normal
	# Histogram after Log transformation
	sns.histplot(df.tip.apply(np.log) );

![Log variabile 'tip' trasformato. Ora è una distribuzione normale](https://miro.medium.com/max/470/1*3ndfyjWvFUk5ojhj0HB7JA.png)

La trasformazione del registro rimuoverà l'asimmetria di un set di dati perché mette tutto in prospettiva.  
Le varianze saranno proporzionali anziché assolute, quindi la forma cambia e assomiglia a una distribuzione normale. 
Una bella descrizione che ho visto a riguardo è che la trasformazione del registro è come guardare una mappa con una legenda in scala dove 1 cm = 1 km.  
Mettiamo l'intero spazio mappato sulla prospettiva dei centimetri.  
Abbiamo normalizzato i dati.

# [Perché registrare la trasformazione.](https://stats.stackexchange.com/questions/18844/when-and-why-should-you-take-the-log-of-a-distribution-of-numbers)

# [Normalizzazione e forma dei dati.](https://stats.stackexchange.com/questions/360323/does-applying-standard-normalisation-lead-to-normal-distribution)

# [Normalizzare o no. ](https://stats.stackexchange.com/questions/217953/data-normalization-or-not)

# [Quando normalizzare o standardizzare.](https://towardsai.net/p/data-science/how-when-and-why-should-you-normalize-standardize-rescale-your-data-3f083def38ff)

