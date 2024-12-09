# CSE151AGroupProject - NBA Player Scoring Prediction

## Introduction to our project
Our project looks to examine the key factors influencing NBA player scoring, utilizing a comprehensive dataset of player statistics from 1950 to 2017. By examining a wide range of features from our dataset—including player position, minutes played, field goal percentage, and usage rate—we seek to identify the elements that contribute most significantly to a player's scoring ability, and to be able to predict future stats with our supervised learning model (supervised because we have quantitative labeled data). 

In addition, we want to know which combinations of these features are the best predictors for high point totals in an NBA player's entire career. The insights gained from this study can be a valuable resource for stakeholders in the realm of basketball:
- Sports bettors: Offers a data-driven edge for predicting player performance.
- Fantasy basketball players: Helps construct optimized teams by focusing on high-potential players.
- Coaches or GMs: Assists in identifying players with scoring potential for better team-building decisions.

The project’s broader impact lies in the ability to enhance basketball analytics, making statistical predictions more accurate and accessible to all.



## Figures (need to provide explanation and elaboration??!!)
The dataset spans from 1950s and contains key statistics for players across multiple teams.

1. Team Distribution:
- Largest pool: 'TOT' (>2000 players)
- Second largest: 'NYK' (~1050 players)
- Third largest: 'BOS' (~1000 players)
![alt text](/figures/image.png)
Figure 1.1: Distribution of individuals in each team


2. Visualization of relationship between each player feature
At a glance, we can see that PTS are generally normally distributed. We also observe a linear relationship between PTS and FGA, and PTS and FTA.
![alt text](/figures/image-1.png)
Figure 1.2: Pair plot of Player Features


3. Correlation between each player feature
The strongest correlations were between: FGA and PTS at 0.99, followed by MP and PTS, and MP and FGA at 0.93.
![alt text](/figures/image-2.png)
Figure 1.3: Correlation Heatmap of Player Features

4. Missing Data
We notice that `blanl` and `blank2` have the most missing values of greater than 20,000. There were also significant missing values in `3P%` of more than 9,000, followed by `3PAr, GS, TOV and USG` at about 5,000 data points each.


## Methods

1. Data Exploration and Results
2. Data Preprocessing
3. Models

Model 1: Linear Regression
Model 2: Polynomial Regression
Model 3: PCA and SVD

## Results
1. Model Performance

## Discussion

## Conclusion

## Statement of Collaboration (please add on)

1. Alex Sieh: Conducted preliminary research on the dataset. Worked on the abstract
2. Dionne Leow: Helped with data cleaning and preprocessing. Also wrote the final write up of the Readme.
3. Edgar Guzman: Contributed to data preprocessing. Developed model 3, experimenting with PCA and SVDs to improve our model.
4. Jonathan Duong: Helped with data cleaning and preprocessing. Contributed to the creation of model 1 and 2.
5. Ryan Chon: Helped with data cleaning and preprocessing. Wrote up the Readme for milestone 3 and 4.







# PREVIOUS SUBMISSIONS:

### How will you preprocess your data:
- to (one-hot) encode categorical data like Team, Positions
- for null/nan values, we will replace it with an adjusted mean values
- for each of the season total categories (PTS, TRB, AST), we will divide it by the games played to get the per game average.
- use min-max scaling to normalize the features


### How we preprocessed the data 
#### Encoding
- We performed one-hot encoding for categorical variables Team (`Tm`) and Position(`Pos`).
- Included a `dummy_na=True` parameter to handle missing values during encoding.

#### Imputing Null Values
- We begin by dropping unwanted values
  - unnamed: 0, blanl, blank2 were removed because they were empty/meaningless values
  - GS, Player were dropped because we didn't think GS would be a good predictor and Player name wouldn't also matter at all
  - ORB and DRB were removed because Total rebounds was a cumulative stat of the two
  - 2 pointers,3 pointers and FG made were all removed because they directly indicate points made and we believed it could cause problems with overfitting
- Next we filled in the null values of our columns
  - The columns of observations are filled with the mean values from their respective year to ensure the data aligns with the style and era of basketball played.
  - We manually punched in null values in MP by the global average of minutes played divided by global average of Games played because we encountered problems with small game sizes from player to player.
  - The last drop is to make sure that any null values remaining are dropped and don't cause a problem
#### Changing Stats to Per game
- We changed Points, Rebounds,Assists, Minitues played, Field Goal Attemptes, 3 Point Attempts, 2 point Atttempts, and Free Throw Attempts to their per game variations because we want to be able to predict points per game of future players

#### Scaling our data and Splitting the data
- To improve the performance and accuracy of our linear regression model, we scaled our input data using Min-Max normalization to prevent features of large range from dominating the results
- We split our training and testing sizes 80:20 using sk:learn
- y_train and y_test serve as ground truth labels, representing the actual points per game derived from our dataset. These values were calculated by dividing each player’s total points in the season by the number of games played, providing a reliable basis for evaluating the model's predictions.
### Training Our First Model and How it performed
- Using SciKitlearn Linear Regression library, we created a linear regression model and trained on our training data
- Using MSE as our cost function we had a cost of .81 from our testing data and .76 from our training data

### Our model on the fitting graph
With our model being quite simple, our nearly identical training and testing error, which are characteristics or a model on the left end of the fitting graph. For our next model, we are thinking of trying polynomial regression models because of the added complexities of adding the polynomial terms of the original features.

### Conclusion 1st Model
The conclusion of our 1st model shows our model can achieve a moderate level of accuracy in predicting PPG. To improve our model in the future, we might want to exclude most of the data from 1950s to 1980s because lots of the data had to be imputed and are carried by global averages. 

## Our second Model
Our second Model was a polynomial regression that would be  to be able to read complex statlines like(WS, BPM, PER) in order to predict the more complicated
effect they had on PPG. We tested out multiple different degrees for the polynomial degrees ultimately deciding that 3rd degree polynomials was the right balance
between performance and complexity

### How it fits on the fitting graph
Our model lies in the middle of this fitting graph, where degree equals 3. A decently complex model, which makes sure we are not underfitting while also minimizing the difference between our training and testing error

### Conclusion 2nd Model
Overall compared to our first model, we observed a large increase in performance, with our model more accurately determining PPG by .5 points better.
With the added complexity of the polynomial terms, we were able to challenge the underfitting problem we encountered with our first simple linear regression model.
In order to make it better: 
- We could of have had possible overfitting from all of the imputed data, that largely makes up players pre 1980s era. Training without so much imputed data can lead to better predictions
- To tackle the problem of overfitting that could result of the higher degree polynomials, adding a regularization term to emphasize the cost of large values may lead to better performance
- If our model is too reliant on the polynomial terms we can instead try something like a Decisicion Tree Regression that isn't linear and manually creates new features.

### FNFP and Correct
Correct Values:  3736
FNFP Values:  1092
These values were gotten when our model was within .5 of the true value of the PPG. Although our model had showcased a small error cost, in the context of sports betting such small values can derail a possible hit which is a small problem with our model.
