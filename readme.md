# HarpiyaUI UI

A set of components and utilities for rapid UI development.

HarpiyaUI UI components are built using Vue 3 and Tailwind. Along with components,
it also ships with directives and utilities that make UI development easier.

<details>
  <summary>Show components</summary>
  <img src="https://user-images.githubusercontent.com/9355208/152111395-a62e1599-112c-450f-a32a-5debc791eb2e.png" width="80%" />
</details>

## Installation

```sh
npm install harpiya-ui
# or
yarn add harpiya-ui
```

Now, import the HarpiyaUIUI plugin and components in your Vue app's `main.js`:

```js
import { createApp } from 'vue'
import { HarpiyaUIUI, Button } from 'harpiya-ui'
import App from './App.vue'
import './index.css'

let app = createApp(App)
app.use(HarpiyaUIUI)
app.component('Button', Button)
app.mount('#app')
```

In your `tailwind.config.js` file, include the harpiya-ui preset:

```js
module.exports = {
  presets: [
    require('harpiya-ui/src/utils/tailwind.config')
  ],
  ...
}
```

## Components

HarpiyaUI UI ships with a bunch of components. To use a component, you can import it directly from `harpiya-ui`:

```html
<template>
  <button>Click me</button>
</template>
<script>
  import { Button } from 'harpiya-ui'
  export default {
    components: {
      Button,
    },
  }
</script>
```

You can also register components on the root `app` so that you don't have to import them in every component.

`main.js`

```js
import { createApp } from 'vue'
import { Button, Input } from 'harpiya-ui'

let app = createApp(App)
app.component('Button', Button)
app.component('Input', Input)
app.mount('#app')
```

### Alert

```html
<Alert title="Info">Your account has been created.</Alert>
```

### Avatar

```html
<Avatar label="John Doe" />
<Avatar label="John Doe" imageURL="https://picsum.photos/200" />
```

### Badge

```html
<Badge>Open</Badge>
<Badge color="green">Completed</Badge>
<Badge color="red">Error</Badge>
<Badge color="yellow">Closed</Badge>
<Badge color="blue">Running</Badge>
```

### Button

```html
<button>Default</button>
<button type="primary">Primary</button>
<button type="danger">Danger</button>
<button type="white">White</button>
<button icon="x" />
<button icon-left="menu">Menu</button>
<button icon-right="external-link">Link</button>
<button :loading="true">Loading</button>
```

### Card

```html
<Card title="Heading" subtitle="Sub text">
  <div class="text-base">Card content</div>
</Card>
```

### Dialog

The Dialog component uses `teleport` feature and requires `#modals` to exist.
Make sure you add a `<div id="modals"></div>` before the end of your body tag.

```html
<button @click="dialogOpen = true">Open Dialog</button>
<dialog title="This is Dialog" v-model="dialogOpen">
  <div class="text-base">Dialog content</div>
</dialog>
```

### Dropdown

The Dropdown component uses `teleport` feature and requires `#popovers` to exist.
Make sure you add a `<div id="popovers"></div>` before the end of your body tag.

```html
<Dropdown :items="[{ label: 'Option 1' }, { label: 'Option 2' }]">
  <template v-slot="{ toggleDropdown }">
    <button @click="toggleDropdown()">Open Dropdown</button>
  </template>
</Dropdown>
```

### ErrorMessage

```html
<ErrorMessage message="There was an error" />
```

### FeatherIcon

Uses [`feather-icons`](https://github.com/feathericons/feather) under the hood.

```html
<FeatherIcon class="h-4 w-4" name="menu" />
<FeatherIcon class="h-4 w-4" name="circle" />
<FeatherIcon class="h-4 w-4" name="arrow-left" />
<FeatherIcon class="h-4 w-4" name="arrow-right" />
```

### GreenCheckIcon

```html
<GreenCheckIcon class="h-4 w-4" />
```

### Input

```html
<input label="Text" type="text" value="" placeholder="Text" />
<input label="Long Text" type="textarea" value="" placeholder="Textarea" />
<input
  label="Select"
  type="select"
  value=""
  :options="['Option 1', 'Option 2']"
/>
<input label="Check" type="checkbox" value="" />
```

### ListItem

```html
<ListItem title="List Item 1" subtitle="Sub text 1">
  <template #actions>
    <button icon="more-horizontal" />
  </template>
</ListItem>
<ListItem title="List Item 2" subtitle="Sub text 2" />
```

### LoadingIndicator

```html
<LoadingIndicator />
```

### LoadingText

```html
<LoadingText />
```

### Spinner

```html
<Spinner class="w-5" />
```

### SuccessMessage

```html
<SuccessMessage message="Completed successfully" />
```

## Directives

### onOutsideClick

This directive is used when you want to execute a function when the user clicks outside of a target element. For e.g., when user clicks outside a dropdown, the dropdown should close.

```html
<button v-on-outside-click="handleOutsideClick">Click me</button>
```

## Utilities

### call

This function wraps `fetch` API. It is built for making web requests to a HarpiyaUI server.

```js
call('harpiya.client.get_value', {
  doctype: 'ToDo',
  filters: { name: 'adsfasdf' },
  fieldname: 'description',
})
```

### resources

This is a helper for managing async data fetching in Vue apps that work with a HarpiyaUI backend.

```html
<template>
  <div>
    <LoadingText v-if="$resources.todos.loading" />
    <div v-for="todo in $resources.todos.data || []" :key="todo.name">
      <div>{{ todo.description }}</div>
      <Badge>{{ todo.status }}</Badge>
    </div>
    <ErrorMessage message="$resources.todos.error" />
  </div>
</template>
<script>
  import { Badge, LoadingText, ErrorMessage } from 'harpiya-ui'

  export default {
    name: 'ToDos',
    resources: {
      todos: {
        method: 'harpiya.client.get_list',
        params: {
          doctype: 'ToDo',
          fields: ['*'],
        },
      },
    },
    components: {
      Badge,
      LoadingText,
      ErrorMessage,
    },
  }
</script>
```

### socketio

This module pre-configures a socketio instance on the port 9000. If you install the HarpiyaUIUI plugin, `this.$socket` will be available in all Vue components.

Usage:

```js
this.$socket.on('list_update', (data) => {
  // handle list update event
})
```

### tailwind.config

This is a [tailwind preset](https://tailwindcss.com/docs/presets) that customizes the standard tailwind config to include HarpiyaUI design tokens.

Usage:

```js
module.exports = {
  presets: [
    require('harpiya-ui/src/utils/tailwind.config')
  ],
  ...
}
```

## Vue Plugin

Vue plugin that installs call, resources and socketio in your Vue app

`main.js`

```js
import { createApp } from 'vue'
import { HarpiyaUIUI } from 'harpiya-ui'
import App from './App.vue'

let app = createApp(App)
app.use(HarpiyaUIUI)
app.mount('#app')
```

You can now use these features in your Vue components.

```html
<script>
  export default {
    resources: {
      ping: 'harpiya.handler.ping',
    },
    mounted() {
      this.$call('ping')
      this.$socket.on('list_update', (data) => {
        // handle list update event
      })
    },
  }
</script>
```

## License

MIT
