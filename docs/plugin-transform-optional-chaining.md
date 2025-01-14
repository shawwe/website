---
id: babel-plugin-transform-optional-chaining
title: "@babel/plugin-transform-optional-chaining"
sidebar_label: optional-chaining
---

> **NOTE**: This plugin is included in `@babel/preset-env`, in [ES2020](https://github.com/tc39/proposals/blob/master/finished-proposals.md)

## Example

### Accessing deeply nested properties

```js title="JavaScript"
const obj = {
  foo: {
    bar: {
      baz: 42,
    },
  },
};

const baz = obj?.foo?.bar?.baz; // 42

const safe = obj?.qux?.baz; // undefined

// Optional chaining and normal chaining can be intermixed
obj?.foo.bar?.baz; // Only access `foo` if `obj` exists, and `baz` if
// `bar` exists

// Example usage with bracket notation:
obj?.["foo"]?.bar?.baz; // 42
```

### Calling deeply nested functions

```js title="JavaScript"
const obj = {
  foo: {
    bar: {
      baz() {
        return 42;
      },
    },
  },
};

const baz = obj?.foo?.bar?.baz(); // 42

const safe = obj?.qux?.baz(); // undefined
const safe2 = obj?.foo.bar.qux?.(); // undefined

const willThrow = obj?.foo.bar.qux(); // Error: not a function

// Top function can be called directly, too.
function test() {
  return 42;
}
test?.(); // 42

exists?.(); // undefined
```

### Constructing deeply nested classes

```js title="JavaScript"
const obj = {
  foo: {
    bar: {
      baz: class {
      },
    },
  },
};

const baz = new obj?.foo?.bar?.baz(); // baz instance

const safe = new obj?.qux?.baz(); // undefined
const safe2 = new obj?.foo.bar.qux?.(); // undefined

const willThrow = new obj?.foo.bar.qux(); // Error: not a constructor

// Top classes can be called directly, too.
class Test {
}
new Test?.(); // test instance

new exists?.(); // undefined
```

### Deleting deeply nested properties

Added in: `v7.8.0`

```js title="JavaScript"
const obj = {
  foo: {
    bar: {},
  },
};

const ret = delete obj?.foo?.bar?.baz; // true
```

## Installation

```shell npm2yarn
npm install --save-dev @babel/plugin-transform-optional-chaining
```

## Usage

### With a configuration file (Recommended)

```json title="babel.config.json"
{
  "plugins": ["@babel/plugin-transform-optional-chaining"]
}
```

### Via CLI

```sh title="Shell"
babel --plugins @babel/plugin-transform-optional-chaining script.js
```

### Via Node API

```js title="JavaScript"
require("@babel/core").transformSync("code", {
  plugins: ["@babel/plugin-transform-optional-chaining"],
});
```

## Options

### `loose`

`boolean`, defaults to `false`.

When `true`, this transform will pretend `document.all` does not exist,
and perform loose equality checks with `null` instead of strict equality checks
against both `null` and `undefined`.

> ⚠️ Consider migrating to the top level [`noDocumentAll`](assumptions.md#nodocumentall) assumption.

```json title="babel.config.json"
{
  "assumptions": {
    "noDocumentAll": true
  }
}
```

#### Example

In

```js title="JavaScript"
foo?.bar;
```

Out (`noDocumentAll === true`)

```js title="JavaScript"
foo == null ? void 0 : foo.bar;
```

Out (`noDocumentAll === false`)

```js title="JavaScript"
foo === null || foo === void 0 ? void 0 : foo.bar;
```

> You can read more about configuring plugin options [here](https://babeljs.io/docs/en/plugins#plugin-options)

## References

- [Proposal: Optional Chaining](https://github.com/tc39/proposal-optional-chaining)
