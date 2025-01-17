# Storybook Addon HTML

This addon for Storybook adds a tab that displays the compiled HTML for each
story. It uses [highlight.js](https://highlightjs.org/) for syntax highlighting.

![Animated preview](https://raw.githubusercontent.com/whitespace-se/storybook-addon-html/master/image.gif)

## Getting Started

Install the addon and its dependencies.

With NPM:

```sh
npm i --save-dev @whitespace/storybook-addon-html prettier react-syntax-highlighter
```

With Yarn:

```sh
yarn add -D @whitespace/storybook-addon-html prettier react-syntax-highlighter
```

### Register addon

```js
// .storybook/main.js

module.exports = {
  // ...
  addons: [
    "@whitespace/storybook-addon-html",
    // ...
  ],
};
```

## Usage

The HTML is formatted with Prettier. You can override the Prettier config
(except `parser` and `plugins`) by providing an object following the
[Prettier API override format](https://prettier.io/docs/en/options.html) in the
`html` parameter:

```js
// .storybook/preview.js

export const parameters = {
  // ...
  html: {
    prettier: {
      tabWidth: 4,
      useTabs: false,
      htmlWhitespaceSensitivity: "strict",
    },
  },
};
```

You can override the wrapper element selector used to grab the component HTML.

```js
export const parameters = {
  html: {
    root: "#my-custom-wrapper", // default: #root
  },
};
```

Some frameworks put comments inside the HTML. If you want to remove these you
can use the `removeComments` parameter. Set it to `true` to remove all comments
or set it to a regular expression that matches the content of the comments you
want to remove.

```js
export const parameters = {
  html: {
    removeComments: /^\s*remove me\s*$/, // default: false
  },
};
```

You can also use the `removeEmptyComments` parameter to remove only empty
comments like `<!---->` and `<!-- -->`.

```js
export const parameters = {
  html: {
    removeEmptyComments: true, // default: false
  },
};
```

You can override the `showLineNumbers` and `wrapLines` settings for the syntax
highlighter by using the `highlighter` parameter:

```js
export const parameters = {
  html: {
    highlighter: {
      showLineNumbers: true, // default: false
      wrapLines: false, // default: true
    },
  },
};
```

Another way to hide unwanted code is to define the `customReplacement` option.
You can provide:
- a single search (string or RegEx) that will be replaced by an empty string
- an object containing `search` and `replace` properties
- an array containing one or more of the options aforementioned

```js
// Providing a RegExp
export const parameters = {
  html: {
    customReplacement: /ng-reflect.*?="[\S\s]*?"/g,
  },
};
```

```js
// Providing an object
export const parameters = {
  html: {
    customReplacement: {
      search: /ng-reflect.*?="[\S\s]*?"/g,
      replace: "",
    },
  },
};
```

```js
// Providing an array
export const parameters = {
  html: {
    customReplacement: [
      {
        search: /_nghost.*?="[\S\s]*?"/g,
        replace: "",
      },
      {
        search: "foo",
        replace: "bar",
      },
      /ng-reflect.*?="[\S\s]*?"/g,
      "ng-",
    ],
  },
};
```
