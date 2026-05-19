---
title: "Phase I"
date: 2026-05-19
draft: false
description: "EUtopia"
#slug: "phase1post"
tags: ["project", "Setup"]
authors:
  - "vineeth_kanpa"
  - "sidra_ansari"
  - "meghan_paclob"
  - "bennett"
showAuthorsBadges: false
---

# Project Description

Many EU citizens, especially younger students, struggle to understand how the European Union’s elections and political decision-making processes work. Information about the EU can be complex and difficult to navigate, and is often presented in ways that are not engaging for students. Our group aims to build an educational platform inspired by learning systems such as Canvas that helps students learn about the EU through modules and assessments. Teachers will be able to create and assign modules on topics such as EU elections, the European Parliament, citizens’ rights, and policymaking, while parents can monitor their children’s progress in a separate parent view.

To enhance the learning experience, our platform will include two machine learning models trained on real datasets. Students will be able to input hypothetical data to build their own EU country and discover its predicted European Parliament election turnout levels. This model will use demographic and political indicators from EU countries to help learners understand factors that influence election participation. A second model will predict trust in EU institutions based on factors such as age, education, country of origin, and satisfaction with democracy. Students can take a diagnostic survey to determine the level of trust in the EU held by people who are similar to them. This will help achieve our goal of bolstering peoples' understanding of and engagement with European political systems.

# Personas

Core Needs/User Stories:
As a teacher, I want a dashboard showing which EU topics my whole class is weak on so that I can focus our next discussion sessions where it’s most needed
As a teacher, I want to assign specific modules to the whole class or individual students manually so that I can override the adaptive system when I know a topic needs class-wide attention
As a teacher, I want pre-built lesson materials and assessments so that I can integrate EU education into my curriculum without spending hours creating content from scratch
As a teacher, I want to track student engagement and completion rates so that I can identify which students may need additional support or encouragement

Parent
Bio: 
Parent of a secondary school/university student (40-60 years old)
EU citizen who follows major political events but does not deeply understand EU institutions
Values education and preparing their child for the future
Behaviors:
Encourages educational growth, but has limited time to teach politics or civics at home actively
Prefers simple explanations over technical political language
More engaged when information connects directly to real-life impacts such as jobs or inflation
Likes progress summaries and visible learning outcomes for their child
Appreciates trustworthy, fact-based educational platforms
Core Needs/User Stories:
As a parent, I want to see my child’s learning progress so that I know they are becoming more informed about EU politics and voting
As a parent, I want the app to explain EU topics in simple language so that I can understand and discuss them with my child
As a parent, I want personalized recommendations based on my country and current events so that the lessons actually feel relevant to our daily lives
As a parent, I want my child to stay engaged through interactive activities rather than long readings so that they actually continue using the platform
As a parent in a country with low voter turnout, I want my child to understand why voting matters, so they become an active citizen in the future


(/eurobarometer.jpg)


## Data Description
 
 We have data from three major sources: Eurostat, the World Bank, and eurobarometer. The eurostat and world bank data is mostly numeric and contains general summary statistics such as population, population density, GDP per capita, Urban population percentage, and tertiary education rates. Eurobarometer has categorical data of the public's opinion on the EU and their home countries. It contains data such as satisfaction with the EU, satisfaction with national government, and satisfaction with democracy. I wrote a python script to analyze our sources and have pasted some the script's output below.


 ![eurobarometer data](eurobarometer.png)
 ![world bank and eurostat data](data_wb_eurostat.png)


## Connection to Personas

**Student:** The EU trust model using Eurobarometer data helps reveal what EU concepts students don't understand, making module personalisation easier.

**Teacher:** Aggregate data like GDP per capita, unemployment rates, and tertiary education rates help teachers and module creators tailor content to their students based on country.

**Parent:** Data like GDP per capita and unemployment rates connect real huworld economic conditions to EU policy, making the content more relevant to parents' daily lives.
