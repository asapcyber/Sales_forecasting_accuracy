# Sales_forecasting_accuracy
R project to develop predictive models to improve accuracy of quarterly Sales Forecasting.

Sales opportunities are tracked in Salesforce.com to forecast what the likely bookings are that will be closed in a given quarter. The traditional forecasting method uses values of attributes captured by Sales Reps on each opportunity to determine a best estimated boooking number. For example, whether an Opportunity status is "low Risk", or "Committed", or the stage it is in (T1-Qualified, T-4 Negotiated etc.). The challenges with the current methos are:
* forecasting at the beginning of the quarter is very difficult so initial estimates tend to be poor quality. Accuracy is closer to 80-90% in the last 1-3 weeks of the quarter.
* accuracy is not measured for ongoing improvements, i.e. the method doesn't change over time, so any learnings from Opportunities that were won/lost are not incoporated in future forecasting models.

This project aims to improve Sales forecastung accuracy by applying statistical analysis and models to Opportunities data. The main objectives of the approach are:
* leverage large amounts of historical Opportunities data to create a model for forecasting sales bookings with an accuracy level that is 10-20 % better than today's method.
* improve timing of accuracy such that 80%+ accurate forecasts can be published at the start of the quarter.
* once a model has been developed, create a framework for contiuous improvement using Machine Learning (ML) methods to incorporate learnings from past forecasts in future forecasts.

The steps to implement this project are:
1. Extract Opportunities data for the past 2 years with as many usable attributes as possible.
2. Build predictive models in R using 3 different methods: Logistic Regression, CART and Random Forest.
3. Evaluate the accuracy of the model using historical data split into training/testing samples, and select a model that yields 80%+ accuracy.
4. Make preditcions on future quarters' data and based on accuracy results adjust the selected model.

# Extracting data
A tabular report is developed in Salesforce.com to extract all opportunities data for the past 2 years, including approx. 50 attributes like Opportunity Age, Status, Region, Sales Price etc. This report is saved as a CSV file and split into a Training and a Testing set based on dates of the opportunities. The ones before the most recent quarter are used for <b>training</b> the model, and the ones in the past quarter are used for <b>testing</b> it. After reading the CSV files into separate data frames, a subset is created for each that only includes opportunities for which the Stage is either Won or Lost, i.e. we know whether they ended up in booking an order or not. This way once the model is trained we can verify its accuracy using the actual designation of the closed Opportunities.
Before building different predicitive models a set of data preparation operations needs to be performed:
* Convert factors to numbers using as.numeric function for Logistic Regression model
* Create a new numeric variable (<b>WinLoss</b>) using the Stage of an Opportunity, taking value 1 if the Opportunity was WON, and value 0 if it was LOST. This will be used as the dependent variable for the predictive models.
* Load libraries for CART and Random Forest models

# Build predictive models
The three models that are created to predict Opportunities that will be booked are:
* Generalized Linear Model (GLM) using logistic regression, which predicts a binary outcome, i.e. is an opportunity won or lost based on various independent variables. The initial variables used to predict WinLoss are: Opp.Age, Type, Strategic.Account, Competitor.Count, Account.Type, Close.Date.Changed, Forecast.Category, Industry, Deal.Forward, In.Forecast. After a few iterations the list is reduced to the following: Opp.Age, Type, Close.Date.Changed, In.Forecast.
* Classification And Regression Tree (CART) model with a set.seed parameter of 3000, using the same independent variables as in the Logistic Regression model. Other parameters set in the model are method="class", minbucket=10.
* Random Forest Model, which requires the dependent variable to be changed to a factor using the as.factor function. Other parameters set in the model include ntree=300, nodesize=10, na.action=na.roughfix.
Each model is run on the <b>training data</b> using the specified parameters, and then the model is used to run predictions.

# Make predictions
For each of the models predictions are made on the <b>test</b> data that translate for each opportunity in the current quarter to the predicted Win/Loss outcome. This prediction is then used to measure accuracy and determine what % of the bookings tied to the in-quarter opportunities will lead to forecasted total $$ bookings.

# Evaluate accuracy
The accuracy of each model is measured using a confusion matrix to compare actual wins/losses againts the ones forecasted by each model, using the test data set. Sensitivity is used to help forecast actual win rate, i.e.the true positive rate, which measures the proportion of positives that are correctly identified as such.
Sensitivity=#of true positives (correctly forecasted wins)/# of TP + # of false negatives (incorrectly forecasted losses)
The total dollars amount of the in-quarter bookings is multipled by the sensitivity % number to then derive the forecasted bookings for the current quarter, using the following criteria and formula:
- all opportunities WHERE 'Stage' is not lost (i.e. exclude any oportunities that have already closed and been lost)
- all opportunities WHERE 'Prediction' is 1 (=Win)
- Forecasted bookings (in USD) = Total.Price..converted*sensitivity

# Write output file
Using the prediction from the most accurate model a new column is added to the <b>test</b> data that has values 1(=win) or 0(=loss) for each opportunity in the data set.

#Iterate model with new data and measure accuracy again
Once the bookings forecast is completed the prediction process is re-run daily using updated opportunity information on the previsou and current quarter, to ensure the forecast continues to be refined as the win/loss rate of opportunities changes. The models are refined based on updated accuracy scores and the most accurate model is re-applied to the new opportunity data and used to create a new Win/Loss prediction for each opportunity.





