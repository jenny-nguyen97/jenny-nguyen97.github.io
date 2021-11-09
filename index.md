# Portfolio

---
## Predicting Bike Share Demand for Capital Bikeshare
### Using Machine Learning Algorithms (Linear Regression, Lasso Regression, CART, Random Forest), Holdout Sample, k-fold Cross Validation, Visualization with Tableau and R.

[![View on GitHub](https://img.shields.io/badge/GitHub-View_on_GitHub-blue?logo=GitHub)](https://github.com/jennynguyen-97/bikesharedemand) 

[Capital Bikeshare](https://www.capitalbikeshare.com) is metro DC's bikeshare service, with 4,500 bikes and 500+ stations across 7 jurisdictions: Washington, DC.; Arlington, VA; Alexandria, VA; Montgomery, MD; Prince George's County, MD; Fairfax County, VA; and the City of Falls Church, VA. Designed for quick trips with convenience in mind, it’s a fun and affordable way to get around.

![CaBi-how-it-works-hero2](https://user-images.githubusercontent.com/93355594/139561008-607a884b-785c-4fa6-a6c9-ace57caca446.jpg)

The [dataset](https://www.kaggle.com/c/bike-sharing-demand/overview) includes hourly rental data spanning two years of Capital Bikeshare. The training set is comprised of the first 19 days of each month, while the test set is the 20th to the end of the month. 

This project focuses on predicting the bike share demand based on available data such as date, time, holiday, working day, season, weather, temperature, humidity, and windspeed. By utilizing the information that is available ahead of time, bike-share firms can forecast the bike demand within the next week and thus reduce the uncertainty and ambiguity in their daily operations. Specifically, firms can supply and refill sufficient bikes at each docks, increase customer representative staff to assist customers on their cycling journeys, and implement an appropriate repair and mainteance schedule to minimize service disruption. Being able to predict demands ahead of time will enable bike-sharing programs to improve their operational efficiency, customer satisfaction, and business profits.

My recommended random forest model achieved an out-of-sample <img src="https://render.githubusercontent.com/render/math?math=R^2"> of 81.3% and RMSE of 79.62, outperformed three other machine learning algorithms: multiple linear regression, random forest, and classification and regression tree (CART). 

<p align="center"><img width="649" alt="Screen Shot 2021-11-07 at 11 56 21 PM" src="https://user-images.githubusercontent.com/93355594/140686479-6ea0ce3e-107c-4321-834b-d51a7259b1d0.png">

The random forest model is versatile, meaning that their is very little pre-processing required and the data does not need to be rescaled or transformed. Additionally, the model has low bias, moderate variance, and is robust to outliers and non-linear data. Needless to say, besides these advantages in utilizing random forest, there are several drawbacks that firms should be aware of regarding deployment. Firstly, the random forest model is not easily interpretable. It is a predictive modeling tool and not a descriptive tool, meaning if firms are looking for a description of the relationships in their bike demand data, other approaches would be better. Secondly, random forest can't extrapolate, it can only make a prediction that is an average of previously observed labels. Consequently, if there are operational disruptions caused by natural disasters such as winter storms, tornadoes, or pandemics, the random forest model will not make accurate predictions. Firms can mitigate these limitations by using the random forest model as a part of ensemble learning.
