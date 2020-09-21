
# Udactity Data Analyst

## Bivariate Visualizations Excercises

### Scatterplots

If we want to inspect the relationship between two numeric variables, the standard choice of plot is the scatterplot.


```python
import pandas as pd
import seaborn as sb
import matplotlib.pyplot as plt
import numpy as np

%matplotlib inline
```


```python
fuel_econ = pd.read_csv('fuel-econ.csv')
print(fuel_econ.shape)
fuel_econ.sample(3)
```

    (3929, 20)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>make</th>
      <th>model</th>
      <th>year</th>
      <th>VClass</th>
      <th>drive</th>
      <th>trans</th>
      <th>fuelType</th>
      <th>cylinders</th>
      <th>displ</th>
      <th>pv2</th>
      <th>pv4</th>
      <th>city</th>
      <th>UCity</th>
      <th>highway</th>
      <th>UHighway</th>
      <th>comb</th>
      <th>co2</th>
      <th>feScore</th>
      <th>ghgScore</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2197</th>
      <td>36452</td>
      <td>Chevrolet</td>
      <td>Sonic</td>
      <td>2016</td>
      <td>Compact Cars</td>
      <td>Front-Wheel Drive</td>
      <td>Manual 5-spd</td>
      <td>Regular Gasoline</td>
      <td>4</td>
      <td>1.8</td>
      <td>0</td>
      <td>91</td>
      <td>26.1013</td>
      <td>33.9000</td>
      <td>33.8960</td>
      <td>49.2000</td>
      <td>29.1141</td>
      <td>305</td>
      <td>7</td>
      <td>7</td>
    </tr>
    <tr>
      <th>3353</th>
      <td>38682</td>
      <td>Hyundai</td>
      <td>Elantra SE</td>
      <td>2018</td>
      <td>Midsize Cars</td>
      <td>Front-Wheel Drive</td>
      <td>Automatic (S6)</td>
      <td>Regular Gasoline</td>
      <td>4</td>
      <td>2.0</td>
      <td>0</td>
      <td>96</td>
      <td>29.0422</td>
      <td>38.2345</td>
      <td>38.3341</td>
      <td>56.5397</td>
      <td>32.5978</td>
      <td>272</td>
      <td>8</td>
      <td>8</td>
    </tr>
    <tr>
      <th>443</th>
      <td>33045</td>
      <td>BMW</td>
      <td>M6 Coupe</td>
      <td>2013</td>
      <td>Subcompact Cars</td>
      <td>Rear-Wheel Drive</td>
      <td>Manual 6-spd</td>
      <td>Premium Gasoline</td>
      <td>8</td>
      <td>4.4</td>
      <td>88</td>
      <td>0</td>
      <td>14.6821</td>
      <td>18.1212</td>
      <td>21.5792</td>
      <td>30.0000</td>
      <td>17.1485</td>
      <td>516</td>
      <td>4</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



One basic way of creating a scatterplot is through Matplotlib's `scatter` function:


```python
plt.scatter(data = fuel_econ, x = 'displ', y = 'comb')
plt.xlabel('Displacement (l)')
plt.ylabel('Combined fuel efficiency (mpg)');
```


![png](output_4_0.png)


Seaborn's `regplot` function combines scatterplot creation with regression function fitting:


```python
sb.regplot(data = fuel_econ, x = 'displ', y = 'comb')
plt.xlabel('Displacement (l)')
plt.ylabel('Combined fuel efficiency (mpg)');
```


![png](output_6_0.png)



```python
sb.regplot(data = fuel_econ, x = 'displ', y = 'comb', fit_reg = False)
plt.xlabel('Displacement (l)')
plt.ylabel('Combined fuel efficiency (mpg)');
```


![png](output_7_0.png)


### Overplotting, Transparency, and Jitter

If we have a very large number of points to plot or our numeric variables are discrete-valued, then it is possible that using a scatterplot straightforwardly will not be informative. The visualization will suffer from overplotting, where the high amount of overlap in points makes it difficult to see the actual relationship between the plotted variables.


```python
sb.regplot(data = fuel_econ, x = 'year', y = 'comb');
```


![png](output_9_0.png)


In cases like this, we may want to employ `transparency` and `jitter` to make the scatterplot more informative. 
We can add jitter to move the position of each point slightly from its true value. This is not a direct option in matplotlib's `scatter` function, but is a built-in option with seaborn's `regplot` function. x- and y- jitter can be added independently, and won't affect the fit of any regression function, if made:


```python
sb.regplot(data = fuel_econ, x = 'year', y = 'comb', x_jitter = 0.3);
```


![png](output_11_0.png)


Transparency can be added to a `scatter` call by adding the "alpha" parameter set to a value between 0 (fully transparent, not visible) and 1 (fully opaque). In seaborn's regplot, to set the transparency, we need to assign a dictionary to the "scatter_kws" parameter. This is necessary so that transparency is specifically associated with the `scatter` component of the `regplot` function.


```python
sb.regplot(data = fuel_econ, x = 'year', y = 'comb', x_jitter = 0.3, scatter_kws = {'alpha': 1/20});
```


![png](output_13_0.png)


### Heat Maps
A heat map is a 2-d version of the histogram that can be used as an alternative to a scatterplot. Like a scatterplot, the values of the two numeric variables to be plotted are placed on the plot axes. Similar to a histogram, the plotting area is divided into a grid and the number of points in each grid rectangle is added up. Since there won't be room for bar heights, counts are indicated instead by grid cell color. A heat map can be implemented with Matplotlib's `hist2d` function.


```python
sb.regplot(data = fuel_econ, x = 'displ', y = 'comb', x_jitter = 0.04, 
           scatter_kws = {'alpha': 1/10}, fit_reg = False)
plt.xlabel('Displacement (l)')
plt.ylabel('Combined fuel efficiency (mpg)');
```


![png](output_15_0.png)



```python
plt.hist2d(data = fuel_econ, x = 'displ', y = 'comb');
plt.colorbar()
plt.xlabel('Displacement (l)')
plt.ylabel('Combined fuel efficiency (mpg)');
```


![png](output_16_0.png)



```python
fuel_econ[['displ', 'comb']].describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>displ</th>
      <th>comb</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>3929.000000</td>
      <td>3929.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.950573</td>
      <td>24.791339</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.305901</td>
      <td>6.003246</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.600000</td>
      <td>12.821700</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.000000</td>
      <td>20.658100</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.500000</td>
      <td>24.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.600000</td>
      <td>28.227100</td>
    </tr>
    <tr>
      <th>max</th>
      <td>7.000000</td>
      <td>57.782400</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize = [12, 5])

# left plot: scatterplot of discrete data with jitter and transparency
plt.subplot(1, 2, 1)
sb.regplot(data = fuel_econ, x = 'displ', y = 'comb', x_jitter = 0.04, 
           scatter_kws = {'alpha': 1/10}, fit_reg = False)
plt.xlabel('Displacement (l)')
plt.ylabel('Combined fuel efficiency (mpg)');

# right plot: heat map with bin edges between values
plt.subplot(1, 2, 2)
bins_x = np.arange(0.6, 7+0.3, 0.3)
bins_y = np.arange(12, 58 +3, 3)
plt.hist2d(data = fuel_econ, x = 'displ', y = 'comb', cmin = 0.5, cmap = 'viridis_r',
          bins = [bins_x, bins_y])
plt.colorbar();
```


![png](output_18_0.png)


Notice that since we have two variables, the "bins" parameter takes a list of two bin edge specifications, one for each dimension. We add a `colorbar` function call to add a colorbar to the side of the plot, showing the mapping from counts to colors.


```python

```
