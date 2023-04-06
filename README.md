# Vendure Plus Issues
This repo is for reporting issues with the paid Vendure Plus plugins:

- Advanced Search Plugin
- Gift Card Plugin
- Wishlist Plugin

## Setup Instructions

1. Associate the `@vendure-plus` scope with the Vendure package registry:
   ```bash
   npm config set @vendure-plus:registry https://registry.vendure.io
   ```
2. Log in to the Vendure package registry using the credentials provided when you purchased your license
   ```bash
   npm login --registry=https://registry.vendure.io
   ```
   **Note for npm v9+:** If you use npm version 9, you should add the argument `--auth-type=legacy` with npm adduser or npm login. Otherwise, you will get error `npm ERR! Web login not supported`.
3. If not already done, install the ui-devkit package
   ```bash
   npm install @vendure/ui-devkit
   ```

## Installing in CI

In your CI workflows, you'll need to let the npm client know how to access packages in the `@vendure-plus` organization. 

This is covered in the npm docs [Using private packages in a CI/CD workflow](https://docs.npmjs.com/using-private-packages-in-a-ci-cd-workflow).

An example implementation involves creating a script which is called in CI which can set up a project-specific `.npmrc` file:

```js
// register-vendure-plus.js
const path = require('path');
const fs = require('fs');

const npmRcFilePath = path.join(__dirname, '.npmrc');
const fileContents = [
    `@vendure-plus:registry=https://registry.vendure.io`,
    `//registry.vendure.io/:_authToken="${process.env.VENDURE_PLUS_AUTH_TOKEN}"`,
];
fs.writeFileSync(npmRcFilePath, fileContents.join('\n'), 'utf-8');
console.log(`Generated .npmrc file "${npmRcFilePath}" for access to @vendure-plus registry:`);
```

The value of `VENDURE_PLUS_AUTH_TOKEN` can be found in your local `~/.npmrc` file after logging in following the instructions above. It should then be stored using your CI secrets config, e.g. in GitHub Actions it would be set here:

![image](https://user-images.githubusercontent.com/6275952/228183492-6c43f179-8e84-40d4-9055-b26353a2720f.png)

Then in your workflow config, you'd have a step like this:
```yaml
  - name: Install dependencies
    run: |
      node register-vendure-plus.js
      npm install
    env:
      VENDURE_PLUS_AUTH_TOKEN: ${{ secrets.VENDURE_PLUS_AUTH_TOKEN }}
```
