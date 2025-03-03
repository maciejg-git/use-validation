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

const msg = ref()

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
