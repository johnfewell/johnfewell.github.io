---
layout: post
title:      "Rails Portfolio Project: Continuing Education Certificates"
date:       2018-02-20 21:09:50 +0000
permalink:  rails_portfolio_project_continuing_education_certificates
---

## Background

For my project, I decided to tackle a problem that I was very familiar with from my work. The post-graduate training institute where I work is certified by New York State to issue continuing education credits to licensed social workers, art therapists, and psychoanalysts. Atteding programs or classes that grant these continuing education credits is required of all licensed professionals in new york stae in order for those with these liceses to maintain their licenses. 

People who want these credits must first attend a qualified program. Then, they fill out an evalition of the program in order to obtain a certificate which they can submit to the state to prove that they attended the program and earned the credits they need to renew their license. Making sure that every one who attended a program fills out an evalution and then receives the PDF of the certificate has been difficult. I thought it would be interesting to try to create a rail app to solve this problem. 

## Initial issues

I started with the models I knew I would need: Instructors, Attendees, and Courses. Instructors had many Courses, they also had many Attendees through Courses. Attendees had many Courses and many Instructors through Courses. So far, so good. 
Setting up those first few migrations and models went smoothly until it came time to wrap my head around Evalutions. Evalutions have many Questions, and Questions have many Evalutions. But how to do you answer these questions? And then where are those answers stored? I spent a long time researching this question and goolging other peoples' solutions. Then, I finally found the answer: There needed to be an Answer model! Of course there needed to be an answer model. Answers had one Question and Questions had many Answers. But how does the app keep track a group of Answers? I needed another model, Finished Evaluations. Now that I had Questions, Answers, Evaluations and Finished Evalutions, and their relationships to Attendees and Instructors in place, the basic structure was complete.

## Custom writer

```
class FinishedEvaluation

  def answers_attributes=(answers_hashes)
    answers_hashes.each do |i, answer_attributes|
      answer = Answer.create(content: answer_attributes[:content], attendee_id: answer_attributes[:attendee_id], question_id: answer_attributes[:question_id])
      attendee = Attendee.find(answer_attributes[:attendee_id]) unless answer_attributes[:rating].nil?
      attendee.rate(answer, answer_attributes[:rating]) unless answer_attributes[:rating].nil?
      self.answers << answer
    end
  end
```

## Scoped Methods
http://localhost:3000/courses/finished

```
class Course 

  scope :complete, -> {
    where('end_date < ?', Date.today)
  }

  scope :not_complete, -> {
    where('end_date > ?', Date.today)
  }

  scope :upcoming, -> {
    where('start_date > ?', Date.today)
  }

  scope :started, -> {
    where('start_date <= ?', Date.today)
  }

  scope :published, -> {
    where('published == ?', true )
  }

  scope :unpublished, -> {
    where('published == ?', false )
  }
```

![](http://)
## Reports
http://localhost:3000/courses/1/report
![This](https://iptar.org/wp-content/uploads/2018/02/Selection_002.jpg)


## Evaluations

## Autheniticaton

I used Devise. Overriding some of the default views and controllers was tricky to figure out, but the actual installtion and getting Facebook login up and running was easy. 

## Authorization

I learned from using Devise that perhaps I needed a less robust solution than using a complicated gem like CanCanCan. However, rolling my own wasn't simple either, and I'm sure there are many improvements to be made to my various is_authorized? helpers. I 
