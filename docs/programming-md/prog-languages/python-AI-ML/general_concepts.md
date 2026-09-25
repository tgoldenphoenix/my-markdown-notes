# AI, Machine Learning Notes

## Jargon

k

## ML vs AI

Deep learning is a subset of machine learning, which is itself a subset of artificial intelligence.

AI includes components beyond just machine learning, such as natural language processing, computer vision, and robotics.

- Artificial Intelligence (AI): This is the biggest circle. It refers to any machine or software that mimics human cognitive functions—like problem-solving, reasoning, and learning.
- Machine Learning (ML): This is a specialized circle inside AI. Instead of a human writing every single rule (if-then statements), the machine uses statistical models to learn patterns from data and make predictions.
- Deep Learning (DL): A smaller circle inside ML that uses Neural Networks (like a digital brain) to handle massive, complex data like photos and speech

## Types of Machine Learning

Machine learning algorithms learn from data to make predictions or decisions. These algorithms are broadly categorized into supervised, unsupervised, and reinforcement learning.

`Supervised learning` (học có giám sát) is a machine learning technique that uses labeled data sets to train artificial intelligence (AI) models to identify the underlying patterns and relationships.

There are two main types of supervised learning problems:

1. `Classification`: The goal is to predict a discrete category or class label. Think "Is this email spam or not spam?", "Is this tumor malignant or benign?", or "What type of animal is in this picture (cat, dog, bird)?". The output is a specific category.
2. `Regression`: The goal is to predict a continuous numerical value. Think "What will the temperature be tomorrow?", "How much will this house sell for?", or "How many customers will visit the store next week?". The output is a number on a scale.

Most practical machine learning applications today use supervised learning because having labeled data often leads to more accurate predictions for specific tasks.

---

In `unsupervised learning`, the algorithm is given input data without any corresponding output labels. The goal is for the algorithm to explore the data and find meaningful structure, patterns, or relationships on its own. It's "unsupervised" because there's no teacher or correct answer guiding the process.

Common types of unsupervised learning tasks include:

- Clustering: Grouping similar data points together based on their features. For example, grouping customers with similar purchasing habits for targeted marketing, or grouping news articles about the same topic.
- Dimensionality Reduction: Simplifying data by reducing the number of features (dimensions) while retaining important information. This can be useful for visualization or improving the performance of other ML algorithms.
- Association Rule Learning: Discovering rules that describe relationships between items in large datasets. A classic example is finding that customers who buy diapers often also buy beer.

Unsupervised learning is often used for exploratory data analysis and can reveal insights you might not have expected.

---

Reinforcement Learning: Learning through Trial and Error

While very powerful, RL is often more complex to implement than supervised or unsupervised learning and is generally covered in more advanced courses.

## LLMs

Large Language Models (LLMs) are sophisticated AI systems trained on vast amounts of text data to understand, generate, and manipulate human language.

## Model, Data

Data is represented using table.

Each row is a D-dimensional vector, referred to as an `example` or data point in machine learning.  
Each column represents a particular `feature` of interest
about the example.

In supervised learning, each example is associated with a label.

Examples with similar features should have similar labels.

