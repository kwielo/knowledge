# Machine Learning

A highly opinionated ML cheat sheet.

## Getting Started

* Deep Learning Demystified, by [Brandon Rohrer](https://brohrer.github.io/):
    * [How Neural Networks work?](https://brohrer.github.io/deep_learning_demystified.html)
    * [How Convolutional Neural Networks work?](https://brohrer.github.io/how_convolutional_neural_networks_work.html)
* A Neural Network in 15 lines of Python, by [Andrew Trask](https://twitter.com/iamtrask):
    * [basic Neural Network in 11 lines](http://iamtrask.github.io/2015/07/12/basic-python-network/)
    * [adding Gradient Descent Optimizer](http://iamtrask.github.io/2015/07/27/python-network-part2/)
    * [adding Hinston's Dropout](https://iamtrask.github.io/2015/07/28/dropout/)
* Understanding the maths behind Deep Learning, with code implementations:
    * [A Gentle Introduction to Artificial Neural Networks](https://theclevermachine.wordpress.com/2014/09/11/a-gentle-introduction-to-artificial-neural-networks/) by [Dustin Stansbury](https://twitter.com/corrcoef)
    * Machine Learning, by [Andrew Gibiansky](http://andrew.gibiansky.com/):
        * [The bascics](http://andrew.gibiansky.com/blog/machine-learning/machine-learning-the-basics/)
        * [Neural Networks](http://andrew.gibiansky.com/blog/machine-learning/machine-learning-neural-networks/)
        * [Convolutional Neural Networks](http://andrew.gibiansky.com/blog/machine-learning/convolutional-neural-networks/)
        * [Recurrent Neural Networks](http://andrew.gibiansky.com/blog/machine-learning/recurrent-neural-networks/)
    * [Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/index.html) by [Michael Nielsen](https://twitter.com/michael_nielsen)
* Gentlest Introduction to Tensorflow, by [Soon Hin Khor](https://twitter.com/neth_6):
    * [Linear regression for single feature single outcome model](https://medium.com/all-of-us-are-belong-to-machines/the-gentlest-introduction-to-tensorflow-248dc871a224#.pbfs8sxmz)
    * [Training illustrated in diagrams/code, and exploring training variations](https://medium.com/all-of-us-are-belong-to-machines/gentlest-introduction-to-tensorflow-part-2-ed2a0a7a624f#.eerdfyjcs)
    * [Matrices and multi-feature linear regression](https://medium.com/all-of-us-are-belong-to-machines/gentlest-intro-to-tensorflow-part-3-matrices-multi-feature-linear-regression-30a81ebaaa6c#.1c6z3z79z)
* Practical Examples:
    * [Qualifying sales leads](https://medium.com/xeneta/boosting-sales-with-machine-learning-fbcf2e618be3#.192b2lj98), by [Per Harald Borgen](https://twitter.com/perborgen)
    * [Deep Learning for RegEx](http://dlacombejr.github.io/2016/11/13/deep-learning-for-regex.html)

## Regression

### Neural Network

Neural Networks can approximate any function, by applying an "activation" function to a bias plus the weighted sum of inputs: `y = σ(b + Σwx)`.

Since for any probability problem there exists a function to calculate it, Neural Networks can be used to automatically
find that function using meaningful "training" data and a "learning" algorithm such as the Stochastic Gradient Descent:

1. initialize weights and biases
2. calculate a prediction for the given training input
3. calculate the pediction error, using the expected output from the training data
3. calculate the Gradient of the error
4. adjust weights and biases with the gradient, then repeat from step 2 for a given number of epochs

#### 1. Weights/Biases Initialization

Depending on the chosen activation function, initialize weights and biases as follow:

* for sigmoid (standard, tanh): Gaussian distribution with mean 0 and standard deviation:
  * 1 for each bias (so it's picked randomly between -1 and 1 with mean 0)
  * 1 / √n for each weight with n being the number of neurons in the layer
    (using a standard deviation of 1 with a high number of neurons result with a saturated layers,
    and saturated layers are slow learners)
* for the rest (softmax, ReLU, etc): all weights and biases set to 0

> **Tip**: For debugging, seed random numbers to make calculation deterministic.

See: [Weight Initialization](http://neuralnetworksanddeeplearning.com/chap3.html#weight_initialization)

#### 2. Activation Function

For regression problems, a standard sigmoid function (also known as logistic function) can be used for activation:
`sigmoid(y) = 1 / 1 + exp(-y)`, it ranges from 0 (saturates near -6) to 1 (saturates near 6).

Sometimes the Hyperbolic Tangent (tanh) function performs better: `tanh(y) = (exp(y) - exp(-y)) / (exp(y) + exp(-y))`,
it ranges from -1 (saturates near -3) to 1 (saturates near 3).

Rectified Linear Unit (ReLU) often improve results when used on image recognition problems: `relu(y) = max(0, y)`,
it returns 0 for negative sum of weighted input, otherwise it returns the sum of weighted input.

Finally the softmax function is used in classification problems, where there are many outputs:
`softmax(yj) = exp(zj) / Σexp(zk)`. The output of a neuron `j` is the exponential function of its sum of weighted input,
divided by the sum of all the output neurons. This means the total of all output neurons will always be 1.

References:

* [Sigmoid neurons](http://neuralnetworksanddeeplearning.com/chap1.html#sigmoid_neurons)
* [softmax](http://neuralnetworksanddeeplearning.com/chap3.html#softmax)
* [Other models of artificial neuron](http://neuralnetworksanddeeplearning.com/chap3.html#other_models_of_artificial_neuron)

#### 3. Error Function

For regression problems, the cross entropy cost function is usually used.

> **Note**: The quadratic cost (aka Mean Squarred Error) function is often used in examples: as it is easier to learn
> than the cross entropy, however it isn't as efficient.

For classification problems, the log-likelihood function is used.

References:

* [Learning with gradient](http://neuralnetworksanddeeplearning.com/chap1.html#learning_with_gradient_descent)
* [The cross entropy function](http://neuralnetworksanddeeplearning.com/chap3.html#the_cross-entropy_cost_function)
* [Softmax and log-likelihood](http://neuralnetworksanddeeplearning.com/chap3.html#softmax)

#### 4. Gradient

#### 5. Epochs

#### Data

In order to train our network, we need examples that map input to an expected output. We can split it in 3 sets:

1. Training set: 70% of the examples,
   the mean error will be used with the Gradient to modify parameters (weights and biases)
2. Validating set: 15% of the examples,
   the mean error will be used to stop the training once saturated,
   it's purpose is to evaluate hyper parameters and prevent overfitting on training data
3. Testing set: 15% of the examples,
   the mean error will be used to evaluate parameters (weights and biases) and prevent overfitting on testing data

#### Overfitting

Overfitting is when our model optimizes for our training data but fails to be useful for new unknown data.
To prevent this from happening, we can:

* increase the amount of training data
* use L2 regularlization
* use L1 regularlization
* use dropout

## Classification

### Bag of Words Model

Assuming the occurence of each words can be used as a feature for training a classifier.

#### How it works

1. fitting: "learn" vocabulary by extracting each words from all sentences
2. transforming: extract vocabulary word count for each sentence
3. optionally keep only the most x used words

#### Reference

[Bag of Words for beginners](https://www.kaggle.com/c/word2vec-nlp-tutorial/details/part-1-for-beginners-bag-of-words)
by [Kaggle](https://www.kaggle.com/)

### Naive Bayes Classifier

Assuming that a statement probability of being of a certain type depends if this type is more likely
and if the statement contains more words of this type.

#### How it works

1. "learning" types:
   * count the total number of documents
   * count how many times a word from a statement occurs for a given type
2. "guessing" types:
   * calculate type probability: count of documents of this type / total count of documents
   * calculate statement type probability: for each words in the statements
     * calculate word type probability: count occurence of this word for this type / total number of words
     * multiply all word type probabilities
   * calculate likelihood: multiply type probability by statement type probability
   * the statement is likely being of the type that has the highest likelihood

#### Reference

[Machine Learning: Naive Bayes](https://stovepipe.systems/post/machine-learning-naive-bayes),
by [Yannick de Lange](https://twitter.com/yannickl88)

### Random Forest Classifier

#### How it works

1. "training":
   * take a subset (~66%) of the data
   * for each node:
     - take randomly _m_ items from the subset
     - pick the item that provides the best split and use it to do a binary split on that node
     - for the next node, do the same with another _m_ random items from the subset
2. "running":
   * run input down all the trees
   * take the average, or weighted average, or voting majority of all the results

> _m_ can be either (your choice):
>
> * 1 (random splitter selection)
> * total number of items (breiman’s bagger)
> * something in between, e.g. ½√m, √m, and 2√m (random forest)

#### Reference

[A Gentle Introduction to Random Forests](https://citizennet.com/blog/2012/11/10/random-forests-ensembles-and-performance-metrics/),
by [Dan Benyamin](https://twitter.com/dbenyamin)
