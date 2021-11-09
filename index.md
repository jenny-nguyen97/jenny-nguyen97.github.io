# Portfolio

---
## Predicting Bike Share Demand for Capital Bikeshare
### Using Machine Learning Algorithms (Linear Regression, Lasso Regression, CART, Random Forest), Holdout Sample, k-Fold Cross Validation, Visualization with Tableau and R.

[![View on GitHub](https://img.shields.io/badge/GitHub-View_on_GitHub-blue?logo=GitHub)](https://github.com/jennynguyen-97/bikesharedemand) 

[Capital Bikeshare](https://www.capitalbikeshare.com) is metro DC's bikeshare service, with 4,500 bikes and 500+ stations across 7 jurisdictions: Washington, DC.; Arlington, VA; Alexandria, VA; Montgomery, MD; Prince George's County, MD; Fairfax County, VA; and the City of Falls Church, VA. Designed for quick trips with convenience in mind, it’s a fun and affordable way to get around.

![CaBi-how-it-works-hero2](https://user-images.githubusercontent.com/93355594/139561008-607a884b-785c-4fa6-a6c9-ace57caca446.jpg)

The [dataset](https://www.kaggle.com/c/bike-sharing-demand/overview) includes hourly rental data spanning two years of Capital Bikeshare. The training set is comprised of the first 19 days of each month, while the test set is the 20th to the end of the month. 

### Business Problem
Every day, many people use bike sharing to get around, whether for work or leisure. Acknowledging the importance of convenience, availability, and affordability in maintaining a successful bike-share program, I aim to build a predictive model that can closely and accurately predict the actual hourly demand of bikes for each day. If bike-share programs don't have enough bikes available, riders will lower their expectations, abandon bike sharing, and switch to other transportation methods. When sufficient numbers of functioning bikes are on the ground in trafficked areas, riders will trust the bike share as a reliable means of transportation.

This project focuses on predicting the bike share demand based on available data such as date, time, holiday, working day, season, weather, temperature, humidity, and windspeed. 

### Modeling 
My recommended random forest model achieved an out-of-sample <img src="https://render.githubusercontent.com/render/math?math=R^2"> of 81.3% and RMSE of 79.62, outperformed three other machine learning algorithms: multiple linear regression, random forest, and classification and regression tree (CART). 

<img width="649" alt="Screen Shot 2021-11-07 at 11 56 21 PM" src="https://user-images.githubusercontent.com/93355594/140686479-6ea0ce3e-107c-4321-834b-d51a7259b1d0.png">

The random forest model is versatile, meaning that their is very little pre-processing required and the data does not need to be rescaled or transformed. Additionally, the model has low bias, moderate variance, and is robust to outliers and non-linear data. Needless to say, besides these advantages in utilizing random forest, there are several drawbacks that firms should be aware of regarding deployment. Firstly, the random forest model is not easily interpretable. It is a predictive modeling tool and not a descriptive tool, meaning if firms are looking for a description of the relationships in their bike demand data, other approaches would be better. Secondly, random forest can't extrapolate, it can only make a prediction that is an average of previously observed labels. Consequently, if there are operational disruptions caused by natural disasters such as winter storms, tornadoes, or pandemics, the random forest model will not make accurate predictions. Firms can mitigate these limitations by using the random forest model as a part of ensemble learning.

### Conclusion
The final result shows that demand for bikes is higher during weekdays compared to weekends and during timeblocks for morning commutes (7 a.m. - 9 a.m.) and afternoon commutes (16 p.m. - 19 p.m.). Consequently, firms should develop strategies to not only provide sufficient bikes to stations during busy hours, but also reallocate bikes to appropriate stations that match the commute patterns.

![Hours](https://user-images.githubusercontent.com/93355594/140855632-73d2edc6-9669-4c7d-948d-8155d4e09be2.png)

Season and weather patterns also play a role in bike demand. Specifically, demand is higher during summer and fall with total rentals for each season approximate 916,000 and 1,050,000 rents, respectively. Additionally, higher demand often happens mostly when the weather is clear and partly cloudy.

![Sheet 1](https://user-images.githubusercontent.com/93355594/140856152-a2bfb026-d612-4c04-b778-c96f3c46cc4f.png)

![Weather](https://user-images.githubusercontent.com/93355594/140856156-c665f67a-364d-4289-be78-95096db915ae.png)

By utilizing the information that is available ahead of time, bike-share firms can forecast the bike demand within the next week and thus reduce the uncertainty and ambiguity in their daily operations. Specifically, firms can supply and refill sufficient bikes at each docks, increase customer representative staff to assist customers on their cycling journeys, and implement an appropriate repair and mainteance schedule to minimize service disruption. Being able to predict demands ahead of time will enable bike-sharing programs to improve their operational efficiency, customer satisfaction, and business profits.

### Note
Full version of the project including reproducible code can be found on github depository. The above write-up is meant to serve as an abstract of the project.






