# Framework Overview

July 6, 2023 

# File structure

A mini-program is divided into two layers: `app` and `page`. The `app` layer is used to describe the entire application, while the `page` layer is used to describe individual pages. Additionally, there are optional configuration descriptions for the entire `project`.

The `app`layer consists of three files that must be placed in the root directory of the project.

| File | Required | Purpose |
| --- | --- | --- |
| app.js | Yes | Mini-program logic |
| app.json | Yes | Mini-program global settings |
| app.acss | No | Mini-program global style sheet |

A `page`consists of four files, which are:

| File Type | Required | Purpose |
| --- | --- | --- |
| js | Yes | Page logic |
| axml | Yes | Page structure |
| json | Yes | Page configuration |
| acss | No | Page style sheet |

# Logic structure

The mini-program's logic structure consists of a responsive data binding system, which includes the view layer and the logic layer. These two layers are always in sync, so whenever data is modified in the logic layer, the view layer will be updated accordingly. Here's an example code:

```html
<!-- View Layer -->
<view> Hello {{name}}! </view>
<button onTap="changeName">Click me!</button>
```

```jsx
// Logic Layer
var initialData = {
	name: 'taobao',
};

// Register a page
Page({
	data: initialData,
	changeName(e) {
	// Change the data
	this.setData({
		name: 'alipay',
	});
	},
});
```

In the code above, the framework automatically binds the **`name`** data in the logic layer with the **`name`** in the view layer. So when the page is opened, it will display "Hello taobao!". When the user clicks the button, the view layer sends the **`changeName`** event to the logic layer, where the corresponding event handler is found. The logic layer then executes the **`setData`** operation, changing the **`name`** from "taobao" to "alipay". Since the data is bound to the view layer, the view layer will automatically update to display "Hello alipay!".

Note: Since the framework does not run in a browser, some JavaScript capabilities commonly used in web development, such as **`document`** and **`window`** objects, are not available. In the logic layer JavaScript, you can organize code using ES2015 module syntax:

```jsx
import util from './util'; // Import using relative path
import absolute from '/absolute'; // Import using project root path
```

# Reserved words

In mini-programs, certain built-in browser object names (such as **`window`** and **`document`**) are reserved keywords to accommodate future needs. The reserved keywords include: **`globalThis`**, **`global`**, **`AlipayJSBridge`**, **`fetch`**, **`self`**, **`window`**, **`document`**, **`location`**, and **`XMLHttpRequest`**. Please refrain from using these reserved keywords as module names to avoid issues with accessing the modules correctly. For example:

```jsx
import { window } from './myWindow';
console.log(window); // undefined
```

In the above code, using a reserved keyword as a module name causes the imported module to become **`undefined`**. The correct approach is to avoid using reserved keywords as module names or use the **`as`** keyword to rename the module during import. For example:

```jsx
import { window as myWindow } from './myWindow';
console.log(myWindow);
```

By doing so, you can import the module with a different name and avoid conflicts with reserved keywords.

# Third-Party NPM Modules

Mini-programs support importing third-party modules. To install a module, you need to execute the following command in the root directory of your mini-program:

```bash
npm install query-string --save
```

After installing the module, you can directly use it in the logic layer by importing it:

```jsx
import queryString from 'query-string'; // Importing the third-party npm module
```

For more detailed information about NPM, you can refer to the documentation on NPM package management.

Note: Since the code in the **`node_modules`** directory for third-party modules does not go through the transpiler, it is recommended to transform the code in the **`node_modules`** directory into ES5 format for compatibility across different platforms before importing. The module format using ES2015's **`import`**/**`export`** is recommended. Additionally, browser-related web capabilities cannot be used in the mini-program environment.