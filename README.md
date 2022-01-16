# React + TypeScript App template

## :tractor: DEV

```sh
$ cp .vscode/settings.sample.json .vscode/settings.json
$ npm run start
> open http://localhost:3000/
```

## :rotating_light: ESLint

```sh
$ npx eslint --init
✔ How would you like to use ESLint? · problems
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · react
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser
✔ What format do you want your config file to be in? · JavaScript
✔ Would you like to install them now with npm? · No / Yes
```

### Merge eslint config to `.eslintrc.js` from `package.json`

`package.json`

```diff
{
- "eslintConfig": {
-   "extends": "react-app",
-   "react-app/jest",
- },
}
```

`.eslintrc.js`

```diff
module.export = {
  "extends": [
+   "extends": "react-app",
+   "react-app/jest",
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended"
  ]
```

## :sparkles: Prettier

[prettier](https://prettier.io/docs/en/install.html)

```sh
$ npm i -D --save-exact prettier
$ touch .prettierrc.json
```

### Prettier with ESLint

> If you use ESLint, install eslint-config-prettier to make ESLint and Prettier play nice with each other. It turns off all ESLint rules that are unnecessary or might conflict with Prettier. There’s a similar config for Stylelint: stylelint-config-prettier  
> cf. https://prettier.io/docs/en/install.html#eslint-and-other-linters

```sh
$ npm i -D eslint-config-prettier
```

`.eslintrc.js`

```diff
{
  "extends": [
    "react-app",
    "react-app/jest",
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
+   "prettier"
  ],
```

## Format

```sh
# ESLint
$ npm run lint
# ESLInt --fix
$ npm run lint:fix
# ESLint --fix with Prettier format
$ npm run format
```

### :construction: VSCode settings

Auto format with file save!

`.vscode/setting.json`

```json
{
  "files.exclude": {
    "**/node_modules": true
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "[markdown]": {
    "files.trimTrailingWhitespace": false
  }
}
```

---

## Path alias

- `@/*` -> `./src/*`
- `~/*` -> `./public/*`

### tsconfig

```sh
$ touch tsconfig.paths.json
```

`tsconfig.paths.json`

```json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"],
      "~/*": ["public/*"]
    }
  }
}
```

`tsconfig.json`

```diff
{
+ "extends": "./tsconfig.paths.json",
  "compilerOptions": {
```

### Webpack path.resolve

[react-app-rewired](https://github.com/timarney/react-app-rewired)

```sh
$ npm i -D react-app-rewired
$ touch config-overrides.js
```

`config-overrides.js`

```js
const path = require('path');

module.exports = (config) => {
  config.resolve = {
    ...config.resolve,
    alias: {
      ...config.alias,
      '@': path.resolve(__dirname, './src'),
      '~': path.resolve(__dirname, './public'),
    },
  };

  return config;
};
```

`.eslintignore`

```
/config-overrides.js
```

#### Replace react-script to react-app-rewired

`package.json`

```diff
{
  "scripts": {
-   "start": "react-scripts start",
-   "build": "react-scripts build",
-   "test": "react-scripts test",
-   "eject": "react-scripts eject",
+   "start": "react-app-rewired start",
+   "build": "react-app-rewired build",
+   "test": "react-app-rewired test",
+   "eject": "react-app-rewired eject",
}
```

### :gear: VSCode settings for auto-completion of path aliases

`.vscode/settings.json`

```diff
{
+ "path-autocomplete.pathMappings": {
+   "@": "${folder}/src",
+   "~": "${folder}/public"
+ }
}
```

### Resolve path alias for ESLint + TypeScript

[eslint-import-resolver-typescript](https://www.npmjs.com/package/eslint-import-resolver-typescript)

```sh
$ npm i -D eslint-import-resolver-typescript
```

`.eslintrc.js`

```diff
{
  "settings": {
    "react": {
      "version": "detect"
    },
+   "import/resolver": {
+     "typescript": {
+       "project": "./tsconfig.json"
+     }
+   }
  },
  "extends": [
```

---

## Remove unused imports

[eslint-plugin-unused-imports](https://github.com/sweepline/eslint-plugin-unused-imports)

```sh
$ npm i -D eslint-plugin-unused-imports
```

`.eslintrc.json`

```diff
{
  "plugins": [
    "react",
    "@typescript-eslint",
+   "unused-imports"
  ],
  "rules": {
+   "@typescript-eslint/no-unused-vars": "off", // or "no-unused-vars": "off",
+   "unused-imports/no-unused-imports": "error",
+   "unused-imports/no-unused-vars": [
+     "warn",
+     { "vars": "all", "varsIgnorePattern": "^_", "args": "after-used", "argsIgnorePattern": "^_" }
+   ]
  }
}
```

---

# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).
