# Instacart Market Basket Analysis
 
Instacart is a eCommerce platform that connects shoppers to stores in respective areas and provides delivery in as fast as an hour. Company works with popular national and regional retailers such as Albertsons, ALDI, Costco, CVS, Kroger, Loblaw, Publix, Sam's Club, Sprouts, and Wegmans, among others. The Instacart marketplace offers more than 300 retailers and trusted local grocers that customers love. Beside door to door delivery, stock pick up option is also available. 

Company's revenue comes from delivery fee, range from $5.99 on orders over $35 to $7.99 on orders under $35.

### Objectives:
- Predict products mixes that will be included in the next purchase order by users. That includes products a user will buy again, try for the first time, or add to their cart next during a session.


### Clients & Impacts:

**Instacart e-commerce Team**: By knowing user's purchase habits and what user would like to purchase in next order, Instacart can customize product pages to individuals by showing relevant products, which enables better conversion.

**Instacart users**: Users will save time on browsing the needed products, and may find new products of their interests faster, which enhance user experiences.

**Partner retailers**: Instacart can aggregate prediction of product purchases by store and send that to partner retailers for inventory planning purchases.

### Data Source:
- [Kaggle](https://www.kaggle.com/c/instacart-market-basket-analysis/data) or [Instacart Website](https://www.instacart.com/datasets/grocery-shopping-2017)
- [Data Dictionary](https://gist.github.com/jeremystan/c3b39d947d9b88b3ccff3147dbcf6c6b)


### Outline of Approach

* Data Cleansing/Wrangling: Understand the data structures, checking for missing values or invalid records
    - findings: the data has been cleaned by data provider, no missing records, and data are in the right data types

* Exploratory: Finding variables that may have influence on the re-ordering decision by users. Also identify factor that may changes product mix in orders over time.
    - findings: 
        - Product categories has different re-order rate, among all beverage (water), diary eggs (milk, yogurt) and produce (fruits) are most popular items for re-ordering. Pantry items are the least likely to be re-ordered
        - Product categories mix does not vary significantly over time. Users will gradually trade off one item favor the other, but the re-order items appear to be similar to the previous order that user made
        - Users tend to purchase on day 0 and 1, from 9am to 4pm
        - Some products are being purchased on different days (e.g. Alcohol more likely to be purchased on Friday)
        - Most users who signed up will use the service for 3-4 times, on average 15 days between each other, then users attrition starts. Perhaps that's opportunity to improve user loyalty. 10% of the users become very loyal to the service and purchase among every week.
        - In each order, around 9-10 items are added to carts.

* Statistical Test: after discussing with mentor, there isn't an obvious statistical question that need to be addressed. We will skip this portion for this projects.

* Machine Learning: this is a classification problem. With the recommendation from mentor, we will adopt [PyCaret](https://pycaret.org/guide/) to perform cross validation on F1 score across multiple classification models. From the exploratory analysis, we will pursue the follow features (all at the user/product level):
    - number of times of reordering
    - is the product purchased within the last purchase
    - number of times a product had been purchased in the last 3 orders
    - % of order which a product had appear in user overall purchase history

### Summary of Findings

|Model | Kaggle Score (F1) |
|------| -------------|
|Naive| 0.31180|
|Light Gradient Boosting| 0.33601|
|Random Forest|0.35746 |
|Extreme Gradient Boosting|0.35970|


### Recommendations/next steps

Extreme Gradient Boosting is the recommended model. To maximize F1 score, we were trying to balance between Recall and Precision. From the training, most model only have Recall rate of ~0.5, which mean half of the items won't be classified correct. Hence, with the smaller size of test dataset, we have to lower the probability threshold of classifying 0 or 1 to mitigate the penalty from low recall. The final model has probability threshold set at 0.2.

To further improve on the F1 score, we can go back to identify new features. One candidate is the product reorder duration (i.e. how many day would a user to buy the same product again). We attempted to add that to the current model but encountered memory error (the combination of order x product x user is too big in shape). Further study is needed on how to create that feature.

Another option is to create two step models to engineer on maximizing F1 score. The mechanic will be more robust and involve more mathematical engineering on featured. We considered creating a prediction model of number of product to be purchased in the next order. Unfortunately, we did not find consistent purchase pattern at the individual user level. The output turn out to be inferior than one step model. More investigations will be needed.


[Final Presentation]