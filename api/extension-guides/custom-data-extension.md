---
layout: default
parent: Extension Guides
title: Custom Data Extension
nav_order: 12
description: ""
---

# 커스텀 데이터 익스텐션

<!--
# Custom Data Extension -->

[커스텀 데이터 포맷](https://github.com/microsoft/vscode-custom-data)를 통해 익스텐션 작성자는 추가 코드 작성 없이, VS Code의 HTML / CSS 언어 지원 확장 할 수 있습니다.

<!--
[Custom Data format](https://github.com/microsoft/vscode-custom-data) allows extension authors to easily extend VS Code's HTML / CSS language support without having to write code. -->

익스텐션에 커스텀 데이터를 위해 쓰이는 2가지 [Contribution Points](/api/references/contribution-points)는 아래와 같습니다. :

<!--
The two [Contribution Points](/api/references/contribution-points) for using custom data in an extension are: -->

- `contributes.html.customData`
- `contributes.css.customData`

예를 들어, 익스텐션의 `package.json`에 다음을 포함하여: 

<!-- For example, by including this section in an extension's `package.json`:-->

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

VS Code는 모든 파일에 정의 된 HTML/CSS를 로드하고 자동 완성이나 호버링 같은 기능을 지원 할 것입니다.

<!--
VS Code will load the HTML/CSS entities defined in both files and provide language support such as auto-completion and hover information for those entities. -->

[예시 커스텀 데이터](https://github.com/microsoft/vscode-extension-samples/tree/master/custom-data-sample)를 [microsoft/vscode-extension-samples](https://github.com/Microsoft/vscode-extension-samples)에서 확인 할 수 있습니다. 

<!--
You can find the [custom-data-sample](https://github.com/microsoft/vscode-extension-samples/tree/master/custom-data-sample) at [microsoft/vscode-extension-samples](https://github.com/Microsoft/vscode-extension-samples).-->
