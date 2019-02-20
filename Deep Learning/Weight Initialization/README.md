# Weight Initialization
**Calibrating the variances with 1/sqrt(n)**. One problem with the above suggestion is that the distribution of the outputs from a randomly initialized neuron has a variance that grows with the number of inputs. It turns out that we can normalize the variance of each neuron’s output to 1 by scaling its weight vector by the square root of its fan-in (i.e. its number of inputs). That is, the recommended heuristic is to initialize each neuron’s weight vector as: `w = np.random.randn(n) / sqrt(n)`, where `n` is the number of its inputs. This ensures that all neurons in the network initially have approximately the same output distribution and empirically improves the rate of convergence.

The sketch of the derivation is as follows: Consider the inner product ![Equation1](./images/1.png) between the weights `w` and input `x`, which gives the raw activation of a neuron before the non-linearity. We can examine the variance of `s`:

<p align="center">
    <img src="./images/2.png">
    <img src="./images/3.png">
    <img src="./images/5.png">
</p>

Equation step 2 understand: 
<p align="center">
    <img src="./images/4.png">
</p>