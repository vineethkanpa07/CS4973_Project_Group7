---
title: "Phase II"
date: 2026-05-27
draft: false
description: "EUtopia Week 2"
#slug: "phase1post"
tags: ["project", "Setup"]
authors:
  - "vineeth_kanpa"
  - "sidra_ansari"
  - "meghan_paclob"
  - "bennett"
showAuthorsBadges: false
---

## Updates to Existing Personas

### Personas:

### Sofia Novak - Citizen Learner 
Bio: 
- 17, Poland
- An EU member state citizen
- Will be eligible to vote soon
- Interested in politics when it feels relevant to daily life, but finds EU institutions confusing and overly formal
- Mostly gets information through short videos, social media, and school
- “I hear about the EU all the time: elections, policies, climate laws, but I still don’t really understand what it actually does or why it matters to me.”
Behaviors:
- Engages more when examples connect directly to Poland or issues she cares about (student mobility, climate policy, jobs)
- Short attention span, prefers visual, interactive formats over walls of text
- Likes to see/track their current progress through visual indicators
Core Needs/User Stories:
- As a citizen learner, I want to create a hypothetical EU country by entering population, demographics, and political indicators so that I can see how those factors influence predicted turnout in European Parliament elections.
- As a citizen learner, I want to track my progress with visual indicators like progress bars, achievements, and completed modules so that I feel motivated to keep learning
- As a student, I want interactive quizzes and simulations instead of long readings so that learning about the EU feels engaging rather than boring
- As a citizen learner, I want to complete a diagnostic survey about my background and political attitudes so that I can see predicted trust in EU institutions among people similar to me.

### Marco De Smet - Teacher/Content Creator
Bio:
- 38, Belgium
- Secondary-school civics and history teacher
- Interested in improving civic participation among young people
- Also creates learning content for other educators on the platform
- “I want my students to care about the EU, but I also want reusable materials that other teachers can benefit from.”
Behaviors:
- Plans in weekly/unit blocks
- Monitor class-level progress, not just individual
- Comfortable editing templates rather than building from scratch
- Reuses existing content when possible
- Looks at learner completion and quiz data to refine lessons
- Interested in community-created educational resources
- Core Needs/User Stories:
- As a teacher, I want to assign specific modules to the whole class or individual students manually so that I can override the adaptive system when I know a topic needs class-wide attention
- As a teacher, I want pre-built lesson materials and assessments so that I can integrate EU education into my curriculum without spending hours creating content from scratch
- As a content creator, I want to submit my modules for moderator review before publication so that quality remains consistent across the platform.
- As a content creator, I want to see learner completion rates, quiz performance, and average engagement time so that I can improve future lessons.


### EU Official
Bio:
- Government official, EU education coordinator, or approved civic education moderator (30–60 years old)
- Works with educational or governmental institutions focused on civic engagement and political literacy
- Responsible for ensuring that educational content is accurate, unbiased, and aligned with EU standards before being published to citizens and students
- “I want citizens to learn about the EU through trustworthy, engaging content while making sure misinformation or inaccurate political material is not distributed.”

Behaviors:
- Reviews newly submitted modules daily
- Tracks creator activity and platform engagement
- Flags outdated or inaccurate content for revision
User Stories:
- As a moderator, I want to approve or reject submitted modules so that all published content meets educational quality standards.
- As a moderator, I want to request revisions on submitted modules so that creators can improve them before publication.
- As a moderator, I want to see how many modules were created in the past week or month so that I can monitor creator activity.
- As a moderator, I want to view platform-wide statistics such as active learners, module completion rates, and most popular topics so that I can evaluate the platform’s impact.
- As a moderator, I want to identify modules with low completion or poor quiz performance so that I can prioritize them for improvement.


## Models

![Localized Data Model](/localizedmodels.png)

![Global Data Model](/globalmodels.png)

![Relational Data Model](/relationalmodel.png)

## ML Model

## Data Visualizations and EDA

Voter turnout has sharply decilined since the EU's inception and was hovering around 49% for the election in 2024. Cluster analysis of the data revealed that this decline isn't uniform; Western Europe's turnout has been steadily improving and sits at 63% for 2024 while Northern, Southern, and Eastern Europe are at 45.9%, 42.1%, and 42.9% respectivly with Northern and Southern Europe having decreased from the last election. The strongest predictors of voter turnout are urbanization rate (r=.495), corruption index (r=.355, slight caveat being that this data is only available from 2013 onwards), and gdp per capita (r=.250 all time, for 2024 its r=.58). Metrics like unemployment and population have practically 0 coorelation with turnout and should probably be dropped

EU Trust, institutional opinions dominate everything. Metrics like gender, political leaning, and political interest don't seem to coorelate at all with trust in the EU. Interestingly enough age has a non linear relationship with EU Trust, it steadily decreases until 65 and then starts to rise again. It might be worth checking the other low metrics for non linear relationships. The biggest indicators of trust in EU are trust in politcians and satsifaction with democracy, both have a positive relationship with EU Trust.

Some of the data raises colinearity concerns. The strongest two are gdp per capita and years in EU (r=.641) and gdp per capita and corruption index (r=.710). If we decide to use linear regression we will need to take a closer look and do some trial and error.

![Eurobarometer csv](csv1.jpg)
![Voter turnout csv](csv2.jpg)
![Urbanization](fit_urbanization_rate.png)
![Age](trust_by_age.png)



## Wireframes

### Student

![Wireframe for student simulation](/studentwf1.jpg)
![Wireframe for student dianositc survey](/studentwf2.jpg)

### Teacher

![Wireframe for teacher grading](/teacherwf1.jpg)

### EU Official

![Wireframe for eu official 1](/euwf1.jpg)

### Home/Landing Page

![Wireframe for home page](/homewf2.jpg)