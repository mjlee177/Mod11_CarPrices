# Mod11_CarPrices
Analysis of kaggle data on 3 million used cars.  Analyzed for used car dealerships.

Like to github repository: https://github.com/mjlee177/Mod11_CarPrices.git

Summary of Findings:
The data was found to be very messy.  After filtering was complete, only 36% of the original data was kept.  The best R2 score achieved was 0.35, which is NOT acceptable for successful modeling.

Evaluation:
Time and computing power were our main constraints for analyzing the used car dataset. If time permitted, we would have been able to 
- try more iterations of which features to keep
- spent more time on filtering (quartile filtering could be tried instead of the eye-test)
- imputed missing data
- run individual models for features that had too many unique entries for OneHotEncoder (region, manufacturer, and model are all impactful features that were impractical for OneHotEncoder).

If we had more computing power, we could have
- run RFE to assist with feature selection
- run more hyperparameters in GridSearchCV.

The data itself was very messy, requiring a lot of duplicate rows to be removed, features to be removed due to too many NA values, and pricing data to be filtered. At the end, we were left with only 36% of the original data. The methods and discipline in collecting the data must be vastly improved.

The final dataset kept features: years_old, fuel, odometer, title_status, and transmission. OneHotEncoder was used on categorical features and then linear regression, ridge, lasso, and polynomial features were run on all of the data. Ridge and Lasso resulted in approximately the same errors as linear regression, so it doesn't matter which is selected in the model. Hyperparameters were optimized using GridSearchCV, which resulted in ridge alpha = 0.1 and polynomial of 2.  We observed that the order of importance (coefficient magnitudes) are ordered as title_status, fuel, years_old, odometer, then transmission.

The best R2 result we got was 0.35, meaning our best model explains 35% of the variance in price. This is NOT a good score and the model needs more work in order to be confidently implemented.

Deployment:
It is NOT recommended that we use the model as-is. The model is currently at an R2 score of 35, which will not have a high success rate at predicting the price of a used car.

It is recommended that instead a model is created for each dealership based on their region and the model of car sold. These features had too many unique entries for OneHotEncoder but are undoubtedly two of the top features that would impact price. To execute this, we should ask the dealership for these two pieces of information and then run the above GridSearchCV on the filtered data.

For example, if San Francisco Toyota wants to know how much to charge for a 2020 Toyota Camry, we would first filter to only keep region='SF bay area'. We can then filter out anything that contains 'Camry' in the 'model' column. From there we can run the same code that was in this analysis - delete duplicates, delete na, delete outliers, then run GridsearchCV on a pipeline with OneHotEncoder, StandardScaler, PolynomailFeatures, Ridge (or Lasso). We'd then check for an acceptable R2 score. If all looks good, we can run .predict on any used car Toyota wants to sell.
