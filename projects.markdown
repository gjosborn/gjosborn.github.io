---
layout: page
title: Projects
permalink: /projects/
---

# Red Leg Safety

Check out the GitHub Repository [here](https://github.com/gjosborn/RedLegSafety)

*A Django webapp that completes the basic functions of manual gunnery*

#### Below is a copy of the current README for RedLegSafety
One of the most formative experiences of a youn artilleryman's career is completing the manual gunnery
unit during Field Artillery Basic Officer Leader's Course. This unit culminates in the dreaded "safety test"
This test measures whether you have a deep enough understanding of the math and physics of artillery to safely
operate on a gunline or in a Fire Direction Center.

These tests are verified by hand or in some cases through sophisticated excel workbooks that double check the
the calculations made. However, this limits the amount of practice students may get since they are limited to
those released by their instructors and the creation of the exams and study materials is a time consuming process.

What if students were able to login to a webapp, select what type of safety problems they would like assistance
with and have example safety worksheets generated for them to complete. What if instructors could login and generate
their safety exams from their account, generating a different exam for each student to prevent cheating.

That is what I aim to accomplish in this webapp, create a simpler process for one of the more arduous processes
during the course, lightening the load on instructors, and creating more adept field artillerymen and women.


# TFT_Parse

*A supporting project to Red Leg Safety*

As part of Red Leg Safety, I need a way to create a database of the data in the Tabular Firing Tables (TFTs) from the PDF tables in which they currently exist.

As a solution to the database creation task, I am also working on TFT_Parse. This repository utilizes the Camelot package to scan the tables from the PDFs. I am currently refining this and attempting to generalize the code to make it easier to add tables of any type from PDF to this or any database in the future.

#### Below is a copy of the current README for TFT_Parse

*Parsing the tables from a Tabular Firing Table (TFT) to add to a SQLite database*

The tabular firing table is the base for all manual field artillery calculations. They are published by The U.S. Army Armament Research, Development and Engineering Center (ARDEC) and are available to those in the artillery community. Since they contain all the technical firing data of current US howitzers, they are not available publicly.

This parser parses the tables of technical data from this PDF and organizes it so it can be used to populate a SQLite Database in a Django webapp

Check out the GitHub Repository for TFT_Parse [here](https://github.com/gjosborn/TFT_Parse)