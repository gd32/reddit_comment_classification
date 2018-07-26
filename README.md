# Predicting Reddit thread participation for NBA Playoff games

## Table of Contents

* Summary
* Methods
* Results

## Summary

In this project, I developed machine learning models that aim to predict if a Reddit comment was posted by a fan whose team was participating in a NBA playoff game (participants) or by a fan of a non-participating team (non-participants). 

On the NBA subreddit, users are allowed to 'flair' themselves with an icon that appears next to their username on public posts. This flair acts as a badge to identify that user as a supporter of the displayed team. While users can select any flair for use on the subreddit, it has become common practice for users to select a flair that represents the team that they primarily support.

I scraped comment data from the NBA subreddit's 'Official Game Threads', which are posted before every scheduled preseason, regular season, and postseason game and allow users to discuss the events that occur during the game as well as the game's results. For this analysis, I chose to limit my data collection to comment threads pertaining to NBA playoff games played during the 2017-2018 NBA playoffs and only to top level and 1st-child level comments in each game thread. From the extant playoff Game Threads, I collected a total of **43998** unique user comments along with the score of each comment (i.e. the number of times the comment was liked), the publicly shown flair of the poster, and the unique game thread ID. 

Using the game thread ID, I identified which teams were participating in each game and labeled each comment as one posted by a participant or one posted by a non-participant to generate the target values for my classification models. The model features were the words contained in the comment text. 

## Methods

To scrape the reddit comments, I used the PushShift API (https://github.com/pushshift/api) which is an enhanced version of the reddit API that allows for improved searches of reddit submissions and comments.

I used regex matching to clean the text data and used both sklearn's CountVectorizer and TfIdfVectorizer to prepare the data for use in modeling.

I fit the following classification model algorithms to predict participant status: Logistic Regression, Decision Tree, SVM, Random Forest, Bagging (with both Decision Tree and Random Forest as base classifiers), and Gradient Boosting.

To compare model strength, I looked at 3 metrics: accuracy, ROC AUC, and fit time. 

## Results

| Model                   | Accuracy |  ROC AUC | Fit Time |
|-------------------------|----------|----------|----------|
| Logistic Regression     | 0.671    | 0.552  | 0 min 46s  |     
| Decision Tree           | 0.604    | 0.534   | 2 min 24s |      
| SVM                     | 0.643    | 0.643   | 21 min 14s|      
| Random Forest           | 0.671    | 0.672   | 2 min 11s |      
| Bagging (Decision Tree) | 0.638   | 0.574  | 9 min 48s   |    
| Bagging (Random Forest) | 0.656    | 0.543   | 6 min 0s  |      
| Gradient Boosting       | 0.634    | 0.634   | 17 min 48s|      

The Random Forest classifier was chosen as the best mix of short processing time and high scoring in my metrics.
