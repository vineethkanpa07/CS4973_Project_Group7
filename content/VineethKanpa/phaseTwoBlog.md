---
title: "Week Two"
date: 2025-05-26
draft: false
description: "Talking about the first model."
#slug: "first"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "vineeth_kanpa"
showAuthorsBadges : false
---

# My Contribution and Model I Info

For this phase, I built a machine learning model that predicts EU Parliament voter turnout for a hypothetical country based on key demographic, economic, and political characteristics. The model takes inputs like GDP per capita, corruption perception index, compulsory voting status, urbanization rate, unemployment rate, median age, years of EU membership, and national election turnout, and outputs a predicted voter turnout percentage with an estimated margin of error of ±10.5 percentage points. The model predictes EU voter turnout percentage with a cross-validated R² of 0.57. I collected historical European Parliament (EP) election data across all 27 EU member states from 1979–2024, supplemented with national election turnout data from the IDEA Voter Turnout Database. I cleaned and merged this second dataset in Python using pandas, and a Random Forest Regressor was trained using scikit-learn. When the model is fully implemented, users will be able to adjust sliders and inputs to design their own hypothetical EU country and receive a predicted voter turnout.

# Week 2 Highlights

This week I really enjoyed going to Ghent. It was very fun to get closer with my friends and explore the city. It's my favorite place that we've been to this summer and reminds me a lot of Italy. It was also my birthday and I had fun chilling at the bar with my friends for a little bit. I'm looking forward to going to Strasbourg next week! #knicksinseven
