---
title:EDA
---

# Basics to know before even you start exploratory data analysis (EDA)
https://medium.com/analytics-vidhya/basics-to-know-before-even-you-start-exploratory-data-analysis-eda-cbd434add655


![](https://miro.medium.com/max/1400/1*nHcYk2fRw03UTlzveEf4yg.png)

![](https://miro.medium.com/max/1400/1*yoXc1T7jmmBU6vnDHVaBWw.png)

![](https://miro.medium.com/max/1072/1*wnpwSOrJK-O0wFPpDuMnZg.png)

![](https://miro.medium.com/max/1364/1*r89p1-66SwnS49M4zByqDg.png)

![](https://miro.medium.com/max/1016/1*TS1XkbpBlrzjiY7GnvSbag.png)

![](https://miro.medium.com/max/1140/1*UAIvAXIWFzyFpksNK2IHFg.png)

![](https://miro.medium.com/max/916/1*Rs_9F0VCLob26rE9q2s7dw.png)

![](https://miro.medium.com/max/858/1*C1fA5dq-ydVl9wBypO0ziQ.png)




#  pandas_profiling
!pip install pandas_profiling

	import pandas_profiling
	from pandas_profiling import ProfileReport
	profile = ProfileReport(df, explorative=True, dark_mode=True)
	profile.to_file('output.html')


![](https://miro.medium.com/max/1400/1*JkUMm0F-dHl6Pger8VcKBg.png)

![](https://miro.medium.com/max/1400/1*2132JSOwbwWxqbDlcOixww.png)


# dtale
!pip install dtale
	
	import dtale 
	dtale.show(df)

![](https://miro.medium.com/max/1400/1*RbXhrXistWpw4PMHI8YlJw.png)



# dataprep
!pip install dataprep

	from dataprep.eda import create_report
	create_report(df)
	
![](https://miro.medium.com/max/1400/1*XsTf_gEYnDZlXGifkAMjKA.png)


# 10 Pandas essential functions for Data scientists

##  read_csv( ):

	This function is used to read the data file (in csv format). Pandas show the data in dataframe. In this function we add the path where file is located. In case the data file and notebook you are working on, are at same place then you will feed only name to this function.

	data = pd.read_csv("breastCancer.csv")

##  head( ):

	This function is used to see the data structure. It shows the top 5 rows by default. And if you want to see more rows you can add the count inside (). To see bottom 5 rows we use tail( ) function.

	data.head()

##  shape:

	Shape function helps us to know about the data size. it tells the count of rows and columns.

	data.shape

##  astypes( ):

	This function reveals the data type of a particular feature present in the data. It also helps in the cases where we spot that the data type is showing incorrect for a particular feature we can use this function to assign correct type to the same.

	data['Gender'] = data.Gender.astype('category')

##  value_counts( ):

	This function tells us the count of any feature, class or where ever we use it.

	data['City'].value_counts()

##  isnull( ):

	Every dataset is not always contain all required values. To check if there are missing values we use this function. we also add sum() tin the emd to know about the sum of missing values.

	data.isnull()

##  fillna( ):

	While treating missing values NaN (not a number) we often use fillna function to fill appropriate value in place of NaN value.

	data['Age'].fillna(27, inplace=True)

## sort_values( ):

	This function is helpful when we want to arrange our column data in pandas dataframe either in ascending or descending order. Inside function just by adding “Inplace =True” we can update change in actual dataframe.

	data.sort_values(by='Country', inplace=True)

##  describe( ):

	THis function provides descriptive statistic of the numeric columns in the dataset. It’s output includes, count of measurement, min value, max value and also reveals central tendency and dispersion. It helps in data understanding.

	data.describe()

## merge( ):

	Merge function is used when we have two dataframes and we want to merge their data. Here these two dataframes should have any common point.

	data1.merge(data2, on='State', how='left')


# Scrutinize the data

The reason of this step is to figure out the variables and shape of the dataset. It answers questions such as “Is this dataset large enough?” or “How many features or rows does it contain?” etc. After loading the dataset, checking the first five rows with the head() function would be a nice start to understand the structure of the dataset as seen below.

	import pandas as pd
	import numpy as np 
	import seaborn as sns
	import matplotlib.pyplot as plt
	import warnings
	warnings.filterwarnings('ignore')#Loading the dataset
	penguins_size = pd.read_csv('penguins_size.csv', sep = ",")
	penguins_size.head()

	print("Shape is: ", penguins_size.shape)

![](https://miro.medium.com/max/1400/1*Tt5CVjke9iWeb1nVvNCnIg.png)

From here, it is understood that the dataset’s shape is (344, 7) meaning that 7 features and 344 rows are present which emphasizes that the dataset is not large enough. The seven features can be listed as species, island, depth of the culmen, length of the culmen, length of the flipper, body mass and sex.

In order to visualize the data types of the features, info() function can be used as seen below. The results let us understand that species, island and sex are objects and remaining features are float variables. The use of dtypes is also an alternative to learn the data types of the columns.

	penguins_size.info()

![](https://miro.medium.com/max/1400/1*Ng0CmUTp4XkdTcwpZdGn5g.png)

	penguins_size.dtypes
![](https://miro.medium.com/max/1400/1*QkqmNLB9Mxth5PcpG85gzg.png)


# Data Cleaning

Finding missing values, removing duplicates etc. are crucial step in exploratory data analysis. These values could lead our models to draw incorrect conclusions at the end. Investigating only isnull() is not enough. For instance, in a dataset containing a feature of heart rate, the value of that feature cannot be 0. In this case 0 is also a missing value and needs to be dealt with.

There are various ways to deal with the missing values of the data such as deleting the rows containing missing values (if the dataset is large enough and the number of missing values is not too many, that could be an option), imputation methods (mean/median of the feature) etc.

	penguins_size.isnull().sum()

![](https://miro.medium.com/max/1400/1*iqVL0An4Sg26eL13kp4QFw.png)

As seen above, all features except island and species contain missing values in this dataset. I’ve chosen to impute the missing values of the float features with the mean of the corresponding feature since the dataset is quite small.

	penguins_size.value_counts(["sex"])

![](https://miro.medium.com/max/1400/1*e8pGuzFl7S9H0-ZPpyugJQ.png)
	penguins_size['sex'] = penguins_size['sex'].fillna('MALE')

For the sex of the penguin, after checking the count of the values for female and male, the most frequent value will be taken into consideration, in this case missing values will be imputed with “MALE”. As seen above, another value as “.” is seen which has to be imputed or dropped. I preferred to drop it since it contains only one row, but making equal to “MALE” could’ve been another solution. The index of the problematic row is 336. Finally, after all the missing values have been imputed or dropped, we check again with the isna() function and it is understood that no missing values are left.

	penguins_size.drop(axis = 0, inplace = True, index = 336)
	penguins_size.isna().sum()
![](https://miro.medium.com/max/1400/1*NyKM0Yqrg-ZPKww1YWJ9Aw.png)

Lastly for this section, the presence of any duplicate rows is checked and in this dataset none was found.

	duplicated = penguins_size.duplicated()
	print(duplicated.sum())

# Statistical insights

This section is also a part of understanding the data. After handling with the missing values, the describe() function can be used in order to grasp information such as the mean, maximum, minimum and standard deviation of the data. This method can also be useful to detect missing values such as if the minimum of a feature value is 0 where it shouldn’t be, the describe function facilitates the process of handling missing values if any of them are left.

	penguins_size.describe()

![](https://miro.medium.com/max/1400/1*R1Nz25Vbgl8RYNZOpZA3Eg.png)

By the use of value_counts() function, the count of unique values for the objects is done. In this case, the number of rows belonging to each species are calculated as 152, 123 and 68 meaning that Adélie penguins are the ones who dominate the dataset. Moreover, the mean of body mass for each species can be found by using groupby(). For continuous features, this function is useful in terms of splitting the data in categories (in this example species) and observing it better.

	penguins_size['species'].value_counts()
![](https://miro.medium.com/max/1400/1*IWyrw8UsSIlCWZG1cI4p0A.png)


# Find body mass mean for each species.
	mean_bodymass = penguins_size.groupby('species')['body_mass_g'].mean()
	mean_bodymass

![](https://miro.medium.com/max/1400/1*XxW31xYCm2hEcclNbe804A.png)

# Data visualization

Various plotting techniques can be used in order to better visualize the dataset. In this section only few of these techniques will be explained and shown. Some plots are better in visualizing categorical data and some of them are more suitable for numerical data.

Box Plot

These plots are a good way to check the outliers or understand the relationship between a categorical and continuous feature by showing the distribution of data.

As it can be seen below, no outliers is detected since no data point is seen above or below the maximum and minimum respectively. Furthermore, the median of data points can be easily found since the horizontal line passing inside of the box represents it.

# Relationship of the culmen length and sex of the penguins.fig = plt.figure(figsize=(5,8))
	ax= sns.boxplot(x = penguins_size.sex, y=penguins_size['culmen_length_mm'],orient="v", palette = "cividis")
	plt.title('Culmen_length_mm')
	plt.show()

![](https://miro.medium.com/max/656/1*i7HJ3yD_VSXzE9l_XvW2vg.png)
# Histogram

Histograms are used to depict the frequency distribution. It can be only used with numerical data.

Below, histograms which emphasize the frequencies of depth and length of the culmen, length of flipper and body mass since these features contain numerical data.

	#Shows us frequency distribution.
	fig,axs = plt.subplots(1,4,figsize=(20,6))
	axs[0].hist(penguins_size.culmen_depth_mm)
	axs[0].set_title('culmen_depth_mm')
	axs[0].set_ylabel('Frequency')
	axs[1].hist(penguins_size.culmen_length_mm)
	axs[1].set_title('culmen_length_mm')
	axs[2].hist(penguins_size.flipper_length_mm)
	axs[2].set_title('flipper_length_mm')
	axs[3].hist(penguins_size.body_mass_g)
	axs[3].set_title('body_mass_g')plt.show()

![](https://miro.medium.com/max/1400/1*W3SNd9JIQxalosRetq9BgQ.png)

Moreover, kdeplot is another way to visualize the distribution of the data. This plot is really similar to histograms but instead of putting the values into bins, it draws a curve. This is smoother than histograms which can lose little information.

	#Used for visualizing the probability density of a continuous var.sns.kdeplot(penguins_size.flipper_length_mm,color='Cyan')
	plt.show()

![](https://miro.medium.com/max/796/1*V9xAl1Aa1LEfaxcmqj9bYQ.png)


# Bar Plot

In a bar plot, the x-axis represents a categorical variable while the y-axis is a numerical variable. That is why the bar plot depicts a relationship between these two variables. For instance, below the body masses of the penguins for each islands are seen. The categorical data in the x-axis is the islands whereas the numerical data in y-axis is the body mass of the penguin.

	plt.figure(figsize=(8,5))
	colors = ["cyan","lightblue", "darkblue"]
	sns.barplot(x =penguins_size['island'],
	y = penguins_size['body_mass_g'], palette = colors)
	plt.title('Body Mass of Penguins for different Islands')
	plt.show()

![](https://miro.medium.com/max/1014/1*1BOKdN1QrYePhBC7Hj087w.png)

By the use of pandas function crosstab, the relationship between two or more variables can be analyzed. As an illustration, the bar plot below underlines the relationship between the number of penguins of specific species living in a particular island. It is seen that in the Torgersen island approximately 50 Adelie penguins live.

	pd.crosstab(penguins_size['island'], penguins_size['species']).plot.bar(color=('DarkBlue', 'LightBlue', 'Teal'))
	plt.tight_layout()

![](https://miro.medium.com/max/848/1*KuZqngjf_mmEWgrnX94iDg.png)

# Count Plot

A count plot is really similar to a bar plot but used for only categorical data. It plots the count of observations by category in form of a bar plot. It differs from bar plots as bar plots show the mean of a feature by category. The number of observations of each species in the dataset can be seen below.

	sns.countplot('species',data=penguins_size, palette = "Oranges")
	plt.show()

![](https://miro.medium.com/max/778/1*bQut4CckBfS93CA73nqRmA.png)

# Violin Plot

Violin plots have common properties with box plots and used when the objective is to observe the distribution of numerical data for different categories. Its difference from a box plot is that it depicts the probability density of the dataset. It gives more insights than a box plot, because two different categories might have the same mean but it doesn’t mean that they are the same. Their distributions might differ and in that case, violin plots would be more useful to observe.

In the violin plot below, the mean of the body mass of the penguins are clustered between 3000 and 4000 g in Dream island whereas in Biscoe island the mean is between approximately 4500 and 5500 g. Other conclusions can be drawn from these plots, too.

	sns.violinplot(x = 'island',y = 'body_mass_g',data = penguins_size, palette="YlOrRd_r")
	plt.title('Violin plot')

![](https://miro.medium.com/max/792/1*PIjq0Y44sMczgLp-mJJp7A.png)

# Correlation Matrix

The summarization of our data is finally done with the correlation matrix. This matrix shows the correlation between features. The diagonal values are 1 since the features are correlated with themselves. When the relationship between length of flipper and the body mass is questioned, from the correlation matrix it is understood that their correlation is 0.87 which is quite high.

	corr = penguins_size.corr()
	plt.figure(figsize=(8,8))
	sns.heatmap(corr, annot = True, cmap = "PuBu")
	plt.title('Correlation Matrix')
	plt.show()

![](https://miro.medium.com/max/928/1*MejxPMB0yA0svuRbnY84bg.png)

When lots of features are present, visualizing the heatmap only with high values is more useful. As seen below, features correlated more than 0.8 are show and only one correlation (which is between body mass and flipper length) is seen.

	sns.heatmap(corr[(corr > 0.8)],annot = True, cmap="PuBu")

![](https://miro.medium.com/max/888/1*TK4fGCC3lP2iiJa9a4ScOg.png)

This is a really short and quick way to perform exploratory data analysis and this dataset is a small and easily understandable one. The full code can be reached from here. I hope you enjoyed reading. Thanks a lot!

# 100+ Python Pandas commands for EDA | Tips & Tricks

# imports
	import random
	import time
	import os
	import pandas as pd
	import seaborn as sns
	import numpy as np
	from scipy import stats as sts
	import matplotlib.pyplot as plt
	a4_dims = (11.7, 8.27)
	plt.rcParams['figure.figsize'] = a4_dimsimport warnings
	warnings.filterwarnings('ignore')#to display all rows columns 
	pd.set_option('display.max_rows', None)
	pd.set_option('display.max_columns', None)  
	pd.set_option('display.expand_frame_repr', False)
	pd.set_option('max_colwidth', -1)# to remove scientific notation
	pd.set_option('display.float_format', lambda x: '%.3f' % x)#timing your program?
	import time
	start = time.time()
# your code here
	end = time.time()
	print(end - start)
# working with date time
	# convert a col to datetime pandas
	df['date'] = pd.to_datetime(df['date'])#Change working directory
	import os
	os.getcwd()
	os.chdir("directory")%pwd
	%cd folder# get df value
	df['col'][1].item()# create empty df with n cols & m rows#read excel
	pd.read_excel('.xlsx', sheet_name = 'Sheet1')# remove index while exporting
	df.to_csv('csv', index = False)#importing multiple files in a directory
	l = [pd.read_csv(filename) for filename in glob.glob("path\*.csv")]
	df = pd.concat(l, axis = 0)df = pd.concat(l, axis = 1)
	df = df1.append(df2)axis - 0 row - 1 coldf = pd.merge(df1, df2, on = 'com_col', how = 'outer')#index reset
	df.reset_index(drop = True, inplace = True)#change dtype
	df.Weight = df.Weight.astype('int64')#replace blanks with NaN
	df.replace(r'^\s*$', np.nan, regex=True)#accepts only 1D , get all unique elements in a column 
	pd.unique(df['col1'])
	df['col1'].unique()#to flat 2D into 1D
	df[['col1', 'col2']].values.ravel()#to flat 2D into 1D & get only unique values
	pd.unique(df[['col1', 'col2']].values.ravel())#number of unique elements in one column
	df['col1'].value_counts()#number of unique elements in all columns
	df.nunique()#missing values
	sns.heatmap(df.isnull())# NaNs by col
	df.isnull().sum(axis = 0)#drop a column in df
	df.drop(['colname'], axis = 1)#percentile
	df['col'].quantile(0.1)  #top 10 percentile#filter columns based on names
	col_list = list(df.filter(like = 'Avg_').columns)#create a sample dataframe
	df = pd.DataFrame({'col1': [1,2,3], 'col2': [11,22,33]})
	df = pd.DataFrame({'x': [1,2,3], 'y':[11,22,33]}, columns = ['x1', 'y1'])
# with n cols & rows
	pd.DataFrame(index=np.arange(1), columns=np.arange(8))#sorting values by 1 col
	df.sort_values(by = ['col1'], ascending = True)#sorting values by more columns
	df.sort_values(by = ['col1', 'col2', 'col3'], ascending = True)#renaming the columns
	df.rename(columns = {'col1':'rnm1', 'col2':'rnm2'}, inplace = True)#column slicing
	all = df.columns
	except last one = df.columns[:-1]
	mirror columns = df.columns[::-1]#filter function
	df.filter(['col1', 'col2', 'col3'])
	df.filter(regex = '/d')#upto 2 place decimal
	 "{:.2f}".format(x)#row slicing
# top 4 rows
	df[:4]#col slicing
	df[(cond1) | (cond2) & (cond3)]    #where cond1 = df['col1'] > 2#iloc & loc
	df.iloc[<index>, <index>]
	df.loc[(cond1), ['col1', 'col2']] #where cond1 = df['col1'] > 2#groupby
	df.groupby(by = ['col1'])['reqcols'].mean()#replacing nan with space
	df['col1'] = df['col1'].replace('whattoreplace', 'replacewith')
	df = df.replace('','')
	df = df.fillna('')#drop rows with nan
	df.dropna()#converting string to datetime
	df['col1'] = pd.to_datetime(df['col1'])#summary & transpose
	df.describe().transpose()#check for null values in a column
	df.isnull().any()
	df.isnull().all()
	df['col1'].isnull()
	df['col2'].notnull()#null values in each col
	df.isna().sum()#check for non-null values
	pd.notnull()#isin in andas
	df['col1'].isin('somelist')#dropping duplicates
# drops duplicates excluding first occurence
	df.drop_duplicates()
# drops duplicates excluding last occurence
	df.drop_duplicates(keep = 'last')
# drops duplicates by col
	df.drop_duplicates(['col1'])
	df.drop_duplicates(['col1'], keep = 'last')df.drop_duplicates(subset = 'Col1')#joining dataframes#Creating a pivot
	df.pivot('A', 'B', 'C') - [A - vertical, B - Horizontal, C - values]
	pd.pivot_table(df, values = '', index = ['',''], columns = [''], aggfunc = np.sum)
# Unpivot
	pd.DataFrame(pivoted.to_records())#replace infinity with nan
	df.replace([np.inf, -np.inf], np.nan)
# check for infinite values 
	np.isfinite(df).any()#data types of all columns
	df.dtypes
# data type of a single column
	df.colname.dtypes
# convert dtypes
	df['col'].astype(str).astype(int)#lambda function
	lambda x : x + 10#applying functions to a dataframe
	df.apply(lambda x: x + 3)
# apply function referencing multiple columns
	df['Value'] = df.apply(lambda row: my_test(row['a'], row['c']), axis=1)
# if else loop in a lambda function
	df.apply(lambda x: 1 if x == 'W' else 0)#List Comprehensions
	ls = [i for i in ls1 if i not in ls2]#numpy array methods
	np.zeros((shape))
	np.ones((shape))
	np.full(5, -1)
	np.full((2,5), -1)#list methods
	list.sort()
	list.sort(reverse = true)
	ls = ['a','b','c']
	"".join(ls) = abc
	"-".join(ls) = a-b-c#reverse a list
	list.reverse()
# remove list items
	list.clear()
# remove - removes the element from the list
	ls.remove(element)
	list.pop[0]#pop - last element
	ls.pop()
# pop at index
	ls.pop(0)
	del list[0]
# get list index
	list_name.index('element')#append - adds an element to the list
	ls.append(element)#extend - adds ls2 to the end of ls1
	ls1.extend(ls2)#insert
	ls.insert(position, element)#delete - deletes the element at that index
	del ls[0]
	del ls[:] #deletes all elements from the list#enumerate
	enumerate(iterable, start)
	li = ['a','b','c']
	ob = enumerate(li)#String methods
	str.endswith('pattern')# EDA notes
	https://pandas-profiling.github.io/pandas-profiling/docs/master/rtd/

