---
layout: post
title:      "Redux: Beware deep state mutations"
date:       2018-07-23 16:29:14 -0400
permalink:  redux_beware_deep_state_mutations
---

One issue I had that was particularly vexing was when I was trying to create this view:
![](https://imgur.com/LBAJ0Sw.png)

I wanted to be able to submit a question on the left, hit enter and have it show up on the right after it had been saved to the database. This is the first thing I tried:

```
    case 'CREATE_QUESTION':
const newQuestion = Object.assign({}, action.payload);
const formId = Object.keys(state)[0]
const formObject = state[formId].questions
return { ...state,
	 questions: {
		 ...state[action.payload.form_id].questions,
		 [newQuestion.id]: newQuestion
 }}
```

The state was never the same, it was always nested incorrectly. 
Then, this code fixed the state so that it was the same when it came out as when it went in, just with the new question appended.

```
    case 'CREATE_QUESTION':
      const newQuestion = Object.assign({}, action.payload);
      let questions = [...state[newQuestion.form_id].questions, newQuestion]
      let newState = {...state}
      newState[newQuestion.form_id].questions = newState[newQuestion.form_id].questions.concat(newQuestion)
      return newState
```

This however did not cause the component on the right to refresh. Why? I was assigning the new data too deeply and Redux didn't detect that the state was now different. 
			 
```
case 'CREATE_QUESTION':
      const newQuestion = Object.assign({}, action.payload);
      let questions = [...state[newQuestion.form_id].questions, newQuestion]
      let newState = {
        ...state,
        [newQuestion.form_id]: {
          questions
        }
      }
```

This worked at last. Becuause I return a fresh copy of the state, Redux saw that the state had indeed changed and refreshed the component. 
