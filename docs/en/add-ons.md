Although `electron-webpack` is provided as a single module, you can also install add-ons. Add-ons are made available to help setup certain frameworks that may require a lot of extra configuration or dependencies, such as TypeScript or Vue.js.

These add-ons are completely optional and may not support all use cases. If you need something more custom, please know that the entirety of `webpack`'s documentation still applies to `electron-webpack` ([more info](./extending-as-a-library.md)).

Current Add-ons:

- [JavaScript Frameworks](#javascript-frameworks)
    - [Adding TypeScript support](#adding-typescript-support)
    - [Adding ESLint support](#adding-eslint-support)
  - [React JSX](#react-jsx)
  - [Adding TypeScript support for JSX (`.tsx` files)](#adding-typescript-support-for-jsx-tsx-files)
- [Pre-processors](#pre-processors)
  - [Babel](#babel)
  - [ESLint](#eslint)
  - [TypeScript](#typescript)
  - [Less](#less)
  - [Sass/SCSS](#sassscss)
  - [EJS](#ejs)
  - [Nunjucks](#nunjucks)
- [UI Libraries](#ui-libraries)
  - [iView](#iview)
  - [Element](#element)
- [Miscellaneous](#miscellaneous)
  - [Build Notifications](#build-notifications)

---

## JavaScript Frameworks


#### Adding TypeScript support
Install the [TypeScript](#typescript) add-on, followed by adding the below file to shim Vue component files.

```ts tab="src/renderer/vue-shims.d.ts"
declare module '*.vue' {
  import Vue from 'vue'
  export default Vue
}
```

And of course, make sure to let `vue-loader` know you want to use TypeScript in your component file using the `lang="ts"` attribute.

```html
<template></template>

<script lang="ts">
  /* your TypeScript code */
</script>

<style></style>
```

#### Adding ESLint support
Install the [ESLint](#eslint) add-on, install `eslint-plugin-html`, and add the following additional configurations.

Install `html` plugin to lint Vue component files:
```bash
yarn add eslint-plugin-html --dev
```

```js tab=".eslintrc.js"
module.exports = {
  plugins: [
    'html'
  ]
}
```

### React JSX
Add support for compiling JSX files:

```bash
yarn add @babel/preset-react --dev
```

### Adding TypeScript support for JSX (`.tsx` files)
Install the [TypeScript](#typescript) add-on, followed by extending your `tsconfig.json` to include the jsx compiler option as below:

```json tab="tsconfig.json"
{
    "extends": "./node_modules/electron-webpack/tsconfig-base.json",
    "compilerOptions": {
        "jsx": "react"
    }
}
```

---

## Pre-processors

### Babel

[@babel/preset-env](https://github.com/babel/babel/tree/master/packages/babel-preset-env) is automatically configured based on your `electron` version.

All direct dev dependencies `babel-preset-*` and `babel-plugin-*` are automatically configured also.

### ESLint
Add support for script file linting using `eslint`. Internally uses `eslint`, `eslint-loader`, `eslint-friendly-formatter`, and makes `babel-eslint` available if needed.

```bash
yarn add electron-webpack-eslint --dev
```

Create `.eslintrc.js` in the root directory:

```js tab=".eslintrc.js"
module.exports = {
  /* your base configuration of choice */
  extends: 'eslint:recommended',

  parser: 'babel-eslint',
  parserOptions: {
    sourceType: 'module'
  },
  env: {
    browser: true,
    node: true
  },
  globals: {
    __static: true
  }
}
```

### TypeScript
Add support for compiling TypeScript script files. Internally uses both `ts-loader` and `fork-ts-checker-webpack-plugin` to compile `*.ts`. Note that entry files can also use the `*.ts` extension.

```bash
yarn add typescript electron-webpack-ts --dev
```

Create `tsconfig.json` in the root directory:
```json
{
  "extends": "./node_modules/electron-webpack/tsconfig-base.json"
}
```

### Less
Add support for compiling Less style files.

```bash
yarn add less less-loader --dev
```

### Sass/SCSS
Add support for compiling Sass/SCSS style files.

```bash
yarn add node-sass sass-loader --dev
```

### EJS
Add support for compiling EJS template files.

```bash
yarn add ejs ejs-html-loader --dev
```

### Nunjucks
Add support for compiling Nunjucks template files:

```bash
yarn add nunjucks nunjucks-loader --dev
```

---

## UI Libraries

### iView
Once you have the [Vue.js](#vuejs) add-on installed, `electron-webpack` will internally support iView's "import on demand" feature. No further setup is necessary.

### Element
Once you have the [element-ui](https://github.com/ElemeFE/element) installed, `electron-webpack` will internally support Element's "import on demand" feature. No further setup is necessary.

---

## Miscellaneous

### Build Notifications
Provide OS-level notifications from `webpack` during development.

```bash
yarn add webpack-build-notifier --dev
```
