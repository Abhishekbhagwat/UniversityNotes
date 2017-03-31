# Bayesian Decision Theory

## Bayesian Classifiers

In many applications, the relationship between the input features and output labels is non-deterministic (i.e. uncertain). Bayesian classifiers aim to model probabilistic relationships between the input features and output class.

## Probability Concepts

Let $X$ and $Y$ be random variables.

**Marginal probability** refers to the probability that $Y$ will take on the value $y$.

$$
P(Y = y)
$$

$$
\sum_{y_i} P(Y = y_i) = 1
$$

**Joint probability** refers to the probability that variable $X$ will take on the value $x$ and variable $Y$ will take on the value $y$.

$$
P(X = x, Y = y)
$$

**Conditional probability** refers to the probability that the variable $Y$ will take on the value $y$, given that the variable $X$ is observed to have the value $x$.

$$
P(Y = y | X = x)
$$

$$
\sum_{y_i} P(Y = y_i | X = x) = 1
$$

**Sum Rule**

$$
P(X = x) = \sum_{y_i} P(X = x , Y = y_i)
$$

$$
P(X) = \sum_Y P(X, Y)
$$

$$
P(X = x) = \sum_{z_j} \sum_{y_i} P(X = x , Y = y_i, Z = z_j)
$$

$$
P(X) = \sum_Z \sum_Y P(X, Y, Z)
$$

**Product Rule**

$$
\begin{split}
P(X = x, Y = y) &= P(Y = y | X = x) \times P(X = x) \\
                &= P(X = x | Y = y) \times P(Y = y)
\end{split}
$$

$$
P(X, Y) = P(Y | X) \times P(X) = P(X | Y) \times P(Y)
$$

### Decision with Priors

> A decision rule prescribes what action to take based on observed input.

Predict $Y = y_i$ if:

$$
P(Y = y_i) = \max_{k} P(Y = y_k)
$$

## Bayes Theorem

$$
P(Y | X) = \frac{P(X | Y) P(Y)}{P(X)}
$$

- $P(Y | X)$: Posterior Probability
- $P(X | Y)$: Likelihood
- $P(Y)$: Prior Probability
- $P(X)$: Evidence

### General Bayesian Classifier

Predict $Y = y_i$ if:

$$
P(Y = y_i | X = x) = \max_{k} P(Y = y_k | X = x)
$$

Applying Bayes Theorem:

$$
P(Y = y_i | X = x) = \frac{P(X = x | Y = y_i)P(Y = y_i)}{P(X = x)}
$$

Since $P(X = x)$ is constant w.r.t to different values of $Y$, predict $Y = y_i$ if:

$$
P(X | Y = y_i) P(Y = y_i) = \max_{k} P(X | Y = y_k)P(Y = y_k)
$$

## Losses & Risks

So far, the decisions are assumed to be equally good or costly. However, the cost of misclassification for different classes may be different. For example, misdiagnosing a healthy patient as ill versus misdiagnosing an ill patient as healthy.

### Formulae

- Let $a_i$ be the action of predicting $Y = y_i$.
- Let $\lambda_{ik}$ be the loss of $a_i$ when the class value is actually $y_k$.

**Expected Risk for Action $a_i$:**

$$
R(a_i | X) = \sum^{K}_{k = 1} \lambda_{ik} P(Y = y_k | X)
$$

**Choose $a_i$ if:**

$$
R(a_i | X) = \min_{k} R(a_k | X)
$$

### 0/1 Loss

$$
\lambda_{ik} =
\begin{cases}
0 & i = k \\
1 & i \ne k
\end{cases}
$$

If all correct decisions have no loss and all errors are equally costly:

$$
\begin{split}
R(a_i | X) &= \sum_{k = 1}^K \lambda_{ik} P(Y = y_k | X) \\
           &= \sum_{k \ne i} P(Y = y_k | X) \\
           &= 1 - P(Y = y_i | X) \\
\end{split}
$$

### Reject or Doubt

If the automatic system has low certainty of its decision and making a wrong decision may have very high cost, a manual decision is required.

The solution is to define an additional action for *reject* or *doubt*, $a_{K + 1}$, with $a_i$, $i = 1, ... , K$, being the usual actions of predicting on classes $y_i$.

The revised 0/1 loss function is now:

$$
\lambda_{ik} =
\begin{cases}
0 & i = k \\
\theta & i = K + 1 \\
1 & i \ne k
\end{cases}
$$

where $0 < \theta < 1$ is the loss incurred for choosing the reject action.

The expected risk of taking the reject action:

$$
R(a_{K + 1} | X) = \sum_{k = 1}^K \theta P(Y = y_k | X) = \lambda_{[K + 1][k]} = \theta
$$

### Optimal Decision Rule

The following rules are meaningful if $0 < \theta < 1$. If $\theta = 0$, we will always reject and if $\theta \geq 1$, we will never reject.

#### Decision Rule 1

Take action $a_i$ if the following conditions are true:

$$
R(a_{i} | X) < R(a_{k} | X), \forall k | k \ne i
$$

$$
R(a_{i} | X) < R(a_{K + 1} | X)
$$

Choose reject if:

$$
R(a_{K + 1} | X) < R(a_{i} | X), i = 1, ... , K
$$

#### Decision Rule 2

Predict $Y = y_i$ if the following conditions are true:

$$
P(Y = y_i | X) > R(Y = y_k | X), \forall k | k \ne i
$$

$$
P(Y = y_i | X) > 1 - \theta
$$

Choose reject otherwise.