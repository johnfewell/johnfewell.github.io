---
layout: post
title:      "Rails Portfolio Project: Continuing Education Certificates"
date:       2018-02-20 16:09:51 -0500
permalink:  rails_portfolio_project_continuing_education_certificates
---

## Background

For my project, I decided to tackle a problem that I was very familiar with from my work. The post-graduate training institute where I work is certified by New York State to issue continuing education credits to licensed social workers, art therapists, and psychoanalysts. Attending programs or classes that grant these continuing education credits is required of all licensed professionals in new york state in order for those with these licenses to maintain their licenses. 

People who want these credits must first attend a qualified program. Then, they fill out an evaluation of the program in order to obtain a certificate which they can submit to the state to prove that they attended the program and earned the credits they need to renew their license. Making sure that everyone who attended a program fills out an evaluation and then receives the PDF of the certificate has been difficult. I thought it would be interesting to try to create a rail app to solve this problem. 

## Initial issues

I started with the models I knew I would need: Instructors, Attendees, and Courses. Instructors had many Courses, they also had many Attendees through Courses. Attendees had many Courses and many Instructors through Courses. So far, so good. 
Setting up those first few migrations and models went smoothly until it came time to wrap my head around Evaluations. Evaluations have many Questions, and Questions have many Evaluations. But how to do you answer these questions? And then where are those answers stored? I spent a long time researching this question and goolging other peoples' solutions. Then, I finally found the answer: There needed to be an Answer model! Of course there needed to be an answer model. Answers had one Question and Questions had many Answers. But how does the app keep track a group of Answers? I needed another model, Finished Evaluations. Now that I had Questions, Answers, Evaluations and Finished Evolutions, and their relationships to Attendees and Instructors in place, the basic structure was complete.

## Custom attribute writer
To satisfy the custom attribute writer requirement, and because it seemed the only logical way to create FinishedEvaluations, I had the FinishedEvaluations class create answers and associate them to an instance of the FinishedEvaluations class. My code is below.

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
I really glad one of the requirements was to write scoped methods. They are amazing, and can be chained together. It makes it easy to select published, upcoming courses by calling Course.published.upcoming.

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

The URL of one of the scopes can be seen at /courses/finished. All of the menu links to the scoped courses can be seen below. 

![](https://iptar.org/wp-content/uploads/2018/02/Selection_003.jpg)

## Reports
I decided to create reports that aggregate course feedback. They exist at this URL pattern: /courses/:id/report

And this is what they look like:

![This](https://iptar.org/wp-content/uploads/2018/02/Selection_004.jpg)

## Evaluations



## Authentication

I used Devise. Overriding some of the default views and controllers was tricky to figure out, but the actual installation and getting Facebook login up and running was easy. 

## Authorization

I learned from using Devise that perhaps I needed a less robust solution than using a complicated gem like CanCanCan. However, rolling my own wasn't simple either, and I'm sure there are many improvements to be made to my various is_authorized? helpers. I 

## Final Thoughts

This project taught me more than I expected about Rails. I suffered through many hours stuck on simple syntax issues. I’ve learned that it’s vital to know what version of Rails any particular piece of advice on Stack Exchange refers to since syntax and ways of doing things change so much from version to version. 
