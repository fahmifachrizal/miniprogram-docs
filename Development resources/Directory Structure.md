# Directory Structure

July 6, 2023 

# Directory Structure

A mini-program project consists of 3 global files:

| File | Required | Description |
| --- | --- | --- |
| app.js | Yes | Application logic |
| app.json | Yes | Application configuration |
| app.acss | No | Application global style |

A mini-program page consists of 4 files:

| File Type | Required | Description |
| --- | --- | --- |
| js | Yes | Page logic |
| json | Yes | Page configuration |
| axml | Yes | Page structure |
| acss | No | Page style |

Create a **`mini.project.json`** file in the project root directory to add additional configuration items for project compilation and development functionality. The **`miniprogramRoot`** configuration item is used to specify the directory of the mini-program source code, where the **`app.json`** file is located.

# Directory Structure Example

Example directory structure:

```markdown
.
├── mini.project.json
├── app.acss
├── app.js
├── app.json
└── pages
    └── index
        ├── index.acss
        ├── index.axml
        ├── index.js
        └── index.json

```