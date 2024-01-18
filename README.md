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
4. Find your npm auth token for the Vendure registry. This can be found in the `.npmrc` in your home directory (`~/.npmrc` on Linux or MacOS, `C:\Users\<username>\.npmrc` on Windows). Assuming you have successfully logged in as in step 2 above, you should see a line like this in you npmrc file:
   ```
   //registry.vendure.io/:_authToken="<auth token string>"
   ```
   The `<auth token string>` will be needed for when you want to install dependencies on other computers withour requiring a manual login each time, such as in CI. This is covered in the next section.

## Installing in CI

In your CI workflows, you'll need to let the npm client know how to access packages in the `@vendure-plus` organization. To do so, you'll need the auth token you located in step 4 above, and you'll need to add that to a project-specific `.npmrc` file.

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

The value of `VENDURE_PLUS_AUTH_TOKEN` is the token from your `~/.npmrc` file as explained in step 4 above. It should then be stored using your CI secrets config, e.g. in GitHub Actions it would be set here:

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

More information on this process can be found in the npm docs [Using private packages in a CI/CD workflow](https://docs.npmjs.com/using-private-packages-in-a-ci-cd-workflow).
