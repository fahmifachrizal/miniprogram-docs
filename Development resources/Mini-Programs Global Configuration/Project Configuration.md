# Project Configuration

# **Introduction**

**[openvideo-Project Configuration](https://gw.alipayobjects.com/mdn/rms_aefee5/afts/file/A*8c7BRqv_eyMAAAAAAAAAAAAAARQnAQ)**

Developers can create **`mini.project.json`** in the project root directory to implement project compilation, development, and other related functionalities.

## **Instructions**

- Alipay IDE version 3.0.1 or above
- Command-line tool CLI version 1.4.0 or above

# **Complete Properties**

The complete **`mini.project.json`** format2 has the following properties.

| Property | Type | Default Value | Description |
| --- | --- | --- | --- |
| format | Number | 2 | Fixed value |
| compileType | "mini" | "plugin" | "mini" | Compilation type to differentiate between a mini-program application or a mini-program plugin. |
| miniprogramRoot | Path String | "./" | Specifies the relative path of the mini-program source code (the directory where the app.json file is located).
- When compileType is miniprogram, it refers to the application directory.
- When compileType is plugin, it is only used for the host application directory for previewing mini-program plugins.
Note: Developers cannot preview mini-program plugins independently. To support this feature, Alipay has embedded a preview host application in the mini-program plugin project. |
| pluginRoot | Path String | - | Specifies the relative path of the plugin project (the directory where the plugin.json file is located).<br />Note: This property is only valid when compileType is plugin and is used to declare the plugin directory and the actual query path of plugin.json. For more details, refer to https://opendocs.alipay.com/mini/plugin/plugin-development. |
| compileOptions | Object | - | Compilation-related configurations. For more details, refer to the table below. |
| uploadExclude | String[] | - | Files to be ignored when uploading local code.
Accepts an array of strings. Supports glob syntax. Uses minimatch version 3.0.4 for matching. |
| assetsInclude | String[] | - | Assets to be included in the build output.
Accepts an array of strings. Supports glob syntax. Uses minimatch version 3.0.4 for matching. |
| developOptions | Object | - | Configuration related to local development |
| pluginResolution | Object | - | Plugin debugging options |
| scripts | Object | - | Configuration related to pre-build scripts for mini-programs |

## **compileOptions**

| Property | Type | Default Value | Description |
| --- | --- | --- | --- |
| component2 | Boolean | false | Whether to enable the new lifecycle running model for custom components. For more details, please refer to https://opendocs.alipay.com/mini/framework/component-lifecycle.
Note: If plugins or useDynamicPlugins is enabled in app.json, component2 mode will be forcibly enabled. |
| typescript | Boolean | false | Whether to enable TypeScript support for the mini-program. |
| less | String | false | Whether to enable Less support for the mini-program. |
| enableNodeModuleBabelTransform | Boolean | false | Whether to enable Babel compilation (minimal transpilation) for npm directories. |
| nodeModulesES6Whitelist | String[] | [] | Whether to enable Babel compilation for specific modules within npm. |
| treeShaking | Boolean | false | Whether to perform tree shaking optimization during production build. For more information, see https://developer.mozilla.org/zh-CN/docs/Glossary/Tree_shaking. |
| resolveAlias | Object | - | Path alias configuration supported since IDE 3.7.5. For detailed information, please refer to https://opendocs.alipay.com/mini/03dbc3#resolveAlias |

## **uploadExclude/assetsInclude**

The following are the default packaged resources for the mini-program build output.

### **assetsInclude Whitelist**

By default, the mini-program build output only includes the necessary business code and resource files, and unrecognized resource types will not be included in the package to reduce package size. The default packaged resource files are as follows:

- Images
    - .png
    - .jpg
    - .jpeg
    - .gif
    - .svg
    - .webp
- Fonts
    - .eot
    - .woff
    - .ttf
    - .woff2
    - .otf
- Multimedia
    - .mp3
    - .mp4

If the mini-program needs to include custom resource files, you can configure the **`assetsInclude`** whitelist. The following configuration will include all files with the .aaa and .bbb extensions in the package output:

```json
"assetsInclude" : [
  "**/*.aaa",
  "**/*.bbb",
]
```

### **uploadExclude Blacklist**

When uploading a mini-program, the local source code is packaged and sent to the cloud for building. In addition to the resource file list mentioned above, the source code package will also include the following contents:

- Mini-program source code files
    - .acss
    - .axml
    - .js
    - .json
    - .sjs
- Dependencies
    - node_modules directory

If the source code package, even after being compressed into a zip file, still exceeds the IDE threshold (currently 20MB), an error message of **Package Size Exceeded** will be displayed during the upload. To resolve this, you can configure the **`uploadExclude`** blacklist to exclude files that are not needed for cloud building, such as pre-compiled code in the src > dist directory and devDependencies outside the miniprogramRoot directory. The following configuration means that the source code package will not include files in the src and node_modules directories under the project root directory:

```json
"uploadExclude": [
  "src/**",
  "node_modules/**"
]
```

## **developOptions**

The **`developOptions`** field accepts an object with the following properties.

| Property | Type | Default Value | Description |
| --- | --- | --- | --- |
| hotReload | Boolean | false | Whether to enable hot reloading of the build output. Configuring this option enables hot updates in the simulator.<br />Supported scopes:<br />- AXML<br />- ACSS<br />- Methods in JS files<br /> |
| disableParallel | Boolean | false | Whether to disable multi-process building. By default, the mini-program uses multi-process compilation. If resource usage is too high, you can disable parallel processing using disableParallel. |
| disableSourcemap | Boolean | false | Whether to disable sourcemap. By default, sourcemap is enabled during local development. If resource usage is too high, you can disable sourcemap using disableSourcemap. |

## **pluginResolution**

The **`pluginResolution`** field accepts an object with the following properties.

| Property | Type | Default Value | Description |
| --- | --- | --- | --- |
| enable | Boolean | false | Whether to enable debugging configuration. |
| plugins | Record<string, Object> | {} | Specifies the versions of static plugins for plugin debugging. |
| dynamicPlugins | Record<string, Object> | {} | Specifies the versions of dynamic plugins for plugin debugging. |

When you need to perform **[Plugin Quick Debugging](https://opendocs.alipay.com/mini/plugin/01phjs)**, you can set **`enable`** to **`true`** and configure the corresponding plugin version information in **`plugins`** and **`dynamicPlugins`**. The key is the plugin **`appId`**, and the value is the plugin version information with the following properties.<br />**Note:** Quick debugging is only effective in offline versions generated by the IDE simulator, real device preview, real device debugging, etc.

| Property | Type | Required | Description |
| --- | --- | --- | --- |
| version | String | Yes | Plugin debugging version information |

Here's a common debugging example, which specifies that the static plugin **`2020111122223333`** is using the specific debugging version **`dev:a.b.c.d`**.

```
jsonCopy code
{
  "pluginResolution": {
    "enable": true,
    "plugins": {
      "2020111122223333": {
        "version": "dev:a.b.c.d"
      }
    }
  }
}

```

## **scripts**

For preprocessing scenarios, a configuration entry **`precompile`** is provided, allowing users to execute preprocessing logic before compilation/preview/upload. This logic will take effect in the IDE, as well as in the partner and Rainbird link.

The **`scripts`** field accepts an object with the following properties.

| Property | Type | Execution Timing |
| --- | --- | --- |
| watch | String | Executed during IDE simulator compilation, the process persists. |
| beforeCompile | String | Executed during IDE simulator compilation, the beforeCompile process exits after execution. |
| beforePreview | String | Executed before real device preview/real device debugging. |
| beforeUpload | String | Executed before IDE upload. |

### **Execution Timing**

- During IDE simulator compilation:
    - The **`watch`** script has a higher priority than **`beforeCompile`**, only one of them will be executed.
    - In **`watch`** mode, the pre-build script runs concurrently with the IDE build server.
    - In **`beforeCompile`** mode, the IDE build server waits for the script to finish before starting.
- The **`beforePreview`** hook will be executed before real device preview/real device debugging.
- The **`beforeUpload`** hook will be executed before IDE upload.

## **resolveAlias**

By using the **`resolveAlias`** configuration option, you can customize the mapping rules for source file paths. The keys in **`resolveAlias`** represent the matching rules for file paths in the code, and the values represent the mapped real paths.

### **Example**

Suppose the mini-program project path is **`/Workspace/todoApp`**.

The content of its **`mini.project.json`** is as follows:

```json
{
  "format": 2,
  "miniprogramRoot": "./src",
  "compileOptions": {
    "resolveAlias": {
      "a/*": "./a/*",
      "b/*": "../b/*",
      "c/*": "c/*"
    }
  }
}
```

When the specified paths in **`resolveAlias`** are referenced in JavaScript files within the mini-program project, the following effects will be achieved:

```jsx
import "a/utils" /* Maps to /Workspace/todoApp/a/utils */
import "b/utils" /* Maps to /Workspace/b/utils */
import "c/utils" /* Maps to /Workspace/todoApp/c/utils */
```

**Note:** The **`b/utils`** is a reference to a file outside the project, so the build will fail. It is provided here for demonstration purposes only.

### **Mapping Rule Limitations**

1. The target paths configured in **`resolveAlias`** are all relative to the directory where **`mini.project.json`** is located.
2. **`resolveAlias`** only performs directory-level aliasing, so the keys in **`resolveAlias`** must end with **`/*`**.
3. **`resolveAlias`** only performs prefix matching on paths and cannot configure more complex rules.
4. The keys in **`resolveAlias`** cannot start with **`/`** because paths starting with **`/`** in a mini-program always have higher priority than aliases and are relative to **`miniprogramRoot/pluginRoot`**.
5. The values in **`resolveAlias`** cannot start with **`/`**.
6. If there are two rules, and one rule is a prefix of the other, the **most specific key** will take effect during matching.

For example, if the **`resolveAlias`** configuration contains both **`hello/world/* => ./a/*`** and **`hello/* => ./b/*`**, then **`hello/world/a`** will point to **`./a/a`**, and **`hello/b`** will point to **`./b/b`**.

### **Scope of Effectiveness**

In JavaScript/TypeScript files:

- **`import`** statements
- **`require()`** function calls

In Acss/Less files:

- **`@import`** directives
- **`url()`** function calls

In SJS files:

- **`import`** statements

In AXML files:

- **`<import>`**
- **`<include>`**
- **`<import-sjs>`**

**Note:**

- Paths written in all other places, such as app.json and various .json files, will not follow the **`resolveAlias`** configuration.
- **`resolveAlias`** does not take effect on files in the node_modules.

## **Upgrading to format2 Steps**

| mini.project.json | mini.project.json format2 | Description |
| --- | --- | --- |
| enableAppxNg | - | format2 enables Base Library 2.x by default. |
| component2 | compileOptions.component2 | - |
| enableNodeModuleBabelTransform | compileOptions.enableNodeModuleBabelTransform | - |
| node_modules_es6_whitelist | compileOptions.nodeModulesES6Whitelist | - |
| include | assetsInclude | Provides a clear semantic meaning for assets that need to be included in the build output. |
| exclude | uploadExclude | Provides a clear semantic meaning for files to be ignored when uploading local code. |
| enableHMR | developOptions.hotReload | - |
| debugOptions | pluginResolution | Provides a clear semantic meaning for plugin quick debugging configuration. |