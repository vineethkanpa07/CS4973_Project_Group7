---
title: "Phase IV"
date: 2026-06-11
draft: false
description: "EUtopia Week 4"
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

### The Voter Turnout and KNN Model

The voter turnout model is a linear regression trained on 184 observations of EU parliamentary elections from 1979 to 2024. The six core features (compulsory voting, region, median age, unemployment rate, population, and national turnout) are the same as phase 3, along with the same nonlinear terms. We use LOO-CV as the primary evaluation metric rather than a standard train/test split because the dataset is small enough that a single split would be unreliable. A standard 80/20 split gave us a train R² of 0.85 and a test R² of 0.81, which was too optimistic. LOO-CV brought that down to 0.79 with an RMSE of 8.8 percentage points, which is probably a more honest figure.

The KNN model is built on top of the voter turnout model. It uses the same 11 features and the same scaler, fit on the same 184 observations with k=1. When a student runs a country simulation, KNN finds the single most similar real country-year observation in the training data and returns it alongside the prediction. The idea is to give context and a point of refrence to the student's country simulations.

### Assumption checks

The three big assumptions have been verified. The residuals vs fitted plot checks linearity and homoscedasticity. The scatter looks pretty random around zero which is good, though there is some mild spread in the middle range. The high-turnout cluster on the right side is Belgium and Luxembourg, whose consistently high turnout is handled by the compulsory x Western interaction term.

{{< iframe src="residuals_vs_fitted.html" width="100%" height="600" >}}

The residuals vs order plot checks for autocorrelation, there isn't an obvious pattern
.
{{< iframe src="residuals_vs_order.html" width="100%" height="600" >}}

### Architecture

The model's coefficient vector and scaler parameters are stored in the database under voter_turnout_params and voter_turnout_scaler. When a student hits predict in the country simulation the route reads the latest parameters from the DB, applies the stored standardization, and computes the prediction. This means the EU official can retrain the model through the web app and the new parameters take effect immediately without a redeploy.

The KNN model is an extension of the voter turnout model. It uses the same features, the same scaler, and is fit on the same dataset with k=1. Unlike the voter turnout model it is used exclusively in the student's country simulation as a point of reference. When a student creates a hypothetical country, KNN finds the most similar real country-year observation and surfaces it alongside the prediction so students have something concrete to compare against.
