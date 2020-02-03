---
layout: default
parent: Extension Guides
title: Virtual Documents
nav_order: 7
description: ""
---

# 가상 문서

<!--# Virtual Documents -->

텍스트 문서 컨텐츠 제공 API는 여러분이 읽기 전용 문서를 임의의 소스로부터 Visual Studio Code를 통해 생성 할 수 있게 합니다. 소스코드가 제공된 예시 익스텐션을 여기서 확인하십시오: [https://github.com/Microsoft/vscode-extension-samples/blob/master/virtual-document-sample/README.md](https://github.com/Microsoft/vscode-extension-samples/blob/master/virtual-document-sample/README.md)

<!--
The text document content provider API allows you to create readonly documents in Visual Studio Code from arbitrary sources. You can find a sample extension with source code at: https://github.com/Microsoft/vscode-extension-samples/blob/master/virtual-document-sample/README.md-->

## 텍스트 문서 컨텐츠 제공기

<!--
## TextDocumentContentProvider -->

API는 제공기에 쓰일 uri-scheme을 입력받아 텍스트 컨텐츠를 돌려주는 방식으로 작동합니다. scheme은 제공기를 등록할때 반드시 제공되어야 하며, 이 후에는 변경이 불가능 합니다. 같은 제공기는 여러 종류에 scheme에 대해 사용 할 수 있고, 여러개의 제공기를 1개의 scheme에 등록 할 수도 있습니다.

<!--
The API works by claiming an uri-scheme for which your provider then returns text contents. The scheme must be provided when registering a provider and cannot change afterwards. The same provider can be used for multiple schemes and multiple providers can be registered for a single scheme. -->

```ts
vscode.workspace.registerTextDocumentContentProvider(myScheme, myProvider);
```
 
 `registerTextDocumentContentProvider`를 호출하는 것은 등록 취소 가능한 임시변수를 리턴합니다. 제공기는 반드시 uri, 취소 토큰과 같이 호출되는  `provideTextDocumentContent` 함수를 구현해야 합니다. 

<!--
Calling `registerTextDocumentContentProvider` returns a disposable with which the registration can be undone. A provider must only implement the `provideTextDocumentContent`-function which is called with an uri and cancellation token. -->

```ts
const myProvider = class implements vscode.TextDocumentContentProvider {
  provideTextDocumentContent(uri: vscode.Uri): string {
    // simply invoke cowsay, use uri-path as text
    return cowsay.say({ text: uri.path });
  }
};
```

제공기가 가상문서에 쓰일 uri를 생성하지 않는지 유념하십시오 - 이 역할은 uri와 같이 주어진 것에 대해 컨텐츠를 **제공** 하는 것입니다. 리턴시, 컨텐츠 제공기는 열려있는 문서 로직에 조합되어 제공기가 항상 고려되게 합니다. 

<!--
Note how the provider doesn't create uris for virtual documents - its role is to **provide** contents given such an uri. In return, content providers are wired into the open document logic so that providers are always considered. -->

이 예시는 `cowsay` 커맨드를 사용하여 에디터가 보여줄 uri를 만듭니다. 

<!--
This sample uses a 'cowsay'-command that crafts an uri which the editor should then show: -->

```ts
vscode.commands.registerCommand('cowsay.say', async () => {
  let what = await vscode.window.showInputBox({ placeHolder: 'cow say?' });
  if (what) {
    let uri = vscode.Uri.parse('cowsay:' + what);
    let doc = await vscode.workspace.openTextDocument(uri); // calls back into the provider
    await vscode.window.showTextDocument(doc, { preview: false });
  }
});
```

입력을 위한 명령프롬프트는, `cowsay`-sheme 의 uri를 생성하고, uri에 해당하는 문서를 열고, 마지막으로 문서를 위한 에디터를 엽니다. 3번째 단계에서, 문서를 여는동안 제공기는 uri에 해당하는 컨텐츠를 요구받게 됩니다. 

<!--
The command prompts for input, creates an uri of the `cowsay`-scheme, opens a document for the uri, and finally opens an editor for that document. In step 3, opening the document, the provider is being asked to provide contents for that uri. -->

이를 통해 우리는 완벽하게 기능적인 텍스트 문서 컨텐츠 제공기를 가지게 되었습니다. 다음 주제에서는 가상문서가 어떤 방식으로 업데이트 되고, 가상문서를 위한 UI 커맨드가 어떻게 등록되는지 알아볼 것 입니다.

<!--
With this we have a fully functional text document content provider. The next sections describe how virtual documents can be updated and how UI commands can be registered for virtual documents. -->

### 가상문서 업데이트

<!--
### Update Virtual Documents -->

가상문서 업데이트 시나리오에 의존합니다. 이를 위해서 제공기는 `onDidChange` 이벤트를 구현 할 수 있습니다. 

<!--
Depending on the scenario virtual documents might change. To support that, providers can implement a `onDidChange`-event. -->

`vscode.Event` 타입은 VS Code에서 이벤트 발생을 설정합니다. 이벤트를 구현하는 가장 쉬운 방법은 다음과 같이 `vscode.EventEmitter`입니다:

<!--
The `vscode.Event`-type defines the contract for eventing in VS Code. The easiest way to implement an event is `vscode.EventEmitter`, like so: -->

```ts
const myProvider = class implements vscode.TextDocumentContentProvider {
  // emitter and its event
  onDidChangeEmitter = new vscode.EventEmitter<vscode.Uri>();
  onDidChange = this.onDidChangeEmitter.event;

  //...
};
```

이벤트 이미터는 문서에 변화가 생겼을때 VS Code에 알리기 위해 쓰일 수 있는 `fire`라는 메소드를 가지고 있습니다. 변경이 생긴 문서는 그 uri가 `fire`메소드에 사용되어 인식 됩니다. 제공기는, 문서가 여전히 열려있다는 가정하에, 업데이트된 컨텐츠를 제공하기 위해 다시 호출 될 것입니다. 

<!--
The event emitter has a `fire` method which is can be used to notify VS Code when a change has happened in a document. The document which has changed is identified by its uri given as argument to the `fire` method. The provider will then be called again to provide the updated content, assuming the document is still open. -->

이것이 VS Code 가 가상 문서에서 변경을 감지하기위해 필요한 전부 입니다. 이 기능을 사용한 더 복잡한 예시를 이곳에서 참조하십시오 : [https://github.com/Microsoft/vscode-extension-samples/blob/master/contentprovider-sample/README.md](https://github.com/Microsoft/vscode-extension-samples/blob/master/contentprovider-sample/README.md)

<!--
That's all what's needed to make VS Code listen for changes of virtual document. To see a more complex example making use of this feature, look at: https://github.com/Microsoft/vscode-extension-samples/blob/master/contentprovider-sample/README.md -->

### 에디터 커맨드 추가

<!--
### Add Editor Commands -->

에디터 액션은 연관된 컨텐츠 제공기가 제공된 문서에만 추가 될 수 있습니다. 아래는 cow가 말한것을 역전환하는 예시 커맨드 입니다. 

<!--
Editor actions can be added which only interact with documents provided by an associated content provider. This is a sample command that reverses what the cow just said: -->

```ts
// register a command that updates the current cowsay
subscriptions.push(
  vscode.commands.registerCommand('cowsay.backwards', async () => {
    if (!vscode.window.activeTextEditor) {
      return; // no editor
    }
    let { document } = vscode.window.activeTextEditor;
    if (document.uri.scheme !== myScheme) {
      return; // not my scheme
    }
    // get path-components, reverse it, and create a new uri
    let say = document.uri.path;
    let newSay = say
      .split('')
      .reverse()
      .join('');
    let newUri = document.uri.with({ path: newSay });
    await vscode.window.showTextDocument(newUri, { preview: false });
  })
);
```

위의 snippet은 우리가 활성된 에디터를 가지고 있는것과, 그것의 문서가 scheme 중 하나 인것을 확인합니다. 커맨드는 모두에게 사용가능하기 때문에 (실행가능하기도) 이러한 확인이 필요합니다. 그후 uri의 경로 부분이 역전환 되어 새로운 uri로 생성되고, 마지막으로 에디터가 열립니다.

<!--
The snippet above checks that we have an active editor and that its document is one of our scheme. These checks are needed because commands are available (and executable) to everyone. Then the path-component of the uri is reversed and a new uri is created from it, last an editor is opened. -->

에디터 커맨드로 작업을 시작하기 위해서는 `package.json`에 선언이 필요합니다. `contributes` 부분에 다음 설정을 추가하십시오:

<!--
To top things with an editor command a declarative part in `package.json` is needed. In the `contributes`-section add this config: -->

```json
"menus": {
  "editor/title": [
    {
      "command": "cowsay.backwards",
      "group": "navigation",
      "when": "resourceScheme == cowsay"
    }
  ]
}
```

위의 예시는 `contributes/commands`에 정의된 `cowsay.backwards` 커맨드를 참조하며, 이 커맨드가 에디터 타이틀 메뉴에 (우측 상단의 툴바) 위치 해야 됨을 명시합니다. 동시에 커맨드가 모든 에디터에 보여진다는 것을 의미하기 때문에 `when` 구문이 사용 되어야 합니다, 이는 액션을 보이기 위해서 어떤 컨디션을 만족시켜야 하는지를 설정합니다. 이 샘플에서는 에디터의 문서 scheme이 반드시 `cowsay` scheme 일때라는 컨디션을 설정했습니다. 이 설정은 `commandPalette`메뉴에서 반복되며 기본으로는 모든 커맨드를 표시합니다. 

<!--
This references the `cowsay.backwards`-command that defined in the `contributes/commands`-section and says it should appear in the editor title menu (the toolbar in the upper right corner). Now, just that would mean the command always shows, for every editor. That's what the `when`-clause is used for - it describes what condition must be true to show the action. In this sample it states that the scheme of the document in the editor must be the `cowsay`-scheme. The configuration is then repeated for the `commandPalette`-menu - it shows all commands by default. -->

![cowsay-bwd](images/virtual-documents/cowsay-bwd.png)

### 이벤트와 가시성

<!--
### Events and Visibility -->

문서 제공기는 VS Code에서의 1등급 시민입니다, 컨텐츠는 일반 텍스트 문서에 보여지며, 파일과 같은 인프라를 사용합니다. 하지만, 이는 "여러분의" 문서를 숨길 수 없다는것을 의미하며, `onDidOpenTextDocument`와 `onDidCloseTextDocument` 이벤트를 통해 보여진다는 것과, `vscode.workspace.textDocuments`에 소속 된다는 것을 의미합니다. 모두를 위한 규칙은 문서의 `scehe`을 확인하는 다음 문서로 혹은 문서를 위해 할 것을 정하는 것입니다. 

<!--
Document providers are first class citizens in VS Code, their contents appears in regular text documents, they use the same infrastructure as files etc. However, that also means that "your" documents cannot hide, they will appear in `onDidOpenTextDocument` and `onDidCloseTextDocument`-events, they are part of `vscode.workspace.textDocuments` and more. The rule for everyone is check the `scheme` of documents and then decide if you want to do something with/for the document. -->

### 파일 시스템 API

<!--
### File System API -->

더 강하고 유연한 사용을 위해서, [`FileSystemProvider`](/api/references/vscode-api#FileSystemProvider) API를 참조하십시오. 이는 텍스트뿐 아니라 파일, 폴더, 바이너리 데이터, 파일의 생성과 삭제 등 모든 것을 포함합니다.

<!--
If you need more flexibility and power take a look at the [`FileSystemProvider`](/api/references/vscode-api#FileSystemProvider) API. It allows to implement a full file system, having files, folders, binary data, file-deletion, creation and more. -->
