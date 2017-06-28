---
layout: post
title:  "Sinatra Portfolio Project: School Registration"
date:   2017-06-28 16:03:23 -0400
---

## Introduction

Since I work at a training institute, the instructor, course, and student models are famliar to me. I decided to create a school registration app with user logins for the students. 

I used the [Pure CSS](https://purecss.io) framework so that I could use their basic styling without having to do everything from scratch while still having a very small external CSS file. I also added my own styling via the '''/public''' directory in the Sinatra project. I also added a favicon to the '''/public''' directory so that I would top seeing constant 404 errors when I ran rackup. 

I added a small bit of Javascript to the flash messages so that the user can dismiss them. 

I ran into small problems doing this project, but I knew how to debug everything I came up against and never really felt stuck. The preceeding Sinatra labs had similar routes and challenges, so while this was an extension of those lessons, it wasn't onerous. 

Students can sign up, register for classes, view which instructor it teaching which class, view class details and edit their profile. 
The admin user can create, edit, and delete any student's registration or student, any instructor's assigned courses or instructor, and any course info or courses. 

## Models

Courses belong to instructors, have many students through the join table course_students.
Instructors have many courses and have many students through courses.
Students have many courses through the join table course_students. They also have many instructors through courses. 

## Screenshots
The login screen.

![](http://i.imgur.com/hNySuZt.png)

The greeting message for the admin. 

![](http://i.imgur.com/FpNP26L.png)

This is the edit profile screen. 

![](http://i.imgur.com/8cKmZ18.png)

You can't register for classes when logged into the admin account, but you can when logged in as a student.d

![](http://i.imgur.com/OV22ejq.png)

Viewing a single course.

![](http://i.imgur.com/sc8a4ci.png)

This is the flash message after deleting a course. 

![](http://i.imgur.com/5yBuRiZ.png)

This is the view for all the instructors.

![](http://i.imgur.com/4LKnXbf.png)

This is the edit instructors page.

![](http://i.imgur.com/8cKmZ18.png)

Sheri now has two classes assigned for her to teach. 

![](http://i.imgur.com/IqDoldf.png)

This is the view for all of the students. 

![](http://i.imgur.com/SmnPEbT.png)

This is the edit student page. 

![](http://i.imgur.com/7m7uZOk.png)

Now Ronnie has an instructor and a class because he's now in her class. 

![](http://i.imgur.com/q2B1p0u.png)

## Video Walkthrough

<iframe width="560" height="315" src="https://www.youtube.com/embed/Le4umWT-auk" frameborder="0" allowfullscreen></iframe>
