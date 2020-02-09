---
layout: default
parent: References
title: Activation Events
nav_order: 3
description: ""
---

# 활성화 이벤트

<!--
# Activation Events -->

**활성화 이벤트** 는 `package.json` [Extension Manifest](/api/references/extension-manifest)의 `activationEvents` 부분에서 작성되는 JSON 선언의 모음 입니다. 익스텐션은 **활성화 이벤트** 가 일어날때 활성화 됩니다. 아래는 모든 사용 가능한 **활성화 이벤트** 의 목록입니다:

<!--
**Activation Events** is a set of JSON declarations that you make in the `activationEvents` field of `package.json` [Extension Manifest](/api/references/extension-manifest). Your extension becomes activated when the **Activation Event** happens. Here is a list of all available **Activation Events**: -->

- [`onLanguage`](/api/references/activation-events#onLanguage)
- [`onCommand`](/api/references/activation-events#onCommand)
- [`onDebug`](/api/references/activation-events#onDebug)
  - [`onDebugInitialConfigurations`](/api/references/activation-events#onDebugInitialConfigurations)
  - [`onDebugResolve`](/api/references/activation-events#onDebugResolve)
- [`workspaceContains`](/api/references/activation-events#workspaceContains)
- [`onFileSystem`](/api/references/activation-events#onFileSystem)
- [`onView`](/api/references/activation-events#onView)
- [`onUri`](/api/references/activation-events#onUri)
- [`onWebviewPanel`](/api/references/activation-events#onWebviewPanel)
- [`*`](/api/references/activation-events#Start-up)

또한 [`package.json` extension manifest]의 모든 필드에 대하여 참조할 수 있는 내용을 제공합니다. 

<!--
We also provide a reference of all fields in the [`package.json` extension manifest](/api/references/extension-manifest). -->

## onLanguage

특정 언어로 해석되는 파일이 열릴때마다 이 활성화 이벤트가 발생하고, 연관된 익스텐션이 활성화 됩니다. 

<!--
This activation event is emitted and interested extensions will be activated whenever a file that resolves to a certain language gets opened. -->

```json
...
"activationEvents": [
    "onLanguage:python"
]
...
```

`onLanguage` 이벤트는 [언어 식별자](/docs/languages/identifiers) 값을 입력받습니다.
<!--
The `onLanguage` event takes a [language identifier](/docs/languages/identifiers) value. -->

`activationEvents` 어레이에서 별도의 `onLanguage` 항목으로 여러 언어들을 선언 할 수 있습니다.

<!--
Multiple languages can be declared with separate `onLanguage` entries in the `activationEvents` array. -->

```json
"activationEvents": [
    "onLanguage:json",
    "onLanguage:markdown",
    "onLanguage:typescript"
]
...
```

## onCommand

커맨드가 실행 될때마다 이 활성화 이벤트는 실행되고 관련된 익스텐션이 활성화 될 것입니다:
<!--
This activation event is emitted and interested extensions will be activated whenever a command is being invoked: -->

```json
...
"activationEvents": [
    "onCommand:extension.sayHello"
]
...
```

## onDebug

디버그 세션이 시작되기전에 이 활성화 이벤트가 실행되고 관련된 익스텐션이 활성화 될 것입니다:
<!--
This activation event is emitted and interested extensions will be activated before a debug session is started:-->

```json
...
"activationEvents": [
    "onDebug"
]
...
```

### onDebugInitialConfigurations

### onDebugResolve

다음은 2개의 더욱 세밀한 `onDebug` 활성화 이벤트 입니다:
<!--
These are two more fine-grained `onDebug` activation events: -->

- `onDebugInitialConfigurations` 는 `DebugConfigurationProvider`의 `provideDebugConfigurations`메소드가 호출 되기 직전에 발생합니다.
- `onDebugResolve:type` 는 특정한 타입에 대한 `DebugConfigurationProvider`의 `resolveDebugConfiguration`메소드가 호출 되기 직전에 발생합니다.

<!--
- `onDebugInitialConfigurations` is fired just before the `provideDebugConfigurations` method of the `DebugConfigurationProvider` is called.
- `onDebugResolve:type` is fired just before the `resolveDebugConfiguration` method of the `DebugConfigurationProvider` for the specified type is called. -->

**기본 원칙:** 디버그 익스텐션 익스텐션이 가벼운 경우에는, `onDebug`를 사용하십시오. 만약 무거운 경우, `DebugConfigurationProvider`가 대응하는 메소드인 `provideDebugConfigurations` 혹은 `resolveDebugConfiguration`을 사용하는 지에 따라 `onDebugInitialConfigurations` 나  `onDebugResolve`를 사용하십시오. 이러한 메소드에 대한 더 자세한 내용은 [Using a DebugConfigurationProvider](/api/extension-guides/debugger-extension#using-a-debugconfigurationprovider)를 참조하십시오. 

<!--
**Rule of thumb:** If activation of a debug extension is lightweight, use `onDebug`. If it is heavyweight, use `onDebugInitialConfigurations` and/or `onDebugResolve` depending on whether the `DebugConfigurationProvider` implements the corresponding methods `provideDebugConfigurations` and/or `resolveDebugConfiguration`. See [Using a DebugConfigurationProvider](/api/extension-guides/debugger-extension#using-a-debugconfigurationprovider) for more details on these methods. -->

## workspaceContains

폴더가 열리고, 해당 폴더가 최소 1개 이상의 glob 패턴과 일치하는 파일을 포함하는 경우 이 활성화 이벤트가 실행되고 관련된 익스텐션이 활성화 될 것입니다.

<!--
This activation event is emitted and interested extensions will be activated whenever a folder is opened and the folder contains at least one file that matches a glob pattern.-->

```json
...
"activationEvents": [
    "workspaceContains:**/.editorconfig"
]
...
```

## onFileSystem

특정 _scheme_ 의 파일이나 폴더를 읽을 때마다 이 활성화 이벤트가 실행되고 관련있는 익스텐션이 활성화 될 것입니다. 이는 일반적으로 `file`-체계 이지만, 커스텀 파일 시스템 제공자를 사용하여 `ftp`나 `ssh` 같은 더 많은 체계를 허용 할 수 있습니다.

<!--
This activation event is emitted and interested extensions will be activated whenever a file or folder from a specific _scheme_ is read. This is usually the `file`-scheme, but with custom file system providers more schemes come into place, e.g `ftp` or `ssh`. -->

```json
...
"activationEvents": [
    "onFileSystem:sftp"
]
...
```

## onView

VS Code 사이드바에서 지정된 id의 뷰가 확장 될 때 마다 이 활성화 이벤트가 실행되고 관련된 익스텐션이 활성화 될 것입니다. (빌트인 뷰의 예시로는 익스텐션이나 소스 컨트롤이 있습니다) 

<!--
This activation event is emitted and interested extensions will be activated whenever a view of the specified id is expanded in the VS Code sidebar (Extensions or Source Control are examples of built-in views). -->

아래의 활성화 이벤트는 `nodeDependencies` id를 가진 뷰가 보여질때마다 실행 될 것입니다:

<!--
The activation event below will fire whenever a view with the `nodeDependencies` id is visible: -->

```json
...
"activationEvents": [
    "onView:nodeDependencies"
]
...
```

## onUri

익스텐션에 해당하는  전-시스템 Uri가 열릴때마다, 이 활성화 이벤트가 실행되고 관련있는 익스텐션이 활성화 될 것입니다. Uri 의 형태는 `vscode` 나 `vscode-insiders` 로 고정되어 있습니다. Uri의 권한은 반드시 익스텐션의 식별자 여야합니다. Uri의 나머지 부분은 임의적입니다.

<!--
This activation event is emitted and interested extensions will be activated whenever a system-wide Uri for that extension is opened. The Uri scheme is fixed to either `vscode` or `vscode-insiders`. The Uri authority must be the extension's identifier. The rest of the Uri is arbitrary. -->

```json
...
"activationEvents": [
    "onUri"
]
...
```

만약 `vscode.git` 익스텐션이 `onUri`를 활성화 이벤트로 정의하는 경우, 다음 Uri가 열릴때마다 활성화 될 것입니다:

<!--
If the `vscode.git` extension defines `onUri` as an activation event, it will be activated in any of the following Uris are open: -->

- `vscode://vscode.git/init`
- `vscode://vscode.git/clone?url=https%3A%2F%2Fgithub.com%2FMicrosoft%2Fvscode-vsce.git`
- `vscode-insiders://vscode.git/init` (for VS Code Insiders)

## onWebviewPanel

VS Code가 일치하는 `viewType`으로 웹 뷰를 복원할때마다 이 활성화 이벤트가 실행되고 관련있는 익스텐션이 활성화 될 것입니다.
<!--
This activation event is emitted and interested extensions will be activated whenever VS Code needs to restore a webview with the matching `viewType`. -->

예를 들어, 아래와 같은 onWebViewPanel 선언은:

<!--
For example, the declaration of onWebviewPanel below: -->

```json
"activationEvents": [
    ...,
    "onWebviewPanel:catCoding"
]
```

VS Code viewType : `catCoding`을 사용하여 웹 뷰를 복원 할때마다 익스텐션이 활성화 되게 할 것입니다. viewType은 `window.createWebviewPanel` 호출에서 설정되며 익스텐션을 처음 활성화 하고, 웹 뷰를 생성하기 위하여 다른 활성화 이벤트 (예시로, onCommand)가 있어야 합니다.  

<!--
will cause the extension to be activated when VS Code needs to restore a webview with the viewType: `catCoding`. The viewType is set in the call to `window.createWebviewPanel` and you will need to have another activation event (for example, onCommand) to initially activate your extension and create the webview. -->

## 시작
<!--
## Start up -->

`*` 활성화 이벤트는 VS Code 가 시작 될 때 실행되고 관련된 익스텐션이 활성화 될 것입니다. 최상의 사용자 경험을 위해, 익스텐션에서 다른 활성화 이벤트 조합이 작동하지 않는 경우에만 이 활성화 이벤트를 사용하십시오. 

<!--
The `*` activation event is emitted and interested extensions will be activated whenever VS Code starts up. To ensure a great end user experience, please use this activation event in your extension only when no other activation events combination works in your use-case. -->

```json
...
"activationEvents": [
    "*"
]
...
```

> **주의:** 익스텐션은 여러개의 활성화 이벤트를 수신 할 수 있으므로, `"*"`를 수신하는 것이 좋습니다.

<!--
> **Note:** An extension can listen to multiple activation events, and that is preferable to listening to `"*"`. -->

> **주의:** 익스텐션은 **반드시** 메인 모듈에서 `activate()` 기능을 내보내야하며 이는 VS Code 에 의해 지정된 활성화 이벤트가 실행 되었을 경우에  **한번만** 실행 될 것입니다. 또한 익스텐션은 메인 모듈에서 `deactivate()`를 내보내야 하며 이를 통해 VS Code가 종료되었을때 정리 작업을 수행해야 합니다. 익스텐션은 정리작업이 비동기방식으로 실행 되는 경우 **반드시** `deactivate()`로 부터 프로미스를 반환해야합니다. 정리가 동기방식으로 실행되는 경우 익스텐션은 `deactivate()`에서 `undefined`를 반환 할 수 있습니다.

<!--
> **Note:** An extension **must** export an `activate()` function from its main module and it will be invoked **only once** by VS Code when any of the specified activation events is emitted. Also, an extension **should** export a `deactivate()` function from its main module to perform cleanup tasks on VS Code shutdown. Extension **must** return a Promise from `deactivate()` if the cleanup process is asynchronous. An extension may return `undefined` from `deactivate()` if the cleanup runs synchronously.
-->
