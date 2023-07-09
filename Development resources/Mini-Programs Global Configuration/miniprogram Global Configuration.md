# miniprogram Global Configuration

**`App()`** represents the top-level application that manages all pages, global data, and provides lifecycle callbacks, among other functionalities. **`App()`** is also a constructor that generates an instance of the App, and a mini-program consists of one App instance.

Generally, the top-level of a mini-program consists of three files:

- **`app.json`**: Application configuration.
- **`app.js`**: Application logic.
- **`app.acss`**: Application style (optional).

# **Simple Example**

A simple example of **`app.json`** is as follows:

```json
{
  "pages": ["pages/index/index", "pages/logs/logs"],
  "window": {
    "defaultTitle": "Demo"
  }
}
```

This code specifies that the mini-program contains two pages (**`index`** and **`logs`**) and sets the default title of the application window to **Demo**. A simple example of **`app.js`** is as follows:

```jsx
App({
  onLaunch(options) {
    // First launch
  },
  onShow(options) {
    // Mini-program is launched or reopened from the background
  },
  onHide() {
    // Mini-program goes into the background from the foreground
  },
  onError(msg) {
    // Script error or API call error in the mini-program
    console.log(msg);
  },
  globalData: {
    // Global data
    name: 'alipay',
  },
});
```

# **Related Documentation**

- **[app.json Global Configuration](https://opendocs.alipay.com/mini/framework/app-json)**
- **[ACSS Syntax Reference](https://opendocs.alipay.com/mini/framework/acss)**
- **[app.js Register Mini-program](https://opendocs.alipay.com/mini/framework/app-detail)**