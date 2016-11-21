---
title: Externals
sort: 13
contributors:
  - sokra
  - skipjack
  - pksjce
---

`externals` configuration provides a way of excluding a dependency from the bundle. Instead, the created bundle relies on the excluded dependency to be present in the consumer's environment.
This typically applies to library developers though application developers can make good use of this feature too.

## `externals`

`string | regex | function | array | object`

**Prevent bundling** of certain `import`ed packages and instead retrieve these external packages *at runtime*.

For example, to include `jQuery` from a CDN instead of bundling it:

**index.html**

```html
...
<script 
  src="https://code.jquery.com/jquery-3.1.0.js"
  integrity="sha256-slogkvB1K3VOkzAI8QITxV3VzpOnkeNVsKvtkYLMjfk="
  crossorigin="anonymous"
></script>
...
```

**webpack.config.js**

```javascript
externals: {
  jquery: 'jQuery'
}
```

This leaves any dependent modules unchanged, i.e. the code shown below will still work:

```javascript
import $ from 'jquery';

$('.my-element').animate(...);
```

T> __consumer__ here is any end user application that includes the library that you have bundled using webpack.

Your bundle which has external dependencies can be used in various module contexts — mainly [CommonJS, AMD, global and ES2015 modules](/concepts/modules). The external library may be available in any of the above forms but under different namespaces.

`externals` supports the following module contexts:

  * __global__ - An external library can be available as a global variable. The consumer can achieve this by including the external library in a script tag. This is the default setting for externals.
  * __commonjs__ -  The consumer application may be using a CommonJS module system and hence the external library should be available as a CommonJS module.
  * __amd__ - Similar to `commonjs` but using AMD module system.

`externals` can be specified in one of the following forms:

### string

`jQuery` in the externals indicates that your bundle will need `jQuery` variable in the global form.

### array

```javascript
externals: {
  subtract: ['./math', 'subtract']
}
```

`subtract: ['./math', 'subtract']` converts to a parent child construct, where `./math` is the parent module, and your bundle only requires the subset under `subtract` variable.

### object

```javascript
externals : {
  react: 'react'
}

// or

externals : {
  lodash : {
    commonjs: "lodash",
    amd: "lodash",
    root: "_" // indicates global variable
  }
}
```

This syntax is used to describe all the possible contexts that an external library can be available. `lodash` here is available as `lodash` under AMD and CommonJS module systems but available as `_` in a global variable form.

### function

?> TODO - Not sure how to use it in and function form. Would be great to see a sample.

### regex

?> TODO - I think its overkill to list externals as regex.

For more information on how to use this configuration, please refer to the article on [how to author a library](/guides/author-libraries).
