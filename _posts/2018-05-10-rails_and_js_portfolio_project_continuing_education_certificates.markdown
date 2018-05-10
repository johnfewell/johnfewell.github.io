---
layout: post
title:      "Rails & JS Portfolio Project: Continuing Education Certificates"
date:       2018-05-10 13:41:09 -0400
permalink:  rails_and_js_portfolio_project_continuing_education_certificates
---


## Background
A few months ago, I completed my Rails portfolio project. For that project, I decided to tackle a problem that I was very familiar with from my work. The post-graduate training institute where I work is certified by New York State to issue continuing education credits to licensed social workers, art therapists, and psychoanalysts. Attending programs or classes that grant these continuing education credits is required of all licensed professionals in new york state in order for those with these licenses to maintain their licenses. 

People who want these credits must first attend a qualified program. Then, they fill out an evaluation of the program in order to obtain a certificate which they can submit to the state to prove that they attended the program and earned the credits they need to renew their license.

I decided to add JS to the evaluation model of my app since that had the most obvious need for it, especially for adding new questions to a new evaluation without having to create them on a separate page and then add them to the evaluation later.

## Index page
In order to keep the JS from throwing an error and 

```
function addEvalsToDom(evalsArray) {
	evalsArray.forEach(function(eval) {
		//javascript optional parameters
		eval.course = eval.course || {}
		const newEval = new Evaluation(eval.id, eval.name, eval.course.title, eval.course.start_date)
		$('#evaluations-main').append(newEval.renderEval())
	})
}
```

```
          Class: ${this.course || "Not assigned"}
          Date: ${this.start_date || "Not assigned"}
```
## Show page

Setting up the ES 5 templates and append them went fairly smoothly at first. However, I had a hard time displaying the JSON has_many relationship which for an evaluation was questions.  When an evaluation had a collection of JSON question object and I tried to iterate over them use `.map`, I always got `object Object` returned. 

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

Success!

![Solution to object Object JSON issue in Rails](https://i.imgur.com/z8hPjQS.jpg)
## New Resource

cocoon gem

```
  <div id="question-forms" class="field mx-5 mt-2">
    <div class='links'>
      <%= link_to_add_association 'add question', f, :questions %>
    </div>
  </div>
```

```
<div class='nested-fields'>
  <div class="field">
    <%= f.label "Question:" %>
    <%= f.text_field :content %><br>
  </div>
  <div class="field">
    <%= f.label "Check if this is a text question, leave it blank for a 1-5 rating scale" %>:
    <%= f.check_box :text %><br>
  </div>
  <%= link_to_remove_association "remove question", f %>
</div>
```


```
  $(function () {
      $('form').submit(function(event) {
      event.preventDefault();

      let values = $(this).serialize();
      let posting = $.post('/evaluations', values);

      posting.done(function(evaluation) {
        let evalUrl = "evaluations/" + evaluation.id
        addEvalToDom(evaluation)
        history.pushState(null, null, evalUrl)
        addEventHandler()
      });
    });
  });
```

## Final Thoughts

It's been a lot of fun learning how to JS to my Rails project. I was especially excited about adding the ability to dynamically add questions to a new evaluation without having to go to another place in the application to create them and then add them after the fact. It makes for a much more intuitive user experience. Cycling through the evaluations is also a nice feature and makes the app more usable. 
