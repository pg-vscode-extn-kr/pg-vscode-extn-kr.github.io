---
layout: default
parent: Extension Capabilities
title: Theming
nav_order: 3
description: ""
---

# 테마

Visual Studio Code에는, 두 종류의 테마가 존재합니다:

- **색상 테마**: UI 컴포넌트 식별자와 텍스트 토큰 식별자의 색을 지정합니다. 색상 테마는 VS Code의 UI 컴포넌트와 편집기의 텍스트 모두 원하는 색상을 적용할 수 있습니다.
- **아이콘 테마**: 파일 종류 / 파일 이름을 특정 이미지로 지정합니다. 파일 아이콘은 VS Code UI에서 파일 탐색기, 빠른 열기 목록, 에디터 탭과 같은 위치에 표시됩니다.

<!--
# Theming

In Visual Studio Code, there are two types of themes:

- **Color Theme**: A mapping from both UI Component Identifier and Text Token Identifier to colors. Color theme allows you to apply your favorite colors to both VS Code UI Components and the text in the editor.
- **Icon Theme**: A mapping from file type / file name to images. The file icon is displayed across the VS Code UI in places such as File Explorer, Quick Open List, and Editor Tab.
-->

## 컬러 테마

![컬러-테마](images/theming/color-theme.png)

이미지에서 보듯이, 색상 테마는 두 가지 매핑 종류가 있습니다:

- `colors`는 UI 컴포넌트를 제어하는 매핑 종류입니다.
- `tokenColors`는 특정 소스 코드 토큰을 제어하는 매핑 종류입니다. (소스 코드는 [문법](/api/language-extensions/syntax-highlight-guide)단위로 나뉘어 집니다.)

We have a [컬러 테마 가이드](/api/extension-guides/color-theme) and a [컬러 테마 예제](https://github.com/Microsoft/vscode-extension-samples/tree/master/theme-sample) that illustrates how to create a theme.


<!--
## Color Theme

![color-theme](images/theming/color-theme.png)

As you can see in the illustration, Color Theme defines two mappings:

- The `colors` mapping that controls colors for UI Components.
- The `tokenColors` mapping that controls colors for each source code token (your source code is broken into tokens by a [grammar](/api/language-extensions/syntax-highlight-guide)).

We have a [Color Theme Guide](/api/extension-guides/color-theme) and a [Color Theme Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/theme-sample) that illustrates how to create a theme.
-->


## 아이콘 테마

아이콘 테마는 다음과 같은 기능을 합니다:

- 특정 아이콘 식별자에서 이미지 또는 폰트 아이콘으로 매핑합니다.
- 파일을 파일 이름 또는 언어 종류를 이용해 특정 아이콘 식별자와 연결합니다.

[아이콘 테마 가이드](/api/extension-guides/icon-theme)는 어떻게 아이콘 테마를 만드는지 설명하는 가이드입니다.

![아이콘-테마](images/theming/icon-theme.png)


<!--
## Icon Theme

Icon themes allow you to:

- Create a mapping from unique icon identifiers to images or font icons.
- Associate files to these unique icon identifiers by filenames or file language types.

The [Icon Theme Guide](/api/extension-guides/icon-theme) discusses how to create an Icon Theme.

![icon-theme](images/theming/icon-theme.png)
-->
