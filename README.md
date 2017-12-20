

# parcel-plugin-vue [![npm](https://img.shields.io/npm/v/parcel-plugin-vue.svg)](https://www.npmjs.com/package/parcel-plugin-vue) [![david-dm](https://david-dm.org/parcel-bundler/parcel.svg)](https://david-dm.org/lc60005457/parcel-plugin-vue)

> Stability: 1 - Experimental This feature is still under active development and subject to non-backwards compatible changes, or even removal, in any future version. Use of the feature is not recommended in production environments.

<img src="https://img.souche.com/f2e/b1f71b545619350ff92458bbcfa01056.png" align="right" width="140">

__Make Parcel surport Vue single file components.__

[【What's the Parcel】](https://parceljs.org/)[【What's the Vue】](https://vuejs.org/)[【What's the Vue single file components】](https://vuejs.org/v2/guide/single-file-components.html)

## Using Plugin

> Using plugins in Parcel could not be any simpler. All you need to do is install them and save in your package.json. Plugins should be named with the prefix parcel-plugin-, e.g. parcel-plugin-foo. Any dependencies listed in package.json with this prefix will be automatically loaded during initialization.

You must `node >= 8`

```
npm i parcel-plugin-vue -D
```

## Examples

* [default](https://github.com/lc60005457/parcel-vue-demo) - Maybe you like [parcel-plugin-eslint](https://github.com/lc60005457/parcel-plugin-eslint)
* [with ts(official)](https://github.com/lc60005457/parcel-vue-demo/tree/feature/typescript)
* [with parcel-plugin-typescript](https://github.com/lc60005457/parcel-vue-demo/tree/feature/plugin-typescript)

## Make some issues clear

### You Should install parcel-bundler yourself

The plugins for parcel-bundler need a same version of parcel-bundler at runtime.

### You Should install vue / vue-template-compiler yourself

You can choose the version of Vue yourself.

But the version of vue-template-compiler must be eq the version of Vue.

I will move the 'vue-template-compiler' to peerDependencies from devDependencies at next version.

### You Should config Babel yourself

Default, Vue rely on Babel so that need install 'babel-plugin-transform-runtime' and 'babel-preset-es2015'.

But 'babel-preset-es2015' will be replace by 'babel-preset-env'.

__So, We recommend more__:

Make and edit the file '.babelrc' yourself.

### CSS Extraction

You can make a file named 'vue.config.js', edit and save it

```js
module.exports = {
    // If extractCSS is always true, the 'Hot module replacement' will not work.
    extractCSS: process.env.NODE_ENV === 'production'
};
```

For other attributes of 'vue.config.js', you can refer to https://github.com/vuejs/vueify#configuring-options

### Custom Compilers

The plugin for Vue is using built-in compiler compiles the other lang.

Those compilers are:

`coffee`,`babel`
`less`,`sass`,`scss`,`stylus`
`jade`,`pug`

That will allow you to use other parcel plugins to process a part of a Vue component at next version.

But now, you need do it yourself, I'm sorry for this.

You can make a file named 'vue.config.js', edit and save it

```js
var TypeScriptAsset = require('parcel-bundler/src/assets/TypeScriptAsset.js');

module.exports = {
    customCompilers: {
        ts: function (content, cb, compiler, filePath) {
            let ts = new TypeScriptAsset(filePath, {}, {});
            ts.contents = content;
            ts.process().then((res) => {
                cb(null, res.js);
            });
        }
    }
};
```

For 'vue.config.js', you can refer to https://github.com/vuejs/vueify#configuring-options

### This Plugin only support '*.vue'

When you meet this:

> [Vue warn]: You are using the runtime-only build of Vue where the template compiler is not available. Either pre-compile the templates into render functions, or use the compiler-included build.

Maybe in your code:

```js
import Vue from 'vue';

new Vue({
  el: '#app',
  template: '...', // This is reason for Error 
  ...
});
```

You should change to:

```js
import Vue from 'vue/dist/vue.esm.js';

new Vue({
  el: '#app',
  template: '...',
  ...
});
```

__or We recommend more__:

```js
import Vue from 'vue';
import YourVue from 'YourVue.vue';

const app = new Vue({
  el: '#app',
  render: h => h(Index)
});
```
