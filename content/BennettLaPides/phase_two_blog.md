---
title: "Bruges, Gent, and phase 2 of the project"
date: 2026-05-27
draft: false
description: "A breif recap of my contributions and experiences since the previous post"
#slug: "first"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "bennett"
showAuthorsBadges : false
---

## Phase 2 Deliverables

My primary contributions for this phase were the data visulations, EDA, and further data cleaning. The first thing I did wasa to compile the data we gathered in phase one into 3 seperate csvs. One contains data such as gdp per capita, population, urbanization rate, etc. All information that is going to be used to predict voter turnout. The second csv is the heavily cleaned eurobarometer data which is going to be used to predict eu trust and the third is just suplimetary statistics. 

### Voter Prediction Data

I ran a script to visualize a lot of the numerical data for the prediction model and was able to identify some interesting relationships with some of our features. There was no coorelation between population and voter turnout so moving forward we can remove that column. The EDA revealed that on their own our features are weakly coorelated with voter turnout at best, we haven't found like one big indicator of voter turnout. From what we have so far the biggest signal is urbanization rate 

![Urbanization graph](fit_urbanization_rate.png)

### EU Trust Data

I also spent some time to analyze the Eurobaromter data and got some interesting information about EU trust from it. I still need to spend some more time analyzing it but from what I've found features like gender, political orientation, and age seem to have little direct coorelation with trust in the EU. All three are <.100 coorelated with EU Trust. As you might expect features like satisfaction with democracy and trust in politians show a much stronger coorelation with EU trust. I've considered aggregating all of this individual data into a national level, that will likely be my next step with this dataset.

![trust by statisfaction](trust_by_satisfaction.png)

### Exploring Belgium

Outside of all of the work I've been having a lot of fun here in Belgium. I got to Bruges last weekend and it was a beautiful little town. Both the Atomium and Gent were nice as well. The trip has been an incredible experience so far.

![bruges](bruges_bennett.jpg)