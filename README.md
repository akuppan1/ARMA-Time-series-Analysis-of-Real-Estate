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
The data used are as follows and are all taken from https://data.cityofnewyork.us/
<br></br>
[**NYC Citywide Rolling Calendar Sales**](https://data.cityofnewyork.us/dataset/NYC-Citywide-Rolling-Calendar-Sales/usep-8jbt)
  - Please note upon writing this project, they updated and merged all the datasets into one giant one called "NYC Citywide Rolling Calendar Sales"
<br></br>
[**DOB Permit Issuance**](https://data.cityofnewyork.us/Housing-Development/DOB-Permit-Issuance/ipu4-2q9a)
<br></br>
[**Property Valuation and Assessment Data**](https://data.cityofnewyork.us/City-Government/Property-Valuation-and-Assessment-Data/yjxr-fw8i)
<br></br>
#### Challenges with the data:
1. "Property Valuation and Assessment Data" is 2GB in size. I could open it through jupyter notebook at the expense of 100% RAM utilization. If I tried to do anything with the data, the notebook crashed. I could not export any data out of this dataset. For future work, I would require more RAM in a computer or try running the notebook through a cloud VM with sufficient compute power. 
2. The Rolling Calendar sales datasets had nulls and Nans that had to be dealt with
3. The column names required to be changed to string type
4. I had to make qualitative decisions on which columns to keep and what data to ignore
5. Permit Issuance Data, although smaller, was still a large file and contained 3.75 million rows. I had clean and sort through the data for my recommendations. 
6. The rolling datasets came in .xls format
<br></br>

### 4. Cleaning and Prepping the data:
I had to make the following changes to the rolling datasets:
| Change | Reason |
| ----- | ----- |
|Skip rows when loading .xls file| This allowed for proper loading of the data. There were some rows at the top which pandas was not reading properly.|
|Take only a couple columns|I chose: 'TAX CLASS AT PRESENT','ZIP CODE', 'RESIDENTIAL UNITS', 'TOTAL UNITS', 'LAND SQUARE FEET', 'GROSS SQUARE FEET', 'YEAR BUILT', 'TAX CLASS AT TIME OF SALE', 'SALE PRICE', 'SALE DATE', because I thought they were the most significant information I can analyze|
|dropna()|There were nulls and nans in the dataset which I dropped. It was not much|
|reset_index()| Index had to be reset after dropping rows|
|Manhattan specific| I chose the bare minimum relevant data because there were ALOT of missing data in the other columns, espescially the square footage column. I chose: 'TAX CLASS AT PRESENT','ZIP CODE', 'YEAR BUILT','SALE PRICE', 'SALE DATE' |

### 5. ARMA Forecasting data - all NYC boroughs and observations
#### QUEENS Line Graph of Data vs 7-day rolling average
![image](https://user-images.githubusercontent.com/62908910/123027405-c7ce8180-d3ab-11eb-8947-b1dfd935e741.png)
##### Observation
    - The spikes in the data where the price goes to the millions or tens of millions is due to buildings being bought.
    - Other than that, the rest are residential properties well under a million in price
#### QUEENS Statsmodels decomposition 
![image](https://user-images.githubusercontent.com/62908910/123027581-0f550d80-d3ac-11eb-8e20-6aa7ae68fba9.png)
##### Observations: 
 - A large amount of sales happened between August 2020 and November 2020. 
 - Looks like there may be some seasonality every month
#### QUEENS Auto-Correlation and Partial Auto-Correlation
![image](https://user-images.githubusercontent.com/62908910/123027746-5d6a1100-d3ac-11eb-93c6-0fb9b0bc8619.png)
#### QUEENS ARMA forecast
![image](https://user-images.githubusercontent.com/62908910/123028145-f305a080-d3ac-11eb-8a33-6505d01a249e.png)
#### Error Analysis 
1. Length of Predictions :  90
2. Length of Test data :  90
3. RMSE :  584288.5171829152
4. Standard Error :  54947.9592801162

![image](https://user-images.githubusercontent.com/62908910/123028019-ba65c700-d3ac-11eb-9423-aee7721f2fe2.png)

##### Observation:
RMSE is not too high or low. Lower values of RMSE indicate better fit. I believe this is a good range and model fits well.
- RMSE is 584288.5171829152
- Standard error is 54947.9592801162
#### QUEENS Comparing predictions with fresh data from June 2021 dataset (4/1/2021 - 4/31/2021)
![image](https://user-images.githubusercontent.com/62908910/123028296-35c77880-d3ad-11eb-8d48-36e440d9b6a3.png)
![image](https://user-images.githubusercontent.com/62908910/123028422-4ed02980-d3ad-11eb-87ee-a842232e987d.png)
##### Observation
We see that the model has a decent fit with the test data of the last 30 days. However, the outlier will affect the error

1. Length of Predictions :  30
2. Length of Test data :  30
3. RMSE :  2263584.943411371
4. Standard Error :  54947.9592801162

![image](https://user-images.githubusercontent.com/62908910/123028597-9bb40000-d3ad-11eb-93f4-d47600d2391f.png)

##### Observation:
- We see that RMSE is much higher due to a large sale that occured near the end of the month. 
- Below I remove that outlier and re-check RMSE and Standard error

#### QUEENS Comparing predictions with fresh data - removed outlier
![image](https://user-images.githubusercontent.com/62908910/123028681-c736ea80-d3ad-11eb-8321-eb3d1c0a3e0f.png)

#### QUEENS Observations/Conclusions/Recommendations
    1. The point of this analysis was to see if the borough was good to invest in
    2. Based on the model:
        - We can enter to buy or exit to sell based on when the market will do well
    3. The borough sales look predictable
    4. There are unpredictable building sales which are very large amounts in the millions to tens of millions
    5. We can look at the top 10 building permit heavy locations further
#### MANHATTAN Line Graph of Data vs 7-day rolling average
![image](https://user-images.githubusercontent.com/62908910/123028851-154bee00-d3ae-11eb-82c7-ce5633942244.png)
##### Observation
    - The spikes in the data where the price goes to the millions or tens of millions is due to buildings being bought.
    - Other than that, the rest are residential properties well under a million in price
#### MANHATTAN Statsmodels decomposition   
![image](https://user-images.githubusercontent.com/62908910/123028914-36acda00-d3ae-11eb-85ff-b14f2c88b805.png)
##### Observations: 
 - A large amount of sales happened between August 2020 and November 2020. 
 - Looks like there may be some seasonality every month
#### MANHATTAN Auto-Correlation and Partial Auto-Correlation 
![image](https://user-images.githubusercontent.com/62908910/123029296-e71ade00-d3ae-11eb-8c72-228c42fc035d.png)
#### MANHATTAN ARMA forecast
![image](https://user-images.githubusercontent.com/62908910/123029382-074a9d00-d3af-11eb-99f1-9d48d79b2e79.png)
#### Error Analysis
![image](https://user-images.githubusercontent.com/62908910/123029443-1e898a80-d3af-11eb-8db5-f12b5983df95.png)
#### MANHATTAN Comparing predictions with fresh data from June 2021 dataset (4/1/2021 - 4/31/2021)
![ARMA_QUEENS_5](https://user-images.githubusercontent.com/62908910/123029553-4f69bf80-d3af-11eb-9853-1cdbd18da592.PNG)
![ARMA_QUEENS_6](https://user-images.githubusercontent.com/62908910/123029570-5690cd80-d3af-11eb-8f1b-fac5d502e66a.PNG)
##### Observation
- We see that the model looks like it fits well versus the test data of 4/1/2021 until 4/31/2021
- There is a spike in April 2021, probably a building got sold for millions
    - This will affect our RMSE
#### MANHATTAN Observations/Conclusions/Recommendations
    1. The point of this analysis was to see if the borough was good to invest in
    2. Based on the model:
        - We can enter to buy or exit to sell based on when the market will do well
    3. The borough sales look predictable
        - Espescially for Manhattan, there is a predictable fluctuation
    4. There are unpredictable building sales which are very large amounts in the millions to tens of millions
    5. We can look at the top 10 building permit heavy locations further

#### BRONX Line Graph of Data vs 7-day rolling average
![ARMA_BRONX_1](https://user-images.githubusercontent.com/62908910/123030024-17af4780-d3b0-11eb-9711-fdf104efcdb5.PNG)
#### BRONX Statsmodels decomposition 
![ARMA_BRONX_2](https://user-images.githubusercontent.com/62908910/123030050-2269dc80-d3b0-11eb-873c-bda8e8ebf5d5.PNG)
#### BRONX Auto-Correlation and Partial Auto-Correlation 
![ARMA_BRONX_3](https://user-images.githubusercontent.com/62908910/123030067-2990ea80-d3b0-11eb-8a7f-7d8a3ebaffa1.PNG)
#### BRONX ARMA forecast
![ARMA_BRONX_4](https://user-images.githubusercontent.com/62908910/123030082-2f86cb80-d3b0-11eb-9950-9b8f19f4ad75.PNG)
#### Error Analysis
![ARMA_BRONX_5](https://user-images.githubusercontent.com/62908910/123030092-344b7f80-d3b0-11eb-837e-b16660df603f.PNG)
#### BRONX Comparing predictions with fresh data from June 2021 dataset (4/1/2021 - 4/31/2021)
![ARMA_BRONX_6](https://user-images.githubusercontent.com/62908910/123030111-3ca3ba80-d3b0-11eb-8f83-bada57ac9cab.PNG)
![ARMA_BRONX_7](https://user-images.githubusercontent.com/62908910/123030128-43cac880-d3b0-11eb-8127-609f093e1796.PNG)
#### BRONX Observations/Conclusions/Recommendations
    1. The point of this analysis was to see if the borough was good to invest in
    2. Based on the model:
        - We can enter to buy or exit to sell based on when the market will do well
    3. The borough sales look predictable
        - There is predicable fluctuation in Bronx
    4. There are unpredictable building sales which are very large amounts in the millions to tens of millions
    5. We can look at the top 10 building permit heavy locations further
    
#### BROOKLYN Line Graph of Data vs 7-day rolling average
![ARMA_BROOKLYN_1](https://user-images.githubusercontent.com/62908910/123030584-06b30600-d3b1-11eb-9416-06d7b033f0f2.PNG)
#### BROOKLYN Statsmodels decomposition 
![ARMA_BROOKLYN_2](https://user-images.githubusercontent.com/62908910/123030597-0a468d00-d3b1-11eb-9b54-171b89366add.PNG)
#### BROOKLYN Auto-Correlation and Partial Auto-Correlation 
![ARMA_BROOKLYN_3](https://user-images.githubusercontent.com/62908910/123030605-0e72aa80-d3b1-11eb-9e9d-10396ab7c9a2.PNG)
#### BROOKLYN ARMA forecast
![ARMA_BROOKLYN_4](https://user-images.githubusercontent.com/62908910/123030622-13375e80-d3b1-11eb-8af8-27200f25e3d3.PNG)
#### Error Analysis
![ARMA_BROOKLYN_5](https://user-images.githubusercontent.com/62908910/123030633-17637c00-d3b1-11eb-89fc-bf87f949784e.PNG)
#### BROOKLYN Comparing predictions with fresh data from June 2021 dataset (4/1/2021 - 4/31/2021)
![ARMA_BROOKLYN_6](https://user-images.githubusercontent.com/62908910/123030646-1af70300-d3b1-11eb-81e7-aeaa69c8efb8.PNG)
![ARMA_BROOKLYN_7](https://user-images.githubusercontent.com/62908910/123030650-1cc0c680-d3b1-11eb-93a3-233439574798.PNG)
![ARMA_BROOKLYN_8](https://user-images.githubusercontent.com/62908910/123030663-20ece400-d3b1-11eb-9735-3a7602f5b6e4.PNG)
#### BROOKLYN Observations/Conclusions/Recommendations
    1. The point of this analysis was to see if the borough was good to invest in
    2. Based on the model:
        - We can enter to buy or exit to sell based on when the market will do well
    3. The borough sales look predictable
        - There is predicable fluctuation in Brooklyn
    4. We can look at the top 10 building permit heavy locations further





#### MANHATTAN Line Graph of Data vs 7-day rolling average
##### Observation
#### MANHATTAN Statsmodels decomposition   
##### Observations: 
#### MANHATTAN Auto-Correlation and Partial Auto-Correlation 
#### MANHATTAN ARMA forecast
#### Error Analysis
##### Observation:
#### MANHATTAN Comparing predictions with fresh data from June 2021 dataset (4/1/2021 - 4/31/2021)
##### Observation
#### MANHATTAN Observations/Conclusions/Recommendations













