
## PROJECT: INTRODUCTION TO DATACAMP PROJECTS
### 1. This is a Jupyter notebook!
A Jupyter notebook is a document that contains text cells (what you're reading right now) and code cells. What is special with a notebook is that it's interactive: You can change or add code cells, and then run a cell by first selecting it and then clicking the run cell button above ( ▶| Run ) or hitting ctrl + enter.

The result will be displayed directly in the notebook. You could use a notebook as a simple calculator. For example, it's estimated that on average 256 children were born every minute in 2016. The code cell below calculates how many children were born on average on a day.


```python
# I'm a code cell, click me, then run me!
256 * 60 * 24 # Children × minutes × hours
```




    368640



### 2. Put any code in code cells
But a code cell can contain much more than a simple one-liner! This is a notebook running python and you can put any python code in a code cell (but notebooks can run other languages too, like R). Below is a code cell where we define a whole new function (greet). To show the output of greet we run it last in the code cell as the last value is always printed out.


```python
def greet(first_name, last_name):
    greeting = 'My name is ' + last_name + ', ' + first_name + ' ' + last_name + '!'
    return greeting

greet('Jasmy', 'Joson')
```




    'My name is Joson, Jasmy Joson!'



### 3. Jupyter notebooks ♡ data
We've seen that notebooks can display basic objects such as numbers and strings. But notebooks also support the objects used in data science, which makes them great for interactive data analysis!

For example, below we create a pandas DataFrame by reading in a csv-file with the average global temperature for the years 1850 to 2016. If we look at the head of this DataFrame the notebook will render it as a nice-looking table.


```python
# Importing the pandas module
import pandas as pd

# Reading in the global temperature data
global_temp = pd.read_csv('https://raw.githubusercontent.com/Jasmy118/MyProjects/master/Project%201/Images%26Files/global_temperature.csv')
country_region = pd.read_csv('https://raw.githubusercontent.com/Jasmy118/MyProjects/master/Project%201/Images%26Files/countries.csv')
# Take a look at the first datapoints
print(global_temp.head())
print(country_region.head())
```

       year  degrees_celsius
    0  1850             7.74
    1  1851             8.09
    2  1852             7.97
    3  1853             7.93
    4  1854             8.19
        Country  Region
    0   Algeria  AFRICA
    1    Angola  AFRICA
    2     Benin  AFRICA
    3  Botswana  AFRICA
    4   Burkina  AFRICA
    

#### Getting/Downloading a file from github:
![prj%201a.JPG](https://github.com/Jasmy118/MyProjects/blob/master/Project%201/Images%26Files/prj%201a.JPG)
![prj%201b.JPG](https://github.com/Jasmy118/MyProjects/blob/master/Project%201/Images%26Files/prj%201b.JPG)

### 4. Jupyter notebooks ♡ plots
Tables are nice but — as the saying goes — "a plot can show a thousand data points". Notebooks handle plots as well, but it requires a bit of magic. Here magic does not refer to any arcane rituals but to so-called "magic commands" that affect how the Jupyter notebook works. Magic commands start with either % or %% and the command we need to nicely display plots inline is %matplotlib inline. With this magic in place, all plots created in code cells will automatically be displayed inline.

Let's take a look at the global temperature for the last 150 years.


```python
# Setting up inline plotting using jupyter notebook "magic"
%matplotlib inline

import matplotlib.pyplot as plt

fig = plt.figure()
# Plotting global temperature in degrees celsius by year
ax = fig.add_subplot(111)
ax.plot(global_temp['year'], global_temp['degrees_celsius'])
#Adding labels and titles
ax.set(title='Year-Temperature Plot', xlabel='Year', ylabel='Degree Celsius')
plt.show()
```


![png](output_8_0.png)


### 5. Jupyter notebooks ♡ a lot more
Tables and plots are the most common outputs when doing data analysis, but Jupyter notebooks can render many more types of outputs such as sound, animation, video, etc. Yes, almost anything that can be shown in a modern web browser. This also makes it possible to include interactive widgets directly in the notebook!

For example, this (slightly complicated) code will create an interactive map showing the locations of the three largest smartphone companies in 2016. You can move and zoom the map, and you can click the markers for more info!

pip install folium in the working environment:

![prj%201c.JPG](https://github.com/Jasmy118/MyProjects/blob/master/Project%201/Images%26Files/prj%201c.JPG)


```python
# Making a map using the folium module
import folium
phone_map = folium.Map()

# Top three smart phone companies by market share in 2016
companies = [
    {'loc': [37.4970,  127.0266], 'label': 'Samsung: 20.5%'},
    {'loc': [37.3318, -122.0311], 'label': 'Apple: 14.4%'},
    {'loc': [22.5431,  114.0579], 'label': 'Huawei: 8.9%'}] 

# Adding markers to the map
for company in companies:
    marker = folium.Marker(location=company['loc'], popup=company['label'])
    marker.add_to(phone_map)

# The last object in the cell always gets shown in the notebook
phone_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgCiAgICAgICAgPHNjcmlwdD4KICAgICAgICAgICAgTF9OT19UT1VDSCA9IGZhbHNlOwogICAgICAgICAgICBMX0RJU0FCTEVfM0QgPSBmYWxzZTsKICAgICAgICA8L3NjcmlwdD4KICAgIAogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjUuMS9kaXN0L2xlYWZsZXQuanMiPjwvc2NyaXB0PgogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY29kZS5qcXVlcnkuY29tL2pxdWVyeS0xLjEyLjQubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9qcy9ib290c3RyYXAubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5qcyI+PC9zY3JpcHQ+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjUuMS9kaXN0L2xlYWZsZXQuY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vYm9vdHN0cmFwLzMuMi4wL2Nzcy9ib290c3RyYXAubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLXRoZW1lLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9mb250LWF3ZXNvbWUvNC42LjMvY3NzL2ZvbnQtYXdlc29tZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuanMuY2xvdWRmbGFyZS5jb20vYWpheC9saWJzL0xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLzIuMC4yL2xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL3Jhd2Nkbi5naXRoYWNrLmNvbS9weXRob24tdmlzdWFsaXphdGlvbi9mb2xpdW0vbWFzdGVyL2ZvbGl1bS90ZW1wbGF0ZXMvbGVhZmxldC5hd2Vzb21lLnJvdGF0ZS5jc3MiLz4KICAgIDxzdHlsZT5odG1sLCBib2R5IHt3aWR0aDogMTAwJTtoZWlnaHQ6IDEwMCU7bWFyZ2luOiAwO3BhZGRpbmc6IDA7fTwvc3R5bGU+CiAgICA8c3R5bGU+I21hcCB7cG9zaXRpb246YWJzb2x1dGU7dG9wOjA7Ym90dG9tOjA7cmlnaHQ6MDtsZWZ0OjA7fTwvc3R5bGU+CiAgICAKICAgICAgICAgICAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwKICAgICAgICAgICAgICAgIGluaXRpYWwtc2NhbGU9MS4wLCBtYXhpbXVtLXNjYWxlPTEuMCwgdXNlci1zY2FsYWJsZT1ubyIgLz4KICAgICAgICAgICAgPHN0eWxlPgogICAgICAgICAgICAgICAgI21hcF9jNDEyNzI2MmJlYTI0MzYzOWUwZmYxOWJkN2ZmZWU4ZCB7CiAgICAgICAgICAgICAgICAgICAgcG9zaXRpb246IHJlbGF0aXZlOwogICAgICAgICAgICAgICAgICAgIHdpZHRoOiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgbGVmdDogMC4wJTsKICAgICAgICAgICAgICAgICAgICB0b3A6IDAuMCU7CiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgIDwvc3R5bGU+CiAgICAgICAgCjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICAgICAgICAgIDxkaXYgY2xhc3M9ImZvbGl1bS1tYXAiIGlkPSJtYXBfYzQxMjcyNjJiZWEyNDM2MzllMGZmMTliZDdmZmVlOGQiID48L2Rpdj4KICAgICAgICAKPC9ib2R5Pgo8c2NyaXB0PiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFwX2M0MTI3MjYyYmVhMjQzNjM5ZTBmZjE5YmQ3ZmZlZThkID0gTC5tYXAoCiAgICAgICAgICAgICAgICAibWFwX2M0MTI3MjYyYmVhMjQzNjM5ZTBmZjE5YmQ3ZmZlZThkIiwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBjZW50ZXI6IFswLCAwXSwKICAgICAgICAgICAgICAgICAgICBjcnM6IEwuQ1JTLkVQU0czODU3LAogICAgICAgICAgICAgICAgICAgIHpvb206IDEsCiAgICAgICAgICAgICAgICAgICAgem9vbUNvbnRyb2w6IHRydWUsCiAgICAgICAgICAgICAgICAgICAgcHJlZmVyQ2FudmFzOiBmYWxzZSwKICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgKTsKCiAgICAgICAgICAgIAoKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgdGlsZV9sYXllcl8xMmFkNGVjZDg5OWQ0YTZiYTA1YmQ1ZDNlY2YyMDlmNCA9IEwudGlsZUxheWVyKAogICAgICAgICAgICAgICAgImh0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nIiwKICAgICAgICAgICAgICAgIHsiYXR0cmlidXRpb24iOiAiRGF0YSBieSBcdTAwMjZjb3B5OyBcdTAwM2NhIGhyZWY9XCJodHRwOi8vb3BlbnN0cmVldG1hcC5vcmdcIlx1MDAzZU9wZW5TdHJlZXRNYXBcdTAwM2MvYVx1MDAzZSwgdW5kZXIgXHUwMDNjYSBocmVmPVwiaHR0cDovL3d3dy5vcGVuc3RyZWV0bWFwLm9yZy9jb3B5cmlnaHRcIlx1MDAzZU9EYkxcdTAwM2MvYVx1MDAzZS4iLCAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsICJtYXhOYXRpdmVab29tIjogMTgsICJtYXhab29tIjogMTgsICJtaW5ab29tIjogMCwgIm5vV3JhcCI6IGZhbHNlLCAib3BhY2l0eSI6IDEsICJzdWJkb21haW5zIjogImFiYyIsICJ0bXMiOiBmYWxzZX0KICAgICAgICAgICAgKS5hZGRUbyhtYXBfYzQxMjcyNjJiZWEyNDM2MzllMGZmMTliZDdmZmVlOGQpOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNzY4MDEwZmJiZTBlNGY0MTkxZGE3YTU2ZmFmMmRlNjYgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy40OTcsIDEyNy4wMjY2XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2M0MTI3MjYyYmVhMjQzNjM5ZTBmZjE5YmQ3ZmZlZThkKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9lMGM3NzMwZWIwZDQ0M2ExODBjNzZmY2FiM2FjOTFmMCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNDM4NWE1YzM4Y2RjNGQ2ZGI5NGExODA3OTBkZGQyYmIgPSAkKGA8ZGl2IGlkPSJodG1sXzQzODVhNWMzOGNkYzRkNmRiOTRhMTgwNzkwZGRkMmJiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TYW1zdW5nOiAyMC41JTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9lMGM3NzMwZWIwZDQ0M2ExODBjNzZmY2FiM2FjOTFmMC5zZXRDb250ZW50KGh0bWxfNDM4NWE1YzM4Y2RjNGQ2ZGI5NGExODA3OTBkZGQyYmIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNzY4MDEwZmJiZTBlNGY0MTkxZGE3YTU2ZmFmMmRlNjYuYmluZFBvcHVwKHBvcHVwX2UwYzc3MzBlYjBkNDQzYTE4MGM3NmZjYWIzYWM5MWYwKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2YzMDFkM2QxMTA3MDQ2ODY5MDIxYWFkZWY0OTE3YTY3ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuMzMxOCwgLTEyMi4wMzExXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2M0MTI3MjYyYmVhMjQzNjM5ZTBmZjE5YmQ3ZmZlZThkKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF83M2QzN2QzMjViNzE0YmIxYTQ5N2Q3NDhjOTNhNGZlYiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYjcyOGRkNGY5YmIzNGE4M2I1ZjA5Y2MyYjJiOWFlNzYgPSAkKGA8ZGl2IGlkPSJodG1sX2I3MjhkZDRmOWJiMzRhODNiNWYwOWNjMmIyYjlhZTc2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5BcHBsZTogMTQuNCU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNzNkMzdkMzI1YjcxNGJiMWE0OTdkNzQ4YzkzYTRmZWIuc2V0Q29udGVudChodG1sX2I3MjhkZDRmOWJiMzRhODNiNWYwOWNjMmIyYjlhZTc2KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2YzMDFkM2QxMTA3MDQ2ODY5MDIxYWFkZWY0OTE3YTY3LmJpbmRQb3B1cChwb3B1cF83M2QzN2QzMjViNzE0YmIxYTQ5N2Q3NDhjOTNhNGZlYikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8wMTJlZDk2NzRhYWY0ZWFjOGYzOTA0M2Y2ZjAyZTBjOSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzIyLjU0MzEsIDExNC4wNTc5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2M0MTI3MjYyYmVhMjQzNjM5ZTBmZjE5YmQ3ZmZlZThkKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8zODE5ZGExZTEzZmU0MjUzOGRmN2JlMjAwODlkMzI4MCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNzA1ODBkYmRiNjE4NDc2ZDliZWVmYWYzYmZjN2E4YTcgPSAkKGA8ZGl2IGlkPSJodG1sXzcwNTgwZGJkYjYxODQ3NmQ5YmVlZmFmM2JmYzdhOGE3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5IdWF3ZWk6IDguOSU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMzgxOWRhMWUxM2ZlNDI1MzhkZjdiZTIwMDg5ZDMyODAuc2V0Q29udGVudChodG1sXzcwNTgwZGJkYjYxODQ3NmQ5YmVlZmFmM2JmYzdhOGE3KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzAxMmVkOTY3NGFhZjRlYWM4ZjM5MDQzZjZmMDJlMGM5LmJpbmRQb3B1cChwb3B1cF8zODE5ZGExZTEzZmU0MjUzOGRmN2JlMjAwODlkMzI4MCkKICAgICAgICA7CgogICAgICAgIAogICAgCjwvc2NyaXB0Pg==" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



This was just a short introduction to Jupyter notebooks, an open source technology that is increasingly used for data science and analysis.
