# Stochastic-Oscillator
The Stochastic Oscillator has two lines, the %K and %D. The %D line is more important to produce better trading signals.
# Prerequisites
* Python 3.6
* Libraries (pandas, numpy, matplotlib)
# What is Stochastic Oscillator
 The stochastic oscillator is one of the most recognized momentum indicators in technical analysis. The indicator works on the premise that prices should be closing near the highs of a trading range during upswings and toward the lower end of a trading range during downswings.
# How it Works
 The stochastic oscillator is plotted within a range of zero and 100. Readings above 80 are considered overbought while readings below 20 are considered oversold. The oscillator has two lines, the %K and %D, where the former measures momentum and the latter measures the moving average of the former. The %D line is more important of the two indicators and tends to produce better trading signals.
# Mathematical Description
Stochastic Oscillator indicator has two series, both being calculated with the help of other indicators (EMA or SMA). Also it has 3 types: Fast Stochastic Oscillator, Slow Stochastic Oscillator and Full Stochastic Oscillator.

**Fast Stochastic Oscillator:**

The defaults create a Fast Stochastic Oscillator indicator.
* *Fast %K = %K basic calculation*
<p align="center"> 
<img src="https://static.anychart.com/images/technical_indicators/fastK.png"></p>

Where pK is the first period that is set through the stochastic() method, which is a period for the %K value.
* *Fast %D = 3-period SMA of Fast %K*
<p align="center"> 
<img src="https://static.anychart.com/images/technical_indicators/fastD.png"></p>

Where p3 is the third period that is set through the stochastic() method, which is a period for the %D value.

**Slow Stochastic Oscillator:**

While Fast Stochastic Oscillator is used for signals, the Slow Stochastic Oscillator is supposed to reflect this emphasis.
* *Slow %K = Fast %K smoothed with 3-period SMA*
<p align="center"> 
<img src="https://static.anychart.com/images/technical_indicators/slowK.png"></p>

Where 3 is a default period for getting slow K.
* *Slow %D = 3-period SMA of Slow %K*
<p align="center"> 
<img src="https://static.anychart.com/images/technical_indicators/slowD.png"></p>

Where 3 is a default period for getting slow D.

**Full Stochastic Oscillator:**

The Full Stochastic Oscillator is a fully customizable version of the Slow Stochastic Oscillator. Users can set the look-back period, the number of periods to slow %K and the number of periods for the %D moving average.
* *Full %K = Fast %K smoothed with X-period SMA*
<p align="center"> 
<img src="https://static.anychart.com/images/technical_indicators/fullK.png"></p>

Where p2 is the second period that is set through the stochastic() method, which is a period for the smoothed %K value.
* *Full %D = X-period SMA of Full %K*
<p align="center"> 
<img src="https://static.anychart.com/images/technical_indicators/fullD.png"></p>

Where p3 is the third period that is set through the stochastic() method, which is a period for the %D value.
# Implementing Stochastic Oscillator
**Importing Data**
```python
#Importing important libraries
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
df = pd.read_csv('Put Your CSV File Here!!!')
# converting from UNIX timestamp to normal
df['date'] = pd.to_datetime(df['date'],unit='s').dt.date
array_date = np.array(df['date'])
array_close = np.array(df['close'])
array_open = np.array(df['open'])
array_high = np.array(df['high'])
array_low = np.array(df['low'])
array_volume = np.array(df['volume'])
print("High Array size",array_high.size)
print("Low Array size",array_low.size)
print("Open Array size",array_open.size)
print("Close Array size",array_close.size)
```
**Finding Highest Values within k Periods**
```python
y=0
z=0
# kperiods are 14 array start from 0 index
kperiods=13
array_highest=[]
for x in range(0,array_high.size-kperiods):
	z=array_high[y]
	for j in range(0,kperiods):
		if(z<array_high[y+1]):
			z=array_high[y+1]
		y=y+1
	# creating list highest of k periods
	array_highest.append(z)
  # skip one from starting after each iteration
	y=y-(kperiods-1)
print("Highest array size",len(array_highest))
print(array_highest)
```
**Finding Lowest Values within k Periods**
```python
y=0
z=0
array_lowest=[]
for x in range(0,array_low.size-kperiods):
	z=array_low[y]
	for j in range(0,kperiods):
		if(z>array_low[y+1]):
			z=array_low[y+1]
		y=y+1
	# creating list lowest of k periods
	array_lowest.append(z)
  # skip one from starting after each iteration
	y=y-(kperiods-1)
print("Lowest array size",len(array_lowest))
print(array_lowest)
```
**Finding %K Line Values**
```python
Kvalue=[]
for x in range(kperiods,array_close.size):
   k = ((array_close[x]-array_lowest[x-kperiods])*100/(array_highest[x-kperiods]-array_lowest[x-kperiods]))
   Kvalue.append(k)
print(len(Kvalue))
print(Kvalue)
```
**Finding %D Line Values**
```python
y=0
# dperiods for calculate d values
dperiods=3
Dvalue=[None,None]
mean=0
for x in range(0,len(Kvalue)-dperiods+1):
	sum=0
	for j in range(0,dperiods):
		sum=Kvalue[y]+sum
		y=y+1
	mean=sum/dperiods
	# d values for %d line adding in the list Dvalue
	Dvalue.append(mean)
  # skip one from starting after each iteration
	y=y-(dperiods-1)
print(len(Dvalue))
print(Dvalue)
```
**Output**
```python
# Visualising the result
plt.figure(figsize=(25,15), dpi=50, facecolor='w', edgecolor='k')
ax = plt.gca() 
plt.plot(Kvalue,color='red',label = '%K line')
plt.plot(Dvalue,color='blue',label = '%D line')
plt.title('Stochastic Oscillator', fontsize=20)
df['date'] = df['date'].reset_index()
x=df['date'].index
labels = array_date[13:]
plt.xticks(x, labels, rotation = 'vertical')
for tick in ax.xaxis.get_major_ticks():
    tick.label1.set_fontsize(10)
for tick in ax.yaxis.get_major_ticks():
    tick.label1.set_fontsize(10)
plt.ylabel('KD_Values', fontsize=20)
plt.xlabel('Dates', fontsize=15)
plt.legend()
plt.show()
```
<p align="center">
  <img src=https://user-images.githubusercontent.com/26857440/38804573-77031dfc-4190-11e8-954f-1dcc2d2e7d46.PNG></p>
  
# Reference
* [Technical Analysis: Indicators And Oscillators](https://www.investopedia.com/terms/s/stochasticoscillator.asp) Investopedia
* [Technical Indicators Mathematical Description](https://docs.anychart.com/Stock_Charts/Technical_Indicators/Mathematical_Description#stochastic_oscillator) AnyChart
* [Stochastic Oscillator](http://stockcharts.com/school/doku.php?id=chart_school:technical_indicators:stochastic_oscillator_fast_slow_and_full) StockCharts
