# Datathon 2022 - UPC - Supply Chain Resilience
2022-11-12

## Challenge
Understand main root-cause affecting supply chain deliveries and
predict product delays based on historical inbound/outbound orders of the company.
Providing visibility of supply chain resiliency to maximize customer service levels

*What is it expected from the technical challenge?*
- Provide data insights based on descriptive analytics of historical data
- Regression model to predict likelihood of order delay


## Evaluation Criteria
*What will be measured?*

- **Business case** presentation outlining descriptive insights of main drivers’ toward order delays.
How those insights can be translated to business actions and value proposition.


- **Predictive model** to get likelihood of order delay.
ROC Curve (AUC) evaluation metric for given test data set.
A [Kaggle Competition](https://www.kaggle.com/t/187d01ee269e404ca7267e8caddf0eae) have been created for teams to submit and test their model results.
Please sign-up to the competition and follow the instructions


## Data

### orders.csv
Transactional historical data of the company supply chain inbound/outbound shipments
- `order_id` *(string)*: unique identifier of transactional order from port inbound to final destination. Primary key of data set.
- `origin_port` *(string)*: location of port where order imports arrives.
- `3pl` *(string)*: Third-party logistic company id used for distribution, warehousing, and fulfillment services.
- `customs_procedure` *(string)*: Type of procedure to be used in the imports legal process
- `logistic_hub` *(string)*: city name of company logistic hub address. Intermediate step between *origin_port* and *customer*
- `customer` *(string)*: city name of customer destination address
- `product_id` *(string)*: unique identifier of final product
- `units` *(integer)*: order size quantity
- `late_order` *(boolean)*: target variable, if 1 the order_id have been tagged as a late delivery, 0 is on-time


### product_attributes.csv
Master data of product unit weight
- `product_id` *(string)*: unique identifier of final product
- `weight` *(integer)*: product weight per 1 unit in grams
- `material_handling` *(integer)*: Classification id for product safety risk and risk of damage e.g. fragile, toxic, flammable.


### cities_data.csv
Geographic coordinates of cities involve in the supply chain.
Including distance between pair of cities
- `city_from_name` *(string)*: City of location starting point
- `city_to_name` *(string)*: City of destination location
- `city_from_coord` *(tuple)*: Coordinates in (latitude, longitude) of city_from
- `city_to_coord` *(tuple)*: Coordinates in (latitude, longitude) of city_to
- `distance` *(float)*: kilometers between the pair cities


### test.csv
Same as `orders.csv` but variable `late_order` has been truncated.
This is the target variable



## Starter Script
[link to notebook]
Example of a python code that reads all data sources and
fit a naive average model to predict likelihood of an order arriving late.
Notebook generates a submission file, based on the test data set,
as an example of the expected file be uploaded to [Kaggle Competition](https://www.kaggle.com/t/187d01ee269e404ca7267e8caddf0eae)


## FAQ
1. **What is the phisical flow path of the supply chain?**
Inbound orders arrives at first stage at the `origin_port`.
The order import is processed following a custom proceduce and
it is sent to the `logistic_hub`. Later on, the order is transported to the final destionation of the `customer`.


2. **Is the `product_id` in `product_attributes.csv` and `orders.csv` a 1:1 relationship?**
Yes, `product_attributes.csv` is a master data file,
having `product_id` as primary key and attibutes `weight` and `material_handling` fix value for each product.


3. **Is it the `material_handling` a ranking category?**
No, data has been annonimized, values represent a category type not a ranking
i.e value=3 does not mean it is more complex/riskier to manipulate the product compare to value=1


4. **Are all cities in `orders.csv` available in `cities_data.csv`?**
Yes, all cities represented on columns `origin_port`, `logistic_hub` and `customer` are shown in the file.
Notice that city could be either on `city_from_name` or `city_to_name`.
File contains a unique pair combination between all cities and
it is the same distance from *city_a -> city_b* as *city_b -> city_a*


5. **What does it mean to have a empty value in `logistic_hub` column for a specific order?**
Orders with missing info in `logistic_hub` are "direct shipments", going for port to customer directly.


6. **What should be the format of my submission file?**
Starter script shows an example of expected rows and columns of file.
Please, make sure submission file have all and unique `order_id` as in the test set and
target variable `late_order` is a numerical value between 0 and 1.
File must be comma-separate and does not include 'index', only columns `order_id` and `late_order`
