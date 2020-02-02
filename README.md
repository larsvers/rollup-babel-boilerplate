# Rollup projet set up

1. Create a project folder
2. Add `.gitignore` excluding `.DS_Store` and `node_modules`
3. `git init` and `npm init`
4. Add a `src` folder and `src/index.js`
5. Add a `dist` folder and gitignore it
6. Install packages

   ```bash
   npm i -D rollup // bundling
   npm i -D @rollup/plugin-node-resolve // to resolve to `node_module` packages
   npm i -D @rollup/plugin-commonjs // converts CJS to ESM as rollup needs ESM
   npm i -D rollup-plugin-babel // to use babel with rollup
   npm i -D @babel/core @babel/preset-env // babel
   ```

7. Create `rollup.config.js`

   ```js
   import resolve from '@rollup/plugin-node-resolve';
   import commonjs from '@rollup/plugin-commonjs';
   import babel from 'rollup-plugin-babel';

   export default {
     input: 'src/index.j',
     output: {
       file: 'dist/bundle.js',
       type: 'umd'
     },
     plugins: [
       resolve(), // resolve `node_modules` paths
       commonjs(), // convert cjs to ESM (for rollup to convert back to cjs)
       babel({
         exclude: 'node_modules/** // this excludes transpiling node_modules
       })
     ]
   }
   ```

8. Set up `.babelrc` at root (or in `src`)

   ```js
   {
     plugins: ['@babel/preset-env`'];
   }
   ```

9. Define browsers you want to cater for

   ```json
   /* package.json */
   "browserlist": "> 0.25%, not dead"
   ```

## Rollup

Rollup bundles Javascript using the ES6 module system into a single file. This file can be of different types - `cjs`, `iife` or `umd` (the safest bet) and applies tree-shaking to remove unused code.

ES6 module files in → single bundle file out.

## Babel

Great [post](https://blog.jakoblind.no/babel-preset-env/) about what Babel and what preset-env does.

In essence, Babel core doesn't do anything out of the box. For each ES6 feature you want to transpile, you need a plugin like e.g. `@babel/plugin-transform-arrow-functions` for arrow functions. Here's [a list](https://babeljs.io/docs/en/plugins/) of all plugins.

We tell Babel what plugin to use with the `.babelrc` file we add to root:

```js
{
  plugins: ['@babel/plugin-transform-arrow-functions'];
}
```

But instead of installing each plugin yourself Babel and others have bundled some plugins to **presets**. You just install one and have a bunch of plugins.

`preset-env` is the smartest of the presets. Upon install you have **all** plugins in one go. However, before it transpiles any of your code Babel checks the `.browserlistrc` file or the `browserlist` property in `package.json` with which you control what browsers you support. Either by naming them directly, setting usage thresholds or regions. Check the list of options [here](https://github.com/browserslist/browserslist#full-list).

This means, Babel doesn't have to throw the kitchen sink at each each function to please old browsers your users aren't using. It can just transpile the features your users' browsers don't support.
