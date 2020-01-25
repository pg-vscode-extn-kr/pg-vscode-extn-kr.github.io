---
layout: default
parent: Extension Guides
title: Markdown Extension
nav_order: 11
description: ""
---

# Markdown 익스텐션
<!--
# Markdown Extension -->

Markdown 익스텐션은 여러분이 Visual Studio Code의 빌트인 Markdown 미리보기를 확장하고 개량 할 수 있게 합니다. 이는 미리보기의 모양을 바꾸거나 새로운 Markdown 표현을 더하는 것을 포함합니다.

<!--
Markdown extensions allow you to extend and enhance Visual Studio Code's built-in Markdown preview. This includes changing the look of the preview or adding support for new Markdown syntax. -->

## CSS로 Markdown 미리보기 모양 변경
<!--
## Changing the look of the Markdown preview with CSS -->

익스텐션은 CSS를 이용하여 Markdown 미리보기의 모양이나 레이아웃을 변경 할 수 있습니다. 익스텐션의 `package.json`안에 있는 `markdown.previewStyles`[Contribution Point](/api/references/contribution-points)를 이용하여 스타일시트를 등록 할 수 있습니다.

<!--
Extensions can contribute CSS to change the look or layout of the Markdown preview. Stylesheets are registered using the `markdown.previewStyles` [Contribution Point](/api/references/contribution-points) in the extension's `package.json`: -->

```json
"contributes": {
    "markdown.previewStyles": [
        "./style.css"
    ]
}
```

`"markdown.previewStyles"`는 익스텐션의 루트 폴더에 있는 파일들의 목록입니다. 

<!-- `"markdown.previewStyles"` is a list of files relative to the extension's root folder.-->

사용된 스타일은 빌트인 Markdown 미리보기 스타일 보다는 나중에, 사용자의 `"markdown.styles"`보다는 먼저 적용 됩니다. 
<!--
Contributed styles are added after the built-in Markdown preview styles but before a user's `"markdown.styles"`. -->

[Markdown Preview GitHub Styling](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-preview-github-styles) 익스텐션은 스타일 시트를 사용하여 Markdown 미리보기 모양을 Github의 Markdown 처럼 바꾸는 좋은 예시 입니다. 여러분은 익스텐션의 소스 코드를 [GitHub](https://github.com/mjbvz/vscode-github-markdown-preview-style)에서 검토 할 수 있습니다.

<!--
The [Markdown Preview GitHub Styling](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-preview-github-styles) extension is a good example that demonstrates using a stylesheet to make the Markdown preview look like GitHub's rendered Markdown. You can review the extension's source code on [GitHub](https://github.com/mjbvz/vscode-github-markdown-preview-style). -->

## Markdown-it 플러그인으로 새로운 구문 지원 추가 

<!--
## Adding support for new syntax with markdown-it plugins -->

VS Code Markdown 미리보기는 [CommonMark specification](https://spec.commonmark.org)를 지원합니다. 익스텐션은 [markdown-it plugin.](https://github.com/markdown-it/markdown-it#syntax-extensions)를 작성하는 것으로 추가 Markdown 구문을 지원 할 수있습니다. 

<!--
The VS Code Markdown preview supports the [CommonMark specification](https://spec.commonmark.org). Extensions can add support for additional Markdown syntax by contributing a [markdown-it plugin.](https://github.com/markdown-it/markdown-it#syntax-extensions) -->

markdown-it 플러그인을 사용하기 위해서, 먼저 `"markdown.markdownItPlugins"` contribution을 여러분 익스텐션의 `package.json`에 추가하십시오:

<!--
To contribute a markdown-it plugin, first add a `"markdown.markdownItPlugins"` contribution in your extension's `package.json`: -->

```json
"contributes": {
    "markdown.markdownItPlugins": true
}
```

그 다음, 익스텐션의 메인 `activation`함수에서, `extendMarkdownIt`라는 이름을 가진 함수를 포함한 오브젝트를 리턴하십시오. 이 함수는 현재 markdown-it 인스턴스를 받아 새로운 markdown-it 인스턴스를 리턴 해야 합니다. 

<!--
Then, in the extension's main `activation` function, return an object with a function named `extendMarkdownIt`. This function takes the current markdown-it instance and must return a new markdown-it instance: -->

```ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  return {
    extendMarkdownIt(md: any) {
      return md.use(require('markdown-it-emoji'));
    }
  };
}
```

여러개의 markdown-it 플러그인을 사용하기 위해서는, 단순히 여러개의 연속된 `use` 표현을 사용하십시오:
<!--
To contribute multiple markdown-it plugins, simply return multiple `use` statements chained together: -->

```ts
return md.use(require('markdown-it-emoji')).use(require('markdown-it-hashtag'));
```

markdown-it 플러그인을 사용하는 익스텐션은 Markdown 미리보기가 처음으로 보이는 때에 lazy하게 활성화 됩니다. 
<!--
Extensions that contribute markdown-it plugins are activated lazily, when a Markdown preview is shown for the first time. -->

[markdown--emoji](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-emoji) 익스텐션은 markdown-it 플러그인을 사용하여 markdown 미리보기에 이모티콘 지원을 추가합니다. 여러분은 이 익스텐션의 소스 코드를 [GitHub](https://github.com/mjbvz/vscode-markdown-emoji)에서 검토 할 수 있습니다. 

<!--
The [markdown-emoji](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-emoji) extension demonstrates using a markdown-it plugin to add emoji support to the markdown preview. You can review the Emoji extension's source code on [GitHub](https://github.com/mjbvz/vscode-markdown-emoji). -->

다음을 검토하는것도 좋습니다:
<!--
You may also want to review: -->

- [Guidelines](https://github.com/markdown-it/markdown-it/blob/master/docs/development.md) for markdown-it plugin developers
- [Existing markdown-it plugins](https://www.npmjs.com/browse/keyword/markdown-it-plugin)

## 스크립트로 고급 기능 추가

<!--
## Adding advanced functionality with scripts -->

고급 기능을 위해서, 익스텐션은 Markdown 미리보기 내부에서 실행되는 스크립트를 사용할 수 있습니다.
<!--
For advanced functionality, extensions may contribute scripts that are executed inside of the Markdown preview. -->

```json
"contributes": {
    "markdown.previewScripts": [
        "./main.js"
    ]
}
```

사용된 스크립트는 비동기 적으로 로드되고, 컨텐츠가 바뀔 때 마다 다시 로드 됩니다.

<!--
Contributed scripts are loaded asynchronously and reloaded on every content change. -->

[Markdown Preview Mermaid Support](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-mermaid) 익스텐션은 스크립트를 사용하여 [mermaid](https://knsv.github.io/mermaid/index.html) 다이어그램과 flowchart를 markdown 미리보기에 추가 합니다. Mermaid 익스텐션의 소스 코드를 [GitHub](https://github.com/mjbvz/vscode-markdown-mermaid)에서 확인하십시오. 

<!--
The [Markdown Preview Mermaid Support](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-mermaid) extension demonstrates using scripts to add [mermaid](https://knsv.github.io/mermaid/index.html) diagrams and flowchart support to the markdown preview. You can review the Mermaid extension's source code on [GitHub](https://github.com/mjbvz/vscode-markdown-mermaid).-->
