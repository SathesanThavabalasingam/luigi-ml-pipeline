## Datasets

There are two files: `airline_tweets.csv` and `cities.csv`.

`airline_tweets.csv` has twitter data regarding airline sentiment augmented
with some extra columns.  The relevant columns are:

* `airline_sentiment`: a string indicating if the tweet had positive,
  neutral or negative sentiment.
* `tweet_coord`: is a string with form "[<lat>, <long>]" if a
  geo-coordinate exists for that tweet, or an empty string otherwise.

The `cities.csv` contains information about latitude and longitude for large cities.
The relevant columns are:

* `name`: The name of the city.
* `latitude`: The latitude of the city.
* `longitude`: The longitude of the city.

## Problem

Build a basic ML pipeline using the `luigi` Python framework.  The pipeline
should clean the tweet data, prepare features for building a model, train a
classifier and score using the model.  The pipeline should have these steps:

 * `CleanDataTask`: Cleans the input tweet CSV file by removing any rows without valid geo-coordinates.
    * An invalid coordinate has either an empty `tweet_coord` column or is coordinate (0.0, 0.0).
 * `TrainingDataTask`: Extracts features/outcome variable in preparation for training a model.
    * This prepares the cleaned data into the exact form that is able to be fit by the model.
    * The "y" variable will be the multi-class sentiment (0, 1, 2 for negative, neutral and positive respectively).
    * The "X" variables will be the closest city to the "tweet_coord" using Euclidean distance.
    * You should use the `cities.csv` file to find the closest city.
    * You probably will need to one-hot encode the city names.
 * `TrainModelTask`: Trains a classifier to predict negative, neutral, positive based only on the input city.
    * Train a classifier that uses closest cities as features.
    * Dump the fitted model to the output file.
 * `ScoreTask`: Uses the scored model to compute the sentiment for each city.
    * Use the trained model to predict the probability/score for each city the
      negative, neutral and positive sentiment.
    * Output a sorted list of cities by the predicted positive sentiment score to the output file.


## References

 * Luigi package: `http://luigi.readthedocs.io/en/stable/`
