# TwitterPrediction
Prediction on Twitter data (Superbowl 49, 2015)  
### Introduction:
A useful practice in social network analysis is to predict future popularity of a subject or event. Twitter, with its public discussion model, is a good platform to perform such analysis. With Twitter’s topic structure in mind, the problem can be stated as: knowing current (and previous) tweet activity for a hashtag, can we predict its tweet activity in the future? More speciﬁcally, can we predict if it will become more popular and if so by how much? In this project, we will try to formulate and solve an instance of such problems.
The available Twitter data is collected by querying popular hashtags related to the 2015 Super Bowl spanning a period starting from 2 weeks before the game to a week after the game. We will use data from some of the related hashtags to train a regression model and then use the model to make predictions for other hashtags. To train the model, you need to prepare training sets out of the data, extract features for them, and then ﬁt a regression model on it. The regression model will try to ﬁt a curve through observed values of features and outcomes to create a predictor for new samples. Designing and choosing good features is one of the most important steps in this process and is essential to getting a more accurate system. There are examples of such analysis and useful features in literature 1 (You should look into the literature for this). You will be given training data to create the model, and test data to make predictions. The test data consists of tweets containing a hashtag in a speciﬁed time window, and you will use your model to predict number of tweets containing the hashtag posted within one hour immediately following the given time window.

### Part 1: Popularity Prediction

#### TASK 1. A ﬁrst look at the data - dowloaded twitter data for Superbowl 2015  

We will report the following statistics for each hashtag:   
* Average number of tweets per hour      
* Average number of followers of users posting the tweets per tweet (to make it simple, we average over the number of tweets; if a users posted twice, we count the user and the user’s followers twice as well)     
* Average number of retweets per tweet    

#### TASK 2 :  Plot “number of tweets in hour” over time for #SuperBowl and #NFL (a histogram with 1-hour bins). The tweets are stored in separate ﬁles for diﬀerent hashtags and ﬁles are named as tweet [#hashtag].txt  


##### 2. Linear regression    
Create time windows from the data to extract features. Here, use 1-hour time window and calculate the features in each time window, resulting in <# of hours> data points.    
For each hashtag data ﬁle, ﬁt a linear regression model using the following 5 features to predict number of tweets in the next hour, with features extracted from tweet data in the previous hour. The features you should use are:  

* Number of tweets   
* Total number of retweets   
* Sum of the number of followers of the users posting the hashtag   
* Maximum number of followers of the users posting the hashtag    
* Time of the day (which could take 24 values that represent hours of the day with respect to a given time zone)    

For each hashtag, you should train a separate model.  
#### TASK 3: For each of your models, report your model’s Mean Squared Error (MSE) and R-squared measure. Also, analyse the signiﬁcance of each feature using the t-test and p-value. You may use the OLS in the libarary statsmodels in Python.


##### 3. Feature analysis  
#### TASK 4: Design a regression model using any features from the papers you ﬁnd or other new features you may ﬁnd useful for this problem. Fit your model on the data of each hashtag and report ﬁtting MSE and signiﬁcance of features.  
#### TASK 5: For each of the top 3 features (i.e. with the smallest p-values) in your measurements, draw a scatter plot of predictant (number of tweets for next hour) versus value of that feature, using all the samples you have extracted, and analyze it.  
Do the regression coeﬃcients agree with the trends in the plots? If not, why?

##### 4. Piece-wise linear regression  
Since we know the Super Bowl’s date and time, we can create diﬀerent regression models for diﬀerent periods of time. First, when the hashtags haven’t become very active; second, their active period; and third, after they pass their high-activity time.  
#### TASK 6: We deﬁne three time periods and their corresponding window length as follows:  

* Before Feb. 1, 8:00 a.m.: 1-hour window  
* Between Feb. 1, 8:00 a.m. and 8:00 p.m.: 5-minute window  
* After Feb. 1, 8:00 p.m.: 1-hour window  

For each hashtag, train 3 regression models, one for each of these time periods (the times are all in PST). Report the MSE and R-squared score for each case.  
#### TASK 7: Also, aggregate the data of all hashtags, and train 3 models (for the intervals mentioned above) to predict the number of tweets in the next hour on the aggregated data.  
Perform the same evaluations on your combined model and compare with models you trained for individual hashtags.  


##### 5. Nonlinear regressions
**Ensemble methods**
In this part, we use RandomForestRegressor and GradientBoostingRegressor from sklearn as two examples of ensemble regressors. Still use the aggregated data in this part.

#### TASK 8: Use grid search to ﬁnd the best parameter set for RandomForestRegressor and GradientBoostingRegressor respectively. Use the following param grid  
{  
’max_depth’: [10, 20, 40, 60, 80, 100, 200, None], ’max_features’: [’auto’, ’sqrt’], ’min_samples_leaf’: [1, 2, 4],     ’min_samples_split’: [2, 5, 10], ’n_estimators’: [200, 400, 600, 800, 1000, 1200, 1400, 1600, 1800, 2000]  
}  
Set cv = KFold(5, shuffle=True), scoring=’neg mean squared error’ for the grid search.  
Analyze the result of the grid search. Do the test errors from cross-validation look good? If not, please explain the reason.  

#### TASK 9: Compare the best estimator you found in the grid search with OLS on the entire dataset.
#### TASK 10: For each time period described in TASK 6, perform the same grid search above for GradientBoostingRegressor (with corresponding time window length). Does the crossvalidation test error change? Are the best parameter set you ﬁnd in each period agree with those you found above?
**Neural network**
#### TASK 11: Now try to regress the aggregated data with MLPRegressor. Try diﬀerent architectures (i.e. the structure of the network) by adjusting hidden layer sizes. You should try at least 5 architectures with various numbers of layers and layer sizes. Report the architectures you tried, as well as its MSE of ﬁtting the entire aggregated data.
#### TASK 12: Use StandardScaler to scale the data before feeding it to MLPRegressor (with the best architecture you got above). Does its performance increase?
#### TASK 13: Using grid search, ﬁnd the best architecture for each period (with corresponding window length) described in Question 6.

##### 6. Using 6x window to predict
Download the test data3. Each ﬁle in the test data contains a hashtag’s tweets from a 6xwindow-length time range. Fit a model on the aggregate of the training data for all hashtags, and predict the number of tweets in the next hour for each test ﬁle. The ﬁle names consist of sample number followed by the period number the data is from. For example, a ﬁle named sample0 period1.txt contains tweets in a sample 6-hour4 window that lies in the 1st time period described in Question 6, while a ﬁle named sample0 period2.txt contains tweets in a sample 30-min5 window that lies in the 2nd time period. One can be creative here, and use the data from all previous 6 hours for making more accurate predictions (a
## TASK 14: Report the model you use. For each test ﬁle, provide your predictions on the number of tweets in the next time window
**Note**: Test data should not be used as a source for training. You are not bounded to only linear models. You can ﬁnd your best model through cross validation of your training data


## Part 2: Fan Base Prediction  
The textual content of a tweet can reveal some information about the author. For instance, users tweeting on a topic may have opposing views about it. In particular, tweets posted by fans of diﬀerent teams during a sport game describe similar events in diﬀerent terms and sentiments. Recognizing that supporting a sport team has a lot to do with the user location, we try to use the textual content of the tweet posted by a user to predict their location. In order to make the problem more speciﬁc, let us consider all the tweets including #superbowl, posted by the users whose speciﬁed location is either in the state of Washington (not D.C.!) or Massachusetts. For example, in order to include all the tweets with the author located in the state of Washington, we consider the tweets that include the following substrings in the location ﬁeld ( json_object[’tweet’][’user’][’location’] ):
* Seattle, Washington  Washington • WA • Seattle, WA • Kirkland, Washington • etc.
#### TASK 15:
1. Explain the method you use to determine whether the location is in Washington, Massachusetts or neither. Only use the tweets whose authors belong to either Washington or Massachusetts for the next part.
2. Train a binary classiﬁer to predict the location of the author of a tweet (Washington or Massachusetts), given only the textual content of the tweet (using the techniques you learnt in project 1). Try diﬀerent classiﬁcation algorithms (at least 3). For each, plot ROC curve, report confusion matrix, and calculate accuracy, recall and precision.
## Part 3: Deﬁne Your Own Project
#### TASK 16: The dataset in hands is rich as there is a lot of metadata to each tweet. Be creative and propose a new problem (something interesting that can be inferred from this dataset) other than the previous parts. You can look into the literature of Twitter data analysis to get some ideas. Implement your idea and show that it works. As a suggestion, you might provide some analysis based on changes of tweet sentiments for fans of the opponent teams participating in the match. 
