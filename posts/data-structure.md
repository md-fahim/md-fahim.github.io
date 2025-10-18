Here's your entire content rewritten in **Markdown** format, preserving **all information** and using proper formatting conventions (headings, math, callouts, etc.):

---

# Generative AI

Generative AI (GenAI) represents a fundamental branch of machine learning centered on **generative modeling**, which aims to understand and replicate the process by which data is produced. This contrasts with **discriminative modeling**, which directly learns a mapping from inputs (e.g., images) to outputs (e.g., labels).

While discriminative models focus on prediction, generative models seek to learn the **joint probability distribution** of all variables in a system — essentially, modeling how the world creates the data we observe.

The core concepts in GenAI are described as follows:

## i. Generative vs. Discriminative Modeling

* **Generative Modeling** attempts to simulate how real-world data is generated, often capturing underlying causal and physical processes.

* **Discriminative Modeling** learns a direct mapping from inputs to outputs for prediction tasks and is typically more accurate when ample labeled data is available.

Generative modeling reflects how most scientific disciplines approach understanding phenomena — by building models that describe causal or physical processes (e.g., weather prediction, galaxy formation).

## ii. Advantages of Generative Modeling

* **Causality & Generalization**: Generative models capture causal relationships that generalize better across contexts (e.g., understanding earthquakes in different locations).

* **Intuition & Interpretability**: Such models are often more interpretable, as they reflect how systems behave rather than just correlating variables.

* **Low-Data Regimes**: Generative models shine when labeled data is scarce — for example, in semi-supervised learning where a few labeled examples are supported by many unlabeled ones.

---

# Generative AI and Deep Learning

The primary goal of Generative AI (GenAI) is to create **realistic images**. If it were possible to store every conceivable image, generating a desired image could be as simple as selecting one that fits specific constraints — for instance, a dog behind a tree, a dog running on the street, or a dog in front of a building.

However, this approach is impractical, as it's impossible to store all possible variations of images.

**How can we overcome this issue?**
GenAI aims to **learn patterns from existing data** and generate **new, realistic images** that have never been seen before, based on those learned patterns.

It all starts with the unknown real data distribution, denoted as:
$p_{\text{data}}(x)$

We assume the observed variable ( x ) is drawn from an unknown true distribution ( p^*(x) ). We aim to approximate this with a parameterized model ( p_\theta(x) ):

```math
x \sim p_\theta(x)
```

Learning involves finding parameters ( \theta ) such that:

```math
p_\theta(x) \approx p^*(x)
```

The model ( p_\theta(x) ) should be flexible enough to fit the data well, while also allowing incorporation of prior knowledge about the data distribution.

> ### 📦 **Parameterizing Conditional Distributions with Neural Networks**
>
> In many tasks like classification or regression, we aim to learn a conditional model ( p_\theta(y|x) ) that approximates the true conditional distribution ( p^*(y|x) ):
>
> ```math
> p_\theta(y|x) \approx p^*(y|x)
> ```
>
> Here, ( x ) is the input (e.g., an image), and ( y ) is the output (e.g., a class label). A common example is image classification, where ( p_\theta(y|x) ) is modeled as a **categorical distribution** whose parameters depend on ( x ).
>
> In neural network-based models:
>
> ```math
> p = \text{NeuralNet}(x) \quad \text{and} \quad p_\theta(y|x) = \text{Categorical}(y; p)
> ```
>
> The output vector ( p ) is typically obtained via a `softmax` layer, ensuring ( \sum_i p_i = 1 ).

---

## Learning in Fully Observed Models

When all variables in a directed graphical model are observed, we can directly compute and differentiate the **log-likelihood** of the data, enabling efficient optimization.

We assume a dataset
( \mathcal{D} = {x^{(i)}}_{i=1}^{N} \equiv x^{(1:N)} )
of ( N ) i.i.d. samples from an unknown distribution. The model assigns a probability to the dataset as:

```math
\log p_\theta(\mathcal{D}) = \sum_{x \in \mathcal{D}} \log p_\theta(x)
```

We optimize parameters ( \theta ) via **Maximum Likelihood Estimation (MLE)**, aiming to maximize:

```math
\max_\theta \ \log p_\theta(\mathcal{D})
```

Gradients ( \nabla_\theta \log p_\theta(\mathcal{D}) ) are computed using **automatic differentiation**.

When computed over the full dataset, this is **batch gradient descent** — which can be expensive for large ( N ).

To reduce cost, we generally use **Stochastic Gradient Descent (SGD)** with minibatches
( \mathcal{M} \subset \mathcal{D} ), with ( |\mathcal{M}| = N_M ), forming an **unbiased estimator**:

```math
\frac{1}{N} \log p_\theta(\mathcal{D}) \approx \frac{1}{N_M} \sum_{x \in \mathcal{M}} \log p_\theta(x)
```

The corresponding gradient estimate is:

```math
\frac{1}{N} \nabla_\theta \log p_\theta(\mathcal{D}) \approx \frac{1}{N_M} \sum_{x \in \mathcal{M}} \nabla_\theta \log p_\theta(x)
```

These **stochastic gradients** are used to update ( \theta ) in iterative optimization methods.

---

## Latent Variable Models

Imagine you're training a model to understand images. You provide it with countless examples — dogs in parks, cats on couches — but no matter how big the dataset, it can't cover every possible scenario.

To truly generalize, the model needs to capture patterns that go **beyond** what it has seen.

That’s where **latent variables** come in. These hidden variables, usually denoted by ( z ), represent the underlying factors — like lighting, pose, or style — that shape the data, even though they're not labeled or directly observed.

> How are **latents** different from **features**?
>
> * **Features** are typically interpretable and derived from the data — like edges, textures, or shapes.
> * **Latents** are abstract and not necessarily human-interpretable. They’re internal representations the model uses to organize and make sense of complex data.
>
> In essence:
> **Features help us see, while latents help the model understand** — enabling it to infer, imagine, and generate what it hasn’t explicitly seen.

In **unconditional modeling**, the joint distribution over observed ( x ) and latent ( z ) is:

```math
p_\theta(x, z)
```

The marginal distribution over ( x ), also called the **marginal likelihood** or **model evidence**, is:

```math
p_\theta(x) = \int p_\theta(x, z) \, dz
```

This marginal ( p_\theta(x) ) can represent rich distributions. For example:

* If ( z ) is discrete and ( p_\theta(x|z) ) is Gaussian or a mixture of Gaussians.
* If ( z ) is continuous, it forms an **infinite mixture** — also called a **compound distribution**.

A **Deep Latent Variable Model (DLVM)** uses neural networks to parameterize components of ( p_\theta(x, z) ), optionally conditioned on context ( y ):

```math
p_\theta(x, z \mid y)
```

Even if each component (e.g., prior or conditional) is simple (like Gaussian), the resulting marginal ( p_\theta(x) ) can be highly expressive.

A common DLVM factorizes the joint distribution as:

```math
p_\theta(x, z) = p_\theta(z) \cdot p_\theta(x|z)
```

Here:

* ( p_\theta(z) ) is the **prior** over latent variables.
* ( p_\theta(x|z) ) is the **likelihood** of data given latent variables.

This structure allows flexible modeling of complex data distributions ( p^*(x) ).

---

## Challenges in DLVM: Intractabilities

Deep Latent Variable Models (DLVMs) are powerful, but training them isn't always straightforward. The main challenge lies in the **intractability** of computing the **marginal likelihood** of the data ( p_\theta(x) ).

Recall:

```math
p_\theta(x) = \int p_\theta(x, z) \, dz
```

This integral marginalizes out the latent variable ( z ), giving us the likelihood of ( x ) under the model.

However, in most DLVMs, this integral **doesn't have a closed-form solution**, nor can it be computed efficiently. This makes it hard to optimize ( p_\theta(x) ) directly — unlike in fully observed models where we


can differentiate the log-likelihood easily.

But the problem runs deeper.

This intractability also affects the **posterior distribution** over the latent variable:

```math
p_\theta(z|x) = \frac{p_\theta(x, z)}{p_\theta(x)}
```

* The numerator ( p_\theta(x, z) ) is usually tractable.
* The denominator ( p_\theta(x) ) is intractable (as seen above), making the **posterior ( p_\theta(z|x) )** also intractable.

And yet, learning from data often **requires** computing or approximating this posterior.

### So, what do we do?

We turn to **approximate inference methods**.

These methods help us estimate both ( p_\theta(x) ) and ( p_\theta(z|x) ) without computing them exactly. However:

* Traditional inference approaches can be **slow or inaccurate**.
* They often require **optimization for each individual data point**.
* They can produce **poor-quality posterior approximations**.

The challenge doesn’t stop at latent variables.

In **Bayesian** treatments of DLVMs, the posterior over the model parameters themselves, ( p(\theta | \mathcal{D}) ), is **also intractable**. This again forces us to use **approximation methods** to make learning feasible.

---

Let me know if you’d like this content converted to another format (e.g., HTML, LaTeX, or PDF), or if you'd like diagram suggestions or visual enhancements.
