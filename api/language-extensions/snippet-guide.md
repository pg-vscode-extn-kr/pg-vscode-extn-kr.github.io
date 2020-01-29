---
layout: default
parent: Language Extensions
title: Snippet Guide
nav_order: 3
description: ""
---

# Snippet 가이드
<!--
# Snippet Guide -->

여러분은 [`contributes.snippets`](/api/references/contribution-points#contributes.snippets) Contribution Point를 사용하여 Visual Studio Code 익스텐션에 snippet을 번들링 하여 공유 할 수 있습니다. 
<!--
The [`contributes.snippets`](/api/references/contribution-points#contributes.snippets) Contribution Point allows you to bundle snippets into a Visual Studio Code extension for sharing. -->

[snippet 생성](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_creating-your-own-snippets) 주제에는 snippet 생성에 관한 모든 정보가 포함되어 있습니다. 이번 가이드와 예시에서는 여러분의 snippet을 익스텐션으로 바꾸는 방법만을 보여주고 있습니다. 제안 되는 작업 순서는 아래와 같습니다 :
<!--
The [Creating snippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_creating-your-own-snippets) topic contains all information for creating snippets. This guide / sample just shows how you can turn your own snippets into an extension for sharing. The suggested workflow is:-->


- `Preferences: Configure User Snippets` 커맨드를 이용하여 snippet을 생성하고 테스트하십시오
- snippet이 정상작동할 경우, 전체 JSON 파일을 익스텐션 폴더로 복사하십시오
- `package.json`에 다음 snippet contribution을 추가하십시오

<!--
- Create and test your snippets using `Preferences: Configure User Snippets` command
- Once you are happy with the snippets, copy the whole JSON file into an extension folder, such as `snippets.json`
- Add the following snippet contribution to your `package.json`
-->

```json
{
  "contributes": {
    "snippets": [
      {
        "language": "javascript",
        "path": "./snippets.json"
      }
    ]
  }
}
```

완전한 소스코드를 이곳에서 확인 할 수 있습니다: https://github.com/Microsoft/vscode-extension-samples/tree/master/snippet-sample

<!--
You can find the complete source code at: https://github.com/Microsoft/vscode-extension-samples/tree/master/snippet-sample-->

**팁**: `package.json`에 아래와 같은 설정을 통해 여러분의 익스텐션에 snippet 익스텐션 태그를 붙이십시오:
<!--
**Tip**: Tag your extension as a snippet extension with the following config in your `package.json`: -->

```json
{
  "categories": ["Snippets"]
}
```

## TextMate snippets 사용
<!--
## Using TextMate snippets -->

[yo code](/api/get-started/your-first-extension) 익스텐션 생성기를 사용하여 VS Code 설치에 TextMate snippet (.tmSnippets)을 추가 하는 것도 가능합니다. 생성기는 여러개의 .tmSnippets 파일을 포함한 폴더를 지정하는 `New Code Snippets`라는 옵션이 있으며 그 파일들은 VS Code snippet 익스텐션으로 패키징 될 것입니다. 생성기는 Sublime snippets 또한 지원합니다. (.sumlime-snippets) 
<!--
You can also add TextMate snippets (.tmSnippets) to your VS Code installation using the [yo code](/api/get-started/your-first-extension) extension generator. The generator has an option `New Code Snippets` which lets you point to a folder containing multiple .tmSnippets files and they will be packaged into a VS Code snippet extension. The generator also supports Sublime snippets (.sublime-snippets).-->

생성기의 최종 출력물은 2 파일로 구성됩니다 : snippet을 VS Code에 통합하기 위한 메타데이터를 갖는 익스텐션 manifest `package.json` 과, VS Code snippet 형태로 변환된 snippet을 포함하는 `snippets.json`파일.

<!--
The final generator output has two files: an extension manifest `package.json` which has metadata to integrate the snippets into VS Code and a `snippets.json` file which includes the snippets converted to the VS Code snippet format.-->

```bash
.
├── snippets                    // VS Code integration
│   └── snippets.json           // The JSON file w/ the snippets
└── package.json                // extension's manifest
```

생성된 snippet 폴더를 `.vscode/extensions` 폴더 아래의 새 폴더로 복사한 다음 VS Code를 재 시작하십시오. 

<!--Copy the generated snippets folder to a new folder under your `.vscode/extensions` folder and restart VS Code.
-->
