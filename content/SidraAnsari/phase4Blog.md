---
title: "Github & Goodbyes: Week Four Reflection"
date: 2026-06-11
draft: false
description: "Github & Goodbyes"
#slug: "fourth"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "sidra_ansari"
showAuthorsBadges : false
---

## Individual Contributions

As we finished up our final project for this course, a lot of loose ends needed to be tied up. I implemented the initial routes, including user initialization. This served as the basis for the originaL StreamLit pages.

Once basic functionality was implemented, I expanded the UI to improve accessibility for different uses. 
For student, I separated the country simulation tabs to distinguish between the preset country values and build-your-own. I also made the simulation results saveable so the student can retrieve them at a later time. I also developed and edited all the questions and related responses so that the multiple-choice questions actually has a correct response, and that non-true/false questions are left open-ended.
For teacher, I implemented access for all lessons, not just their own. They are in separate tabs so that it is easier to see the approval/rejection status.
For the EU official, I created the lesson approval system that allows for them to control whether a lesson is accepted or rejected based on existing pending lessons as well as lessons created by teachers on the website.


A lot of troubleshooting also had to be done because the basic program had everything hardcoded in. Below are the components I automated so the program could fetch them without being directly fed the values:
- Users: This was uncovered when only 2 EU officials were showing up even though there were several in the database.
- Values in Guided Simulation: I modified the code to call the country's existing values directly so they didn't have to be listed twice.


## Goodbye Leuven!

My favorite moments of this week were the chocolate-making class and the EnergyVille Labs (as an ECE major I found it super interesting!). My least favorite part was the escape room because although it was a lot of fun, we unfortunately failed due to factors beyond our control. Though I am sad to be leaving Leuven, I am excited to return to the beautiful land of New Jersey. I have made lasting memories here and will cherish them as I go back to the US.

![On a Ferris wheel in Antwerp!](/sidphase4.png)