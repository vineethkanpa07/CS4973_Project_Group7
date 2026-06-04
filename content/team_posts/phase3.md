---
title: "Phase III"
date: 2026-06-04
draft: false
description: "EUtopia Week 3"
#slug: "phase1post"
tags: ["project", "Setup"]
authors:
  - "vineeth_kanpa"
  - "sidra_ansari"
  - "meghan_paclob"
  - "bennett"
showAuthorsBadges: false
---

## Project Updates


## Machine Learning Features

### Model 1: Voter Turnout Predictor

### Model 2: EU Trust Classifier
The EU Trust Classifier uses five features to predict whether a survey respondent trusts the European Union: education level, trust in national parliament, trust in national politicians, satisfaction with democracy, and left/right political orientation. These features were selected after an initial analysis that included age, gender, and political interest. Age was dropped despite having the highest feature importance (44%) because removing it actually improved testing accuracy, suggesting that the model was overfitting to age-specific patterns in the training data instead of learning  trends that were generalizable. Gender and political interest were dropped due to a very low feature importance (1.7% and 1.4% respectively), showing that they held little predictive importance. 

The remaining five features make sense intuitively. More educated people are more likely to understand the complexities of the EU, people who trust their national political institutions tend to extend that trust to larger ones like the EU, satisfaction with democracy reflects a general agreeableness toward political systems, and left/right orientation captures  alignment with EU values. It is important to note that education was kept as a weaker  input, but is still relevant.

## REST API Matrix

## App Screenshots and Implementation