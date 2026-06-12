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

### The Voter Turnout and KNN Model

The voter turnout model is a linear regression trained on 184 observations of EU parliamentary elections from 1979 to 2024. The six core features (compulsory voting, region, median age, unemployment rate, population, and national turnout) are the same as phase 3, along with the same nonlinear terms. We use LOO-CV as the primary evaluation metric rather than a standard train/test split because the dataset is small enough that a single split would be unreliable. A standard 80/20 split gave us a train R² of 0.85 and a test R² of 0.81, which was too optimistic. LOO-CV brought that down to 0.79 with an RMSE of 8.8 percentage points, which is probably a more honest figure.

The KNN model is built on top of the voter turnout model. It uses the same 11 features and the same scaler, fit on the same 184 observations with k=1. When a student runs a country simulation, KNN finds the single most similar real country-year observation in the training data and returns it alongside the prediction. The idea is to give context and a point of refrence to the student's country simulations.

### Assumption Checks

The three big assumptions have been verified. The residuals vs fitted plot checks linearity and homoscedasticity. The scatter looks pretty random around zero which is good, though there is some mild spread in the middle range. The high-turnout cluster on the right side is Belgium and Luxembourg, whose consistently high turnout is handled by the compulsory x Western interaction term.

{{< iframe src="residuals_vs_fitted.html" width="100%" height="600" >}}

The residuals vs order plot checks for autocorrelation, there isn't an obvious pattern
.
{{< iframe src="residuals_vs_order.html" width="100%" height="600" >}}

### EU Trust Classifier Model

The EU Trust Classifier is a logistic regression model that predicts whether an individual trusts the European Union based on five survey inputs: education level, trust in national parliament, trust in politicians, satisfaction with democracy, and left-right political orientation. Trained on over 20,000 Eurobarometer survey responses with a testing accuracy of 73.2%, the model is integrated into the student diagnostic survey. Students answer a short questionnaire and the model predicts whether they trust the EU without asking them directly (avoiding social desirability bias, a well-documented phenomenon that can occur when studying EU trust levels). For EU officials, acts as a monitoring tool. As students complete surveys, the admin dashboard aggregates all predictions and visualizes trust distribution across education levels and political orientations in real time. This gives EU officials a better idea of where trust is strongest and weakest among the student population, which can inform decisions about civic education priorities.

### Architecture

The model's coefficient vector and scaler parameters are stored in the database under voter_turnout_params and voter_turnout_scaler. When a student hits predict in the country simulation the route reads the latest parameters from the DB, applies the stored standardization, and computes the prediction. This means the EU official can retrain the model through the web app and the new parameters take effect immediately without a redeploy.

The KNN model is an extension of the voter turnout model. It uses the same features, the same scaler, and is fit on the same dataset with k=1. Unlike the voter turnout model it is used exclusively in the student's country simulation as a point of reference. When a student creates a hypothetical country, KNN finds the most similar real country-year observation and surfaces it alongside the prediction so students have something concrete to compare against.


## Software Architecture

Our project is built as a multi-container web application with three main layers: a MySQL database, a Flask REST API backend, and a Streamlit frontend. The frontend handles all user interactions and displays different screens based on the logged-in user's role. These users include students, teachers, and EU officials. Students can view lessons, take assessments, take a diagnostic survey, and run country simulations. Teachers can view and manage lessons, while EU officials can review submitted lessons and choose to approve or reject them based on the content.


The backend is organized using Flask Blueprints which allows routes to be separated by feature area, such as initialization and users, lessons, assessments, classes, surveys, and machine learning. The frontend communicates with the backend through HTTP requests using the CRUD operations combined with requests (i.e. requests.get()). This keeps the UI separate from the data logic and makes the project easier to expand.


The database stores all persistent application data, including users, roles, classes, lessons, assessments, questions, student responses, progress records, simulations, diagnostic survey results, platform metrics, and machine learning model parameters. The machine learning features are also connected through backend routes. For example, the country simulation sends user inputs to the voter turnout prediction endpoint, which returns a predicted EU election turnout and saves the result to the student's simulation history.


## Final Database Model

The final database model centers around users and their roles. The "Users" table stores account information, while "Roles" and "UserRole" allow each user to be assigned as a student, teacher, or EU official. Teachers are connected to classes through the "Class" table, and students are connected to classes through "StudentProfile." 


Lessons are stored in the "Lessons" table. Each lesson belongs to a teacher and can optionally belong to a class. Lessons also include approval information, such as "approvalStatus" and "approvedBy," which logically supports the workflow of the EU official approval. This allows teachers to create lessons and EU officials to approve or reject them before they are displayed.


Assessments are connected to lessons through the "Assessment" table. Each assessment contains questions stored in the "Question" table. Student answers are stored in the "Response" table, which links each student to the question they answered. A new "QuestionOption" table was also added to support multiple choice questions by storing answer choices and marking which option is correct.


Student learning progress is tracked in "StudentProgress," which records completion rate, quiz performance, engagement time, and completion status for each student and lesson. Country simulation results are stored in the "Simulation table," including the student's country name, population, unemployment rate, compulsory voting status, median age, region, national turnout, and predicted turnout.


The project also includes machine learning related tables. eu_turnout_dataset stores the dataset used for EU voter turnout simulations. voter_turnout_params and voter_turnout_scaler store the trained turnout model parameters and scaling values. eurobarometer_dataset and eu_trust_params support the diagnostic survey model that predicts trust in the EU.


Overall, the database model supports the full flow of the application: users log in by role, students access class lessons, teachers create lessons, EU officials approve lessons, students take assessments, responses are saved, simulations are stored, and machine learning models provide predictions based on stored data.


## Reflection

Developing EUtopia from (almost) scratch provided valuable experience in designing and implementing a full-stack software system. Throughout the course of the project, our team learned how to integrate a Streamlit frontend, Flask REST API backend, MySQL databases, and machine learning components into one website. Separating the functionalities into different route files and organizing the database around user roles made the system easier to maintain.



One of the most important lessons we learned was the importance of designing a database model and architecture that is flexible. As we ideated more and wanted to implement more features such as lesson approval and multiple-choice questions, the already existing structure allowed these additions to be incorporated without redesigning heavily. Working on EUtopia also strengthened our understanding of concepts learned in our CS and DS classes such as REST APIs and database design. Since we were also immersed in collaboration through Git and Github, we learned to communicate effectively and work towards one common goal. Overall, this project was fulfilling in understanding how basic architecture principles can transform a basic idea into a fully functional education platform.