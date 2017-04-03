---
layout: post
title: "Traditional Classification Methods"
description: "Notes on machine learning from the book Python Machine Learning. The classification methods Perceptron, Adaline, Logistic Regression, SVM, Decision Tree and KNN are covered in this note."
date: 2017-02-26
tags: [Machine learning, Python, notes]
comments: true
share: true
---

## Classification


### Notations
$\textbf{x}$ represents the input data, independent variable matrix, where element $x_i^j$ is the $i$-th feature of the $j$-th sample, e.g. for 150 samples with 4 features, $\textbf{x}\in R^{150\times 4}$. And $\textbf{y}$ is a column vector representing the output.

### Perceptron
#### 1. Rosenblatt's perceptron


```python
from IPython.display import Image
Image(filename='02_perceptron.png',width=600)
```




![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_3_0.png){:height="200px" width="250px"}



Rosenblatt's perceptron first defines an **_activation function $\phi(z)$_** that takes a linear combination of certain input values $x$ and a corresponding weight vector $w$, 
$$z=w_1x_1+...+w_mx_m$$
where $z$ is called net input. Activation function is simply a function that transforms the net input into an output signal. In this case, the activation function is defined as the Heaviside step function:

$$
\begin{align*}
\phi(z)=&\quad 1 \quad\textit{ if } z\geq\theta; \\
&-1 \quad\textit{ otherwise}.
\end{align*}
$$

$\theta$ here is also a parameter (which means it needs to be learned, not pre-defined by the user) and for consistency, we put it as one of the weights, say $w_0$. Then,
$$z=w_0x_0+w_1x_1+...+w_mx_m,$$ where $x_0=1$ and 

$$
\begin{align*}
\phi(z)=&\quad 1 \quad\textit{ if } z\geq 0; \\
&-1 \quad\textit{ otherwise}.
\end{align*}
$$

To predict (predict the label of testing data), Rosenblatt's perceptron directly use the value of $\phi(\hat{z})$. However, in AdaLine (see below), there will be an additional _quantizer_, since $\phi(z)$ could be of other form, e.g. linear in Adaline. 

#### Weight updating procedure
1. Initialize the weights to 0 or small random numbers.
2. For each training sample $x^i$ perform the following steps:<br>
   1. Compute the ouput value $\hat{y}=\phi(\hat{z})$ by the current weight vector;
   2. Update the weights by
      $$w_j=w_j+\Delta w_j,$$
      where $$\Delta w_j=\eta(y^i-\hat{y}^i)x_j^i,$$
      and $\eta$ is user-defined and is called **_learning rate_**.<br>
      Observations: 1. If $\hat{y}^i=y^i$, the weight vector will not change; 2. If misclassification occurs, the weights are being pushed towards the direction of the positive (class 1) or negative (class -1) target class, respectively:
      
      $$
      \begin{align*}
      \Delta w_j&=\eta(1-(-1))x_j^i=2\eta x_j^i;\\
      \Delta w_j&=\eta(-1-(-1))x_j^i=-2\eta x_j^i.
      \end{align*}
      $$

#### Convergence
The convergence of Rosenblatt's perceptron is not guarateed if the classes are not linearly separable.

#### Computation speed

#### Training via scikit-learn

Perceptron object takes in training data $x$, $y$ and learning rate $\eta$ as inputs to calculate the weights $w_0$ and $w_1,...,w_m$.


```python
# Import data from dataIris.py.
from dataIris import X_train_std, X_test_std, y_train, y_test

from sklearn.linear_model import Perceptron
from sklearn.multiclass import OneVsOneClassifier
from sklearn.multiclass import OneVsRestClassifier

# Default multiclass classification in Perceptron is One-vs-Rest.
ppn = OneVsOneClassifier(Perceptron(eta0=0.1, random_state=0, n_iter=40))
ppn2 = OneVsRestClassifier(Perceptron(eta0=0.1, random_state=0, n_iter=40))
ppn.fit(X_train_std, y_train)  # Train the model.
ppn2.fit(X_train_std, y_train)
y_pred = ppn.predict(X_test_std)  # Predict the data.
y_pred2 = ppn2.predict(X_test_std)

# Output the number of prediction errors.
print (y_test != y_pred2).sum()
print (y_test != y_pred).sum()

# Plot the decision boundary
import numpy as np
%matplotlib inline
import matplotlib.pyplot as plt
from plot_decision_regions import plot_decision_regions

X_combined_std = np.vstack((X_train_std, X_test_std))
y_combined = np.hstack((y_train, y_test))

plot_decision_regions(X_combined_std, y_combined,
                      classifier=ppn, test_idx=range(105, 150))
plt.xlabel('petal length [standardized]')
plt.ylabel('petal width [standardized]')
plt.legend(loc='upper left')
plt.show()

```

    4
    1



![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_9_1.png =100x)


#### 2. Adaline (Adaptive linear neurons)


```python
Image(filename='02_Adaline.png',width=600)
```




![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_11_0.png)



Activation function now is $\phi(z)=\phi(w^Tx)=w^Tx$, where $w=(w_0,w_1,...w_m)'$ and quantizer (to do the prediction) is 

$$
\begin{align*}
\hat{y}^i=q(\phi(z^i))=&\quad 1 \quad\textit{ if } \phi(z^i)\geq 0; \\
&-1 \quad\textit{ otherwise}.
\end{align*}
$$

#### Optimal weights
To obtain the optimal weights, we minimize the **_cost function_**. In this case, it is Sum of Squared Errors(SSE):
$$J(w)=\frac{1}{2}\sum_i(y^i-\phi(z^i))^2.$$
#### Use gradient descent to update the weights
Update the weigts by taking a step away from the gradient $\nabla J(w)$:
$$w^{next}=w^{prev}-\eta\nabla J(w).$$

#### Convergence
Its convergence is guaranteed as the $J(w)$ in this case is convex w.r.t $w$.

#### Computation speed

### Logistic Regression


```python
Image(filename='03_logistic.png',width=600)
```




![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_17_0.png)



In logistic regression, $$z=logit(P(y|x))=\frac{P(y|x)}{1-P(y|x)}=w_0x_0+w_1x_1+...w_mx_m\in [0,+\infty).$$ The activation function is
$$\phi(z)=\frac{1}{1+e^{-z}}\in (0,1).$$ The quantizer is

$$
\begin{align*}
\hat{y}^i=q(\phi(z^i))=&\quad 1 \quad\textit{ if } \phi(z^i)\geq 1/2; \\
&-1 \quad\textit{ otherwise}.
\end{align*}
$$

Cost function is negative log-likelihood
$$J(w)=-\sum_i log(\phi(z^i))-(1-y^i)log(1-\phi(z^i)).$$

#### Use gradient descent to update the weights
Update the weigts by taking a step away from the gradient $\nabla J(w)$:
$$w^{next}=w^{prev}-\eta\nabla J(w).$$

#### Tackling overfitting via regulazation
Take $L_2$ penalty as an example,
$$J(w)=-\sum_i log(\phi(z^i))-(1-y^i)log(1-\phi(z^i))+\frac{\lambda}{2}||w||^2.$$
If let $C=1/\lambda$,
$$J(w)=C[-\sum_i log(\phi(z^i))-(1-y^i)log(1-\phi(z^i))]+\frac{1}{2}||w||^2.$$
Smaller C, larger penalty.

#### Training via scikit-learn

Logistic regression object takes in training data $x$, $y$, learning rate $\eta$ and penalty coefficient $C$ as inputs to calculate the weights $w_0$ and $w_1,...,w_m$.


```python
from sklearn.linear_model import LogisticRegression

lr = LogisticRegression(C=1000.0, random_state=0)
lr.fit(X_train_std, y_train)

plot_decision_regions(X_combined_std, y_combined,
                      classifier=lr, test_idx=range(105, 150))
plt.xlabel('petal length [standardized]')
plt.ylabel('petal width [standardized]')
plt.legend(loc='upper left')
plt.tight_layout()
# plt.savefig('./figures/logistic_regression.png', dpi=300)
plt.show()
```


![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_22_0.png)


### SVM
Instead of minimizing misclassification errors, SVM maximizes the margin by using hyperplane.


```python
Image(filename='03_SVM.png',width=600)
```




![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_24_0.png)



Every two parallel hyperplanes can be written as

$$
\begin{align*}
w_0+w^Tx&=1\\
w_0+w^Tx&=-1.
\end{align*}
$$

The margin between these two hyperplane is $$\frac{2}{||w||}.$$
Now the objective function of SVM becomes maximizing this margin under the constraint that the samples are classified correctly, i.e.,

$$
\begin{align*}
&\min \frac{1}{2}||w||^2\\
s.t. &w_0+w^Tx^i\geq 1 \quad if \quad y^i=1\\
&w_0+w^Tx^i\leq -1 \quad if \quad y^i=-1,
\end{align*}
$$

where the constraints are equivalent to
$$y^i(w_0+w^Tx^i)\geq 1 \quad \forall i.$$
It is a quadratic programming problem. The dual problem of this primal problem is

$$
\begin{align*}
&\max \sum_i \lambda_i-\frac{1}{2}\sum_{i,j}\lambda_i \lambda_j y^i y^j x^{i'}\cdot x^j\\
s.t. &\lambda_i\geq 0\\
&\sum_i\lambda_iy^i=0.
\end{align*}
$$

It is still a QP problem and strong duality always holds for QP, so the KKT conditions are satisfied. And according to the complementary slackness,
$$\lambda_i(1-y^i(w_0+w^Tx))=0.$$ Therefore, when $y^i(w_0+w^Tx)>1$, $\lambda_i=0$. And when $y^i(w_0+w^Tx)=1$, $\lambda_i\neq 0$ and these vectors are called **_support vectors_** which lie on the hyperplanes.
And according to gradient of Lagrangian with respect to x vanishes(KKT), optimal $w$ satisfies
$$w*=\sum_i\lambda_i y^ix^i.$$ Also, $\lambda_i=0$ if it is not support vector.

To predict, we calculate whether $$w'x+w_0=\sum_i\lambda_i x^{i'}x+w_0=\sum_{\{support\}}\lambda_i x^{i'}x+w_0$$
is greater 0 or not.

#### Dealing with the nonlinearly separable case using slack variables
The _positive-values_ slack variable is simply added to the linear constraints:

$$
\begin{align*}
&w_0+w^Tx^i\geq 1 \quad if \quad y^i=1-\xi^i\\
&w_0+w^Tx^i\leq -1 \quad if \quad y^i=-1+\xi^i,
\end{align*}
$$

so the new problem is to minimize 
$$\frac{1}{2}||w||^2+C(\sum_i\xi^i),$$
where $C$ is user defined. The problem is still a QP.

#### Kernel SVM
Project the data onto a higher dimensional space via a mapping function $m(\cdot)$ and hope it is linearly separable after mapping.

The motivation of using kernel function is to avoid calculating the $m(x)$ explicitly because of computational issue. And based on the observation that in the dual problem, what we need is only $x^{i'}\cdot x^j,$ so, if we directly know $m(x^{i'})\cdot m(x^j),$ we could avoid calculating $m(x)$. That is why kernel is defined as
$$k(x^i,x^j)=m(x^i)\cdot m(x^j).$$
And kernel measures the similarity of two vectors.

One of the most widely used kernels is the Radial Basis Function kernel or Gaussian kernel:
$$k(x^i,x^j)=\exp(-\frac{||x^i-x^j||^2}{2\sigma^2}).$$
In sklearn package, $\gamma=1/2\sigma^2$.

Recall the decision rule uses
$$w^Tx+w_0=\sum_{\{support\}}\lambda_iy^ik(x_i,x)+w_0,$$
if $\sigma$ is smaller, kernel function is small, the similarity is smaller, so the impact of support vectors on test data nearby is smaller(since they treat them less similar), then the boundary will be more close to the support vectors. 



```python
Image(filename='03_gaussiankernel.png',width=600)
```




![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_28_0.png)



#### Training via scikit-learn

SVM object takes in training data $x$, $y$, kernel function and penalty coefficient $C$ as inputs.


```python
from sklearn.svm import SVC

# linear kernel here; default is rbf
svm = SVC(kernel='linear', C=1.0, random_state=0)
svm.fit(X_train_std, y_train)
plot_decision_regions(X_combined_std, y_combined,
                      classifier=svm, test_idx=range(105, 150))
plt.xlabel('petal length [standardized]')
plt.ylabel('petal width [standardized]')
plt.legend(loc='upper left')
plt.tight_layout()
plt.show()
```


![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_30_0.png)


### Decision Tree



```python
Image(filename='03_decision.png',width=600)
```




![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_32_0.png)



The objective function is to maximize the information gain at each split, which we define as follows:
$$IG(D_p,f)=I(D_p)-\sum_{j=1}^m \frac{N_j}{N_p}I(D_j).$$
Here, $f$ is the rule of split, $D_p$ and $D_j$ are the dataset of the parent and $j$th child node, $I$ is **_impurity_** measure(to be defined), $N_p$ is the total number of samples at the parent node, and $N_j$ is the number of samples in the child node. Usually, split into two nodes at each node.
The rule $f$ in decision tree is alywas of the form $x_j<C_j$ for quantatitive, and whether $x_j=Group_j$ for qualified variable.

Typical impurity measure: entropy ($I_H$), Gini index($I_G$)...
$$I_H(node)=-\sum_{i=1}^c p(i|node)\log_2 p(i|node),$$
where $p(i|node)$ is the proportion of the samples that belongs to class $i$ for a particular node.

#### Training via scikit-learn

Decision tree object takes in training data $x$, $y$, impurity measure and maximum depth (avoid overfitting) as inputs.


```python
from sklearn.tree import DecisionTreeClassifier
tree = DecisionTreeClassifier(criterion='entropy', max_depth=7, random_state=0)
tree.fit(X_train_std, y_train)
plot_decision_regions(X_combined_std, y_combined,
                      classifier=tree, test_idx=range(105, 150))
plt.xlabel('petal length [standardized]')
plt.ylabel('petal width [standardized]')
plt.legend(loc='upper left')
plt.tight_layout()
plt.show()
```


![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_35_0.png)


### K-nearest neighbors (KNN)
Look at the k nearest neighbors and assign the class label by majority vote.

#### Training via scikit-learn

KNN object takes in training data $x$, $y$, k and metric of distance as inputs.


```python
from sklearn.neighbors import KNeighborsClassifier

knn = KNeighborsClassifier(n_neighbors=5, p=2, metric='minkowski')
knn.fit(X_train_std, y_train)
plot_decision_regions(X_combined_std, y_combined,
                      classifier=knn, test_idx=range(105, 150))
plt.xlabel('petal length [standardized]')
plt.ylabel('petal width [standardized]')
plt.legend(loc='upper left')
plt.tight_layout()
plt.show()
```


![png](https://scuiaa555.github.io/assets/images/Classification_files/Classification_38_0.png)

