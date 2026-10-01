# Math for ML

## Inference

When we train a machine learning model, we usually evaluate its performance on a separate dataset called a test set. This test set acts like our sample. The performance metric we calculate, such as accuracy, precision, or mean squared error, is essentially a `point estimate`. It's our best guess, based on the test set sample, of how well the model would perform on all possible unseen data (the population).

## Event & Sample Space

In machine learning, we often deal with data points. You can think of observing a single data point (like a customer's purchase amount or whether an email is spam) as an outcome of an experiment. The sample space represents all possible observations, and an event might correspond to observing a data point with specific characteristics (e.g., purchase amount over $100, or email classified as spam).

## Linear Algebra

In AI, matrices ARE the model:

- Neural network weights → matrices that transform input into output
- Attention scores → matrices that decide what to focus on
- Embeddings → matrices that map words to vectors

---

- The dot product of two vectors tells you how similar they are.
  - Same direction: $a \cdot b > 0$  (similar)
  - Perpendicular: $a \cdot b = 0$  (unrelated)
  - Opposite direction:  $a \cdot b < 0$  (dissimilar)
- This is the basis of similarity search in AI. This is literally how search engines, recommendation systems, and RAG work -- find vectors with high dot products.

Your feature matrix should have linearly independent columns. If two features are perfectly correlated (linearly dependent), the model cannot distinguish their effects. This causes multicollinearity in regression -- the weight matrix becomes unstable, and small input changes produce wild output swings.
