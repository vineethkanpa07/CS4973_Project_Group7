---
title: "Phase III"
date: 2026-06-04
draft: false
description: "EUtopia Week 3"
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

On the DS side, ...... 

On the CS side, the personas were finalized and implemented into routes that were integrated in the REST API. Mock data was also generated that has been implemented in the routes, alongside the voter turnout dataset.


## Machine Learning Features

### Model 1: Voter Turnout Predictor

### Model 2: EU Trust Classifier
The EU Trust Classifier uses five features to predict whether a survey respondent trusts the European Union: education level, trust in national parliament, trust in national politicians, satisfaction with democracy, and left/right political orientation. These features were selected after an initial analysis that included age, gender, and political interest. Age was dropped despite having the highest feature importance (44%) because removing it actually improved testing accuracy, suggesting that the model was overfitting to age-specific patterns in the training data instead of learning  trends that were generalizable. Gender and political interest were dropped due to a very low feature importance (1.7% and 1.4% respectively), showing that they held little predictive importance. 

The remaining five features make sense intuitively. More educated people are more likely to understand the complexities of the EU, people who trust their national political institutions tend to extend that trust to larger ones like the EU, satisfaction with democracy reflects a general agreeableness toward political systems, and left/right orientation captures  alignment with EU values. It is important to note that education was kept as a weaker  input, but is still relevant.

## REST API Matrix

During this phase, we developed the routes that serve as the basis for the app. Originally, we framed these routes around the different personas based on our DDL, but quickly realized that it made more sense to have a "user" attribute serve as the core of the user definition rather than completely separating the personas.

![REST API Matrix](/restapimatrix.jpg)

This is the tentative matrix we developed in order to initialize the route creation. It includes at least one CRUD operation. 

<details>
<summary>Click to view the Phase 3 developments of the routes.</summary>

```sql
from flask import Blueprint, request, jsonify
from backend.db_connection import get_db
from backend.ml_models.voter_turnout_model import predict_turnout


api_bp = Blueprint("api", __name__)


@api_bp.route("/users", methods=["GET"])
def get_users():
    db = get_db()
    cursor = db.cursor(dictionary=True)
    cursor.execute("SELECT * FROM Users")
    users = cursor.fetchall()
    cursor.close()
    return jsonify(users)


@api_bp.route("/users/<int:userID>", methods=["GET"])
def get_user(userID):
    db = get_db()
    cursor = db.cursor(dictionary=True)
    cursor.execute("SELECT * FROM Users WHERE userID = %s", (userID,))
    user = cursor.fetchone()
    cursor.close()

    if not user:
        return jsonify({"error": "user not found"}), 404

    return jsonify(user)


@api_bp.route("/roles", methods=["GET"])
def get_roles():
    db = get_db()
    cursor = db.cursor(dictionary=True)
    cursor.execute("SELECT * FROM Role")
    roles = cursor.fetchall()
    cursor.close()
    return jsonify(roles)


@api_bp.route("/users/<int:userID>/roles", methods=["GET"])
def get_user_roles(userID):
    db = get_db()
    cursor = db.cursor(dictionary=True)

    cursor.execute(
        """
        SELECT Role.roleID, Role.roleName
        FROM UserRole
        JOIN Role ON UserRole.roleID = Role.roleID
        WHERE UserRole.userID = %s
        """,
        (userID,)
    )

    roles = cursor.fetchall()
    cursor.close()
    return jsonify(roles)


@api_bp.route("/lessons", methods=["GET"])
def get_lessons():
    db = get_db()
    cursor = db.cursor(dictionary=True)
    cursor.execute("SELECT * FROM Lessons")
    lessons = cursor.fetchall()
    cursor.close()
    return jsonify(lessons)


@api_bp.route("/lessons/<int:lessonID>", methods=["GET"])
def get_lesson(lessonID):
    db = get_db()
    cursor = db.cursor(dictionary=True)
    cursor.execute("SELECT * FROM Lessons WHERE lessonID = %s", (lessonID,))
    lesson = cursor.fetchone()
    cursor.close()

    if not lesson:
        return jsonify({"error": "lesson not found"}), 404

    return jsonify(lesson)


@api_bp.route("/lessons", methods=["POST"])
def create_lesson():
    data = request.get_json()
    db = get_db()
    cursor = db.cursor()

    cursor.execute(
        """
        INSERT INTO Lessons
        (classID, teacherID, approvedBy, title, topicName, content, difficultyLevel, approvalStatus, createdBy, updatedBy)
        VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s)
        """,
        (
            data.get("classID"),
            data["teacherID"],
            data.get("approvedBy"),
            data["title"],
            data.get("topicName"),
            data["content"],
            data.get("difficultyLevel"),
            data.get("approvalStatus", "Pending"),
            data.get("createdBy"),
            data.get("updatedBy"),
        )
    )

    db.commit()
    lessonID = cursor.lastrowid
    cursor.close()

    return jsonify({"message": "lesson created", "lessonID": lessonID}), 201


@api_bp.route("/lessons/<int:lessonID>", methods=["PUT"])
def update_lesson(lessonID):
    data = request.get_json()
    db = get_db()
    cursor = db.cursor()

    cursor.execute(
        """
        UPDATE Lessons
        SET title = %s,
            topicName = %s,
            content = %s,
            difficultyLevel = %s,
            approvalStatus = %s,
            updatedBy = %s
        WHERE lessonID = %s
        """,
        (
            data["title"],
            data.get("topicName"),
            data["content"],
            data.get("difficultyLevel"),
            data.get("approvalStatus"),
            data.get("updatedBy"),
            lessonID,
        )
    )

    db.commit()
    cursor.close()

    return jsonify({"message": "lesson updated"})


@api_bp.route("/lessons/<int:lessonID>", methods=["DELETE"])
def delete_lesson(lessonID):
    db = get_db()
    cursor = db.cursor()
    cursor.execute("DELETE FROM Lessons WHERE lessonID = %s", (lessonID,))
    db.commit()
    cursor.close()

    return jsonify({"message": "lesson deleted"})


@api_bp.route("/assessments", methods=["GET"])
def get_assessments():
    db = get_db()
    cursor = db.cursor(dictionary=True)
    cursor.execute("SELECT * FROM Assessment")
    assessments = cursor.fetchall()
    cursor.close()
    return jsonify(assessments)


@api_bp.route("/questions/<int:assessmentID>", methods=["GET"])
def get_questions_for_assessment(assessmentID):
    db = get_db()
    cursor = db.cursor(dictionary=True)
    cursor.execute("SELECT * FROM Question WHERE assessmentID = %s", (assessmentID,))
    questions = cursor.fetchall()
    cursor.close()
    return jsonify(questions)


@api_bp.route("/progress/<int:studentID>", methods=["GET"])
def get_student_progress(studentID):
    db = get_db()
    cursor = db.cursor(dictionary=True)
    cursor.execute("SELECT * FROM StudentProgress WHERE studentID = %s", (studentID,))
    progress = cursor.fetchall()
    cursor.close()
    return jsonify(progress)


@api_bp.route("/simulations/<int:studentID>", methods=["GET"])
def get_student_simulations(studentID):
    db = get_db()
    cursor = db.cursor(dictionary=True)
    cursor.execute("SELECT * FROM Simulation WHERE studentID = %s", (studentID,))
    simulations = cursor.fetchall()
    cursor.close()
    return jsonify(simulations)


@api_bp.route("/ml/turnout-prediction", methods=["POST"])
def turnout_prediction():
    data = request.get_json()
    prediction = predict_turnout(data)

    return jsonify({
        "predictedTurnout": prediction,
        "inputs": data
    })
```

</details>


Below is an explanation of an example of each of the operations we did.



1. CREATE (POST)

POST /lessons

This inserts a new lesson into the database and assigns it a lessonID;



2. READ (GET)

GET /lessons

This selects one specific lesson based on the lessonID.



3. UPDATE (PUT)

PUT /lessons/12

This allows for a lesson to be modified.



4. DELETE (DELETE)

DELETE /lessons/12

This provides the option for a lesson to be deleted. This is operable by the EU Official who has the power to delete a lesson.


## App Screenshots and Implementation

## Plans for Final Phase

As the project wraps up, we are looking to fully integrate both data models into our routing. Implementing all the routes will serve as the complete backend for the app. Ensuring all the pages have all features done will be the most time-consuming part, as making sure everything connects may prove challenging.