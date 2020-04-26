## Instacart Market Basket Analysis 

### Problem: 
Predict products mixes that will be included in the next purchase order by users. That includes products a user will buy again, try for the first time, or add to their cart next during a session.

### Clients: 

**Instacart e-commerce Team**: By knowing user's purchase habits and what user would like to purchase in next order, Instacart can customize product pages to individuals by showing relevant products, which enables better conversion.
**Instacart users**: Users will save time on browsing the needed products, and may find new products of their interests faster, which enhance user experiences.
**Partner retailers**: Instacart can aggregate prediction of product purchases by store and send that to partner retailers for inventory planning purchases.

### Data:
* Kaggle competition (https://www.kaggle.com/c/instacart-market-basket-analysis/data)
* Kaggle API command: kaggle competitions download -c instacart-market-basket-analysis

### Outline:
* Data Cleansing/Wrangling: Understand the data structures, checking for missing values or invalid records
* Exploratory: Finding variables that may have influence on the re-ordering decision by users. Also identify factor that may changes product mix in orders over time. 
* Statistical Test: evaluate the statistical significance of the features identified in the exploratory stage on predicting target (i.e. re-order), plan on applying chi-square test to confirm features importance
* Machine Learning: this is a classification problem with a lot of text string category label. Apply CatBoost model, optimizing F1 score
* Draw recommendations/next steps

### Deliverable
* Codes, presentation slides to summary findings. All materials will be uploaded to Github folder below:
    https://github.com/sittingman/instacart_product_repurchase
