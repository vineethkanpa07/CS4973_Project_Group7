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

We revised the team blog post for phase 2 according to the feedback we recieved. We replaced the plots with interactive plots and ensured that all the wireframes are right side up.

## Machine Learning Features

### Model 1: Voter Turnout Predictor

The Voter Turnout Predictor is a linear regression model that predicts EU parliamentary election voter turnout. It achieves a LOO-CV R² of 0.7928 with an average prediction error of roughly 8.8 percentage points. Because of the small dataset (n=184), we used leave one out cross validation as the primary evaluation metric rather than a standard train/test split.

The model has 11 features: compulsory voting, median age, median age squared, national turnout, national turnout squared, unemployment rate, population, a compulsory voting x Western region interaction term, and three region binary columns (Northern, Southern, and Western Europe with Eastern Europe as the reference category).

National turnout is the strongest predictor, countries with high civic engagement in national elections tend to carry that into EU elections. Compulsory voting is a strong predictor specifically because of Belgium and Luxembourg, which consistently enforce it. Because compulsory voting is only meaningfully enforced in Western Europe, we added a compulsory voting x Western region interaction term to capture this. The region binary columns were added because the data visualization showed a clear regional difference in voting turnout. Western Europe consistently votes much more than Northern, Southern, and Eastern Europe regardless of what the other features predict.

The proof of concept from Phase 2 helped us identify two non-linear relationships. Both national turnout and median age showed parabolic patterns so we added squared terms for both. We also investigated a log transform of population, an unemployment squared term, and whether country-level EU trust rate correlated with turnout (r=-0.084). None of these improved LOO-CV so they were dropped.

### Regression Assumptions

The residuals vs fitted plot checks two things: linearity and whether the error is consistent across predictions. The scatter looks pretty random around zero which is good, though there is some mild spread in the middle range. The high-turnout cluster on the right are Belgium and Luxembourg.

{{< iframe src="residuals_vs_fitted.html" width="100%" height="600" >}}

The residuals vs order plot checks whether the errors are correlated with each other, the plot doesn't show any clear pattern so everything should be good.

{{< iframe src="residuals_vs_order.html" width="100%" height="600" >}}

### Model 2: EU Trust Classifier
The EU Trust Classifier uses five features to predict whether a survey respondent trusts the European Union: education level, trust in national parliament, trust in national politicians, satisfaction with democracy, and left/right political orientation. These features were selected after an initial analysis that included age, gender, and political interest. Age was dropped despite having the highest feature importance (44%) because removing it actually improved testing accuracy, suggesting that the model was overfitting to age-specific patterns in the training data instead of learning  trends that were generalizable. Gender and political interest were dropped due to a very low feature importance (1.7% and 1.4% respectively), showing that they held little predictive importance. 

The remaining five features make sense intuitively. More educated people are more likely to understand the complexities of the EU, people who trust their national political institutions tend to extend that trust to larger ones like the EU, satisfaction with democracy reflects a general agreeableness toward political systems, and left/right orientation captures  alignment with EU values. It is important to note that education was kept as a weaker  input, but is still relevant.

## REST API Matrix

## App Screenshots and Implementation