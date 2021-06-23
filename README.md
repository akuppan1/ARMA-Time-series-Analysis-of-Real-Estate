# NYC Real Estate Analysis during COVID and Recommendations
![](https://lp-cms-production.imgix.net/2021-05/GettyRF_494607942.jpg?auto=format&fit=crop&sharp=10&vib=20&ixlib=react-8.6.4&w=850)
<br></br>
from: https://lp-cms-production.imgix.net/2021-05/GettyRF_494607942.jpg?auto=format&fit=crop&sharp=10&vib=20&ixlib=react-8.6.4&w=850
## README Table of Contents
1. Project Goals and Abstract
2. Project Summary
3. The Data used and Challenges with it
4. Cleaning and Prepping the Data
5. ARMA Forecasting data - all NYC boroughs and observations
6. LinkedIn GreyKite forecasating of data - all NYC boroughs and observations
7. LSTM Forecasting data - all NYC boroughs and observations
8. Permit Data Analysis and decisions
9. Observations/Recommendations
10. Future Work   
<br></br>

### 1. Project Goals and Abstract
The project started off initially as a means to understand for myself the real estate market before and after the peaks of the COVID pandemic within the city. Seeing no people on the streets was an eerie sight. The data I took was from the open data repository maintained by the city. NYC tracks all real estate transactions and maintained rolling datasets per borough about the sales that occured in each borough. At the time of me doing this project, I had data from 04/01/2020 up until 3/31/2021. The datasets were huge. The largest that I tried to use was 9.85 million rows and was making short work of my computer. The goals then solidified into the following:
<br></br>
  - Analyze the boroughs to see which ones are trending and in which direction
  - Forecast the data to see if the prices of the properties will go up or down
  - Create 10 recommended zip codes per borough that a potential investor can look into as a lucrative area to buy property
<br></br>
The last bullet point is important. Real estate transactions are still going on in the city and I want to know where there are potential hotspots to purchase property in. 
Upon analysis of the datasets of the five boroughs and permit data. I made recommendations on which zip codes an investor should take a look at in depth if they are looking to purchase property.
<br></br>
### 2. Project Summary
First, I went to the NYC Open Data Website and perused their volumes of records that they keep on every moving part in the city. Real Estate is no exception. They had a dataset that is continuously growing of all transactions that went back into the late 2000s. Sadly, I could not use this dataset because my hardware did not have the RAM to do so. 
I then downloaded the rolling data for all 5 boroughs and forecasted the data for the next 30 days with ARMA models, LinkedIN Greykite Forecasting software, and LSTM (Long-Short Term Memory) Neural Network using Keras. Upon analysis, the markets looked stable with the exception of one or two trending slightly downward. Within each of the datasets, there were massive spikes in sale price due to buildings being sold but they were being sold unpredictably. After analyzing the boroughs and seeing their trends and forecast, I looked at the permit data for each borough. Basically, contractors/architects/owners have to file for different job types based on what kind of renovation they want to do to their property. The job types I focused on were Alteration Type 1, Alteration Type 2, New Building, and Demolition. Respectively, the job type acronyms are: A1, A2, NB, and DM.
I based my zip code recommendations on the value_counts of the sum of these job types in each zip code. I was able to get recommendations for each borough this way. 
<br></br>
### 3. The Data Used and Challenges
