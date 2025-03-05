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

let validation = useValidation(
  {
    rules,
    mode: "blur-silent",
  },
  (res) => {
    status.value = res.status
    messages.value = res.messages
    state.value = res.state
  }
)

watch(msg, (value) => {
  validation.updateValue(value)
}, { immediate: true })
</script>

<template>
  <h1>{{ msg }}</h1>
  <input v-model="msg" :class="{ valid: state === 'valid', invalid: state === 'invalid' }" @blur="validation.touch()"/>
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

<style>
input {
  outline: none;
}
.valid {
  border-color: green;
}
.invalid {
  border-color: red;
}
</style>
```
