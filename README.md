# @drewborneman/form-text-tags

A simple tagged textbox Vue.js component derived from the @odyzeo/form-autocomplete
component.

## Installation

### npm

```
npm install --save @drewborneman/form-text-tags
```

### yarn

```
yarn add @drewborneman/form-text-tags
```

Import the component where you want to use it and register it:

```
import 'FormTextTags' from 'form-text-tags';
export default {
  components: {
    FormTextTags,
  },
}
```

Import styles or make your own.

```
import 'form-text-tags/dist/form-text-tags.css';
```

## Usage

```
<template>
  <div
          id="app"
          class="app"
  >
    <div class="container">
      <h1>Demo</h1>

      <p>Adding and removing tags:</p>

      <form-text-tags
              v-model="textBoxTags"
              :close-on-add="false"
              :hide-text-on-blur="false"
              :clear-text-on-blur="false"
              :placeholder="'Add tags here'"
      >
        <template #label>
          Tags
        </template>
      </form-text-tags>

        <p>Textbox value:</p>

        <pre>{{ textBoxTags }}</pre>
    </div>
  </div>
</template>

<script>
  import FormTextTags from '@/components/FormTextTags';

  export default {
    name: 'App',
    components: {
      FormTextTags,
    },
    data() {
      return {
        textBoxTags: [
                "Apple",
                "Orange",
                "Banana",
        ],
      };
    },
  };
</script>

<style lang="less">
  @import '../src/less/app.less';
</style>

```
## Events
### Entered
Returns the text of the item entered as a tag

## Props

| Property name | Type | Default value | Description |
| ------------- | ---- | ------------- | ----------- |
| `value or v-model` | array | `[]` | Array of initial selected option(s) |
| `placeholder` | string | | Input `placeholder` attribute |
| `formErrors` | array | `[]` | Array of errors to display |
| `blurOnAdd` | boolean | `false` | Whether to blur the element after a tag has been added |
| `hideTextOnBlur` | boolean | `false` | Whether to collapse the input area when the element is blurred |
| `clearTextOnBlur` | boolean | `false` | Whether to clear the input area when the element is blurred |



## Slots
### label
Label value for the input element.

### prepend
Div block before the input element, use it to display an icon

### append
Div block after the input element, use it to display an icon

## Development

```
npm run serve
```

or

```bash
yarn serve
```

