#My own solution to Harvard CS 109 HW4

## Data
The data set was extracted from the Yelp Phoenix restaurant dataset. 

Joint information about users & business.

| Column | Descriptions |
| -------|--------------|
| user_id | Unique user id |
| business_id | Unique restaurant id |
| date | The date of review |
| review_id | Unique review id |
| stars | Star rating |
| usefulvotes_review | Votes for this review |
| user_name | Name of the user |
| categories | Classiication of this restaurant |
| biz_name | The full business name |
| latitude | Latitude |
| longitude | Longitude |
| business_avg | Average stars over all users reviews for business |
| business_review_count | Review count for this restaurant |
| user_avg | Average rating for this user over all businesses |
| user_review_count | Review count for this user |

## Goal

### 1. Neighborhood-based CF(collaborative filtering) recommender
*Global similar items*

* **Reduce the sparsity** of the original dataset by filtering out business with <= 150 review or users with <= 60 review.
* Get the **common user support** *(count of users who reviewed both)* of each pair of restaurants.
* Write a function to calculate the **similarity** between each pair of restaurants by using **Pearson correlation** *(similarity measurement)*.
    * Use module scipy.stats.stats.
    * Based on reviewers who have reviewed both restaurants.
    * If the common user support == 0, set similarity to 0, otherwise, calculate pearson's r.
* Write a function to provide the **common reviewers** who have reviewed both restaurants in a given pair.
    * Then we can separately pick out the reviews by common reviewers for restaurants in a pair. 
    * The reviews from common reviewers will furtherly be used as inputs to the similarity calculation function.
* Create a **DataBase** of similarities for each pair of restaurants *(global similar restautants)* 
    * Use python class.
    * Use nested loop to calculate similarity between each restaurant pairs.
    * Write a function to enable fetching similarity by providing names of restaurants. 
* Write a function to get **K-Nearest** restaurants of a given restaurant. *(shrink Pearson coefficients to control the effect of small common supports)*
    * Not KNN for classification, we've already created the database for all similarities, just fetch them to use.
* **Sort** the K nearest restaurants to get the top N recommendations.
* Finally, find the **user's top rated restaurants**, find the **nearest neighbors** of these restaurants, **merge** these lists while removing the duplicates and the ones that the user has already rated, and sort by the restaurant's average rating. 

 #### Flow of calculating similarity between restaurants in a pair *&* K nearset restaurants
<p align="justify">
  <img src="https://cloud.githubusercontent.com/assets/7127935/16394991/a6ff77c4-3c6c-11e6-9d83-d5b916c9d0b0.JPG" width="350"/>
  <img src="https://cloud.githubusercontent.com/assets/7127935/16396281/d66d9f30-3c72-11e6-82bb-4ebce2dd13e2.JPG" width="450"/>
</p>

### 2. User based recommender with predicted ratings 
*Given a user & a restaurant, predict the rating of that restaurant of that specific user*

* Find the **K nearest neighbors** from restaurants which have been **rated by this user**.
    * Filter out restaurants rated by this user.
    * Reuse the knearest function from partI using the filtered rather than whole restaurant set as parameter.
* Calculate important variables and get the **predicted rating**.
    * **Baseline** rating = Average rating + (User average - Average rating) + (Restaurant average - Average rating)
    * Key idea of the prediction fomula: ratings on more similar restaurants will affect the predicted rating more.

## Libraries Used
* [pandas](http://pandas.pydata.org/)
* [matplotlib](http://matplotlib.org/)
* [sklearn](http://scikit-learn.org/stable/)
* [numpy](http://www.numpy.org/)
* [scipy](https://www.scipy.org/)
* [operator](https://docs.python.org/3/library/operator.html)
