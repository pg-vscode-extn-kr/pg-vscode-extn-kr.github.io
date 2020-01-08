---
layout: default
parent: Get Started
title: 익스텐션 구조
nav_order: 2
description: ""
---

# 익스텐션 구조

지난 주제에서, 당신은 기초적인 익스텐션을 만들 수 있었습니다. 내부는 어떻게 작동할까요?

`Hello World` 익스텐션은 3가지 기능이 있습니다:

- [**Activation Event**](/api/references/activation-events) 인 [`onCommand`](/api/references/activation-events#onCommand) : `onCommand:extension.helloWorld` 을 등록하고, 유저가 `Hello World` 명령을 실행시켰을 때. 익스텐션이 활성화됩니다.
- [**Contribution Point**](/api/references/contribution-points) 인 [`contributes.commands`](/api/references/contribution-points#contributes.commands) 를 사용해서 명령어 `Hello World` 를 명령팔레트에서 이용할 수 있게 합니다. 그리고 명령어 ID `extension.helloWorld` 에 연결합니다.
- [**VS Code API**](/api/references/vscode-api) 인 [`commands.registerCommand`](/api/references/vscode-api#commands.registerCommand) 를 사용하여 등록된 명령어 ID `extension.helloWorld` 에 하나의 함수를 연결합니다.

이 세 가지 개념을 이해하는 것이 VS Code에서 익스텐션을 작성하는 데 중요합니다:

- [**Activation Events**](/api/references/activation-events): 여러분의 익스텐션이 활성화된 이벤트.
- [**Contribution Points**](/api/references/contribution-points): 여러분들이 VS Code를 확장시키기 위해 `package.json` [Extension Manifest](#extension-manifest) 에 만든 정적인 선언.
- [**VS Code API**](/api/references/vscode-api): 여러분들의 익스텐션 코드에서 불러올 수 있는 자바스크립트 APIs 모음.

일반적으로, 여러분들의 익스텐션은 VS Code의 기능을 확장하기 위해 Contribution Points과 VS Code API의 조합을 사용합니다. [Extension Capabilities Overview](/api/extension-capabilities/overview) 에 관한 내용은 여러분들의 익스텐션에 적합한 Contribution Point 와 VS Code API 를 찾는 데 도움을 줄 것입니다.
`Hello World` 예제 소스 코드를 자세히 보고 위 개념들이 어떻게 적용되었는 지 확인합시다.

<!--
# Extension Anatomy

In the last topic, you were able to get a basic extension running. How does it work under the hood?

The `Hello World` extension does 3 things:

- Registers the [`onCommand`](/api/references/activation-events#onCommand) [**Activation Event**](/api/references/activation-events): `onCommand:extension.helloWorld`, so the extension becomes activated when user runs the `Hello World` command.
- Uses the [`contributes.commands`](/api/references/contribution-points#contributes.commands) [**Contribution Point**](/api/references/contribution-points) to make the command `Hello World` available in the Command Palette, and bind it to a command ID `extension.helloWorld`.
- Uses the [`commands.registerCommand`](/api/references/vscode-api#commands.registerCommand) [**VS Code API**](/api/references/vscode-api) to bind a function to the registered command ID `extension.helloWorld`.

Understanding these three concepts is crucial to writing extensions in VS Code:

- [**Activation Events**](/api/references/activation-events): events upon which your extension becomes active.
- [**Contribution Points**](/api/references/contribution-points): static declarations that you make in the `package.json` [Extension Manifest](#extension-manifest) to extend VS Code.
- [**VS Code API**](/api/references/vscode-api): a set of JavaScript APIs that you can invoke in your extension code.

In general, your extension would use a combination of Contribution Points and VS Code API to extend VS Code's functionality. The [Extension Capabilities Overview](/api/extension-capabilities/overview) topic helps you find the right Contribution Point and VS Code API for your extension.

Let's take a closer look of `Hello World` sample's source code and see how these concepts apply to it.
-->

## 익스텐션 파일 구조

```
.
├── .vscode
│   ├── launch.json     // 익스텐션을 실행하고 디버깅하기 위한 설정
│   └── tasks.json      // 타입스크립트를 컴파일하는 빌드 작업을 위한 설정
├── .gitignore          // 빌드결과와 node_module을 무시한다
├── README.md           // 여러분의 익스텐션 기능에 대한 읽을 수 있는 설명
├── src
│   └── extension.ts    // 익스텐션 소스코드
├── package.json        // 익스텐션 manifest
├── tsconfig.json       // 타입스크립트 환경 설정
```

여러분은 환경 설정 파일에 대한 더 많은 내용을 읽어 볼 수 있습니다:

- `launch.json` 은 VS Code [Debugging](/docs/editor/debugging) (디버깅)을 위해 사용합니다.
- `tasks.json` 는 VS Code [Tasks](/docs/editor/tasks) 를 정의하는 데 사용합니다.
- `tsconfig.json` 는 타입스크립트 [Handbook](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) 을 사용합니다

그러나, `package.json` 과 `extensions.ts` 에 집중하도록 합시다. 왜냐하면, 이 두 가지가 `Hello World` 익스텐션을 이해하는 데 필수적이기 때문입니다.

<!--
## Extension File Structure

```
.
├── .vscode
│   ├── launch.json     // Config for launching and debugging the extension
│   └── tasks.json      // Config for build task that compiles TypeScript
├── .gitignore          // Ignore build output and node_modules
├── README.md           // Readable description of your extension's functionality
├── src
│   └── extension.ts    // Extension source code
├── package.json        // Extension manifest
├── tsconfig.json       // TypeScript configuration
```

You can read more about the configuration files:

- `launch.json` used to configure VS Code [Debugging](/docs/editor/debugging)
- `tasks.json` for defining VS Code [Tasks](/docs/editor/tasks)
- `tsconfig.json` consult the TypeScript [Handbook](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

However, let's focus on `package.json` and `extensions.ts`, which are essential to understanding the `Hello World` extension.
-->


### Extension Manifest(익스텐션 매니페스트)

각각의 VS Code 익스텐션은 하나의 `package.json` 을 [Extension Manifest](/api/references/extension-manifest) 로서 가지고 있어야 합니다.  `package.json` 은 Node.js 의 `scripts` 와 `dependencies` 부분과 VS Code의 특정 필드인 `publisher`, `activationEvents`, `contributes` 부분들을 모두 가지고 있습니다. 여러분들은 [Extension Manifest Reference](/api/references/extension-manifest) 에서 모든 VS Code의 특정 필드에 대한 모든 설명을 찾을 수 있습니다. 여기에는 가장 중요한 몇 개의 필드들이 있습니다:

- `name` 와 `publisher`: VS Code는 `<publisher>.<name>` 을 익스텐션을 위한 유일한 ID로 사용합니다. 예를 들어, Hello World 예제는 ID `vscode-samples.helloworld-sample` 를 가지고 있습니다. VS Code 여러분의 익스텐션이라는 것을 확실하게 증명하기 위해 ID를 사용합니다.
- `main`: 익스텐션의 시작점.
- `activationEvents` 와 `contributes`: [Activation Events](/api/references/activation-events) 와 [Contribution Points](/api/references/contribution-points).
- `engines.vscode`: 이것은 익스텐션이 의존하는 VS Code API의 최소 버전을 명시합니다.
- `postinstall` 스크립트: 이것은 `engines.vscode`에 명시한 VS Code API의 1.34.0 버전을 설치합니다. 일단 `vscode.d.ts` 파일이 `node_modules/vscode/vscode.d.ts` 에 다운로드 되고 난 뒤, 여러분은 모든 VS Code API에 대한 인텔리센스, 정의로 이동하는 기능, 에러 체크 기능을 사용할 수 있습니다.

<!--
### Extension Manifest

Each VS Code extension must have a `package.json` as its [Extension Manifest](/api/references/extension-manifest). The `package.json` contains a mix of Node.js fields such as `scripts` and `dependencies` and VS Code specific fields such as `publisher`, `activationEvents` and `contributes`. You can find description of all VS Code specific fields in [Extension Manifest Reference](/api/references/extension-manifest). Here are some most important fields:

- `name` and `publisher`: VS Code uses `<publisher>.<name>` as a unique ID for the extension. For example, the Hello World sample has the ID `vscode-samples.helloworld-sample`. VS Code uses the ID to uniquely identify your extension
- `main`: The extension entry point.
- `activationEvents` and `contributes`: [Activation Events](/api/references/activation-events) and [Contribution Points](/api/references/contribution-points).
- `engines.vscode`: This specifies the minimum version of VS Code API that the extension depends on.
- The `postinstall` script: This would install the 1.34.0 version of VS Code API as specified in `engines.vscode`. Once the `vscode.d.ts` file is downloaded to `node_modules/vscode/vscode.d.ts`, you will get IntelliSense, jump to definition and error checking for all usage of VS Code API.
-->

```json
{
  "name": "helloworld-sample",
  "displayName": "helloworld-sample",
  "description": "HelloWorld example for VS Code",
  "version": "0.0.1",
  "publisher": "vscode-samples",
  "repository": "https://github.com/Microsoft/vscode-extension-samples/helloworld-sample",
  "engines": {
    "vscode": "^1.34.0"
  },
  "categories": ["Other"],
  "activationEvents": ["onCommand:extension.helloWorld"],
  "main": "./out/extension.js",
  "contributes": {
    "commands": [
      {
        "command": "extension.helloWorld",
        "title": "Hello World"
      }
    ]
  },
  "scripts": {
    "vscode:prepublish": "npm run compile",
    "compile": "tsc -p ./",
    "watch": "tsc -watch -p ./",
    "postinstall": "node ./node_modules/vscode/bin/install",
  },
  "devDependencies": {
    "@types/node": "^8.10.25",
    "@types/vscode": "^1.34.0",
    "tslint": "^5.16.0",
    "typescript": "^3.4.5"
  }
}
```
## 익스텐션 시작 파일

익스텐션 파일은 두 개의 함수 `activate` and `deactivate` 로 익스포트 됩니다. `activate`은 여러분이 등록한 **Activation Event** 이 발생했을 때 실행됩니다. `deactivate`는 여러분의 익스텐션이 비활성화 되기 전에, 정리할 기회를 제공합니다.

[`vscode`](https://www.npmjs.com/package/vscode) 모듈은 `node ./node_modules/vscode/bin/install` 에 위치한 하나의 스크립트를 가지고 있습니다. 이 스크립트는 `package.json` 파일 안에 `engines.vscode` 필드에 정의된 VS Code API 정의 파일을 가져옵니다. 이 스크립트를 실행한 후에, 여러분들은 인텔리센스, 코드에서 정의로 이동하는 기능, 다른 타입스크립트 언어 특징을 얻게 됩니다.

```ts
// 'vscode' 모듈은 VS Code 확장 API를 포함합니다.
// 아래의 코드에서 모듈을 가져오고, vscode라는 이름으로 참조합니다.
import * as vscode from 'vscode';

// 이 함수 익스텐션이 활성화 되었을 때 호출됩니다.
// 여러분의 익스텐션은 명령이 실행된 바로 첫 순간에 활성화 됩니다.
export function activate(context: vscode.ExtensionContext) {
  // 로그 (console.log) 와 에러 (console.error)를 출력하기 위해 콘솔을 사용합니다.
  // 이 줄의 코드는 익스텐션이 활성화 되었을 때, 오직 한 번만 실행될 것입니다.
  console.log('Congratulations, your extension "helloworld-sample" is now active!');

  // 명령어는 the package.json 파일에 정의되었습니다.
  // 이제 registerCommand 명령어의 구현이 있습니다.
  // commandId 파라미터는 package.json 에 있는 명렁어 필드와 매칭되어야 합니다.
  let disposable = vscode.commands.registerCommand('extension.helloWorld', () => {
    // 여러분이 여기에 쓴 코드는 명령이 실행될 때마다 실행될 것입니다.

    // 유저에게 메시지 박스를 보여줍니다.
    vscode.window.showInformationMessage('Hello World!');
  });

  context.subscriptions.push(disposable);
}

// 이 함수는 여러분의 익스텐션이 비활성화되었을 때 호출됩니다.
export function deactivate() {}
```

<!--
## Extension Entry File

The extension entry file exports two functions, `activate` and `deactivate`. `activate` is executed when your registered **Activation Event** happens. `deactivate` gives you a chance to clean up before your extension becomes deactivated.

The [`vscode`](https://www.npmjs.com/package/vscode) module contains a script located at `node ./node_modules/vscode/bin/install`. The script pulls the VS Code API definition file depending on the `engines.vscode` field in `package.json`. After running the script, you would get IntelliSense, jump to definition and other TypeScript language features in your code.

```ts
// The module 'vscode' contains the VS Code extensibility API
// Import the module and reference it with the alias vscode in your code below
import * as vscode from 'vscode';

// this method is called when your extension is activated
// your extension is activated the very first time the command is executed
export function activate(context: vscode.ExtensionContext) {
  // Use the console to output diagnostic information (console.log) and errors (console.error)
  // This line of code will only be executed once when your extension is activated
  console.log('Congratulations, your extension "helloworld-sample" is now active!');

  // The command has been defined in the package.json file
  // Now provide the implementation of the command with registerCommand
  // The commandId parameter must match the command field in package.json
  let disposable = vscode.commands.registerCommand('extension.helloWorld', () => {
    // The code you place here will be executed every time your command is executed

    // Display a message box to the user
    vscode.window.showInformationMessage('Hello World!');
  });

  context.subscriptions.push(disposable);
}

// this method is called when your extension is deactivated
export function deactivate() {}
```
-->
