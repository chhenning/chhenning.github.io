---
date:
  created: 2025-12-23
  updated: 2025-12-23

draft: True

tags:
  - glossary
---

# Glossary

My attempt to provide an explanation to all the various AI/ML terms, so that I can understand them...

<!-- more -->

## Classification

Predicting classes.

## Multimodal Distribution

A distribution with two or more peaks, called `modes`.

## Multivariate Regression problem

Predicting multiple values

## Power Law Distribution

A distribution with a very long tail.

## Regression

Predicting values

## Regularization

## RNN - Recurrent Neural Network

## Sampling Bias

Training data was collected with flawed sampling method. For instance, asking only rich people who they would vote for a presidental election.

## Semisupervised Learning

Class of algorithms that can deal with data that's partially labeled. For instance, Google Photo can tell that a person A shows up in photos 1,5, and 11, while another person B shows up in photos 2, 5, and 7.

## Skewed Dataset

Some classes/Values/Labels are much more frequent than others.

Standard Correlation Coefficient

- aka `Pearson's`.

Coefficients around zero means there is no linear correlation.

## Standard Deviation

Measures how disperse values are.

## Standardization

In Feature Scaling. (value - mean) / std. Result has unit variance. Standardization is much less affected by
outliers. See `StandardScaler` in Scikit-Learn.

## Stratified Sampling

A population is split into similar groups, called strata. Samples are then taken from each group in the right proportions so the test set reflects the full population. For example, the U.S. population is about 51.3% female and 48.7% male, so a representative sample should follow the same split.

## Supervised Learning 

The training set contains the correct answers, called `labels`. One common task is `classification`, where the goal is to assign items to categories—for example, a spam filter that learns from emails labeled as spam or ham.

Another common task is `regression`, where the goal is to predict a numeric target value, such as a car’s price. Here there is a set of features (also called predictors) like mileage, age, and brand.

To train the model, you provide many examples that include both the features and their labels (for example, cars along with their prices). The terms attribute and feature are often used interchangeably.

## Underfitting

Opposite of `overfitting`. Model is too simple to learn the underlying structure of the data

## Univariate Regression problem

Predict one value

## Unsupervised Learning

Training set is unlabeled.