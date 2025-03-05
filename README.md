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
  <input 
    v-model="msg" 
    :class="{ valid: state === 'valid', invalid: state === 'invalid' }" 
    @blur="validation.touch()"
  />
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
