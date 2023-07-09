# app.js registers the mini program

# **App(object: Object)**

**`App()`** is used to register a mini program and accepts an **`Object`** as its parameter to configure the lifecycle and other settings of the mini program. **`App()`** must be called in the **`app.js`** file and can only be called once.

## **Description of the `object` properties**

| Property | Type | Description | Trigger Event | Minimum Base Library Version |
| --- | --- | --- | --- | --- |
| onLaunch | Function | Lifecycle callback: listens to app init | Triggered when the mini program is initialized. | - |
| onShow | Function | Lifecycle callback: listens to app show | Triggered when the mini program is shown. | - |
| onHide | Function | Lifecycle callback: listens to app hide | Triggered when the mini program is hidden. | - |
| onError | Function | Listens to mini program errors | Triggered when a JavaScript error occurs. | - |
| onShareAppMessage | Function | Global share configuration | Triggered when the user clicks the Share button in the upper right corner of the page menu. | - |
| onUnhandledRejection | Function | Listens to the unhandledrejection event | Triggered when a Promise is rejected without a rejection handler. | https://opendocs.alipay.com/mini/framework/lib |
| onPageNotFound | Function | Listens to page not found events | Triggered when the requested page does not exist. | https://opendocs.alipay.com/mini/framework/lib-upgrade-v2 |

**Foreground/Background Definitions:**

- Generally, when a user closes the mini program by clicking the close button in the upper right corner or by pressing the Home button on the device, the mini program does not get destroyed immediately but goes into the background.
- When the user re-opens Alipay or re-opens the mini program, the mini program goes from the background to the foreground.
- The mini program is only completely destroyed if it stays in the background for 5 minutes or if it consumes excessive system resources.
- Whether the mini program is destroyed or enters the background also depends on the mini program's business logic and the current memory resource usage.

## **onLaunch(object: Object) and onShow(object: Object)**

Description of the **`object`** properties:

| Property | Type | Description |
| --- | --- | --- |
| query | Object | The query of the current mini program, parsed from the query field in the launch options. The parsing rules can be found in the https://opendocs.alipay.com/mini/03durs document. |
| scene | String | The https://opendocs.alipay.com/mini/framework/scene that launches the mini program. |
| path | String | The page address of the current mini program, parsed from the page field in the launch options. The default value is the homepage if the page field is ignored. |
| referrerInfo | Object | Information about the referrer. |

For example, if the mini program is launched with the following scheme:

```jsx
alipays://platformapi/startapp?appId=1999&query=number%3D1&page=x%2Fy%2Fz
```

- When the mini program is launched for the first time, the **`onLaunch`** method can access the **`query`**, **`path`**, and other property values.
- When the mini program is in the background and reopened from a scheme or QR code, the **`onShow`** method is used to obtain the **`query`**, **`path`**, and other property values.

```jsx
App({
  onLaunch(options) {
    // First launch
    console.log(options.query);
    // {number:1}
    console.log(options.path);
    // x/y/z
  },
  onShow(options) {
    // Reopened from the background using a scheme
    console.log(options.query);
    // {number:1}
    console.log(options.path);
    // x/y/z
  },
});

```

Description of the **`referrerInfo`** sub-properties:

| Property | Type | Description | Minimum Base Library Version |
| --- | --- | --- | --- |
| appId | String | The source mini program. | - |
| extraData | Object | Data passed from the source mini program. | - |

**Note:**

- Do not perform page stack manipulation operations such as **[my.redirectTo](https://opendocs.alipay.com/mini/api/fh18ky)** or **[my.navigateTo](https://opendocs.alipay.com/mini/api/zwi8gx)** in **`onShow()`**.
- Do not call the **[getCurrentPages](https://opendocs.alipay.com/mini/framework/getcurrentpages)** method in **`onLaunch()`** because the **`page`** has not been created yet.
- The **`decodeQuery`** option in the **`behavior`** configuration in **`app.json`** allows disabling the additional **`decodeComponent`** on the key-value pairs. Refer to the **[Mini Program Global/Page Parameter Settings and Parsing Details](https://opendocs.alipay.com/mini/03durs)** document for more information.

## **onHide()**

The **`onHide()`** function is triggered when the mini program goes from the foreground to the background. Example code:

```jsx
App({
  onHide() {
    // When entering the background
    console.log('app hide');
  },
});
```

## **onError(error, stack)**

The **`onError()`** function is triggered when a script error occurs in the mini program application. This event can also be listened to using **[my.onError](https://opendocs.alipay.com/mini/00nnsx)**. The function has the following parameters:

| Property | Type | Description |
| --- | --- | --- |
| error | String | The description of the exception, usually the message field of the Error object. |
| stack | String | The stack trace of the exception, usually the stack field of the Error object.<br />Minimum Base Library Version: 2.6.6. |

Example code:

```jsx
App({
  onError(error, stack) {
    // When the mini program encounters an error
    console.log(error);
    // Minimum Base Library Version: 2.6.6, supports the `stack` parameter
    console.error(stack);
  },
});
```

## **onShareAppMessage(object: Object)**

Global configuration for sharing. When a page does not set **`page.onShareAppMessage`**, the global sharing settings will be used when sharing is invoked. For more details, please refer to the **[Page Event Handling Functions](https://opendocs.alipay.com/mini/framework/page-detail#%E9%A1%B5%E9%9D%A2%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86%E5%87%BD%E6%95%B0)** document.

## **onUnhandledRejection(object: Object)**

Triggered when a **`Promise`** is rejected without a reject handler. It can also be listened to using **[my.onUnhandledRejection](https://opendocs.alipay.com/mini/00nd0f)**. The parameter and notes are the same as **[my.onUnhandledRejection](https://opendocs.alipay.com/mini/00nd0f)**. Example code:

```jsx
App({
  onUnhandledRejection(res) {
    // Triggered when a Promise in the mini program code is rejected without a reject handler.
    console.log(res.reason, res.promise);
    // res.reason is the reason for the rejection, and res.promise is the rejected Promise object.
  },
});
```

## **onPageNotFound(object: Object)**

Triggered when the page that the mini program tries to open does not exist. It can also be listened to using **[my.onPageNotFound](https://opendocs.alipay.com/mini/01zdng)**. The parameter and notes are the same as **`my.onPageNotFound`**. Example code:

```jsx
App({
  onPageNotFound(res) {
    my.redirectTo({
      url: '/pages/...',
    }); // Use my.switchTab if it is a tab bar page
  },
});
```

# **globalData Global Data**

The **`App()`** function can set global data **`globalData`**. Example code:

```jsx
// app.js
App({
  globalData: 1,
});
```

# **More**

Developers can add any functions or data variables to the **`Object`** parameter, which can be accessed using **`this`**.

Other common methods can also be imported into **`app.js`** and attached to **`app.js`**.

Example code:

```jsx
// app.js
import { getUserInfo } from '/utils/getOpenUserInfo';
App({
  onLaunch() {},
  onShow() {
    this.login(); // Accessed using `this`
  },
  // Custom function
  login() {
    console.log('Custom function');
  },
  getUserInfo,
});
```

Accessing in a mini program page:

```jsx
const app = getApp();
Page({
  onLoad() {
    app.getUserInfo();
    app.login(); // Logs 'Custom function'
  },
});
```

# **Frequently Asked Questions**

### **Q: Can I close the mini program in `app.js`?**

A: No, the method to close the mini program is only available for mini program pages by clicking the close button in the upper-right corner.

# **Related Documentation**

- **[onShareAppMessage](https://opendocs.alipay.com/mini/framework/page-detail#%E9%A1%B5%E9%9D%A2%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86%E5%87%BD%E6%95%B0)**
- **[getApp Method](https://opendocs.alipay.com/mini/framework/get-app)**