# Project Configuration (Old Version)

# **Introduction**

Configure project compilation and packaging strategies in the root directory **`mini.project.json`**. It is recommended to use the new version of **[Project Configuration](https://opendocs.alipay.com/mini/03dbc3)**.

# **Configuration Options**

| Field Name | Type | Description |
| --- | --- | --- |
| miniprogramRoot | String | The directory of the mini-program source code, relative path. It should contain the app.json file. |
| pluginRoot | String | The directory of the plugin source code, relative path. |
| compileType | String | Compilation type.
Possible values: 
- mini (Mini-Program)
- plugin (Plugin)
Default: mini |
| axmlStrictCheck | Boolean | AXML strict syntax check. Errors such as unclosed tags can be detected during build, and there is a corresponding configuration option in the IDE details panel.
Default: false |
| component2 | Boolean | Whether to enable component2 compilation. For more details, see https://opendocs.alipay.com/mini/framework/component-lifecycle. There is a corresponding configuration option in the IDE details panel.
Default: false |
| nonLoadingIndicator | Boolean | Whether to hide the default loading animation when initializing the mini-program.
Default: true |
| enableParallelLoader | Boolean | Enable multi-process compilation, which has a noticeable effect when there is no cache for the first time. There is a corresponding configuration option in the IDE details panel.
Default: false |
| enableDistFileMinify | Boolean | Whether to compress the final output during real device preview/real device debugging. There is a corresponding configuration option in the IDE details panel.
Default: false |
| scripts | Object | Custom pre-processing scripts. For more details, see https://opendocs.alipay.com/mini/ide/01s3hm#%E9%A2%84%E5%A4%84%E7%90%86%E8%84%9A%E6%9C%AC.
Default: null |
| include | Array | Files/directories to include during packaging, following the Glob syntax. See include whitelist for more details. |
| exclude | Array | Files/directories to ignore during packaging, following the Glob syntax. See exclude whitelist for more details. |
| enableNodeModuleBabelTransform | Boolean | appx 2.0 feature, babel compiles modules in node_modules.
Default: false |
| enableAppxNg | Boolean | Enable appx 2.0 compilation. For the upgrade details, see https://opendocs.alipay.com/mini/framework/lib-upgrade-v2.
Default: false |
| treeShaking | Boolean | Whether to perform tree shaking optimization during production build. See https://developer.mozilla.org/zh-CN/docs/Glossary/Tree_shaking.
Default: false |
| debugOptions | Object | WIP: Additional configuration for debugging/interoperability, does not take effect during production build. For more details, see https://opendocs.alipay.com/mini/ide/01s3hm#debugOptions.
Default: null |

## **Preprocessing Scripts**

With pre-build, users can execute custom commands before **compilation** / **preview** / **upload**. This feature can be used for handling scenarios such as TypeScript/Less compilation and framework-to-mini-program conversion.

Preprocessing scripts are written under the **`scripts`** field in **`mini.project.json`**.

### **Configuration Example**

```json
{
  "scripts": {
    "watch": "npm run something --watch",
    "beforeCompile": "npm run compile",
    "beforePreview": "npm run compile",
    "beforeUpload": "npm run compile"
  }
}
```

### **Trigger Timing**

- During IDE simulator compilation:
    - The "watch" script takes priority over "beforeCompile", and only one of them will be executed.
    - In watch mode, the precompile script runs concurrently with the IDE build server.
    - In beforeCompile mode, the IDE build server waits for the script to finish before starting.
- Before previewing/debugging on a real device, the "beforePreview" hook will be executed.
- Before uploading in the IDE, the "beforeUpload" hook will be executed.

## **include Packaging Whitelist**

By default, the built package of the mini-program only includes the necessary business code and resource files. Unrecognized resource types will not be included in the package to reduce its size. The default resource files to be packaged are as follows:

- Images:
    - .png
    - .jpg
    - .jpeg
    - .gif
    - .svg
    - .webp
- Fonts:
    - .eot
    - .woff
    - .ttf
    - .woff2
    - .otf
- Multimedia:
    - .mp3
    - .mp4

If the mini-program needs to include custom resource files, you can configure the "include" whitelist. The following configuration will also include all files with the .aaa and .bbb extensions in the package:

```jsx
"include": [
  "**/*.aaa",
  "**/*.bbb"
]
```

## **exclude Packaging Blacklist**

During mini-program uploading, the local source code is packaged and sent to the cloud for build. In addition to the resource file list mentioned above, the source code package also includes the following content:

- Mini-program source code files:
    - .acss
    - .axml
    - .js
    - .json
    - .sjs
- Dependency packages:
    - node_modules directory

If the size of the source code package exceeds the IDE threshold (currently 20MB) even after being compressed into a zip file, an "Exceeded Package Size" error will be reported during uploading. To exclude unnecessary files from the cloud build, such as precompiled code in src > dist or devDependencies outside the miniprogramRoot directory, you can configure the "exclude" blacklist. The following configuration indicates that the source code package will not include files in the src and node_modules directories under the project root:

```json
"exclude": [
  "src/**",
  "node_modules/**"
]
```

## **debugOptions**

Additional configuration for debugging/co-debugging, which does not take effect during production builds.

- enable: Boolean, whether to enable this configuration.
- plugins: Object, plugin debugging configuration, specifying certain plugins to use offline versions (with four-digit version numbers) for co-debugging.
- dynamicPlugins: Object, dynamic plugin debugging configuration (format is the same as plugins).

### **Configuration Example**

```json
"debugOptions": {
  "enable": true,
  "plugins": {
    "2021001126652765": {
      "version": "dev:0.2.2009071414.41"
    }
  },
  "dynamicPlugins": {
    "2021001126652765": {
      "version": "inspect:0.2.2009181119.27"
    }
  }
}
```