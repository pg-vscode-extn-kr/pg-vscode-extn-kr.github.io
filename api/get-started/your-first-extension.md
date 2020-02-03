---
layout: default
parent: Get Started
title: 당신의 첫번째 익스텐션
nav_order: 1
description: ""
---

# 당신의 첫번째 익스텐션

<!---
# Your First Extension
-->

이번 주제에서는, 익스텐션을 만들기 위한 기본 컨셉에 대해 설명합니다.
[Node.js](https://nodejs.org/en/) 와 [Git](https://git-scm.com/)을 먼저 설치 한 다음, [Yeoman](http://yeoman.io/) 과 [VS Code Extension Generator](https://www.npmjs.com/package/generator-code)가 정상적으로 설치 되어 있는지 확인해주십시오.

<!---
In this topic, we'll teach you the fundamental concepts for building extensions.
Make sure you have [Node.js](https://nodejs.org/en/) and [Git](https://git-scm.com/) installed,
then install [Yeoman](http://yeoman.io/) and [VS Code Extension Generator](https://www.npmjs.com/package/generator-code) with:)
-->

```bash
npm install -g yo generator-code
```

generator로 타입스크립트나 자바스크립트 프로젝트의 틀을 만들어 개발을 준비합니다.
generator를 실행하고 TypeScript project를 위한 몇 필드를 채우십시오:

<!---
The generator scaffolds a TypeScript or JavaScript project ready for development. Run the generator and fill out a few fields for a TypeScript project:
-->

```bash
yo code

# ? What type of extension do you want to create? New Extension (TypeScript)
# ? What's the name of your extension? HelloWorld
### Press <Enter> to choose default for all options below ###

# ? What's the identifier of your extension? helloworld
# ? What's the description of your extension? LEAVE BLANK
# ? Initialize a git repository? Yes
# ? Which package manager to use? npm

code ./helloworld
```

다음으로 에디터 안에서, `kb(workbench.action.debug.start)` 를 누르면 새로운 **Extension Development Host** 창에서 익스텐션을 컴파일, 실행 할 수 있습니다. 

<!-- 
Then, inside the editor, press `kb(workbench.action.debug.start)`. This will compile and run the extension in a new **Extension Development Host** window.
-->

`Hello World` 커맨드를 새 창의 Command Palette (`kb(workbench.action.showCommands)`)에서 실행합니다:

<!--
Run the `Hello World` command from the Command Palette (`kb(workbench.action.showCommands)`) in the new window:
-->

<video autoplay loop muted playsinline controls title="Launch your first VS Code extension video">
  <source src="https://code.visualstudio.com/api/get-started/your-first-extension/launch.mp4" type="video/mp4">
</video>

`Hello World` 알림이 뜨는걸 볼 수 있습니다. 성공했습니다 !

<!-- 
You should see the `Hello World` notification showing up. Success!
-->

## 익스텐션 개발

<!--
## Developing the extension
-->

메시지에 변화를 만들어봅시다:

<!-- 
Let's make a change to the message:
-->

- `extension.ts`파일에서 메시지를 `Hello World`에서 `Hello VS Code`로 바꾸십시오.
- 새 창에서 `Reload Window`를 실행합니다.
- `Hello World` 커맨드를 다시 실행합니다.

<!--
- Change the message from `Hello World` to `Hello VS Code` in `extension.ts`
- Run `Reload Window` in the new window
- Run the command `Hello World` again
-->

업데이트된 메시지가 올라오는걸 볼 수 있습니다.

<!--
You should see the updated message showing up.
-->

<video autoplay loop muted playsinline controls title="Reload VS Code extension video">
  <source src="https://code.visualstudio.com/api/get-started/your-first-extension/reload.mp4" type="video/mp4">
</video>

여기 몇 가지 시도 해볼만한 아이디어가 있습니다:

<!-- 
Here are some ideas for you to try:
-->

- Command Palette 에서 `Hello World` 커맨드에 새 이름을 주십시오.
- 정보 창에 현재 시간을 표기해주는 다른 커맨드에 [Contribute](/api/references/contribution-points) 하십시오.
- `vscode.window.showInformationMessage` 를 다른 [VS Code API](/api/references/vscode-api) 로 교체해서 경고 메시지를 띄워주십시오.

<!--
- Give the `Hello World` command a new name in the Command Palette.
- [Contribute](/api/references/contribution-points) another command that displays current time in an information message.
- Replace the `vscode.window.showInformationMessage` with another [VS Code API](/api/references/vscode-api) call to show a warning message.
-->

## 디버깅 익스텐션
<!--
## Debugging the extension
-->

VS Code의 자체 디버깅 기능으로 익스텐션을 쉽게 디버그 할 수 있습니다. 라인 옆을 클릭해서 VS Code가 멈추게 될 breakpoint 를 설정할 수 있습니다. 에디터에서 변수 위에 마우스를 올리거나, 왼쪽의 Debug View를 사용해서 변수의 값을 확인 할 수 있습니다. 디버그 콘솔로 expression을 계산 할 수 있습니다.

<!--
VS Code's built-in debugging functionality makes it easy to debug extensions. Set a breakpoint by clicking the gutter next to a line, and VS Code will hit the breakpoint. You can hover over variables in the editor or use the Debug View in the left to check a variable's value. The Debug Console allows you to evaluate expressions.
-->

<video autoplay loop muted playsinline controls title="Debug VS Code extension video">
  <source src="https://code.visualstudio.com/api/get-started/your-first-extension/debug.mp4" type="video/mp4">
</video>

VS Code 에서 Node.js 앱 디버깅하는걸 [Node.js Debugging Topic](/docs/nodejs/nodejs-debugging)에서 더 알아볼 수 있습니다.

<!-- 
You can learn more about debugging Node.js apps in VS Code in the [Node.js Debugging Topic](/docs/nodejs/nodejs-debugging).
-->

## 다음 단계
<!-- 
## Next steps
-->

다음 [Extension Anatomy](/api/get-started/extension-anatomy)주제에서는, `Hello World` 예제의 소스 코드를 더 자세히 보고, 주요 컨셉들을 설명합니다.

<!-- 
In the next topic, [Extension Anatomy](/api/get-started/extension-anatomy), we'll take a closer look at the source code of the `Hello World` sample and explain key concepts.
-->

이번 튜토리얼의 소스 코드는 [https://github.com/Microsoft/vscode-extension-samples/tree/master/helloworld-sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/helloworld-sample) 에서 확인 할 수 있습니다.
[Extension Guides](/api/extension-guides/overview)주제에서는, 각각 다른 VS Code API 혹은 Contribution Point를 설명하는 다른 예제들을 포함하고 있습니다.

<!-- 
You can find the source code of this tutorial at: https://github.com/Microsoft/vscode-extension-samples/tree/master/helloworld-sample. The [Extension Guides](/api/extension-guides/overview) topic contains other samples, each illustrating a different VS Code API or Contribution Point.
-->

### 자바스크립트의 경우

<!-- 
### Using JavaScript
-->

저희는 타입스크립트가 VS Code 익스텐션을 개발할 때 있어서 가장 좋은 경험을 준다고 믿기 때문에, 이 가이드에서 VS Code 익스텐션을 타입스크립트로 개발하는 방법 위주로 기술했습니다. 하지만 만약 자바스크립트를 선호 한다면, [helloworld-minimal-sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/helloworld-minimal-sample) 를 통해 계속 진행하십시오.

<!--
In this guide, we mainly describe how to develop VS Code extension with TypeScript because we believe TypeScript offers the best experience for developing VS Code extensions. However, if you prefer JavaScript, you can still follow along using [helloworld-minimal-sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/helloworld-minimal-sample).
-->
