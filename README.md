# Instacart Market Basket Analysis
 
Instacart is an eCommerce platform that connects shoppers to stores in respective areas and provides delivery in as fast as an hour. The company works with popular national and regional retailers such as Albertsons, ALDI, Costco, CVS, Kroger, Loblaw, Publix, Sam's Club, Sprouts, and Wegmans, among others. The Instacart marketplace offers more than 300 retailers and trusted local grocers that customers love. Besides door to door delivery, store pick up option is also available. 

Company's revenue comes from a delivery fee, range from $5.99 on orders over $35 to $7.99 on orders under $35.

### Objectives:
- Predict product mixes that will be included in the next purchase order by users. That includes products a user will buy again, try for the first time, or add to their cart next during a session.


### Clients & Impacts:

**Instacart e-commerce Team**: By knowing user's purchase habits and what users would like to purchase in the next order, Instacart can customize product pages to individuals by showing relevant products, which enables better conversion.

**Instacart users**: Users will save time on browsing the needed products, and may find new products of their interests faster, which enhance user experiences.

**Partner retailers**: Instacart can aggregate prediction of product purchases by store and send that to partner retailers for inventory planning purchases.

### Data Source:
- [Kaggle](https://www.kaggle.com/c/instacart-market-basket-analysis/data) or [Instacart Website](https://www.instacart.com/datasets/grocery-shopping-2017)
- [Data Dictionary](https://gist.github.com/jeremystan/c3b39d947d9b88b3ccff3147dbcf6c6b)


### Outline of Approach

* Data Cleansing/Wrangling: Understand the data structures, checking for missing values or invalid records
    - findings: the data has been cleaned by the data provider, no missing records, and data are in the right data types

* Exploratory: Finding variables that may influence the re-ordering decision by users. Also, identify factors that may changes product mix in orders over time.
    - findings: 
        - Product categories have different re-order rates, among all beverage (water), diary eggs (milk, yogurt) and produce (fruits) are the most popular items for re-ordering. Pantry items are the least likely to be re-ordered
        - Product categories mix does not vary significantly over time. Users will gradually trade-off one item favor the other, but the re-order items appear to be similar to the previous order that user made
        - Users tend to purchase on day 0 and 1, from 9 am to 4 pm
        - Some products are being purchased on different days (e.g. Alcohol more likely to be purchased on Friday)
        - Most users who signed up will use the service for 3-4 times, on average 15 days between each other, then users attrition starts. Perhaps that's an opportunity to improve user loyalty. 10% of the users become very loyal to the service and purchase every week.
        - In each order, around 9-10 items are added to carts.

* Statistical Test: after discussing with my mentor, there isn't an obvious statistical question that is meaningful to be addressed. We will skip this portion for this project.

* Machine Learning: this is a classification problem. With the recommendation from my mentor, we will adopt [PyCaret](https://pycaret.org/guide/) to perform cross-validation on F1 score across multiple classification models. From the exploratory analysis, we will pursue the following features (all at the user/product level):
    - number of times of reordering
    - is the product purchased within the last purchase
    - number of times a product had been purchased in the last 3 orders
    - % of order which a product appeared in user overall purchase history

### Summary of Findings

|Model | Kaggle Score (F1) |
|------| -------------|
|Naive| 0.31180|
|Light Gradient Boosting| 0.33601|
|Random Forest (base) |0.35746 |
|Random Forest (tuned) | 0.36164 |
|Extreme Gradient Boosting (base)|0.35970|
|Extreme Gradient Boosting (tuned)|0.36049|


Model Limitations - as the model mainly based on historical purchases of users, it lacks features to predict new product purchases by users. It also won't be able to predict well on product substitutes (for example, users may switch between different snacks that have different product id).

### Recommendations/next steps

Both Extreme Gradient Boosting and Random Forecast have a similar level of performance and would recommend both as the final model. To maximize F1 score, we were trying to balance between Recall and Precision. From the training dataset, most models only have Recall rates of ~0.5 (i.e. half of the items won't be classified correctly). With the smaller size of test dataset, we have to lower the probability threshold of classifying 0 or 1 to mitigate the penalty from low recall. The final model has a probability threshold set ~ 0.2.

To further improve on the F1 score, we can go back to identify new features. One candidate is the product reorder duration (i.e. how many days would a user buy the same product again). We attempted to add that to the current model but encountered memory error (the combination of order x product x user is too big in shape). Further study is needed on how to create that feature.

Another option is to create two-step models to engineer on maximizing F1 score. The mechanic will be more robust and involve more mathematical engineering on featured. We considered creating a model to predict the number of products to be purchased in the next order. Unfortunately, we did not find a consistent purchase pattern in terms of the number of products at the individual user level. More investigations will be needed.


[Final Presentation]