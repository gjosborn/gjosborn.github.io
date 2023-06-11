---
layout: page
title: Projects
permalink: /projects/
---

# Red Leg Safety

Check out the GitHub Repository [here](https://github.com/gjosborn/RedLegSafety)

*A Django web app that completes the basic functions of manual gunnery*

#### The Case for this Project
Manual gunnery is the practice of using manual techniques (as opposed to a digital approach like
AFATDS) to provide technical fire direction, the deflection and elevation of the howitzer to hit the
target and achieve desired effects. One of the most important aspects of manual gunnery is the
manual computation of safety data and the production of the Safety T (figure 1). Manual gunnery
and manual safety are taught at both Basic Officer’s Leader Course (BOLC) and Advanced Leaders
Course (ALC) to provide the requisite knowledge to arrive at any training location prepared to safely
execute artillery operations and adapt to changing circumstances.

In BOLC, of the roughly eight exams on various artillery topics throughout the course, the exam
following instruction on manual safety calculations (’the safety test’) is known as the most difficult
evaluation of the course. Homework assignments regarding safety T calculations are completed in paper
workbooks and checked by or turned in to the instructor. Students also have access to 5-10 additional
examples with provided answers which may be completed outside of the mandatory homework examples
in preparation for the exam. The production of class workbooks, additional examples, and the safety
test itself have to be created by hand or be reused from previous classes resulting in repeated material
or unnecessary time consuming work for instructors.

The current paper-based practice and evaluation approach to manual gunnery instruction lacks efficient
record-keeping and analytical capabilities, hindering the ability to gather valuable insights from the
practice and evaluations completed. To address this issue, a transition to a web application enables
enhanced record keeping of individual performance and aggregate trends, thereby improving the
evaluation process and facilitating data-driven decision-making.

Create a web application that transforms the studying for and evaluation of field artillery manual
gunnery. The application will provide students with a platform to access (1) study materials, (2)
generate practice problems and tests, and (3) receive automatic feedback on their performance and
areas for improvement. Instructors will be able to use the platform to (4) administer exams and (5)
varied individual and aggregate performance and analytics.

# TFT_Parse

*A supporting project to Red Leg Safety*

Check out the GitHub Repository for TFT_Parse [here](https://github.com/gjosborn/TFT_Parse)

As part of Red Leg Safety, I need a way to create a database of the data in the Tabular Firing Tables (TFTs) from the PDF tables in which they currently exist. Specifically, Table F, which consists of 19 columns and subcolumns. The table's dividing lines are designed to be able to scan easily by someone looking quickly for the correct row. However, this means that the dividing lines by which most python packages would scapre a PDF table are irregular and note particularly useful.

As a solution to this database creation task, I am working on TFT_Parse. This repository utilizes the Camelot package to scan the tables from the PDFs. Ideally, I will work through the database creation issues on the most common TFT and because of the standardization of these tables, this parser will also work on TFTs for other projectiles or round types, although this is not a guarantee.

