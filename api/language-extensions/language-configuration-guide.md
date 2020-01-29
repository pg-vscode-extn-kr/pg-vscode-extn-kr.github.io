---
layout: default
parent: Language Extensions
title: Language Configuration Guide
nav_order: 4
description: ""
---

# 언어 구성 가이드
<!--
# Language Configuration Guide -->

[`contributes.languages`](/api/references/contribution-points#contributes.languages) Contribution Point를 사용하여 선언적 언어 기능 구성을 조절하는 언어 구성을 정의 할 수 있습니다. 

<!--
The [`contributes.languages`](/api/references/contribution-points#contributes.languages) Contribution Point allows you to define a language configuration that controls the following Declarative Language Features: -->

- 주석 설정
- 괄호 정의
- 자동 폐쇄
- 자동 포함
- 접기
- 단어 패턴
- 들여쓰기 규칙

<!--
- Comment toggling
- Brackets definition
- Autoclosing
- Autosurrounding
- Folding
- Word pattern
- Indentation Rules
-->

다음은 자바스크립트 파일의 편집 환경을 구성하는 [언어 구성 예시](https://github.com/Microsoft/vscode-extension-samples/tree/master/language-configuration-sample)입니다. 이번 가이드에서는 `language-configuration.json`의 내용을 설명합니다. 

<!--
Here is a [Language Configuration Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/language-configuration-sample) that configures the editing experience for JavaScript files. This guide explains the content of `language-configuration.json`:
-->

**주의**: 언어 구성 파일의 이름이 `language-configuration.json`이거나 혹은 이로 끝날 경우, VS Code에서 자동 완성 및 유효성 검사가 제공됩니다.

<!--
**Note: If your language configuration file name is or ends with `language-configuration.json`, you will get autocompletion and validation in VS Code.**-->

```json
{
  "comments": {
    "lineComment": "//",
    "blockComment": ["/*", "*/"]
  },
  "brackets": [["{", "}"], ["[", "]"], ["(", ")"]],
  "autoClosingPairs": [
    { "open": "{", "close": "}" },
    { "open": "[", "close": "]" },
    { "open": "(", "close": ")" },
    { "open": "'", "close": "'", "notIn": ["string", "comment"] },
    { "open": "\"", "close": "\"", "notIn": ["string"] },
    { "open": "`", "close": "`", "notIn": ["string", "comment"] },
    { "open": "/**", "close": " */", "notIn": ["string"] }
  ],
  "autoCloseBefore": ";:.,=}])>` \n\t",
  "surroundingPairs": [
    ["{", "}"],
    ["[", "]"],
    ["(", ")"],
    ["'", "'"],
    ["\"", "\""],
    ["`", "`"]
  ],
  "folding": {
    "markers": {
      "start": "^\\s*//\\s*#?region\\b",
      "end": "^\\s*//\\s*#?endregion\\b"
    }
  },
  "wordPattern": "(-?\\d*\\.\\d\\w*)|([^\\`\\~\\!\\@\\#\\%\\^\\&\\*\\(\\)\\-\\=\\+\\[\\{\\]\\}\\\\\\|\\;\\:\\'\\\"\\,\\.\\<\\>\\/\\?\\s]+)",
  "indentationRules": {
    "increaseIndentPattern": "^((?!\\/\\/).)*(\\{[^}\"'`]*|\\([^)\"'`]*|\\[[^\\]\"'`]*)$",
    "decreaseIndentPattern": "^((?!.*?\\/\\*).*\\*/)?\\s*[\\}\\]].*$"
  }
}
```

## 주석 토글
<!--
## Comment toggling-->

VS Code 에서는 주석 토글을 위한 2가지 커맨드를 제공합니다. **Toggle Line Comment** 와 **Toggle Block Comment** 입니다. `comments.blockComment` 와 `comments.lineComment`를 명시하여 VS Code가 줄 / 블록을 주석 처리 하는 방법을 제어 할 수 있습니다. 

<!--
VS Code offers two commands for comment toggling. **Toggle Line Comment** and **Toggle Block Comment**. You can specify `comments.blockComment` and `comments.lineComment` to control how VS Code should comment out lines / blocks.-->

```json
{
  "comments": {
    "lineComment": "//",
    "blockComment": ["/*", "*/"]
  }
}
```

## 괄호 정의

<!-- ## Brackets definition -->

아래의 정의된 괄호로 커서를 이동하면, VS Code는 괄호와 그 쌍을 같이 강조 할 것입니다. 

<!--
When you move the cursor to a bracket defined here, VS Code will highlight that bracket together with its matching pair.
-->

```json
{
  "brackets": [["{", "}"], ["[", "]"], ["(", ")"]]
}
```

그리고, **Go to Bracket** 이나 **Select to Bracket**을 실행하면, VS Code는 위의 정의를 사용하여 가장 가까운 괄호와 그 쌍을 찾을 것입니다.

<!--
Moreover, when you run **Go to Bracket** or **Select to Bracket**, VS Code will use the definition above to find the nearest bracket and its matching pair. -->

## 자동 폐쇄
<!--
## Autoclosing -->

`'`를 입력하면, VS Code는 한쌍의 작은 따옴표 를 생성하고 그 안에 커서를 둘 것입니다: `'|'`. 이번 주제에선 이 쌍을 정의합니다. 

<!--
When you type `'`, VS Code creates a pair of single quotes and puts your cursor in the middle: `'|'`. This section defines such pairs.
-->

```json
{
  "autoClosingPairs": [
    { "open": "{", "close": "}" },
    { "open": "[", "close": "]" },
    { "open": "(", "close": ")" },
    { "open": "'", "close": "'", "notIn": ["string", "comment"] },
    { "open": "\"", "close": "\"", "notIn": ["string"] },
    { "open": "`", "close": "`", "notIn": ["string", "comment"] },
    { "open": "/**", "close": " */", "notIn": ["string"] }
  ]
}
```

`notIn` 키는 특정 범위에 이 기능을 비활성화 시킵니다. 예를 들어 다음과 같은 코드를 작성할때:

<!--
The `notIn` key disables this feature in certain code ranges. For example, when you are writing the following code: -->

```js
// ES6's Template String
`ES6's Template String`;
```

작은 따옴표는 자동으로 폐쇄 되지 않을 것입니다. 
<!--
The single quote will not be autoclosed. -->

사용자는 `editor.autoClosingQuotes`와 `editor.autoClosingBrackets`설정을 통해 이 자동 폐쇄 동작을 조절 할 수 있습니다. 
<!--
Users can tweak the autoclosing behavior with the `editor.autoClosingQuotes` and `editor.autoClosingBrackets` settings.-->

### 자동 폐쇄 이전

<!-- ### Autoclosing before -->

기본적으로, VS Code는 커서 뒤에 공백이 있는 경우에만 쌍을 자동 폐쇄 합니다. 그렇기 때문에 여러분이 `{`를 JSX 코드에 입력하면 자동 폐쇄가 발생 하지 않습니다. 

<!--
By default, VS Code only autocloses pairs if there is whitespace right after the cursor. So when you type `{` in the following JSX code, you would not get autoclose: -->

```js
const Component = () =>
  <div className={>
                  ^ Does not get autoclosed by default
  </div>
```

한편, 아래의 정의는 이 행동을 오버라이드 합니다:

<!--
However, this definition overrides that behavior:-->

```json
{
  "autoCloseBefore": ";:.,=}])>` \n\t"
}
```

이제 `>` 전에 `{`를 입력할경우,  VS Code는 이를 `}`를 자동 폐쇄 할 것입니다. 

<!--Now when you enter `{` right before `>`, VS Code autocloses it with `}`. -->

## 자동 포함
<!--
## Autosurrounding -->

VS Code에서 범위를 선택한다음 여는 괄호를 입력하면, VS Code는 선택된 내용을 괄호쌍에 포함 시킬 것입니다. 이 기능은 자동 포함이라고 불리며, 아래와 같이 특정 언어에 대하여 자동 포함을 정의 할 수 있습니다.

<!--
When you select a range in VS Code and enter an opening bracket, VS Code surrounds the selected content with a pair of brackets. This feature is called Autosurrounding, and here you can define the autosurrounding pairs for a specific language: -->

```json
{
  "surroundingPairs": [
    ["{", "}"],
    ["[", "]"],
    ["(", ")"],
    ["'", "'"],
    ["\"", "\""],
    ["`", "`"]
  ]
}
```

사용자는 이를 `editor.autoSurround` 설정을 통해 조절 할 수 있습니다. 
<!--
Users can tweak the autosurrounding behavior with the `editor.autoSurround` setting. -->

## 접기

<!--## Folding -->

VS Code에는 3가지 종류의 접기가 있습니다:

<!--
In VS Code, there are three kinds of folding: -->

- 들여쓰기 기반 접기: VS Code의 기본 접기 방식입니다. 들여쓰기가 동일한 두 줄에 대하여, 해당 영역을 접을 수 있는 마커가 생성됩니다. 
- 언어 구성 접기: VS Code가 `folding.markers`에 정의된 `start`와 `end` 정규식을 찾으면, 그 내부의 내용을 접을 수 있는 마커를 생성합니다. 아래의 JSON은 `//#region` 과 `//#endregion` 사이의 접는 마커를 생성합니다. 

<!--
- Indentation-based folding: This is VS Code's default folding behavior. When it sees two lines of the same indentation level, it creates a folding marker that allows you to collapse that region.
- Language configuration folding: When VS Code finds both the `start` and `end` regex defined in `folding.markers`, it creates a folding marker enclosing the content inside the pair. The following JSON creates folding markers for `//#region` and `//#endregion`.
-->
```json
{
  "folding": {
    "markers": {
      "start": "^\\s*//\\s*#?region\\b",
      "end": "^\\s*//\\s*#?endregion\\b"
    }
  }
}
```
<!--
- Language server folding: The Language Server responds to the [`textDocument/foldingRange`](https://microsoft.github.io/language-server-protocol/specification#textDocument_foldingRange) request with a list of folding ranges, and VS Code would render the ranges as folding markers. Learn more about the folding support in Language Server Protocol at the [Programmatic Language Feature](/api/language-extensions/programmatic-language-features) topic.
-->

## 단어 패턴

<!-- ## Word Pattern-->

`wordPattern`은 프로그래밍 언어에서 단어로 간주 되는것을 정의합니다. 이 `wordPattern`이 설정 되어있을경우, 코드 제안 기능이 이를 사용하여 단어의 바운더리를 결정합니다. 주의 할것은 이 설정은 에디터의 `editor.wordSepeartors`에 의해 조절되는 단어와 연관된 에디터 커맨드에는 영향을 주지 않습니다. 

<!--
`wordPattern` defines what's considered as a word in the programming language. Code suggestion features will use this setting to determine word boundaries if `wordPattern` is set. Note this setting won't affect word-related editor commands, which are controlled by the editor setting `editor.wordSeparators`. -->

```json
{
  "wordPattern": "(-?\\d*\\.\\d\\w*)|([^\\`\\~\\!\\@\\#\\%\\^\\&\\*\\(\\)\\-\\=\\+\\[\\{\\]\\}\\\\\\|\\;\\:\\'\\\"\\,\\.\\<\\>\\/\\?\\s]+)"
}
```
## 들여쓰기 규칙
<!--
## Indentation Rules-->

`indentationRules` 는 에디터가 현재줄이나 다음줄을 입력, 붙여넣기, 줄이동을 했을때 어떻게 들여쓰기를 할지 정의합니다. 

<!--
`indentationRules` defines how the editor should adjust the indentation of current line or next line when you type, paste, and move lines.-->

```json
{
  "indentationRules": {
    "increaseIndentPattern": "^((?!\\/\\/).)*(\\{[^}\"'`]*|\\([^)\"'`]*|\\[[^\\]\"'`]*)$",
    "decreaseIndentPattern": "^((?!.*?\\/\\*).*\\*/)?\\s*[\\)\\}\\]].*$"
  }
}
```

예를 들어, `if (true) {` 는 `increaseIndentPattern`에 일치하기 때문에, 만약 `kbstyle(Enter)`를 여는 괄호 `{` 이후에 입력한다면, 에디터는 자동적으로 한칸을 들여쓰기 할 것이며, 코드는 아래와 같이 변경됩니다:

<!--
For example, `if (true) {` matches `increaseIndentPattern`, then if you press `kbstyle(Enter)` after the open bracket `{`, the editor will automatically indent once, and your code will end up as: -->

```javascript
if (true) {
	console.log();
```

만약 프로그래밍 언어에 대하여 설정된 들여쓰기 규칙이 없다면, 에디터는 줄이 열린 괄호로 끝나는 경우 들여쓰기를 하고 닫는 괄호를 입력할때 들여쓰기를 닫을 것입니다. 여기서 괄호는 `brackets`에 정의된 것을 말합니다. 

<!-- If there is no indentation rule set for the programming language, the editor will indent when the line ends with an open bracket and outdent when you type a closing bracket. The bracket here is defined by `brackets`. -->

`editor.formatOnPaste`설정은 [`DocumentRangeFormattingEditProvider`](/api/references/vscode-api#DocumentRangeFormattingEditProvider)에 의해 조절되고 자동 들여쓰기에는 영향을 받지 않는다는것에 주의하십시오. 

<!--
Notice that `editor.formatOnPaste` setting is controlled by the [`DocumentRangeFormattingEditProvider`](/api/references/vscode-api#DocumentRangeFormattingEditProvider) and not affected by auto indentation.-->
