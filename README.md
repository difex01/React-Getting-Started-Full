# React Getting Started Full

## 1. Create project with Vite

Open your terminal and run the next command:

```cmd
npm create vite@latest
```

Select next options:

```cmd
√ Project name: ... react-full
√ Select a framework: » React
√ Select a variant: » TypeScript
```

Now install dependencies
```cmd
npm i
```

## 2. ESLint

### 2.1. ESLint installation

Install ESLint as a dev dependency

```cmd
npm i eslint -D
```

To create ESLint configurations run the next command

```cmd
npx eslint --init
```

```cmd
✔ How would you like to use ESLint? · To check syntax, find problems, and enforce code style
✔ What type of modules does your project use? · JavaScript modules (import/export)
✔ Which framework does your project use? · react
✔ Does your project use TypeScript? · Yes
✔ Where does your code run? · browser
✔ How would you like to define a style for your project? · Use a popular style guide
✔ Which style guide do you want to follow? · Standard: https://github.com/standard/eslint-config-standard-with-typescript
✔ What format do you want your config file to be in? · JavaScript
✔ Would you like to install them now? · Yes
✔ Which package manager do you want to use? · npm
```

### 2.2. ESLint configurations

To avoid problems executing `npx eslint` to review our files, let's add this configurations:

**.eslintrc.cjs**

```js
...
  extends: [
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'standard-with-typescript' // Added
  ],
  overrides: [
  ],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: './tsconfig.json' // Added
  },
...
```

Create a new file named `.eslintignore` and add the next values to be ignored.
*__node_modules__ is added by default, so there's no need to add it on the file.*

**.eslintignore**

```
dist
vite-env.d.ts
vite.config.ts
```

If you got this warning issue *Parsing error: Cannot read file '\tsconfig.json'* just add this configuration

**.eslintrc.cjs**

```js
...
parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: './tsconfig.json',
    tsconfigRootDir: __dirname, // Added
  },
...
```

## 3. Prettier

### 3.1. Prettier Installation


Install Prettier as a dev dependency
```cmd
npm i prettier -D
```

### 3.1. Prettier configurations

Create a file named `.prettierrc`

To add rules, you can go to the [Prettier](https://prettier.io) web page and see all [options](https://prettier.io/docs/en/options.html).
Here's an example

**.prettierrc**

```json
{
  "singleQuote": true,
  "jsxSingleQuote": true,
  "trailingComma": "all",
  "arrowParens": "avoid"
}
```

Create a file named `.prettierignore` and add the next values to be ignored.
*__node_modules__ is added by default, so there's no need to add it on the file.*

**.prettierignore**

```
dist
package-lock.json
```

## 4. ESLint and Prettier coexistence

Let's install a library to use it as a extend configuration as a dev dependency.
```cmd
npm i eslint-config-prettier -D
```

Add this dependency as a extend value

**.eslintrc.cjs**

```js
...
  extends: [
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'standard-with-typescript',
    'eslint-config-prettier', // Added
  ],
```

## 5. App.tsx fix issues

To avoid *Missing return type on function.* update `App` function

**App.tsx**

from this

```tsx
function App() {
```

to this

```tsx
const App: React.FC = () => {
```

## 6. Check code with commands

Run this commando to check if there are files or an specific one with errors

```cmd
npx eslint .
```

Also, you can create scripts on `package.json`

**package.json**

```json
...
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "format": "prettier --write .", // Added
    "lint": "eslint --fix . --ext .ts,.tsx" // Added
  },
...
```

## 7. Husky

### 7.1. Husky intallation

You should already have initialized a repository.

Install husky with this command

```cmd
npx husky-init && npm install
```

This will create a new script on `package.json`

**package.json**

```json
...
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "format": "prettier --write .",
    "lint": "eslint --fix . --ext .ts,.tsx",
    "prepare": "husky install" // Created
  },
...
```

This also create a new directory `.husky` on the root of the project.
This new directory has a file named `pre-commit` which has the commands you want to run on different phases of development with git.

## 8. lint-staged

### 8.1. lint-staged intallation

Install eslint-staged with this command

```
npm i lint-staged -D
```

### 8.2. lint-staged configurations

Add these configurations on `package.json`

**package.json**

```json
...
  "devDependencies": {
    ...
  },
  "lint-staged": { // Added
    "*.(js|jsx|ts|tsx)": [
      "eslint",
      "prettier --check",
      "prettier --write"
    ]
  }
...
```

### 8.3. Configure Husky to run lint-staged

Go to the `pre-commit` file and eplace the current command to use lint-staged

**.husky/pre-commit**

```
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx lint-staged -q
```

## 9. Commit changes

It is important to know that you need to stage your files with `git add .` once you detect errors and want to try to make another commit, because if you do not stage your changes then errors will be detected again and your file will be overwritten by staged files.