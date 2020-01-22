---
layout: default
parent: Extension Guides
title: Webview
nav_order: 6
description: ""
---

# 웹 뷰 API

<!--
# Webview API -->

웹 뷰 API를 통해 익스텐션은 Visual Studio Code안에서 완전히 커스터마이즈 가능한 뷰를 만들 수 있습니다. 예를 들어, 빌트인 `Markdown` 익스텐션은 웹류를 사용하여 `Markdown` 프리뷰를 렌더링합니다. 웹 뷰는 또한 기본 VS Code API가 제공하는 것 이상으로 복잡한 사용자 인터페이스를 만들 수 있습니다. 

<!--
The webview API allows extensions to create fully customizable views within Visual Studio Code. For example, the built-in Markdown extension uses webviews to render Markdown previews. Webviews can also be used to build complex user interfaces beyond what VS Code's native APIs support. -->


웹 뷰는 VS Code 내부에서 익스텐션이 조절하는 `iframe`이라 생각할 수 있습니다. 웹 뷰는 대부분의 HTML 컨텐츠를 렌더링 할 수 있고, 익스텐션과 메세지 패싱을 통해서 소통이 가능합니다. 이러한 자유는 웹 뷰가 굉장히 강력하고, 익스텐션의 완전한 새로운 가능성을 제시합니다. 

<!--
Think of a webview as an `iframe` within VS Code that your extension controls. A webview can render almost any HTML content in this frame, and it communicates with extensions using message passing. This freedom makes webviews incredibly powerful, and opens up a whole new range of extension possibilities. -->

## 링크
<!--
## Links-->

- [웹 뷰 예제](https://github.com/Microsoft/vscode-extension-samples/blob/master/webview-sample/README.md)
<!--
- [Webview Sample](https://github.com/Microsoft/vscode-extension-samples/blob/master/webview-sample/README.md) -->

### VS Code API 사용
<!--
### VS Code API Usage -->

- [`window.createWebviewPanel`](/api/references/vscode-api#window.createWebviewPanel)
- [`window.registerWebviewPanelSerializer`](/api/references/vscode-api#window.registerWebviewPanelSerializer)

## 웹 뷰를 사용해야 하는가?
<!--
## Should I use a webview? -->

웹 뷰는 상당히 놀랍지만, VS Code의 기본 API로 부족할때에만 사용 되어야 합니다. 웹 뷰는 굉장히 무거운 리소스로, 보통의 익스텐션과 분리되어 실행 됩니다. 잘못 디자인된 웹 뷰는 VS Code 안에서 쉽게 드러납니다.  

<!--
Webviews are pretty amazing, but they should also be used sparingly and only when VS Code's native API is inadequate. Webviews are resource heavy and run in a separate context from normal extensions. A poorly designed webview can also easily feel out of place within VS Code. -->

웹 뷰를 사용하기 전에, 다음을 먼저 고려하십시오:

- 이 기능이 VS Code 내에서 라이브로 필요한가, 분리된 어플리케이션이나 웹사이트로 있는것은 어떠한가?

- 해당 특징을 구현하는 방법은 웹 뷰밖에 없는가, 기본 VS Code의 API를 사용 할 수는 없는가?

- 웹뷰의 추가로 인한 높은 리소스 사용을 사용자가 충분히 납득할만한가?

<!--
Before using a webview, please consider the following: -->
<!--
- Does this functionality really need to live within VS Code? Would it be better as a separate application or website?-->
<!--
- Is a webview the only way to implement your feature? Can you use the regular VS Code APIs instead? -->
<!--
- Will your webview add enough user value to justify its high resource cost? -->

기억하십시오 : 단지 여러분이 웹 뷰를 사용하여 무언가를 하고 싶다고 해서 그것이 해야하는 이유는 아닙니다. 하지만 만약 여러분이 웹 뷰를 사용해야 하는 이유가 확실하다면 이 문서가 여러분에게 도움이 될 것입니다. 

<!-- 
Remember: Just because you can do something with webviews, doesn't mean you should. However, if you are confident that you need to use webviews, then this document is here to help. Let's get started. -->

## 기초 웹 뷰 API
<!--
## Webviews API basics -->

웹 뷰 API를 설명하기 위해서, **Cat Coding**이라는 간단한 익스텐션을 제작할 것입니다. 이 익스텐션은 웹 뷰를 사용하여 고양이가 (아마 VS Code에서) 코드를 작성하는 gif를 보여줄 것입니다. API로 작업하는동안, 우리의 고양이가 얼마나 많은 소스 코드를 작성하는지 세는 기능과 고양이가 버그를 생성했을때 사용자에게 알리는 기능등을 익스텐션에 더해나갈 것입니다. 

<!--
To explain the webview API, we are going to build a simple extension called **Cat Coding**. This extension will use a webview to show a gif of a cat writing some code (presumably in VS Code). As we work through the API, we'll continue adding functionality to the extension, including a counter that keeps track of how many lines of source code our cat has written and notifications that inform the user when the cat introduces a bug. -->

다음은 **Cat Coding** 익스텐션 처음 버전의 `packages.json`입니다. 여러분은 이 어플리케이션의 완전한 코드를 [여기](https://github.com/Microsoft/vscode-extension-samples/blob/master/webview-sample/README.md) 에서 확인 할 수 있습니다. 이 익스텐션의 처음 버전은 `catCoding.start`라는 [커맨드를 작성](/api/references/contribution-points#contributes.commands) 할 것입니다. 사용자가 이 커맨드를 실행 할 때, 고양이가 들어있는 간단한 웹 뷰를 보여줄 것입니다. 사용자는 이 커맨드를 **Command Palette**에서 **Cat Coding: Start new cat coding session** 커맨드나 생성된 키바인딩을 바탕으로 실행 될 것입니다. 

<!--
Here's the `package.json` for the first version of the **Cat Coding** extension. You can find the complete code for the example app [here](https://github.com/Microsoft/vscode-extension-samples/blob/master/webview-sample/README.md). The first version of our extension [contributes a command](/api/references/contribution-points#contributes.commands) called `catCoding.start`. When a user invokes this command, we will show a simple webview with our cat in it. Users will be able to invoke this command from the **Command Palette** as **Cat Coding: Start new cat coding session** or even create a keybinding for it if they are so inclined.
-->

```json
{
  "name": "cat-coding",
  "description": "Cat Coding",
  "version": "0.0.1",
  "publisher": "bierner",
  "engines": {
    "vscode": "^1.23.0"
  },
  "activationEvents": ["onCommand:catCoding.start"],
  "main": "./out/src/extension",
  "contributes": {
    "commands": [
      {
        "command": "catCoding.start",
        "title": "Start new cat coding session",
        "category": "Cat Coding"
      }
    ]
  },
  "scripts": {
    "vscode:prepublish": "tsc -p ./",
    "compile": "tsc -watch -p ./",
    "postinstall": "node ./node_modules/vscode/bin/install"
  },
  "dependencies": {
    "vscode": "*"
  },
  "devDependencies": {
    "@types/node": "^9.4.6",
    "typescript": "^2.8.3"
  }
}
```

이제 `catCoding.start` 커맨드를 구현할 것입니다. 익스텐션의 메인 파일에서 `catCoding.start` 커맨드를 등록하고 기본 웹 뷰를 보여주기 위하여 사용 하십시오 :  
<!--
Now let's implement the `catCoding.start` command. In our extension's main file, we register the `catCoding.start` command and use it to show a basic webview: -->

```ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      // Create and show a new webview
      const panel = vscode.window.createWebviewPanel(
        'catCoding', // Identifies the type of the webview. Used internally
        'Cat Coding', // Title of the panel displayed to the user
        vscode.ViewColumn.One, // Editor column to show the new webview panel in.
        {} // Webview options. More on these later.
      );
    })
  );
}
```
`vscode.window.createWebviewPanel` 함수는 에디터 안에 웹 뷰를 생성하고 보여줄 것입니다. 아래는 여러분이 지금 상태에서 `catCoding.start` 커맨드를 실행 했을때의 결과입니다. 

<!--
The `vscode.window.createWebviewPanel` function creates and shows a webview in the editor. Here is what you see if you try running the `catCoding.start` command in its current state:
-->

![An empty webview](images/webview/basics-no_content.png)

우리 커맨드는 새로운 웹 뷰 패널을 타이틀과 함께 생성 했지만, 컨텐츠를 포함 하지 않았습니다. 고양이를 새 패널에 추가 하기 위하여, `webview.html`을 이용하여 웹 뷰 안의 HTML 컨텐츠를 설정 해야 합니다.

<!--
Our command opens a new webview panel with the correct title, but with no content! To add our cat to new panel, we also need to set the HTML content of the webview using `webview.html`: -->

```ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      // Create and show panel
      const panel = vscode.window.createWebviewPanel(
        'catCoding',
        'Cat Coding',
        vscode.ViewColumn.One,
        {}
      );

      // And set its HTML content
      panel.webview.html = getWebviewContent();
    })
  );
}

function getWebviewContent() {
  return `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cat Coding</title>
</head>
<body>
    <img src="https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif" width="300" />
</body>
</html>`;
}
```

커맨드를 다시 실행하면, 웹 뷰는 아래와 같을 것입니다:
<!--
If you run the command again, now the webview looks like this: -->

![A webview with some HTML](images/webview/basics-html.png)

<!--
Progress! -->

`webview.html`은 항상 완전한 HTML 문서여야 합니다. HTML 파편이나 에러가 난 HTML은 예기치 않은 행동을 유발 할 수 있습니다.
<!--
`webview.html` should always be a complete HTML document. HTML fragments or malformed HTML may cause unexpected behavior. -->

### 웹 뷰 컨텐츠 업데이트  

<!--
### Updating webview content -->

`webview.html`은 웹 뷰의 컨텐츠 생성 이후에도 업데이트를 할 수 있습니다. 이를 이용하여 **Cat Coding**을 더 다이나믹하게 하는 고양이의 회전 기능을 추가합시다.  

<!--
`webview.html` can also update a webview's content after it has been created. Let's use this to make **Cat Coding** more dynamic by introducing a rotation of cats: -->

```ts
import * as vscode from 'vscode';

const cats = {
  'Coding Cat': 'https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif',
  'Compiling Cat': 'https://media.giphy.com/media/mlvseq9yvZhba/giphy.gif'
};

export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      const panel = vscode.window.createWebviewPanel(
        'catCoding',
        'Cat Coding',
        vscode.ViewColumn.One,
        {}
      );

      let iteration = 0;
      const updateWebview = () => {
        const cat = iteration++ % 2 ? 'Compiling Cat' : 'Coding Cat';
        panel.title = cat;
        panel.webview.html = getWebviewContent(cat);
      };

      // Set initial content
      updateWebview();

      // And schedule updates to the content every second
      setInterval(updateWebview, 1000);
    })
  );
}

function getWebviewContent(cat: keyof typeof cats) {
  return `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cat Coding</title>
</head>
<body>
    <img src="${cats[cat]}" width="300" />
</body>
</html>`;
}
```

![Updating the webview content](images/webview/basics-update.gif)

`webview.html`을 설정하는것은 iframe을 새로 고침하는것과 유사하게, 웹 뷰의 컨텐츠를 교체합니다. 이는 매우 중요한데 여러분이 한번 웹 뷰 에서 스크립트를 사용하는경우 `webview.html`을 설정한다는것은 스크립트의 상태 또한 리셋 한다는 것을 의미하기 때문입니다. 

<!--
Setting `webview.html` replaces the entire webview content, similar to reloading an iframe. This is important to remember once you start using scripts in a webview, since it means that setting `webview.html` also resets the script's state.-->

위의 예시는 또한 `webview.title`을 설정하여 에디터의 표시되는 문서의 타이틀을 수정합니다. 타이틀의 설정은 웹 뷰를 새로 고침 하지 않습니다. 

<!--
The example above also uses `webview.title` to change the title of the document displayed in the editor. Setting the title does not cause the webview to be reloaded. -->


### 생명주기

<!--
### Lifecycle -->
  
웹 뷰 패널은 생성한 익스텐션에 소유 됩니다. 익스텐션은 `createWebviewPanel`에서 리턴된 웹 뷰를 반드시 보유 해야합니다. 만약 익스텐션이 이러하지 못하면, 해당 웹 뷰가 VS Code에서 계속 보여진다고 해도, 웹 뷰에 대한 접근권한을 다시 얻지 못할 것입니다. 

<!--
Webview panels are owned by the extension that creates them. The extension must hold onto the webview returned from `createWebviewPanel`. If your extension loses this reference, it cannot regain access to that webview again, even though the webview will continue to show in VS Code. -->

텍스트 에디터와 같이, 사용자는 웹 뷰 패널을 언제든 닫을 수 있습니다. 웹 뷰 패널이 사용자에 의해서 닫힌 경우, 웹 뷰는 파괴됩니다. 파괴된 웹뷰를 사용하려 할 경우 익셉션을 초래하며 이는 `setInterveal`을 사용한 위의 예시가 중요한 버그를 가집니다 : 사용자가 패널을 닫은 경우, `setInterval`은 계속 진행될 것이고 계속하여 `panel.webview.html`을 업데이트 하려고 시도하여 익셉션을 생성해 낼 것입니다. 고양이는 익셉션을 싫어하니 이를 수정합시다!

<!--
As with text editors, a user can also close a webview panel at any time. When a webview panel is closed by the user, the webview itself is destroyed. Attempting to use a destroyed webview throws an exception. This means that the example above using `setInterval` actually has an important bug: if the user closes the panel, `setInterval` will continue to fire, which will try to update `panel.webview.html`, which of course will throw an exception. Cats hate exceptions. Let's fix this! -->

`onDidDispose`이벤트는 웹 뷰가 파괴될 때 실행됩니다. 이 이벤트를 사용하여 웹 뷰의 리소스 사용을 정리하고, 이후의 업데이트를 취소 할 수 있습니다.

<!--
The `onDidDispose` event is fired when a webview is destroyed. We can use this event to cancel further updates and clean up the webview's resources: -->

```ts
import * as vscode from 'vscode';

const cats = {
  'Coding Cat': 'https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif',
  'Compiling Cat': 'https://media.giphy.com/media/mlvseq9yvZhba/giphy.gif'
};

export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      const panel = vscode.window.createWebviewPanel(
        'catCoding',
        'Cat Coding',
        vscode.ViewColumn.One,
        {}
      );

      let iteration = 0;
      const updateWebview = () => {
        const cat = iteration++ % 2 ? 'Compiling Cat' : 'Coding Cat';
        panel.title = cat;
        panel.webview.html = getWebviewContent(cat);
      };

      updateWebview();
      const interval = setInterval(updateWebview, 1000);

      panel.onDidDispose(
        () => {
          // When the panel is closed, cancel any future updates to the webview content
          clearInterval(interval);
        },
        null,
        context.subscriptions
      );
    })
  );
}
```

익스텐션은 또한 `dispose()`를 사용하여 프로그래밍적으로 웹 뷰를 닫을 수 있습니다. 예를 들어 만약 우리가 고양이를 5초 후에 정지 시키고 싶다면 :

<!--
Extensions can also programmatically close webviews by calling `dispose()` on them. If, for example, we wanted to restrict our cat's workday to five seconds: -->

```ts
export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      const panel = vscode.window.createWebviewPanel(
        'catCoding',
        'Cat Coding',
        vscode.ViewColumn.One,
        {}
      );

      panel.webview.html = getWebviewContent(cats['Coding Cat']);

      // After 5sec, pragmatically close the webview panel
      const timeout = setTimeout(() => panel.dispose(), 5000);

      panel.onDidDispose(
        () => {
          // Handle user closing panel before the 5sec have passed
          clearTimeout(timeout);
        },
        null,
        context.subscriptions
      );
    })
  );
}
```

### 가시성과 이동

<!--
### Visibility and Moving -->

웹 뷰 패널이 백그라운드 탭으로 이동하면, 히든 상태가 됩니다. 즉, 파괴 되지는 않지만 백그라운드에서 다시 앞으로 나오게 되면, VS Code가 자동적으로 `webview.html`의 컨텐츠를 복원 할것입니다. 

<!--
When a webview panel is moved into a background tab, it becomes hidden. It is not destroyed however. VS Code will automatically restore the webview's content from `webview.html` when the panel is brought to the foreground again: -->

![Webview content is automatically restored when the webview becomes visible again](images/webview/basics-restore.gif)

`.visible` 속성은 여러분에게 웹뷰 패널이 현재 보이는지의 여부를 알려줍니다. 

<!--
The `.visible` property tells you if the webview panel is currently visible or not. -->

익스텐션은 `reveal()`을 호출하여 프로그래밍적으로 웹 뷰 패널을 앞으로 가져 올 수 있습니다. 이 메소드는 옵션으로 타겟 뷰가 패널의 어느 칼럼에 보일지를 입력 받습니다. 웹 뷰 패널은 한번에 1개의 에디터 컬럼에서만 보일 수 있습니다. `reveal()`을 호출하거나 웹 뷰 패널을 새 에디터 칼럼에 드래그 하는것은 해당하는 칼럼으로 웹 뷰를 이동시킵니다.

<!--
Extensions can programmatically bring a webview panel to the foreground by calling `reveal()`. This method takes an optional target view column to show the panel in. A webview panel may only show in a single editor column at a time. Calling `reveal()` or dragging a webview panel to a new editor column moves the webview into that new column. -->

![Webviews are moved when you drag them between tabs](images/webview/basics-drag.gif)

익스텐션을 한번에 한개의 웹 뷰에서만 존재하게 업데이트 합시다. 만약 패널이 백그라운드에 있다면, `catCoding.start` 커맨드가 앞으로 가져올 것입니다. 

<!--
Let's update our extension to only allow a single webview to exist at a time. If the panel is in the background, then the `catCoding.start` command will bring it to the foreground: -->

```ts
export function activate(context: vscode.ExtensionContext) {
  // Track currently webview panel
  let currentPanel: vscode.WebviewPanel | undefined = undefined;

  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      const columnToShowIn = vscode.window.activeTextEditor
        ? vscode.window.activeTextEditor.viewColumn
        : undefined;

      if (currentPanel) {
        // If we already have a panel, show it in the target column
        currentPanel.reveal(columnToShowIn);
      } else {
        // Otherwise, create a new panel
        currentPanel = vscode.window.createWebviewPanel(
          'catCoding',
          'Cat Coding',
          columnToShowIn,
          {}
        );
        currentPanel.webview.html = getWebviewContent(cats['Coding Cat']);

        // Reset when the current panel is closed
        currentPanel.onDidDispose(
          () => {
            currentPanel = undefined;
          },
          null,
          context.subscriptions
        );
      }
    })
  );
}
```

아래는 작동하는 새 익스텐션 입니다. 
<!--
Here's the new extension in action: -->

![Using a single panel and reveal](images/webview/basics-single_panel.gif)

웹 뷰의 가시성이 변하거나, 혹은 웹 뷰가 새로운 칼럼으로 이동 할 때마다, `onDidChangeViewState` 이벤트가 실행 될 것입니다. 우리의 익스텐션은 이 이벤트를 사용하여 웹 뷰가 어느 칼럼에 속해있는지에 따라 고양이가 변할 수 있게 사용 할수 있습니다. 

<!--
Whenever a webview's visibility changes, or when a webview is moved into a new column, the `onDidChangeViewState` event is fired. Our extension can use this event to change cats based on which column the webview is showing in: -->

```ts
const cats = {
  'Coding Cat': 'https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif',
  'Compiling Cat': 'https://media.giphy.com/media/mlvseq9yvZhba/giphy.gif',
  'Testing Cat': 'https://media.giphy.com/media/3oriO0OEd9QIDdllqo/giphy.gif'
};

export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      const panel = vscode.window.createWebviewPanel(
        'catCoding',
        'Cat Coding',
        vscode.ViewColumn.One,
        {}
      );
      panel.webview.html = getWebviewContent(cats['Coding Cat']);

      // Update contents based on view state changes
      panel.onDidChangeViewState(
        e => {
          const panel = e.webviewPanel;
          switch (panel.viewColumn) {
            case vscode.ViewColumn.One:
              updateWebviewForCat(panel, 'Coding Cat');
              return;

            case vscode.ViewColumn.Two:
              updateWebviewForCat(panel, 'Compiling Cat');
              return;

            case vscode.ViewColumn.Three:
              updateWebviewForCat(panel, 'Testing Cat');
              return;
          }
        },
        null,
        context.subscriptions
      );
    })
  );
}

function updateWebviewForCat(panel: vscode.WebviewPanel, catName: keyof typeof cats) {
  panel.title = catName;
  panel.webview.html = getWebviewContent(cats[catName]);
}
```

![Responding to onDidChangeViewState events](images/webview/basics-ondidchangeviewstate.gif)

웹 뷰 검토 및 디버그
<!-- 
### Inspecting and debugging webviews-->

**Developer: Open Webview Developer Tools** VS Code 커맨드로 웹 뷰를 디버그 할 수 있습니다. 커맨드를 실행하면 현재 보이고 있는 웹 뷰들에 대한 개발자 도구 인스턴스를 생성 할 것입니다. 

<!--
The **Developer: Open Webview Developer Tools** VS Code command lets you debug webviews. Running the command launches an instance of Developer Tools for any currently visible webviews: -->

![Webview Developer Tools](images/webview/basics-developer_tools.png)

웹 뷰의 컨텐츠는 웹 뷰 문서안의 iframe입니다. 여러분은 개발자 도구를 사용하여 웹 뷰의 DOM을 조사, 수정 혹은 디버그 스크립트를 웹 뷰 안에서 실행 할 수 있습니다.

<!--
The contents of the webview are within an iframe inside the webview document. You can use Developer Tools to inspect and modify the webview's DOM, and debug scripts running within the webview itself.-->

만약 여러분이 웹 뷰 개발자 도구 콘솔을 사용한다면, 콘솔 패널 좌측 상단의 드롭다운에서 **active frame** 환경을 선택하는것을 잊지 마십시오.
<!--
If you use the webview Developer Tools console, make sure to select the **active frame** environment from the drop-down in the top left corner of the Console panel: -->

![Selecting the active frame](images/webview/debug-active-frame.png)

**active frame** 환경은 웹 뷰 스크립트가 실행 되는 곳입니다.
<!--
The **active frame** environment is where the webview scripts themselves are executed. -->

추가로, **Developer: Reload Webview** 커맨드는 모든 활성상태의 웹 뷰를 재로드 합니다. 이는 웹 뷰의 상태를 리셋하거나, 웹 뷰를 위한 디스크의 컨텐츠가 수정되어서 새로운 컨텐츠가 로드되길 원할때 도움이 됩니다.

<!--
In addition, the **Developer: Reload Webview** command reloads all active webviews. This can be helpful if you need to reset a webview's state, or if some webview content on disk has changed and you want the new content to be loaded. -->

## 로컬 컨텐츠 불러오기

<!--
## Loading local content -->

보안 이슈로 인해 웹 뷰는 로컬 리소스가 직접 접근 할 수 없는 분리된 곳에서 실행 됩니다. 이는 여러분의 익스텐션에서 이미지, 스타일시트나 다른 리소스혹은 사용자의 현재 작업공간에서 어떤 컨텐츠를 불러오기 위해서는 반드시 `Webview.asWebviewUri` 함수를 사용하여 로컬의 `file:` URI를 VS Code가 사용 하여 불러 올 수 있는 UIR로 변환 해야함을 의미합니다.

<!--
Webviews run in isolated contexts that cannot directly access local resources. This is done for security reasons. This means that in order to load images, stylesheets, and other resources from your extension, or to load any content from the user's current workspace, you must use the `Webview.asWebviewUri` function to convert a local `file:` URI into a special URI that VS Code can use to load a subset of local resources. -->

우리가 고양이 gif를 Giphy에서 불러오지 않고 익스텐션에 넣어 번들링하기를 원한다고 가정합시다. 이를 위해 먼저 디스크의 파일에 대한 URI를 생성하고 `asWebviewUri` 함수를 거쳐서 입력 해야 합니다.

<!--
Imagine that we want to start bundling the cat gifs into our extension rather pulling them from Giphy. To do this, we first create a URI to the file on disk and then pass these URIs through the `asWebviewUri` function: -->

```ts
import * as vscode from 'vscode';
import * as path from 'path';

export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      const panel = vscode.window.createWebviewPanel(
        'catCoding',
        'Cat Coding',
        vscode.ViewColumn.One,
        {}
      );

      // Get path to resource on disk
      const onDiskPath = vscode.Uri.file(
        path.join(context.extensionPath, 'media', 'cat.gif')
      );

      // And get the special URI to use with the webview
      const catGifSrc = panel.webview.asWebviewUri(onDiskPath);

      panel.webview.html = getWebviewContent(catGifSrc);
    })
  );
}
```

이 코드를 디버그 할경우, `catGifSrc`의 실제 값은 아마 이러 할 것입니다: 
<!--
If we debug this code, we'd see that the actual value for `catGifSrc` is something like: -->

```
vscode-resource:/Users/toonces/projects/vscode-cat-coding/media/cat.gif
```

VS Code는 이 특별한 URI를 인식하여 우리 디스크의 gif를 불러올때 사용 합니다.

<!--
VS Code understands this special URI and will use it to load our gif from the disk!-->

기본으로, 웹 뷰는 다음 위치의 리소스에만 접근 할 수 있습니다 :

- 익스텐션의 설치 디렉토리
- 사용자의 현재 활성 작업공간

<!--
By default, webviews can only access resources in the following locations: -->

<!--
- Within your extension's install directory.
- Within the user's currently active workspace. -->

`WebviewOptions.localResourceRoots`를 사용하여 추가 로컬 리소스에 대한 접근을 허가 하십시오.

<!--
Use the `WebviewOptions.localResourceRoots` to allow access to additional local resources. -->

여러분은 또한 언제나 데이터 URI를 사용하여 리소스를 웹 뷰에 바로 임베드 할 수 있습니다.

<!--
You can also always use data URIs to embed resources directly within the webview. -->

### 로컬 리소스에 대한 권한 조절
<!--
### Controlling access to local resources -->

웹 뷰는 `localResourceRoots`옵션을 통해 사용자의 머신에서 어느 리소스를 로드할지 조절 할 수 있습니다. `localResourceRoot`는 루트 URI들을 통해 어느 로컬 컨텐츠가 로드 될지를 정의합니다.

<!--
Webviews can control which resources can be loaded from the user's machine with `localResourceRoots` option. `localResourceRoots` defines a set of root URIs from which local content may be loaded. -->

`localResourceRoots`를 사용하여 **Cat Coding** 웹 뷰가 오직 익스텐션의 `media` 디렉토리 리소스만 사용하게 제한 하십시오:

<!--
We can use `localResourceRoots` to restrict **Cat Coding** webviews to only load resources from a `media` directory in our extension:-->

```ts
import * as vscode from 'vscode';
import * as path from 'path';

export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      const panel = vscode.window.createWebviewPanel(
        'catCoding',
        'Cat Coding',
        vscode.ViewColumn.One,
        {
          // Only allow the webview to access resources in our extension's media directory
          localResourceRoots: [
            vscode.Uri.file(path.join(context.extensionPath, 'media'))
          ]
        }
      );

      const onDiskPath = vscode.Uri.file(
        path.join(context.extensionPath, 'media', 'cat.gif')
      );
      const catGifSrc = panel.webview.asWebviewUri(onDiskPath);

      panel.webview.html = getWebviewContent(catGifSrc);
    })
  );
}
```

모든 로컬 리소스를 제한 하기 위해서는, `localResourceRoots`를 `[]`로 설정하십시오. 
<!-- 
To disallow all local resources, just set `localResourceRoots` to `[]`. -->

보통의 경우, 웹 뷰는 로컬 리소스를 불러오는데에 있어서 최대한 제한적이어야 합니다. 그러나 `localResourceRoots`는 보안적 이슈로 부터 완전한 보호를 제공하지 않는 다는것을 유념하십시오. 여러분의 웹 뷰가 [security best practices](#security)를 따르며, [content security policy](#content-security-policy)를 더하여 불러올수 있는 컨텐츠에 제한되게 하십시오. 

<!--
In general, webviews should be as restrictive as possible in loading local resources. However, keep in mind that `localResourceRoots` does not offer complete security protection on its own. Make sure your webview also follows [security best practices](#security), and add a [content security policy](#content-security-policy) to further restrict the content that can be loaded. -->

### 웹 뷰 컨텐츠 테마

<!--
### Theming webview content -->

웹 뷰는 CSS를 사용하여 VS Code의 현재 테마를 바탕으로 외관을 수정 할 수 있습니다. VS Code 는 테마를 3개의 카테고리로 구분하며, 각각 `body` 엘리먼트에 현재 테마를 표기하기 위해 특별한 클래스를 더합니다.

<!--
Webview can use CSS to change their appearance based on VS Code's current theme. VS Code groups themes into three categories, and adds a special class to the `body` element to indicate the current theme: -->


- `vscode-light` - 밝은 테마.
- `vscode-dark` - 다크 테마.
- `vscode-high-contrast` - 고대비 테마.

<!--
- `vscode-light` - Light themes.
- `vscode-dark` - Dark themes.
- `vscode-high-contrast` - High contrast themes.
-->

다음 CSS는 웹뷰의 텍스트 색상을 사용자의 현재 테마를 바탕으로 수정합니다.

<!--
The following CSS changes the text color of the webview based on the user's current theme: -->

```css
body.vscode-light {
  color: black;
}

body.vscode-dark {
  color: white;
}

body.vscode-high-contrast {
  color: red;
}
```

웹 뷰 어플리케이션을 개발할때, 3종류의 테마에 대해 작동하는지 확인 하십시오. 또한 시각능력에 제한이 있는 사람들도 사룡 가능하게 고대비 모드에서 웹 뷰를 테스트 하십시오. 

<!--
When developing a webview application, make sure that it works for the three types of themes. And always test your webview in high-contrast mode to make sure it will be usable by people with visual disabilities. -->

웹 뷰는 또한 [CSS variables](https://developer.mozilla.org/docs/Web/CSS/Using_CSS_variables)를 이용해 VS Code 테마 색상에 접근 할 수 있습니다. 이 변수들의 이름은 `vscode`를 앞에 붙이고 `.`를 `-`로 교체하여 사용합니다. 예로 `editor.foreground`는 `var(--vscode-editor-foreground)`가 됩니다. 

<!--
Webviews can also access VS Code theme colors using [CSS variables](https://developer.mozilla.org/docs/Web/CSS/Using_CSS_variables). These variable names are prefixed with `vscode` and replace the `.` with `-`. For example `editor.foreground` becomes `var(--vscode-editor-foreground)`: -->

```css
code {
  color: var(--vscode-editor-foreground);
}
```

사용 가능한 테마 변수는 [Theme Color Reference](/api/references/theme-color) 에서 확인 하십시오. 

<!--
Review the [Theme Color Reference](/api/references/theme-color) for the available theme variables. -->

아래의 폰트 관련 변수 또한 정의 되어 있습니다:
<!--
The following font related variables are also defined: -->


- `--vscode-editor-font-family` - 에디터 폰트 패밀리 (`editor.fontFamily` 설정의).
- `--vscode-editor-font-weight` - 에디터 폰트 weight (`editor.fontWeight` 설정의).
- `--vscode-editor-font-size` - 에디터 폰트 사이즈 (`editor.fontSize` 설정의).

<!--
- `--vscode-editor-font-family` - Editor font family (from the `editor.fontFamily` setting).
- `--vscode-editor-font-weight` - Editor font weight (from the `editor.fontWeight` setting).
- `--vscode-editor-font-size` - Editor font size (from the `editor.fontSize` setting).
-->

## 스크립트, 메세지 
<!--
## Scripts and message passing -->

웹 뷰는 iframe과 같습니다, 즉 스크립트의 실행도 가능합니다. 자바스크립트는 기본적으로 사용불가 하지만, `enableScript: true`옵션을 설정하여 가능하게 다시 설정 할 수 있습니다.

<!--
Webviews are just like iframes, which means that they can also run scripts. JavaScript is disabled in webviews by default, but it can easily re-enable by passing in the `enableScripts: true` option. -->

고양이가 작성한 소스 코드의 라인 수를 세는 기능을 추가하기 위해, 스크립트를 사용합시다. 스크립트를 실행 가는것은 매우 간단합니다, 그러나 주의할점은 이 예제는 데모 목적으로만 사용됩니다. 실제 에서는, 여러분의 웹 뷰는 항상 [content security policy](#content-security-policy)를 통해 인라인 스크립트의 사용을 금지해야합니다. 

<!--
Let's use a script to add a counter tracking the lines of source code our cat has written. Running a basic script is pretty simple, but note that this example is only for demonstration purposes. In practice, your webview should always disable inline scripts using a [content security policy](#content-security-policy): -->

```ts
import * as path from 'path';
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      const panel = vscode.window.createWebviewPanel(
        'catCoding',
        'Cat Coding',
        vscode.ViewColumn.One,
        {
          // Enable scripts in the webview
          enableScripts: true
        }
      );

      panel.webview.html = getWebviewContent();
    })
  );
}

function getWebviewContent() {
  return `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cat Coding</title>
</head>
<body>
    <img src="https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif" width="300" />
    <h1 id="lines-of-code-counter">0</h1>

    <script>
        const counter = document.getElementById('lines-of-code-counter');

        let count = 0;
        setInterval(() => {
            counter.textContent = count++;
        }, 100);
    </script>
</body>
</html>`;
}
```

![A script running in a webview](images/webview/scripts-basic.gif)

<!--
Wow! that's one productive cat. -->

웹 뷰 스크립트는 보통의 웹페이지 스크립트가 가능한 모든 일을 할 수 있습니다. 웹 뷰는 그 내부 안에서 존재하기 때문에, VS Code API로의 접근권한이 없다는것을 주의하십시오. 메세지 패싱이 필요한 이유입니다. 

<!--
Webview scripts can do just about anything that a script on a normal webpage can. Keep in mind though that webviews exist in their own context, so scripts in a webview do not have access to the VS Code API. That's where message passing comes in! -->

### 익스텐션에서 웹 뷰로의 메세지 패싱
<!--
### Passing messages from an extension to a webview -->

익스텐션은 웹 뷰로 `webview.postMessage()`를 이용하여 데이터를 전송 할 수 있습니다. 이 메소드는 모든 JSON 형태의 데이터를 웹 뷰로 보낼 수 있고 웹 뷰 에서 `message`이벤트를 통해 수신 됩니다. 

<!--
An extension can send data to its webviews using `webview.postMessage()`. This method sends any JSON serializable data to the webview. The message is received inside the webview through the standard `message` event. -->

이를 실행 하기 위해서, **Cat Coding**에 새로운 커맨드를 더하여 고양이가 코드를 리팩토링하게 (코드의 수를 줄이게) 합시다. 새로운 `catCoding.doRefactor`커맨드는 `postMessage`를 사용하여 현재 웹 뷰에 메세지를 보내고, 웹 뷰 내부에서는 `window.addEventListener('message', event => {...})` 를 통해 메시지를 처리 합니다 .

<!--
To demonstrate this, let's add a new command to **Cat Coding** that instructs the currently coding cat to refactor their code (thereby reducing the total number of lines). The new `catCoding.doRefactor` command use `postMessage` to send the instruction to the current webview, and `window.addEventListener('message', event => { ... })` inside the webview itself to handle the message: -->

```ts
export function activate(context: vscode.ExtensionContext) {
  // Only allow a single Cat Coder
  let currentPanel: vscode.WebviewPanel | undefined = undefined;

  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      if (currentPanel) {
        currentPanel.reveal(vscode.ViewColumn.One);
      } else {
        currentPanel = vscode.window.createWebviewPanel(
          'catCoding',
          'Cat Coding',
          vscode.ViewColumn.One,
          {
            enableScripts: true
          }
        );
        currentPanel.webview.html = getWebviewContent();
        currentPanel.onDidDispose(
          () => {
            currentPanel = undefined;
          },
          undefined,
          context.subscriptions
        );
      }
    })
  );

  // Our new command
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.doRefactor', () => {
      if (!currentPanel) {
        return;
      }

      // Send a message to our webview.
      // You can send any JSON serializable data.
      currentPanel.webview.postMessage({ command: 'refactor' });
    })
  );
}

function getWebviewContent() {
  return `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cat Coding</title>
</head>
<body>
    <img src="https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif" width="300" />
    <h1 id="lines-of-code-counter">0</h1>

    <script>
        const counter = document.getElementById('lines-of-code-counter');

        let count = 0;
        setInterval(() => {
            counter.textContent = count++;
        }, 100);

        // Handle the message inside the webview
        window.addEventListener('message', event => {

            const message = event.data; // The JSON data our extension sent

            switch (message.command) {
                case 'refactor':
                    count = Math.ceil(count * 0.5);
                    counter.textContent = count;
                    break;
            }
        });
    </script>
</body>
</html>`;
}
```

![Passing messages to a webview](images/webview/scripts-extension_to_webview.gif)

### 웹 뷰에서 익스텐션으로 메세지 패싱
<!--
### Passing messages from a webview to an extension -->

웹 뷰에서 익스텐션으로 메세지를 보내는 것 또한 가능합니다. 이는 웹 뷰 내부에서 특별한 VS Code API 오브젝트의 `postMessage` 함수를 통해 가능합니다. 이 VS Code API 오브젝트에 접근하기 위해, 웹 뷰 내부에서 `acquireVsCodeApi`를 호출 하십시오. 이 함수는 오직 세션별로 한번씩 실행 가능합니다. 이 메소드에서 리턴된 VS Code API 인스턴스를 사용하여 다른 사용하고자 하는 함수에 사용하십시오. 

<!--
Webviews can also pass messages back to their extension. This is accomplished using a `postMessage` function on a special VS Code API object inside the webview. To access the VS Code API object, call `acquireVsCodeApi` inside the webview. This function can only be invoked once per session. You must hang onto the instance of the VS Code API returned by this method, and hand it out to any other functions that wish to use it. -->

**Cat Coding** 웹 뷰 에서 VS Code API와 `postMessage`를 사용하여 고양이가 코드에 버그를 만들어 냈을때 알릴 수 있습니다. 

<!--
We can use the VS Code API and `postMessage` in our **Cat Coding** webview to alert the extension when our cat introduces a bug in their code: -->

```js
export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      const panel = vscode.window.createWebviewPanel(
        'catCoding',
        'Cat Coding',
        vscode.ViewColumn.One,
        {
          enableScripts: true
        }
      );

      panel.webview.html = getWebviewContent();

      // Handle messages from the webview
      panel.webview.onDidReceiveMessage(
        message => {
          switch (message.command) {
            case 'alert':
              vscode.window.showErrorMessage(message.text);
              return;
          }
        },
        undefined,
        context.subscriptions
      );
    })
  );
}

function getWebviewContent() {
  return `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cat Coding</title>
</head>
<body>
    <img src="https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif" width="300" />
    <h1 id="lines-of-code-counter">0</h1>

    <script>
        (function() {
            const vscode = acquireVsCodeApi();
            const counter = document.getElementById('lines-of-code-counter');

            let count = 0;
            setInterval(() => {
                counter.textContent = count++;

                // Alert the extension when our cat introduces a bug
                if (Math.random() < 0.001 * count) {
                    vscode.postMessage({
                        command: 'alert',
                        text: '🐛  on line ' + count
                    })
                }
            }, 100);
        }())
    </script>
</body>
</html>`;
}
```

![Passing messages from the webview to the main extension](images/webview/scripts-webview_to_extension.gif)

보안 이슈로 인해, 여러분은 반드시 VS Code API 오브젝트를 private하게 사용하여 전역에 노출 되지 않도록 해야합니다.

<!--
For security reasons, you must keep the VS Code API object private and make sure it is never leaked into the global scope. -->

## 보안
<!--
## Security -->

웹 페이지와 마찬가지로, 웹 뷰를 생성할때도 기본 보안 모범 사례를 따라야 합니다. 

<!--
As with any webpage, when creating a webview you must follow some basic security best practices. -->

### 기능 제한
<!--
### Limit capabilities -->

웹 뷰는 기능에 있어서 필요한 최소만큼으로 제한해야 합니다. 예를 들어 여러분의 웹 뷰가 스크립트를 실행 할 필요가 없다면, `enableScritps: true`를 설정 하시 마십시오. 웹 뷰가 리소스를 사용자 작업공간에서 로드할 필요가 없다면 `localResourseRoots` 를 `[vscode.Uri.file(extensionContext.extensionPath)]`나 `[]`로 설정하여 모든 로컬 리소스에 접근 하지 못하게 하십시오. 

<!--
A webview should have the minimum set of capabilities that it needs. For example, if your webview does not need to run scripts, do not set the `enableScripts: true`. If your webview does not need to load resources from the user's workspace, set `localResourceRoots` to `[vscode.Uri.file(extensionContext.extensionPath)]` or even `[]` to disallow access to all local resources. -->

### 컨텐츠 보안 정책

<!--
### Content security policy -->

[컨텐츠 보안 정책](https://developers.google.com/web/fundamentals/security/csp/)은 웹 뷰 에서 로드, 실행되는 컨텐츠를 제한합니다. 예를 들어 컨텐츠 보안 정책은 오직 허용된 스크립트 목록만이 웹 뷰 에서 실행되거나 아니면 `https`가 붙은 이미지 만을 로드 할수 있게 설정 할 수 있습니다.

<!--
[Content security policies](https://developers.google.com/web/fundamentals/security/csp/) further restrict the content that can be loaded and executed in webviews. For example, a content security policy can make sure that only a list of allowed scripts can be run in the webview, or even tell the webview to only load images over `https`. -->

컨텐츠 보안 정책을 추가 하기 위하여, `<meta http-equiv="Content-Security-Policy">`를 웹 뷰의  `<head>` 상단에 추가 하십시오. 

<!-- 
To add a content security policy, put a `<meta http-equiv="Content-Security-Policy">` directive at the top of the webview's `<head>` -->

```ts
function getWebviewContent() {
  return `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">

    <meta http-equiv="Content-Security-Policy" content="default-src 'none';">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Cat Coding</title>
</head>
<body>
    ...
</body>
</html>`;
}
```

 `default-src 'none';` 정책은 모든 컨텐츠를 제한합니다. 그 후 우리의 익스텐션이 필요로 하는 최소한의 컨텐츠만을 허용 하게 수정 할 수 있습니다.
 다음은 `https`가 붙은 이미지, 로컬 스크립트 스타일시트를 허용하는 컨텐츠 보안 정책입니다.

<!--
The policy `default-src 'none';` disallows all content. We can then turn back on the minimal amount of content that our extension needs to function. Here's a content security policy that allows loading local scripts and stylesheets, and loading images over `https`: -->

```html
<meta
  http-equiv="Content-Security-Policy"
  content="default-src 'none'; img-src ${webview.cspSource} https:; script-src ${webview.cspSource}; style-src ${webview.cspSource};"
/>
```

`${webview.cspSource}` 값은 웹 뷰 오브젝트에서 나온 값에 대한 식별자 입니다. [웹 뷰 예제](https://github.com/Microsoft/vscode-extension-samples/blob/master/webview-sample)를 통해 이 값을 어떻게 사용하는지 참조하십시오.

<!--
The `${webview.cspSource}` value is a placeholder for a value that comes from the webview object itself. See the [webview sample](https://github.com/Microsoft/vscode-extension-samples/blob/master/webview-sample) for a complete example of how to use this value. -->

이 컨텐츠 보안 정책은 또한 인라인 스크립트와 스타일을 금지 합니다. 보안정책을 위반하지 않으며 로드하기 위해 모든 인라인 스타일과 스크립트를 외부 파일로 추출하여 사용하는 것이 권장됩니다.

<!--
This content security policy also implicitly disables inline scripts and styles. It is a best practice to extract all inline styles and scripts to external files so that they can be properly loaded without relaxing the content security policy. -->

### https 를 통한 컨텐츠만을 로드

<!--
### Only load content over https -->

만약 여러분의 웹 뷰가 외부 리소스를 불러오게 허용한다면, 해당 리소스가 http가 아닌 `https`를 통해서만 로드되게 허용하는것이 강하게 권장됩니다. 위의 예시 컨텐츠 보안 정책은 이미 `https`를 통한 이미지만을 로드하게 허용합니다.

<!--
If your webview allows loading external resources, it is strongly recommended that you only allow these resources to be loaded over `https` and not over http. The example content security policy above already does this by only allowing images to be loaded over `https:`. -->

### 모든 사용자 입력 검역

<!--
### Sanitize all user input -->

보통의 웹 페이지 처럼, 웹 뷰에서 HTML을 사용하려 할때, 여러분은 모든 사용자 입력을 검토해야합니다. 허용 되지 않는 사용자 입력은 컨텐츠 주입을 허용할 것이고 이는 사용자로 하여금 보안 리스크에 노출되도록 합니다.

<!--
Just as you would for a normal webpage, when constructing the HTML for a webview, you must sanitize all user input. Failing to properly sanitize input can allow content injections, which may open your users up to a security risk. -->

다음 예시들은 반드시 검토 되야합니다:

<!--
Example values that must be sanitized: -->

- 파일 컨텐츠
- 파일과 폴더 경로
- 사용자와 작업공간 설정

<!--
- File contents.
- File and folder paths.
- User and workspace settings. -->

HTML 문자열을 만들기 위해 헬퍼 라이브러리를 사용하는것을 검토하거나, 사용자의 작업공간의 모든 컨텐츠가 검토 되어 있게 하십시오. 

<!--
Consider using a helper library to construct your HTML strings, or at least ensure that all content from the user's workspace is properly sanitized. -->

보안을 위해 검토만 고려하는 것이 아니라, 잠재적인 컨텐츠 주입 효과를 최소화 하기 위해 [컨텐츠 보안 정책](#content-security-policy)을 설정하는 것과 같은 다른 보안 예시들을 참조하십시오. 

<!--
Never rely on sanitization alone for security. Make sure to follow the other security best practices, such as having a [content security policy](#content-security-policy) to minimize the impact of any potential content injections. -->

## 지속성

<!--
## Persistence-->

표준 웹 뷰 [생활주기](#lifecycle)에서, 웹 뷰는 `createWebviewPanel`를 통해 생성되고 사용자가 닫거나, `.dispose()`를 호출 했을때 파괴 됩니다. 한편 웹 뷰의 컨텐츠는 웹 뷰가 보여질때 생성되고 백그라운드로 넘어갈때 파괴됩니다. 백그라운드로 넘어갈때의 웹 뷰의 상태정보는 소실 됩니다. 

<!--
In the standard webview [lifecycle](#lifecycle), webviews are created by `createWebviewPanel` and destroyed when the user closes them or when `.dispose()` is called. The contents of webviews however are created when the webview becomes visible and destroyed when the webview is moved into the background. Any state inside the webview will be lost when the webview is moved to a background tab. -->

이를 해결하기 위한 가장 좋은 방법은 여러분의 웹 뷰를 stateless하게 만드는 것입니다. [메세지 패싱](#passing-messages-from-a-webview-to-an-extension)을 이용하여 웹 뷰의 상태를 저장하고 다시 보여질때 이를 복구 하십시오. 

<!--
The best way to solve this is to make your webview stateless. Use [message passing](#passing-messages-from-a-webview-to-an-extension) to save off the webview's state and then restore the state when the webview becomes visible again. -->

### getState 와 setState

<!--
### getState and setState -->

웹 뷰 안에서 실행되는 스크립트는 `getState`와 `setState` 메소드를 사용하여 JSON state 오브젝트의 형태로 저장, 복원이 가능합니다. 이 state는 웹 뷰의 패널이 숨겨져 웹 뷰 컨텐츠가 파괴 되었을때에도 지속됩니다. 이 state는 웹 뷰 패널이 파괴 되었을때 같이 파괴 됩니다.

<!--
Scripts running inside a webview can use the `getState` and `setState` methods to save off and restore a JSON serializable state object. This state is persisted even the webview content itself is destroyed when a webview panel becomes hidden. The state is destroyed when the webview panel is destroyed. -->

```js
// Inside a webview script
const vscode = acquireVsCodeApi();

const counter = document.getElementById('lines-of-code-counter');

// Check if we have an old state to restore from
const previousState = vscode.getState();
let count = previousState ? previousState.count : 0;
counter.textContent = count;

setInterval(() => {
  counter.textContent = count++;
  // Update the saved state
  vscode.setState({ count });
}, 100);
```

`getState`와 `setState`는 `retainContextWhenHidden`에 비해 퍼포먼스 하락이 덜하기 때문에 state를 지속하기 위해 선호되는 방법입니다. 

<!--
`getState` and `setState` are the preferred way to persist state, as they have much lower performance overhead than `retainContextWhenHidden`. -->

### 직렬화

<!--
### Serialization -->

`WebviewPanelSerializer`를 구현하여, 여러분의 웹 뷰가 VS Code가 재시작되었을때 자동적으로 복원 될 수 있습니다. 직렬화는 `getState`와 `setStae`에서 작성되며 익스텐션이 `WebviewPanelSerializer`를 웹 뷰에 설정 했을때만 가능합니다.

<!--
By implementing a `WebviewPanelSerializer`, your webviews can be automatically restored when VS Code restarts. Serialization builds on `getState` and `setState`, and is only enabled if your extension registers a `WebviewPanelSerializer` for your webviews. -->

우리의 coding cats가 VS Code 재시작에도 지속 되기 위하여, 우선 익스텐션의 `package.json`에 `onWebviewPanel` 활성화 이벤트를 더하십시오. 

<!--
To make our coding cats persist across VS Code restarts, first add a `onWebviewPanel` activation event to the extension's `package.json`: --> 

```json
"activationEvents": [
    ...,
    "onWebviewPanel:catCoding"
]
```

이 활성화 이벤트는 익스텐션이 VS Code가 뷰 타입 `catCoding`이라는 웹 뷰를 복원 하려 할때 활성화 될 것입니다. 

<!--
This activation event ensures that our extension will be activated whenever VS Code needs to restore a webview with the viewType: `catCoding`. -->

그후 , 익스텐션의 `activate` 메소드에서 `registerWebviewPanelSerializer`를 호출하여 새로운  `WebviewPanelSerializer`를 등록하십시오.  `WebviewPanelSerializer`는 웹 뷰의 지속된 state에서 컨텐츠를 복원하는 역할을 합니다. 여기서 말하는 state는 `setState`를 통해 설정된 웹 뷰 JSON blob 컨텐츠 입니다.

<!--
Then, in our extension's `activate` method, call `registerWebviewPanelSerializer` to register a new `WebviewPanelSerializer`. The `WebviewPanelSerializer` is responsible for restoring the contents of the webview from its persisted state. This state is the JSON blob that the webview contents set using `setState`. -->

```ts
export function activate(context: vscode.ExtensionContext) {
  // Normal setup...

  // And make sure we register a serializer for our webview type
  vscode.window.registerWebviewPanelSerializer('catCoding', new CatCodingSerializer());
}

class CatCodingSerializer implements vscode.WebviewPanelSerializer {
  async deserializeWebviewPanel(webviewPanel: vscode.WebviewPanel, state: any) {
    // `state` is the state persisted using `setState` inside the webview
    console.log(`Got state: ${state}`);

    // Restore the content of our webview.
    //
    // Make sure we hold on to the `webviewPanel` passed in here and
    // also restore any event listeners we need on it.
    webviewPanel.webview.html = getWebviewContent();
  }
}
```

이제 여러분이 cat coding 패널이 열린채로 VS Code를 재시작 하여도, 패널이 자동적으로 같은 에디터 위치에 복원 될 것입니다.

<!--
Now if you restart VS Code with a cat coding panel open, the panel will be automatically restored in the same editor position. --?

### retainContextWhenHidden

매우 복잡한 UI를 가졌거나, state가 빠르게 저장 / 복원 되기 어려운 웹 뷰의 경우 여러분은 `retainContextWhenHidden` 옵션을 사용 할 수 있습니다.
이 옵션은 웹 뷰의 컨텐츠를 웹 뷰가 백그라운드로 넘어간 상태에서도 숨겨진 상태로 계속 저장되게 합니다.

<!--
For webviews with very complex UI or state that cannot be quickly saved and restored, you can instead use the `retainContextWhenHidden` option. This option makes a webview keep its content around but in a hidden state, even when the webview itself is no longer in the foreground. -->

**Cat Coding**은 복잡한 state를 가진다고 할 순 없지만, `retainContextWhenHidden`를 허용하여 이 옵션이 웹 뷰의 행동을 어떻게 바꾸는지 확인해볼 수 있습니다.  

<!--
Although **Cat Coding** can hardly be said to have complex state, let's try enabling `retainContextWhenHidden` to see how the option changes a webview's behavior: -->

```ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  context.subscriptions.push(
    vscode.commands.registerCommand('catCoding.start', () => {
      const panel = vscode.window.createWebviewPanel(
        'catCoding',
        'Cat Coding',
        vscode.ViewColumn.One,
        {
          enableScripts: true,
          retainContextWhenHidden: true
        }
      );
      panel.webview.html = getWebviewContent();
    })
  );
}

function getWebviewContent() {
  return `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cat Coding</title>
</head>
<body>
    <img src="https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif" width="300" />
    <h1 id="lines-of-code-counter">0</h1>

    <script>
        const counter = document.getElementById('lines-of-code-counter');

        let count = 0;
        setInterval(() => {
            counter.textContent = count++;
        }, 100);
    </script>
</body>
</html>`;
}
```

![persistence retrain](images/webview/persistence-retrain.gif)

웹 뷰가 히든상태였다가 다시 복원된다하더라도 카운터가 리셋되지 않는것을 확인 하십시오. `retainContextWhenHidden`를 사용하면 추가 코드를 작성하지 않아도 됩니다, 웹 뷰는 웹 브라우저의 백그라운드 탭과 유사하게 활동합니다. 스크립트와 다른 동적 컨텐츠는 중지 되지만, 웹 뷰가 다시 보일때 즉시 재개됩니다. 여러분은 `retainContextWhenHidden`가 가능하다고 해도, 히든 웹 뷰에는 메세지를 보낼 수 없습니다. 

<!--
Notice how the counter does not reset now when the webview is hidden and then restored. No extra code required! With `retainContextWhenHidden`, the webview acts similarly to a background tab in a web browser. Scripts and other dynamic content are suspended, but immediately resumed once the webview becomes visible again. You cannot send messages to a hidden webview, even when `retainContextWhenHidden` is enabled. -->

`retainContextWhenHidden`는 흥미롭지만, 이 것이 높은 메모리 사용량을 가지며 다른 지속성 기술이 사용될 수 없을때에만 사용해야 한다는 것을 유념하십시오. 
<!-- 
Although `retainContextWhenHidden` may be appealing, keep in mind that this has high memory overhead and should only be used when other persistence techniques will not work. -->

## 다음 단계

<!--
## Next steps -->

VS Code의 확장성에 관심이 있다면, 다음 주제들을 확인하십시오: 

<!-- If you'd like to learn more about VS Code extensibility, try these topics: -->

- [Extension API](/api) - 완전한 VS Code 익스텐션 API에 대해 배우십시오. 
- [Extension Capabilities](/api/extension-capabilities/overview) - VS Code를 확장하는 다른 방법을 확인하십시오. 

<!--
- [Extension API](/api) - Learn about the full VS Code Extension API.
- [Extension Capabilities](/api/extension-capabilities/overview) - Take a look at other ways to extend VS Code.
-->
