import pandas as pd
import numpy as np
from pandas_datareader import data as pdr
import matplotlib.pyplot as plt
import seaborn as sns
from datetime import datetime as dt
import random
import plotly.graph_objects as go
import plotly.express as px

#Data Stocks
tickers = ["AAPL", "AMZN","GOOGL","TSLA","MSFT","FB"]
startdate=dt(2013,1,1)
enddate=dt(2021,11,19)
df=pdr.get_data_yahoo(tickers, start=startdate,end=enddate) ['Adj Close']

df_a=df.resample("M").last().pct_change().dropna().round(4)
df_a=df_a*100
df_a.head()
final=df_a.unstack().reset_index()
final=final.rename(columns={0:"Performance"})

tickerss= ["NDX"]
startdate=dt(2013,1,1)
enddate=dt(2021,11,19)
dff=pdr.get_data_yahoo(tickerss, start=startdate,end=enddate) ['Adj Close']
dff_a=dff.resample("M").last().pct_change().dropna().round(4)
dff_a=dff_a*100

#Determine Relative Performance
TT=df_a['AAPL']/dff_a['NDX']
TT2=df_a['AMZN']/dff_a['NDX']
TT3=df_a['GOOGL']/dff_a['NDX']
#TT4=df_a['TSLA']/dff_a['NDX']

#Line Graph
df_single=pd.concat([TT,TT2,TT3], axis=1)
df_single.columns=["AAPL", "AMZN","GOOGL"]
df_single.plot(figsize=(20,10)).legend(loc='best')

#xline --> Extremums values
plt.axhline(y=0, color = 'k')

plt.axhline(y=-26, color = 'b')
plt.axhline(y=69, color = 'b')

plt.axhline(y=-25, color = 'y')
plt.axhline(y=29, color = 'y')

plt.axhline(y=-88, color = 'g')
plt.axhline(y=23, color = 'g')







#Box Graph
df_singles=df_single.unstack().reset_index()
df_singles["Date"]= df_singles["Date"]
df_singles.columns = ['Tickers', 'Date', 'Performance']
px.box(df_singles, x="Tickers",y="Performance", color = 'Tickers')
#df_singles.head()

px.box(df_singles, x="Tickers",y="Performance", color = 'Tickers')


        ##############################################

#Determine cumulated sum for each stocks performance + hist.graph
Sum=0
Cumm={}
for key,values in TT.items():
    Sum+=values
    Cumm[key]=Sum

fig1,ax0=plt.subplots() #SOURCE MATPLOTLIB

x0=Cumm.keys()
y0=Cumm.values()
ax0.stackplot(x0,y0,alpha=1,color='b')
#plt.legend([ax0],['AAPL'],loc='best', title='APPL')
plt.show()

            #####
Sum=0
Cumm={}
for key,values in TT2.items():
    Sum+=values
    Cumm[key]=Sum

fig2,ax1=plt.subplots() #SOURCE MATPLOTLIB

x1=Cumm.keys()
y1=Cumm.values()
ax1.stackplot(x1,y1,alpha=1,color='r')

plt.show()

        #####
Sum=0
Cumm={}
for key,values in TT3.items():
    Sum+=values
    Cumm[key]=Sum

fig3,ax2=plt.subplots() #SOURCE MATPLOTLIB

x2=Cumm.keys()
y2=Cumm.values()
ax2.stackplot(x2,y2,alpha=1,color='g')
plt.show()


#Creation Triptych
fig4, ax4=plt.subplots(nrows=3,constrained_layout=True)
#plt.legend([ax0,ax1,ax2],['APPL','AMZN','GOOGL'],loc='best')
ax4[0].stackplot(x0,y0,alpha=1,color='b')
ax4[1].stackplot(x1,y1,alpha=1,color='r')
ax4[2].stackplot(x2,y2,alpha=1,color='g')
