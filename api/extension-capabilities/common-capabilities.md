---
layout: default
parent: Extension Capabilities
title: Common Capabilities
nav_order: 2
description: ""
---


# 공용 기능 
<!-- # Common Capabilities-->

공용 기능은 여러분의 익스텐션 블록을 짓는데에 중요합니다. 거의 모든 익스텐션이 이러한 기능 중 몇가지를 사용합니다. 다음은 이를 이용하는 방법을 설명합니다.  

<!--
Common Capabilities are important building blocks for your extensions. Almost all extensions use some of these functionalities. Here is how you can take advantage of them.-->

## 커맨드
<!--
## Command-->

커맨드는 VS Code 작동 방식의 중심입니다. 여러분은 커맨드를 실행하기 위해 `Command Palette`를 열어, 커스텀 키조합을 커맨드에 설정하고, 우클릭을 통해 `Context 메뉴`의 커맨드를 불러올 수 있습니다. 

<!--
Command is central to how VS Code works. You open the Command Palette to execute commands, bind custom keybindings to commands, and right-click to invoke commands in Context Menus.-->


익스텐션으로 : 

<!--
An extension could: -->

- [`vscode.commands`](/api/references/vscode-api#commands) API를 사용하여, 커맨드를 등록하고 실행할 수 있습니다.  
- [`contributes.commands`](/api/references/contribution-points#contributes.commands) Contribution Point를 사용하여 커맨드를 `Command Palette`에서 이용할 수 있습니다. 

<!--
- Register and execute commands with the [`vscode.commands`](/api/references/vscode-api#commands) API.
- Make commands available in the Command Palette with the [`contributes.commands`](/api/references/contribution-points#contributes.commands) Contribution Point.
-->

[Extension Guides / Command](/api/extension-guides/command) 주제를 참조하여, 커맨드에 대하여 더 알 수 있습니다. 

<!--
Learn more about commands at the [Extension Guides / Command](/api/extension-guides/command) topic.
-->

## 구성

<!--
## Configuration
-->

익스텐션은 [`contributes.configuration`](/api/references/contribution-points#contributes.configuration) Contribution Point 를 통해서 특정 익스텐션 설정에 기여 하고, [`workspace.getConfiguration`](/api/references/vscode-api#workspace.getConfiguration) API를 사용하여 설정을 읽을 수 있습니다. 

<!-- 
An extension can contribute extension-specific settings with the [`contributes.configuration`](/api/references/contribution-points#contributes.configuration) Contribution Point and read them using the [`workspace.getConfiguration`](/api/references/vscode-api#workspace.getConfiguration) API.
-->

## 키바인딩

<!-- 
## Keybinding-->

익스텐션은 커스텀 키조합을 더할 수 있습니다. [`contributes.keybindings`](/api/references/contribution-points#contributes.keybindings)와 
[Key Bindings](/docs/getstarted/keybindings) 주제에서 더 많은 내용을 확인하십시오. 

<!--
An extension can add custom keybindings. Read more in the [`contributes.keybindings`](/api/references/contribution-points#contributes.keybindings) and [Key Bindings](/docs/getstarted/keybindings) topics.
-->

## Context 메뉴

<!-- ## Context Menu -->
익스텐션은 VS Code에서 우클릭을 통해 다른 부분을 보여주는 UI 커스텀 Context 메뉴를 등록 할 수 있습니다. [`contributes.menus`](/api/references/contribution-points#contributes.menus) Contribution Point 에서 더 많은 내용을 확인하십시오.

<!--
An extension can register custom Context Menu items that will be displayed in different parts of the VS Code UI on right-click. Read more at the [`contributes.menus`](/api/references/contribution-points#contributes.menus) Contribution Point.
-->

## 데이터 저장

<!--
## Data Storage -->

데이터를 저장하기 위해서, 4가지 방법이 있습니다. 

<!-- There are four options for storing data: -->


- [`ExtensionContext.workspaceState`](/api/references/vscode-api#ExtensionContext.workspaceState): key/value쌍을 작업공간에 저장하기.
VS Code는 동일한 작업공간이 다시 열렸을때 데이터 저장 내역을 복구 할 것입니다. 
- [`ExtensionContext.globalState`](/api/references/vscode-api#ExtensionContext.globalState): key/value쌍을 전역환경에 저장하기. VS Code는 각 익스텐션이 활성화 될때 데이터 저장 내역을 복구 할 것입니다. 
- [`ExtensionContext.storagePath`](/api/references/vscode-api#ExtensionContext.storagePath): 익스텐션이 읽기/쓰기 권한을 가진 local directory 작업공간에 저장하기. 이 방법은 현재 작업공간에서만 접근가능한 큰 파일을 저장 할 때 좋습니다. 
- [`ExtensionContext.globalStoragePath`](/api/references/vscode-api#ExtensionContext.globalStoragePath): 익스텐션이 읽기/쓰기 권한을 가진 로컬 디렉토리를 가리키는 전역 저장 경로에 저장하기. 이 방법은 모든 작업공간에서 접근가능한 큰 파일을 저장 할 때 좋습니다. 

<!--
- [`ExtensionContext.workspaceState`](/api/references/vscode-api#ExtensionContext.workspaceState): A workspace storage where you can write key/value pairs. VS Code manages the storage and will restore it when the same workspace is opened again.
- [`ExtensionContext.globalState`](/api/references/vscode-api#ExtensionContext.globalState): A global storage where you can write key/value pairs. VS Code manages the storage and will restore it for each extension activation.
- [`ExtensionContext.storagePath`](/api/references/vscode-api#ExtensionContext.storagePath): A workspace specific storage path pointing to a local directory where your extension has write/read access. This is a good option if you need to store large files that is accessible only from current workspace.
- [`ExtensionContext.globalStoragePath`](/api/references/vscode-api#ExtensionContext.globalStoragePath): A global storage path pointing to a local directory where your extension has write/read access. This is a good option if you need to store large files that is accessible from all workspaces.
-->

익스텐션 context는 [Extension Entry File](/api/get-started/extension-anatomy#extension-entry-file)의 `activate` 기능에 사용 할 수 있습니다. <!-- 
The extension context is available to the `activate` function in the [Extension Entry File](/api/get-started/extension-anatomy#extension-entry-file). -->

## 디스플레이 알림

<!-- ## Display Notifications -->
거의 모든 익스텐션은 사용자에게 때에 따라 정보를 보여야 합니다. VS Code는 알림을 보이기 위한 3가지 API를 중요도에 따라 제공하고 있습니다. 

<!--
Almost all extensions need to present information to the user at some point. VS Code offers three APIs for displaying notification messages of different severity:-->

- [`window.showInformationMessage`](/api/references/vscode-api#window.showInformationMessage)
- [`window.showWarningMessage`](/api/references/vscode-api#window.showWarningMessage)
- [`window.showErrorMessage`](/api/references/vscode-api#window.showErrorMessage)

<!--
- [`window.showInformationMessage`](/api/references/vscode-api#window.showInformationMessage)
- [`window.showWarningMessage`](/api/references/vscode-api#window.showWarningMessage)
- [`window.showErrorMessage`](/api/references/vscode-api#window.showErrorMessage)
-->

## 빠른 선택
<!--
## Quick Pick -->

[`vscode.QuickPick`](/api/references/vscode-api#QuickPick) API를 통해, 여러분은 쉽게 사용자 입력을 수집하거나, 사용자가 여러가지 선택지를 고르게 할 수 있습니다. [QuickInput Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/quickinput-sample)에서 API를 설명하고 있습니다.  

<!--
With the [`vscode.QuickPick`](/api/references/vscode-api#QuickPick) API, you can easily collect user input or let the user make a selection from multiple options. The [QuickInput Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/quickinput-sample) illustrates the API. -->

## 파일 선택
<!--
## File Picker -->

익스텐션은 [`vscode.window.showOpenDialog`](/api/references/vscode-api#vscode.window.showOpenDialog) API를 이용하여 시스템의 파일 혹은 폴더를 선택 할 수 있습니다. 
<!--
Extensions can use the [`vscode.window.showOpenDialog`](/api/references/vscode-api#vscode.window.showOpenDialog) API to open the system file picker and select files or folders.-->

## 출력 채널
<!--
## Output Channel
-->

출력 패널은 로그를 남기는 목적에 적합한 여러 [`OutputChannel`](/api/references/vscode-api#OutputChannel)들을 보여줍니다. 여러분은 [`window.createOutputChannel`](/api/references/vscode-api#window.createOutputChannel) API를 사용하여 이를 쉽게 이용 할 수 있습니다. 

<!--
The Output Panel displays a collection of [`OutputChannel`](/api/references/vscode-api#OutputChannel), which are great for logging purpose. You can easily take advantage of it with the [`window.createOutputChannel`](/api/references/vscode-api#window.createOutputChannel) API. -->
 
## 진행과정 API
<!-- 
## Progress API -->

[`vscode.Progress`](/api/references/vscode-api#Progress) API를 사용하여 사용자에게 진행과정을 업데이트 하십시오.

<!--
You can use the [`vscode.Progress`](/api/references/vscode-api#Progress) API for reporting progress updates to the user.
-->

진행과정은 [`ProgressLocation`](/api/references/vscode-api#ProgressLocation)옵션을 통해 여러 위치에서 볼 수 있습니다. 
<!--
Progress can be shown in different locations using the [`ProgressLocation`](/api/references/vscode-api#ProgressLocation) option:
-->

- Notification 공간에서 
- Source Control view 에서
- VS Code 윈도우의 General progress에서

<!--
- In the Notifications area
- In the Source Control view
- General progress in the VS Code window
-->

[Progress Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/progress-sample)에서 이 API를 설명하고 있습니다. 

<!--
The [Progress Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/progress-sample) illustrates this API.
-->
