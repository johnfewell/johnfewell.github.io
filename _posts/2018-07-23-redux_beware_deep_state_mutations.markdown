---
layout: post
title:      "Redux: Beware deep state mutations"
date:       2018-07-23 20:29:13 +0000
permalink:  redux_beware_deep_state_mutations
---

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


```
    case 'CREATE_QUESTION':
      const newQuestion = Object.assign({}, action.payload);
      let questions = [...state[newQuestion.form_id].questions, newQuestion]
      let newState = {...state}
      newState[newQuestion.form_id].questions = newState[newQuestion.form_id].questions.concat(newQuestion)
      return newState
```

			 
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
