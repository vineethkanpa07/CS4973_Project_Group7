---
title: "Phase 3 of the project"
date: 2026-06-04
draft: false
description: "Brief post on my experiences in the first week of the dialogue"
#slug: "first"
tags: ["authors", "config", "docs"]
authors:
  - "bennett"
showAuthorsBadges : false
---

## Phase 3 Deliverables

### Model Overview

My main responsability for this phase was the EU voter turnout ML model. We decided to change from random forest to linear regression and at the end of this phase its R² value is .7928 and its margin of error is roughly 8.8%. For evaluation I decided to use leave one out cross validation because the sample size was so small (n=184).

### Training\Testing the Model

Training the model was really just a lot of trial and error. We had to drop a few columns, mainly corruption index, becauses the data just didn't go back far enough and we didn't have a good way to impute it. The proof of concept from last week's phase helped identify some interesting relationships with median age and national turnout. Both features ended up having a non-linear relationship so we added them as squared terms. The data visualizations also revealed a pretty clear pattern where Western Europe has good turnout and the other 3 regions don't so we added 3 region binary columns to capture that. We tried a bunch of other combinations of features and non-linear terms and I think the model is nearing its limit. The dataset size is so small and since its based off the eu election cycle I can't really do much but add more columns.

### Model Checking

I created a file in the team repo, voter_turnout_diagnostics, which creates residual plots. There is a weird trend on the residual plot with Luxembourg and Belgium. The residuals for those two are very percise compared to the rest, as far as I can tell this is because they both have compulsory voting and actually bother enforcing it. I addressed this by adding a compulsory voting * western region term. Aside from that exception everything else looks pretty good to me.

{{< iframe src="residuals_vs_fitted.html" width="100%" height="600" >}}
{{< iframe src="residuals_vs_order.html" width="100%" height="600" >}}

### Backend Contributions

I created the voter_turnout_model file and the predict, test, and train functions which we are intending to have the EU offical persona call. The file also contains private helper methods to query weights and parameters from the sql database. As of writing the methods are incomplete, the sql database isn't quite ready yet so I added to do comments which specify to add the sql when it is ready.

### Miscellaneous

I updated my portion of the phase 2 team blog (The data viz and EDA) post based on the feedback we recieved. The main improvement was adding the interactive plots, I also rewrote parts of my section to try and address the feedback.

