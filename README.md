# vue-use-validation

Light validation function.

## Usage:

`useValidation` function takes two arguments:
- object that should contain `rules` array and number of optional properties to customize validation for specific input,
- callback that is called after each validation. The results of the validation are in the callback argument.

`useValidation` function returns object that contains functions (`touch`, `formValidate`, `reset`) to perform validation related actions on the input.

```typescript
let validation = useValidation(options, callback)
```

### Arguments:

```typescript
{
    name: String,
    rules: Array,
    mode: String,
},
callback: Function
```

- **rules** - an array of validation rules where each item is a name of validator (`string` or `object`) or an object with function and tested value as argument. Functions should return true for valid values or a string message if the result is invalid. For list of available validators check [validators.js](https://github.com/maciejg-git/vue-use-validation/blob/main/validators.js)
- **name** - name of the input.
- **mode** - is a mode that defines when to validate input and how to update state depending on the validation results:
    - `"blur-silent"` - validate after input loses focus. If the input is valid then do not set state to valid.
    - `"blur-eager"` - validate after input loses focus. Always set state for valid and invalid inputs.
    - `"form-silent"` - validated manually after calling formValidate function. If the input is valid then do not set state to valid.
    - `"form-eager"` - validated manually after calling formValidate function. Always set state for valid and invalid inputs.
    - `"immediate-eager"` - validate on each input update. Always set state for valid and invalid inputs.
- **callback** - function that is called after each validation. The results of the valiation are in the callback argument. See below for the description of the validation results object.

### Validation results

The validation result is an object with following propreties:

- **status** - object containg the results of validation and the current state of the input (for example `touched`, `dirty` etc). It is updated once initially and then after each value change.
- **state** - final validation state of input. This state is based on the current `status` and is updated only when conditions for the `mode` are fulfilled (for example input lost focus etc). By default it is empty string for initial state of input, `"valid"` for valid input and `"invalid"` for invalid input.
- **messages** - object containing validation messages.

### Returns:

`useValidation` function returns single object:

```javascript
{
    name: String,
    touch: Function,
    formValidate: Function,
    updateValue: Function,
    reset: Function,
}
```

#### Functions:

- **updateValue** - function that should be called when the value of the input changes
- **touch** - function that should be set as handler for inputs blur event.
- **formValidate** - function to manually validate and update validation state of the input.
- **resetValidation** - function that resets validation to default state.

## Examples

### Vue

Here is example of `useValidation` in the Vue. Check it online in [Vue SFC Playground](https://play.vuejs.org/#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)

```vue
<script setup>
import { ref, watch } from 'vue'
import useValidation from "./use-validation"

const msg = ref("")

let status = ref({})
let messages = ref({})
let state = ref("")

let rules = ["required", { minLength: 3 }, { maxLength: 5}]

let validation = useValidation({
  rules,
  mode: "blur-silent",
},
(res) => {
  status.value = res.status
  messages.value = res.messages
  state.value = res.state
})

watch(msg, (value) => {
  validation.updateValue(value)
}, { immediate: true })
</script>

<template>
  <h1>{{ msg }}</h1>
  <input v-model="msg" @blur="validation.touch()"/>
  <div>
    Status: {{ status }}
  </div>
  <div>
    Messages: {{ messages }}
  </div>
  <div>
    State: {{ state }}
  </div>
</template>
```
