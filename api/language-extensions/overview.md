---
layout: default
parent: Language Extensions
title: Overview
nav_order: 1
description: ""
---

# 언어 익스텐션 오버뷰 

<!--
# Language Extensions Overview -->

Visual Studio Code는 언어 익스텐션을 통해 다른 프로그래밍 언어에 대해서 스마트한 수정 기능을 제공합니다. VS Code는 빌트인 언어지원은 없지만 대신 다양한 언어 기능을 가능하게 하는 API들을 제공합니다. 예를 들어 번들된 [HTML](https://github.com/Microsoft/vscode/tree/master/extensions/html)익스텐션 은 VS Code가 HTML 파일에 대해서 구문을 강조하여 보여지게 합니다. 비슷하게, 여러분이 `console.`을 입력하면 IntelliSense에 `log`가 표시되며 이는 [타입스크립트 언어 기능](https://github.com/Microsoft/vscode/tree/master/extensions/typescript-language-features) 익스텐션이 작동 했기 때문입니다. 

<!--
Visual Studio Code provides smart editing features for different programming languages through Language Extensions. VS Code doesn't provide built-in language support but offers a set of APIs that enable rich language features. For example, it is a bundled [HTML](https://github.com/Microsoft/vscode/tree/master/extensions/html) extension that allows VS Code to show syntax highlighting for HTML files. Similarly, when you type `console.` and `log` shows up in IntelliSense, it is the [Typescript Language Features](https://github.com/Microsoft/vscode/tree/master/extensions/typescript-language-features) extension at work.-->

언어 기능은 크게 2가지 분류로 나눠질 수 있습니다:
<!--Language features can be roughly put into two categories:-->

## 선언적 언어 기능
<!--
## Declarative language features -->

선언적 언어 기능은 구성 파일에서 정의됩니다. [html](https://github.com/Microsoft/vscode/tree/master/extensions/html), [css](https://github.com/Microsoft/vscode/tree/master/extensions/css) 그리고 [typescript-basic](https://github.com/Microsoft/vscode/tree/master/extensions/typescript-basics) 예시를 포함한 익스텐션이 VS Code에 포함되어 있으며, 다음과 같은 선언적 언어 기능의 하위 기능들을 제공합니다. 

<!--Declarative language features are defined in configuration files. Examples include [html](https://github.com/Microsoft/vscode/tree/master/extensions/html), [css](https://github.com/Microsoft/vscode/tree/master/extensions/css) and [typescript-basic](https://github.com/Microsoft/vscode/tree/master/extensions/typescript-basics) extensions bundled with VS Code, which offer a subset of the following Declarative Language Features: -->

- 구문 강조
- Snippet 완성
- 중괄호 연결
- 중괄호 자동 폐쇄
- 중괄호 자동 포함
- 주석 전환
- 자동 들여쓰기
- 접기 ( 마커에 의해 )

<!--
- Syntax highlighting
- Snippet completion
- Bracket matching
- Bracket autoclosing
- Bracket autosurrounding
- Comment toggling
- Auto indentation
- Folding (by markers)
-->

다음은 선언적 언어 기능을 제공하는 언어 익스텐션 작성을 위한 3가지 가이드입니다.

<!--
We have three guides for writing Language Extensions that provide Declarative Language Features.-->

- [구문 강조 가이드](/api/language-extensions/syntax-highlight-guide): VS Code는 구문 강조를 위해 TextMate grammer를 사용합니다. 이 가이드는 여러분이 간단한 TextMate grammer를 작성하고,  VS Code 익스텐션으로 바꿀 수 있게 도울 것입니다. 
- [Snippet 완성 가이드](/api/language-extensions/snippet-guide): 이 가이드는 snippet들을 익스텐션에 번들링 하는 방법을 설명 할 것입니다. 
- [언어 구성 가이드](/api/language-extensions/language-configuration-guide): VS Code는 익스텐션이 어떤 프로그래밍 언어던지 **언어 구성**을 정의 할 수 있게 합니다. 이 파일은 주석 전환, 중괄호 연산, 부분 접기(레거시) 와 같은 기본 편집 기능을 조절합니다. 

<!--
- [Syntax Highlight Guide](/api/language-extensions/syntax-highlight-guide): VS Code uses TextMate grammar for syntax highlighting. This guide will walk you through writing a simple TextMate grammar and converting it into a VS Code extension.
- [Snippet Completion Guide](/api/language-extensions/snippet-guide): This guide explains how to bundle a set of snippets into an extension.
- [Language Configuration Guide](/api/language-extensions/language-configuration-guide): VS Code allows extensions to define a **language configuration** for any programming language. This file controls basic editing features such as comment toggling, bracket matching/surrounding and region folding (legacy). -->

## 프로그래밍 언어 기능

<!--
## Programmatic language features -->

프로그래밍 언어 기능은 자동 완성, 에러 확인, 정의로 이동을 포함합니다. 이러한 기능들은 프로젝트를 분석하여 동적 기능을 제공하는 프로그램인 Language Sever에 의해 구동됩니다. 
한가지 예로 VS code에 번들로 제공되는 [`typescript-language-features`](https://github.com/Microsoft/vscode/tree/master/extensions/typescript-language-features) 익스텐션 입니다. 이는 [TypeScript Language Service](https://github.com/Microsoft/TypeScript/wiki/Using-the-Language-Service-API)를 활용하여 다음과 같은 프로그래밍 언어 기능을 제공합니다 : 

<!--
Programmatic Language Features include auto completion, error checking, and jump to definition. These features are often powered by a Language Server, a program that analyzes your project to provide the dynamic features.
One example is the [`typescript-language-features`](https://github.com/Microsoft/vscode/tree/master/extensions/typescript-language-features) extension bundled in VS Code. It utilizes the [TypeScript Language Service](https://github.com/Microsoft/TypeScript/wiki/Using-the-Language-Service-API) to offer Programmatic Language Features such as: -->

- 호버링 정보 ([`vscode.languages.registerHoverProvider`](/api/references/vscode-api#languages.registerHoverProvider))
- 자동 완성 ([`vscode.languages.registerCompletionItemProvider`](/api/references/vscode-api#languages.registerCompletionItemProvider))
- 정의로 이동 ([`vscode.languages.registerDefinitionProvider`](/api/references/vscode-api#languages.registerDefinitionProvider))
- 에러 확인
- 포맷팅
- 리팩토링
- 접기

<!--
- Hover information ([`vscode.languages.registerHoverProvider`](/api/references/vscode-api#languages.registerHoverProvider))
- Auto completion ([`vscode.languages.registerCompletionItemProvider`](/api/references/vscode-api#languages.registerCompletionItemProvider))
- Jump to definition ([`vscode.languages.registerDefinitionProvider`](/api/references/vscode-api#languages.registerDefinitionProvider))
- Error checking
- Formatting
- Refactoring
- Folding
-->

완전한 목록은 여기 [프로그래밍 언어 기능](/api/language-extensions/programmatic-language-features)에 있습니다. 
<!-- Here is a complete list of [Programmatic Language Features](/api/language-extensions/programmatic-language-features).-->

![multi-ls](images/overview/multi-ls.png)

## Language Server 프로토콜
<!--
## Language Server Protocol -->

Language Sever(정적 코드 분석 도구)와 Language Client (소스 코드 에디터)간의 통신을 표준화 하는 것으로,[Language Server Protocol](https://microsoft.github.io/language-server-protocol/) 는 익스텐션 저자가 코드 분석 프로그램을 작성하고, 여러 에디터에서 재 사용할 수 있게 합니다. 
<!--
By standardizing the communication between a Language Server (a static code analysis tool) and a Language Client (usually a source code editor), the [Language Server Protocol](https://microsoft.github.io/language-server-protocol/) allows extension authors to write one code analysis program and reuse it in multiple editors. -->

[프로그래밍 언어 기능](/api/language-extensions/programmatic-language-features) 목록에서, 여러분은 모든 VS Code 언어 기능 목록을 찾을 수 있고, 어떻게 [Language Server Protocol Specification](https://microsoft.github.io/language-server-protocol/specification)에 매핑 되어 있는지 확인 할 수 있습니다..

<!--
In the [Programmatic Language Features](/api/language-extensions/programmatic-language-features) listing, you can find a listing of all VS Code language features and how they map to the [Language Server Protocol Specification](https://microsoft.github.io/language-server-protocol/specification). -->

다음은 VS Code에서 Language Server 익스텐션을 구현하는 더 자세한 가이드입니다:
<!--
We offer an in-depth guide that explains how to implement a Language Server extension in VS Code: -->

- [Language Server 익스텐션 가이드](/api/language-extensions/language-server-extension-guide)
<!-- - [Language Server Extension Guide](/api/language-extensions/language-server-extension-guide)-->

![multi-editor](images/overview/multi-editor.png)

## 특수 케이스
<!--
## Special cases -->

### 다중 루트 작업공간 지원
<!-- ### Multi-root workspace support-->

사용자가 [다중 루트 작업공간](/docs/editor/multi-root-workspaces)을 연 경우, 여러분은 Language Server 익스텐션이 잘 작동하게 변형 시켜야 합니다. 이 주제에서는 다중 루트 작업공간을 지원하기 위한 여러 방법들을 논의 합니다. 

<!--
When the user opens a [multi-root workspace](/docs/editor/multi-root-workspaces), you might need to adapt your Language Server extensions accordingly. This topic discusses multiple approaches to supporting multi-root workspaces. -->


### 임베디드 언어
<!--
### Embedded languages -->

임베디드 언어는 웹 개발에서 자주 쓰입니다. 예를 들어 HTML 내부에 CSS/JS, 그리고 자바스크립트/타입스크립트 내부의 GraphQL이 있습니다. 이 주제에서는 
임베디드 언어에서 언어 기능을 사용하게 하는 방법에 대해 다룹니다. 

<!--
Embedded languages are common in web development. For example, CSS/JS inside HTML, and GraphQL inside JavaScript/TypeScript. This topic discusses how you can make language features available to embedded languages.-->
