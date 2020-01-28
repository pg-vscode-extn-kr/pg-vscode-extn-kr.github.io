---
layout: default
parent: Language Extensions
title: Syntax Highlight Guide
nav_order: 2
description: ""
---

# 구문 강조 가이드

# Syntax Highlight Guide

구문 강조는 Visual Studio Code 에디터에서 표시되는 소스 코드의 색상과 스타일을 결정합니다. 이는 자바스크립트의 `if`나 `for`와 같은 키워드를 다른 문자열이나 주석, 변수 이름과 다르게 채색하는 역할을 합니다. 

<!--
Syntax highlighting determines the color and style of source code displayed in the Visual Studio Code editor. It is responsible for colorizing keywords like `if` or `for` in JavaScript differently than strings and comments and variable names. -->

구문 강조에는 2가지 구성 요소가 있습니다. 

<!-- There are two components to syntax highlighting:-->

- grammar를 이용하여 텍스트를 토큰과 스코프로 나누기
- 그 다음 테마를 이용하여 스코프를 특정 색상, 스타일에 매핑하기

<!--
- Breaking text into a list of tokens and scopes using a grammar
- Then using a theme to map these scopes to specific colors and styles -->

이 문서는 첫번째부분만 다룹니다: 텍스트를 기존의 색상 테마가 채색 할 수 있게 토큰과 스코프로 나누는것. 에디터의 다른 스코프의 스타일을 커스터마이즈 하는것에 대해서는, [색상 테마 가이드](/api/extension-guides/color-theme#syntax-colors)를 참조하십시오.

<!--
This document only discusses the first part: breaking text into tokens and scopes that existing color themes can colorize. For more information about customizing the styling of different scopes in the editor, see the [Color Theme Guide](/api/extension-guides/color-theme#syntax-colors)-->

## TextMate grammars

VS Code는 [TextMate grammars][tm-grammars]를 사용하여 텍스트를 토큰으로 나눕니다. TextMate grammar는 구조화된 [Oniguruma regular expressions](https://macromates.com/manual/en/regular_expressions)의 모음이며, 일반적으로 plist나 JSON으로 작성되어 있습니다. 여러분은 TextMate grammar에 대한 유용한 소개를 [여기](https://www.apeth.com/nonblog/stories/textmatebundle.html)에서 확인 할 수 있으며, 기존의 TextMate grammar를 통해 어떻게 작동하는지 배울 수 있습니다. 

<!--
VS Code uses [TextMate grammars][tm-grammars] to break text into a list of tokens. TextMate grammars are a structured collection of [Oniguruma regular expressions](https://macromates.com/manual/en/regular_expressions) and are typically written as a plist or JSON. You can find a good introduction to TextMate grammars [here](https://www.apeth.com/nonblog/stories/textmatebundle.html), and you can take a look at existing TextMate grammars to learn more about how they work.-->

### 토큰과 스코프

<!--
### Tokens and scopes-->

토큰은 동일한 프로그램 엘리먼트의 일부인 한개 이상의 문자 입니다. 예를 들면 `+`, `*`와 같은 연산자, `myVar`같은 변수이름, `"my string"`같은 문자열이 있습니다. 

<!--
Tokens are one or more characters that are part of the same program element. Example tokens include operators such as `+` and `*`, variable names such as `myVar`, or strings such as `"my string"`.-->

각 토큰은 토큰의 컨텍스트를 지정하는 스코프와 연관이 있습니다. 스코프는 점으로 구분된 식별자 목록으로, 현재 토큰의 컨텍스트를 지정합니다. 자바스크립트의 `+`연산자는 `keyword.operator.arithmetic.js`라는 스코프를 갖습니다.

<!--
Each token is associated with a scope that defines the context of the token. A scope is a dot separated list of identifiers that specify the context of the current token. The `+` operation in JavaScript for example has the scope `keyword.operator.arithmetic.js`. -->

테마는 스코프를 매핑하여 구문 강조에 색상과 스타일을 지정합니다. TextMate는 많은 테마에서 쓰이는 [공통 스코프 목록][tm-grammars]를 제공합니다. 여러분의 grammar가 가능한 많은 부분을 지원하기 위해서, 새롭게 작성하기 보단, 기존의 스코프를 기반으로 작성 하십시오. 

<!--
Themes map scopes to colors and styles to provide syntax highlighting. TextMate provides [list of common scopes][tm-grammars] that many themes target. In order to have your grammar as broadly supported as possible, try to build on existing scopes rather than defining new ones. -->

스코프는 각 토큰이 상위 스코프와 연관 되도록 중첩됩니다. 아래의 예시는 간단한 자바스크립트 합수에서 `+`연산자의 스코프 구조를 보여주기 위해 [스코프 인스펙터](#scope-inspector)를 사용한 것입다. 이 때 가장 구체적인 스코프는 위쪽에, 일반적인 스코프는 아래쪽에 위치합니다. 

<!--
Scopes nest so that each token is also associated with a list of parent scopes. The example below uses the [scope inspector](#scope-inspector) to show the scope hierarchy for the `+` operator in a simple JavaScript function. The most specific scope is listed at the top, with more general parent scopes listed below:-->

![syntax highlighting scopes](images/syntax-highlighting/scopes.png)

부모 스코프 정보 또한 테마에 쓰입니다. 테마가 스코프를 타겟할때 따로 색상이 지정되지 않은 모든 토큰은 부모 스코프에 따라 색상이 지정됩니다.

<!--
Parent scope information is also used for theming. When a theme targets a scope, all tokens with that parent scope will be colorized unless the theme also provides a more specific colorization for their individual scopes. -->

### 기본 grammar 작성

<!--
### Contributing a basic grammar -->

VS Code는 json TextMate grammar를 지원합니다. 이는 `grammars` [contribution point](/api/references/contribution-points)를 통해 작성 될 수 있습니다.

<!--
VS Code supports json TextMate grammars. These are contributed through the `grammars` [contribution point](/api/references/contribution-points).-->

각 grammar 작성은 : grammar가 적용될 언어 식별자, grammar의 토큰의 최상위 스코프 이름, grammar 파일의 상대 경로를 명시합니다. 아래의 예시는 가상의 `abc`언어에 대한 grammar 작성을 표시합니다. 

<!--
Each grammar contribution specifies: the identifier of the language the grammar applies to, the top level scope name for the tokens of the grammar, and the relative path to a grammar file. The example below shows a grammar contribution for a fictional `abc` language:-->

```json
{
  "contributes": {
    "languages": [
      {
        "id": "abc",
        "extensions": [".abc"]
      }
    ],
    "grammars": [
      {
        "language": "abc",
        "scopeName": "source.abc",
        "path": "./syntaxes/abc.tmGrammar.json"
      }
    ]
  }
}
```

Grammar파일 자체는 최상위 레벨 규칙으로 구성되어 있습니다. 이는 일반적으로 프로그램의 최상위 레벨 엘리먼트의 목록인 `patterns`와 각 엘리먼트를 정의하는 `repository`로 구성됩니다. Grammar의 다른 규칙은 `repository`에서 `{"include":"#id"}`를 사용하여 엘리먼트를 참조 할 수 있습니다.

<!--
The grammar file itself consists of a top level rule. This is typically split into a `patterns` section that lists the top level elements of the program and a `repository` that defines each of the elements. Other rules in the grammar can reference elements from the `repository` using `{ "include": "#id" }`.-->

예시 `abc` grammar는 `a`,`b` 그리고 `c`를 keyword로 그리고 parens의 중첩을 expression으로 표기합니다. 

<!-- The example `abc` grammar marks the letters `a`, `b`, and `c` as keywords, and nestings of parens as expressions.-->

```json
{
  "scopeName": "source.abc",
  "patterns": [{ "include": "#expression" }],
  "repository": {
    "expression": {
      "patterns": [{ "include": "#letter" }, { "include": "#paren-expression" }]
    },
    "letter": {
      "match": "a|b|c",
      "name": "keyword.letter"
    },
    "paren-expression": {
      "begin": "\\(",
      "end": "\\)",
      "beginCaptures": {
        "0": { "name": "punctuation.paren.open" }
      },
      "endCaptures": {
        "0": { "name": "punctuation.paren.close" }
      },
      "name": "expression.group",
      "patterns": [{ "include": "#expression" }]
    }
  }
}
```

아래의 간단한 프로그램 처럼, Grammar 엔진은 `expression`규칙을 문서의 모든 텍스트에 적용하려 시도 할 것입니다 : 

<!--The grammar engine will try to successively apply the `expression` rule to all text in the document. For a simple program such as:-->

```
a
(
    b
)
x
(
    (
        c
        xyz
    )
)
(
a
```

예시 grammar는 다음과 같은 스코프를 생성합니다(구체적인것에서 일반적인것으로 좌우로 나열):

<!--
The example grammar produces the following scopes (listed left-to-right from most specific to least specific scope):-->

```
a               keyword.letter, source.abc
(               punctuation.paren.open, expression.group, source.abc
    b           keyword.letter, expression.group, source.abc
)               punctuation.paren.close, expression.group, source.abc
x               source.abc
(               punctuation.paren.open, expression.group, source.abc
    (           punctuation.paren.open, expression.group, expression.group, source.abc
        c       keyword.letter, expression.group, expression.group, source.abc
        xyz     expression.group, expression.group, source.abc
    )           punctuation.paren.close, expression.group, expression.group, source.abc
)               punctuation.paren.close, expression.group, source.abc
(               source.abc
a               keyword.letter, source.abc
```

문자열 `xyz`처럼 규칙과 일치 하지 않는 텍스트는 현재 스코프에 포함되는 것을 유념하십시오. 파일 끝의 마지막 paren은 `end` 규칙에 맞지 않기 때문에 `expression.group` 의 부분이 되지 않습니다.

<!--
Note that text that is not matched by one of the rules, such as the string `xyz`, is included in the current scope. The last paren at the end of the file is not part of the an `expression.group` since the `end` rule is not matched. -->

### 임베디드 언어
<!--
### Embedded languages -->

HTML 안의 CSS 스타일 블록처럼, grammar가 부모 언어 내의 임베디드 언어를 포함한다면 `embeddedLanguages` contribution point를 사용하여 VS Code가 임베디드 언어를 부모 언어와 다르게 처리 할 수 있도록 하십시오. 이는 대괄호 매칭, 주석, 그리고 임베디드 언어에서 기대되는 기본 언어 기능들을 제공합니다.

<!--
If your grammar include embedded languages within the parent language, such as CSS style blocks in HTML, you can use the `embeddedLanguages` contribution point to tell VS Code to treat the embedded language as distinct from the parent language. This ensures that bracket matching, commenting, and other basic language features work as expected in the embedded language. -->

`embeddedlanguages` contribution point는 임베디드 언어의 스코프를 최상위 레벨 언어 스코프로 매핑합니다. 아래의 예시에서, `meta.embedded.block.javascript` 스코프의 모든 토큰은 자바스크립트 주석으로써 처리 될 것입니다.

<!--
The `embeddedLanguages` contribution point maps a scope in the embedded language to a top level language scope. In the example below, any tokens in the `meta.embedded.block.javascript` scope will be treated as javascript content: -->

```json
{
  "contributes": {
    "grammars": [
      {
        "path": "./syntaxes/abc.tmLanguage.json",
        "scopeName": "source.abc",
        "embeddedLanguages": {
          "meta.embedded.block.javascript": "javascript"
        }
      }
    ]
  }
}
```

이제 `meta.embedded.block.javascript` 로 표기된 토큰 내부에서 주석을 달거나 snippet을 시도 할 경우, 올바른 자바스크립트 스타일 주석 `//` 과 snippet을 갖게 될 것입니다.

<!--
Now if you try to comment code or trigger snippets inside an set of tokens marked `meta.embedded.block.javascript`, they will get the correct `//` JavaScript style comment and the correct JavaScript snippets. -->

## 새 grammar 익스텐션 개발

<!--
## Developing a new grammar extension -->

빠르게 새 grammar 익스텐션을 생성하기 위해, [VS Code's Yeoman templates](/api/get-started/your-first-extension)를 사용하여 `yo code`를 실행하고 `New Language`를 선택하십시오:

<!--
To quickly create a new grammar extension, use [VS Code's Yeoman templates](/api/get-started/your-first-extension) to run `yo code` and select the `New Language` option: -->

![Selecting the 'new language' template in 'yo code'](images/syntax-highlighting/yo-new-language.png)

Yeoman은 여러분에게 몇가지 기초 질문을 통해 새 익스텐션 생성을 도울 것입니다. 새 grammar를 생성하기 위한 중요한 질문은 아래와 같습니다:
<!--
Yeoman will walk you through some basic questions to scaffold the new extension. The important questions for creating a new grammar are:-->

- `Language Id` - 언어의 특수한 식별자.
- `Language Name` - 사람이 읽을 수 있는 언어의 이름.
- `Scope names` - Grammar의 루트 TextMate 스코프

<!--
- `Language Id` - A unique identifier for your language.
- `Language Name` - A human readable name for your language.
- `Scope names` - Root TextMate scope name for your grammar
-->

![Filling in the 'new language' questions](images/syntax-highlighting/yo-new-language-questions.png)

생성기는 여러분이 새 언어와 기존 언어의 새 grammar를 작성하는 것으로 간주 합니다. 만약 기존의 언어에 대해 grammar를 생성하는 경우, 이를 목표 언어의 정보에 채워넣고 생성된 `language` contribution point를 삭제하십시오. 

<!--The generator assumes that you want to define both a new language and a new grammar for that language. If you are creating a grammar for an existing language, just fill these in with your target language's information and be sure to delete the `languages` contribution point in the generated `package.json`.-->

모든 질문에 답변을 하고나면, Yeoman은 구조를 가진 새 익스텐션을 생성 할 것입니다:

<!--
After answering all the questions, Yeoman will create a new extension with the structure: -->

![A new language extension](images/syntax-highlighting/generated-new-language-extension.png)

VS Code가 이미 알고 있는 언어의 grammar를 작성하려는 경우, `package.json`의 `languages` contribution point를 삭제하는 것을 잊지 마십시오. 

<!-- Remember, if you are contributing a grammar to a language that VS Code already knows about, be sure to delete the `languages` contribution point in the generated `package.json`. -->

### 기존 TextMate grammar 변형
<!--
### Converting an existing TextMate grammar -->

`yo code`는 기존 TextMage grammar를 VS Code 익스텐션으로 변환할때도 쓰입니다. 다시, `yo code`를 실행하고 `Language extension`을 선택하십시오. 기존 grammar 파일을 요청 할 경우 `.tmLanguage`나 `.json` TextMate grammar 파일의 전체 경로를 제공 하십시오. 


<!-- `yo code` can also help convert an existing TextMate grammar to a VS Code extension. Again, start by running `yo code` and selecting `Language extension`. When asked for an existing grammar file, give it the full path to either a `.tmLanguage` or `.json` TextMate grammar file: -->

![Converting an existing TextMate grammar](images/syntax-highlighting/yo-convert.png)

### YAML로 Grammar 작성
<!--
### Using YAML to write a grammar -->

Grammar가 복잡해질수록, 이해하기와 json으로 유지하는 것은 어려워집니다. 만약 복잡한 정규표현식을 작성하거나 grammar에 설명을 하는 주석을 달아야 한다면, yaml을 사용하는 방안을 고려해보십시오. 

<!--
As a grammar grows more complex, it can become difficult to understand and maintain it as json. If you find yourself writing complex regular expressions or needing to add comments to explain aspects of the grammar, consider using yaml to define your grammar instead. -->

Yaml grammar는 json 기반 grammar와 동일한 구조를 갖지만 대신 yaml의 더 간결한 기능, 여러줄 문자열 이나 주석등을 사용 할 수 있게 합니다. 

<!--
Yaml grammars have the exact same structure as a json based grammar but allow you to use yaml's more concise syntax, along with features such as multi-line strings and comments. -->

![A yaml grammar using multiline strings and comments](images/syntax-highlighting/yaml-grammar.png)

VS Code는 json grammar만 불러 올 수 있기 때문에, yaml 기반 grammar는 json으로 변환 되어야 합니다. 이는 [`js-yaml` package](https://www.npmjs.com/package/js-yaml)와 커맨드 라인 도구를 이용하면 쉽게 수행 할 수 있습니다. 

<!--
VS Code can only load json grammars, so yaml based grammars must be converted to json. The [`js-yaml` package](https://www.npmjs.com/package/js-yaml) and command-line tool makes this easy. -->

```bash
# Install js-yaml as a development only dependency in your extension
$ npm install js-yaml --save-dev

# Use the command-line tool to convert the yaml grammar to json
$ npx js-yaml syntaxes/abc.tmLanguage.yaml > syntaxes/abc.tmLanguage.json
```

### 스코프 인스펙터
<!--
### Scope inspector -->

VS Code의 빌트인 스코프 인스펙터 도구는 Grammar를 디버그 할 수 있게 돕습니다. 이는 파일의 현재 위치에 있는 토큰의 스코프를 보여주며, 해당 토큰에 적용 되는 테마 규칙에 대한 메타 데이터를 보여줍니다.

<!--
VS Code's built-in scope inspector tool helps debug grammars. It displays the scopes for the token at the current position in a file, along with metadata about which theme rules apply to that token. -->

Command Palette에서 `Developer: Inspect TM Scores` 커맨드 나 [키바인딩 생성](/docs/getstarted/keybindings)을 통해 스코프 인스펙터를 실행 하십시오. 

<!--
Trigger the scope inspector from the Command Palette with the `Developer: Inspect TM Scopes` command or [create a keybinding](/docs/getstarted/keybindings) for it: -->

```json
{
  "key": "cmd+alt+shift+i",
  "command": "editor.action.inspectTMScopes"
}
```

![scope inspector](images/syntax-highlighting/scope-inspector.png)

스코프 인스펙터는 다음 정보를 보여줍니다:

<!--The scope inspector displays the following information:-->


1. 현재 토큰.
1. 토큰의 메타데이터와 계산된 모양. 임베디드 언어의 경우 `language`와 `token type`이 중요합니다. 
1. 토큰에 적용된 테마 규칙들. 토큰에 적용된 현재 스타일 테마 규칙만 보여주며 오버라이드 된 규칙은 보여주지 않습니다. 
1. 완전한 스코프 목록, 가장 구체적인것은 최상단에 위치합니다.

<!--
1. The current token.
1. Metadata about the token and information about its computed appearance. If you are working with embedded languages, the important entries here `language` and `token type`.
1. Theme rules that apply to the token. This only shows the theme rules that are responsible for the token's current style, it does not show overridden rules.
1. Complete scope list, with the most specific scope at the top. -->

## Injection grammars

Injection grammars 는 기존 Grammar를 확장하게 합니다. 이는 일반 TextMate grammar로 기존 grammar에서 특정 스코프에 추가 됩니다. Injection grammar의 예시 역할는 아래와 같습니다:

<!--
Injection grammars let you extend an existing grammar. An injection grammar is a regular TextMate grammar that is injected into a specific scope within an existing grammar. Example applications of injection grammars: -->

- 주석내의 `TODO`와 같은 키워드 강조.
- 기존 grammar에 특정 스코프 정보 추가.
- 새 언어 Markdown 코드 블록에 대해 강조 추가.

<!--
- Highlighting keywords such as `TODO` in comments.
- Add more specific scope information to an existing grammar.
- Adding highlighting for a new language to Markdown fenced code blocks. -->

### 기본 injection grammar 생성

<!--
### Creating a basic injection grammar -->

Injection grammar는 일반 grammar와 같이 `package.json`에 작성 됩니다. 그러나 `language`에 명시되지 않고 `injectTo`를 사용하여 목표 언어 스코프목록에 추가 될 지를 명시합니다. 

<!--
Injection grammars are contributed though the `package.json` just like regular grammars. However, instead of specifying a `language`, an injection grammar uses `injectTo` to specify a list of target language scopes to inject the grammar into.-->

아래 예시의 경우, 자바스크립트의 `TODO` 키워드를 강조하는 단순한 injection grammar를 만들것 입니다. 자바스크립트 파일에서 이를 적용 하기위해, `source.js` 를 `injectTo`의 타겟 언어 스코프로 사용하십시오:
<!--
For this example, we'll create a very simple injection grammar that highlights `TODO` as a keyword in javascript comments. To apply our injection grammar in javascript files, we use the `source.js` target language scope in `injectTo`:-->

```json
{
  "contributes": {
    "grammars": [
      {
        "path": "./syntaxes/injection.json",
        "scopeName": "todo-comment.injection",
        "injectTo": ["source.js"]
      }
    ]
  }
}
```

Grammar자체는 최상위 `injectionSelector`를 제외하면 표준 TextMate grammar입니다. `injectionSelector`는 스코프 선택기로 어느 범위에 injected grammar가 적용 될지를 선택합니다. 예시에선 모든 `//` 주석에 `TODO`단어를 강조하는것이 목표입니다. [스코프 인스펙터](#scope-inspector) 를 사용하여, 자바스크립트의 이중 슬래시 주석이 `comment.line.double-slash`라는 스코프를 가진다는것을 알 수 있으니 우리의 injection 선택기는 `L:comment.line.double-slash`입니다:

<!--
The grammar itself is a standard TextMate grammar except for the top level `injectionSelector` entry. The `injectionSelector` is a scope selector that specifies which scopes the injected grammar should be applied in. For our example, we want to highlight the word `TODO` in all `//` comments. Using the [scope inspector](#scope-inspector), we find that JavaScript's double slash comments have the scope `comment.line.double-slash`, so our injection selector is `L:comment.line.double-slash`: -->

The grammar itself is a standard TextMate grammar except for the top level `injectionSelector` entry. The `injectionSelector` is a scope selector that specifies which scopes the injected grammar should be applied in. For our example, we want to highlight the word `TODO` in all `//` comments. Using the [scope inspector](#scope-inspector), we find that JavaScript's double slash comments have the scope `comment.line.double-slash`, so our injection selector is `L:comment.line.double-slash`:

```json
{
  "scopeName": "todo-comment.injection",
  "injectionSelector": "L:comment.line.double-slash",
  "patterns": [
    {
      "include": "#todo-keyword"
    }
  ],
  "repository": {
    "todo-keyword": {
      "match": "TODO",
      "name": "keyword.todo"
    }
  }
}
```

injection 선택기의 `L:` 은 injection이 기존 grammar 규칙의 왼쪽에 추가 된다는 의미입니다. 이는 우리의 injected grammar 규칙이 기존 grammar 규칙보다 먼저 적용 됨을 의미합니다.

<!--
The `L:` in the injection selector means that the injection is added to the left of existing grammar rules. This basically means that our injected grammar's rules will be applied before any existing grammar rules. -->

### 임베디드 언어

<!--
### Embedded languages -->

Injection grammar는 부모 grammar에 임베디드 언어를 제공 할 수 있게 합니다. 일반 grammar와 마찬가지로 `embeddedLanguages`를 사용하여 임베디드 언어의 스코프를 최상위 레벨 언어의 스코프로 매핑하십시오.

<!--
Injection grammars can also contribute embedded languages to their parent grammar. Just like with a normal grammar, an injection grammars can use `embeddedLanguages` to map scopes from the embedded language to a top level language scope. -->

자바스크립트 문자열에서 SQL 쿼리를 강조하는 익스텐션은 `embeddedLanguages`를 사용하여 `meta.embedded.inline.sql`로 표기된 문자열 내부의 모든 토큰이 대괄호 일치나 snippet 선택 같은 기본 언어 기능을 sql로 처리 할 수 있게 합니다. 

<!--
An extension that highlights sql queries in javascript strings for example may use `embeddedLanguages` to make sure all token inside the string marked `meta.embedded.inline.sql` are treated as sql for basic language features such as bracket matching and snippet selection.-->

```json
{
  "contributes": {
    "grammars": [
      {
        "path": "./syntaxes/injection.json",
        "scopeName": "sql-string.injection",
        "injectTo": ["source.js"],
        "embeddedLanguages": {
          "meta.embedded.inline.sql": "sql"
        }
      }
    ]
  }
}
```

### 토큰 타입과 임베디드 언어

<!--
### Token types and embedded languages -->

inejction 임베디드 언어에 대하여 한가지 추가로 알아야 할 점은: VS Code는 문자열 내의 모든 토큰을 문자열 컨텐츠로 취급하고 주석의 모든 토큰을 토큰으로 취급합니다. 대괄호 매칭 같은 기능과 자동 닫기 등이 문자열과 주석내에선 사용 불가능하기 때문에, 만약 임베디드 언어가 문자열이나 주석내에 나타난다면 이러한 기능 또한 임베디드 언어에서는 비활성화될 것입니다. 

<!--
There is one additional complication for injection languages embedded languages: by default, VS Code treats all tokens within a string as string contents and all tokens with a comment as token content. Since features such as bracket matching and auto closing pairs are disabled inside of strings and comments, if the embedded language appears inside a string or comment, these features will also be disabled in the embedded language.-->

이 규칙을 오버라이드 하기 이ㅜ해, `meta.embedded.*` 스코프를 사용하여 VS Code의 문자령이나 주석 컨텐츠에 대한 토큰 마킹을 다시 설정하십시오. VS Code가 임베디드 언어를 적절히 다루기 위해 `meta.embedded.*`스코프 내부에 임베디드 언어를 래핑하는것은 좋은 선택입니다. 

<!--
To override this behavior, you can use a `meta.embedded.*` scope to reset VS Code's marking of tokens as string or comment content. It is a good idea to always wrap embedded language in a `meta.embedded.*` scope to make sure VS Code treats the embedded language properly.-->

만약 `meat.embedded.*` 스코프를 grammar에 더할 수 없다면 대신 grammar의 `tokenTypes` contribution point를 사용하여 특정 스코프를 컨텐츠에 매핑 할 수 있습니다. 아래의 `tokenTypes` 주제는 `my.sql.template.string`스코프의 모든 컨텐츠가 소스 코드로 취급 되는 것을 보여 줍니다. 

<!--
If you can't add a `meta.embedded.*` scope to your grammar, you can alternatively use `tokenTypes` in the grammar's contribution point to map specific scopes to content mode. The `tokenTypes` section below ensures that any content in the `my.sql.template.string` scope is treated as source code: -->

```json
{
  "contributes": {
    "grammars": [
      {
        "path": "./syntaxes/injection.json",
        "scopeName": "sql-string.injection",
        "injectTo": ["source.js"],
        "embeddedLanguages": {
          "my.sql.template.string": "sql"
        },
        "tokenTypes": {
          "my.sql.template.string": "other"
        }
      }
    ]
  }
}
```

[tm-grammars]: https://macromates.com/manual/en/language_grammars
