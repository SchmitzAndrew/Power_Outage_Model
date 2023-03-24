# Power_Outage_Model

Our exploratory data analysis on this dataset can be found [here](https://schmitzandrew.github.io/Power_Outage_Analysis/).

## Framing The Problem

### Our Prediction Problem/ Type:

Our prediction problem is determinging the duration of a power outage based on it's characteristics. This is a regression problem.

### Response Variable:
We are trying to predict the duration of a power outage given the information we have about the outage. 

### Metric:
We are using R^2 as our metric for model performance. We chose this metric since it accurately measures how well our model is fit to the data. R^2 is a also good metric for our problem since we are trying to predict a continuous variable.We also used this metric since it is built in the .score() method of sklearn. Other methods require additional methods and are not built into the library we are using. 

### Justify Information Known At "Time of Prediction"
We carefully selected features from the dataset that were only known when the power outage had just started. These are all variables such as the location of the power outage, the start date, and climate information of the power outage. These are all information that is readily available to us when the power outage has just started.


## Baseline Model

### Features and Description of Model
Our first model is very simple and does basic feature engineering and uses a limited amount of data. We used the following features:
Population:
- Population of the state the power outage is in
Cause Category: 
- The cause of the power outage
Population urban:
- The percentage of the population in the state that is urban

We have two quantitative features and one categorical feature that is nominal. 

We performed the neccessary encoding of the cause.category column. Obviously, this was a categorical column so we used the OneHotEncoder in our pipeline. 


### Model Performance

Our current model is very bad since it is very overfit. We need to add more features, perform feature engineering, and optimize our tree depth to improve our model. Our model would average a training R^2 of .70 and testing R^2 of -.10. This model is very overfit and does not generalize well to new data.

## Final Model

### Features Added With Reasoning
We added the following features to our model:
- Month: The month the power outage started. This feature might of impropved our perfromance since winter months might have longer power outages than summer months. 
- Climate Category: The climate category of the state the power outage is in. This feature might have improved our performance since some climates might have longer lasting outages than others.
- Anomaly Level: This gives us more weather information about the power outage's location. We believe that worse weather might make it harder for power outages to be fixed, increasing the length of the outage.

### Model Chosen, Feature Engineering, and Hyperparameters
We chose to stick with the Decision Tree Regressor since it is a very simple model that is easy to interpret. We also continued to use the OneHotEncoder for our categorical features.
We also binarized the POPDEN_RURAL variable since it was a continuous variable. We determined the cutoff for this variable in project 3, and binarized it since you can seperate states into rural and urban based on this variable.
We also quantailized the Population feature since it was a continuous variable. This allows for the model to better understand the relationship between population and the duration of the power outage.

For our feature engineering, we used GridSearchCV to find the best hyperparameters for our model. We used the following hyperparameters: max_depth, min_samples_split. These are good hyperparameters since they prevent overfitting. The hyperparamters we ended up using are max_depth=3 and min_samples_split=20. These hyperparameters are good since they prevent overfitting and allow the model to generalize well to new data.

### Model Performance Compared to Baseline

Although are training R^2 is lower than our baseline model, our testing R^2 is higher. This means that our model is not overfit and is able to generalize well to new data. Our model would average a training R^2 of .25 and testing R^2 of .25. This model is better than our baseline model since the testing R^2 is higher.

## Fairness Analysis

### State Choice of Group X and Y

#### Our choice for Group X:
Power outages that occur in the months 1-6.

#### Our choice for Group Y:
Power outages that occur in the months 7-12.

### Evaluation Metric

We are going to be using the R^2 as our evaluation metric

### Hypotheses

#### Null Hypothesis:
Our model is fair. The R^2 for Group X and Group Y are roughly the same, and any differences are due to random chance.

#### Alternative Hypothesis:
Our model is unfair. The R^2 for Group X is higher than the R^2 for Group Y.

### Test Statistic, Signifigance Level, P-Value

Our test-statistic is going to be the difference between R^2 scores of Group X and Y.
Our alpha or significance level that we are going to be measuring our strength of evidence with is 0.05.
After running our permutation tests, we get a p-value of 0.31 which is greater than our significance level of 0.05. We fail to reject 
the null because our p_value is higher than our significance level of 0.05. This evidence suggests that we cannot support 
the claim that the R^2 for Group X is higher than the R^2 for Group Y.
