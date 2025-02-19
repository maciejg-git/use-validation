# vue-use-validation

Light validation function.

# Usage:

`useValidation` function takes as argument object describing your input. It contains `rules` array, the validation object that stores validation results and number of optional properties to customize validation for specific input. Function returns object that contains functions (`touch`, `formValidate`, `reset`) to perform validation related actions on input.

```typescript
let validation = useValidation(options)
```

### Arguments:

```typescript
{
    name: String,
    rules: Array,
    validation: Object,
    validateOn: String,
    validateMode: String,
}
```

#### Required properties:

- **rules** - an array of validation rules where each item is a name of global validator (`string` or `object`) or object with single function and tested value as argument. Functions should return true for valid values or the string message if the result is invalid. For list of available validators check [validators.js](https://github.com/maciejg-git/vue-use-validation/blob/main/validators.js)
- **validation** - object used to store validation results. It has following properties
    - **status** - object containg the results of validation and the current state of input (for example `touched`, `dirty` etc). It is updated once initially and then after each value change.
    - **state** - final validation state of input. This state is based on current `status` and is updated only when conditions in `validateOn` and `validateMode` options are fulfilled. By default it is empty string for initial state of input, "valid" for valid input and "invalid" for invalid input.
    - **messages** - object containing validation messages.

#### Optional properties:

- **name** - name of input. Name is required when adding multiple inputs.
- **validateOn** - defines conditions to start validation. Possible conditions are:
    - "blur" - validate after input loses focus (default)
    - "immediate" - starts validating immediately after first input
    - "form" - validate after calling formValidate function
- **validateMode** - defines how to update state according to validation results. Possible modes are:
    - "silent" - valid values does not change inputs validaton state to valid unless it was invalid before (only for validate on blur)(default)
    - "eager" - invalid and valid values always change inputs validation state

### Returns:

useValidation function returns single object:

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
