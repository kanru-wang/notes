# Overfitting, gap between train and test performance matters

From https://towardsdatascience.com/are-you-really-taking-care-of-overfitting-b7f5cc893838

You may end up having the following two models:

            Training performance    Test performance
    Model A                 0.94                0.79
    Model B                 0.82                0.79

You may conclude that Model A and Model B are basically the same, because they obtain the same result on test data. However, if you have the whole picture, you will notice that Model B is way better than model A, because model A is clearly overfitting.

And there are many reasons why you want to avoid overfitting:

- When you have an overfitting model, it is probably feasible to find a better model (i.e. a model with a lower training performance and/or a higher test performance), which is: moving from model A to model B.
- A machine learning model is meant to learn patterns. An overfitting model is a model that has learned many wrong patterns.
- An overfitting model will get old soon. If your intention is to use your model over time, then you will suffer more of concept drift.