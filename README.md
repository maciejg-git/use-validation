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

For more examples of components using this function check [Litewind-vue](https://vue-litewind.netlify.app/documentation/form-validation) or [Litewind-alpine](https://litewind-alpine.netlify.app/documentation/validation/) validation documentation.

## Default messages

To change or add default messages for the validators import `validationMessages` object from validators.js file. A message can contain `%d` characters that are replaced with a rule value.

## Default validators

To add new validators import `globalValidators` object from validators.js file.

---

### Todo

- async validators
