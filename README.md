<p align="center">
<img src = images/used-car.jpg width = 50%/>
</p>

# Business
According to [statista](https://www.statista.com/topics/9879/used-vehicles-in-the-united-states/#topicOverview) there is a USD 138.1 bn used car dealer market size in the US. The used car vehicle sales amount to USD 38.6M with an average selling price of \$30.7K per vehicle. Effective pricing and stocking strategies can help any used car dealership to sell effectively and maximize profits.
The goal for this project is to use the industry standard CRISP-DM methodology to analyze and model used car sales data and make predictions on car prices. The other goal is also to use those models to make clear recommendations to used car dealership about what consumers value in a used car and what factors drive price up or down.

# Data
The input dataset contains user car sales information and has the following features

 1   region
 2   price
 3   year
 4   manufacturer
 5   model
 6   condition
 7   cylinders
 8   fuel
 9   odometer
 10  title_status
 11  transmission
 12  VIN
 13  drive
 14  size
 15  type
 16  paint_color
 17  state

Here I would like to use supervised machine learning methods to be able to model the price of used cars and attempt to evaluate the features that most contributes to the price of the car and make recommendations

## Analyze the data

Here is a plot of the price in each region
<p align="center">
<img src = images/price-scatter.png width = 50%/>
</p>

This clearly shows outlier in the data. Eliminating the outliers in the price and analyzing further

#### Observation:
- Maximum sales happens for the lower priced cars. The 75th percentile is at $25990
- The average odometer reading is about 98000 miles

Lets analyze the histograms of the important features
<p align="center">
<img src = images/price-hist.png width = 100%/>
<img src = images/year-hist.png width = 100%/>
<img src = images/region-hist.png width = 100%/>
<img src = images/manu-hist.png width = 100%/>
<img src = images/fuel-hist.png width = 100%/>
<img src = images/odo-hist.png width = 100%/>
<img src = images/tran-hist.png width = 100%/>
<img src = images/type-hist.png width = 100%/>
</p>

#### Observation
- There is more sales on newer cars. This may be due to higher inventory of such cars
- There are no clear patterns for regions. Certain regions have more sale than others.
- Ford, Toyota and chevrolet are popular manufacturers
- Gas cars are more popular than other types
- Lower odometer cars are more preferred
- Automatic transmission seems to be the preferred choice
- Sedans, SUV, followed by pickups have the highest sale over other types.

Lets try to further analyze the type and transmission spread for the price

<p float="left">
 <img src = images/date-price-type-joint.png width = 500/>
 <img src = images/date-price-type-joint.png width = 500/>
</p>

#### Observation
- Among the newer cars the SUV and sedan type appears to be the dominant type.
- Among the newer cars, the pickups and trucks seem be higher priced
- Automatic transmission seems be the majority of the sales
- Manual and Automatic transmission has a lower average price than other transmission

## Recent trends

<p align="center">
<img src = images/price-box.png width = 100%/>
<img src = images/transmission-price-box.png width = 100%/>
<img src = images/type-price-box.png width = 100%/>
<img src = images/manu-price-box.png width = 100%/>
</p>

## Data Preparation
The following transformations had to be done on the data

- The following features were dropped and not included in the modeling
  * id
  * VIN
  * state
  * region
- About 40% of cylinders data dn 41% of condition data was missing. Hence drop them
- About 71% of size data was missing. Hence drop it.
- Clean up rows that did not have data against it for remaining features

Used one hot encoding for the following categorical features

  * manufacturer
  * fuel
  * transmission
  * typ


#### The number of features are very high. But since our ultimage goal is to understand what factors/features drive up or down the cost, it is better not to perform a PCA. This is because PCA would remove our ability to explain how the target moves with respect to the input features. It makes inference harder.

## Modeling

Built the following two models
1. A simple Linear Regression model
2. A Ridge model and a corresponding Grid search to find out optimal hyper parameters

Note: I decided to avoid Polynomial Features as the dimension was high already
