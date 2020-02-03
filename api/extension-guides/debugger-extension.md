---
layout: default
parent: Extension Guides
title: Debugger Extension
nav_order: 10
description: ""
---

# 디버거 익스텐션

<!--
# Debugger Extension -->

Visual Studieo Code의 디버그 설계는 익스텐션 저자가 쉽게 기존 디버거와 공통된 사용자 인터페이스를 남겨둔채 VS Code에 통합할 수 있게 합니다.

<!--
Visual Studio Code's debugging architecture allows extension authors to easily integrate existing debuggers into VS Code, while having a common user interface with all of them. -->

VS Code는 1가지 빌트인 디버거 익스텐션으로, 많은 VS Code의 디버그 기능을 지원하는 [Node.js](https://nodejs.org) 디버거 익스텐션을 제공합니다.

<!--
VS Code ships with one built-in debugger extension, the [Node.js](https://nodejs.org) debugger extension, which is an excellent showcase for the many debugger features supported by VS Code: -->

![VS Code Debug Features](images/debugger-extension/debug-features.png)

위 스크릿샷은 다음과 같은 디버그 기능을 보여줍니다:

<!-- This screenshot shows the following debugging features: -->

1. 디버그 구성 관리.
2. 시작/중지, 진행을 위한디버그 액션.
3. 소스-, 함수-, 조건-, 인라인 중지점, 로그 포인트.
4. 멀티쓰레드와 멀티프로세스를 지원하는 스택 트레이싱.
5. 뷰와 호버링을 통한 복잡한 데이터 구조 탐색
6. 호버링 혹은 소스의 인라인을 통해 변수값 표기.
7. 표현식 관리.
8. 자동완성을 포함한 인터랙티브 평가용 디버그 콘솔.

<!-- 
1. Debug configuration management.
2. Debug actions for starting/stopping and stepping.
3. Source-, function-, conditional-, inline breakpoints, and log points.
4. Stack traces, including multi-thread and multi-process support.
5. Navigating through complex data structures in views and hovers.
6. Variable values shown in hovers or inlined in the source.
7. Managing watch expressions.
8. Debug console for interactive evaluation with autocomplete.
-->

이 문서는 여러분이 VS Code와 작동하는 어느 디버거도 만들 수 있는 디버거 익스텐션 생성을 도울 것입니다. 

<!--
This documentation will help you create a debugger extension which can make any debugger work with VS Code. -->

## VS Code의 디버깅 구조
<!--
## Debugging Architecture of VS Code -->

VS Code는 디버거 벡엔드와 통신하기 위해 추상화 프로토콜에 기반한 제네릭 (언어 비의존적) 디버거 UI를 구현합니다. 
디버거는 보통 이 프로토콜을 구현하지 않기 때문에, 디버거와 프로토콜을 "연결"하기 위한 중간역이 필요합니다. 이 중간역은 보통 디버거와 통신하는 독립적인 프로세스 입니다.

<!--
VS Code implements a generic (language-agnostic) debugger UI based on an abstract protocol that we've introduced to communicate with debugger backends. 
Because debuggers typically do not implement this protocol, some intermediary is needed to "adapt" the debugger to the protocol.
This intermediary is typically a standalone process that communicates with the debugger. -->

![VS Code Debug Architecture](images/debugger-extension/debug-arch1.png)

이 중간역을 **Debug Adapter** (줄여서 **DA**)로, 그리고 DA와 VS Code간에 쓰이는 추상화 프로토콜을 **Debug Adapter Protocol** (줄여서 **DAP**)이라 부르겠습니다.
Debug Adapter Protocol은 VS Code와 독립적이기 때문에, [소개와 오버뷰](https://microsoft.github.io/debug-adapter-protocol/overview), 구체적인 [특징](https://microsoft.github.io/debug-adapter-protocol/specification), [알려진 구현과 보조 도구](https://microsoft.github.io/debug-adapter-protocol/implementors/adapters/) 를 포함한 자체 [웹사이트](https://microsoft.github.io/debug-adapter-protocol/)를 가지고 있습니다. 
DAP에 관한 역사와 동기는 이 [블로그 포스트](https://code.visualstudio.com/blogs/2018/08/07/debug-adapter-protocol-website#_why-the-need-for-decoupling-with-protocols)에 설명되어 있습니다. 

<!--
We call this intermediary the **Debug Adapter** (or **DA** for short) and the abstract protocol that is used between the DA and VS Code is the **Debug Adapter Protocol** (**DAP** for short).
Since the Debug Adapter Protocol is independent from VS Code, it has its own [web site](https://microsoft.github.io/debug-adapter-protocol/) where you can find an [introduction and overview](https://microsoft.github.io/debug-adapter-protocol/overview), the detailed [specification](https://microsoft.github.io/debug-adapter-protocol/specification), and some lists with [known implementations and supporting tools](https://microsoft.github.io/debug-adapter-protocol/implementors/adapters/).
The history of and motivation behind DAP is explained in this [blog post](https://code.visualstudio.com/blogs/2018/08/07/debug-adapter-protocol-website#_why-the-need-for-decoupling-with-protocols).
-->

Debug adapters는 VS Code와 독립적이며 [다른 개발 도구](https://microsoft.github.io/debug-adapter-protocol/implementors/tools/)에서도 쓰일 수 있는 만큼, VS Code의 익스텐션과 contribution point를 기반으로 하는 확장성 구조에 일치 하지 않습니다. 

<!--
Since debug adapters are independent from VS Code and can be used in [other developments tools](https://microsoft.github.io/debug-adapter-protocol/implementors/tools/), they do not match VS Code's extensibility architecture which is based on extensions and contribution points. -->

이러한 이유로 VS Code는 debug adapter가 특정 디버그 타입에 대응 할 수 있는 (예를 들어 Node.js 디버거를 위한 `node`) `debuggers`라는  contribution point를 제공합니다. VS Code는 사용자가 해당 타입의 디버그 세션을 시작할때 마다 등록된 DA를 실행합니다.

<!--
For this reason VS Code provides a contribution point, `debuggers`, where a debug adapter can be contributed under a specific debug type (e.g. `node` for the Node.js debugger). VS Code launches the registered DA whenever the user starts a debug session of that type. -->

가장 간단한 형태의 디버거 익스텐션은 debug adapter 구현에 대한 선언형식이며, 익스텐션은 기본적으로 추가 코드가 없는 debug adapter를 위한 패키징 컨테이너입니다.

<!--
So in its most minimal form, a debugger extension is just a declarative contribution of a debug adapter implementation and the extension is basically a packaging container for the debug adapter without any additional code. -->

![VS Code Debug Architecture 2](images/debugger-extension/debug-arch2.png)

조금더 현실적인 디버거 익스텐션은 아래와 같은 내용의 다수 혹은 전부를 VS code에 선언합니다:

<!--
A more realistic debugger extension contributes many or all of the following declarative items to VS Code: -->

- 디버거가 지원하는 언어의 목록. VS Code는 UI가 해당 언어에 대해서 중지점설정을 가능 하게 합니다. 
- 디버거가 사용하는 디버그 구성 속성에 대한 JSON schema. VS Code는 이 schema를 launch.json 에디터에서 확인하기 위해 사용하며 IntelliSense를 제공합니다.
- VS Code가 생성하는 초기 launch.json을 위한 기본 디버그 설정.
- 사용자가 launch.json 파일에 추가 할 수 있는 디버그 구성 snippet.
- 디버그 구성에서 사용 할 수 있는 변수들의 선언.

<!--
- List of languages supported by the debugger. VS Code enables the UI to set breakpoints for those languages.
- JSON schema for the debug configuration attributes introduced by the debugger. VS Code uses this schema to verify the configuration in the launch.json editor and provides IntelliSense.
- Default debug configurations for the initial launch.json created by VS Code.
- Debug configuration snippets that a user can add to a launch.json file.
- Declaration of variables that can be used in debug configurations.
-->

더 많은 정보를 [`contributes.breakpoints`](/api/references/contribution-points#contributes.breakpoints)과 [`contributes.debuggers`](/api/references/contribution-points#contributes.debuggers) 에서 확인 할 수 있습니다. 

<!--
You can find more information in [`contributes.breakpoints`](/api/references/contribution-points#contributes.breakpoints) and [`contributes.debuggers`](/api/references/contribution-points#contributes.debuggers) references. -->

위의 선언적인 내용들에 더하여, 디버그 익스텐션 API는 아래와 같은 코드-기반의 기능도 가능합니다:

<!--
In addition to the purely declarative contributions from above, the Debug Extension API enables this code-based functionality: -->

- VS Code에 의해 생성된 초기 launch.json을 위한 동적으로 생성된 기본 디버그 구성.
- 동적 사용을 위한 debug adapter 결정.
- debug adapter로 넘어가기전 디버그 구성 확인 및 수정.
- debug adapter와의 통신
- 디버그 콘솔에 메세지 작성.

<!--
- Dynamically generated default debug configurations for the initial launch.json created by VS Code.
- Determine the debug adapter to use dynamically.
- Verify or modify debug configurations before they are passed to the debug adapter.
- Communicate with the debug adapter.
- Send messages to the debug console. -->

이 문서의 남은 부분에서는 디버거 익스텐션을 개발하는 방법을 보여드리겠습니다.

<!--
In the rest of this document we show how to develop a debugger extension. -->

## 연습용 디버그 익스텐션

<!-- 
## The Mock Debug Extension -->

debug adatper 를 만드는 것은 이번 튜토리얼을 위하여 조금 무겁기 때문에, 우리가 만들었던 간단한 교육용 "debug adapter 스타터 킷" 에서 시작하겠습니다. 이는 _Mock Debug_ 라고 불리며, 이는 실제 디버거에 작용하지 않기 때문입니다. 연습용 디버그에서는 디버거를 시뮬레이션하고 진행, 중지점, 예외 변수 접근을 지원하지만, 어떤 실제 디버거와도 연결 되진 않습니다.

<!--
Since creating a debug adapter from scratch is a bit heavy for this tutorial, we will start with a simple DA which we have created as an educational "debug adapter starter kit". It is called _Mock Debug_ because it does not talk to a real debugger, but mocks one. Mock Debug simulates a debugger and supports step, continue, breakpoints, exceptions, and variable access, but it is not connected to any real debugger. -->

연습용 디버그를 개발 설치 하기에 앞서, 먼저 VS Code 마켓플레이스에서 [만들어진 버전](https://marketplace.visualstudio.com/items/andreweinand.mock-debug)을 설치하고 이를 실행해보겠습니다: 

<!--
Before delving into the development setup for mock-debug, let's first install a [pre-built version](https://marketplace.visualstudio.com/items/andreweinand.mock-debug)
from the VS Code Marketplace and play with it: -->

- 익스텐션 viewlet으로 전환하고 "mock"를 입력하여 연습용 디버그 익스텐션을 검색한 다음,
- 익스텐션을 "Install" 하고 "Reload" 하십시오. 

<!--
- Switch to the Extensions viewlet and type "mock" to search for the Mock Debug extension,
- "Install" and "Reload" the extension. -->

연습 디버그를 위해 :

<!-- To try Mock Debug:-->

- 새로운 빈 폴더 `mock test`를 만들고 이를 VS Code에서 여십시오. 
- `readme.md`라는 파일을 만들고 임의의 텍스트 내용을 입력하십시오.
- 디버그 뷰로 전환한뒤 기어아이콘을 누르십시오. 
- VS Code 는 여러분이 기본 실행 구성을 만들기 위해 "environment"를 선택하게 할 것입니다. "Mock Debug"를 선택하십시오.
- 초록색 Start 버튼을 누르고 Enter를 눌러 제안된 `readme.md` 파일을 확인하십시오. 

<!--
- Create a new empty folder `mock test` and open it in VS Code.
- Create a file `readme.md` and enter several lines of arbitrary text.
- Switch to the Debug view and press the gear icon.
- VS Code will let you select an "environment" in order to create a default launch configuration. Pick "Mock Debug".
- Press the green Start button and then Enter to confirm the suggested file `readme.md`. -->

디버그 세션이 시작되고 readme 파일을 "진행" 하거나, 중지점을 설정하고, 예외처리를 할 수 있습니다. (`exception`이라는 단어가 줄에 있다면)
A debug session starts and you can "step" through the readme file, set and hit breakpoints, and run into exceptions (if the word `exception` appears in a line).

![Mock Debugger running](images/debugger-extension/mock-debug.gif)

여러분이 직접 개발한 연습용 디버그를 사용하기 전에, 먼저 만들어진 버전을 삭제 하십시오 :

- 익스텐션 viewlet으로 전환한 다음 연습용 디버그 익스텐션의 기어 아이콘을 누르십시오.
- "Uninstall"을 실행하고 윈도우를 "Reload"하십시오. 

<!--
Before using Mock Debug as a starting point for your own development, we recommend to uninstall the pre-built version first: -->

<!--
- Switch to the Extensions viewlet and click on the gear icon of the Mock Debug extension.
- Run the "Uninstall" action and then "Reload" the window. -->

## 연습용 디버그를 위한 개발 설정

<!--
## Development Setup for Mock Debug -->

이제 연습용 디버그를 위한 소스를 받아 VS Code에서 개발을 시작합시다:

<!--
Now let's get the source for Mock Debug and start development on it within VS Code: -->

```bash
git clone https://github.com/Microsoft/vscode-mock-debug.git
cd vscode-mock-debug
npm install
```

VS Code에서 프로젝트 폴더 `vscode-mock-debug`를 여십시오. 
<!--
Open the project folder `vscode-mock-debug` in VS Code. -->

패키지 안에 무엇이 있습니까? 
<!--
What's in the package? -->

- `package.json` 은 연습용 디버그 익스텐션의 설명서입니다:
  - 연습용 디버그 익스텐션의 contribution의 목록이 쓰여있습니다.
  - `compile`과 `watch` 스크립트는 타입스크립트 소스를 `out`폴더에 transpile하고, 후속 소스 수정을 확인하기 위해 쓰입니다.
  - `vscode-debugprotocol`, `vscode-debugadapter` 그리고 `vscode-debugadapter-testsupport` 디펜던시는 노드 기반 debug adapter를 단순화 하는 NPM 모듈입니다. 
- `src/mockRuntime.ts`는 단순한 디버그 API를 가진 연습용 런타임입니다.
- _adapts_ 코드는 `src/mockDebug.ts`내부에 있는 Debug Adapter Protocol의 런타임입니다. 여러분은 여기서 DAP의 여러 요청을 위한 핸들러를 찾을 수 있습니다. 
- 디버거 익스텐션은 debug adapter 내부에서 구현되기 때문에, 익스텐션 코드를 작성할 필요가 전혀 없습니다. ( 익스텐션 호스트 프로세스에서 실행되는 코드) 그러나 연습용 디버그는 작은 `src/extension/ts`를 가지고 있는데 이는 디버거 익스텐션의 익스텐션 코드가 할 수 있는 일을 묘사하기 때문입니다. 

<!--
- `package.json` is the manifest for the mock-debug extension:
  - It lists the contributions of the mock-debug extension.
  - The `compile` and `watch` scripts are used to transpile the TypeScript source into the `out` folder and watch for subsequent source modifications.
  - The dependencies `vscode-debugprotocol`, `vscode-debugadapter`, and `vscode-debugadapter-testsupport` are NPM modules that simplify the development of node-based debug adapters.
- `src/mockRuntime.ts` is a _mock_ runtime with a simple debug API.
- The code that _adapts_ the runtime to the Debug Adapter Protocol lives in `src/mockDebug.ts`. Here you find the handlers for the various requests of the DAP.
- Since the implementation of debugger extension lives in the debug adapter, there is no need to have extension code at all (i.e. code that runs in the extension host process). However, Mock Debug has a small `src/extension.ts` because it illustrates what can be done in the extension code of a debugger extension. -->

이제 **Extension** 실행 구성을 선택하여 연습용 디버그 익스텐션을 빌드하고 `F5`를 눌러 실행하십시오. 
처음에, 이는 타입스크립트 소스를 `out`폴더로 트랜스파일 할 것입니다. 
빌드 이후에, _watcher task_ 가 시작되어 여러분이 만드는 모든 변화를 트랜스파일합니다. 

<!--
Now build and launch the Mock Debug extension by selecting the **Extension** launch configuration and hitting `F5`.
Initially, this will do a full transpile of the TypeScript sources into the `out` folder.
After the full build, a _watcher task_ is started that transpiles any changes you make. -->

소스를 트랜스파일링 한 이후, "[Extension Development Host]"라는 새로운 VS Code 창이 디버그 모드에서 실행되는 연습용 디버그 익스텐션과 나타날 것입니다. 그 창에서 여러분의 `mock test` 프로젝트의 `readme.md`파일을 열고, 'F5'로 디버그 세션을 시작한뒤 디버그를 진행 하십시오. 

<!--
After transpiling the source, a new VS Code window labelled "[Extension Development Host]" appears with the Mock Debug extension now running in debug mode. From that window open your `mock test` project with the `readme.md` file, start a debug session with 'F5', and then step through it: -->

![Debugging Extension and Server](images/debugger-extension/debug-mock-session.png)

익스텐션을 디버그 모드에서 실행 하고 있기 때문에, 여러분은 이제 `src/extension.ts` 에서 중지점을 설정할 수 있습니다 하지만 위에서 언급한대로, 익스텐션에서 실행되는 관심있는 코드는 없습니다. 관심있는 코드는 다른 프로세스인 debug adapter에서 실행됩니다. 

<!--
Since you are running the extension in debug mode, you could now set and hit breakpoints in `src/extension.ts` but as I've mentioned above, there is not much interesting code executing in the extension. The interesting code runs in the debug adapter which is a separate process. -->

debug adapter를 디버그 하기 위해서, 그것을 디버그 모드로 실행해야 합니다. 이는 _server mode_ 에서 debug adapter를 실행하고 VS Code에 연결하여 구성하는 것으로 쉽게 할 수 있습니다. 여러분의 VS Code vscode-mock-debug 프로젝트의 드롭다운 메뉴에서 구성 **Server**를 선택한 뒤 녹색 시작버튼을 누르십시오. 

<!--
In order to debug the debug adapter itself, we have to run it in debug mode. This is most easily achieved by running the debug adapter in _server mode_ and configure VS Code to connect to it. In your VS Code vscode-mock-debug project select the launch configuration **Server** from the drop down menu and press the green start button. -->

익스텐션에 대한 활성 디버그 세션이 이미 있기 때문에, 이제 VS Code 디버거 UI는 CALL STACK 뷰에 **Extension**과 **Server** 이름을 표기하는 2개의 디버그 세션을 가지는 _multi session_ 모드로 진입 합니다. 

<!--
Since we already had an active debug session for the extension the VS Code debugger UI now enters _multi session_ mode which is indicated by seeing the names of the two debug sessions **Extension** and **Server** showing up in the CALL STACK view: -->

![Debugging Extension and Server](images/debugger-extension/debugger-extension-server.png)

이제 우리는 익스텐션과 DA를 한꺼번에 디버그 할 수 있습니다.
이를 이루기 위한 더 빠른 방법은 두가지 세션을 같이 실행하는 **Extension + Server** 구성 실행을 하는 것입니다. 

<!--
Now we are able to debug both the extension and the DA simultaneously.
A faster way to arrive here is by using the **Extension + Server** launch configuration which launches both sessions automatically.-->

익스텐션과 DA를 디버그 하기 위한 더 간단한 대안을 [여기](#alternative-approach-to-develop-a-debugger-extension)에서 확인 하십시오. 

<!-- 
An alternative, even simpler approach for debugging the extension and the DA can be found [below](#alternative-approach-to-develop-a-debugger-extension).-->

`src/mockDebug.ts`파일에서 `launchRequset(...)`의 시작 부분에 중지점을 설정하고, 마지막 단계로 DA server에 연결하기 위해 `debugServer` 속성을 포트 `4711`로 연습용 디버거 설정을 구성하십시오.

<!--
Set a breakpoint at the beginning of method `launchRequest(...)` in file `src/mockDebug.ts` and as a last step configure the mock debugger to connect to the DA server by adding a `debugServer` attribute for port `4711` to your mock test launch config: -->

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "mock",
      "request": "launch",
      "name": "mock test",
      "program": "${workspaceFolder}/readme.md",
      "stopOnEntry": true,
      "debugServer": 4711
    }
  ]
}
```

이런 디버그 구성을 실행했다면, VS Code는 연습용 debug adapter를 분리 프로세스가 아닌, 돌아가는 서버의 포트 4711 에 바로 연결 할 것이며, 여러분인 `launchRequset`에 중지점을 확인 할 수 있습니다.

<!--
If you now launch this debug configuration, VS Code does not start the mock debug adapter as a separate process, but directly connects to local port 4711 of the already running server, and you should hit the breakpoint in `launchRequest`. -->

이 설정을 통해, 이제 여러분은 쉽게 수정, 트랜스파일, 디버그를 할 수 있습니다.
<!--
With this setup, you can now easily edit, transpile, and debug Mock Debug. -->

그러나 이제 진짜 작업이 시작됩니다: 여러분은 `src/mockDebug.ts`의 debug adapter의 연습용 구현을 교체 해야하며, `src/mockRuntime.ts`를 실제 디버거 혹은 런타임과 통신하는 코드로 수정해야 합니다. 이는 Debug Adapter Protocol에 대한 이해와 구현을 포함합니다. 더 자세한 정보를 [여기](https://microsoft.github.io/debug-adapter-protocol/overview#How_it_works)서 확인 할 수 있습니다. 

<!--
But now the real work begins: you will have to replace the mock implementation of the debug adapter in `src/mockDebug.ts` and `src/mockRuntime.ts` by some code that talks to a "real" debugger or runtime. This involves understanding and implementing the Debug Adapter Protocol. More details
about this can be found [here](https://microsoft.github.io/debug-adapter-protocol/overview#How_it_works).
-->

## 디버거 익스텐션의 package.json 구조

<!--
## Anatomy of the package.json of a Debugger Extension -->

debug adapter에 특정 디버거 구현을 제공하는것 외에도, 디버거 익스텐션은 다양한 debug 관련 contribution point와 연관된 `package.json`을 필요로 합니다.

<!--
Besides providing a debugger-specific implementation of the debug adapter a debugger extension needs a `package.json` that contributes to the various debug-related contributions points. -->

이제 연습용 디버그의 `package.json`을 더 깊게 봅시다.
<!--
So let's have a closer look at the `package.json` of Mock Debug. -->

다른 VS Code 익스텐션 처럼, `package.json`은 익스텐션의 **name**, **publisher** 그리고 **version** 과 같은 기본 속성을 선언합니다. **categories**필드를 사용하여 VS Code 익스텐션 마켓플레이스에서 쉽게 검색 될 수 있게 하십시오. 

<!--
Like every VS Code extension, the `package.json` declares the fundamental properties **name**, **publisher**, and **version** of the extension. Use the **categories** field to make the extension easier to find in the VS Code Extension Marketplace. -->

```json
{
  "name": "mock-debug",
  "displayName": "Mock Debug",
  "version": "0.24.0",
  "publisher": "...",
  "description": "Starter extension for developing debug adapters for VS Code.",
  "author": {
    "name": "...",
    "email": "..."
  },
  "engines": {
    "vscode": "^1.17.0",
    "node": "^7.9.0"
  },
  "icon": "images/mock-debug-icon.png",
  "categories": ["Debuggers"],

  "contributes": {
    "breakpoints": [{ "language": "markdown" }],
    "debuggers": [
      {
        "type": "mock",
        "label": "Mock Debug",

        "program": "./out/mockDebug.js",
        "runtime": "node",

        "configurationAttributes": {
          "launch": {
            "required": ["program"],
            "properties": {
              "program": {
                "type": "string",
                "description": "Absolute path to a text file.",
                "default": "${workspaceFolder}/${command:AskForProgramName}"
              },
              "stopOnEntry": {
                "type": "boolean",
                "description": "Automatically stop after launch.",
                "default": true
              }
            }
          }
        },

        "initialConfigurations": [
          {
            "type": "mock",
            "request": "launch",
            "name": "Ask for file name",
            "program": "${workspaceFolder}/${command:AskForProgramName}",
            "stopOnEntry": true
          }
        ],

        "configurationSnippets": [
          {
            "label": "Mock Debug: Launch",
            "description": "A new configuration for launching a mock debug program",
            "body": {
              "type": "mock",
              "request": "launch",
              "name": "${2:Launch Program}",
              "program": "^\"\\${workspaceFolder}/${1:Program}\""
            }
          }
        ],

        "variables": {
          "AskForProgramName": "extension.mock-debug.getProgramName"
        }
      }
    ]
  },

  "activationEvents": ["onDebug", "onCommand:extension.mock-debug.getProgramName"]
}
```

이제 특정 디버그 익스텐션에 연관된 내용을 포함한 **contributes** 부분을 보겠습니다. 

<!-- 
Now take a look at the **contributes** section which contains the contributions specific to debug extensions.-->

첫번째로, **breakpoints** contribution point를 사용하여 중지점 설정이 가능한 언어의 목록을 작성합니다. 이를 사용하지 않으면 Markdown 파일에서 중지점을 설정하는 것이 불가능 합니다. 

<!--
First, we use the **breakpoints** contribution point to list the languages for which setting breakpoints will be enabled. Without this, it would not be possible to set breakpoints in Markdown files. -->

다음은 **debuggers** 부분입니다. **type** `mock` 다음에 한 디버거가 표기 되어있습니다. 사용자는 이런 타입을 실행 구성에서 참조 할 수 있습니다. 옵션 속성인 **label**은 UI에 표시되는 디버그 타입을 표기할때 쓰일 수 있습니다. 

<!--
Next is the **debuggers** section. Here, one debugger is introduced under a debug **type** `mock`. The user can reference this type in launch configurations. The optional attribute **label** can be used to give the debug type a nice name when showing it in the UI. -->

디버그 익스텐션이 debug adapter를 사용하기 때문에, 그 코드의 상대 경로는 **program** 속성으로 주어집니다. 
익스텐션을 self-contained로 만들기 위해서, 어플리케이션은 반드시 익스텐션 폴더 내부에 있어야 합니다. 일반적으로 `out` 혹은 `bin`폴더 내부에 포함하지만 다른 이름을 써도 좋습니다.

<!--
Since the debug extension uses a debug adapter, a relative path to its code is given as the **program** attribute.
In order to make the extension self-contained the application must live inside the extension folder. By convention, we keep this applications inside a folder named `out` or `bin`, but you are free to use a different name.-->

VS Code 가 다른 플랫폼들에서 돌아가기 때문에, DA 프로그램이 다른 플랫폼들에서도 잘 지원되는지 확인해야합니다. 이를 위해서 다음과 같은 옵션이 있습니다 : 

<!--
Since VS Code runs on different platforms, we have to make sure that the DA program supports the different platforms as well. For this we have the following options: -->

1. 프로그램이 플랫폼 독립적으로 구현된경우, 예를 들어 모든 플랫폼을 지원하는 런타임에서 돌아가는 경우, **runtime** 속성에 이 런타임을 명시하십시오. 현재 VS Code는 `node`와 `mono` 런타임을 지원합니다. 위의 연습용 debug adapter가 이 방식을 사용합니다.

1. 여러분의 DA 구현이 다른 플랫폼에서 다른 실행 방식을 필요하는 경우, 아래와 같이 **program** 속성을 통해 설정하십시오. 

   ```json
   "debuggers": [{
       "type": "gdb",
       "windows": {
           "program": "./bin/gdbDebug.exe",
       },
       "osx": {
           "program": "./bin/gdbDebug.sh",
       },
       "linux": {
           "program": "./bin/gdbDebug.sh",
       }
   }]
   ```

1. 위의 방식들을 조합 하는 것 또한 가능합니다. 아래의 예시는 Mono DA의 것으로 macOS와 Linux에서는 런타임을 필요로 하는 mono 어플리케이션으로 구현되었지만 윈도우에서는 그렇지 않습니다. 

   ```json
   "debuggers": [{
       "type": "mono",
       "program": "./bin/monoDebug.exe",
       "osx": {
           "runtime": "mono"
       },
       "linux": {
           "runtime": "mono"
       }
   }]
   ```

<!--
1. If the program is implemented in a platform independent way, e.g. as program that runs on a runtime that is available on all supported platforms, you can specify this runtime via the **runtime** attribute. As of today, VS Code supports `node` and `mono` runtimes. Our Mock debug adapter from above uses this approach.

1. If your DA implementation needs different executables on different platforms, the **program** attribute can be qualified for specific platforms like this:

   ```json
   "debuggers": [{
       "type": "gdb",
       "windows": {
           "program": "./bin/gdbDebug.exe",
       },
       "osx": {
           "program": "./bin/gdbDebug.sh",
       },
       "linux": {
           "program": "./bin/gdbDebug.sh",
       }
   }]
   ```

1. A combination of both approaches is possible too. The following example is from the Mono DA which is implemented as a mono application that needs a runtime on macOS and Linux but not on Windows:

   ```json
   "debuggers": [{
       "type": "mono",
       "program": "./bin/monoDebug.exe",
       "osx": {
           "runtime": "mono"
       },
       "linux": {
           "runtime": "mono"
       }
   }]
   ```
-->

**configurationAttributes**는 이 디버거에 사용 가능한 `launch.json`속성의 schema를 선언합니다. 이 schema는 `launch.json`을 검증하고, IntelliSense를 지원하고, 설정 구성을 수정할 때 호버링 도움말을 지원할때 사용됩니다.

<!--
**configurationAttributes** declares the schema for the `launch.json` attributes that are available for this debugger. This schema is used for validating the `launch.json` and supporting IntelliSense and hover help when editing the launch configuration. -->

**initialConfigurations** 는 이 디버거를 위한 기본 `launch.json`의 초기내용을 정의합니다. 이는 프로젝트가 `launch.json`을 가지지 않고 디버그 세션을 시작하거나 디버그 viewlet에서 기어 아이콘을 클릭할때 사용됩니다. 이경우 VS Code는 사용자가 디버그 환경을 고르면 해당하는 `launch.json`을 생성합니다. 

<!--
The **initialConfigurations** define the initial content of the default `launch.json` for this debugger. This information is used when a project does not have a `launch.json` and a user starts a debug session or clicks on the gear icon in the debug viewlet. In this case VS Code lets the user pick a debug environment and then creates the corresponding `launch.json`: -->

![Debugger Quickpick](images/debugger-extension/debug-init-config.png)

`package.json`의 `launch.json` 초기 내용을 정적으로 정의 하는 대신, `DebugConfigurationProvider`를 설정하여 초기 구성을 동적으로 계산하는 것이 가능합니다. (자세한 정보는 아래 [Using a DebugConfigurationProvider](#using-a-debugconfigurationprovider)를 확인하십시오.)

<!--
Instead of defining the initial content of the `launch.json` statically in the `package.json`, it is possible to compute the initial configurations dynamically by implementing a `DebugConfigurationProvider` (for details see the section [Using a DebugConfigurationProvider below](#using-a-debugconfigurationprovider)).
-->

**configurationSnippets**는 `launch.json`을 수정할때 IntelliSense에 표기되는 실행 구성 snippets를 정의합니다. 일반적으로, snippet의 `label`속성 앞에 디버그 환경 이름을 붙여서, 많은 snippet 목록에서 쉽게 인식 될수 있도록 합니다.

<!--
**configurationSnippets** define launch configuration snippets that get surfaced in IntelliSense when editing the `launch.json`. As a convention, prefix the `label` attribute of a snippet by the debug environment name so that it can be clearly identified when presented in a list of many snippet proposals.-->

**variable** 은 "variables"를 "commands"에 바인딩 합니다. 이 변수들은 실행 구성에서 **\${comand:xyxz}** 구문을 통해 사용될 수 있으며, 디버그 세션이 시작되었을때 바인딩 된 커맨드에서 반환되는 값으로 대체 됩니다. 

<!--
The **variables** contribution binds "variables" to "commands". These variables can be used in the launch configuration using the **\${command:xyz}** syntax and the variables are substituted by the value returned from the bound command when a debug session is started.-->

구현된 커맨드는 익스텐션 내부에 존재하며, UI가 없는 단순한 표현에서 익스텐션 API에서 사용 가능한 UI기능을 기반으로 하는 복잡한 기능까지 다양합니다. 
연습용 디버그는 `AskForProgramName` 변수를 `extension.mock-debug.getProgramName`커맨드에 바인딩합니다. `src/extension.ts` 내부의 [구현](https://github.com/Microsoft/vscode-mock-debug/blob/606454ff3bd669867a38d9b2dc7b348d324a3f6b/src/extension.ts#L21-L26)된 이 커맨드는 `showInputBox`를 이용하여 사용자가 프로그램 이름을 입력 할 수 있게 합니다. 

<!--
The implementation of a command lives in the extension and it can range from a simple expression with no UI, to sophisticated functionality based on the UI features available in the extension API.
Mock Debug binds a variable `AskForProgramName` to the command `extension.mock-debug.getProgramName`. The [implementation](https://github.com/Microsoft/vscode-mock-debug/blob/606454ff3bd669867a38d9b2dc7b348d324a3f6b/src/extension.ts#L21-L26) of this command in `src/extension.ts` uses the `showInputBox` to let the user enter a program name:
-->

```ts
vscode.commands.registerCommand('extension.mock-debug.getProgramName', config => {
  return vscode.window.showInputBox({
    placeHolder: 'Please enter the name of a markdown file in the workspace folder',
    value: 'readme.md'
  });
});
```

변수는 이제 실행 구성의 모든 문자열 유형 값에서 **\${command:AskForProgramName}** 와 같이 사용 될 수 있습니다 .
<!--
The variable can now be used in any string typed value of a launch configuration as **\${command:AskForProgramName}**.-->

## DebugConfigurationProvider 사용

<!--
## Using a DebugConfigurationProvider -->

`package.json`의 정적 디버그 contributions가 충분 하지 않다면, `DebugConfigurationProvider`를 사용하 여 디버그 익스텐션의 다음과 같은 부분들을 동적으로 조절 할 수 있습니다. 

<!--
If the static nature of debug contributions in the `package.json` is not sufficient, a `DebugConfigurationProvider` can be used to dynamically control the following aspects of a debug extension: -->

- 새롭게 생성된 launch.json 을 위한 초기 디버그 구성을 동적으로 생성 할 수 있습니다. (예로 작업공간에서 사용 가능한 일부 상황, 정보를 기반으로)
- 새로운 디버그 세션을 시작하기 위해 쓰이기 전에 실행 설정을 _resolved_ (혹은 수정) 될 수 있습니다. 이는 작업 공간에서 사용 가능한 정보를 기반으로 기본 값을 채울 수 있습니다.

<!--
- The initial debug configurations for a newly created launch.json can be generated dynamically, e.g. based on some contextual information available in the workspace.
- A launch configuration can be _resolved_ (or modified) before it is used to start a new debug session. This allows for filling in default values based on information available in the workspace. -->

`src/extension.ts`의 `MockConfigurationProvider`은 디버그 세션이 launch.json 없이 시작되었지만 Markdown파일이 활성 에디터에서 열려 있는 경우를 감지하기 위해 `resolveDebugConfiguration`을 구현합니다. 이는 사용자가 에디터에 열린 파일에 대하여 launch.json을 생성하지 않고 디버그를 하려 할때의 전형적인 시나리오입니다. 

<!--
The `MockConfigurationProvider` in `src/extension.ts` implements `resolveDebugConfiguration` to detect the case where a debug session is started when no launch.json exists, but a Markdown file is open in the active editor. This is a typical scenario where the user has a file open in the editor and just wants to debug it without creating a launch.json.-->

디버그 설정 제공자는 특정 디버그 타입을 `vscode.debug.registerDebugConfigurationProvider`를 통해 등록하며, 일반적으로 익스텐션의 `activate` 함수 내부에 있습니다.
`DebugConfigurationProvier`가 이미 등록 된것을 확실히 하기 위해, 익스텐션은 디버그 기능이 시작되는 대로 활성화 되어야 합니다. 이는 `package.json`의 `onDebug`이벤트를 통해 익스텐션 활성화를 설정하여 쉽게 할 수 있습니다. 

<!--
A debug configuration provider is registered for a specific debug type via `vscode.debug.registerDebugConfigurationProvider`, typically in the extension's `activate` function.
To ensure that the `DebugConfigurationProvider` is registered early enough, the extension must be activated as soon as the debug functionality is used. This can be easily achieved by configuring extension activation for the `onDebug` event in the `package.json`: -->

```json
"activationEvents": [
    "onDebug",
    // ...
],
```

이 catch-all `onDebug`는 디버그 기능이 사용 되는 즉시 실행 됩니다. 이 작업은 익스텐션이 실행에 적은 비용을 소모할때 (예를 들어 실행 시퀀스에 많은 시간을 소모 하지 않을때)는 좋은 방법입니다. 만약 디버그 익스텐션이 실행에 높은 비용을 소모한다면, 특정 디버그 타입을 고려하지 않고 초기에 실행되기 때문에 `onDebug` 활성화 이벤트는 다른 디버그 익스텐션에 안좋은 영향을 끼칠 것입니다. 

<!--
This catch-all `onDebug` is triggered as soon as any debug functionality is used. This works fine as long as the extension has cheap startup costs (i.e. does not spend a lot of time in its startup sequence). If a debug extension has an expensive startup (for instance because of starting a language server), the `onDebug` activation event could negatively affect other debug extensions, because it is triggered rather early and does not take a specific debug type into account. -->

고비용의 디버그 익스텐션을 위한 더 나은 방법은 더 세분화된 활성화 이벤트를 사용하는 것입니다. 

<!--
A better approach for expensive debug extensions is to use more fine-grained activation events: -->

- `onDebugInitialConfigurations`는 `DebugConfigurationProvider`의 `provideDebugConfigurations` 메소드 직전에 실행 됩니다. 
- `onDebugResolve:type`는 특정 타입이 호출 되었을때 `DebugConfigurationProvider`의 `resolveDebugConfiguration` 메소드 직전에 실행 됩니다. 

<!--
- `onDebugInitialConfigurations` is fired just before the `provideDebugConfigurations` method of the `DebugConfigurationProvider` is called.
- `onDebugResolve:type` is fired just before the `resolveDebugConfiguration` method of the `DebugConfigurationProvider` for the specified type is called.
-->

**첫번째 규칙:** 만약 디버그 익스텐션 활성화 비용이 낮다면, `onDebug`를 사용하십시오. 만약 비용이 높은 경우, `DebugConfigurationProvider`가 해당하는 `provideDebugConfigurations` 나 `resolveDebugConfiguration` 메소드를 사용하는지에 따라서 `onDebugInitialConfigurations` 혹은 `onDebugResolve`를 사용하십시오.

<!--
**Rule of thumb:** If activation of a debug extensions is cheap, use `onDebug`. If it is expensive, use `onDebugInitialConfigurations` and/or `onDebugResolve` depending on whether the `DebugConfigurationProvider` implements the corresponding methods `provideDebugConfigurations` and/or `resolveDebugConfiguration`. -->

## 디버거 익스텐션 게시

<!--
## Publishing your debugger extension -->

디버거 익스텐션을 생성한 이후에는 마켓플레이스에 게시 할 수 있습니다:

<!--
Once you have created your debugger extension you can publish it to the Marketplace: -->

- `package.json`의 속성을 업데이트 하여 디버거 익스텐션의 이름과 목적을 반영하십시오.
- [Publishing Extension](/api/working-with-extensions/publishing-extension)에 설명된대로 마켓플레이스에 업로드 하십시오.

<!--
- Update the attributes in the `package.json` to reflect the naming and purpose of your debugger extension.
- Upload to the Marketplace as described in [Publishing Extension](/api/working-with-extensions/publishing-extension). -->

## 디버거 익스텐션 개발을 위한 다른 방법
<!--
## Alternative approach to develop a debugger extension -->

앞에서 본것 처럼, 디버거 익스텐션 개발은 익스텐션과 debug adapter를 2개의 병렬 세션에서 디버깅 하는것을 포함합니다. VS Code는 이를 잘 지원하지만 익스텐션과 debug adapter가 1개의 디버그 세션에서 디버그 될수 있는 1개의 프로그램이라면 더 쉬울 것입니다.
<!--
As we have seen, developing a debugger extension typically involves debugging both the extension and the debug adapter in two parallel sessions. As explained above VS Code supports this nicely but development could be easier if both the extension and the debug adapter would be one program that could be debugged in one debug session. -->

이 방법은 debug adapter가 타입스크립트 혹은 자바스크립트로 구현 되어있다면 2배로 쉬울 것입니다. 기본 아이디어는 익스텐션 호스트 내부에 서버로 debug adapter를 실행하고, 세션당 새로운 debug adapter를 실행하는 대신 VS Code에 연결하는 것입니다.

<!--
This approach is in fact easily doable as long as your debug adapter is implemented in TypeScript/JavaScript. The basic idea is to run the debug adapter as a server inside the extension host and to make VS Code to connect to it instead of launching a new debug adapter per session. -->

연습용 디버그는 [DebugAdapterDescriptorFactory](https://github.com/Microsoft/vscode-mock-debug/blob/6a2ef01b95bb22cdf55683f4d616cad501051510/src/extension.ts#L74-L98)를 이용하여 서버 기반의 debug adapter를 생성, [등록](https://github.com/Microsoft/vscode-mock-debug/blob/6a2ef01b95bb22cdf55683f4d616cad501051510/src/extension.ts#L32-L36) 하는 방법을 보여줍니다. 
이 기능은 컴파일 타임 플래그 [`EMBED_DEBUG_ADAPTER`](https://github.com/Microsoft/vscode-mock-debug/blob/6a2ef01b95bb22cdf55683f4d616cad501051510/src/extension.ts#L17)를 설정하는 것으로 가능합니다. 만약 여러분이 **F5**로 익스텐션 디버그를 시작한다면, 익스텐션 host에서뿐만 아니라 debug adapter에서도 중지점에 도달 할 수 있습니다.

<!--
Mock Debug shows how a [DebugAdapterDescriptorFactory](https://github.com/Microsoft/vscode-mock-debug/blob/6a2ef01b95bb22cdf55683f4d616cad501051510/src/extension.ts#L74-L98) can be used to create and [register](https://github.com/Microsoft/vscode-mock-debug/blob/6a2ef01b95bb22cdf55683f4d616cad501051510/src/extension.ts#L32-L36) a server-based debug adapter.
The feature is enabled by setting the compile time flag [`EMBED_DEBUG_ADAPTER`](https://github.com/Microsoft/vscode-mock-debug/blob/6a2ef01b95bb22cdf55683f4d616cad501051510/src/extension.ts#L17) to true. If you now start debugging the extension with **F5** you will hit breakpoints not only in the extension host but in the debug adapter as well.
-->
