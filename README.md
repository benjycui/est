# ECMAScript Template

ECMAScript Template is a solution to write template in JavaScript. It's based on [JsonML](http://www.jsonml.org/) and [ES2015](http://www.ecma-international.org/ecma-262/6.0/).

## Why

* Easy to learn. It is just JavaScript and some conventions.
* High-extensible. JsonML is something like [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree), and we can operate it easily.
* Mature ecosystem.
  * Use the latest JavaScript features with [Babel](https://babeljs.io/).
  * Lint your code style with [ESLint](http://eslint.org/).
  * And so on...
* Any application that can be written in JavaScript, will eventually be written in JavaScript.

## Usage

You can run the following code in [CodePen](http://codepen.io/benjycui/pen/bpoYyy?editors=0012).

```jsx
const layout = (content) => (data) => {
  return [
    'html',
    [
      'head',
      ['title', 'ECMAScript Template'],
    ],
    [
      'body',
      content(data),
    ],
  ];
}

function home({ name }) {
  return [
    'h1',
    `Hello, ${name}!`,
  ];
}
home = layout(home);

const html = JsonML.toHTMLText(home({ name: 'Benjy' }));
console.log(html);

// Output
// <html>
//   <head>
//     <title>ECMAScript Template</title>
//   </head>
//   <body>
//     <h1>Hello, Benjy!</h1>
//   </body>
// </html>
```

**Note:** To simplify examples, we just use [jsonml-html](https://github.com/mckamey/jsonml) to convert JsonML to HTML. However, we can use any library to convert JsonML to any format.

## Documentation

### Tags

An HTML element will be represented as an array, and the first item is tag name.

```jsx
['br']
// =>
<br />
```

### Attributes

If the second item in array is an object, it will be treated as attributes.

```jsx
['a', { class: 'link', href: 'google.com' }, 'Google']
// =>
<a class='link' href='google.com'>Google</a>

// Attribute is not required
['h1', 'Hello world!']
// =>
<h1>Hello world!</h1>
```

### Children

The rest items in array will be treated as children.

```jsx
['ul',
  ['li', 'First.'],
  ['li', 'Second.'],
  ['li', 'Third.'],
]
// =>
<ul>
  <li>First.</li>
  <li>Second.</li>
  <li>Third.</li>
</ul>
```

### Conditionals

```jsx
const hour = 9;

['p',
  hour < 12 ? 'Good morning! ' : 'Good afternoon! ',
  'My friends',
]
// =>
<p>Good morning! My friends</p>
```

### String Interpolation

```jsx
const child = 'boy';

['p', `A ${child} will grow up to be a ${child === 'boy' ? 'man' : 'woman'}.`]
// =>
<p>A boy will grow up to be a man.</p>
```

### Loop

```jsx
['ul',
  ...['First.', 'Second.', 'Third.'].map((item) => ['li', item]),
]
// =>
<ul>
  <li>First.</li>
  <li>Second.</li>
  <li>Third.</li>
</ul>
```

### Template

A template is a function which receives data and return JsonML.

```jsx
function greeting({ name }) {
  return [
    'h1',
    `Hello, ${name}!`,
  ];
}

greeting({ name: 'Benjy' }); // => ['h1', 'Hello, Benjy']
```

### Includes

Includes allow you to insert the contents into template.

```jsx
function todoList({ todos}) {
  return [
    'ul',
    ...todos.map((todo) => ['li', todo]),
  ];
}

function todoApp(data) {
  return [
    'section',
    todoList(data),
  ];
}

todoApp({
  todos: [
    'eat',
    'sleep',
    'beat Doudou',
  ],
});
```

### Layout

A layout is a [HOF](https://en.wikipedia.org/wiki/Higher-order_function) which receives blocks and return a template.

```jsx
// layout.js
export default const layout = (content) => (data) => {
  return [
    'html',
    [
      'head',
      ['title', 'ECMAScript Template'],
    ],
    [
      'body',
      content(data),
    ],
  ];
}
```

### Extends

To extends a layout, just pass blocks(templates) to layout.

```jsx
// home.js
import layout from './layout';

function home(data) {
  return [
    'h1',
    `Hello, ${data.name}!`,
  ];
}

// Extends layout.
home = layout(home);

home({ name: 'Benjy' });
```

## Liscense

MIT
