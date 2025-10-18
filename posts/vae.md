# Generative AI

Generative AI (GenAI) represents a fundamental branch of machine learning centered on generative modeling, which aims to understand and replicate the process by which data is produced. This contrasts with discriminative modeling, which directly learns a mapping from inputs (e.g., images) to outputs (e.g., labels). While discriminative models focus on prediction, generative models seek to learn the joint probability distribution of all variables in a system — essentially, modeling how the world creates the data we observe. The core concepts in GenAI are described as follows:

## i. Generative vs. Discriminative Modeling

**Generative Modeling** attempts to simulate how real-world data is generated, often capturing underlying causal and physical processes.

**Discriminative Modeling** learns a direct mapping from inputs to outputs for prediction tasks and is typically more accurate when ample labeled data is available.

Generative modeling reflects how most scientific disciplines approach understanding phenomena — by building models that describe causal or physical processes (e.g., weather prediction, galaxy formation).

## ii. Advantages of Generative Modeling

**Causality & Generalization:** Generative models capture causal relationships that generalize better across contexts (e.g., understanding earthquakes in different locations).

**Intuition & Interpretability:** Such models are often more interpretable, as they reflect how systems behave rather than just correlating variables.

**Low-Data Regimes:** Generative models shine when labeled data is scarce — for example, in semi-supervised learning where a few labeled examples are supported by many unlabeled ones.

# Generative AI and Deep Learning

The primary goal of Generative AI (GenAI) is to create realistic images. If it were possible to store every conceivable image, generating a desired image could be as simple as selecting one that fits specific constraints—for instance, a dog behind a tree, a dog running on the street, or a dog in front of a building. However, this approach is impractical, as it's impossible to store all possible variations of images. How can we overcome this issue? GenAI aims to learn patterns from existing data and generate new, realistic images that have never been seen before, based on those learned patterns.

It all starts with the unknown real data distribution, denoted as: $p_{\text{data}}(x)$. We assume the observed variable $x$ is drawn from an unknown true distribution $p^*(x)$. We aim to approximate this with a parameterized model $p_\theta(x)$:

$$x \sim p_\theta(x)$$

Learning involves finding parameters $\theta$ such that:

$$p_\theta(x) \approx p^*(x)$$

The model $p_\theta(x)$ should be flexible enough to fit the data well, while also allowing incorporation of prior knowledge about the data distribution.

---

### Parameterizing Conditional Distributions with Neural Networks

In many tasks like classification or regression, we aim to learn a conditional model $p_\theta(y|x)$ that approximates the true conditional distribution $p^*(y|x)$:

$$p_\theta(y|x) \approx p^*(y|x)$$

Here, $x$ is the input (e.g., an image), and $y$ is the output (e.g., a class label). A common example is image classification, where $p_\theta(y|x)$ is modeled as a categorical distribution whose parameters depend on $x$. In neural network-based models:

$$p = \text{NeuralNet}(x) \quad p_\theta(y|x) = \text{Categorical}(y; p)$$

The output vector $p$ is typically obtained via a `softmax` layer, ensuring $\sum_i p_i = 1$.

---

## Learning in Fully Observed Models

When all variables in a directed graphical model are observed, we can directly compute and differentiate the log-likelihood of the data, enabling efficient optimization.

We assume a dataset $\mathcal{D} = \{x^{(i)}\}_{i=1}^{N} \equiv x^{(1:N)}$ of $N$ i.i.d. samples from an unknown distribution. The model assigns a probability to the dataset as:

$$\log p_\theta(\mathcal{D}) = \sum_{x \in \mathcal{D}} \log p_\theta(x)$$

We optimize parameters $\theta$ via Maximum Likelihood (MLE), aiming to maximize:

$$\max_\theta \ \log p_\theta(\mathcal{D})$$

Gradients $\nabla_\theta \log p_\theta(\mathcal{D})$ are computed using automatic differentiation. When computed over the full dataset, this is **batch gradient descent**—which can be expensive for large $N$.

To reduce cost, we generally use SGD with minibatches $\mathcal{M} \subset \mathcal{D}$, $|\mathcal{M}| = N_M$, forming an unbiased estimator:

$$\frac{1}{N} \log p_\theta(\mathcal{D}) \approx \frac{1}{N_M} \sum_{x \in \mathcal{M}} \log p_\theta(x)$$

The corresponding gradient estimate is:

$$\frac{1}{N} \nabla_\theta \log p_\theta(\mathcal{D}) \approx \frac{1}{N_M} \sum_{x \in \mathcal{M}} \nabla_\theta \log p_\theta(x)$$

These stochastic gradients are used to update $\theta$ in iterative optimization methods.

## Latent Variable Models

Imagine you're training a model to understand images. You provide it with countless examples — dogs in parks, cats on couches — but no matter how big the dataset, it can't cover every possible scenario. To truly generalize, the model needs to capture patterns that go beyond what it has seen.

That's where latent variables come in. These hidden variables, usually denoted by $z$, represent the underlying factors — like lighting, pose, or style — that shape the data, even though they're not labeled or directly observed.

Now, how are latents different from features? Features are typically interpretable and derived from the data — like edges, textures, or shapes. In contrast, latents are abstract and not necessarily human-interpretable. They're internal representations the model uses to organize and make sense of complex data. In essence, features help us see, while latents help the model understand — enabling it to infer, imagine, and generate what it hasn't explicitly seen.

In unconditional modeling, the joint distribution over observed $x$ and latent $z$ is:

$$p_\theta(x, z)$$

The marginal distribution over $x$, also called the *marginal likelihood* or *model evidence*, is:

$$p_\theta(x) = \int p_\theta(x, z) \, dz$$

This marginal $p_\theta(x)$ can represent rich distributions. For example:
- If $z$ is discrete and $p_\theta(x|z)$ is Gaussian or mixture of Gaussians.
- If $z$ is continuous it will be infinite mixture, also called a compound distribution.

A Deep Latent Variable Model (DLVM) uses neural networks to parameterize components of $p_\theta(x, z)$, optionally conditioned on context $y$:

$$p_\theta(x, z \mid y)$$

Even if each component (e.g., prior or conditional) is simple (like Gaussian), the resulting marginal $p_\theta(x)$ can be highly expressive.

A common DLVM factorizes the joint distribution as:

$$p_\theta(x, z) = p_\theta(z) \, p_\theta(x|z)$$

Here $p_\theta(z)$ is the prior over latent variables and $p_\theta(x|z)$ is the likelihood of data given latent variables. This structure allows flexible modeling of complex data distributions $p^*(x)$.

## Challenges in DLVM: Intractabilities

Deep Latent Variable Models (DLVMs) are powerful, but training them isn't always straightforward. The main challenge lies in the *intractability* of computing the **marginal likelihood** of the data, $p_\theta(x)$.

Recall from earlier:

$$p_\theta(x) = \int p_\theta(x, z) \, dz$$

This integral marginalizes out the latent variable $z$, giving us the likelihood of $x$ under the model. However, in most DLVMs, this integral ***doesn't have a closed-form solution***, nor can it be computed efficiently. This makes it hard to optimize $p_\theta(x)$ directly — unlike in fully observed models where we can differentiate the log-likelihood easily.

But the problem runs deeper.

This intractability also affects the *posterior distribution* over the latent variable:

$$p_\theta(z|x) = \frac{p_\theta(x, z)}{p_\theta(x)} \tag{1.19}$$

Here, the numerator $p_\theta(x, z)$ is usually tractable — we can compute it because it's based on known parts of the model. But the denominator $p_\theta(x)$ is intractable (as we saw), making the posterior $p_\theta(z|x)$ intractable too.

And yet, learning from data often *requires* computing or approximating this posterior.

**So, what do we do?** We turn to approximate inference methods.

These methods help us estimate both $p_\theta(x)$ and $p_\theta(z|x)$ without computing them exactly. However, traditional inference approaches can be slow or inaccurate — often requiring optimization for each individual data point, or producing poor-quality posterior approximations.

The challenge doesn't stop at latent variables.

In Bayesian treatments of DLVMs, the posterior over the model parameters themselves, $p(\theta | \mathcal{D})$, is also intractable. This again forces us to use approximation methods to make learning feasible.

# Variational Auto Encoder (VAE)

## Variational Inference

Variational Inference (VI) is a technique in Bayesian machine learning used to approximate complex probability distributions — especially the posterior distributions that arise in Bayesian statistics and are often computationally intractable to compute exactly.

Formally, VI is a method for approximating complex posterior distributions by finding a simpler distribution $q(z)$ that is close to the true posterior $p(z \mid x)$.

$$q^{\ast}(z) = \arg\min_{q(z) \in \mathcal{Q}} \mathrm{KL}(q(z) \parallel p(z \mid x))$$

To turn the DLVM's intractable posterior inference and learning problems into tractable problems, VAE introduces a parametric inference model $q_{\phi}(z \mid x)$. This model is also called an *encoder* or *recognition model*. With $\phi$ we indicate the parameters of this inference model, also called the *variational parameters*. We optimize the variational parameters $\phi$ such that:

$$q_{\phi}(z \mid x) \approx p_{\theta}(z \mid x)$$

## ELBO: Evidence Lower Bound for VAE

The optimization objective of the variational autoencoder, like in other variational methods, is the evidence lower bound, abbreviated as ELBO. An alternative term for this objective is variational lower bound. Typically, the ELBO is derived through Jensen's inequality. Here we will use an alternative derivation that avoids Jensen's inequality, providing greater insight about its tightness. Let's derive ELBO for VAE.

Let's start with our variational model $q_{\phi}(z \mid x)$, including the choice of variational parameters $\phi$. Now:

**Step 1:** Multiply by 1 (expressed as an integral)

$$\log p_{\theta}(x) = \int q_{\phi}(z \mid x) \, dz \cdot \log p_{\theta}(x)$$

**Step 2:** Bring the evidence into the integral (since $x$ is independent of $z$)

$$\log p_{\theta}(x) = \int q_{\phi}(z \mid x) \log p_{\theta}(x) \, dz$$

**Step 3:** Rewrite as an expectation

$$\log p_{\theta}(x) = \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \log p_{\theta}(x) \right]$$

**Step 4:** Apply Bayes' rule: $p(x) = \frac{p(x, z)}{p(z \mid x)}$

$$\log p_{\theta}(x) = \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \log \frac{p_{\theta}(x, z)}{p_{\theta}(z \mid x)} \right]$$

**Step 5:** Multiply and divide by $q_{\phi}(z \mid x)$

$$\log p_{\theta}(x) = \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \log \left( \frac{p_{\theta}(x, z)}{q_{\phi}(z \mid x)} \cdot \frac{q_{\phi}(z \mid x)}{p_{\theta}(z \mid x)} \right) \right]$$

**Step 6:** Split the logarithm into ELBO and KL divergence

$$\log p_{\theta}(x) = \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \log \frac{p_{\theta}(x, z)}{q_{\phi}(z \mid x)} \right] + \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \log \frac{q_{\phi}(z \mid x)}{p_{\theta}(z \mid x)} \right]$$

Where the first term is $\mathcal{L}_{\theta, \phi}(x)$ (ELBO) and the second term is $\mathrm{KL}(q_{\phi}(z \mid x) \| p_{\theta}(z \mid x))$.

The second term in the equation is the Kullback-Leibler (KL) divergence between $q_{\phi}(z \mid x)$ and $p_{\theta}(z \mid x)$, which is non-negative:

$$D_{\mathrm{KL}}(q_{\phi}(z \mid x) \,\|\, p_{\theta}(z \mid x)) \geq 0$$

and equal to zero if and only if $q_{\phi}(z \mid x)$ equals the true posterior distribution.

Due to the non-negativity of the KL divergence, the ELBO is a lower bound on the log-likelihood of the data:

$$\mathcal{L}_{\theta, \phi}(x) = \log p_{\theta}(x) - D_{\mathrm{KL}}(q_{\phi}(z \mid x) \,\|\, p_{\theta}(z \mid x)) \leq \log p_{\theta}(x)$$

Now, the ELBO loss can be written as follows:

**Starting point:**

$$\mathcal{L}_{\theta, \phi}(x) = \log p_{\theta}(x) - D_{\mathrm{KL}}(q_{\phi}(z \mid x) \,\|\, p_{\theta}(z \mid x))$$

**Rewrite the first term as an expectation (same logic as before):**

$$\mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \log p_{\theta}(x) \right] - D_{\mathrm{KL}}(q_{\phi}(z \mid x) \,\|\, p_{\theta}(z \mid x))$$

**Express KL divergence as an expectation:**

Recall that $D_{\mathrm{KL}}(a(x) \| b(x)) = \sum_{x} a(x) \log \frac{a(x)}{b(x)} = \mathbb{E}_{a(x)} \left[\log \frac{a(x)}{b(x)}\right]$

$$\mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \log p_{\theta}(x) \right] - \mathbb{E}_{q_{\phi}(z \mid x)} \left[\log \frac{q_{\phi}(z \mid x)}{p_{\theta}(z \mid x)}\right]$$

**Combine into a single expectation and expand the fraction:**

Since $\log \frac{A}{B} = \log A - \log B$:

$$\mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{q_{\phi}(z \mid x)}\left[\log p_{\theta}(x) - \log q_{\phi}(z \mid x) + \log p_{\theta}(z \mid x) \right]$$

**Rearrange terms:**

$$\mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{q_{\phi}(z \mid x)}\left[\log p_{\theta}(x)  + \log p_{\theta}(z \mid x) - \log q_{\phi}(z \mid x) \right]$$

**Combine the first two logarithms:**

Since $\log A + \log B = \log (AB)$:

$$\mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{q_{\phi}(z \mid x)}\left[\log \left( p_{\theta}(x) \cdot p_{\theta}(z \mid x) \right) - \log q_{\phi}(z \mid x) \right]$$

**Simplify to joint distribution:**

$$\mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{q_{\phi}(z \mid x)}\left[\log p_{\theta}(x,z)  - \log q_{\phi}(z \mid x) \right]$$

**Factor the joint distribution:**

$$\mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{q_{\phi}(z \mid x)}\left[\log \left( p_{\theta}(x \mid z) \cdot p_{\theta}(z) \right) - \log q_{\phi}(z \mid x) \right]$$

**Final form - expand the product:**

$$\mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{q_{\phi}(z \mid x)}\left[\log p_{\theta}(x \mid z) + \log p_{\theta}(z) - \log q_{\phi}(z \mid x) \right]$$

Now, let us examine each term in the equation in more detail:

- The term $\log p_{\theta}(x \mid z)$ corresponds to the *decoder* (or generative) model. Given a latent variable $z$, it attempts to reconstruct the observed data $x$, and thus represents the *reconstruction loss*.

- The term $\log p(z)$ denotes the *prior distribution* over the latent variables. This is typically chosen to be a standard Gaussian distribution, but other priors can also be used depending on the modeling assumptions.

- Finally, the term $\log q_{\phi}(z \mid x)$ represents the *encoder* or *variational inference model*. This is usually parameterized by a neural network, which outputs the parameters (e.g., mean and variance) of the approximate posterior distribution—commonly assumed to be Gaussian in many VAE implementations.

**Figure Note:** Overview of VAE and its components (Figure reference: vae.png)

## Gradient of ELBO

An important property of the ELBO is that it allows joint optimization with respect to all parameters ($\phi$ and $\theta$) using stochastic gradient descent (SGD). We can start with random initial values for $\phi$ and $\theta$, and stochastically optimize them until convergence.

Given a dataset with i.i.d. data, the ELBO objective becomes the sum (or average) of individual data point ELBOs:

$$\mathcal{L}_{\theta, \phi}(\mathcal{D}) = \sum_{x \in \mathcal{D}} \mathcal{L}_{\theta, \phi}(x)$$

The individual-data point ELBO, and its gradient $\nabla_{\theta, \phi} \mathcal{L}_{\theta, \phi}(x)$, is, in general, intractable. However, good unbiased estimators $\widetilde{\nabla}_{\theta, \phi} \mathcal{L}_{\theta, \phi}(x)$ exist, as we will show, which allow us to perform minibatch SGD.

Unbiased gradients of the ELBO with respect to the generative model parameters $\theta$ are relatively straightforward to obtain:

**Step 1:** Take the gradient of the ELBO

$$\nabla_{\theta} \mathcal{L}_{\theta, \phi}(x) = \nabla_{\theta} \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \log p_{\theta}(x, z) - \log q_{\phi}(z \mid x) \right]$$

Note that $\mathbb{E}_{q_{\phi}(\cdot)}$ is independent of $\nabla_{\theta}$.

**Step 2:** Move the gradient inside the expectation

$$\nabla_{\theta} \mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \nabla_{\theta} \left( \log p_{\theta}(x, z) - \log q_{\phi}(z \mid x) \right) \right]$$

**Step 3:** Monte Carlo approximation (sample $z \sim q_{\phi}(z \mid x)$)

$$\nabla_{\theta} \mathcal{L}_{\theta, \phi}(x) \approx \nabla_{\theta} \left( \log p_{\theta}(x, z) - \log q_{\phi}(z \mid x) \right)$$

**Step 4:** Simplify (since $q_{\phi}$ doesn't depend on $\theta$)

$$\nabla_{\theta} \mathcal{L}_{\theta, \phi}(x) \approx \nabla_{\theta} \log p_{\theta}(x, z)$$

The last line is a simple Monte Carlo estimator of the second line above, where $z$ in the last two lines is a random sample drawn from $q_{\phi}(z \mid x)$.

Unbiased gradients with respect to the variational parameters $\phi$ are more difficult to obtain, since the ELBO's expectation is taken with respect to the distribution $q_{\phi}(z \mid x)$, which itself depends on $\phi$. That is, in general:

$$\nabla_{\phi} \mathcal{L}_{\theta, \phi}(x) = \nabla_{\phi} \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \log p_{\theta}(x, z) - \log q_{\phi}(z \mid x) \right]$$

This is **not equal** to:

$$\mathbb{E}_{q_{\phi}(z \mid x)} \left[ \nabla_{\phi} \left( \log p_{\theta}(x, z) - \log q_{\phi}(z \mid x) \right) \right]$$

In the case of continuous latent variables, we can use the *reparameterization trick* to compute unbiased estimates of $\nabla_{\theta, \phi} \mathcal{L}_{\theta, \phi}(x)$, as we will now discuss. This stochastic estimate allows us to optimize the ELBO using SGD.

## Reparameterization Trick

For continuous latent variables, and assuming that both the encoder and the generative model are differentiable, the ELBO can be directly differentiated with respect to both $\phi$ and $\theta$. This is achieved through a change of variables known as the *reparameterization trick*.

To apply this trick, we express the random variable $z \sim q_{\phi}(z \mid x)$ as a differentiable and invertible transformation of another random variable $\epsilon$, where the distribution of $\epsilon$ is independent of both $x$ and $\phi$. Specifically,

$$z = g(\epsilon, \phi, x)$$

Here, $g$ is a deterministic function (typically implemented as part of the encoder), and $\epsilon$ is a noise variable drawn from a simple distribution, such as a standard Gaussian.

### Gradient of an Expectation Under Change of Variables

Given this change of variable, expectations over $q_{\phi}(z \mid x)$ can be rewritten as expectations over $p(\epsilon)$:

$$\mathbb{E}_{q_{\phi}(z \mid x)} \left[ f(z) \right] = \mathbb{E}_{p(\epsilon)} \left[ f(g(\epsilon, \phi, x)) \right]$$

Since $z$ is now a deterministic function of $\epsilon$, $\phi$, and $x$, and the expectation is taken over a distribution independent of $\phi$, the gradient and expectation operators can be interchanged:

**Step 1:** Rewrite using the reparameterization

$$\nabla_{\phi} \mathbb{E}_{q_{\phi}(z \mid x)} \left[ f(z) \right] = \nabla_{\phi} \mathbb{E}_{p(\epsilon)} \left[ f(g(\epsilon, \phi, x)) \right]$$

**Step 2:** Interchange gradient and expectation

$$= \mathbb{E}_{p(\epsilon)} \left[ \nabla_{\phi} f(g(\epsilon, \phi, x)) \right]$$

**Step 3:** Monte Carlo approximation (single sample $\epsilon \sim p(\epsilon)$)

$$\approx \nabla_{\phi} f(g(\epsilon, \phi, x))$$

In the last step, we approximate the expectation using a single Monte Carlo sample $\epsilon \sim p(\epsilon)$, which enables efficient and unbiased stochastic gradient estimation.

### Reparameterized ELBO

Using this approach, we can rewrite the ELBO as an expectation over $\epsilon$:

**Original ELBO:**

$$\mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \log p_{\theta}(x, z) - \log q_{\phi}(z \mid x) \right]$$

**Reparameterized ELBO:**

$$\mathcal{L}_{\theta, \phi}(x) = \mathbb{E}_{p(\epsilon)} \left[ \log p_{\theta}(x, z) - \log q_{\phi}(z \mid x) \right]$$

where $z = g(\epsilon, \phi, x)$.

This leads to a simple Monte Carlo estimator of the ELBO for a single data point using a single sample $\epsilon \sim p(\epsilon)$:

**Sampling procedure:**

$$\epsilon \sim p(\epsilon)$$

$$z = g(\phi, x, \epsilon)$$

**ELBO estimator:**

$$\widetilde{\mathcal{L}}_{\theta, \phi}(x) = \log p_{\theta}(x, z) - \log q_{\phi}(z \mid x)$$

This process can be efficiently implemented in computational frameworks like TensorFlow or PyTorch as a symbolic computation graph, allowing for automatic differentiation with respect to both $\theta$ and $\phi$. The resulting gradient $\nabla_{\phi} \widetilde{\mathcal{L}}_{\theta, \phi}(x)$ is used to optimize the ELBO via minibatch SGD.

### Unbiasedness of the Gradient Estimator

The gradient computed using this approach is an unbiased estimator of the true gradient of the ELBO. When averaged over multiple noise samples $\epsilon \sim p(\epsilon)$, we have:

**Step 1:** Expected value of the gradient estimator

$$\mathbb{E}_{p(\epsilon)} \left[ \nabla_{\theta, \phi} \widetilde{\mathcal{L}}_{\theta, \phi}(x; \epsilon) \right] = \mathbb{E}_{p(\epsilon)} \left[ \nabla_{\theta, \phi} \left( \log p_{\theta}(x, z) - \log q_{\phi}(z \mid x) \right) \right]$$

**Step 2:** Interchange gradient and expectation

$$= \nabla_{\theta, \phi} \mathbb{E}_{p(\epsilon)} \left[ \log p_{\theta}(x, z) - \log q_{\phi}(z \mid x) \right]$$

**Step 3:** Recognize this as the true ELBO gradient

$$= \nabla_{\theta, \phi} \mathcal{L}_{\theta, \phi}(x)$$

---

### Change of Variables for Probability Densities

Let $b$ be a continuous random variable with known density $p(b)$, and define a new variable $a = f(b)$, where $f$ is a differentiable and invertible transformation. Our goal is to compute the probability density of $a$, denoted $p(a)$, based on the known density of $b$.

The core principle is that probability mass must be preserved under a change of variables:

$$\int p(a)\, da = \int p(b)\, db$$

To express this in terms of $a$, we use the Jacobian determinant to account for how volume elements scale under the transformation. Specifically,

$$da = \left| \det \left( \frac{\partial f(b)}{\partial b} \right) \right| db$$

which leads to the change-of-variables formula:

$$p(a) = p(b) \left| \det \left( \frac{\partial f(b)}{\partial b} \right) \right|^{-1}$$

This is often written as:

$$p(a) = p(f^{-1}(a)) \left| \det \left( \frac{\partial f^{-1}(a)}{\partial a} \right) \right|$$

**Application in Variational Inference:**

In variational inference, particularly in the reparameterization trick, we express the latent variable $z$ as a deterministic function of a noise variable $\epsilon \sim p(\epsilon)$, such that:

$$z = g(\epsilon, \phi, x)$$

where $g$ is a differentiable, invertible function, and $\phi$ are variational parameters.

To compute the density $q_{\phi}(z \mid x)$ induced by this transformation, we apply the change-of-variables formula:

$$\log q_{\phi}(z \mid x) = \log p(\epsilon) - \log \left| \det \left( \frac{\partial z}{\partial \epsilon} \right) \right|$$

We define:

$$\log d_{\phi}(x, \epsilon) = \log \left| \det \left( \frac{\partial z}{\partial \epsilon} \right) \right|$$

so that the variational posterior becomes:

$$\log q_{\phi}(z \mid x) = \log p(\epsilon) - \log d_{\phi}(x, \epsilon)$$

---

### Computation of $q_{\phi}(z \mid x)$ in terms of $\epsilon$

To compute the ELBO (or its estimator), we need the log-density $\log q_{\phi}(z \mid x)$, given $x$ and a latent sample $z$, or equivalently $\epsilon$. This is tractable if we choose an appropriate, invertible transformation $g(\cdot)$.

Since the noise distribution $p(\epsilon)$ is known, and $z = g(\epsilon, \phi, x)$ is invertible, we can relate the densities via the change-of-variables formula:

$$\log q_{\phi}(z \mid x) = \log p(\epsilon) - \log d_{\phi}(x, \epsilon)$$

where $\log d_{\phi}(x, \epsilon)$ is the log absolute determinant of the Jacobian of the transformation from $\epsilon$ to $z$:

$$\log d_{\phi}(x, \epsilon) = \log \left| \det \left( \frac{\partial z}{\partial \epsilon} \right) \right|$$

Here, the Jacobian matrix is:

$$\frac{\partial z}{\partial \epsilon} = \begin{pmatrix} \frac{\partial z_{1}}{\partial \epsilon_{1}} & \cdots & \frac{\partial z_{1}}{\partial \epsilon_{k}} \\ \vdots & \ddots & \vdots \\ \frac{\partial z_{k}}{\partial \epsilon_{1}} & \cdots & \frac{\partial z_{k}}{\partial \epsilon_{k}} \end{pmatrix}$$

With suitable choices of $g(\cdot)$, the log-determinant $\log d_{\phi}(x, \epsilon)$ remains easy to compute, enabling expressive yet tractable variational posteriors $q_{\phi}(z \mid x)$.

**Figure Note:** Reparameterization Tricks in VAE (Figure reference: reparam-trick.png)

### Computation of $q_{\phi}(z \mid x)$ for Gaussian

A common choice for the variational posterior $q_{\phi}(z \mid x)$ is a simple factorized (mean-field) Gaussian:

$$q_{\phi}(z \mid x) = \mathcal{N}(z; \mu, \operatorname{diag}(\sigma^{2}))$$

$$(\mu, \log \sigma) = \text{EncoderNeuralNet}_{\phi}(x)$$

which implies a fully factorized distribution over the components of $z$:

$$q_{\phi}(z \mid x) = \prod_{i} q_{\phi}(z_{i} \mid x) = \prod_{i} \mathcal{N}(z_{i}; \mu_{i}, \sigma_{i}^{2})$$

where $\mathcal{N}(z_{i}; \mu_{i}, \sigma_{i}^{2})$ is the PDF of a univariate Gaussian.

After reparameterization, the latent variable $z$ is expressed as a deterministic function of noise $\epsilon \sim \mathcal{N}(0, I)$:

$$\epsilon \sim \mathcal{N}(0, I)$$

$$(\mu, \log \sigma) = \text{EncoderNeuralNet}_{\phi}(x)$$

$$z = \mu + \sigma \odot \epsilon$$

where $\odot$ denotes element-wise multiplication. The Jacobian of this transformation is a diagonal matrix:

$$\frac{\partial z}{\partial \epsilon} = \operatorname{diag}(\sigma)$$

whose determinant is simply the product of the diagonal elements. Therefore, the log-determinant becomes:

$$\log d_{\phi}(x, \epsilon) = \log \det \left( \frac{\partial z}{\partial \epsilon} \right) = \sum_{i} \log \sigma_{i}$$

Using the change-of-variables formula, the log-density of the approximate posterior becomes:

$$\log q_{\phi}(z \mid x) = \log p(\epsilon) - \log d_{\phi}(x, \epsilon)$$

Expanding this:

$$\log q_{\phi}(z \mid x) = \sum_{i} \log \mathcal{N}(\epsilon_{i}; 0, 1) - \log \sigma_{i}$$

where $z = g(\epsilon, \phi, x)$ is the reparameterized latent variable.

## Estimating Marginal Likelihood via Importance Sampling

After training a VAE, we may wish to estimate the marginal likelihood $\log p_{\theta}(x)$ of a datapoint under the generative model. Since exact computation is intractable, we can approximate it using *importance sampling*.

The marginal likelihood can be expressed as an expectation under the variational posterior:

$$\log p_{\theta}(x) = \log \mathbb{E}_{q_{\phi}(z \mid x)} \left[ \frac{p_{\theta}(x, z)}{q_{\phi}(z \mid x)} \right]$$

This can be approximated using Monte Carlo sampling. Drawing $L$ samples $z^{(l)} \sim q_{\phi}(z \mid x)$, we obtain the following estimator:

$$\log p_{\theta}(x) \approx \log \left( \frac{1}{L} \sum_{l=1}^{L} \frac{p_{\theta}(x, z^{(l)})}{q_{\phi}(z^{(l)} \mid x)} \right)$$

As $L \to \infty$, this estimator converges to the true log marginal likelihood. When $L = 1$, it reduces to the standard ELBO estimator used in VAE training.

This importance-weighted estimate can also be used as an objective function during training. This is the basis of the *Importance Weighted Autoencoder* (IWAE) introduced by Burda et al. (2015). In their work, it was shown that increasing $L$ yields a tighter bound on the true log-likelihood.

However, a limitation of the IWAE approach is that importance weights tend to become highly imbalanced in high-dimensional latent spaces, making the estimator unreliable as dimensionality increases.