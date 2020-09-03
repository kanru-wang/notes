# Do we cross validate tabular data the same way we do with time series data?

Regarding the hyper-parameter selection cross validation of tabular data, I think
 
1. The size of the training dataset has a large impact for the best hyper-parameter chosen.
e.g. the model need to be very conservative for a very small training dataset.
 
2. The following cross validation method will not find the best hyper-parameter set for the final model, because the process will find the best hyper-parameter set for a smaller training dataset.

        ` stands for training period
        & stands for validation period
        `````````&&&
        ````````&&&
        ```````&&&
        ``````&&&
        `````&&&
 
3. The above cross validation method is recommended for time series, because for example ARIMA model does not use the entire past period to train, so the best ARIMA setting (i.e. hyper-parameter) for a short time series can also be the best for a long one.

        ` stands for effective training period
        ^ stands for non-effective training period
        & stands for validation period
        ^^^^`````&
        ^^^`````&
        ^^`````&
        ^`````&
        `````&

4. If there is a column in the training dataset that can identify the customer, we can do a grouped k-fold cross validation.
It ensure that **rows generated from the same customer will be in one of the followings: training set, or validation set, or testing set, but will not spread into multiple of them.** The whole training dataset is used; do not progress along the timeline.
 
5. Drawbacks of the grouped k-fold cross validation on "customer ID":

    - If there are temporary trends, part of a validation set may be very similar to part of a training set, so data leakage will happen, so a hyper-parameter set that will overfit the training set will be encouraged. See the following solutions to address this concern:

        - Taking the Point 1 into consideration, a very basic 1-fold cross validation

                `````````&&&

        - Or k-fold grouped by time instead

                `````````&&&
                ``````&&&```
                ```&&&``````
                &&&`````````

        - May also add "purging", which is useful when it is very important to eliminate data leakage. It is done to both ends of the testing period. See: https://www.youtube.com/watch?v=hDQssGntmFA

                ` stands for training period
                & stands for validation period
                "B" stands for a trend arise in features
                "A" stands for a trend settle in features

                Just look at one of the CV folds

                for short trend
                ``````&&&```
                BBBBBAABBBBB

                or longer trend
                ``````&&&```
                BBBBBAAAAABB

                or multiple trends (e.g. different unknown marketing campaigns going on)
                ``````&&&```
                CDDEEFFGGHHI

                All can be purged to
                `````  &  ``
                BBBBB  A  BB

        - Or treat it as a feature drifting problem and remove instable features.

    - When there isn’t a column that indicates “customer ID”, need to find an proxy.