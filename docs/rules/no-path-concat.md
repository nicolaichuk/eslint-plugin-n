# n/no-path-concat
> disallow string concatenation with `__dirname` and `__filename`

In Node.js, the `__dirname` and `__filename` global variables contain the directory path and the file path of the currently executing script file, respectively. Sometimes, developers try to use these variables to create paths to other files, such as:

```js
var fullPath = __dirname + "/foo.js";
```

However, there are a few problems with this. First, you can't be sure what type of system the script is running on. Node.js can be run on any computer, including Windows, which uses a different path separator. It's very easy, therefore, to create an invalid path using string concatenation and assuming Unix-style separators. There's also the possibility of having double separators, or otherwise ending up with an invalid path.

In order to avoid any confusion as to how to create the correct path, Node.js provides the `path` module. This module uses system-specific information to always return the correct value. So you can rewrite the previous example as:

```js
var fullPath = path.join(__dirname, "foo.js");
```

This example doesn't need to include separators as `path.join()` will do it in the most appropriate manner. Alternately, you can use `path.resolve()` to retrieve the fully-qualified path:

```js
var fullPath = path.resolve(__dirname, "foo.js");
```

Both `path.join()` and `path.resolve()` are suitable replacements for string concatenation wherever file or directory paths are being created.

## 📖 Rule Details

This rule aims to prevent string concatenation of directory paths in Node.js

Examples of **incorrect** code for this rule:

```js
/*eslint n/no-path-concat: "error"*/

const fullPath1 = __dirname + "/foo.js";
const fullPath2 = __filename + "/foo.js";
const fullPath3 = `${__dirname}/foo.js`;
const fullPath4 = `${__filename}/foo.js`;
```

Examples of **correct** code for this rule:

```js
/*eslint n/no-path-concat: "error"*/

const fullPath1 = path.join(__dirname, "foo.js");
const fullPath2 = path.join(__filename, "foo.js");
const fullPath3 = __dirname + ".js";
const fullPath4 = __filename + ".map";
const fullPath5 = `${__dirname}_foo.js`;
const fullPath6 = `${__filename}.test.js`;
```

## 🔎 Implementation

- [Rule source](../../lib/rules/no-path-concat.js)
- [Test source](../../tests/lib/rules/no-path-concat.js)
