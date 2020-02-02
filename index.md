---
layout: default
title: 개요
nav_order: 1
description: ""
permalink: 
---


# 익스텐션 API
<!--
Visual Studio Code is built with extensibility in mind. From the UI to the editing experience, almost every part of VS Code can be customized and enhanced through the Extension API. In fact, many core features of VS Code are built as [extensions](https://github.com/Microsoft/vscode/tree/master/extensions) and use the same Extension API.
-->
비주얼 스튜디오 코드는 확장성을 염두에 두고 개발되었습니다. UI부터 시작해 편집 방식에 이르기까지, VS Code의 거의 모든 부분을 익스텐션 API를 통해 사용자화하고 발전시킬 수 있습니다. 실제로 많은 VS Code의 핵심 기능들이  이 문서에서 소개하는 API를 활용한 [익스텐션](https://github.com/Microsoft/vscode/tree/master/extensions) 형태로 개발되었습니다. 


<!--
This documentation describes:

- How to build, run, debug, test and publish an extension
- How to take advantage of VS Code's rich Extension API
- Where to find guides and code samples to help get you started

If you are looking for published extensions, head to the [VS Code Extension Marketplace](https://marketplace.visualstudio.com/vscode).

-->

이 문서는 다음과 같은 내용에 대해 기술합니다. 

- VS Code 익스텐션을 빌드, 실행, 디버그, 테스트, 그리고 게시하는 방법
- VS Code의 풍부한 익스텐션 API를 제대로 활용하는 방법
- 당신이 쉽게 개발을 시작할 수 있도록 도와줄 설명과 예시 코드들을 찾을 수 있는 곳

이미 게시된 익스텐션을 찾고 있다면 [VS Code 익스텐션 마켓플레이스](https://marketplace.visualstudio.com/vscode)를 참조하십시오. 

<!--
## What's new?

VS Code updates on a monthly cadence, and that applies to the Extension API as well. New features and APIs become available every month to increase the power and scope of VS Code extensions.

To stay current with the Extension API, you can review the monthly release notes, which have dedicated sections covering:

* [Extension authoring](https://code.visualstudio.com/updates#_extension-authoring) - Learn what new extension APIs are available in the latest release.
* [Proposed extension APIs](https://code.visualstudio.com/updates#_proposed-extension-apis) - Review and give feedback on upcoming proposed APIs.

-->

## 새로운 요소

VS Code는 달마다 업데이트되고, 익스텐션 API 또한 마찬가지입니다. 새로운 특성들과 API가 매달 추가되어 VS Code 익스텐션을 더욱더 풍부하고 강력하게 만듭니다. 

익스텐션 API의 최신 버전과 함께하려면 매달 공개되는 릴리즈 노트를 참조하십시오. 릴리즈 노트는 다음과 같은 섹션을 포함하고 있습니다. 

* [Extension authoring (익스텐션 저작물)](https://code.visualstudio.com/updates#_extension-authoring) - 최신 릴리즈가 포함하고 있는 새로운 익스텐션 API에 대해 소개합니다. 
* [Proposed extension APIs (공개 예정 API)](https://code.visualstudio.com/updates#_proposed-extension-apis) - 릴리즈 예정인 API에 대해 리뷰와 피드백을 주십시오.

<!--
## What can extensions do?

Here are some examples of what you can achieve with the Extension API:

- Change the look of VS Code with a color or icon theme - [Theming](/api/extension-capabilities/theming)
- Add custom components & views in the UI - [Extending the Workbench](/api/extension-capabilities/extending-workbench)
- Create a Webview to display a custom webpage built with HTML/CSS/JS - [Webview Guide](/api/extension-guides/webview)
- Support a new programming language - [Language Extensions Overview](/api/language-extensions/overview)
- Support debugging a specific runtime - [Debugger Extension Guide](/api/extension-guides/debugger-extension)

If you'd like to have a more comprehensive overview of the Extension API, refer to the [Extension Capabilities Overview](/api/extension-capabilities/overview) page. [Extension Guides Overview](/api/extension-guides/overview) also includes a list of code samples and guides that illustrate various Extension API usage.
-->

## 익스텐션이 할 수 있는 일

익스텐션 API를 사용하면 다음과 같은 일들을 할 수 있습니다. 

- 새로운 색상이나 아이콘 테마를 VS Code에 적용 - [Theming](/api/extension-capabilities/theming)
- 사용자화된 구성 요소나 뷰를 UI에 추가 - [Extending the Workbench](/api/extension-capabilities/extending-workbench)
- HTML/CSS/JS를 기반으로 빌드한 새로운 웹페이지를 띄우는 웹뷰 (Webview) 생성 - [Webview Guide](/api/extension-guides/webview)
- 새로운 프로그래밍 언어를 지원 - [Language Extensions Overview](/api/language-extensions/overview)
- 특정 런타임에서의 디버깅 지원 - [Debugger Extension Guide](/api/extension-guides/debugger-extension)
  
익스텐션 API에 대한 좀 더 포괄적인 이해가 필요하다면, [Extension Capabilities Overview](/api/extension-capabilities/overview) 페이지를 참조하십시오. [Extension Guides Overview](/api/extension-guides/overview) 페이지에서도 익스텐션 API를 활용할 다양한 방법을 보여주는 샘플 코드들과 가이드를 제공합니다. 

<!--
## How to build extensions?

Building a good extension can take a lot of effort. Here is what each section of the API doc can help you with:

- **Get Started** teaches fundamental concepts for building extensions with the [Hello World](https://github.com/Microsoft/vscode-extension-samples/tree/master/helloworld-sample) sample.
- **Working with Extensions** includes in-depth guides on various extension development topics, such as [publishing](/api/working-with-extensions/publishing-extension) and [testing](/api/working-with-extensions/testing-extension) extensions.
- **Extension Capabilities** dissects VS Code's vast API into smaller categories and points you to more detailed topics.
- **Extension Guides** includes guides and code samples that explain specific usages of VS Code Extension API.
- **Language Extensions** illustrates how to add support for a programming language with guides and code samples.
- **Advanced Topics** explains advanced concepts such as [Extension Host](/api/advanced-topics/extension-host), [Supporting Remote Development and VS Online](/api/advanced-topics/remote-extensions), and [Proposed API](/api/advanced-topics/using-proposed-api).
- **References** contains exhaustive references for the [VS Code API](/api/references/vscode-api), [Contribution Points](/api/references/contribution-points), and many other topics.

-->

## 익스텐션 빌드하기

좋은 익스텐션을 빌드하기까지는 상당한 노력이 필요합니다. 이 문서의 각 섹션에서는 다음과 같은 도움을 얻을 수 있습니다. 

- **Get Started** 에서는 [Hello World](https://github.com/Microsoft/vscode-extension-samples/tree/master/helloworld-sample) 샘플을 통해 익스텐션 빌드의 핵심 개념들을 소개합니다.
- **Working with Extensions**는 [publishing](/api/working-with-extensions/publishing-extension)이나 [testing](/api/working-with-extensions/testing-extension)과 같은 익스텐션 개발에 관한 다양한 상세 설명을 포함하고 있습니다. 
- **Extension Capabilities**에서는 VS Code의 풍부한 API를 작은 카테고리로 나누고 더 상세한 설명을 제공합니다. 
- **Extension Guides**에서는 VS Code 익스텐션 API를 사용하는 구체적인 방법을 보여주는 가이드와 샘플 코드를 제공합니다. 
- **Language Extensions**는 코드 샘플과 함께 프로그래밍 언어를 서포트하는 방법을 설명합니다. 
- **Advanced Topics**에서는 [Extension Host](/api/advanced-topics/extension-host), [Supporting Remote Development and VS Online](/api/advanced-topics/remote-extensions), 그리고 [Proposed API](/api/advanced-topics/using-proposed-api)과 같은 고급 개념에 대해 설명합니다. 
- **References**는 [VS Code API](/api/references/vscode-api)와 [Contribution Points](/api/references/contribution-points)를 비롯해 다양한 주제에 대한 상세한 레퍼런스를 포함하고 있습니다. 

<!--
## Looking for help

If you have questions for extension development, try asking on:

- [Stack Overflow](https://stackoverflow.com/questions/tagged/visual-studio-code): There are [thousands of questions](https://stackoverflow.com/questions/tagged/visual-studio-code) tagged `visual-studio-code`, and over half of them already have answers. Search for your issue, ask questions, or help your fellow developers by answering VS Code extension development questions!
- [Gitter Channel](https://gitter.im/Microsoft/vscode) and [VS Code Dev Slack](https://aka.ms/vscode-dev-community): Public chatroom for extension developers. Some VS Code team members chime in conversations.

To provide feedback on the documentation, create new issues at [Microsoft/vscode-docs](https://github.com/Microsoft/vscode-docs/issues).
If you have extension questions that you cannot find an answer for, or issues with the VS Code Extension API, please open new issues at [Microsoft/vscode](https://github.com/Microsoft/vscode/issues).

-->

## 도움말

- [스택 오버플로우](https://stackoverflow.com/questions/tagged/visual-studio-code): `visual-studio-code` 태그가 달린 [수천개의 질문](https://stackoverflow.com/questions/tagged/visual-studio-code)이 있으며, 그 중 절반 정도에 이미 답변이 달려 있습니다. 발생한 문제에 대해 검색하고 질문하십시오. 또 VS Code 익스텐션 개발에 대한 질문에 답변을 달아 동료 개발자들을 도와주십시오!
- [Gitter Channel](https://gitter.im/Microsoft/vscode)과 [VS Code Dev Slack](https://aka.ms/vscode-dev-community): 익스텐션 개발자들을 위한 공개 채팅방입니다. VS Code 개발팀 중 일부가 채팅방에 속해 있습니다. 

이 문서에 대해 피드백하고 싶으시다면, [pg-vscode-extn-kr/pg-vscode-extn-kr.github.io](https://github.com/pg-vscode-extn-kr/pg-vscode-extn-kr.github.io)에 새로운 이슈를 달아 주십시오. 원본 문서에 대해 피드백하고 싶으시다면, [Microsoft/vscode-docs](https://github.com/Microsoft/vscode-docs/issues)에 이슈를 다시면 됩니다. 
익스텐션 개발에 대한 해결책을 찾기 힘든 문제가 있거나 API에 대한 문제가 발생한다면, [Microsoft/vscode](https://github.com/Microsoft/vscode/issues)에 새로운 이슈를 생성해 주십시오. 