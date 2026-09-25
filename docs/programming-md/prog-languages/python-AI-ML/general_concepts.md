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

---

Supervised Learning: Learning with Labels

In `supervised learning` (học có giám sát), the machine learning algorithm is trained on a dataset where each data point includes input features and the corresponding correct output or `label`. The goal is for the algorithm to learn a mapping function that can predict the output label for new, unseen input features. It's "supervised" because the presence of correct labels guides the learning process, much like a teacher supervises a student.

There are two main types of supervised learning problems:

1. `Classification`: The goal is to predict a discrete category or class label. Think "Is this email spam or not spam?", "Is this tumor malignant or benign?", or "What type of animal is in this picture (cat, dog, bird)?". The output is a **specific category**.
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

---

While these three are the main categories, you might occasionally hear about others:

- `Semi-Supervised Learning`: Uses a combination of a small amount of labeled data and a large amount of unlabeled data. This is useful when acquiring labels is expensive or time-consuming.
- Self-Supervised Learning: A type of unsupervised learning where labels are generated automatically from the input data itself. For example, predicting the next word in a sentence based on the preceding words.

## The Machine Learning Workflow

For instance, if you want to predict house prices, it's a supervised regression problem, and you might measure success by how close the predictions are to the actual sale prices.

The quality and quantity of data significantly impact the model's performance.

Based on the problem type and data exploration, you choose one or more candidate models (e.g., Linear Regression for predicting values, K-Nearest Neighbors for classification, K-Means for clustering). Then, you 'train' the model by feeding it the prepared data (the training set). During training, the algorithm learns patterns or relationships within the data.

**Evaluate the Model**: Once trained, you need to assess how well the model performs. This is done using data the model hasn't seen before (the test set). You use specific metrics relevant to the problem type (e.g., accuracy for classification, mean squared error for regression) to measure performance. This step helps determine if the model is good enough or if further refinement is needed.

**Fine-Tune and Iterate**: Based on the evaluation results, you might need to adjust the model (e.g., tweak settings called hyperparameters) or even go back to earlier steps. Perhaps you need more data, better features, or a different model entirely. Machine learning is often an iterative process involving cycles of training, evaluating, and tuning.

`Feature engineering` is the process of using domain knowledge, raw data, and analytical thinking to select, create, or modify input variables—known as features—so that machine learning models can learn patterns more effectively.

## Tools You Might Use

Pandas: Built upon NumPy, Pandas offers high-performance, easy-to-use data structures and data analysis tools. Its primary data structure, the DataFrame, is like a spreadsheet or SQL table within Python, making it excellent for loading, manipulating, cleaning, and analyzing structured data (like data from CSV files or databases). You'll use Pandas extensively for preparing your data before feeding it into a machine learning model.

## LLMs

Large Language Models (LLMs) are sophisticated AI systems trained on vast amounts of text data to understand, generate, and manipulate human language.

## Model, Data

Data is represented using table.

Each row is a D-dimensional vector, referred to as an `example` or data point in machine learning.  
Each column represents a particular `feature` of interest about the example.

In supervised learning, each example is associated with a label.

Examples with similar features should have similar labels.

The quality and quantity of data are extremely significant for the success of a machine learning project.  
Quality: The data must be relevant and accurate. If the features collected have no relationship to whether an email is spam, the model won't be able to learn effectively. Similarly, if the labels are often incorrect (e.g., labeling legitimate emails as spam), the model will learn the wrong patterns. This leads to the common saying in the field: "Garbage In, Garbage Out." Poor data will inevitably result in a poor model.

### Feature & Labels

- Identifying Spam Emails: To classify an email as spam or not spam, features could be the sender's email address, the presence of certain keywords (like "offer," "free," "winner"), the number of capital letters used, or whether the email contains attachments.
- Recognizing Handwritten Digits: For a system that recognizes handwritten digits from images, the features might be the values of individual pixels in the image grid.

Other terms you might hear used interchangeably with features include:

- Predictors
- Inputs
- Attributes
- Independent Variables

---

Recognizing Handwritten Digits: The label for each image would be the actual digit it represents (0, 1, 2, ..., 9). This is also a classification problem.

Common synonyms for label include:

- Target Variable
- Output
- Response
- Dependent Variable
- Class (specifically in classification problems)

---

The fundamental goal in supervised machine learning is to use the features to predict the label.

It's important to note that not all machine learning tasks involve labels. In Unsupervised Learning, the goal is often to find structure or patterns within the data based only on the features, without any predefined correct answers.

