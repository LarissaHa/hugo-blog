+++
author = "Larissa"
categories = ["Paper", "Rapid Miner"]
tags = ["prediction", "regression", "machine-learning", "spring2017"]
date = "2017-05-26"
description = "Data Mining 1 - Project Report - Spring Semester 2017"
featured = ""
featuredpath = "date"
linktitle = ""
title = "Predicting Bike Rentals in Hamburg"
type = "post"

+++

> presented by\\
> Larissa Haas\\
> Philipp Jamscikow\\
> Philipp Naeser\\
> Jan Wagner\\
> Julian Weller
> 
> submitted to the Data and Web Science Group\\
> Prof. Dr. Christian Bizer\\
> University of Mannheim

Application Area and Goals
==========================

This year, the city of Mannheim celebrates the 200<sup>th</sup>
anniversary of the invention of the bicycle. Reason enough to dig into
the German bike-sharing market. ’Call a Bike’[1] is one of the main
competitors in Germany and provides a huge amount of information via the
Deutsche Bahn - Open Data Portal[2]. In Hamburg, ’Call a Bike’ operates
under the name ’StadtRad’ in cooperation with the city itself. Hamburg
is also the biggest location of ’Call a Bike’, with the most stations,
bicycles, customers and bookings. Our goal is to predict the demand of
rented bicycles, binned in ’below average’, ’average’ and ’above
average’ for the most frequented rental station in Hamburg
(’Allende-Platz/Grindelhof’).

For our project, we also built up on the work of Stefan Opitz[3] and the
participants of the Kaggle - ’Bike Sharing Demand’ - competition[4].
While most of the Kaggle-participants focused on the exploration of the
data and tried to solve the problem with regression. We focused mainly
on classification and only had a short look on regression, our approach
and findings are topic of this report.


Structure and Size of the Dataset
=================================

## Bike Rental Bookings

The complete data consists of six datasets with a total size of around
16 GB. The most important information contains the BOOKING dataset
(about 6 GB). We examined also the other data sets, but decided not to
use them, because they contain mainly information about the car sharing
project of DB and have nothing or very less to do with our ’Call a Bike’
project. In the end, we only used the time stamp attribute together with
the amount of used bikes from the original dataset.

## Contextual Explanatory Attributes

As riding a bike takes place on the outside, we expect the weather to
heavily impact the demand for bike rentals. Especially, temperature and
precipitation are likely to exhibit explanatory power. Moreover, the
booking behavior of the customers might be determined by whether the
respective day is a weekday or public holiday. Customers might use the
bikes to get to work and return back home, which is why we expect the
number of bookings at the weekend and during holidays to be lower than
usual. We acquired the holiday data from *schulferien.org* and turned to
the web portal of *Deutsche Wetterdienst* for the weather data. Whether
a day is a workday or not, we decided to compute directly with the given
date by means of *Excel*. Further technical details about merging these
data sources are outlined in chapter 3.

Preprocessing
=============

## Preprocessing with Unix Commands and Python 

Please note, all the preprocessing steps described in this section are
documented and described in more detail in the corresponding
Ipython-Notebook (’Data Preprocessing and Descriptive
Statistics.ipynb’).

The first challenge we faced, even before starting with the actual
preprocessing, was to reduce the size of the relevant CSV file provided
by Deutsche Bahn  
(’HACKATHON\_BOOKING\_CALL\_A\_BIKE.csv’). This file is 6.48 GB and thus
too large to be opened in any conventional text editor.

By using Unix commands such as ‘grep’ we were able to filter the initial
CSV dataset, that includes entries for several German cities, for
columns containing the term ’Hamburg’. However, this dataset was still
too large for being processed on a desktop computer (3.44 GB). So we
decided to draw a random sample of this dataset that contained 10,000
lines. We then examined this random sample in detail with Python Pandas.
After deciding on which attributes to keep, we applied the whole
filtering procedure on the entire dataset.

Data is represented in Python Pandas as a dataframe with attributes as
column labels and with an flexible row index to address single entries.
Pandas allowed for easy and fast data-manipulation, querying, and
slicing data. On the one hand, we made use of Pandas to detect the most
frequented station in Hamburg, which is ’Allende-Platz/Grindelhof’, and
then compare its attributes with the other most frequented stations. The
station selected is representative for this group by showing similar
(temporal) usage patterns. On the other hand, Pandas allowed us to
examine the integrity of entries, detect redundant attributes and
missing values.

The output file of this first preprocessing is the file ’Bookings AG
Station.csv’. It serves as input to the Excel-file in chapter 3.2.

## Preprocessing in Excel

As it allows for manipulating the data more directly, we now use Excel
for finishing the preprocessing. The corresponding file is called
**Pre-ProcessingExcel.xlsx**. It consist of four sections: *INFOCONFIG*
just allows for setting values for some parameters and comes with a
brief description of the color codes used. *OUT* contains the output of
the Excel-file, that is, the data in these worksheets will be stored in
CSV-Files and subsequently serve as input to our models. *AGG* is used
to aggregate the data from the worksheets between the *IN* borders.
Basically, the data flows from *IN* through *AGG* to *OUT*.

### IN Section

This section contains four worksheets with tables from different sources
which we want to bring together: (1) *BookingsAGStation* contains the
output of chapter 3.1. (2) *Temperature* and (3) *Precipitation* provide
hourly weather data from *Deutsche Wetterdienst’s* website for the
weather station *Hamburg-Fuhlsbüttel* as it is the most central one for
Hamburg that is offered. It should also be pointed out that in the
*Precipitation*-table some values for some hours were missing. We
checked the values on these days and decided to set all the missing
values equal to zero as it had not rained on the respective days during
the hours for which data was provided anyway. (4) *Vacation* contains
school holiday data for the federal state of Hamburg from
*schulferien.org*.

### AGG Section

We now think of data from 2014 and 2015 as being the training data,
while data from 2016 is meant to be test data. The section contains two
worksheets: (1) *MEANSTD* is used to calculate for both the training and
the test data the respective average counts of bookings per hour in
given a month as well as the corresponding standard deviations. It is
important to mention that for the training data we compute these metrics
regardless of the year. Thus, e.g. the average count of bookings per
hour is the same for January 2014 and January 2015 in the table on the
left of the *MEANSTD*-worksheet. The idea here is to throw all the
hourly counts of bookings from January 2014 and January 2015 into a bag
and calculate the average and standard deviation based on these (2
Januaries \* 31 days per January \* 24 hours per day equals) 1,488
values. At the heart of the (2) *AGG*-worksheet lies the
*Allende-Platz/Grindelhof*-column which contains for each date for a
given hour the (aggregated) count of bookings at that station. The dates
run from January 1<sup>st</sup>, 2014 through December 31<sup>st</sup>,
2015 (for the training data) down to June 30<sup>th</sup>, 2016 (for the
test data). Moreover, it provides some additional date- and time-based
columns which have all been (indirectly) derived from the ID column as
well as the respective data from the *Temperature*-, *Precipitation*-
and *Vacation*-worksheets. We also added a binary column for
*Precipitation* which values are equal to 1 if it rains (that is,
*Precipitation* is greater than 0) and 0, otherwise. There is also a
*Workday*-column that we introduced as the demand for rented bikes might
be lower or higher at the weekend (at Saturdays and Sundays) than from
Monday until Friday. Next, *AG (STD above MEAN)* takes the count of
bookings at a given hour (the respective value in the
*Allende-Platz/Grindelhof*-column) and subtracts the corresponding mean
which was previously calculated in the *MEANSTD*-worksheet given the
month and data set assignment (training or test data) of the respective
record. This deviation from the mean is then set into relation by
division through the respective standard deviation. For example, at
January 1<sup>st</sup>, 2014 there were four bookings at 0 o’clock. The
corresponding mean is 4.0619 and the corresponding standard deviation
equals 4.3771. Consequently, the *AG (STD above MEAN)*-value is -0.0619
divided by 4.3771 and thus -0.0141. The *AG (Count of STD above MEAN)*
column rounds-off these values to zero decimal digits and might be
useful for descriptive statistics. *AG Class* is our target attribute
for prediction. On the *INFOCONFIG*, a threshold value called
*ClassThreshold* can be provided for separating the records into one of
the following three classes: **below**, **average** or **above**. We
decided to set it to 0.5. This seemed to be a reasonable boundary for
calculating the values in the *AG Class* on the *MEANSTD*-worksheet: If
the corresponding *AG (STD above MEAN)* absolute value of a record is
less than or equal to 0.5, we assign the label **average**. If the
corresponding *AG (STD above MEAN)* value on the other hand is less than
-0.5, we label the record as **below**, while for values greater than
0.5 we assign the label **above**. The rightmost column *AG (above
mean)* just checks if the count of bookings for a given record is above
or below the mean of its peer group (the metrics on the
*MEANSTD*-worksheet). It also serves as a simplified additional class
which values could also be predicted.

### OUT Section

Finally, the *OUT*-section contains four worksheets: two training data
worksheets and two test data worksheets. The only difference between the
worksheets with an *HOURS*-suffix and the worksheets without it is that
the worksheets marked with *HOURS* contain 24 additional columns for the
hours. Values of 1 have been assigned if the respective record is from
the given hour and values of 0, otherwise. We introduced these columns
as it allowed for the easier specification of some regression models
which we gave a try. In the end, there are four corresponding output
CSV-files for building and evaluating the models.

Data Mining
===========

## Descriptive Statistics and Visual Exploration

First, let us have a look at some summary statistics of the training
dataset in table 4.1 and table 4.2:

<table>
<caption>Summary Statistics of Training Data<span data-label="tab:meinetabelle3"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Variable</strong></td>
<td align="center"><strong>Observations</strong></td>
<td align="center"><strong>Mean</strong></td>
<td align="center"><strong>Std. Dev.</strong></td>
<td align="center"><strong>Min</strong></td>
<td align="center"><strong>Max</strong></td>
</tr>
<tr class="even">
<td align="left">Temperature</td>
<td align="center">17,502</td>
<td align="center">10.49</td>
<td align="center">6.70</td>
<td align="center">-12.4</td>
<td align="center">36.0</td>
</tr>
<tr class="odd">
<td align="left">Vacation</td>
<td align="center">17,502</td>
<td align="center">0.24</td>
<td align="center">0.43</td>
<td align="center">0</td>
<td align="center">1</td>
</tr>
<tr class="even">
<td align="left">Workday</td>
<td align="center">17,502</td>
<td align="center">0.72</td>
<td align="center">0.45</td>
<td align="center">0</td>
<td align="center">1</td>
</tr>
<tr class="odd">
<td align="left">Allende-Platz/Grindelhof</td>
<td align="center">17,502</td>
<td align="center">6.07</td>
<td align="center">5.88</td>
<td align="center">0</td>
<td align="center">63</td>
</tr>
<tr class="even">
<td align="left">AG (STD above MEAN)</td>
<td align="center">17,502</td>
<td align="center">0.46</td>
<td align="center">1.34</td>
<td align="center">-0.93</td>
<td align="center">13.47</td>
</tr>
<tr class="odd">
<td align="left">AG (above mean)</td>
<td align="center">17,502</td>
<td align="center">0.51</td>
<td align="center">0.50</td>
<td align="center">0</td>
<td align="center">1</td>
</tr>
<tr class="even">
<td align="left">Precipitation</td>
<td align="center">17,502</td>
<td align="center">0.08</td>
<td align="center">0.44</td>
<td align="center">0</td>
<td align="center">14.9</td>
</tr>
<tr class="odd">
<td align="left">Precipitation yn</td>
<td align="center">17,502</td>
<td align="center">0.12</td>
<td align="center">0.32</td>
<td align="center">0</td>
<td align="center">1</td>
</tr>
</tbody>
</table>

<table>
<caption>AG Class Counts of Training Data<span data-label="tab:meinetabelle4"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Label</strong></td>
<td align="center"><strong>Observations</strong></td>
<td align="center"><strong>Observations %</strong></td>
</tr>
<tr class="even">
<td align="left">above</td>
<td align="center">6,776</td>
<td align="center">38.7%</td>
</tr>
<tr class="odd">
<td align="left">average</td>
<td align="center">5,924</td>
<td align="center">33.8%</td>
</tr>
<tr class="even">
<td align="left">below</td>
<td align="center">4,802</td>
<td align="center">27.4%</td>
</tr>
</tbody>
</table>

Tables 4.3 and 4.4 show the same statistics for the test data from 2016:

<table>
<caption>Summary Statistics of Test Data<span data-label="tab:meinetabelle5"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Variable</strong></td>
<td align="center"><strong>Observations</strong></td>
<td align="center"><strong>Mean</strong></td>
<td align="center"><strong>Std. Dev.</strong></td>
<td align="center"><strong>Min</strong></td>
<td align="center"><strong>Max</strong></td>
</tr>
<tr class="even">
<td align="left">Temperature</td>
<td align="center">4,359</td>
<td align="center">8.11</td>
<td align="center">7.29</td>
<td align="center">-8.7</td>
<td align="center">31.5</td>
</tr>
<tr class="odd">
<td align="left">Vacation</td>
<td align="center">4,359</td>
<td align="center">0.10</td>
<td align="center">0.31</td>
<td align="center">0</td>
<td align="center">1</td>
</tr>
<tr class="even">
<td align="left">Workday</td>
<td align="center">4,359</td>
<td align="center">0.71</td>
<td align="center">0.45</td>
<td align="center">0</td>
<td align="center">1</td>
</tr>
<tr class="odd">
<td align="left">Allende-Platz/Grindelhof</td>
<td align="center">4,359</td>
<td align="center">5.65</td>
<td align="center">5.73</td>
<td align="center">0</td>
<td align="center">44</td>
</tr>
<tr class="even">
<td align="left">AG (STD above MEAN)</td>
<td align="center">4,359</td>
<td align="center">0.36</td>
<td align="center">1.31</td>
<td align="center">-0.93</td>
<td align="center">9.12</td>
</tr>
<tr class="odd">
<td align="left">AG (above mean)</td>
<td align="center">4,359</td>
<td align="center">0.48</td>
<td align="center">0.50</td>
<td align="center">0</td>
<td align="center">1</td>
</tr>
<tr class="even">
<td align="left">Precipitation</td>
<td align="center">4,359</td>
<td align="center">0.09</td>
<td align="center">0.54</td>
<td align="center">0</td>
<td align="center">21.7</td>
</tr>
<tr class="odd">
<td align="left">Precipitation yn</td>
<td align="center">4,359</td>
<td align="center">0.13</td>
<td align="center">0.33</td>
<td align="center">0</td>
<td align="center">1</td>
</tr>
</tbody>
</table>

<table>
<caption>AG Class Counts of Test Data<span data-label="tab:meinetabelle6"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Label</strong></td>
<td align="center"><strong>Observations</strong></td>
<td align="center"><strong>Observations %</strong></td>
</tr>
<tr class="even">
<td align="left">above</td>
<td align="center">1,284</td>
<td align="center">29.5%</td>
</tr>
<tr class="odd">
<td align="left">average</td>
<td align="center">1,535</td>
<td align="center">35.2%</td>
</tr>
<tr class="even">
<td align="left">below</td>
<td align="center">1,540</td>
<td align="center">35.3%</td>
</tr>
</tbody>
</table>

Figure 4.1 shows the average count of bookings per hour and how it is
related to the average temperature in both the training and the test
data:

<img src="Figures/Figure41.jpg" alt="Average Count of Bookings versus Temperature" />

In general, greater average counts of bookings per hour seem to be
positively correlated to the temperature. In chapter 4.2, we will now
turn to classification.

## Classification

The goal of our classification is to predict *AG Class* for the year
2016. As training data, we use the years 2014 and 2015. To keep the time
range equal in the training and test data, only the months January to
June are used in the test set, since the data for 2016 was only complete
for those months.

Furthermore, we will not use the complete set, since it includes the
number of bookings and our calculations for e.g. the standard deviation.
Therefore, we will select a subset for our classification first. This
subset includes the following variables: day, month, hour, temperature,
precipitation, precipitation yn, workday and vacation.

For each model, we will first select the relevant variable subset (using
the optimize selection operator in RapidMiner with selection direction
backward) and then optimize the parameters to achieve more accurate
predictions. For the latter, the optimize parameter (grid) operator is
used. Each model is evaluated based on the confusion matrix (precision,
recall and f-measure).  
To prevent a variable to dominate, all numeric values are normalized. In
our dataset, this only applies to temperature and precipitation. The
other values are either polynominal (day, month, hour) or binominal
(precipitation yn, workday, vacation) and therefore do not require
further changing.

### k-Nearest Neighbors

We start with the k-Nearest Neighbors operator. As already described, in
the first step, we will identify the relevant variables for our
classification, and then proceed tuning the parameters of the operator
to improve our results. The optimize selection lead to the following
relevant variables for our classification: temperature, precipitation,
precipitation yn, hour, workday. The values of month, day and vacation
are not used.

As the datasets contain mixed data (polynominal, binominal and
numerical), the measeure type is set to mixed measures, which means we
are fixed to mixed Euclidean distance. This leaves only k as changeable
parameter. Therefore, the optimize parameter operator will try to find
the best possible k. We set k to be possible between 1 and 30. The
optimal k for our purpose was 25.

<table>
<caption>k-NN Results<span data-label="tab:meinetabelle7"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Step</strong></td>
<td align="center"><strong>F-measure</strong></td>
<td align="center"><strong>Recall</strong></td>
<td align="center"><strong>Precision</strong></td>
</tr>
<tr class="even">
<td align="left">unoptimized</td>
<td align="center">60.10%</td>
<td align="center">60.08%</td>
<td align="center">60.13%</td>
</tr>
<tr class="odd">
<td align="left">optimize selection</td>
<td align="center">60.93%</td>
<td align="center">60.89%</td>
<td align="center">60.98%</td>
</tr>
<tr class="even">
<td align="left">k=25 without selection</td>
<td align="center">66.26%</td>
<td align="center">66.12%</td>
<td align="center">66.40%</td>
</tr>
<tr class="odd">
<td align="left">optimize parameter</td>
<td align="center">70.55%</td>
<td align="center">69.69%</td>
<td align="center">71.43%</td>
</tr>
</tbody>
</table>

### Naive Bayes

Since the Naive Bayes operator has only one parameter (use Laplace
correction, which does not influence our results), only the optimize
selection operator is used to improve our results.

The relevant variable subset for Naive Bayes is: day, temperature,
precipitation yn, hour, workday and vacation. Month and precipitation
are not used.

<table>
<caption>Naive Bayes Results<span data-label="tab:meinetabelle8"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Step</strong></td>
<td align="center"><strong>F-measure</strong></td>
<td align="center"><strong>Recall</strong></td>
<td align="center"><strong>Precision</strong></td>
</tr>
<tr class="even">
<td align="left">unoptimized</td>
<td align="center">67.52%</td>
<td align="center">66.71%</td>
<td align="center">68.37%</td>
</tr>
<tr class="odd">
<td align="left">optimize selection</td>
<td align="center">68.12%</td>
<td align="center">67.76%</td>
<td align="center">68.49%</td>
</tr>
</tbody>
</table>

### Decision Trees

For the decision tree, we will again apply both optimizing steps. When
running the optimize selection operator though, the result shows that
the best selection of variables is taking all of them into account.
Therefore, this step does not change the result in any way.

The optimize parameter step however is a very important one for decision
tree, since we have multiple parameters we can change. Here we can
decide the maximal depth of the tree, the leaf size and the minimal
information gain for a split, to find a tree that fits the problem
without overfitting it to the training set.

The best results were found with the following parameters: criterion =
accuracy, maxdepth = 4, minimalgain = 0.01, minimalleafsize = 8.

<table>
<caption>Decision Tree Results<span data-label="tab:meinetabelle9"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Step</strong></td>
<td align="center"><strong>F-measure</strong></td>
<td align="center"><strong>Recall</strong></td>
<td align="center"><strong>Precision</strong></td>
</tr>
<tr class="even">
<td align="left">unoptimized</td>
<td align="center">37.93%</td>
<td align="center">41.01%</td>
<td align="center">35.28%</td>
</tr>
<tr class="odd">
<td align="left">optimize selection</td>
<td align="center">37.93%</td>
<td align="center">41.01%</td>
<td align="center">35.28%</td>
</tr>
<tr class="even">
<td align="left">optimize parameter</td>
<td align="center">70.05%</td>
<td align="center">69.39%</td>
<td align="center">70.72%</td>
</tr>
</tbody>
</table>

## Regression and Time Series Analysis

Besides the classification, i.e. the prediction of an average, low or
high amount of bookings, we thought it would be interesting to predict
the exact booking number of the observed station. This is not a
classification, but an regression task and was also performed with
RapidMiner.

After comparing various regression algorithms, we built a process
containing Gradient Boosted Trees, to predict the exact number of
bicycles rented per hour. After a basic feature subset selection,
parameter optimization, and outlier removal (everything above 4 standard
deviations filtered out), this process achieved a Root Mean Squared
Error on the 2016 testing data set of roughly 3.7 (bookings per hour).

A time series model fitting the data would be a model consisting of an
Error, Trend, and Seasonality component, a so-called ETS model. We saw
in the introductory, descriptive part that the bike rentals exhibit
several temporal patterns: on a daily, weekly, and monthly basis. The
trend from 2014 to 2015 is slightly downward. After failing to model the
time series within RapidMiner, we tried Facebook’s recently open-sourced
forecast library ‘FB Prophet’[5]. However, we could not deliver
comparable results since Prophet only allows for daily observations up
today, and our goal was to predict hourly usage rates. Nonetheless, an
ETS forecast would probably be the go-to solution in a realistic
scenario.

# Conclusion/Lessons Learned

The highest f-measure achieved was 70.55%. This is a good result,
considering that only the time and date, temperature and rain data are
used to predict a booking behavior in three classes. With this limited
information, one will reach a limit of possible predictions since many
possible influences are not included (e.g. events in the area). Also the
training data consists of only two years (since the ’Call a Bike’ offer
only exists since 2014), which is to short to be able to predict a solid
trend. The booking behavior also changed in total over the years, this
cannot be compensated in such a small range of time.  
An important lesson we learned is that one has to make sure, that the
methods applied fit the problem. As the Kaggle competition showed, this
is more of a problem for a regression or time series analysis, and does
not fit classification that well. This is something that has to be
considered when choosing a data set for a classification task.
Furthermore the pre-processing of the data can consume a lot of time,
especially if you have to break down very big sets of data.

[1] Part of the Deutsche Bahn AG: <https://www.callabike-interaktiv.de/>

[2] Deutsche Bahn - Open Data Portal:
<http://data.deutschebahn.com/dataset/data-call-a-bike>

[3] <https://www.stefan-opitz.com/blog/db-hackathon-16-17-12-2016/>

[4] <https://www.kaggle.com/c/bike-sharing-demand/discussion>

[5] Facebook Research - Prophet:
<https://facebookincubator.github.io/prophet/>