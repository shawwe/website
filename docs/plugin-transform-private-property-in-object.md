---
id: babel-plugin-transform-private-property-in-object
title: "@babel/plugin-transform-private-property-in-object"
sidebar_label: private-property-in-object
---

> **NOTE**: This plugin is included in `@babel/preset-env`, in [ES2022](https://github.com/tc39/proposals/blob/master/finished-proposals.md)

## Example

**In**

```js title="JavaScript"
class Foo {
  #bar = "bar";

  test(obj) {
    return #bar in obj;
  }
}
```

**Out**

```js title="JavaScript"
class Foo {
  constructor() {
    _bar.set(this, {
      writable: true,
      value: "bar",
    });
  }

  test() {
    return _bar.has(this);
  }
}

var _bar = new WeakMap();
```

## Installation

```shell npm2yarn
npm install --save-dev @babel/plugin-transform-private-property-in-object
```

## Usage

### With a configuration file (Recommended)

```json title="babel.config.json"
{
  "plugins": ["@babel/plugin-transform-private-property-in-object"]
}
```

### Via CLI

```sh title="Shell"
babel --plugins @babel/plugin-transform-private-property-in-object
```

### Via Node API

```js title="JavaScript"
require("@babel/core").transformSync("code", {
  plugins: ["@babel/plugin-transform-private-property-in-object"],
});
```

## Options

### `loose`

`boolean`, defaults to `false`.

> Note: The `loose` mode configuration setting _must_ be the same as [`@babel/transform-class-properties`](plugin-transform-class-properties.md).

When true, private property `in` expressions will check own properties (as opposed to inherited ones) on the object, instead of checking for presence inside a `WeakSet`. This results in improved
performance and debugging (normal property access vs `.get()`) at the expense
of potentially leaking "privates" via things like `Object.getOwnPropertyNames`.

> ⚠️ Consider migrating to the top level [`privateFieldsAsProperties`](assumptions.md#privatefieldsasproperties) assumption.

```json title="babel.config.json"
{
  "assumptions": {
    "privateFieldsAsProperties": true,
    "setPublicClassFields": true
  }
}
```

Note that both `privateFieldsAsProperties` and `setPublicClassFields` must be set to `true`.

#### Example

**In**

```js title="JavaScript"
class Foo {
  #bar = "bar";

  test(obj) {
    return #bar in obj;
  }
}
```

**Out**

```js title="JavaScript"
class Foo {
  constructor() {
    Object.defineProperty(this, _bar, {
      writable: true,
      value: "bar",
    });
  }

  test() {
    return Object.prototype.hasOwnProperty.call(this, _bar);
  }
}

var _bar = babelHelpers.classPrivateFieldLooseKey("bar");
```

> You can read more about configuring plugin options [here](https://babeljs.io/docs/en/plugins#plugin-options)

## References

- [Proposal: Ergonomic brand checks for Private Fields](https://github.com/tc39/proposal-private-fields-in-in)
