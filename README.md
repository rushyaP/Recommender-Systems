# Recommender-Systems
Implementation of Collaborative Filtering and Latent Matrix Factorization techniques in recommending movies


The aim is to build a movie recommendation system by taking user ratings for nearly 100,000 movies
and make recommendations for movies that a user has not watched, based on the user's rating of other 
movies, and how similar users rate the movie being recommended.

## Collaborative Filtering
Collaborative filtering is a simple but effective method for predicting how a user will rate a movie they 
haven't watched, based on how similar users rate the movie. Unlike the LFM method, collaborative filtering is
a non-iterative method, and does not require the optimization of any error functions.

The following steps sum up the item-item collaborative filtering process:
1. Select a similarity metric (the cosine/adjusted cosine similarity was used in this project)
	- The cosine similarity between two movies relies simply on the ratings of the two movies
	  across all users
	- The adjusted cosine similarity is similar to the cosine similarity, except that it takes
	  into account that different users have different rating schemes
2. Calculate the similarities across all the movies in the training dataset, using the above measures
3. The similarity measures can then be used to predict how a user will rate a movie that they haven't watched
	- The prediction was be calculated by dividing the 
		sum of (similarity of movie of similar movie*rating that the user gave the movie)
						by
		sum of similarities of all similar movies
4. The RMSE of predictions is then calculated by determining the difference in the predicted ratings and the actual ratings

The results from this model can be used to recommend movies to users, with an idea of how far the predicted rating might be from
the actual rating.

## Latent Factor Models
While methods like collaborative fitlering have proved very useful in building recommender systems, predictions rely on somewhat naive assumptions about similarity measures. In order to build a more intuitive system, latent factor models can be used. The concept of stochastic gradient descent lies at the heart of this approach. The LFM approach treats the rating predictions as an optimization problem, with the main aim being the right way to calculate these ratings.

In an approach that shares similarities with the Singular Value Decomposition (SVD) method, the goal with LFM is to find two matrices, P and Q, such that the rating matrix, R ~= PQt (notice closeness to SVD approximation), where Qt is the transpose of Q. The dimensions of P are m x k (movies to "factors"), and the dimensions of Q are n x k (users to "factors"). This can be thought of as mapping movies and users to a latent space. The use of stochastic gradient descent in this optimization problem, is to determine if the algorithm has converged.

This method also keeps overfitting in check by introducing a regularization parameter (lambda). This parameter allows the proper training where there is sufficient data, and causes aggressive shrinkage, where there isn't enough data.

The final function that must be optimized, has two parts:

The error term, which is the squared sum of the difference between the actual ratings and predicted ratings
The product of the regularization parameter (lambda) and the "lengths" of P and Q, which just represents the amount of data that is available in the two matrices.
The final algorithm was put into effect as follows:

1. First, the P and Q matrices were initialized: a) Q = m x k matrix of movies and factors, with all elements initialized to a random value in the range [0, (5/k)^1/2] b) P = n x k matrix of users and factors, with all elements initialized to a random value in the range [0, (5/k)^1/2]
2. Then, stochastic gradient descent is performed, in an interative manner (until convergence) to determine the new values of P & Q:
	- Both P and Q at time t change by a small amount, equal to the product of the learning rate and the derivative of the corresponding matrix. These new values of P and Q 	   are used in the next iteration
	- The gradient of P can be calculated as the negative difference between the actual rating and the predicted rating, multiplied by Q at a time t, added to the product 		  of the lambda and P at time t
	- The gradient of Q can be calculated as the negative difference between the actual rating and the predicted rating, multiplied by P at a time t, added to the product 		  of the lambda and Q at time t
3. Step 2 happens iteratively, and, for each execution, the value of the error function is stored. At the end of the requested number of iterations, this function is plotted against the number of iterations. The value of the error function appears to deecrease, as the number of iterations increase, until it reaches a minimum. After this, the error begins to increase again, signaling to possible overfitting. The number of iterations for which this error is minimum, is the point of convergence.

The resulting model made high-accuracy movie predictions, with an RMSE of less than 0.9.
