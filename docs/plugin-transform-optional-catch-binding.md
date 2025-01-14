---
id: babel-plugin-transform-optional-catch-binding
title: "@babel/plugin-transform-optional-catch-binding"
sidebar_label: optional-catch-binding
---

> **NOTE**: This plugin is included in `@babel/preset-env`, in [ES2019](https://github.com/tc39/proposals/blob/master/finished-proposals.md)

## Examples

```js title="JavaScript"
try {
  throw 0;
} catch {
  doSomethingWhichDoesNotCareAboutTheValueThrown();
}
```

```js title="JavaScript"
try {
  throw 0;
} catch {
  doSomethingWhichDoesNotCareAboutTheValueThrown();
} finally {
  doSomeCleanup();
}
```

## Installation

```shell npm2yarn
npm install --save-dev @babel/plugin-transform-optional-catch-binding
```

## Usage

### With a configuration file (Recommended)

```json title="babel.config.json"
{
  "plugins": ["@babel/plugin-transform-optional-catch-binding"]
}
```

### Via CLI

```sh title="Shell"
babel --plugins @babel/plugin-transform-optional-catch-binding script.js
```

### Via Node API

```js title="JavaScript"
require("@babel/core").transformSync("code", {
  plugins: ["@babel/plugin-transform-optional-catch-binding"],
});
```

## References

- [Proposal: Optional Catch Binding for ECMAScript](https://github.com/babel/proposals/issues/7)
