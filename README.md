#  Investing in a Surf and Shake serving Shop - Season Analysis


## Overview of the analysis

The investment company W Avy requires to determine if it is possible to invest and develop his idea of a Surf and Shake service shop to sell surfboards and ice creams. To help W avy some required summary statistics  were generated and some others were added for future analysis:

1. Summary Statistics of temperatures for June and December (Required)


2. Summary Statistics of precipitations for June and December (Future trendings)

### Resources
  * SurfsUp_Challenge.ipynb
  * hawaii.sqlite
  * Resources folder with plot images

## Results: 
### Summary Statistics of temperatures for June and December (Required)

Key differences in weather between June and December:
 

This data gives us a summary of different statistics for the amount of temperatures in June from 2010 to 2017

* The mean means that the average temperature during Junes is 74.9 degrees.
* Minimun and Maximun temperatures are 64 and 85 degrees repectly 
* The majority of the temperatures are over 70 degrees
 
 
 
 
![june](https://github.com/lindaperez/surfs_up/blob/main/Resources/June.png) 

* This data gives us a summary of different statistics for the amount of temperatures in June from 2010 to 2017

* The average temperature during December is 71 degrees.
* Minimun and Maximun temperatures are 56 and 83 degrees repectly 
* The majority of the temperatures are over 64 degrees




![june](https://github.com/lindaperez/surfs_up/blob/main/Resources/december.png)

|June Temperatures | December Temperatures | 
| --------------- | --------------- | 
| The majority of the temperatures are over 70 degrees| The majority of the temperatures are over 70 degrees| 
| Minimun temperature is 64 | Minimun temperature is 56 | 
| June has more variability | Has more variability  | 




![june](https://github.com/lindaperez/surfs_up/blob/main/Resources/bothJuneDec.png) 

### Summary Statistics of precipitations for June and December (Future trendings)


|June Precipitations | December Precipitations | 
| --------------- | --------------- | 
| The majority of the precipitations are level 0.13 | The majority of the precipitations are around 0.21 |
| Maximun precipitation level 4.43 | Maximun precipitation level is 6.42 | 
| June has regular variability | More fluctuations than June | 



![june](https://github.com/lindaperez/surfs_up/blob/main/Resources/jun_pre.png) 

* Outliers out of the mean and percentiles:

 ![june](https://github.com/lindaperez/surfs_up/blob/main/Resources/jun_outliners.png) 

![june](https://github.com/lindaperez/surfs_up/blob/main/Resources/dec_prep.png) 

* Outliers out of the mean and percentiles:
![june](https://github.com/lindaperez/surfs_up/blob/main/Resources/dec_outliers.png) 
 

## Summary: 

* We could infer that vast majority of observations have temperatures over 64 degrees in both months.
* The variability in both months is not extreme or very hight. 
* The different in average of temperature in both month is only 3 degrees. 

Some queries that would help with the analysis of June and December are:

1. Amount of precipitation in June and Dec to have idea of how much precipitation Ohau has
```
june_prcp = session.query(Measurement.date,Measurement.prcp).\
filter(extract('month',Measurement.date)==6).\
order_by(Measurement.prcp).all()
dec_prcp = session.query(Measurement.date,Measurement.prcp).\
filter(extract('month',Measurement.date)==12).\
order_by(Measurement.prcp).all()


june_prcp_df = pd.DataFrame(list(june_prcp),columns=['Date','Precipitation'])
june_prcp_df.set_index(june_prcp_df['Date'],inplace=True)
june_prcp_df.sort_index(inplace =True)

dec_prcp_df = pd.DataFrame(list(dec_prcp),columns=['Date','Precipitation'])
dec_prcp_df.set_index(dec_prcp_df['Date'],inplace=True)
dec_prcp_df.sort_index(inplace =True)

print('-----June Precipitations----')
print(june_prcp_df.describe())
june_prcp_df.plot()
plt.xticks(rotation = 45, size=10) #
plt.tight_layout()
plt.ylim(0,5)
plt.ylabel('Precipitation Level', size=10)
plt.xlabel('Date', size=10)

print('-----December Precipitations----')
print(dec_prcp_df.describe())
dec_prcp_df.plot()
plt.xticks(rotation = 45, size=10) #
plt.tight_layout()
plt.ylim(0,5)
plt.ylabel('Precipitation Level', size=10)
plt.xlabel('Date', size=10)

```
3. Temperatures by Season to compare June and December with the whole season

```
# The First Days of the Seasons 

seasons_df = pd.DataFrame()
seasons_df['Seasons of 2022'] = ['SPRING','SUMMER','FALL','WINTER']
seasons_df['Meteorological Start'] = ['Tuesday, March 1','Wednesday, June 1','Thursday, September 1','Thursday, December 1']
seasons_df['Meteorological End'] = ['Wednesday, June 1','Thursday, September 1','Thursday, December 1','Tuesday, March 1']
seasons_df

#session.query(extract('year',Measurement.date)).group_by(extract('year',Measurement.date)).all()
query = session.query(extract('year',Measurement.date),extract('month',Measurement.date),Measurement.tobs).filter(extract('month',Measurement.date)>=3).filter(extract('day',Measurement.date)>=1).\
filter(extract('month',Measurement.date)<=6).filter(extract('day',Measurement.date)<=1).group_by(extract('year',Measurement.date),extract('month',Measurement.date)).order_by(Measurement.date).all()
#query

SPRING = session.query(Measurement.date,Measurement.tobs).\
filter(and_(extract('month',Measurement.date)>=3,extract('day',Measurement.date)>=1)).\
filter(and_(extract('month',Measurement.date)<=5,extract('day',Measurement.date)<=31)).\
order_by(Measurement.date).all()
#SPRING


SUMMER = session.query(Measurement.date,Measurement.tobs).\
filter(and_(extract('month',Measurement.date)>=6,extract('day',Measurement.date)>=1,\
       extract('month',Measurement.date)<=8),extract('day',Measurement.date)<=31).\
order_by(Measurement.date).all()
#SUMMER

FALL = session.query(Measurement.date,Measurement.tobs).\
filter(and_(extract('month',Measurement.date)>=9,extract('day',Measurement.date)>=1,\
            extract('month',Measurement.date)<=11,extract('day',Measurement.date)<=30)).\
order_by(Measurement.date).all()
#FALL

WINTER = session.query(Measurement.date,Measurement.tobs).\
filter(or_(and_(extract('month',Measurement.date)>=12,extract('day',Measurement.date)>=1,\
            extract('month',Measurement.date)<=12,extract('day',Measurement.date)<=31),\
          and_(extract('month',Measurement.date)>=1,extract('day',Measurement.date)>=1,\
            extract('month',Measurement.date)<=2,extract('day',Measurement.date)<=29))).\
order_by(Measurement.date).all()
#WINTER
spring_df = pd.DataFrame(SPRING,columns=['Date','Temperature'])
spring_df['season'] = 'Spring'
summer_df = pd.DataFrame(SUMMER,columns=['Date','Temperature'])
summer_df['season'] = 'Summer'
fall_df = pd.DataFrame(FALL,columns=['Date','Temperature'])
fall_df['season'] = 'Fall'
winter_df =pd.DataFrame(WINTER,columns=['Date','Temperature'])
winter_df['season'] = 'Winter'

season = pd.concat([spring_df,summer_df,fall_df,winter_df])

print(spring_df.describe())
spring_df.plot.hist(bins=5,color='brown', alpha=0.5)

```
