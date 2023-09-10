# grouping-algorithm
# Model Description
Overview: <br>
We trained an end to end neural network to predict what pod group a student would be most optimal in, provided their data. The model's goal is to identify a pattern in the training data that would ultimately lead to an accurate prediction when encountering new data, in other words, a regression model. We were able to successfully build a functional model using a deep neural network (DNN) architecture with the ReLU activation function for the hidden layers and a linear activation function for the output layer as well as a dropout rate to prevent overfitting. Finally when compiling and fitting the model, we used the Mean Squared Error (MSE) loss function and a stochastic gradient descent-like optimizer that inherits back-propagation.

Model Description: <br>
Using a neural network to regulate such a task requires utilizing an optimization function, a loss function, an activation function within the layers, and regularization implementations to prevent overfitting. At the heart of the neural network, when defining the architecture (number of nodes, layers, etc.), we need to specify the activation function as well as the dropout rate in order to prevent overfitting on the training data. Activation functions are used to determine the output of each layer and vary depending on which function is used. For example if the activation function was defined as a sigmoid or logistic function, each layer would output a value between 0 and 1. On the other hand if we used a function such as ReLU (Rectified Linear Unit) that we used in our project each layer will output a positive number (0 to infinity). In addition we added a dropout rate of 0.2 meaning that after each update to the layer, 20% of the nodes will be “dropped” or ignored for the next iteration making the network more resilient. Without a regularization technique models will often memorize the training data instead of interpolating within it. Next, we need to assign our model a loss and optimization function which regulates the learning aspect. Through several iterations or epochs, we can compute the loss by comparing the predicted output with the expected output and use an optimization function to decrease the error or loss. We calculated the loss by using the mean squared error loss function which takes the squared differences between true and predicted values. We could have alternatively used a Categorical Cross Entropy loss function which computes the entropy of our outputs, where a high entropy would imply that our model is uncertain as the probability of certainty is distributed equally among several outputs, and conversely a low entropy would mean that one output has a high amount of certainty suggesting a clear prediction. We found that these two loss functions equally performed well. To minimize the loss for more accurate predictions, a gradient descent optimization function will descend in the direction where the derivative of the loss function approaches 0, which represents a local or global minimum. For our optimizer function we used Adam that updates the trainable parameters allowing our model to eventually learn to predict the correct outputs after several epochs.

It's worth to note that our model is interpolating within the convex hull (any convex shape or an ellipsoid) rather than extrapolating outside the scope of the training data. When interpolation occurs a set of samples lies within the convex hull, and if not, extrapolation occurs. This is in part due to the fact that we are training a model on low dimensionality and so the probability that the testing data is within the convex hull is high. On the other hand if we were operating at high dimensions it would be evident that our model would extrapolate since the convex hull would exponentially decrease with respect to the number of dimensions. We can validate this by calculating the probability that a new sample will lie inside the convex hull by dividing the area of the convex hull by the total area of the enclosed space of all the points. Deriving a general equation to calculate the probability that new data points will lie inside the 2D convex hull in terms of the area and a vector (R2) of a new sample set, thereby implying interpolation, can be formulated as c/(x1−x0)2 where c is the convex hull area and x1 and x0 are data points in the vector. Hypothetically, if we were to increment to a three dimensional space by using matrices (R3 ) to simulate a 3-D convex hull shape, the generalized formula now becomes v/(x1−x0)3 where v is the volume of the convex shape and x1/x0 are the data points. This leads to the conclusion that the probability that new data points will lie inside the convex hull, thus interpolating, decreases exponentially as dimensionality increases. By taking the limit as dimension d approaches infinity it becomes apparent that the probability of new data points that lie within the convex hull will approach 0. Interpolation at high dimensions can still occur, though not likely as the number of training samples must grow exponentially with respect to the amount of dimensions. (Randall Balestriero, Jerome Pesenti, Yann LeCun, 2021) 
More on interpolating and extrapolating in high dimensions: https://arxiv.org/pdf/2110.09485.pdf

Why We Used a Neural Network: <br>
Although categorical classification based on data can be achieved from many different algorithms, we ultimately decided to choose a machine learning based approach because we wanted an algorithm that was capable of assigning custom parameters to certain features in the dataset that would have a greater weight or bias when generating the pod groups. In addition, we realized that going forward, our agent will become more powerful and the algorithm will become more accurate as the model can be re-trained end to end with training data of several orders of magnitude greater (past year pod group(s)). Lastly, with the recent popularity and hype that machine learning or more specifically, deep learning, has received, we thought this would be an interesting yet challenging approach to a relatively simple problem (one that could probably be achieved simply with conditional reasoning).

Moderating Results: <br>
Our algorithm also includes gender ratio and school standard deviation (std) optimization. We can consistently achieve about a 40% to 60% split between male to female for most if not all pod groups. We can also consistently achieve a school standard deviation of below 1 for most pods with several being 0 (perfectly spread out). We accomplished this by recursively checking the pods ahead and before to swap a student for a more balanced gender or school ratio. For example, if Pod 1 has 3 girls and 9 boys, and Pod 2 consists of 8 girls and 4 boys, the algorithm will swap a boy from Pod 1 to a girl from Pod 2 until Pod 1 has a 40-60% split or better. We implemented a similar approach for balancing the school STD, however this task proved to be more difficult as there are more variables to equate.

After running the model, all specifications can be found in the django admin interface where the pod group number, members, gender ratio, school standard deviation, and total number of students will be displayed for each pod.
