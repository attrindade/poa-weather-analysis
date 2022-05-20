# Comparing Porto Alegre's weather data from the last two decades

A comparison of the last two decades (2001 - 2021) of weather forecasting data from Porto Alegre.

## Table of Contents
* [Installation](#Installation)
* [Project Motivation](#motivation)
* [How to get the data](#get-data)
* [Project Overview](#p-over)
  * [Data Extraction](#p-extraction)
  * [Data Preparation & Cleaning](#p-preparation)
    * [Creating a Summarized Table](#p-summarized)

## Installation <a name="Installation"></a>

The code requires Python versions of 3.* and general libraries available through the Anaconda package. 

## Project Motivation <a name="motivation"></a>

As a citizen of Porto Alegre for more than 25 years I started to have the feeling that our weather is changing a little on these last years. Out of curiosity I wanted to visualize what differences are perceptible.

This is a simple project intended only to consolidate my knowledge of pandas, matplotlib and seaborn as I'm currently deepening my comprehension of them. I'm not offering a deep and solid analysis of Porto Alegre's weather changes along the years. My only plan is to learn and to see if my sensory perception of the city's temperatures seems to be in line with the data or not.

## How to get the data <a name="get-data"></a>

I got this .csv file with historical data from Porto Alegre from the INMET (Instituto Nacional de Meteorologia) website. It's very simple to get the data in the way want (specific variables, date range, etc). You can go to https://bdmep.inmet.gov.br/ (the INMET database website) and ask for it, it gives you many options to customise your data. The data (.csv) I've used in this project you can find on this repo as "DATA_POA_2001-01-01_2022-03-11.csv".

## Project Overview <a name="p-over"></a>

This project have the main goal of improving my EDA (Exploratory Data Analysis) skills so it was divided in the following parts: data extraction, data preparation (cleaning), creation of a summarized table to facilitate the analysis and EDA (that have multiple specific parts like summer, winter, precipitations)

### Data Extraction <a name="p-extraction"></a>

I read the data received from INMET and generated this dataframe with 7740 observations and 8 cols.

<table style="width:200px" border="1" class="dataframe"> <thead> <tr style="undefined:undefined"> <th></th> <th>Data Medicao</th> <th>PRECIPITACAO TOTAL, DIARIO (mm)</th> <th>TEMPERATURA MAXIMA, DIARIA (C)</th> <th>TEMPERATURA MEDIA, DIARIA (C)</th> <th>TEMPERATURA MINIMA, DIARIA (C)</th> <th>UMIDADE RELATIVA DO AR, MEDIA DIARIA (%)</th> <th>UMIDADE RELATIVA DO AR, MINIMA DIARIA (%)</th> <th>Unnamed: 7</th> </tr> </thead> <tbody> <tr> <th>0</th> <td>2001-01-01</td> <td>0</td> <td>30,1</td> <td>23,616667</td> <td>18,4</td> <td>68,458333</td> <td>48.0</td> <td>NaN</td> </tr> <tr> <th>1</th> <td>2001-01-02</td> <td>0</td> <td>32,1</td> <td>25,475</td> <td>20</td> <td>69,958333</td> <td>45.0</td> <td>NaN</td> </tr> <tr> <th>2</th> <td>2001-01-03</td> <td>0</td> <td>33,4</td> <td>26,345833</td> <td>21</td> <td>69,083333</td> <td>43.0</td> <td>NaN</td> </tr> </tbody></table>

<pre>&lt;class 'pandas.core.frame.DataFrame'&gt;
RangeIndex: 7740 entries, 0 to 7739
Data columns (total 8 columns):
 #   Column                                     Non-Null Count  Dtype  
---  ------                                     --------------  -----  
 0   Data Medicao                               7740 non-null   object 
 1   PRECIPITACAO TOTAL, DIARIO (mm)            7210 non-null   object 
 2   TEMPERATURA MAXIMA, DIARIA (C)             7448 non-null   object 
 3   TEMPERATURA MEDIA, DIARIA (C)              7232 non-null   object 
 4   TEMPERATURA MINIMA, DIARIA (C)             7453 non-null   object 
 5   UMIDADE RELATIVA DO AR, MEDIA DIARIA (%)   7529 non-null   object 
 6   UMIDADE RELATIVA DO AR, MINIMA DIARIA (%)  7628 non-null   float64
 7   Unnamed: 7                                 0 non-null      float64
dtypes: float64(2), object(6)
memory usage: 483.9+ KB
</pre>

### Data Preparation & Cleaning <a name="p-preparation"></a>

In this phase I've made many steps to improve the dataframe's readability and functionality, these steps were: 
- Drop the useless cols
- Simplify and translate cols names
- Convert str data types to datetime (date) and to float (others)
- Decrease the amount of NaN
- Dropping non-useful rows
- Creating 'year', 'month' and 'day' cols
- Set date as index

##### Decreasing the amount of NaN

In an attempt to decrease the amount of NaN and trying to lose the least amount of rows, I try to make the average from min and max temperature and replace the NaN of the avg_temp with it, in the rows where I have min & max temp but not avg.

##### Dropping non-useful rows

As I don't plan right now to apply ML models on this dataset the best choice for the rows that contain too many NaNs is to drop it commpletely

##### Results from this phase

<table border="1" class="dataframe"> <thead> <tr style="text-align: right;"> <th></th> <th>total_precip</th> <th>max_temp</th> <th>avg_temp</th> <th>min_temp</th> <th>avg_humidity</th> <th>min_humidity</th> <th>year</th> <th>month</th> <th>day</th> </tr> <tr> <th>date</th> <th></th> <th></th> <th></th> <th></th> <th></th> <th></th> <th></th> <th></th> <th></th> </tr> </thead> <tbody> <tr> <th>2001-01-01</th> <td>0.0</td> <td>30.1</td> <td>23.616667</td> <td>18.4</td> <td>68.458333</td> <td>48.0</td> <td>2001</td> <td>1</td> <td>1</td> </tr> <tr> <th>2001-01-02</th> <td>0.0</td> <td>32.1</td> <td>25.475000</td> <td>20.0</td> <td>69.958333</td> <td>45.0</td> <td>2001</td> <td>1</td> <td>2</td> </tr> <tr> <th>2001-01-03</th> <td>0.0</td> <td>33.4</td> <td>26.345833</td> <td>21.0</td> <td>69.083333</td> <td>43.0</td> <td>2001</td> <td>1</td> <td>3</td> </tr> </tbody></table>

<pre>&lt;class 'pandas.core.frame.DataFrame'&gt;
DatetimeIndex: 7483 entries, 2001-01-01 to 2022-03-10
Data columns (total 9 columns):
 #   Column        Non-Null Count  Dtype  
---  ------        --------------  -----  
 0   total_precip  7209 non-null   float64
 1   max_temp      7448 non-null   float64
 2   avg_temp      7425 non-null   float64
 3   min_temp      7432 non-null   float64
 4   avg_humidity  7481 non-null   float64
 5   min_humidity  7455 non-null   float64
 6   year          7483 non-null   int64  
 7   month         7483 non-null   int64  
 8   day           7483 non-null   int64  
dtypes: float64(6), int64(3)
memory usage: 584.6 KB
</pre>

After cleaning & preparation: 7483 observations and 9 cols.

#### Creating a Summarized Table <a name="p-summarized"></a>

In this second part of data cleaning/preparation we will summarize all the data from our dataframe and create another dataframe with all these summarized info. We will firstly divide summer, seasons and years in different tables. After that we will put it all together in a summarized table called 'seasons'.

##### Results from this phase

<table border="1" class="dataframe"> <thead> <tr style="undefined:undefined"> <th></th> <th>SUM_temp</th> <th>SUM_max</th> <th>SUM_max_avg</th> <th>SUM_min</th> <th>SUM_min_avg</th> <th>SUM_hum</th> <th>SUM_hum_min</th> <th>WIN_temp</th> <th>WIN_max</th> <th>WIN_max_avg</th> <th>WIN_min</th> <th>WIN_min_avg</th> <th>win_hum</th> <th>WIN_hum_min</th> </tr> </thead> <tbody> <tr> <th>2001</th> <td>24.68</td> <td>37.9</td> <td>30.56</td> <td>14.9</td> <td>20.58</td> <td>69.79</td> <td>49.92</td> <td>16.66</td> <td>30.9</td> <td>21.85</td> <td>2.6</td> <td>12.77</td> <td>77.11</td> <td>58.29</td> </tr> <tr> <th>2002</th> <td>25.47</td> <td>38.0</td> <td>31.20</td> <td>15.7</td> <td>21.11</td> <td>71.35</td> <td>50.67</td> <td>15.71</td> <td>33.7</td> <td>20.57</td> <td>3.4</td> <td>12.05</td> <td>77.31</td> <td>55.75</td> </tr> <tr> <th>2003</th> <td>23.93</td> <td>35.8</td> <td>29.78</td> <td>15.1</td> <td>19.74</td> <td>70.27</td> <td>55.64</td> <td>15.26</td> <td>34.5</td> <td>20.58</td> <td>3.0</td> <td>11.26</td> <td>76.22</td> <td>53.50</td> </tr> <tr> <th>2004</th> <td>24.57</td> <td>39.4</td> <td>30.89</td> <td>15.0</td> <td>20.19</td> <td>67.79</td> <td>45.67</td> <td>15.60</td> <td>36.7</td> <td>21.00</td> <td>2.7</td> <td>11.54</td> <td>76.94</td> <td>45.25</td> </tr> <tr> <th>2005</th> <td>24.63</td> <td>38.7</td> <td>30.47</td> <td>13.5</td> <td>20.60</td> <td>70.91</td> <td>53.08</td> <td>16.34</td> <td>31.9</td> <td>21.53</td> <td>1.9</td> <td>12.50</td> <td>77.30</td> <td>49.54</td> </tr> <tr> <th>2006</th> <td>24.87</td> <td>35.8</td> <td>30.53</td> <td>15.1</td> <td>20.70</td> <td>71.96</td> <td>52.46</td> <td>15.94</td> <td>32.3</td> <td>21.17</td> <td>2.7</td> <td>12.12</td> <td>77.37</td> <td>55.08</td> </tr> <tr> <th>2007</th> <td>24.21</td> <td>37.3</td> <td>30.05</td> <td>14.4</td> <td>20.06</td> <td>71.43</td> <td>56.46</td> <td>14.62</td> <td>33.2</td> <td>19.73</td> <td>2.2</td> <td>10.78</td> <td>78.32</td> <td>48.29</td> </tr> <tr> <th>2008</th> <td>23.66</td> <td>35.3</td> <td>29.16</td> <td>13.9</td> <td>19.82</td> <td>73.34</td> <td>55.25</td> <td>14.92</td> <td>31.8</td> <td>19.80</td> <td>2.3</td> <td>11.40</td> <td>78.33</td> <td>57.12</td> </tr> <tr> <th>2009</th> <td>25.17</td> <td>38.5</td> <td>30.71</td> <td>13.7</td> <td>21.47</td> <td>73.96</td> <td>52.38</td> <td>14.76</td> <td>33.4</td> <td>19.87</td> <td>0.3</td> <td>10.92</td> <td>77.64</td> <td>42.42</td> </tr> <tr> <th>2010</th> <td>24.55</td> <td>36.2</td> <td>30.12</td> <td>14.1</td> <td>20.99</td> <td>75.54</td> <td>63.04</td> <td>15.34</td> <td>32.9</td> <td>20.23</td> <td>2.8</td> <td>11.79</td> <td>78.64</td> <td>58.00</td> </tr> <tr> <th>2011</th> <td>24.85</td> <td>38.4</td> <td>31.08</td> <td>14.7</td> <td>20.55</td> <td>71.39</td> <td>54.67</td> <td>14.29</td> <td>32.4</td> <td>19.36</td> <td>1.9</td> <td>10.71</td> <td>79.40</td> <td>60.75</td> </tr> <tr> <th>2012</th> <td>23.61</td> <td>39.0</td> <td>29.35</td> <td>14.4</td> <td>19.69</td> <td>72.25</td> <td>52.29</td> <td>16.46</td> <td>33.0</td> <td>22.20</td> <td>1.1</td> <td>12.34</td> <td>76.24</td> <td>49.17</td> </tr> <tr> <th>2013</th> <td>25.76</td> <td>40.6</td> <td>31.93</td> <td>17.4</td> <td>21.60</td> <td>72.23</td> <td>41.71</td> <td>14.89</td> <td>35.1</td> <td>20.31</td> <td>1.4</td> <td>11.08</td> <td>80.03</td> <td>55.96</td> </tr> <tr> <th>2014</th> <td>24.86</td> <td>36.5</td> <td>30.35</td> <td>14.4</td> <td>21.13</td> <td>76.41</td> <td>60.62</td> <td>15.88</td> <td>34.3</td> <td>21.16</td> <td>3.6</td> <td>12.16</td> <td>81.22</td> <td>56.17</td> </tr> <tr> <th>2015</th> <td>25.07</td> <td>38.9</td> <td>30.73</td> <td>16.9</td> <td>21.17</td> <td>76.59</td> <td>52.71</td> <td>17.21</td> <td>34.8</td> <td>22.16</td> <td>5.7</td> <td>13.74</td> <td>81.11</td> <td>47.62</td> </tr> <tr> <th>2016</th> <td>25.32</td> <td>38.3</td> <td>31.09</td> <td>13.6</td> <td>21.47</td> <td>78.17</td> <td>65.25</td> <td>14.72</td> <td>32.9</td> <td>19.84</td> <td>4.1</td> <td>11.14</td> <td>81.85</td> <td>57.88</td> </tr> <tr> <th>2017</th> <td>24.10</td> <td>36.8</td> <td>30.10</td> <td>13.9</td> <td>20.06</td> <td>77.08</td> <td>53.08</td> <td>17.66</td> <td>34.8</td> <td>23.49</td> <td>5.5</td> <td>13.89</td> <td>81.34</td> <td>60.83</td> </tr> <tr> <th>2018</th> <td>25.28</td> <td>38.5</td> <td>31.03</td> <td>16.9</td> <td>21.42</td> <td>77.38</td> <td>61.17</td> <td>14.92</td> <td>32.9</td> <td>20.27</td> <td>2.6</td> <td>11.19</td> <td>84.34</td> <td>64.25</td> </tr> <tr> <th>2019</th> <td>25.20</td> <td>40.3</td> <td>31.94</td> <td>14.6</td> <td>20.58</td> <td>68.51</td> <td>51.62</td> <td>16.11</td> <td>36.1</td> <td>21.81</td> <td>2.2</td> <td>12.24</td> <td>79.47</td> <td>60.04</td> </tr> <tr> <th>2020</th> <td>24.67</td> <td>38.3</td> <td>30.71</td> <td>15.7</td> <td>20.58</td> <td>72.95</td> <td>48.71</td> <td>15.56</td> <td>31.3</td> <td>21.19</td> <td>2.7</td> <td>11.52</td> <td>78.93</td> <td>57.33</td> </tr> <tr> <th>2021</th> <td>25.52</td> <td>40.3</td> <td>32.17</td> <td>17.4</td> <td>21.01</td> <td>71.67</td> <td>58.08</td> <td>15.35</td> <td>34.5</td> <td>20.68</td> <td>2.3</td> <td>11.66</td> <td>79.13</td> <td>55.38</td> </tr> </tbody></table>

* SUM : Summer
* WIN : Winter
* SUM/WIN_temp --> The average for that summer's/winter's temperatures
* SUM/WIN_max --> The highest temperature for that summer/winter
* SUM/WIN_min --> The lowest temperature for that summer/winter
* SUM/WIN_max_avg --> The average for that summer's/winter's daily maximum temperatures
* SUM/WIN_min_avg --> The average for that summer's/winter's daily minimum temperatures
* SUM/WIN_hum --> The average of the daily humidities of that summer/winter
* SUM/WIN_hum_min --> The lowest humidity of that summer/winter


### EDA (Exploratory Data Analysis)

In this phase I try to answer a few questions about many topics related to this sensorial feeling that Porto Alegre is getting hotter.

#### What about the summer?

Questions to answer:

- Did the avg temperature rose up in these last 7 years?
- Did the max temperature rose up?

To answer these questions my strategy was to plot the data from the summer's data in the summarised dataframe.

![download](https://user-images.githubusercontent.com/6825324/169603535-e5570e4f-e033-4c45-92bd-0b160bfeb8e4.png)

From the graph above it really seems like the average temperatures and max temperatures really got a little higher. There are many interesting insights that can be made from this plot, but the general idea it gives to me is that all the variables are getting higher on average.

As an attempt to visualize more this change of the averages I've tried to plot a new graph with a line in the center representing the mean of all the year averages from these two decades (which is 24.76C°). After this line, I plotted on this same graph bars that represents how far each year's average is from that mean, they represent the diversion of the the values in relation to that mean/line.

![download](https://user-images.githubusercontent.com/6825324/169598617-5b67bd4a-8ada-4cad-882a-d4a432bebe64.png)

As we can see in the plot above, the year's summer temperature average is increasing. The first decade of the century had on average lower temperatures during the summer and the second decade the opposite.

This analysis reinforce the idea that Porto Alegre's summers are getting hotter. My perception that our summers are being hotter seems to be in line with data.

<table border="1" class="dataframe"> <thead> <tr style="undefined:undefined"> <th></th> <th>Average temperatures</th> <th>Average of year's highest temperatures</th> <th>Average of daily maximum temperatures</th> <th>Average of year's lowest temperatures</th> <th>Average of daily minimum temperatures</th> </tr> </thead> <tbody> <tr> <th>2001 - 2007</th> <td>24.68</td> <td>37.9</td> <td>30.56</td> <td>14.9</td> <td>20.58</td> </tr> <tr> <th>2008 - 2014</th> <td>23.66</td> <td>35.3</td> <td>29.16</td> <td>13.9</td> <td>19.82</td> </tr> <tr> <th>2015 - 2021</th> <td>25.07</td> <td>38.9</td> <td>30.73</td> <td>16.9</td> <td>21.17</td> </tr> </tbody></table>

With the plots that I showed before and with this table that shows the mean of many variables of the city summers I can confirm to myself that at least my perception is true: Porto Alegre is having higher temperatures and higher averages during these last 7 years.

In this last table we can perceiva that the averages oscilate trough the years, but in the year to year comparison the last years seeems to have a slightly hotter weather. We can't confirm with certainty that my perception is related to any real changes in the city's climate, but it certainly goes in the same direction that some studies already found (I will link an interesting one below), the climate change is starting to be noticeable by people who live in the city.

https://www.ufrgs.br/sextante/mudanca-climatica-no-rio-grande-do-sul/

It's interesting to notice that summer's average of max temperatures is quite similar between the beggining of the first decade and the end of the second decade. But also interesting to notice that the data about the minimum temperatures seems to be getting a little higher even though they also seems to be quite oscilating.
