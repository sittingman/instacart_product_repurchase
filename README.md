# Instacart Market Basket Analysis
 
[Instacart](https://www.instacart.com/) is an eCommerce platform that connects shoppers to stores in respective areas and provides delivery in as fast as an hour. The company works with popular national and regional retailers such as Albertsons, ALDI, Costco, CVS, Kroger, Loblaw, Publix, Sam's Club, Sprouts, and Wegmans, among others. The Instacart marketplace offers more than 300 retailers and trusted local grocers that customers love. Besides door to door delivery, store pick up option is also available. 

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

* [Data Cleansing/Wrangling](https://github.com/sittingman/instacart_product_repurchase/blob/master/1_data_obtain_wrangling.ipynb): Understand the data structures, checking for missing values or invalid records
    - findings: the data has been cleaned by the data provider, no missing records, and data are in the right data types

* [Exploratory](https://github.com/sittingman/instacart_product_repurchase/blob/master/2_data_exploratory.ipynb): Finding variables that may influence the re-ordering decision by users. Also, identify factors that may changes product mix in orders over time.
    - findings: 
        - Product categories have different re-order rates, among all beverage (water), diary eggs (milk, yogurt) and produce (fruits) are the most popular items for re-ordering. Pantry items are the least likely to be re-ordered
        - Product categories mix does not vary significantly over time. Users will gradually trade-off one item favor the other, but the re-order items appear to be similar to the previous order that user made
        - Users tend to purchase on day 0 and 1, from 9 am to 4 pm
        - Some products are being purchased on different days (e.g. Alcohol more likely to be purchased on Friday)
        - Most users who signed up will use the service for 3-4 times, on average 15 days between each other, then users attrition starts. Perhaps that's an opportunity to improve user loyalty. 10% of the users become very loyal to the service and purchase every week.
        - In each order, around 9-10 items are added to carts.

* Statistical Test: after discussing with my mentor, there isn't an obvious statistical question that is meaningful to be addressed. We will skip this portion for this project.

* [Machine Learning](https://github.com/sittingman/instacart_product_repurchase/blob/master/3_ML_features.ipynb): this is a classification problem. With the recommendation from my mentor, we will adopt [PyCaret](https://pycaret.org/guide/) to perform cross-validation on F1 score across multiple classification models. From the exploratory analysis, we will pursue the following features (all at the user/product level):
    - number of times of reordering
    - is the product purchased within the last purchase
    - number of times a product had been purchased in the last 3 orders
    - % of order which a product appeared in user overall purchase history
    - average days lag of products under certain departments were purchased by users

### Summary of Findings

|Model | Kaggle Score (F1) | Comments |
|------| -------------| ----- |
|Naive| 0.31180| using last purchase by user |
|Light Gradient Boosting| 0.36354 | with day lag feature |
|Gradient Boosting | 0.36244 | with day lag feature |
|Random Forest | 0.3625 | without days lag feature |
|Extreme Gradient Boosting |0.36049| without days lag feature |


Model Limitations - as the model mainly based on historical purchases of users, it lacks features to predict new product purchases by users. It also won't be able to predict well on product substitutes (for example, users may switch between different snacks that have different product id).

Days lag feature improved F1 score for Lightgbm and Gradient Boosting models but decrease F1 score for Random Forest and Extreme Gradient Boosting.

### Recommendations/next steps

Light Gradient Boosting has the best performance and we would recommend that as the final model. To maximize F1 score, we were trying to balance between Recall and Precision. From the training dataset, most models only have Recall rates of ~0.5 (i.e. half of the purchased items won't be captured by the model). With the smaller size of the test dataset, we have to lower the probability threshold of classifying 0 or 1 to mitigate the penalty from low recall. The final model has a probability threshold set ~ 0.2.

To further improve on the F1 score, we can consider creating two-step models to engineer on maximizing F1 score. The mechanic will be more robust and involve more mathematical engineering on features. One idea is to predict the number of products to be purchased in the next order and apply % ranking by user/product to come up with the orders. More investigations will be needed.

Obtaining features such as product prices or promotional events would be helpful since grocery is a price elastic shopping category.

[Final Presentation](https://github.com/sittingman/instacart_product_repurchase/blob/master/instacart_presentation.pdf)

[Capstone Report](https://github.com/sittingman/instacart_product_repurchase/blob/master/capstone_report_instacart.pdf)