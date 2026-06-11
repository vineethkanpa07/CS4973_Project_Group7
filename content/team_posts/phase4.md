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

<<<<<<< HEAD

=======
### The Voter Turnout and KNN Model
>>>>>>> 0684502b435c8d236230fd0b87c5c8ae367b62ec

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
