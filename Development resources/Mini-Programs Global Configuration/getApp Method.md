# getApp Method

# ****getApp Method****

The mini program provides a global **`getApp()`** method to obtain the current mini program instance, which is usually used to get the top-level application from a child page.

```jsx
// app.js
App({
  globalData: 1
});
```

```jsx
// page.js
var app = getApp();
console.log(app.globalData); // Get globalData
```

**Note:**

- The **`getApp()`** method cannot be called inside the **`App()`** function. Instead, use **`this`** to access the current mini program instance.
- After obtaining the instance with **`getApp()`**, please do not call the lifecycle callback functions.
- Please differentiate between global variables and page-local variables. For example:

```jsx
// app.js
App({
  // Define the global variable globalData, which is valid throughout the App
  globalData: 1
});
```

```jsx
// a.js
// Define the page-local variable localValue, which is only valid in a.js
var localValue = 'a';
// Get the app instance
var app = getApp();
// Access the global data and modify it
app.globalData++;
```

```jsx
// b.js
// Define the page-local variable localValue, which is only valid in b.js
var localValue = 'b';
// If a.js runs first, globalData will be 2
console.log(getApp().globalData);
```

In the above example, both **`a.js`** and **`b.js`** declare a variable named **`localValue`**, but they don't affect each other because the locally declared variables and functions in each file are only valid within that file.

# **Related Documentation**

- **[Mini Program Page Introduction](https://opendocs.alipay.com/mini/framework/page)**
- **[Introduction to Mini Program Global Configuration](https://opendocs.alipay.com/mini/framework/app)**