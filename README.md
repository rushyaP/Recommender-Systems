# Recommender-Systems
Implementation of Collaborative Filtering and Latent Matric Factorization techniques in recommending movies

## Collaborative Filtering

## Latent Factor Models
While methods like collaborative fitlering have proved very useful in building recommender systems, predictions rely on somewhat naive assumptions about similarity measures. In order to build a more intuitive system, latent factor models can be used. The concept of stochastic gradient descent lies at the heart of this approach. The LFM approach treats the rating predictions as an optimization problem, with the main aim being the right way to calculate these ratings.

In an approach that shares similarities with the Singular Value Decomposition (SVD) method, the goal with LFM is to find two matrices, P and Q, such that the rating matrix, R ~= PQt (notice closeness to SVD approximation), where Qt is the transpose of Q. The dimensions of P are m x k (movies to "factors"), and the dimensions of Q are n x k (users to "factors"). This can be thought of as mapping movies and users to a latent space. The use of stochastic gradient descent in this optimization problem, is to determine if the algorithm has converged.

This method also keeps overfitting in check by introducing a regularization parameter (lambda). This parameter allows the proper training where there is sufficient data, and causes aggressive shrinkage, where there isn't enough data.

The final function that must be optimized, has two parts:

The error term, which is the squared sum of the difference between the actual ratings and predicted ratings
The product of the regularization parameter (lambda) and the "lengths" of P and Q, which just represents the amount of data that is available in the two matrices.
The final algorithm was put into effect as follows:

First, the P and Q matrices were initialized: a) Q = m x k matrix of movies and factors, with all elements initialized to a random value in the range [0, (5/k)^1/2] b) P = n x k matrix of users and factors, with all elements initialized to a random value in the range [0, (5/k)^1/2]
Then, stochastic gradient descent is performed, in an interative manner (until convergence) to determine the new values of P & Q:
Both P and Q at time t change by a small amount, equal to the product of the learning rate and the derivative of the corresponding matrix. These new values of P and Q are used in the next iteration
The gradient of P can be calculated as the negative difference between the actual rating and the predicted rating, multiplied by Q at a time t, added to the product of the lambda and P at time t
The gradient of Q can be calculated as the negative difference between the actual rating and the predicted rating, multiplied by P at a time t, added to the product of the lambda and Q at time t
Step 2 happens iteratively, and, for each execution, the value of the error function is stored. At the end of the requested number of iterations, this function is plotted against the number of iterations. The value of the error function appears to deecrease, as the number of iterations increase, until it reaches a minimum. After this, the error begins to increase again, signaling to possible overfitting. The number of iterations for which this error is minimum, is the point of convergence.
The resulting model made high-accuracy movie predictions, with an RMSE of less than 0.9.
