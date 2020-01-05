---
layout: default
parent: Extension Guides
title: Custom Data Extension
nav_order: 12
description: ""
---

# Custom Data Extension

[Custom Data format](https://github.com/microsoft/vscode-custom-data) allows extension authors to easily extend VS Code's HTML / CSS language support without having to write code.

The two [Contribution Points](/api/references/contribution-points) for using custom data in an extension are:

- `contributes.html.customData`
- `contributes.css.customData`

For example, by including this section in an extension's `package.json`:

```json
{
  "contributes": {
    "html": {
      "customData": ["./html.html-data.json"]
    },
    "css": {
      "customData": ["./css.css-data.json"]
    }
  }
}
```

VS Code will load the HTML/CSS entities defined in both files and provide language support such as auto-completion and hover information for those entities.

You can find the [custom-data-sample](https://github.com/microsoft/vscode-extension-samples/tree/master/custom-data-sample) at [microsoft/vscode-extension-samples](https://github.com/Microsoft/vscode-extension-samples).
