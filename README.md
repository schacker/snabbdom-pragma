<h1 align="center"> Snabbdom-pragma </h1>

<div align="center">
  Well tested <code>NotReact.createElement</code> pragma although for Snabbdom !
</div>

<div align="center">
  <a href="https://www.npmjs.com/package/snabbdom-pragma">
    <img src="https://img.shields.io/npm/v/snabbdom-pragma.svg?style=flat-square" alt="npm version"/>
  </a>
  <a href="https://github.com/Swizz/snabbdom-pragma/blob/master/LICENSE.md">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square" alt="mit license"/>
  </a>
</div>

<br/>

Snabbdom-pragma is the favorite way to use the [Facebook JSX](https://facebook.github.io/jsx/) syntax with the virtual DOM library [Snabbdom](https://github.com/snabbdom/snabbdom). Based on many principles, Snabbdom-pragma, aim to handle the same API as [React.createElement](https://facebook.github.io/react/docs/react-api.html#createelement) to take all benefits from the most used transpilers proven by the wider React community.

Snabbdom-pragma draws its strength thanks to the [Snabbdom](https://github.com/snabbdom/snabbdom), [Facebook JSX](https://facebook.github.io/jsx/), and [React.createElement](https://facebook.github.io/react/docs/react-api.html#createelement) specs with some grounded tests.

## Table of Contents

<details>
<summary>Table of Contents</summary>

  - [Overview](#snabbdom-pragma)
  - [Getting started](#getting-started)
  - [Usage](#usage)
    * [Bublé](#bublé)
    * [Babel](#babel)
    * [Traceur](#traceur)
  - [JSX Features](#jsx-features)
    * [Elements](#elements)
    * [Attributes](#attributes)
    * [SVG](#svg)
  - [Snabbdom Features](#snabbdom-features)
    * [Modules object](#modules-object)
    * [Modules attribute](#modules-object)
  - ['NotReact' Features](#notreact-features)
    * [Components](#components)
  - [Misc](#misc)
</details>

## Getting started

- **1.** To use Snabbdom-pragma you need to download it thanks to your favorite JavaScript Package Manager.
  ```sh
  yarn add snabbdom-pragma
  ```

  ```sh
  npm install --save snabbdom-pragma
  ```

- **2.** The pragma option tells to your transpiler to use `Snabbdom.createElement` function instead of the default `React.createElement`.
  ```js
  buble.transform(input, {
    jsx: 'Snabbdom.createElement'
  })
  ```

- **3.** You will need to import the `Snabbdom.createElement` function into your code.
  ```js
  import Snabbdom from 'snabbdom-pragma'
  ```

- **4.** Your JSX source code will now be transpiled to use Snabbdom.
  ```js
  const vnode = <div>Hello World</div>

  patch(document.body, vnode)
  ```

## Usage

### Bublé
Snabbdom-pragma works fine and is fully tested for [Bublé](https://buble.surge.sh/guide/).
```js
buble.transform(input, {
  jsx: 'Snabbdom.createElement'
})
```

### Babel
Snabbdom-pragma works fine and is fully tested for [Babel](https://babeljs.io) with the
[transform-react-jsx](https://babeljs.io/docs/plugins/transform-react-jsx/) plugin enabled.
```js
babel.transform(input, {
  plugins: ['transform-react-jsx', {
    pragma: 'Snabbdom.createElement'
  }]
})
```

### Traceur
Snabbdom-pragma works fine and is fully tested for [Traceur](https://github.com/google/traceur-compiler).
```js
traceur.compile(input, {
  jsx: 'Snabbdom.createElement'
})
```

## JSX Features
Thanks to your transpiler, JSX tags will be transpiled into `NotReact.createElement` function following the `React.creatElement` specifications.

### Elements
As `Snabbdom.createElement` is a straightforward mapping to `Snabbdom/h`, HTML elements will work out of the box.
```js
/* written */
const vnode = <div>Hello World</div>

/* Once Transpiled */
const vnode = Snabbdom.createElement('div', null, 'Hello World')

/* Once Executed */
const vnode = h('div', {}, 'Hello World')
```

### Attributes
By default, attributes will be entrust to the `props` module. (see [Modules Features](#modules-object))
```js
/* written */
const vnode = <input type="text"/>

/* Once Transpiled */
const vnode = Snabbdom.createElement('input', { type: 'text' })

/* Once Executed */
const vnode = h('input', { props: { type: 'text' } }, [])
```

### SVG
SVG tags work without any configuration, but attributes will only work with the `attrs` module.
```js
/* written */
const vnode = <circle cx="43.5" cy="23" r="5"/>

/* Once Transpiled */
const vnode = Snabbdom.createElement('circle', { cx: 43.5, cy: 23, r: 5 })

/* Once Executed */
const vnode = h('circle', { attrs: { cx: 43.5, cy: 23, r: 5 } }, [])
```

## Snabbdom Features
In Snabbdom, functionalities is delegated to separate modules. Like `hook` (lifecycle), `on` (events), `style`, `props`, etc...
Snabbdom-pragma give you two ways to use these modules.

### Modules object
You can deal with modules properties with an object.
```js
/* written */
const vnode = <div style={{ color: 'red', fontWeight: 'bold' }}></div>

/* Once Transpiled */
const vnode = Snabbdom.createElement('div', { style: { color: 'red', fontWeight: 'bold' } })

/* Once Executed */
const vnode = h('div', { style: { color: 'red', fontWeight: 'bold' } }, [])
```

### Modules attribute
Or by using the `MODULE-PROPERTY` attribute.
```js
/* written */
const vnode = <button on-click={ callback }/>

/* Once Transpiled */
const vnode = Snabbdom.createElement('button', { 'on-click': callback })

/* Once Executed */
const vnode = h('button', { on: { click: callback } }, [])
```

## 'NotReact' Features
In React you can create components and use them inside other components, using the `React.createClass` function.

### Components
Snabbdom-pragma use simple functions as component of type `(attributes, children) => vnode`.
```js
/* written */
const Component = (name) =>
  <div>Hello { name }</div>

const vnode = <Component name="world"/>

/* Once Transpiled */
const Component = (name) =>
  Snabbdom.createElement('div', null, 'Hello ', name)

const vnode = Snabbdom.createElement(Component, { name: 'world' })

/* Once Executed */
const Component = (name) =>
  h('div', {}, ['Hello ', name])

const vnode = Component({ name: 'toto' }, [])
```
As in React, components function need to start with a capital letter, while regular HTML tags start with lower case letters. This is the common way to tell to your transpiler to give the function to the `Snabbdom.createElement` instead of a string.

## Misc

- Snabbdom-pragma follows the [Compatible Versioning: major.minor only](https://github.com/staltz/comver) convention.
- [Release notes](https://github.com/Swizz/snabbdom-pragma/releases) are [Keep a Changelog](http://keepachangelog.com/en/0.3.0/) compliants.
- SVG capable thanks to @jvanbruegge PR#4
- Documentation styling have been stolen to the [FlyJS](https://github.com/flyjs/fly) project
- Some part are shameless copy of the [Snabbdom-jsx](https://github.com/yelouafi/snabbdom-jsx) documentation
