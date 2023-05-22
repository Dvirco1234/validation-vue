<!-- [![npm version](https://badge.fury.io/js/angular2-expandable-list.svg)](https://badge.fury.io/js/angular2-expandable-list)
[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier) -->

# Validation Vue

[![npm version](https://badge.fury.io/js/validation-vue.svg)](https://badge.fury.io/js/validation-vue) [![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

Validation-Vue is a powerful Vue.js package that provides components for form validation, including Form, Input, and Select. These components enable you to easily implement form validation logic in your Vue.js applications, ensuring data integrity and enhancing user experience.

## Features

<!-- - Support for multiple validation rules
- Real-time validation feedback
- Easy integration with Vue.js applications
- Lightweight and performant -->

-   **Form Component:** The Form component serves as a wrapper for your form elements, providing a convenient way to manage the form's state and trigger form submission. It emits a "submitForm" event when the form is submitted, providing a boolean parameter indicating whether the form is valid based on the defined validation rules.
-   **Input Component:** The Input component is a versatile input field that supports various input types such as text, email, phone, credit card, and textarea. It provides real-time validation feedback to the user, allowing you to define validation rules for each input field. The supported validation rules include required fields, minimum and maximum lengths, pattern matching, and more. Additionally, the Input component supports both function-based validations and validation names, allowing you to easily implement custom validation logic. It also supports asynchronous validation, enabling you to perform validation checks that require server-side or asynchronous operations. As the library evolves, more input types and validation options will be added to enhance its versatility.
-   **Select Component:** The Select component is an enhanced custom select input with additional features. It not only allows users to select options from a dropdown but also provides a search functionality for easily finding desired options. It also supports validation to ensure a valid selection is made.
-   **Extensibility:** The Validation-Vue package is designed to be easily extendable, allowing you to add more components and validation rules to suit your specific requirements. You can create your custom components and integrate them seamlessly into the validation framework.

## Prerequisites

This project requires NodeJS (version 12 or later) and NPM. [Node](http://nodejs.org/) and [NPM](https://npmjs.org/) are really easy to install. To make sure you have them available on your machine, try running the following command.

```sh
$ npm -v && node -v
8.11.0
v16.15.1
```

## Table of Contents

- [Features](#features)
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Usage](#usage)
  - [Local Usage](#local-usage)
  - [Global Usage](#global-usage)
  <!-- - [Serving the app](#serving-the-app) -->
- [Form Events](#form-events)
- [Properties](#properties)
  - [Shared Properties](#shared-properties)
  - [Input Properties](#input-properties)
  - [Select Properties](#select-properties)
- [Examples](#examples)
- [Contribution](#contribution)
- [Authors](#authors)
- [License](#license)

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

## Installation

**BEFORE YOU INSTALL:** please read the [prerequisites](#prerequisites)

To install and set up the library, run:

```sh
$ npm install validation-vue
```

Or if you prefer using Yarn:

```sh
$ yarn add validation-vue
```

## Usage

### Local Usage

my-component.vue
```js
<template>
  <div>
    <v-form @submitForm="onSubmit">
      <v-input :idx="0" id="input" v-model="name" required/>
      <v-select :idx="1" id="select" v-model="count" :options="[1, 2, 3]" required/>
      <v-input :idx="2" id="input" v-model="email" :rules="['email']" required />
      <button type="submit">Submit</button>
    </v-form>
  </div>
</template>
<script>
import VForm from './v-form.vue'
import VInput from './v-input.vue'
import VSelect from './v-select.vue'

export default {
  data() {
    return {
      name: '',
      count: 0,
      email: '',
    }
  },
  methods: {
    onSubmit(isValid) {
      // Handle form submission
      if (isValid) {
        // Perform actions for valid form
      } else {
        // Handle invalid form
      }
    },
  },
  components: {
    VForm,
    VInput,
    VSelect
  }
}
</script>
```

### Global Usage

main.js

```js
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'

import { VForm, VInput, VSelect } from 'validation-vue'

const app = createApp(App)
app.component('VForm', VForm)
app.component('VInput', VInput)
app.component('VSelect', VSelect)

app.mount('#app')
```

<!-- ### Serving the app

```sh
$ npm start
``` -->
## Form Events

The `<VForm>` component emits the following event:

- **submitForm**: Emitted when the form is submitted. The event handler receives a boolean value indicating the form's validity (true for valid, false for invalid).

## Properties

### Shared Properties

| Property           | Type       | Default        | Description                                    |
| ------------------ | ---------- | -------------- | ---------------------------------------------- |
| `id`           | `String`    | **_required_**  | The unique identifier for the component.               |
| `idx`          | `Number`    | **_required_**  | The index of the input element inside the form. This prop is best used in a `v-for` loop. If the input component is not inside a loop, you need to manually add the index.                            |
| `required`         | `Boolean`  | `false`        | Indicates whether the input / select is required.                    |
| `primaryColor` | `String`    | `'#005FAA'`   | The primary color for the component.                    |
| `errorColor`   | `String`    | `'#c10015'`   | The color to indicate an error state in the component.  |

### Input Properties

Properties define the behavior of the `v-input` component:

| Property           | Type       | Default        | Description                                                 |
| ------------------ | ---------- | -------------- | ----------------------------------------------------------- |
| `type`             | `String`   | `'text'`       | The type of input.                                          |
| `placeholder`      | `String`   | `''`           | The placeholder text for the input.                         |
| `label`            | `String`   | `''`           | The label text for the input.                               |
| `rules`            | `Array`    | `[]`           | The `rules` prop is an array of validation rules that can be applied to the input or select component. It allows you to specify various validation conditions to validate the entered value. The rules can be defined either as strings or as pointers to validation functions from the parent element. When using strings, you can provide names of pre-defined validation options such as 'required', 'email', 'cc' etc. These validation options represent common validation rules that can be applied to the input.   Alternatively, you can define custom validation rules using pointers to validation functions from the parent element. Each validation function should take the value as a parameter and return an object with two properties: `isValid` and `errorMessage`. `isValid` indicates whether the value passes the validation condition, and `errorMessage` provides the error message to display when the validation fails. Here's an example: rules: `[(value) => ({ isValid: /[A-Z]/.test(value), errorMessage: 'Password must contain at least 1 uppercase character' })] `                            |
| `asyncRule`        | `Function` | `null`         | A pointer to an asynchronous validation function. Please refer to [Async Example](#async-example) below for an illustration of the `asyncRule` usage.                 |
| `showValidIcon`    | `Boolean`  | `false`        | Indicates whether to show a valid icon.                     |
| `showPasswordIcon` | `Boolean`  | `false`        | Indicates whether to show a password icon.                  |
| `required`         | `Boolean`  | `false`        | Indicates whether the input is required.                    |
| `isChecklist`      | `Boolean`  | `false`        | Indicates whether the input has a checklist of validations. |
| `isChecklistGrid`  | `Boolean`  | `true`         | Indicates whether to use a grid layout for checklist items. |
| `isDigitsOnly`     | `Boolean`  | `false`        | Indicates whether the input should accept only digits.      |
| `maxLength`        | `Number`   | `Infinity`     | The maximum number of characters allowed in the input.      |
| `minLength`        | `Number`   | `0`            | The minimum number of characters required in the input.     |
| `textareaRows`     | `Number`   | `0`            | The number of rows for a textarea input.                    |
| `errorColor`       | `String`   | `'#c10015'`    | The color of the error message and error icon.              |
| `validColor`       | `String`   | `'#7CB261'`    | The color of the success icon.                              |
| `placeholderColor` | `String`   | `'#7B97AC'`    | The color of the placeholder text.                          |
<!-- | `id`               | `String`   | **_required_** | The unique identifier for the input element.                |
| `idx`              | `Number`   | **_required_** | The index of the input element inside the form. This prop is best used in a `v-for` loop. If the input component is not inside a loop, you need to manually add the index (for every element in the form, not only input elements).                             | -->
<!-- | `primaryColor`     | `String`   | `'#005faa'`    | The primary color used for styling.                         |
| `txtColor`         | `String`   | `'#7B97AC'`    | The color of the label and input text.                      | -->

### Select Properties

Properties define the behavior of the `v-select` component:

| Property           | Type       | Default        | Description  |
| ------------------ | ---------- | -------------- | -------------|
| options        | Array     | []      | An array of options for the Select component. The options can either be primitive values (e.g. `[1,2,3]` or `['USA', 'Canada', 'Israel']`) or objects with `{ label: '', key: '' }` structure.             |
| iconColor      | String    | '#005FAA' | The color of the arrow icon.                             |
| bgColor      | String    | '#fff' | The backgroung color of the select.                             |
| selectLabel    | String    | ''      | The label for the select component.                        |
| isInsideLabel  | Boolean   | false   | Determines if the label should be inside the select box.   |
| defaultOption  | Object    | { label: 'Choose', key: '' } | The default option for the select component.        |
| isSearch       | Boolean   | false         | Determines if a search functionality should be enabled.    |


## Examples

#### Async Example

```js
<template>
  <VInput :idx="0" id="input" v-model="name" :asyncRule="validateAsync"/>
</template>
<script>
import { VInput } from 'validation-vue'
export default {
  data() {
      return {
          name: '',
      }
  },
  methods: {
  validateAsync(val) {
    return new Promise(resolve => {
      setTimeout(() => {
        resolve({
            isValid: val.includes('123'),
            errorMessage: 'Username must include 123',
          })
      }, 2000)
    })
  },
},
  components: { VInput }
}
</script>
```

## Contribution

Contributions to the Validation-Vue package are welcome! If you find any issues or have suggestions for improvement, please open an issue on the [GitHub repository](https://github.com/Dvirco1234/validation-vue) or submit a pull request.

This task will create a distribution version of the project inside your local `dist/` folder

<!-- ## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags).-->

## Authors

-   **Dvir Cohen** - _Initial work_ - [Dvirco1234](https://github.com/Dvirco1234)

<!-- See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project. -->

## License

[MIT License](https://github.com/Dvirco1234/validation-vue/blob/main/LICENSE.md) © Dvir Cohen 
