---
title: Decision Tree
type: docs
prev: models/nb
next: models/regression
weight: 5
---

## Overall Goal

text

## Overview

- describe decision trees (dt) and their applications
- include at least two images supporting explanation
- explain:
  - gini index
  - entropy
  - information gain
- show a small worked example for split quality using either gini or entropy + info gain
- discuss why dts can lead to infinite trees (overfitting concept)

## Data Preparation

- choose suitable labeled dataset
- clean data
- split into disjoint training/testing sets
  - explain how you created the split
  - explain why disjoint sets are important
  - clearly state whether the split was reused or newly made
- create and insert:
  - link to dataset
  - images of cleaned data and splits

## Modeling 

- write code to train and test decision trees
- create at least three different trees:
  - with different root nodes
  - and other differences (depth, pruning, features)
- upload and link to code

## Results

- visualize:
  - each of the three decision trees
  - confusion matrices
  - accuracy results
- compare differences between trees
- clearly discuss results and what they reveal

## Conclusions

- state what you learned or can predict using dts
- connect insights to your project topic
