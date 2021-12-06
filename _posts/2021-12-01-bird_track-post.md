---
layout: post
title: Bird Track
date: 2021-11-30 16:32:20 +0300
description: spring # Add post description (optional)
image: ../assets/images/bird_track.jpg
fig-caption: # Add figcaption (optional)
tags: [python, datacrolling, python3, ]
---
사용 언어 : Python3<br>

목표 : 철새의 이동경로 예측하기

```py
import pandas as pd
import datetime as dt
import  matplotlib.pyplot as plt
import numpy as np
import cartopy.crs as ccrs
import cartopy.feature as cfeature
```

```py
birdata = pd.read_csv('C:/Users/202-14/AI/bird_tracking.csv')
birdata.columns

ix = birdata.bird_name =="Sanne"
date = birdata.date_time[ix]
print(date[:1])


dt.datetime.today()

time1 = dt.datetime.today()
time2 = dt.datetime.today()
time2 - time1

date_str = birdata.date_time[0]
print(date_str)

print(date_str[:-3])
a = dt.datetime.strptime(date_str[:-3],'%Y-%m-%d %H:%M:%S')

timestamps=[]
for k in range(len(birdata)): timestamps.append(dt.datetime.strptime(birdata.date_time.iloc[k][:-3], '%Y-%m-%d %H:%M:%S'))
print(timestamps)
print(birdata.head())
bird_names = pd.unique(birdata.bird_name)
print(bird_names)
birdata['timestamp']= pd.Series(timestamps, index= birdata.index)
times = birdata.timestamp[birdata.bird_name == 'Eric']
elapsed = [time- times[0] for time in times]
print(elapsed[100])

plt.plot(np.array(elapsed)/dt.timedelta(days=1))
plt.xlabel('Observation')
plt.ylabel('Elapsed(days)')
plt.show()
```

<img src= "https://github.com/tigre911/portfoilo.github.io/blob/gh-pages/assets/images/bird_graph.jpg?raw=true" width="100%" height="100%">


```py
proj = ccrs.Mercator()
plt.figure(figsize=(10,10))
ax = plt.axes(projection=proj)
ax.set_extent((-25.0, 20.0, 52.0, 10.0))
ax.add_feature(cfeature.LAND)
ax.add_feature(cfeature.OCEAN)
ax.add_feature(cfeature.COASTLINE)
ax.add_feature(cfeature.BORDERS, linestyle=':')
plt.show()
for name in bird_names:
    ix = birdata['bird_name'] == name
    x,y = birdata.longitude[ix],  birdata.latitude[ix]
    ax.plot(x,y,'.', transform = ccrs.Geodetic(), label = name)
plt.legend(loc='upper left')
plt.show()
```

<img src= "https://github.com/tigre911/portfoilo.github.io/blob/gh-pages/assets/images/bird_track.jpg?raw=true" width="100%" height="100%">