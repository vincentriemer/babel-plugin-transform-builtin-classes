# Babel Builtin Classes Fix

Inspired by [Logan Smyth transformer](https://github.com/loganfsmyth/babel-plugin-transform-builtin-extend),
but actually very different.

This transformer aim is to fix once forever problems with native extends and Custom Elements.

```js
// finally fixed
class List extends Array {}
console.assert(new List instanceof Array);

// finally fixed too
class MyElement extends HTMLElement {
  connectedCallback() {
    this.textContent = 'hello';
  }
}

customElements.define('my-element', MyElement);

const node = new MyElement;
console.assert(node.constructor === MyElement);
console.assert(node instanceof MyElement);
console.assert(node instanceof HTMLElement);

document.body.appendChild(node);
```

## Usage

In your Babel 6 configuration, for example in a `.babelrc` you might have the following,
which would enable the plugin and configure it to look for any class extending `Error` or `Array` globals.

```js
{
  "plugins": [
    ["babel-plugin-transform-builtin-classes", {
      "globals": ["Error", "Array"]
    }]
  ]
}
```

**However**, you can find all known Chrome browser classes already listed in the file `lib/.babelrc` too.


### Compatibility
This transformer works on IE11 and every other browser with `Object.setPrototypeOf` or `__proto__` as fallback.

There is **NO IE <= 10 support**. If you need IE <= 10 don't use this plugin and/or don't extend natives (recommended).