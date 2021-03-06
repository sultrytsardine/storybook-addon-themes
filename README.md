# Storybook Addon Themes

Greatly inspired by [@storybook/addon-backgrounds](https://github.com/storybooks/storybook/tree/next/addons/backgrounds).

This Storybook Theme Decorator can be used to add a custom HTML class or classes to the the preview in [Storybook](https://storybook.js.org).

![Demo](media/demo.gif)

## Compatibility

This version is compatible with storybook version `5.3.x`.

## Installation

```sh
npm i -D storybook-addon-themes
```

## Getting started

Then activate the addon by adding it to the storybook `main.js` file (located in the Storybook config directory):

```js
module.exports = {
  addons: [
    // Maybe other addons here...
    'storybook-addon-themes'
    // Or here...
  ],
};
```

See the [storybook documentation](https://storybook.js.org/docs/addons/using-addons/) for more informations.

## Parameters

The `themes` parameter accept a list of `Theme` object.

Each `Theme` is an object with the following properties :
* `name` (`string`): Name of the theme
* `class` (`string | string[]`): HTML class(es) associated with the theme
* `color` (`string`): The color of the badge in the theme selector
* `default` (`boolean` - optionnal): Is the theme selected by default ?

The `themes` parameter also accept an object with the following properties :

* `list` (`Theme[]` - optionnal): The list of themes
* `disable` (`boolean` - optionnal): Disable the addon for a story

## Configuration

### Globally

You can configure the themes globally in the storybook `preview.js` file:

```js
import { addParameters } from '@storybook/react'; // <- or your storybook framework

addParameters({
  themes: [
    { name: 'twitter', class: 'theme-twt', color: '#00aced', default: true },
    { name: 'facebook', class: 'theme-fb', color: '#3b5998' },
  ],
});
```

See the [storybook documentation](https://storybook.js.org/docs/addons/using-addons/#global-configuration) for more informations.

### In story (Component Story Format)

Or configure the themes in your story file like this:

```js
export default {
  component: Button,
  parameters: {
    themes: [
      { name: 'twitter', class: ['theme-twt', 'light-mode'], color: '#00aced' },
      { name: 'facebook', class: ['theme-fb', 'dark-mode'], color: '#3b5998' },
    ]
  },
  title: 'CSF|Button',
};
```

If you only want to activate the addon or override the themes for a specific story you can write:

```js
export default {
  component: Button,
  title: 'Button',
};

export const withText = () => <Button onClick={action('clicked')}>Hello Button</Button>;
withText.story = {
  parameters: {
    themes: [
      { name: 'twitter', class: ['theme-twt', 'light-mode'], color: '#00aced' },
      { name: 'facebook', class: ['theme-fb', 'dark-mode'], color: '#3b5998' },
    ]
  },
};
```

### In story (StoriesOf API)

Alternatively with the old StoriesOf API:

```js
import { storiesOf } from '@storybook/react'; // <- or your storybook framework

storiesOf('Button', module)
  .addParameters({
    themes: [
      { name: 'twitter', class: 'theme-twt', color: '#00aced', default: true },
      { name: 'facebook', class: 'theme-fb', color: '#3b5998' },
    ],
  })
  .add('with text', () => <button>Click me</button>);
```

And for a single story:

```js
import { storiesOf } from '@storybook/react';

storiesOf('Button', module)
  .add('with text', () => <button>Click me</button>, {
    themes: [{
      name: 'red', class: 'theme-red', color: 'rgba(255, 0, 0)',
    }]
  });

```

## Usage with decorator

By default the classes will be added to the `body` element.

But in this case your theme will not be visible by other addons (like [@storybook/addon-storyshots](https://github.com/storybookjs/storybook/tree/next/addons/storyshots)).

To fix this you can add the `withThemes` decorator in your stories.

But the decorator method is not available for all frameworks

See [here](#framework-support-table) for the list of supported framework.

### Globally

Setup the decorator globally in the `preview.js` file:

```js
import { addDecorator, addParameters } from '@storybook/react'; // <- or your storybook framework
import { withThemes } from 'storybook-addon-themes/react'; // <- or your storybook framework

addParameters({
  themes: [
    { name: 'twitter', class: 'theme-twt', color: '#00aced', default: true },
    { name: 'facebook', class: 'theme-fb', color: '#3b5998' },
  ],
});
addDecorator(withThemes);
```

### In story (Component Story Format)

Or in your story file (for all stories in that file):

```js
export default {
  component: Button,
  decorators: [ withThemes ],
  parameters: {
    themes: [
      { name: 'twitter', class: ['theme-twt', 'light-mode'], color: '#00aced' },
      { name: 'facebook', class: ['theme-fb', 'dark-mode'], color: '#3b5998' },
    ]
  },
  title: 'CSF|Button',
};
```

Or just for a specific story:

```js
export const withText = () => <Button onClick={action('clicked')}>Hello Button</Button>;
withText.story = {
  decorators: [ withThemes ],
  parameters: {
    themes: [
      { name: 'twitter', class: ['theme-twt', 'light-mode'], color: '#00aced' },
      { name: 'facebook', class: ['theme-fb', 'dark-mode'], color: '#3b5998' },
    ]
  },
};
```

### In story (StoriesOf API)

And alternatively with the old StoriesOf API:

```js
import { storiesOf } from '@storybook/react'; // <- or your storybook framework
import { withThemes } from 'storybook-addon-themes/react';

storiesOf('Button', module)
  .addDecorator(withThemes)
  .add('with text', () => <button>Click me</button>);
```

## Framework Support Table

| | [React](app/react)|[React Native](app/react-native)|[Vue](app/vue)|[Angular](app/angular)| [Polymer](app/polymer)| [Mithril](app/mithril)| [HTML](app/html)| [Marko](app/marko)| [Svelte](app/svelte)| [Riot](app/riot)| [Ember](app/ember)| [Preact](app/preact)|
| ----------- |:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
|Usage without decorator |+| |+|+|+|+|+|+|+|+|+|+|
|Usage with decorator    |+| |+| | | | | | | | | |
