# Performance Optimization Suggestions

July 6, 2023 

# Operating Principle

Unlike traditional H5 applications, the mini-program architecture consists of two parts: webview and worker. 

1. The webview is responsible for rendering, 
2. while the worker stores data and executes business logic.

# Performance Optimization for Initial Page Rendering

The definition of the initial page rendering can vary, but in this context, it refers to the first meaningful rendering from a business perspective. For example, in a list page, the initial page rendering is the first rendering of the list content.

## Controlling the Size of the Mini-Program Resource Package

When a user accesses a mini-program, the Alipay client will first download the mini-program resource package from the CDN. Therefore, the size of the resource package affects the startup performance of the mini-program.

Optimization Recommendations:

- Delete unused image resources in a timely manner, as all image resources are included by default in the package.
- Control the size of images and avoid using large images. It is recommended to upload large images through a CDN channel.
- Regularly clean up unused code to minimize the size of the resource package.

## Move Data Requests to `onLoad`

- When a mini-program is running, the onLoad lifecycle function of the page is triggered first, and then the initial data (Page data) of the page is transmitted from the worker to the webview for the initial rendering.
- Once the initial rendering of the page is completed, a notification is sent from the webview to the worker, triggering the onReady lifecycle function.

Some mini-programs make requests in onReady, causing delays in the initial rendering.

Optimization Recommendation:

- Move data requests to the onLoad function to prioritize them and avoid delaying the initial rendering of the first screen.

## Control the number of nodes rendered on the initial screen.

After the business request returns, it is common to call setData to trigger a page re-rendering. The execution process is as follows:

1. Data is transmitted from the worker to the webview.
2. On the webview, a virtual DOM is constructed based on the transmitted data and compared with the previous state by performing a diffing process starting from the root node, and then it is rendered.

Since data needs to be serialized during communication between the worker and the webview, and evaluateJavascript needs to be executed in the webview, if a large amount of data is transmitted at once, it can affect the performance of the initial screen rendering.

In addition, if there are too many nodes constructed on the webview, with deep nesting levels, for example, some mini-program list pages render over 100 list items at once, and each list item has nested content, but in reality, only around 10 items are visible on the screen. This can result in a longer diffing process, and since it's the initial screen rendering, a large number of DOM nodes are constructed at once, impacting the performance of the initial screen rendering.

Optimization Suggestions:

- Avoid passing a large amount of data in a single setData call, and avoid passing excessively long lists all at once.
- For the initial screen rendering, avoid constructing too many nodes at once. If the server-side sends a large amount of data in a single request, avoid using setData all at once. Instead, you can setData a portion of the data first, then wait for a certain period of time (e.g., 400ms, adjust according to the specific business needs), and then use $spliceData to transmit the remaining data.

# Optimizing the `setData` Logic

Any page change triggers a setData, and at the same time, multiple setData calls can trigger page re-rendering. The following four interfaces will trigger a webview page re-rendering:

- Page.prototype.setData: Triggers a diffing process for the entire page.
- Page.prototype.$spliceData: Optimized for long lists, avoids passing the entire list each time, and triggers a diffing process for the entire page.
- Component.prototype.setData: Only performs a diffing process starting from the corresponding component node.
- Component.prototype.$spliceData: Optimized for long lists, avoids passing the entire list each time, and only performs a diffing process starting from the corresponding component node.

Optimization suggestions:

- Avoid frequently triggering setData or $spliceData, whether at the page level or component level. In some analyzed cases, there were instances where countdown logic was implemented, but the countdown was triggered too frequently (at the millisecond level).
- If frequent re-rendering is needed, avoid using page-level setData and $spliceData. Instead, encapsulate that part into a custom component and use component-level setData or $spliceData to trigger component re-rendering.
- When rendering long list data, use $spliceData to append data multiple times instead of passing the entire list each time.
- For complex pages, it is recommended to encapsulate them into custom components to reduce the use of page-level setData.

Optimization Examples:

- It is recommended to set data at specific paths:

```jsx
this.setData({
  'array[0]': 1,
  'obj.x': 2,
});
```

- It is not recommended to use the following approach (even though it copies **`this.data`**, it still directly modifies its properties):

```jsx
const array = this.data.array.concat();
array[0] = 1;
const obj = { ...this.data.obj };
obj.x = 2;
this.setData({ array, obj })
```

- It is even more discouraged to directly modify **`this.data`** (violates the principle of immutable data):

```jsx
this.data.array[0] = 1;
this.data.obj.x = 2;
this.setData(this.data);
```

- For long lists, use **`$spliceData`**:

```jsx
this.$spliceData({ 'a.b': [1, 0, 5, 6] });
```

Note:
Sometimes, business logic is encapsulated within a component, and when the component's UI needs to be re-rendered, you can simply call **`setData`** internally within the component. However, there are cases where you need to trigger the component to re-render from the page. For example, if you listen to the **`onPageScroll`** event on the page and want to notify the corresponding component to re-render when the event is triggered, you can follow this approach:

```jsx
// pages/index/index.js
Page({
    onPageScroll(e) {
        if (this.xxcomponent) {
            this.xxcomponent.setData({
                scrollTop: e.scrollTop
            })
        }
    }
})
```

```jsx
// components/index/index.js
Component({
    didMount(){
        this.$page.xxcomponent = this;
    }
})
```

By mounting the component to the corresponding page in the **`didMount`** lifecycle method, you can call the component-level **`setData`** from the page to trigger the component to re-render.

# Using the **`key`** Parameter

To improve performance, you can use the **`key`** parameter within a **`for`** loop. Note that the **`key`** attribute cannot be set on a **`block`** element.

Example Code:

```html
<view a:for="{{array}}" key="{{item.id}}"></view>
<block a:for="{{array}}"><view key="{{item.id}}"></view></block>
```

Related Documentation:

- Page Configuration
- Mini-Program Runtime Mechanism