# miniprogram tabBar, titleBar multilingual configuration

# **Introduction**

After configuring tabBar and titleBar for multiple languages, the mini program will display the language configuration based on the current language setting of Alipay.

The configuration for multi-language tabBar and titleBar is as follows:

1. Place the language configuration in the locale directory under the root directory.
2. Create a separate directory for each language, such as zh-Hans (Simplified Chinese) and zh-HK (Traditional Chinese - Hong Kong).
3. In each language directory, add the app.json configuration to set the titleBar and tabBar for that language.

**Note:**

- Currently, only four languages are supported: zh-Hans (Simplified Chinese), zh-Hant (Traditional Chinese - Taiwan), zh-HK (Traditional Chinese - Hong Kong), and en (English).
- Use **Real Machine Debugging** in the IDE to see the configuration effect.

Example file directory structure:

[http://mdn.alipayobjects.com/afts/img/A*z9X-S4YOFfMAAAAAAAAAAAAAAa8wAA/original?bz=openpt_doc&t=M4xk7mU9kmNlh19urXD5KgAAAABkMK8AAAAA#align=left&display=inline&height=614&margin=%5Bobject%20Object%5D&originHeight=614&originWidth=696&status=done&style=none&width=696](http://mdn.alipayobjects.com/afts/img/A*z9X-S4YOFfMAAAAAAAAAAAAAAa8wAA/original?bz=openpt_doc&t=M4xk7mU9kmNlh19urXD5KgAAAABkMK8AAAAA#align=left&display=inline&height=614&margin=%5Bobject%20Object%5D&originHeight=614&originWidth=696&status=done&style=none&width=696)

# **tabBar Real Machine Effect**

[http://mdn.alipayobjects.com/afts/img/A*EN7qS5fNwlUAAAAAAAAAAAAAAa8wAA/original?bz=openpt_doc&t=EXyCvJ5SiOGibXswiQoqEAAAAABkMK8AAAAA#align=left&display=inline&height=854&margin=%5Bobject%20Object%5D&originHeight=854&originWidth=855&status=done&style=none&width=855](http://mdn.alipayobjects.com/afts/img/A*EN7qS5fNwlUAAAAAAAAAAAAAAa8wAA/original?bz=openpt_doc&t=EXyCvJ5SiOGibXswiQoqEAAAAABkMK8AAAAA#align=left&display=inline&height=854&margin=%5Bobject%20Object%5D&originHeight=854&originWidth=855&status=done&style=none&width=855)

# **tabBar Sample Code**

Here is an example of the app.json configuration in the locale directory. Only the **`name`** attribute needs to be specified in the **`items`** array.

```jsx
// locale/en/app.json
{
  "pages": {
    "pages/index/index": {
      "defaultTitle": "index"
    },
    "pages/second/second": {
      "defaultTitle": "second"
    },
    "pages/third/third": {
      "defaultTitle": "third"
    }
  },
  "window": {
    "defaultTitle": "Demo"
  },
  "tabBar": {
    "textColor": "#dddddd",
    "selectedColor": "#49a9ee",
    "backgroundColor": "#ffffff",
    "items": [
      {
        "name": "index"
      },
      {
        "name": "second"
      },
      {
        "name": "third"
      }
    ]
  }
}
```

Here is an example of the app.json configuration in the root directory. The **`items`** array does not specify the **`name`** attribute.

```jsx
// app.json
{
  "pages": ["pages/index/index", "pages/second/second", "pages/third/third"],
  "window": {
    "defaultTitle": "Demo"
  },
  "tabBar": {
    "textColor": "#dddddd",
    "selectedColor": "#49a9ee",
    "backgroundColor": "#ffffff",
    "items": [
      {
        "pagePath": "pages/index/index"
      },
      {
        "pagePath": "pages/second/second"
      },
      {
        "pagePath": "pages/third/third"
      }
    ]
  }
}
```

# **Properties**

| Property | Type | Required | Description |
| --- | --- | --- | --- |
| window | Object | No | Set the default title. |
| tabBar | Object | No | Set the names of tabBarItem for languages. |
| pages | Object | No | Set the title for each page in languages. |