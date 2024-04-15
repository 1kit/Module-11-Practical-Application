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


#### NOTE
The number of features are very high. But since our ultimage goal is to understand what factors/features drive up or down the cost,
it is better not to perform a PCA. This is because PCA would remove our ability to explain how the target moves with respect to the input features. It makes inference harder.

## Modeling

Built the following two models
1. A simple Linear Regression model
2. A Ridge model and a corresponding Grid search to find out optimal hyper parameters

#### NOTE
I decided to avoid Polynomial Features as the dimension was high already

#### Scores for Linear Regression

```
 MSE Train = 119918160.71220581
 MSE Test  = 120247164.59735604
 R2 Train  = 0.37970424618253873
 R2 Train  = 0.3782493672729581
```

#### Scores for Ridge Regression

```
 MSE Train = 119938066.0087205
 MSE Test  = 120119560.2995357
 R2 Train  = 0.3796012828712837
 R2 Train  = 0.37890915873809705
```

The alpha value of '1.0' was the best in the GridSearch

This indicates that the fit is not that great for Ridge either. I think the quality of the input data may not be that great.

# Evaluation

To evaluate the model and to infer from it, I did the following

1. Look at Permutation Importance of the various features

    The following features most contribute to the fitted model (in this order)

```
        year         0.177 +/-  0.001
        type_sedan   0.058 +/-  0.000
        type_pickup  0.048 +/-  0.000
        transmission_other 0.032 +/-  0.000
        type_truck   0.021 +/-  0.000
        transmission_automatic 0.020 +/-  0.000
        odometer     0.019 +/-  0.000
```

2. Examine the coeficients

####Observation
The coefficients with the highest negative value is the following. This indicates decrease in price for one unit change in the predictor, while holding others constant


```
    Manufacturer ferrari
    Manufacturer Harley Davidson
    Manufacturer Fiat
    Manufaccturer Saturn
```

The coefficients indicate that the following predictors drive price up (in the order listed below)

```
    Manufacturer Tesla
    Manufacturer Datsun
    Manufacturer Porsche
    Manufacturer Rover
    Type Pickup
```

# Deployment and Final Recommendation

#### Here are some high level observations

- Maximum sales happen for the lower priced cars. The 75th percentile is at $25990
- The average odometer reading is about 98000 miles
- There is more sales on newer cars. This may be due to higher inventory of such cars
- There are no clear patterns for regions. Certain regions have more sale than others.
- Ford, Toyota and chevrolet are popular manufacturers
- Gas cars are more popular than other types
- Lower odometer cars are more preferred
- Automatic transmission seems to be the preferred choice
- Sedans, SUV, followed by pickups have the highest sale over other types.
- Among the newer cars the 'other' type appears to be the dominant type.
- Among the newer cars, the pickups and trucks seem be higher priced
- Manual transmission seems be the majority of the sales
- Manual transmission has a lower average price than Automatic transmission at all times.
- The median price of newer cars are in general higher. There is some anamoly for 2022 year cars.
- There is a larger variation in the price for newer cars
- Van and pickup pricess are higher in general than other types

#### The features the most contribute to the price changing are
- Year of the car
- Sedan type
- Pickup type
- Other transmission type
- Truck type
- Odometer value

#### The following Manufactures contribute to maximum lowering of the price (in the order listed)

  * Manufacturer ferrari
  * Manufacturer Harley Davidson
  * Manufacturer Fiat
  * Manufacturer Saturn

#### The following predictors drive price up (in the order listed below)

  * Manufacturer Tesla
  * Manufacturer Datsun
  * Manufacturer Porsche
  * Manufacturer Rover
  * Type Pickup

#### Recommendation: 
1. Stocking Ferrri, Harley, Fiat or Saturn may not be very good for the dealership
2. Stocking Tesla, Datsun, Porsche, rover and any Pickup type cars in general is good for the dealership as it drives price up and could make be more profitable.
3. Automatic cars are better for sales in general
