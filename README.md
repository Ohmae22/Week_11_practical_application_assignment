## Introduction
In this project, we delve into a detailed dataset from Kaggle, initially containing data on 3 million used cars, trimmed down to 426K for this analysis. Our goal is to identify the primary factors that influence the pricing of used cars, providing essential insights to our client, a used car dealership.

## Dataset Overview
The dataset covers a range of features including make, model, year, mileage, and condition. The objective is to understand how these factors correlate with car prices and guide our client in making data-driven inventory decisions.

## Methodology
The approach includes several analytical steps:

Data Cleaning: Preparing the dataset by removing inconsistencies and handling missing data.
Exploratory Data Analysis (EDA): Visual examination and statistical analysis to uncover patterns.
Model Building: Regression analysis to statistically identify the most impactful factors.
Result Interpretation: Synthesizing results from statistical and visual analyses to formulate concrete recommendations.

## EDA & Findngs

<img width="548" alt="Screenshot 2024-05-06 at 11 26 27 PM" src="https://github.com/Ohmae22/Week_11_practical_application_assignment/assets/88304497/0ce72641-d46b-425d-8c68-7b7374ba932e">
<img width="647" alt="Screenshot 2024-05-06 at 11 26 35 PM" src="https://github.com/Ohmae22/Week_11_practical_application_assignment/assets/88304497/0e7e5bc5-eebf-4528-83d1-c97226b03d0c">
Visual Analysis Results

### Visual Analysis Results

- Price Distribution:
    - The histogram of car prices (filtered to exclude extreme outliers above $100,000) shows a right-skewed distribution. Most used cars are priced under $30,000, with a high frequency of cars in the lower price ranges.


- Correlation Heatmap:
    - Price vs. Year: There's a moderate positive correlation (0.42) between the car's manufacturing year and its price, indicating newer cars tend to be more expensive.
    - Price vs. Odometer: The correlation (-0.40) suggests that higher mileage (odometer readings) is associated with lower prices, as expected.
    - The correlation between year and odometer is also negative (-0.58), meaning newer cars tend to have lower mileage.


- Implications for Data Preparation
    - Given the skewness in price, transformations like logarithmic scaling might be needed to normalize the data for certain types of regression modeling.
    - It's important to clean and possibly impute missing values for year and odometer given their influence on the price.

## PCA Analysis

- Principal Component 1 (PC1):
    - It seems to be influenced positively by features related to the manufacturer and vehicle types like pickups. Negative contributions from odometer readings and car age suggest that PC1 might represent a combination of brand strength and newer, less used vehicles.
      
- Principal Component 2 (PC2):
    - This component has strong positive contributions from vehicle types, notably trucks, and negative contributions from types like sedans. This indicates that PC2 could be capturing vehicle utility or usage types—differentiating between more utility-focused vehicles (trucks) and common city cars (sedans).
      
- Principal Components 3-5 (PC3, PC4, PC5):
    - These components show varied influences from different manufacturer brands and vehicle types. For example, PC4 is positively affected by manufacturer_unknown, possibly indicating a segment of lesser-known brands or generic vehicle types in the dataset.

- Key Insights
  - Dominance of Vehicle Type and Manufacturer: The significant loadings from vehicle types (like pickups, sedans, and trucks) and specific manufacturers highlight their strong influence on the variability in car pricing or features in the dataset.
  - Impact of Odometer and Car Age: The consistent presence of odometer and car age across multiple components underscores their critical role in determining vehicle value, with older and more used cars differing in characteristics from newer, less-used cars.

<img width="1169" alt="Screenshot 2024-05-06 at 11 36 40 PM" src="https://github.com/Ohmae22/Week_11_practical_application_assignment/assets/88304497/6191d1a1-1bdc-4112-a732-7404b7fffc46">

## Modelling Result

- Initial regression models :
  - linear_reg = LinearRegression()
  - ridge_reg = Ridge(alpha=1.0)
  - lasso_reg = Lasso(alpha=0.1)

- with result of :
  - Linear Regression MSE: 172379196.93493977, RMSE: 13129.325837031382
  - Ridge Regression MSE: 172379196.61241466, RMSE: 13129.325824748757
  - Lasso Regression MSE: 172379096.3889644, RMSE: 13129.322007969962

- Perform the grid search to find the optimised params, with result of  :
  - Best Ridge Regression MSE: -172379168.14398727, RMSE: 13129.324740594517
  - Best Lasso Regression MSE: -172376586.60531443, RMSE: 13129.226428290223
  - Best parameters for Ridge: {'alpha': 100}
  - Best parameters for Lasso: {'alpha': 10}

- Fit the optimized Ridge and Lasso Regression model and get the coefficients:
<img width="604" alt="Screenshot 2024-05-06 at 11 55 53 PM" src="https://github.com/Ohmae22/Week_11_practical_application_assignment/assets/88304497/5d86a614-1893-42c5-84d8-c881b17395da">
  
  - Ridge Regression: The coefficients from Ridge regression are generally non-zero and slightly shrunken compared to ordinary least squares regression. Ridge tends to shrink the coefficients evenly, maintaining all the features but with reduced influence.
  - Lasso Regression: Lasso can shrink some coefficients to exactly zero, effectively performing feature selection. This is evident in your results, where some coefficients are zero, indicating that those features do not contribute to the model according to Lasso's solution path.
  - Both models have significant coefficients for features like car age, odometer, manufacturer type, and vehicle type, but the magnitudes vary.
  - Lasso has zeroed out several coefficients, suggesting those variables might be less significant in predicting the target variable, vehicle price.
  - Notable features with high coefficients in both models include factors directly linked to vehicle value depreciation, like age and mileage.
  - The coefficients related to specific vehicle types and manufacturers indicate varying degrees of influence on price.
 
<img width="1102" alt="Screenshot 2024-05-07 at 12 05 43 AM" src="https://github.com/Ohmae22/Week_11_practical_application_assignment/assets/88304497/be55a8e0-fa57-49da-8ce7-cfdc68783445">

## Observation:
- Odometer & Car Age: They have small weights in the first few PCs, which means they are not the most defining features in the dataset when it comes to price variance.
- Manufacturer: Certain manufacturers like 'Unknown', 'Acura', and 'Alfa-Romeo' have higher weighted loadings in some PCs, suggesting that they have a significant impact on how the cars are grouped by price variance.
- Type of Car: Features like 'coupe', 'hatchback', and 'pickup' have more considerable weight in some PCs, indicating these vehicle types are influential in the variance captured by those PCs. For instance, PC1 heavily weighs 'other', 'pickup', and 'hatchback', suggesting that these types of cars might have a distinct pattern in pricing compared to other types.


## What This Means for the Dealership:
- Diverse Inventory: Since different PCs capture different features, it implies the used car market values a diverse range of features. The dealership should maintain a varied inventory to appeal to different consumer preferences.
- Focus on Car Type and Make: Car types like pickups, hatchbacks, and manufacturers might have distinct price points that can be targeted. If pickups have a high weight in PC1, which captures the most variance, it might be indicative of a niche with specific pricing dynamics.
- Pricing Strategy: The dealership might consider their pricing strategy for different car types and makes/models based on the weighted loadings. For instance, if the 'sedan' type has a strong negative weight in a principal component that captures a lot of variance, it might suggest that sedans are varied in price, and there could be an opportunity to compete on price.

## Concluding Thoughts
- The insights derived from the Lasso regression provide a robust foundation for strategic decision-making. By aligning inventory and pricing strategies with these data-driven insights, dealership can better meet market demand and improve sales outcomes.
