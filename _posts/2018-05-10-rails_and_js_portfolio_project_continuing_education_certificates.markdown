---
layout: post
title:      "Rails & JS Portfolio Project: Continuing Education Certificates"
date:       2018-05-10 17:41:08 +0000
permalink:  rails_and_js_portfolio_project_continuing_education_certificates
---


## Background
A few months ago, I completed my Rails portfolio project. For that project, I decided to tackle a problem that I was very familiar with from my work. The post-graduate training institute where I work is certified by New York State to issue continuing education credits to licensed social workers, art therapists, and psychoanalysts. Attending programs or classes that grant these continuing education credits is required of all licensed professionals in new york state in order for those with these licenses to maintain their licenses. 

People who want these credits must first attend a qualified program. Then, they fill out an evaluation of the program in order to obtain a certificate which they can submit to the state to prove that they attended the program and earned the credits they need to renew their license.

I decided to add JS to the evaluation model of my app since that had the most obvious need for it, especially for adding new questions to a new evaluation without having to create them on a separate page and then add them to the evaluation later

## Initial issues
Setting up the ES 5 templates and append them went fairly smoothly at first. However, there were two persistant issues that I cam up against. 

The first was that when I had a collection of JSON objects and I tried to iterate over them use `.map`, I always got `object Object` returned. 

Here is the initial code:

```
   Evaluation.prototype.renderEval = function() {
     return `
         <div class="d-inline-block mb-1">
           <h2>
              ${this.name}
           </h2>
         </div>
         <div class="f4 text-gray mt-2">
               <span class="mr-3" itemprop="programmingLanguage">
                 Class: ${this.course}
               </span>
             Date: ${this.start_date}
         </div>
         <div class="mt-3"></div>
         <div>
          <h4>Questions</h4>
          <ul id="questions" class="f6 text-gray mt-2 mx-5">
          ${this.questions.map(question => `
            <li>
              ${$('#questions').append(JSON.stringify(question.content))}
            </il>
          `)}
          </ul>
         </div>
     `
   }
```

Produces this result. No matter how much I played around with `JSON.stringify`, the result was always `object Object,`

![object Object JSON problem](https://i.imgur.com/TO4MXFQ.jpg)

However, when paused in dev tools, this is what question looks like when passed from `questions.map`:

![](https://i.imgur.com/opsLNEo.jpg)

It looks like calling `question.content` should work, since it exists at this point in the `map()` function. 

Eventually, I decided to try removing the nesting of the questions from the template. I did so by using `reduce()`

```
   let questionList = this.questions.reduce((questionString, question) => {
     return questionString + `<li>${question.content}</li>`
   }, "" )
```

And then I called the `questionList` variable in the template. The full template is below. 

```
Evaluation.prototype.renderEval = function() {
   let questionList = this.questions.reduce((questionString, question) => {
     return questionString + `<li>${question.content}</li>`
   }, "" )
   return `
       <div class="d-inline-block mb-1">
         <h2>
            ${this.name} - ${this.id}
         </h2>
       </div>
       <div class="f4 text-gray mt-2">
             <span class="mr-3" itemprop="programmingLanguage">
               Class: ${this.course || "Not assigned"}
             </span>
           Date: ${this.start_date || "Not assigned"}
       </div>
       <div class="mt-3"></div>
       <div>
        <h4>Questions</h4>
        <ul id="questions" class="f6 text-gray mt-2 mx-5">
          ${questionList}
        </ul>
       </div>
       <div class="mt-3">

       </div>
       <a href="#" class="js-prev btn" data-id="${this.previous}">Previous Evaluation</a>
       <a href="#" class="js-next btn" data-id="${this.next}">Next Evaluation</a>
   `}
```
## Index page


## Show page
## New Resource
## Final Thoughts
