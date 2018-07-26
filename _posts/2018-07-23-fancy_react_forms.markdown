---
layout: post
title:      "Fancy React Forms"
date:       2018-07-23 17:58:52 -0400
permalink:  fancy_react_forms
---

## My React with Redux Final project

For my final project, I decided to try to recreate some of the look of Typeform's interface and unique data collection UI. I have used Typeform extensively at my current job over the last four years and I've always been impressed about how the simple user interface allows for data collection from even some less tech-savvy users. 

## Setting up the API

This was easier than I expected it to be. The many hours I spent on my Rails projects paid off! Serialzing to JSON is a snap with Rails. I had a use a little bit of Ruby to pull the relevent information for from responses out of the database, but that was about it. My code for gathering the information is below. 

```
# forms_controller.rb

  def responses
    keys = []
    values = []
    form = Form.find(params[:id])
    form.form_responses.each do |fe|
      fe.answers.each do |a|
        if !a.content.nil?
          values << a.content
          keys << a.question.content
        end
      end
    end
    pairs = keys.zip(values)
    questions_answers_hash = pairs.group_by(&:first)
    questions_answers_hash.keys.each {
      |k| questions_answers_hash[k] = questions_answers_hash[k].map(&:last) }
    responses = FormResponse.where(form_id: params[:id])
    render json: { name: form.name, responses: questions_answers_hash }
  end
	```

## Early UI Mockup

![Form Editor Mockup](https://i.imgur.com/wEWW2uH.png)

Rather than just stumbling through the design process which I have done in the past, I decided to try making a mockup first. It really helped me wrap my head around what needed to be a container and what needed to be a component in my app. 

## Redux State Obsticles

I had many problems trying to get the form preview screen on the right side of the form editor seen above to update when new questions where created. It was so vexing and complicated, I created a blog post just on 
[the issue of state mutation and deep assignment.](http://http://y9u.org/redux_beware_deep_state_mutations)
TLDR; Make sure you are always returning a new copy of the state in Redux. It's one of the cardinal rules and although I thought I was following it, I wasn't.

## Semantic UI
I deiced to use [Semantic UI ](https://react.semantic-ui.com) to style my app. I also wrote some custom CSS, but using a library allowed me to keep it pretty minimal. 

## React Intersection Observer
One of typeform's hallmark features is that the questions dim when they scroll out of the middle of the viewport. I initially recreated this effect using Semanic UI's dimmer component combined with React Intersection Observer. While this worked, I decided that it was a little too much for each question component to have its own state and have that changed based on it's position in the viewport. Instead, I used a function that tracks the viewport, and I attached event handlers while scrolling. 

## Redux Fetch Issues

I learned so much about both React and Redux during this project. As everyone says, it takes a while to wrap your head around the "loop" of Redux instructions. It initially felt very complicated, but now it seems logical and not mysterious. One of the issues I ran into often and had to figure out was correctly initializing the state the correct way in the reducer so that where would be initial values when a component mounted. For example, the initial state for my forms reducer is structured this way:
```
const initialState = {
  formSubmitStatus: false,
  responsesState: {
    name: '',
    responses: {}
  }
}
```

Another way to gaurd against there not being props because the fetch request isn't complete at the time of the first component render is to simply do this in the render method:

```
  render () {
    if (!this.props.forms) {
      return <Loader />;
    }
    return (...)
		}
		```
		
More specifically, if some child prop isn't available until after the re-render after the fetch is complete, but the parent prop is, you can add this in your render:

```
this.props.forms, form => {
return (
{'questions' in form ? form.questions.length : 0} Questions
)
```

## The Final Product
This is the index view of the app
![](https://imgur.com/jK8cGFs.png)

This is the form edit view:
![](https://imgur.com/LBAJ0Sw.png)

This is the form reponse view intro card:
![](https://imgur.com/xQwWKOD.png)

This what the form content looks like:
![](https://imgur.com/X5jxxZO.png)

This is the submit:
![](https://imgur.com/9RPNokV.png)

This the view to see all of the form repsonses:
![](https://imgur.com/yGnHJq0.png)


