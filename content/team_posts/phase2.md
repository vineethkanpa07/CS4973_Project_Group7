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

### Sourcing
The preliminary implementation of the machine learning model involved a few challenges. The first main difficulty was in sourcing and putting together the dataset. European Parliament (EP) election voter turnout statistics, GDP per capita, corruption perception index, unemployment rate, urbanization rate, compulsory voting status, years of EU membership, and median age all had to be collected from separate sources. These sources included the European Parliament and World Bank. We then manually merged all our data into a single CSV file (there was one feature which was in a separate file that we merged in the notebook itself). 

### Imputation and Date Alignment
A second challenge was handling missing data, particularly for older election years where some values were unavailable. This was resolved using linear interpolation for each country, in order to fill gaps while ensuring that each country's historical trend was followed. Additionally, since national election turnout was added as a feature by merging a separate dataset, we had to align the dates since national elections don't occur on the same schedule as EP elections. We chose to use the rows that represented the closest years to the EP elections. 

### Performance
The model currently achieves an R² value of 0.57 and a mean absolute error of plus/minus 10.5 percentage points. Tasks that we still have to do include improving model performance by sourcing additional features like EU public approval ratings, and further hyperparameter tuning to avoid overfitting. We also have to integrate the model into the Streamlit app so users can interact with it (through the "build your own country" interface).

## Data Visualizations and EDA

### Voter Turnout

Voter turnout has declined sharply since the EU's inception, sitting at 48.8% in 2024. This decline isn't uniform across regions though. Western Europe's turnout has been steadily improving and sits at 62.9% for 2024, while Southern, Eastern, and Northern Europe average 45.9%, 42.9%, and 42.1% respectively.

{{< iframe src="region_turnout_trend.html" width="100%" height="600" >}}

{{< iframe src="region_avg_turnout_2024.html" width="100%" height="550" >}}

The strongest continuous predictors of voter turnout are urbanization rate (r=0.495), years of EU membership (r=0.390), corruption index (r=0.355, but only available from 2013 onwards), and GDP per capita (r=0.250). Using corruption index as a predictor will be difficult since we'd need to impute a lot of missing values, it might be better to just drop it. Unemployment (r=-0.117) and population (r=0.055) show weak linear correlations, but it's worth investigating further since the relationships might not be linear. Population in particular seems dominated by large countries like Germany, France, Italy, and Spain, so a log transform might be worth trying. Compulsory voting shows a large group difference: countries with enforced compulsory voting average 75.8% turnout versus 46.4% for voluntary.

{{< iframe src="fit_urbanization_rate.html" width="100%" height="600" >}}

Median age shows a weak negative linear correlation with turnout (r=-0.27), though the relationship doesn't look strongly linear and may be worth revisiting during model development.

Corruption index shows a positive relationship with turnout (r=0.355), though the limited data window (2013 onwards) means this should be interpreted with caution.

{{< iframe src="fit_corruption_index.html" width="100%" height="600" >}}

Collinearity is a concern for several feature pairs: GDP per capita vs years of EU membership (r=0.641), GDP per capita vs corruption index (r=0.710), and GDP per capita vs urbanization rate (r=0.464). If we go with linear regression we'll need to take a closer look and do some trial and error to figure out which features to keep.

### EU Trust

Our second model is going to predict EU trust, using a cleaned dataset derived from the Eurobarometer survey. The dataset covers 25,936 individual respondents from the Autumn 2024 wave.

Institutional opinions dominate. The strongest predictors of EU trust are trust in parliament (r=-0.394), satisfaction with democracy (r=-0.367), and trust in politicians (r=-0.300). These correlations are negative because of how the Eurobarometer records responses, higher values mean less trust or satisfaction. So the data is actually showing that people who are more satisfied with democracy and trust their politicians tend to trust the EU more. Opinions on politics seem to matter a lot.

{{< iframe src="trust_by_parliament.html" width="100%" height="550" >}}

Features like gender (r=0.010) and left-right political placement (r=-0.023) show negligible linear correlations, but as with the turnout model, weak linear correlation doesn't mean no relationship and we'll need to investigate further.

Age has a non-linear, U-shaped relationship with EU trust. It declines from 67.4% among 15-24 year olds to a low of 51.5% among 55-64 year olds, before rising again to 62.4% among those 75 and older. It's worth checking other low-correlation features for similar patterns before dismissing them.

{{< iframe src="trust_by_age.html" width="100%" height="550" >}}

Trust in parliament and trust in politicians are themselves correlated (r=0.479), which suggests a shared institutional trust factor. Including both risks multicollinearity, so we'll need to address that during model development.

## Wireframes

### Student

![Wireframe for student simulation](/studentwf1.jpg)
![Wireframe for student dianositc survey](/studentwf2.jpg)

### Teacher

![Wireframe for teacher grading](/teacherwf1.jpg)
![Wireframe for teacher](/teacherwf2.jpg)

### EU Official

![Wireframe for eu official 1](/euwf1.jpg)
![Wireframe for eu official 2](/euwf2.jpg)

### Home/Landing Page

![Wireframe for home page](/homewf1.jpg)