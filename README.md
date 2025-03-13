# use-validation

![GitHub License](https://img.shields.io/github/license/maciejg-git/use-validation)
![GitHub package.json version](https://img.shields.io/github/package-json/v/maciejg-git/use-validation)

Light validation function.
- vanilla js, framework independent
- 5 modes of validation
- includes common validators with validation messages
- customizable validators and validation messages

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
    messages: Object,
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
- **messages** - Each rule has default generic validation message for the invalid value. You can customize these messages for the input by adding them to the messages property. The key is a rule name and the value is the new message.
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
    <pre>Status: {{ status }}</pre>
  </div>
  <div>
    <pre>Messages: {{ messages }}</pre>
  </div>
  <div>
    <pre>State: {{ state }}</pre>
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
