---
title: "End of the Program"
date: 2026-06-11
draft: false
description: "What I did during phase 4."
#slug: "first"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "vineeth_kanpa"
showAuthorsBadges : false
---

### My Contribution

Over the past week, I focused on fully implementing the second machine learning model and integrating it into the application for both students and EU officials. On the student side, I built out the Diagnostic Survey page where students answer questions about their trust in national institutions, satisfaction with democracy, education level, and political orientation. The survey feeds these responses into the EU Trust Classifier (Model 2), a logistic regression model trained on Eurobarometer survey data, which predicts whether the student trusts the EU. The result is displayed to the student immediately after submission.

On the EU official side, I built a new Survey Results admin page that visualizes the aggregated student survey data. The page includes a gauge chart showing the overall percentage of students predicted to trust the EU, a bar chart breaking down trust rates by education level, and a bar chart showing trust rates across the political spectrum (1-10 left-right scale). The page only shows the most recent survey response per student to avoid duplicate counting. I also handled several data quality issues including filtering out mock data that had invalid predicted trust values, and ensuring that education levels and political orientations with no responses show as blank on the bar charts while those with 0% trust show a small red indicator.

Additionally I worked on the KNN similar country feature for Model 1, which returns the most similar real EU country to a user's hypothetical country based on all 11 input features simultaneously. I integrated this into the voter turnout prediction page so users see both their predicted turnout and the most similar real country in one response. I also fixed some backend route issues including column name mismatches between the frontend and the database schema.

### Visualizations
![EU Official Visualizations](/EU_Official_Visualizations.png)

### Recent Highlights
This past week, I've really enjoyed the escape room we did and the tour of the C-Mine. This week was also a little less busy than Phase 3 which was a welcome change of pace. I experienced a rollercoaster of emotions since the Knicks lost game 3 but then won game 4 in a crazy comeback. I found a Thai restaurant that gives a lot of food for only 5 euro and have been spamming that method. Lastly, I started texting in Toronto slang--it's a very fun bit and I intend to carry this with me it as I voyage back to the states.