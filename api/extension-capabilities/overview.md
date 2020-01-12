---
layout: default
parent: Extension Capabilities
title: Overview
nav_order: 1
description: ""
---
<!--
# Extensions Capabilities Overview

Visual Studio Code offers many ways for extensions to extend its capabilities. It can sometimes be hard to find the right [Contribution Points](/api/references/contribution-points) and [VS Code API](/api/references/vscode-api) to use. This topic splits extension capabilities into a few categories. Each category describes:

- Some functionalities your extension could use
- Links to more detailed topics for using these functionalities
- A few extension ideas

However, we also impose [restrictions](#restrictions) upon extensions to ensure the stability and performance of VS Code. For example, extensions cannot access the DOM of VS Code UI.

-->

# 익스텐션 기능 개요

비주얼 스튜디오 코드에서는 익스텐션의 기능을 확장할 수 있는 다양한 방법을 제공합니다. 때떄로 사용하기에 적절한 [Contribution Points](/api/references/contribution-points)나 [VS Code API](/api/references/vscode-api)를 찾지 못하는 경우도 있습니다. 이번 섹션에서는 익스텐션 기능을 몇 가지 카테고리로 나누어 설명합니다. 각 카테고리는 다음과 같은 내용을 포함하고 있습니다.

- 당신의 익스텐션에서 이용 가능한 몇 가지 기능들
- 이 기능들을 이용하기 위한 더 자세한 설명으로의 링크
- 몇 가지 익스텐션 아이디어

또한 이 섹션은 VS Code의 안정성과 성능을 위해 [제한](#restrictions)된 부분에 대해서도 설명합니다. 예를 들어, 익스텐션은 VS Code UI의 DOM에 접근할 수 없습니다. 

<!--
## Common Capabilities

[Common Capabilities](./common-capabilities) are core pieces of functionality that you can use in any extension.

Some of these capabilities include:

- Registering commands, configurations, keybindings, or context menu items.
- Storing workspace or global data.
- Displaying notification messages.
- Using Quick Pick to collect user input.
- Open the system file picker to let users select files or folders.
- Use the Progress API to indicate long-running operations.

-->

## 기본 기능

[기본 기능](./common-capabilities)은 어떤 익스텐션에서도 이용할 수 있는 핵심 기능들입니다. 

여기에는 다음과 같은 기능들이 있습니다.

- Command 등록, Configurations, keybindings, 그리고 context menu items
- 전역 데이터나 작업 공간을 저장하기
- 알림 메세지
- 사용자 입력을 받는 빠른 선택 기능
- 파일창을 열어 사용자가 파일이나 폴더를 선택할 수 있게 함
- 지속적인 명령을 나타내기 위한 Progress API

<!--
## Theming

[Theming](./theming) controls the look of VS Code, both the colors of source code in the editor and the colors of the VS Code UI. If you've ever wanted to make it look like you're coding the Matrix by making VS Code different shades of green, or just wanted to create the ultimate, minimalist grayscale workspace, then themes are for you.

**Extension Ideas**

- Change colors of your source code.
- Change colors of the VS Code UI.
- Port an existing TextMate theme to VS Code.
- Add custom file icons.

-->

## 테마

[테마](./theming)는 VS Code UI와 소스 코드의 색상을 결정해 VS Code의 모습을 바꿔줍니다. 당신이 VS Code를 행렬처럼 보이게 하기 위해 UI를 다양한 밝기의 초록색으로 만들고 싶었거나, 그저 궁극적인 단순함을 추구하는  회색 작업 공간을 만들고 싶었다면, 테마 기능은 당신을 위해 준비되어 있습니다.

**확장 아이디어**

- 소스 코드의 색상 변경
- VS Code UI 색상 변경
- 이미 있는 TextMate 테마를 VS Code로 포팅
- 사용자화된 새로운 파일 아이콘 추가


<!--

## Declarative Language Features

[Declarative Language Features](/api/language-extensions/overview#declarative-language-features) adds basic text editing support for a programming language such as bracket matching, auto-indentation and syntax highlighting. This is done declaratively, without writing any code. For more advanced language features, like IntelliSense or debugging, see [Programmatic Language Features](#programmatic-language-features).

**Extension Ideas**

- Bundle common JavaScript snippets into an extension.
- Tell VS Code about a new programming language.
- Add or replace the grammar for a programming language.
- Extend an existing grammar with grammar injections.
- Port an existing TextMate grammar to VS Code.

-->

<!-- Features를 "지원"으로 번역. 문맥상 그게 맞아 보임 -->

## 선언적 언어 지원

[선언적 언어 지원](/api/language-extensions/overview#declarative-language-features)에서는 괄호 매칭, 자동 들여쓰기, 문법 강조와 같은 프로그래밍 언어를 위한 기본적인 텍스트 편집 지원 기능을 제공합니다. 이들은 코드를 작성하지 않고도 선언적으로 실행됩니다. Intellisense나 디버깅과 같이 더 향상된 언어 지원을 원한다면, [프로그래밍적 언어 지원](/api/language-extensions/overview#programmatic-language-features)을 참고하십시오. 

**확장 아이디어**

- 많이 쓰이는 자바스크립트 코드 조각을 익스텐션에 번들로 추가합니다.
- VS Code에게 새로운 프로그래밍 언어에 대해 알립니다.
- 프로그래밍 언어의 문법을 추가하거나 변경합니다. 
- 이미 있는 문법에 새로운 요소를 추가하거나 이를 확장합니다. 
- 이미 있는 TextMate 문법을 VS Code에 포팅합니다. 


<!--
## Programmatic Language Features

[Programmatic Language Features](/api/language-extensions/overview#programmatic-language-features) add rich programming language support such as Hovers, Go to Definition, diagnostic errors, IntelliSense and CodeLens. These language features are exposed through the [`vscode.languages.*`](/api/references/vscode-api#languages) API. An extension can either use these API directly, or write a Language Server and adapt it to VS Code using the VS Code [Language Server library](https://github.com/Microsoft/vscode-languageserver-node).

Although we provide a listing of [language features](/api/language-extensions/programmatic-language-features) and their intended usage, nothing prevents you from using these API creatively. For example, CodeLens and Hovers are a great way to present additional information inline, while diagnostic errors can be used to highlight spelling or code style errors.

**Extension Ideas**

- Add hovers that show sample usage of an API.
- Report spelling or linter errors in source code using diagnostics.
- Register a new code formatter for HTML.
- Provide rich, context-aware IntelliSense.
- Add folding, breadcrumbs and outline support for a language.


-->


## 프로그래밍적 언어 지원

[프로그래밍적 언어 지원](/api/language-extensions/overview#programmatic-language-features)에서는 호버, 정의로 향하기, 에러 진단, IntelliSense나 CodeLen과 같은 풍부한 프로그래밍 관련 지원을 제공합니다. 이러한 지원들은 [`vscode.languages.*`](/api/references/vscode-api#languages) API를 통해 확인 가능합니다. 익스텐션은 이런 API를 직접적으로 사용하거나, Language Server를 작성한 후 VS Code [Language Server library](https://github.com/Microsoft/vscode-languageserver-node)를 이용해 이를 VS Code와 연결할 수도 있습니다. 

우리는 [언어 지원](/api/language-extensions/programmatic-language-features)의 한정된 목록과 의도한 사용법만을 제공하지만, 이를 창의적으로 사용하는 데에는 아무런 제약이 없습니다. 예를 들어, CodeLen이나 Hover는 추가적인 정보를 즉시 보여줄 수 있는 좋은 방법이고, 에러 진단의 경우 철자나 코드 스타일 에러를 강조하는 데 쓰일 수 있습니다. 

**확장 아이디어**

- API의 사용 방법 예시를 보여주는 호버를 추가.
- 진단을 활용해 철자나 linter 에러를 보고함. 
- HTML을 위한 새로운 코드 정리 기능을 등록.
- 풍부하고 문맥에 민감한 IntelliSense를 제공.
- 코드 접기나 특정 위치로 이동하기와 같은 추가적인 지원 제공.


## Workbench Extensions

[Workbench Extensions](./extending-workbench) extend the VS Code Workbench UI. Add new right-click actions to the File Explorer, or even build a custom explorer using VS Code's [TreeView](/api/extension-guides/tree-view) API. And if your extension needs a fully customized user interface, use the [Webview API](/api/extension-guides/webview) to build your own document preview or UI using standard HTML, CSS, and JavaScript.

**Extension Ideas**

- Add custom context menu actions to the File Explorer.
- Create a new, interactive TreeView in the Side Bar.
- Define a new Activity Bar view.
- Show new information in the Status Bar.
- Render custom content using the `WebView` API.
- Contribute Source Control providers.

## Debugging

You can take advantage of VS Code's [Debugging](/docs/editor/debugging) functionality by writing [Debugger Extensions](/api/extension-guides/debugger-extension) that connect VS Code's debugging UI to a specific debugger or runtime.

**Extension Ideas**

- Connect VS Code's debugging UI to a debugger or runtime by contributing a [Debug Adapter implementation](https://microsoft.github.io/debug-adapter-protocol/implementors/adapters/).
- Specify the languages supported by a debugger extension.
- Provide rich IntelliSense and hover information for the debug configuration attributes used by the debugger.
- Provide debug configuration snippets.

On the other hand, VS Code also offers a set of [Debug Extension API](/api/references/vscode-api#debug), with which you can implement debug-related functionality on top of any VS Code debugger, in order to automate users' debugging experience.

**Extension Ideas**

- Start debug sessions based on dynamically created debug configurations.
- Track the lifecycle of debug sessions.
- Create and manage breakpoints programmatically.

<!-- Add below content back after writing ./extending-core-functionalities.md  -->
<!-- ## Core Extensions

[Core Extensions](extending-core-functionalities) are for very advanced users. These let you build a custom back end for many of VS Code's low-level functionality. For example, the `FileSystem` API can be used to support working with files over FTP or other protocols. Core extensions typically work transparently from a user's point of view.

**Extension Ideas**

- Add support for working with remote files over FTP or SFTP.
- Register new source control provider, such as Mercurial.
- Implement a custom file search provider. -->

## Restrictions

There are certain restrictions we impose upon extensions. Here are the restrictions and their purposes.

### No DOM Access

Extensions have no access to the DOM of VS Code UI. You **cannot** write an extension that applies custom CSS to VS Code or adds an HTML element to VS Code UI.

At VS Code, we're continually trying to optimize use of the underlying web technologies to deliver an always available, highly responsive editor and we will continue to tune our use of the DOM as these technologies and our product evolve. To ensure that extensions cannot interfere with the stability and performance of VS Code, and that we can continue to improve the DOM of VS Code without breaking existing extensions, we run extensions in an [Extension Host](/api/advanced-topics/extension-host) process and prevent direct access to the DOM.
