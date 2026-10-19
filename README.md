# IKEA Price and Discount Prediction

What predicts furniture prices, and which product attributes are associated with discounts?


## Overview

This project applied regression and classification models to a dataset of 3,694 IKEA products, using physical dimensions, product category, designer, online availability and colour options to investigate two questions: can we predict what a product will cost and whether it will be discounted?


## Methodology

1. **Exploratory Data Analysis:** distribution, correlation, class-imbalance analysis.
2. **Data Preparation:** applied log transformation to address right-skewed distributions, encoded categorical variables, scaled features, grouped infrequent designer categories and engineered additional features.
3. **Price Prediction:** baseline OLS, log-linear OLS, Ridge, Lasso and Elastic Net (tuned with 10-fold cross-validation) to predict price and log-price.
4. **Discount Classification:** logistic, Ridge, Lasso and Elastic Net models tuned via 5-fold cross-validation to predict whether an item is discounted. The default 0.5 threshold was compared with the Youden’s J threshold (0.175, derived from training data) and evaluated on sensitivity, specificity, precision, balanced accuracy and AUC.  


## Findings

### What predicts price? 

- **Size is the strongest predictor:** larger furniture consistently costs more and this relationship is not linear, and log-transforming the dimensions improves model performance. 
- **Category is important:** for items of the same size, children’s furniture is around 55% cheaper than tables and desks, and nursery furniture, outdoor furniture, bookcases and shelving units, and TV and media furniture are all between 40 and 50% cheaper. 
- **Designers often add a price premium:** items from all significant designers are associated with price premiums of up to 158% relative to IKEA's in-house design team.
- **Model Comparison:** The all-features linear model (RMSE = 800, MAE = 509, R² = 0.693) is recommended for simplicity and model fit when it is important to minimise high-cost errors, although the log-linear model (RMSE = 879, MAE = 469, R² = 0.645) is preferable if average overall accuracy is a priority. Regularisation added no meaningful benefit. 
- **Limitation:** the errors are large relative to the median price of SR 545, so the model captures patterns, not individual prices. 


### What predicts discounts? 

- **Discount is not about item price:** Once category, designer and dimensions are controlled for, a product’s price is not a significant predictor of whether it goes on sale. 
- **Designer is an important indicator:** Several designers are associated with up to seven times the odds of receiving a discount relative to IKEA of Sweden’s own designs (the reference designer). Among the 23 examined, only one was associated with lower odds of discount.  
- **Category effects were limited:** Only three categories significantly differ from the reference category of tables and desks: chests of drawers & drawer units (2.9x the odds), beds (2x) and wardrobes (2x). 
- **Threshold choice matters:** At the default 0.5 threshold the model only catches 15% of discounted items. Lowering the threshold to 0.175 raised sensitivity to 68% but precision dropped to 30%. The preferred threshold depends on whether missing a discount or raising a false alarm is more costly. 
- **Regularisation made no significant difference:** All four logistic models achieved an AUC of approximately 0.75 and paired DeLong tests found no statistically significant differences between their AUCs. The baseline logistic model is therefore recommended for its simplicity and interpretability. 


## Limitations and Future Research

- Results rely on a single train/test split.
- There are no sales volumes or inventory data so whether a discount is driven by stock clearance cannot be tested. 
- Discount status is binary so the severity of a markdown is not captured. 
- Additional data on production and distribution costs, including material, labour and transport, could improve the models. 


## Key Packages

`caret` · `glmnet` · `pROC` · `DataExplorer` · `mctest` · `ggplot2` · `tidyverse`


## Repository Structure

```
ikea-price-discount-modelling/
├── ikea_price_discount_modelling.qmd
├── data/
│   └── ikea_data.csv
├── README.md
└── LICENSE
```


## License

This project is licensed under the MIT License. See LICENSE for details.

