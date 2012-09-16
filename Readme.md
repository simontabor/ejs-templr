# EJS Templr

Built for [Templr](https://github.com/simontabor/templr)

EJS Templr's renderFile method works with a JSON object right at the top of the file, split from the main body using `______`.

It can also use layouts from a `_layouts` folder in the root directory. This will be changed so it can be configured at some point. It can also read a JSON config for the layouts file. The content to be put in the layout is included by simply using `<%- content %>`.

It can also use includes from a `_includes` folder in the root.

## Installation
```bash
$ npm install ejs-templr
```

## Features

  * Complies with the [Express](http://expressjs.com) view system
  * Static caching of intermediate JavaScript
  * Unbuffered code for conditionals etc `<% code %>`
  * Escapes html by default with `<%= code %>`
  * Unescaped buffering with `<%- code %>`
  * Supports tag customization
  * Filter support for designer-friendly templates
  * Includes
  * Client-side support
  * Newline slurping with `<% code -%>` or `<% -%>` or `<%= code -%>` or `<%- code -%>`

## Example
```html
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>
```

## Usage

```javascript
ejs.renderFile(filepath, options,function(error, html) {
	// Callback Function
});
```

## Options

  - `cache`           Compiled functions are cached, requires `filename`
  - `filename`        Used by `cache` to key caches
  - `scope`           Function execution context
  - `debug`           Output generated function body
  - `compileDebug`    When `false` no debug instrumentation is compiled
  - `client`          Returns standalone compiled function
  - `open`            Open tag, defaulting to "<%"
  - `close`           Closing tag, defaulting to "%>"
  - *                 All others are template-local variables

##Layouts

If you use the renderFile on an ejs file, it will look for configuration at the top.

Configuration can be specified at follows…

```json
{
 "title": "Example!",
 "subtitle": "Page Subtitle",
 "layout": "main.ejs"
}
```
\______
```html 
<div>
Actual page content
</div>
```

If main.ejs was a file that looked like…
```json
{
	"nav": ["Page 1","Page 2"]
}
```
\______
```html
<h1><%= page.title %></h1>
<h2><%= page.subtitle %></h2>
<ul>
<% for (var i = 0;i < page.nav.length; i++) { %>
	<li><%= page.nav[i] %></li>
<% } %>
</ul>

<%- content %>

<div>Copyright 2012</div>
```

You'd be returned a string with the following html…
```html
<h1>Example!</h1>
<h2>Page Subtitle</h2>
<ul>
	<li>Page 1</li>
	<li>Page 2</li>
</ul>

<div>
Actual Page Content
</div>

<div>Copyright 2012</div>
```


## Includes

 Includes are relative to the `_includes` folder in the root directory. For example `<% include main.ejs %>` will look for the file `./_includes/main.ejs`.
 
 
## Custom delimiters

Custom delimiters can also be applied globally:

```javascript
var ejs = require('ejs-templr');
ejs.open = '{{';
ejs.close = '}}';
```

Which would make the following a valid template:
```html
<h1>{{= title }}</h1>
```

## Filters

EJS conditionally supports the concept of "filters". A "filter chain"
is a designer friendly api for manipulating data, without writing JavaScript.

Filters can be applied by supplying the _:_ modifier, so for example if we wish to take the array `[{ name: 'tj' }, { name: 'mape' },  { name: 'guillermo' }]` and output a list of names we can do this simply with filters:

Template:

    <p><%=: users | map:'name' | join %></p>

Output:

    <p>Tj, Mape, Guillermo</p>

Render call:

    ejs.render(str, {
        users: [
          { name: 'tj' },
          { name: 'mape' },
          { name: 'guillermo' }
        ]
    });

Or perhaps capitalize the first user's name for display:

    <p><%=: users | first | capitalize %></p>

## Filter list

Currently these filters are available:

  - first
  - last
  - capitalize
  - downcase
  - upcase
  - sort
  - sort_by:'prop'
  - size
  - length
  - plus:n
  - minus:n
  - times:n
  - divided_by:n
  - join:'val'
  - truncate:n
  - truncate_words:n
  - replace:pattern,substitution
  - prepend:val
  - append:val
  - map:'prop'
  - reverse
  - get:'prop'

## Adding filters

 To add a filter simply add a method to the `.filters` object:
 
```js
ejs.filters.last = function(obj) {
  return obj[obj.length - 1];
};
```

## License 

(The MIT License)

Copyright (c) 2009-2010 TJ Holowaychuk &lt;tj@vision-media.ca&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
