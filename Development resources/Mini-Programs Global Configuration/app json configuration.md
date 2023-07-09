# app.json configuration

**`app.json`** is used to configure the global settings for a mini-program, including the file paths of pages, window settings, multi-tabs, sub-packages, plugins, and more.

Here is a basic configuration example:

```json
{
  "pages": ["pages/index/index", "pages/logs/logs"],
  "window": {
    "defaultTitle": "Demo"
  }
}
```

The complete list of configuration properties is as follows:

| Property | Type | Required | Description |
| --- | --- | --- | --- |
| entryPagePath | String | No | The default launch page of the mini-program. |
| pages | Array | Yes | Sets the page paths. |
| window | Object | No | Sets the window style for the default page. |
| tabBar | Object | No | Sets the appearance of the bottom tab bar. |
| subPackages | Object[] | No | Describes the sub-package structure. |
| preloadRule | Object | No | Specifies the preloading rules for sub-packages. |
| plugins | Object | No | Configures static plugin rules. |
| useDynamicPlugins | Boolean | No | Configures dynamic plugin rules. |
| usingComponents | Array | No | Sets global custom component declarations. |
| lazyCodeLoading | String | No | Enables code on-demand execution. |
| permission | Object | No | Configures permissions related to mini-program APIs. |
| behavior | Object | No | Configures settings to modify the behavior of the mini-program. |
| workers | Array | No | Sets the list of Worker code files. |

# **entryPagePath**

Specifies the default launch path (home page) of the mini-program. If not specified, the first item in the **`pages`** list will be used as the default. It does not support path parameters.

**Note**: This feature is supported starting from the **[2.7.20](https://opendocs.alipay.com/mini/framework/lib-upgrade-v2)** basic library and **[IDE 3.1.2](https://opendocs.alipay.com/mini/ide/download)**. If you rely heavily on this feature, it is recommended to set the minimum basic library version to 2.7.20. Otherwise, in lower versions of the basic library, the rendering of the "Return to Home" icon may fail due to the inability to recognize the correct home page.

# **pages**

The **`pages`** property in **`app.json`** is an array used to specify the pages of the mini-program. When adding or deleting pages in the mini-program, the **`pages`** array needs to be modified accordingly.

Each item in the **`pages`** array represents the path information of a corresponding page in the mini-program, where the first item represents the home page of the mini-program.

The page paths should not include any file extensions as the framework will automatically load files with the same name but different extensions, such as **`.json`**, **`.js`**, **`.axml`**, and **`.acss`**. For example, if the development directory structure is as follows:

```markdown
.
├── pages
│   ├──index
│   │    ├── index.json
│   │    ├── index.js
│   │    ├── index.axml
│   │    └── index.acss
│   ├──logs
│   │    ├── logs.json
│   │    ├── logs.js
│   │    └── logs.axml
```

```json
{
  "pages": ["pages/index/index", "pages/logs/logs"]
}
```

# **usingComponents**

The custom components declared in the **`app.json`** file are considered as global custom components. They can be directly used in all pages and custom components of the mini-program without the need for additional declaration.

```json
{
  "usingComponents": {
    "com1": "/components/com1/index",
    "com2": "./components/com2/index"
  }
}
```

**Note**: This feature is supported starting from **[IDE 3.1.2](https://opendocs.alipay.com/mini/ide/download)** and above. The components declared using this feature will be available to all pages and components in the mini-program, which may affect performance and increase the size of the main package. It is recommended to enable **`app.lazyCodeLoading`** for optimization.

# **window**

The **`window`** property is used to set the status bar, navigation bar, title, window background color, and more for the mini-program. Here's an example:

```json
{
  "window": {
    "defaultTitle": "Alipay Interface Demo"
  }
}
```

| Attribute | Type | Required | Description | Minimum Version |
| --- | --- | --- | --- | --- |
| allowsBounceVertical | String | No | Whether dragging downwards is allowed. Default is YES. Supports YES / NO. | - |
| backgroundColor | HexColor | No | The background color of the page. Example: white "#FFFFFF". | - |
| backgroundImageColor | HexColor | No | The background color of the area revealed when pulling down to show the background image. Example: white "#FFFFFF". Only effective on Android, on iOS the background color of the page will use the value of backgroundColor | - |
| backgroundImageUrl | String | No | The URL of the background image displayed when pulling down. | - |
| defaultTitle | String | No | The default title of the page. | - |
| enableScrollBar | String | No | Only supported on Android. Whether to show the WebView scrollbar. Default is YES. Supports YES / NO. | - |
| gestureBack | String | No | Only supported on iOS. Whether to enable gesture-based back navigation. Default is YES. Supports YES / NO. | - |
| onReachBottomDistance | Number | No | The distance from the bottom of the page that triggers the onReachBottom event, in pixels. For more details, see https://opendocs.alipay.com/mini/framework/page-detail#%E9%A1%B5%E9%9D%A2%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86%E5%87%BD%E6%95%B0. | https://opendocs.alipay.com/mini/framework/compatibility. Currently, setting this in page.json on iOS has no effect and can only be set globally. |
| pullRefresh | Boolean | No | Whether to enable pull-down-to-refresh. Default is false. <br /> Note: <br /> 1. Pull-down refresh only takes effect when allowsBounceVertical is set to YES. <br /> 2. When pullRefresh is configured globally in the window, it applies to all pages. However, if an individual page has this parameter configured, the page's configuration takes precedence. | - |
| responsive | Boolean | No | Whether rpx units are responsive. Default is true. When set to false, 2 rpx will always equal 1 px, and will no longer adapt based on screen width. Note that when this is set to false, 750 rpx will no longer equal 100% width. | https://opendocs.alipay.com/mini/framework/compatibility |
| showTitleLoading | String | No | Whether to show the navigation bar loading when entering the page. Default is NO. Supports YES / NO. | - |
| transparentTitle | String | No | Transparency settings for the navigation bar. Default is none. Supports always always transparent / auto transparent when scrolling / none opaque. | - |
| titlePenetrate | String | No | Whether to allow click-through on the navigation bar. Default is NO, supports YES / NO. | - |
| titleImage | String | No | Navigation bar image address. | - |
| titleBarColor | HexColor | No | Background color of the navigation bar. Example: white is "#FFFFFF". | - |

# **tabBar**

If your mini program is a multi-tab application (the bottom bar of the client window can switch pages), you can use the **`tabBar`** configuration to specify the appearance of the tab bar and the corresponding page to display when a tab is switched. The **`tabBar`** is at the same level as **`pages`** and **`window`** configurations, and the properties are as follows:

| Attribute | Type | Required | Description |
| --- | --- | --- | --- |
| textColor | HexColor | No | Text color. |
| selectedColor | HexColor | No | Selected text color. |
| backgroundColor | HexColor | No | Background color. |
| items | Array | Yes | Configuration for each tab. |

Each item configuration:

| Attribute | Type | Required | Description |
| --- | --- | --- | --- |
| pagePath | String | Yes | Set the page path. |
| name | String | Yes | Name. |
| icon | String | No | Icon path for normal state. |
| activeIcon | String | No | Icon path for selected state. |

The recommended size for the icon is 81px * 81px. The system will stretch or scale the images that are not in the recommended size proportionally. Here's an example of **`app.json`** with a **`tabBar`**:

```json
{
  "pages": ["pages/index/index", "pages/logs/logs"],
  "window": {
    "defaultTitle": "Demo"
  },
  "tabBar": {
    "textColor": "#dddddd",
    "selectedColor": "#49a9ee",
    "backgroundColor": "#ffffff",
    "items": [
      {
        "pagePath": "pages/index/index",
        "name": "Home"
      },
      {
        "pagePath": "pages/logs/logs",
        "name": "Logs"
      }
    ]
  }
}
```

In the code, developers can use **[my.setTabBarItem](https://opendocs.alipay.com/mini/api/zu37bk)** to dynamically set the content of a specific **`item`** in the **`tabBar`**.

# **subPackages**

When using **[subpackage loading](https://opendocs.alipay.com/mini/framework/subpackages)**, declare the project's subpackage structure.

# **preloadRule**

Declare the rules for **[subpackage preloading](https://opendocs.alipay.com/mini/framework/subpackages#%E5%88%86%E5%8C%85%E9%A2%84%E4%B8%8B%E8%BD%BD)**.

# **plugins**

Starting from base library 1.22.4 and Alipay client 10.1.85, declare the **[static plugins](https://opendocs.alipay.com/mini/plugin/plugin-usage#%E9%9D%99%E6%80%81%E5%A3%B0%E6%98%8E)** that the mini program needs to use.

# **useDynamicPlugins**

Starting from base library 1.22.4 and Alipay client 10.1.85, declare the **[dynamic plugins](https://opendocs.alipay.com/mini/plugin/plugin-usage#%E5%8A%A8%E6%80%81%E5%8A%A0%E8%BD%BD)** that the mini program needs to use.

# **lazyCodeLoading**

During the startup process of a mini program application, besides the download phase, by default, all the code (including all pages and custom components that are not used in the current page) will be executed, which may have some impact on the startup time. Starting from base library 2.7.0, you can configure the **`lazyCodeLoading`** parameter to only execute the necessary page scripts and custom component scripts for the current page, while other scripts will not be executed.

```jsx
{
  "lazyCodeLoading": "requiredComponents"
}
```

**Note:** After enabling this configuration, the code that is not used in the current page will not be executed, which may have an impact on the logic that relies on the default script execution order.

# **workers**

When using **[Worker](https://opendocs.alipay.com/mini/api/createworker)** to handle multi-threaded tasks, set the list of Worker code files. For example:

```jsx
"workers": [
  "workers/index.js"
]
```

# **permission**

Permission-related settings for mini program interfaces. The field type is an object with the following structure:

| Attribute | Type | Required | Description |
| --- | --- | --- | --- |
| scope.album | PermissionObject | No | Declaration of permissions related to the album (access). Related APIs: https://opendocs.alipay.com/mini/api/media/image/my.chooseimage, https://opendocs.alipay.com/mini/api/media/video/my.choosevideo (sourceType includes album). |
| scope.writePhotosAlbum | PermissionObject | No | Declaration of permissions related to the album (saving). Related APIs: https://opendocs.alipay.com/mini/api/media/image/my.saveimage, https://opendocs.alipay.com/mini/api/media/image/my.saveImagetophotosalbum, https://opendocs.alipay.com/mini/api/media/video/my.savevideotophotosalbum. |
| scope.camera | PermissionObject | No | Declaration of permissions related to the camera. Related APIs: https://opendocs.alipay.com/mini/api/media/image/my.chooseimage, https://opendocs.alipay.com/mini/api/media/video/my.choosevideo (sourceType includes camera). |
| scope.record | PermissionObject | No | Declaration of permissions related to the microphone. Related APIs: https://opendocs.alipay.com/mini/api/getrecordermanager. |
| scope.userLocation | PermissionObject | No | Declaration of permissions related to location. Related APIs: https://opendocs.alipay.com/mini/api/mkxuqd. |

## **PermissionObject Structure**

| Attribute | Type | Required | Description |
| --- | --- | --- | --- |
| desc | String | Yes | The purpose of the interface displayed when the mini program requests permission. |

### **Example**

```json
{
  "permission": {
    "scope.album": {
      "desc": "Read photos for providing beauty services"
    },
    "scope.camera" : {
      "desc" : "Access your camera for scanning QR codes"
    },
    "scope.record" : {
      "desc" : "Access your microphone for song recognition"
    },
    "scope.userLocation": {
      "desc": "Your location information will be used to match your service city"
    },
    "scope.writePhotosAlbum" : {
      "desc" : "Used to save photos after applying beauty filters"
    }
  }
}
```

[https://gw.alipayobjects.com/mdn/rms_282813/afts/img/A*HTBCQrdaRvkAAAAAAAAAAAAAARQnAQ](https://gw.alipayobjects.com/mdn/rms_282813/afts/img/A*HTBCQrdaRvkAAAAAAAAAAAAAARQnAQ)

# **behavior**

Used to change certain behaviors of the mini program. The field type is an object with the following structure:

| Attribute | Type | Required | Description |
| --- | --- | --- | --- |
| shareAppMessage | String | No | Possible value: "appendQuery".<br />When using the default sharing feature of the mini program (i.e., not explicitly setting https://opendocs.alipay.com/mini/framework/page-detail#onShareAppMessage(options%3A%20Object)), setting this field will make the scheme generated by the client for sharing include the query parameters carried by the page that the user currently opens.<br />Supported starting from base library https://opendocs.alipay.com/mini/framework/lib-upgrade-v2, and requires building with https://opendocs.alipay.com/mini/ide/download or above. |
| decodeQuery | String | No | Possible value: "disable".<br />By default, the mini program encodes the keys/values when parsing global parameters and page parameters. When set to "disable", it will no longer encode the keys/values, and the parsing rules can be found in https://opendocs.alipay.com/mini/03durs. Supported starting from base library https://opendocs.alipay.com/mini/framework/lib-upgrade-v2, and requires building with https://opendocs.alipay.com/mini/ide/download or above. |

## **Example**

```
{
  "behavior": {
    "shareAppMessage": "appendQuery", // By using this configuration, you can choose whether the default sharing feature includes query parameters.
    "decodeQuery": "disable" // When set to "disable", the base library no longer encodes the keys/values of global/page parameters.
  }
}
```

# **Frequently Asked Questions**

### **Q: A page (list page) is set to allow pull-down refresh, and B page (detail page) is set to disable pull-down (`allowsBounceVertical: NO`). When I go from A page to B page and then click the upper left corner to return to A page, A page cannot be pulled down to refresh.**

A: To solve this problem, set **`allowsBounceVertical: YES`** when setting pull-down refresh for A page.

**Note**: When setting pull-down refresh, make sure to allow pull-down.