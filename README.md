# vue-use-validation

Light validation function.

## Usage:

`useValidation` function takes two arguments:
- object that should contain `rules` array and number of optional properties to customize validation for specific input,
- callback that is called after each validation. The results of the validation are in the callback argument.

`useValidation` function returns object that contains functions (`touch`, `formValidate`, `reset`) to perform validation related actions on input.

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

#### Required properties:

- **rules** - an array of validation rules where each item is a name of global validator (`string` or `object`) or object with single function and tested value as argument. Functions should return true for valid values or the string message if the result is invalid. For list of available validators check [validators.js](https://github.com/maciejg-git/vue-use-validation/blob/main/validators.js)
- **callback** - function that is called after each validation. The results of the valiation are in the callback argument. See below for the description of the validation results object.

#### Optional properties:

- **name** - name of input.
- **mode** - is a mode that defines when to validate input and how to update state depending on the validation results:
    - "blur-silent" - validate after input loses focus. If the input is valid then do not set state to valid.
    - "blur-eager" - validate after input loses focus. Always set state for valid and invalid inputs.
    - "form-silent" - validated manually after calling formValidate function. If the input is valid then do not set state to valid.
    - "form-eager" - validated manually after calling formValidate function. Always set state for valid and invalid inputs.
    - "immediate-eager" - validate on each input update. Always set state for valid and invalid inputs.

### Validation results

The validation result is an object with following propreties:

- **status** - object containg the results of validation and the current state of input (for example `touched`, `dirty` etc). It is updated once initially and then after each value change.
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
