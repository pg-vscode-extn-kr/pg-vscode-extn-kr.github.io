---
layout: default
parent: Extension Guides
title: Command
nav_order: 2
description: ""
---

# 커맨드
<!-- 
# Commands-->

커맨드는 Visual Studio Code에서 액션을 실행합니다. 만약 여러분이 [키바인딩을 구성](/docs/getstarted/keybindings)했었다면, 커맨드로 작업을 한 것입니다. 또한 커맨드는 익스텐션에서 사용자에게 기능을 접하게 하기 위해서, VS Code의 UI의 액션에 바인드 하고, 내부 로직을 구현합니다. 

<!--
Commands trigger actions in Visual Studio Code. If you have ever [configured a keybinding](/docs/getstarted/keybindings), then you've worked with commands. Commands are also used by extensions to expose functionality to users, bind to actions in VS Code's UI, and implement internal logic.-->

## 커맨드 사용하기
<!-- ## Using Commands-->

VS Code는 여러분이 에디터와 상호작용, 사용자 인터페이스를 조절, 혹은 백그라운드 작업을 할 수 있는, 많은 [빌트인 커맨드](/api/references/commands)를 포함하고 있습니다. 다수의 익스텐션은 내부 기능을 커맨드로 드러내어 사용자와 다른 익스텐션이 활용 할 수 있게 합니다. 

<!--
VS Code includes a large set of [built-in commands](/api/references/commands) that you can use to interact with the editor, control the user interface, or perform background operations. Many extensions also expose their core functionality as commands that users and other extensions can leverage. -->

### 커맨드 프로그래밍
<!--
### Programmatically executing a command -->

[`vscode.commands.executeCommand`](/api/references/vscode-api#commands.executeCommand) API로 커맨드 실행을 프로그래밍 할 수 있습니다. 이를 통해 여러분은 VS Code의 빌트인 기능을 활용하고, VS Code 빌트인 Git이나 Markdown 같은 익스텐션을 제작 할 수 있습니다. 

<!--
The [`vscode.commands.executeCommand`](/api/references/vscode-api#commands.executeCommand) API programmatically executes a command. This lets you leverage VS Code's built-in functionality, and build on extensions such as VS Code's built-in Git and Markdown extensions.-->

예를 들면, `editor.action.addCommentLine`커맨드로 텍스트 에디터에서 현재 선택된 라인에 커멘트를 달 수 있습니다.

<!--
The `editor.action.addCommentLine` command, for example, comments the currently selected lines in the active text editor:
-->

```ts
import * as vscode from 'vscode';

function commentLine() {
  vscode.commands.executeCommand('editor.action.addCommentLine');
}
```

어떤 커맨드는 사용을 위해 변수를 입력받습니다. 또한 결과물을 반환 할 수도 있습니다. 예를 들어 `vscode.executeDefinitionProvider` API와 같은 커맨드는, 주어진 위치에서의 설정을 위해 문서를 읽습니다. 이 때 문서 URI와 위치를 변수로 입력 받아, 정의 된 목록과 promise를 반환 합니다.

<!--
Some commands take arguments that control their behavior. Commands may also return a result. The API-like `vscode.executeDefinitionProvider` command, for example, queries a document for definitions at a given position. It takes a document URI and a position as arguments, and returns a promise with a list of definitions:
-->

```ts
import * as vscode from 'vscode';

async function printDefinitionsForActiveEditor() {
  const activeEditor = vscode.window.activeTextEditor;
  if (!activeEditor) {
    return;
  }

  const definitions = await vscode.commands.executeCommand<vscode.Location[]>(
    'vscode.executeDefinitionProvider',
    activeEditor.document.uri,
    activeEditor.selection.active
  );

  for (const definition of definitions) {
    console.log(definition);
  }
}
```

사용 가능 커맨드 탐색을 위해서:
<!--
To find available commands: -->

- [키보드 단축키 탐색](/docs/getstarted/keybindings)
- [VS Code의 빌트인 고급 커맨드 API 탐색](/api/references/commands)

<!--
- [Browse the keyboard shortcuts](/docs/getstarted/keybindings)
- [Look through VS Code's built-in advanced commands api](/api/references/commands)
-->

### 커맨드 URI
<!--
### Command URIs -->

커맨드 URI는 주어진 커맨드를 실행하는 링크 입니다. 텍스트 위에서 클릭 가능한 링크로 사용, 아이템의 세부내용을 완성, 혹은 웹뷰 내부에서 사용 가능합니다. 
<!--
Commands URIs are links that execute a given command. They can be used as clickable links in hover text, completion item details, or inside of webviews. -->

커맨드 URI는 커맨드 이름 이후에 `command` 를 붙여서 사용합니다. 예를 들어 `editor.action.addCommentLine` 커맨드에 해당하는 커맨드 URI는, `command:editor.action.addCommentLine` 입니다. 아래의 예시는 활성된 텍스트 에디터의 현재 줄 커멘트 링크를 띄워주는 예시입니다. 
<!--
A command URI uses the `command` scheme followed by the command name. The command URI for the `editor.action.addCommentLine` command, for example, is `command:editor.action.addCommentLine`. Here's a hover provider that shows a link in the comments of the current line in the active text editor: -->

```ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  vscode.languages.registerHoverProvider(
    'javascript',
    new class implements vscode.HoverProvider {
      provideHover(
        _document: vscode.TextDocument,
        _position: vscode.Position,
        _token: vscode.CancellationToken
      ): vscode.ProviderResult<vscode.Hover> {
        const commentCommandUri = vscode.Uri.parse(`command:editor.action.addCommentLine`);
        const contents = new vscode.MarkdownString(`[Add comment](${commentCommandUri})`);

        // To enable command URIs in Markdown content, you must set the `isTrusted` flag.
        // When creating trusted Markdown string, make sure to properly sanitize all the
        // input content so that only expected command URIs can be executed
        contents.isTrusted = true;

        return new vscode.Hover(contents);
      }
    }()
  );
}
```

커맨드에 제공되는 변수 목록은 URI 인코딩 된 JSON 의 형태입니다: 다음은 `git.stage` 커맨드를 이용하여 현재 파일의 단계를 띄워주는 예시입니다. 

<!--
The list of arguments to the command is passed as a JSON array that has been properly URI encoded: The example below uses the `git.stage` command to create a hover like that stages the current file:
-->

```ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  vscode.languages.registerHoverProvider(
    'javascript',
    new class implements vscode.HoverProvider {
      provideHover(
        document: vscode.TextDocument,
        _position: vscode.Position,
        _token: vscode.CancellationToken
      ): vscode.ProviderResult<vscode.Hover> {
        const args = [{ resourceUri: document.uri }];
        const stageCommandUri = vscode.Uri.parse(
          `command:git.stage?${encodeURIComponent(JSON.stringify(args))}`
        );
        const contents = new vscode.MarkdownString(`[Stage file](${stageCommandUri})`);
        contents.isTrusted = true;
        return new vscode.Hover(contents);
      }
    }()
  );
}
```

## 새로운 커맨드 생성
<!--
## Creating new commands-->

### 커맨드 등록
<!-- 
### Registering a command -->

[`vscode.commands.registerCommand`](/api/references/vscode-api#commands.registerCommand) 를 이용하여 커맨드의 id와 핸들러 함수를 바인딩 하십시오. 

<!--[`vscode.commands.registerCommand`](/api/references/vscode-api#commands.registerCommand) binds a command id to a handler function in your extension: -->

```ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  const command = 'myExtension.sayHello';

  const commandHandler = (name?: string = 'world') => {
    console.log(`Hello ${name}!!!`);
  };

  context.subscriptions.push(vscode.commands.registerCommand(command, commandHandler));
}
```

핸들러 함수는 `myExtension.sayHellow`커맨드가 실행 될때마다 `executeCommand`나, VS Code UI, 혹은 키바인딩을 통해서 작동 될 것입니다.

<!--
The handler function will be invoked whenever the `myExtension.sayHello` command is executed, be it programmatically with `executeCommand`, from the VS Code UI, or through a keybinding. -->

### 사용자 직면 커맨드 생성 

<!--
### Creating a user facing command
-->

`vscode.commands.registerCommand` 는 커맨드 id를 핸들러 함수에만 바인딩 합니다. 이 커맨드를 Command Pallete를 통해, 사용자가 활용 할수 있게 하기 위해서, 여러분은 익스텐션의 `package.json` 내부에 해당하는 커맨드 `contribution`을 필요로 합니다. 
<!--
`vscode.commands.registerCommand` only binds a command id to a handler function. To expose this command in the Command Palette so it is discoverable by users, you also need a corresponding command `contribution` in your extension's `package.json`: -->

```json
{
  "contributes": {
    "commands": [
      {
        "command": "myExtension.sayHello",
        "title": "Say Hello"
      }
    ]
  }
}
```

`commands` contribution은 VS Code에 여러분의 익스텐션이 특정 커맨드를 제공하는지 전달하거나, 커맨드가 UI에서 어떤식으로 보여질 지 조절 할 수 있게 합니다. 

<!-- 
The `commands` contribution tells VS Code that your extension provides a given command, and also lets you control how the command is displayed in the UI. Now our command will show up in the Command Palette:
-->

![The contributed command in the Command Palette](images/commands/palette.png)

여러분은 여전히 `registerCommand`를 호출하여, 핸들러에 커맨드 id를 바인딩해야합니다. 즉 사용자가 `myExtension.sayHellow` 커맨드를 Command Palette에서 선택하더라도 아직 익스텐션이 활성화 되지 않았기 때문에 아무일도 일어나지 않습니다. 이를 방지 하기 위해서 모든 사용자 직면 명령에 대해 익스텐션에서 `onCommand`와 `activationEvent`를 등록해야 합니다. 

<!-- 
We still need to call `registerCommand` to actually tie the command id to the handler. This means that if the user selects the `myExtension.sayHello` command from the Command Palette but our extension has not been activated yet, nothing will happen. To prevent this, extensions must register an `onCommand` `activationEvent` for all user facing commands:
-->

```json
{
  "activationEvents": ["onCommand:myExtension.sayHello"]
}
```

이제 사용자가 Command Palette나 키바인딩을 통해 `myExtension.sayHello` 커맨드를 처음으로 실행하면, 익스텐션이 활성화 되고 `registerCommand`가 `myExtension.sayHello`에 적당한 핸들러를 바인딩 할 것입니다. 

<!-- 
Now when a user first invokes the `myExtension.sayHello` command from the Command Palette or through a keybinding, the extension will be activated and `registerCommand` will bind `myExtension.sayHello` to the proper handler.
-->

여러분은 내부 커맨드에 대해서는 `onCommand` 활성 이벤트를 필요로 하지 않지만, 다음 커맨드에 대해서는 정의 해야합니다:
<!-- 
You do not need an `onCommand` activation event for internal commands but you must define them for any commands that: -->
- Command Palette를 통해 실행 가능한 경우.
- 키 바인딩을 통해 실행 가능한 경우.
- 에디터 타이틀 바와 같은 VS Code UI를 통해 실행 가능한 경우.
- 다른 익스텐션에서의 활용을 목적으로 하는 API 커맨드.

<!--
- Can be invoked using the Command Palette.
- Can be invoked using a keybinding.
- Can be invoked through the VS Code UI, such as through the editor title bar.
- Is intended as an API for other extensions to consume.
-->

### Command Palette에서 커맨드가 언제 보여질지 조절

<!--
### Controlling when a command shows up in the Command Palette -->

기본적으로, 모든 사용자 직면 커맨드는 `package.json`의 `commands` 부분을 통해 Command Palette에 보여질지 설정 됩니다. 그러나 특정 언어를 사용하는 텍스트 에디터에서, 또는 사용자가 특정 설정 옵션을 가지고 있는 경우 와 같이, 다수의 커맨드는 특정 상황에서만 사용 가능합니다.

<!-- By default, all user facing commands contributed through the `commands` section of the `package.json` show up in the Command Palette. However, many commands are only relevant in certain circumstances, such as when there is an active text editor of a given language or when the user has a certain configuration option set.-->

여러분은 [`menus.commandPalette`](/api/references/contribution-points#contributes.menus) contribution point 를 통해 커맨드가 Command Palette에 보여질지를 조절 할 수 있습니다. 이는 타겟 커맨드의 id와 언제 커맨드가 보여질지를 설정하는 [when clause](/docs/getstarted/keybindings#_when-clause-contexts)를 필요로 합니다. 

<!--
The [`menus.commandPalette`](/api/references/contribution-points#contributes.menus) contribution point lets you restrict when a command should show in the Command Palette. It takes the id of the target command and a [when clause](/docs/getstarted/keybindings#_when-clause-contexts) that controls when the command is shown: -->

```json
{
  "contributes": {
    "menus": {
      "commandPalette": [
        {
          "command": "myExtension.sayHello",
          "when": "editorLangId == markdown"
        }
      ]
    }
  }
}
```

이제 `myExtension.sayHello` 커맨드는 사용자가 Markdown 파일을 작업 하는 경우에만 Command Palette에 보여질 것입니다. 

<!--
Now the `myExtension.sayHello` command will only show up in the Command Palette when the user is in a Markdown file. -->

### 커맨드 활성화

<!-- 
### Enablement of commands -->

커맨드는 `enablement` 속성을 통해 활성화 될 수 있습니다 - [when-clause](/docs/getstarted/keybindings#_when-clause-contexts)를 값으로 가집니다. 활성화는 모든 메뉴와 등록된 키바인딩에 적용됩니다. 
<!--
Commands support enablement via an `enablement` property - its value is a [when-clause](/docs/getstarted/keybindings#_when-clause-contexts). Enablement applies to all menus and to registered keybindings. -->

**주의**: `enablement`와 메뉴 아이템의 `when` 컨디션 사이에는 의미가 중복됩니다. 후자의 경우는 메뉴가 비활성화 된 아이템으로 가득 차는 것을 방지합니다. 예를 들어, 자바스크립트의 정규표현식을 분석하는 커맨드의 경우 파일이 자바스크립트일때 (**when**) 보여져야 하며 오직 커서가 정규표현식위에 있을때만 활성화 (**enabled**) 되어야 합니다. `when`부분은 다른 언어 파일에 경우 커맨드를 숨기는 방식으로 메뉴창을 채우는 것을 방지합니다. 메뉴창을 가득 채우지 않는 것은 강력하게 추천됩니다. 

<!--
> **Note**: There is semantic overlap between `enablement` and the `when` condition of menu items. The latter is used to prevent menus full of disabled items. For example, a command that analyzes a JavaScript regular expression should show **when** the file is JavaScript and be **enabled** only when the cursor is over a regular expression. The `when` clause prevents clutter, by not showing the command for all other language files. Preventing cluttered menus is highly recommended.
-->

마지막으로, Command Palette나 context menus처럼 커맨드를 보이는 메뉴는, 활성화를 다루기 위해 다른 방식으로 구현 합니다. 에디터와 탐색기 context menus 는 활성/비활성 아이템을 렌더링하고, Command Palette 는 필터링 합니다. 

<!--
Last, menus showing commands, like the Command Palette or context menus, implement different ways of dealing with enablement. Editor and explorer context menus render enablement/disablement items while the Command Palette filters them.-->
