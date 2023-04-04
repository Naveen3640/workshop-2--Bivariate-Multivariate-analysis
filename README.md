Bivariate-Multivariate analysis:


from google.colab import files
uploaded=files.upload()
     
Upload widget is only available when the cell has been executed in the current browser session. Please rerun this cell to enable.
Saving FlightInformation.xlsx to FlightInformation.xlsx

import pandas as pd
import numpy as np
import seaborn as sbn
import matplotlib.pyplot as plt
df = pd.read_excel("FlightInformation.xlsx")
df.head(10)
     
Airline	Date_of_Journey	Source	Destination	Route	Dep_Time	Arrival_Time	Duration	Total_Stops	Additional_Info	Price
0	IndiGo	24/03/2019	Banglore	New Delhi	BLR → DEL	22:20	01:10 22 Mar	2h 50m	non-stop	No info	3897
1	Air India	1/05/2019	Kolkata	Banglore	CCU → IXR → BBI → BLR	05:50	13:15	7h 25m	2 stops	No info	7662
2	Jet Airways	9/06/2019	Delhi	Cochin	DEL → LKO → BOM → COK	09:25	04:25 10 Jun	19h	2 stops	No info	13882
3	IndiGo	12/05/2019	Kolkata	Banglore	CCU → NAG → BLR	18:05	23:30	5h 25m	1 stop	No info	6218
4	IndiGo	01/03/2019	Banglore	New Delhi	BLR → NAG → DEL	16:50	21:35	4h 45m	1 stop	No info	13302
5	SpiceJet	24/06/2019	Kolkata	Banglore	CCU → BLR	09:00	11:25	2h 25m	non-stop	No info	3873
6	Jet Airways	12/03/2019	Banglore	New Delhi	BLR → BOM → DEL	18:55	10:25 13 Mar	15h 30m	1 stop	In-flight meal not included	11087
7	Jet Airways	01/03/2019	Banglore	New Delhi	BLR → BOM → DEL	08:00	05:05 02 Mar	21h 5m	1 stop	No info	22270
8	Jet Airways	12/03/2019	Banglore	New Delhi	BLR → BOM → DEL	08:55	10:25 13 Mar	25h 30m	1 stop	In-flight meal not included	11087
9	Multiple carriers	27/05/2019	Delhi	Cochin	DEL → BOM → COK	11:25	19:15	7h 50m	1 stop	No info	8625

df.info()
df.describe()
     
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 10683 entries, 0 to 10682
Data columns (total 11 columns):
 #   Column           Non-Null Count  Dtype 
---  ------           --------------  ----- 
 0   Airline          10683 non-null  object
 1   Date_of_Journey  10683 non-null  object
 2   Source           10683 non-null  object
 3   Destination      10683 non-null  object
 4   Route            10683 non-null  object
 5   Dep_Time         10683 non-null  object
 6   Arrival_Time     10683 non-null  object
 7   Duration         10683 non-null  object
 8   Total_Stops      10683 non-null  object
 9   Additional_Info  10683 non-null  object
 10  Price            10683 non-null  int64 
dtypes: int64(1), object(10)
memory usage: 918.2+ KB
Price
count	10683.000000
mean	9087.064121
std	4611.359167
min	1759.000000
25%	5277.000000
50%	8372.000000
75%	12373.000000
max	79512.000000

df.isnull().sum()
     
Airline            0
Date_of_Journey    0
Source             0
Destination        0
Route              1
Dep_Time           0
Arrival_Time       0
Duration           0
Total_Stops        1
Additional_Info    0
Price              0
dtype: int64

df['Route']=df['Route'].fillna(df['Route'].mode()[0])
df['Total_Stops']=df['Total_Stops'].fillna(df['Total_Stops'].mode()[0])
     

df.isnull().sum()
     
Airline            0
Date_of_Journey    0
Source             0
Destination        0
Route              0
Dep_Time           0
Arrival_Time       0
Duration           0
Total_Stops        0
Additional_Info    0
Price              0
dtype: int64
BIVARIATE ANALYSIS – NUMERICAL


sbn.scatterplot(x='Airline',y='Price',data=df)
plt.xticks(rotation = 90)

     
([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11],
 [Text(0, 0, 'IndiGo'),
  Text(1, 0, 'Air India'),
  Text(2, 0, 'Jet Airways'),
  Text(3, 0, 'SpiceJet'),
  Text(4, 0, 'Multiple carriers'),
  Text(5, 0, 'GoAir'),
  Text(6, 0, 'Vistara'),
  Text(7, 0, 'Air Asia'),
  Text(8, 0, 'Vistara Premium economy'),
  Text(9, 0, 'Jet Airways Business'),
  Text(10, 0, 'Multiple carriers Premium economy'),
  Text(11, 0, 'Trujet')])



     


     

airline=df.loc[:,["Airline","Price"]]
airline=airline.groupby(by=["Airline"]).sum().sort_values(by="Price")
plt.figure(figsize=(17,7))
sbn.barplot(x=airline.index,y="Price",data=airline)
plt.xticks(rotation = 90)
plt.xlabel=("AIRLINE")
plt.ylabel=("PRICE")
plt.show()
     


sbn.barplot(x=df["Price"],y=df["Source"],data=df)

     
<Axes: xlabel='Price', ylabel='Source'>


sbn.displot(df, x="Source", hue="Airline")

     
<seaborn.axisgrid.FacetGrid at 0x7f96836a9670>


sbn.displot(df, x="Destination", hue="Airline")
     
<seaborn.axisgrid.FacetGrid at 0x7f9681ae6670>


sbn.countplot(x=df['Total_Stops'],hue=df['Additional_Info'])
     
<Axes: xlabel='Total_Stops', ylabel='count'>

MULTIVARIATE ANALYSIS


sbn.pointplot(x=df['Source'],y=df['Price'],hue=df['Additional_Info'])
     
<Axes: xlabel='Source', ylabel='Price'>


sbn.barplot(x=df['Source'],y=df['Price'],hue=df['Destination'])

     
<Axes: xlabel='Source', ylabel='Price'>


sbn.barplot(x=df['Source'],y=df['Price'],hue=df['Additional_Info'])
     
<Axes: xlabel='Source', ylabel='Price'>


df.corr()

     
Price
Price	1.0

sbn.heatmap(df.corr(),annot=True)
     
<Axes: >


