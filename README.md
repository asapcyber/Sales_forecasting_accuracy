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
A tabular report is developed in Salesforce.com to extract all opportunities data for the past 2 years, including approx. 50 attributes like Opportunity Age, Status, Region, Sales Price etc. This report is saved as a CSV file and split into a Training and a Testing set based on dates of the opportunities. The ones before the most recent quarter are used for training the model, and the ones in the past quarter are used for testing it. After reading the CSV files into separate data frames, a subset is created for each that only includes opportunities for which the Stage is either Won or Lost, i.e. we know whether they ended up in booking an order or not. This way once the model is trained we can verify its accuracy using the actual designation of the closed Opportunities.

# Build predictive models

# Evaluate accuracy

# Make predictions and iterate



