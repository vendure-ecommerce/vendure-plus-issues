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
