---
title: "Week Three"
date: 2026-06-04
draft: false
description: "What I did during phase 3."
#slug: "first"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "vineeth_kanpa"
showAuthorsBadges : false
---

### My Contribution and Model II info

For Phase III, my primary contributions were the development and validation of the EU Trust Classifier (Model 2) and refinement work on the Voter Turnout Predictor (Model 1). For Model 2, I built a logistic regression classifier to predict whether a survey respondent trusts the European Union based on Eurobarometer survey data. At first, I also built a Random Forest Classifier, but it did not perform as well as the logistic regression version. Creating the model involved cleaning the dataset by identifying and removing unnecessary values (97 and 98 were encoded to represent someone with no political affiliation in the left_right feature, for instance), handling null values, and selecting the most predictive features through iterative feature testing. I dropped age, gender, and political interest after finding further feature testing, which I discussed in more depth in the team blog post. To validate the model I produced a feature correlation heatmap, which confirmed no significant pairwise multicollinearity. I also created a boxplot for outlier analysis that revealed the left_right encoding issue with 97 and 98 values (which I then dropped). For Model 1, I worked on refining the Random Forest Regressor for voter turnout prediction by tuning hyperparameters including max_depth, min_samples_leaf, and min_samples_split to reduce overfitting. I achieved improvement in the cross-validated R² score. However, Bennett developed a significantly stronger voter turnout model, so his implementation was adopted as the final version.

### Recent Highlights
Recently, I really enjoyed going to Strasbourg and Luxembourg. The hotels were super nice and it was nice to get closer with all my friends. I saw _Obsession_ in Luxembourg and it was very solid. It was also fun to chill at a bar before and after the movie with my friends. Luxembourg is my new favorite place that we’ve been to this summer. I am so very very happy that the Knicks won their first game in the finals and look forward to watching the rest of the games. #knicksinsix