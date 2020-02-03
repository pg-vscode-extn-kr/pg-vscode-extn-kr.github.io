---
layout: default
parent: Language Extensions
title: Programmatic Language Features
nav_order: 5
description: ""
---

# 프로그래밍 언어 기능
<!--
# Programmatic Language Features -->

프로그래밍 언어 기능은 [`vscode.languages.*`](/api/references/vscode-api#languages) API를 통해 제공되는 스마트 편집 기능 모음입니다. Visual Studio Code에서 동적 언어 기능을 제공하는 방법은 크게 2가지가 있습니다. [Hover](#hover)를 예시로 들어보겠습니다:

<!--
Programmatic Language Features is a set of smart-editing features powered by the [`vscode.languages.*`](/api/references/vscode-api#languages) API. There are two common ways to provide a dynamic language feature in Visual Studio Code. Let's take [Hover](#hover) as an example: -->

```ts
vscode.languages.registerHoverProvider('javascript', {
  provideHover(document, position, token) {
    return {
      contents: ['Hover Content']
    };
  }
});
```
위에서 볼 수 있듯이, [`vscode.languages.registerHoverProvider`](/api/references/vscode-api#languages.registerHoverProvider) API는 자바스크립트 파일에 대하여 호버링을 제공 하는 쉬운 방법 입니다. 이 익스텐션이 활성화 된 이후, 여러분이 어떤 자바스크립트 코드 위에 마우스를 올릴때 마다, VS Code는 모든 자바스크립트에 대한 [`HoverProvider`](/api/references/vscode-api#HoverProvider)를 쿼리하여 그 결과를 호버링 위젯에 표기 할 것입니다. [언어 기능 목록](#language-features-listing)과 아래의 gif는 여러분에게 익스텐션이 필요로 하는 VS Code API / LSP 메소드를 찾는 방법을 제공합니다. 

<!--
As you see above, the [`vscode.languages.registerHoverProvider`](/api/references/vscode-api#languages.registerHoverProvider) API provides an easy way to provide hover contents to JavaScript files. After this extension gets activated, whenever you hover over some JavaScript code, VS Code queries all [`HoverProvider`](/api/references/vscode-api#HoverProvider) for JavaScript and shows the result in a Hover widget. The [Language Feature Listing](#language-features-listing) and illustrated gif below provides an easy way for you to locate which VS Code API / LSP Method your extension needs.-->

다른 방법은 [언어 서버 프로토콜](https://microsoft.github.io/language-server-protocol/)을 제공하는 언어 서버를 구현하는 것입니다. 작동 방식은 아래와 같습니다 : 

<!--
An alternative approach is to implement a Language Server that speaks [Language Server Protocol](https://microsoft.github.io/language-server-protocol/). The way it works is: -->

- 익스텐션이 언어 클라이언트와 자바스크립트를 위한 언어 서버를 제공합니다.
- 언어 클라이언트는 다른 VS Code 익스텐션과 같이, Node.js 익스텐션 호스트 컨텍스트에서 실행됩니다. 활성화 되면, 다른 프로세스에서 언어 서버를 생성하고, [언어 서버 프로토콜](https://microsoft.github.io/language-server-protocol/)을 통해 언어 서버와 통신 합니다. 
- VS Code의 자바스크립트 코드에 마우스를 올립니다.
- 언어 클라이언트가 호버링 결과를 언어 서버에 쿼리한 다음, VS Code로 다시 보냅니다.
- VS Code가 호버 위젯에 호버링 결과를 표기 합니다.

<!--
- An extension provides a Language Client and a Language Server for JavaScript.
- The Language Client is like any other VS Code extension, running in the Node.js Extension Host context. When it gets activated, it spawns the Language Server in another process and communicates with it through [Language Server Protocol](https://microsoft.github.io/language-server-protocol/).
- You hover over JavaScript code in VS Code
- VS Code informs the Language Client of the hover
- The Language Client queries the Language Server for a hover result and sends it back to VS Code
- VS Code displays the hover result in a Hover widget
-->

이 프로세스는 더 복잡해보이지만, 2가지 큰 장점이 있습니다:

<!-- The process seems more complicated, but it provides two major benefits:-->

- 언어 서버는 어떤 언어로도 작성 될 수 있습니다
- 언어 서버는 여러 에디터에 스마트 편집 기능을 제공하기 위해 재사용될 수 있습니다. 

<!--
- The Language Server can be written in any language
- The Language Server can be reused to provide smart editing features for multiple editors
-->

보다 자세한 가이드를 위해, [언어 서버 익스텐션 가이드](/api/language-extensions/language-server-extension-guide)를 참조하십시오. 

<!--
For a more in-depth guide, head over to the [Language Server Extension Guide](/api/language-extensions/language-server-extension-guide).
-->

---

## 언어 기능 목록
<!--
## Language Features Listing -->

아래 목록은 각 언어 기능에 해당하는 다음 항목을 포함합니다:
<!--
This listing includes the following items for each language feature: -->

- VS Code의 언어 기능 설명
- 연관된 VS Code API
- 연관된 LSP 메소드

<!--
- An illustration of the language feature in VS Code
- Related VS Code API
- Related LSP methods
-->

| VS Code API                                                                                                                       | LSP method                                                                                                                                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`createDiagnosticCollection`](/api/references/vscode-api#languages.createDiagnosticCollection)                                   | [PublishDiagnostics](https://microsoft.github.io/language-server-protocol/specification#textDocument_publishDiagnostics)                                                                                                                 |
| [`registerCompletionItemProvider`](/api/references/vscode-api#languages.registerCompletionItemProvider)                           | [Completion](https://microsoft.github.io/language-server-protocol/specification#textDocument_completion) & [Completion Resolve](https://microsoft.github.io/language-server-protocol/specification#completionItem_resolve)               |
| [`registerHoverProvider`](/api/references/vscode-api#languages.registerHoverProvider)                                             | [Hover](https://microsoft.github.io/language-server-protocol/specification#textDocument_hover)                                                                                                                                           |
| [`registerSignatureHelpProvider`](/api/references/vscode-api#languages.registerSignatureHelpProvider)                             | [SignatureHelp](https://microsoft.github.io/language-server-protocol/specification#textDocument_signatureHelp)                                                                                                                           |
| [`registerDefinitionProvider`](/api/references/vscode-api#languages.registerDefinitionProvider)                                   | [Definition](https://microsoft.github.io/language-server-protocol/specification#textDocument_definition)                                                                                                                                 |
| [`registerTypeDefinitionProvider`](/api/references/vscode-api#languages.registerTypeDefinitionProvider)                           | [TypeDefinition](https://microsoft.github.io/language-server-protocol/specification#textDocument_typeDefinition)                                                                                                                         |
| [`registerImplementationProvider`](/api/references/vscode-api#languages.registerImplementationProvider)                           | [Implementation](https://microsoft.github.io/language-server-protocol/specification#textDocument_implementation)                                                                                                                         |
| [`registerReferenceProvider`](/api/references/vscode-api#languages.registerReferenceProvider)                                     | [References](https://microsoft.github.io/language-server-protocol/specification#textDocument_references)                                                                                                                                 |
| [`registerDocumentHighlightProvider`](/api/references/vscode-api#languages.registerDocumentHighlightProvider)                     | [DocumentHighlight](https://microsoft.github.io/language-server-protocol/specification#textDocument_documentHighlight)                                                                                                                   |
| [`registerDocumentSymbolProvider`](/api/references/vscode-api#languages.registerDocumentSymbolProvider)                           | [DocumentSymbol](https://microsoft.github.io/language-server-protocol/specification#textDocument_documentSymbol)                                                                                                                         |
| [`registerCodeActionsProvider`](/api/references/vscode-api#languages.registerCodeActionsProvider)                                 | [CodeAction](https://microsoft.github.io/language-server-protocol/specification#textDocument_codeAction)                                                                                                                                 |
| [`registerCodeLensProvider`](/api/references/vscode-api#languages.registerCodeLensProvider)                                       | [CodeLens](https://microsoft.github.io/language-server-protocol/specification#textDocument_codeLens) & [CodeLens Resolve](https://microsoft.github.io/language-server-protocol/specification#codeLens_resolve)                           |
| [`registerDocumentLinkProvider`](/api/references/vscode-api#languages.registerDocumentLinkProvider)                               | [DocumentLink](https://microsoft.github.io/language-server-protocol/specification#textDocument_documentLink) & [DocumentLink Resolve](https://microsoft.github.io/language-server-protocol/specification#documentLink_resolve)                   |
| [`registerColorProvider`](/api/references/vscode-api#languages.registerDocumentColorProvider)                                     | [DocumentColor](https://microsoft.github.io/language-server-protocol/specification#textDocument_documentColor) & [Color Presentation](https://microsoft.github.io/language-server-protocol/specification#textDocument_colorPresentation) |
| [`registerDocumentFormattingEditProvider`](/api/references/vscode-api#languages.registerDocumentFormattingEditProvider)           | [Formatting](https://microsoft.github.io/language-server-protocol/specification#textDocument_formatting)                                                                                                                                 |
| [`registerDocumentRangeFormattingEditProvider`](/api/references/vscode-api#languages.registerDocumentRangeFormattingEditProvider) | [RangeFormatting](https://microsoft.github.io/language-server-protocol/specification#textDocument_rangeFormatting)                                                                                                                       |
| [`registerOnTypeFormattingEditProvider`](/api/references/vscode-api#languages.registerOnTypeFormattingEditProvider)               | [OnTypeFormatting](https://microsoft.github.io/language-server-protocol/specification#textDocument_onTypeFormatting)                                                                                                                     |
| [`registerRenameProvider`](/api/references/vscode-api#languages.registerRenameProvider)                                           | [Rename](https://microsoft.github.io/language-server-protocol/specification#textDocument_rename) & [Prepare Rename](https://microsoft.github.io/language-server-protocol/specification#textDocument_prepareRename)                       |
| [`registerFoldingRangeProvider`](/api/references/vscode-api#languages.registerFoldingRangeProvider)                               | [FoldingRange](https://microsoft.github.io/language-server-protocol/specification#textDocument_foldingRange)                                                                                                                             |

## 진단 제공

<!--
## Provide Diagnostics -->

진단은 코드의 문제를 표현하는 방식입니다. 
<!--
Diagnostics are a way to indicate issues with the code. -->

![Diagnostics at Work](images/language-support/diagnostics.gif)

#### 언어 서버 프로토콜
<!--
#### Language Server Protocol -->

언어 서버는 언어 클라이언트에 `textDocument/publishDiagnostics` 메세지를 전송합니다. 이 메세지는 리소스 URI에 대한 진단 항목으로 구성된 어레이를 전달합니다. 

<!--
Your language server sends the `textDocument/publishDiagnostics` message to the language client. The message carries an array of diagnostic items for a resource URI. -->

**주의**: 클라이언트는 서버에 진단을 요청하지 않으며, 서버가 클라이언트에 진단 정보를 푸시합니다.

<!--
**Note**: The client does not ask the server for diagnostics. The server pushes the diagnostic information to the client. -->

#### 직접 구현

<!--
#### Direct Implementation -->

```typescript
let diagnosticCollection: vscode.DiagnosticCollection;

export function activate(ctx: vscode.ExtensionContext): void {
  ...
  ctx.subscriptions.push(getDisposable());
  diagnosticCollection = vscode.languages.createDiagnosticCollection('go');
  ctx.subscriptions.push(diagnosticCollection);
  ...
}

function onChange() {
  let uri = document.uri;
  check(uri.fsPath, goConfig).then(errors => {
    diagnosticCollection.clear();
    let diagnosticMap: Map<string, vscode.Diagnostic[]> = new Map();
    errors.forEach(error => {
      let canonicalFile = vscode.Uri.file(error.file).toString();
      let range = new vscode.Range(error.line-1, error.startColumn, error.line-1, error.endColumn);
      let diagnostics = diagnosticMap.get(canonicalFile);
      if (!diagnostics) { diagnostics = []; }
      diagnostics.push(new vscode.Diagnostic(range, error.msg, error.severity));
      diagnosticMap.set(canonicalFile, diagnostics);
    });
    diagnosticMap.forEach((diags, file) => {
      diagnosticCollection.set(vscode.Uri.parse(file), diags);
    });
  })
}
```

> **기본**
> 
> 열린 에디터에 진단을 보고 하십시오. 최소한, 저장할때마다 해야 하며. 에디터의 저장 되지 않은 내용을 기반으로 계산한 진단은 더 좋습니다.

<!--
> **Basic**
>
> Report diagnostics for open editors. Minimally, this needs to happen on every save. Better, diagnostics should be computed based on the un-saved contents of the editor.
-->

> **고급**
>
> 열린 에디터 뿐 아니라, 에디터에서 열렸는지 여부의 관계 없이 열린 폴더의 모든 리소스에 대해 진단하십시오.

<!--
> **Advanced**
>
> Report diagnostics not only for the open editors but for all resources in the open folder, no matter whether they have ever been opened in an editor or not.
-->

## 코드 완성 제안 

<!--
## Show Code Completion Proposals -->

코드 완성은 사용자에게 상황에 따른 제안을 제공합니다. 
<!--
Code completions provide context sensitive suggestions to the user. -->

![Code Completion at Work](images/language-support/code-completion.gif)

#### 언어 서버 프로토콜
<!--
#### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 완료와, 계산이 완료된 추가 정보를 제공하기 위해 `completionItem\resolve` 메소드를 지원 하는지 에 대한 여부를 알려야 합니다. 

<!--
In the response to the `initialize` method, your language server needs to announce that it provides completions and whether or not it supports the `completionItem\resolve` method to provide additional information for the computed completion items.-->

```json
{
    ...
    "capabilities" : {
        "completionProvider" : {
            "resolveProvider": "true",
            "triggerCharacters": [ '.' ]
        }
        ...
    }
}
```

#### 직접 구현
<!--
#### Direct Implementation -->

```typescript
class GoCompletionItemProvider implements vscode.CompletionItemProvider {
    public provideCompletionItems(
        document: vscode.TextDocument, position: vscode.Position, token: vscode.CancellationToken):
        Thenable<vscode.CompletionItem[]> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(getDisposable());
    ctx.subscriptions.push(
        vscode.languages.registerCompletionItemProvider(
            GO_MODE, new GoCompletionItemProvider(), '.', '\"'));
    ...
}
```
> **기본**
>
> 해결 제공자를 지원하지 않습니다. 

> **고급**
>
> 사용자가 완료한 선택에 대하여, 추가 정보를 계산하는 분석 제공자를 지원합니다. 이 정보는 선택한 항목과 같이 표기 됩니다. 

<!--
> **Basic**
>
> You don't support resolve providers.
-->
<!--
> **Advanced**
>
> You support resolve providers that compute additional information for completion proposal the user selects. This information is displayed along-side the selected item.
-->

## 호버링
<!--
## Show Hovers -->

호버링은 마우스 커서 아래의 심볼/오브젝트에 대한 정보를 보여줍니다. 이는 보통 심볼의 타입과 설명입니다. 

<!--
Hovers show information about the symbol/object that's below the mouse cursor. This is usually the type of the symbol and a description.
-->

![Hovers at Work](images/language-support/hovers.gif)


#### 언어 서버 프로토콜

<!--
#### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 호버링을 제공 알려야 합니다. 
<!--
In the response to the `initialize` method, your language server needs to announce that it provides hovers. -->

```json
{
    ...
    "capabilities" : {
        "hoverProvider" : "true",
        ...
    }
}
```

추가로, 언어 서버는 `textDocument/hover` 리퀘스트에 응답해야 합니다. 

<!--
In addition, your language server needs to respond to the `textDocument/hover` request. -->

#### 직접 구현
<!--
#### Direct Implementation -->

```typescript
class GoHoverProvider implements HoverProvider {
    public provideHover(
        document: TextDocument, position: Position, token: CancellationToken):
        Thenable<Hover> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerHoverProvider(
            GO_MODE, new GoHoverProvider()));
    ...
}
```

> **기본**
>
> 가능한 경우 타입 정보와 다큐멘트를 포함하십시오.

> **고급**
>
> 코드와 같은 스타일로 메소드 시그니쳐에 색상을 제공하십시오. 

<!--
> **Basic**
>
> Show type information and include documentation if available.-->

<!--
> **Advanced**
>
> Colorize method signatures in the same style you colorize the code. -->


## 함수와 메소드 시그니쳐 도움말

<!--
## Help With Function and Method Signatures -->

사용자가 함수나 메소드를 입력할 때, 호출된 함수/메소드에 대한 정보를 표기합니다. 
<!--
When the user enters a function or method, display information about the function/method that is being called. -->

![Type Hover](images/language-support/signature-help.gif)

#### 언어 서버 프로토콜
<!--
#### Language Server Protocol -->

`initialize`메소드에 대한 응답으로, 언어 서버는 시그니쳐 도움말을 제공 알려야 합니다. 
<!--
In the response to the `initialize` method, your language server needs to announce that it provides signature help. -->

```json
{
    ...
    "capabilities" : {
        "signatureHelpProvider" : {
            "triggerCharacters": [ '(' ]
        }
        ...
    }
}
```

추가로 언어 서버는 `textDocument/signatureHelp`요청에 응답하여야 합니다. 
<!--
In addition, your language server needs to respond to the `textDocument/signatureHelp` request.-->

#### 직접 구현

<!--#### Direct Implementation -->

```typescript
class GoSignatureHelpProvider implements SignatureHelpProvider {
    public provideSignatureHelp(
        document: TextDocument, position: Position, token: CancellationToken):
        Promise<SignatureHelp> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerSignatureHelpProvider(
            GO_MODE, new GoSignatureHelpProvider(), '(', ','));
    ...
}
```

> **기본**
>
> 시그니쳐 도움말에 함수 혹은 메소드에 파라미터에 관한 문서가 포함되어있는지 확인하십시오. 

<!--
> **Basic**
>
> Ensure that the signature help contains the documentation of the parameters of the function or method. -->

> **고급**
>
> 추가할 것이 없습니다. 

<!--
> **Advanced**
>
> Nothing additional. -->

## 심볼 정의

<!--
## Show Definitions of a Symbol -->

사용자가 변수/함수/메소드가 사용되는 곳에서 변수/함수/메소드의 내용을 확인 할 수 있게 합니다. 

<!--
Allow the user to see the definition of variables/functions/methods right where the variables/functions/methods are being used. -->

![Type Hover](images/language-support/goto-definition.gif)

#### 언어 서버 프로토콜

<!--
#### Language Server Protocol -->

`initialize`메소드에 대한 응답으로, 언어 서버는 goto-definition 의 위치를 제공한다고 알려야합니다.  

<!--
In the response to the `initialize` method, your language server needs to announce that it provides goto-definition locations. -->

```json
{
    ...
    "capabilities" : {
        "definitionProvider" : "true"
        ...
    }
}
```

추가로, 언어 서버는 `textDocument/definition` 요청에 응답하여야 합니다. 

<!--
In addition, your language server needs to respond to the `textDocument/definition` request. -->

#### 직접 구현

<!--
#### Direct Implementation -->

```typescript
class GoDefinitionProvider implements vscode.DefinitionProvider {
    public provideDefinition(
        document: vscode.TextDocument, position: vscode.Position, token: vscode.CancellationToken):
        Thenable<vscode.Location> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerDefinitionProvider(
            GO_MODE, new GoDefinitionProvider()));
    ...
}
```
> **기본**
>
> 심볼이 여러개일 경우, 여러 정의를 표시 할 수 있습니다.

> **고급**
>
> 추가 될 것이 없습니다. 

<!--
> **Basic**
>
> If a symbol is ambivalent, you can show multiple definitions. -->

<!--
> **Advanced**
>
> Nothing additional. -->

## 모든 심볼 참조 검색

<!-- ## Find All References to a Symbol-->

사용자가 사용하는 특정 변수/함수/메소드/심볼의 소스 코드의 위치를 볼 수 있게 합니다. 

<!--
Allow the user to see all the source code locations where a certain variable/function/method/symbol is being used. -->

![Type Hover](images/language-support/find-references.gif)

#### 언어 서버 프로토콜

<!--
#### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 심볼 참조 위치를 제공한다고 알려야합니다. 

<!--
In the response to the `initialize` method, your language server needs to announce that it provides symbol reference locations. -->

```json
{
    ...
    "capabilities" : {
        "referencesProvider" : "true"
        ...
    }
}
```

추가로, 언어 서버는 `textDocument/references` 요청에 응답해야 합니다. 

<!-- 
In addition, your language server needs to respond to the `textDocument/references` request. -->

#### 직접 구현

<!--
#### Direct Implementation -->

```typescript
class GoReferenceProvider implements vscode.ReferenceProvider {
    public provideReferences(
        document: vscode.TextDocument, position: vscode.Position,
        options: { includeDeclaration: boolean }, token: vscode.CancellationToken):
        Thenable<vscode.Location[]> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerReferenceProvider(
            GO_MODE, new GoReferenceProvider()));
    ...
}
```

> **기본**
>
> 모든 참조에 대하여 위치를 (리소스 URI와 범위) 반환하십시오. 

> **고급**
>
> 추가 될 것이 없습니다. 

<!--
> **Basic**
>
> Return the location (resource URI and range) for all references.
-->
<!--
> **Advanced**
>
> Nothing additional.
-->

## 문서의 모든 심볼 강조

<!-- ## Highlight All Occurrences of a Symbol in a Document -->

사용자에게 열려있는 에디터의 모든 심볼을 확인할 수 있게 합니다. 

<!--
Allow the user to see all occurrences of a symbol in the open editor. -->

![Type Hover](images/language-support/document-highlights.gif)

#### 언어 서버 프로토콜

<!-- #### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 심볼 문서 위치를 제공한다고 알려야합니다.

<!--
In the response to the `initialize` method, your language server needs to announce that it provides symbol document locations. -->

```json
{
    ...
    "capabilities" : {
        "documentHighlightProvider" : "true"
        ...
    }
}
```
추가로, 언어 서버는 `textDocument/documentHighlight` 요청에 응답해야 합니다. 

<!--
In addition, your language server needs to respond to the `textDocument/documentHighlight` request. -->

#### 직접 구현

<!--
#### Direct Implementation -->

```typescript
class GoDocumentHighlightProvider implements vscode.DocumentHighlightProvider {
    public provideDocumentHighlights(
        document: vscode.TextDocument, position: vscode.Position, token: vscode.CancellationToken):
        vscode.DocumentHighlight[] | Thenable<vscode.DocumentHighlight[]>;
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerDocumentHighlightProvider(
            GO_MODE, new GoDocumentHighlightProvider()));
    ...
}
```

> **기본**
>
> 에디터의 문서에서 참조가 발견 되는 범위를 반환합니다.

> **고급**
>
> 추가 될 것이 없습니다. 

<!--
> **Basic**
>
> You return the ranges in the editor's document where the references are being found. -->
<!--
> **Advanced**
>
> Nothing additional. -->

## 문서의 모든 심볼 정의

<!-- ## Show all Symbol Definitions Within a Document -->

사용자에게 열린 에디터에서 빠르게 심볼의 정의를 확인 할 수 있게 합니다. 

<!--
Allow the user to quickly navigate to any symbol definition in the open editor. -->

![Type Hover](images/language-support/document-symbols.gif)

#### 언어 서버 프로토콜
<!--
#### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 심볼 문서 위치를 제공한다고 알려야합니다.
<!-- In the response to the `initialize` method, your language server needs to announce that it provides symbol document locations.-->

```json
{
    ...
    "capabilities" : {
        "documentSymbolProvider" : "true"
        ...
    }
}
```

추가로, 언어 서버는 `textDocument/documentSymbol` 요청에 응답해야 합니다. 

<!--
In addition, your language server needs to respond to the `textDocument/documentSymbol` request. -->

#### 직접 구현

<!--
#### Direct Implementation -->

```typescript
class GoDocumentSymbolProvider implements vscode.DocumentSymbolProvider {
    public provideDocumentSymbols(
        document: vscode.TextDocument, token: vscode.CancellationToken):
        Thenable<vscode.SymbolInformation[]> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerDocumentSymbolProvider(
            GO_MODE, new GoDocumentSymbolProvider()));
    ...
}
```

> **기본**
>
> 문서의 모든 심볼을 반환합니다. 변수, 함수, 클래스, 메소드 등과 같은 심볼의 종류를 정의하십시오. 

> **고급**
>
> 추가 될 것이 없습니다. 

<!--
> **Basic**
>
> Return all symbols in the document. Define the kinds of symbols such as variables, functions, classes, methods, etc. -->

<!--
> **Advanced**
>
> Nothing additional. -->

## 폴더의 모든 심볼 정의

<!--
## Show all Symbol Definitions in Folder -->

사용자가 VS Code의 (작업공간) 열린 폴더 내부의 심볼 정의를 확인 할 수 있게 합니다. 

<!--
Allow the user to quickly navigate to symbol definitions anywhere in the folder (workspace) opened in VS Code. -->

![Type Hover](images/language-support/workspace-symbols.gif)

#### 언어 서버 프로토콜

<!-- 
#### Language Server Protocol-->

`initialize` 메소드에 대한 응답으로, 언어 서버는 전역 심볼 위치를 제공한다고 알려야합니다.
<!--
In the response to the `initialize` method, your language server needs to announce that it provides global symbol locations. -->

```json
{
    ...
    "capabilities" : {
        "workspaceSymbolProvider" : "true"
        ...
    }
}
```

추가로, 언어 서버는 `workspace/symbol` 요청에 응답해야 합니다. 

<!--
In addition, your language server needs to respond to the `workspace/symbol` request. -->

#### 직접 구현

<!-- #### Direct Implementation -->

```typescript
class GoWorkspaceSymbolProvider implements vscode.WorkspaceSymbolProvider {
    public provideWorkspaceSymbols(
        query: string, token: vscode.CancellationToken):
        Thenable<vscode.SymbolInformation[]> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerWorkspaceSymbolProvider(
            new GoWorkspaceSymbolProvider()));
    ...
}
```

> **기본**
>
> 열린 폴더의 모든 소스 코드에 정의 된 심볼을 반환합니다. 변수, 함수, 클래스, 메소드 등과 같은 심볼의 종류를 정의하십시오. 

> **고급**
>
> 추가 될 것이 없습니다. 

<!--
> **Basic**
>
> Return all symbols define by the source code within the open folder. Define the kinds of symbols such as variables, functions, classes, methods, etc. -->

<!--
> **Advanced**
>
> Nothing additional. -->

## 가능한 액션, 에러, 경고

<!--
## Possible Actions on Errors or Warnings -->

사용자에게 에러나 경고에 대하여 가능한 액션을 제공합니다. 만약 액션이 사용가능 하다면, 전구 모양이 에러 혹은 경고 옆에 나타날 것입니다. 사용자가 전구를 클릭하면, 가능한 코드 액션 목록이 표시 됩니다.

<!--
Provide the user with possible corrective actions right next to an error or warning. If actions are available, a light bulb appears next to the error or warning. When the user clicks the light bulb, a list of available Code Actions is presented. -->

![Type Hover](images/language-support/quick-fixes.gif)

#### 언어 서버 프로토콜

<!--
#### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 코드 액션을 제공한다고 알려야합니다.

<!-- In the response to the `initialize` method, your language server needs to announce that it provides Code Actions.-->

```json
{
    ...
    "capabilities" : {
        "codeActionProvider" : "true"
        ...
    }
}
```

추가로, 언어 서버는 `textDocument/codeAction` 요청에 응답해야 합니다. 

<!-- In addition, your language server needs to respond to the `textDocument/codeAction` request.-->

#### 직접 구현
<!--
#### Direct Implementation-->

```typescript
class GoCodeActionProvider implements vscode.CodeActionProvider {
    public provideCodeActions(
        document: vscode.TextDocument, range: vscode.Range,
        context: vscode.CodeActionContext, token: vscode.CancellationToken):
        Thenable<vscode.Command[]> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerCodeActionsProvider(
            GO_MODE, new GoCodeActionProvider()));
    ...
}
```

> **기본**
>
> 에러/경고를 고칠 수 있는 코드 액션을 제공하십시오. 

> **고급**
>
> 추가로, 리팩토링과 같은 소스 코드 조작 액션을 제공하십시오. 예를 들면 **Extract Method** 입니다 .

<!--
> **Basic**
>
> Provide Code Actions for error/warning correcting actions. -->

<!--
> **Advanced**
>
> In addition, provide source code manipulation actions such as refactoring. For example, **Extract Method**. -->

## CodeLens - 소스 코드내부의 실행 가능한 컨텍스트 정보 제공

<!--
## CodeLens - Show Actionable Context Information Within Source Code -->

사용자에게 소스 코드와 함께 표시되는 실행 가능한 컨텍스트 정보를 제공합니다.

<!-- Provide the user with actionable, contextual information that is displayed interspersed with the source code. -->

![Type Hover](images/language-support/code-lens.gif)

#### 언어 서버 프로토콜

<!-- #### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 CodeLens 결과를 제공하는 것과, CodeLens 커맨드에 바인딩 하는 `condLens\resolve`를 지원하는지의 여부를 알려야합니다. 

<!--
In the response to the `initialize` method, your language server needs to announce that it provides CodeLens results and whether it supports the `codeLens\resolve` method to bind the CodeLens to its command. -->

```json
{
    ...
    "capabilities" : {
        "codeLensProvider" : {
            "resolveProvider": "true"
        }
        ...
    }
}
```

추가로, 언어 서버는 `textDocument/codeLens` 요청에 응답해야 합니다. 

<!--
In addition, your language server needs to respond to the `textDocument/codeLens` request. -->

#### 직접 구현
<!--
#### Direct Implementation -->

```typescript
class GoCodeLensProvider implements vscode.CodeLensProvider {
    public provideCodeLenses(document: TextDocument, token: CancellationToken):
        CodeLens[] | Thenable<CodeLens[]> {
    ...
    }

    public resolveCodeLens?(codeLens: CodeLens, token: CancellationToken):
         CodeLens | Thenable<CodeLens> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerCodeLensProvider(
            GO_MODE, new GoCodeLensProvider()));
    ...
}
```

> **기본**
>
> 문서에 사용가능한 CodeLens 결과를 정의하십시오. 

> **고급**
>
> `codeLens/resolve`에 응답하여 CodeLens 결과를 커맨드에 바인딩 하십시오. 

<!--
> **Basic**
>
> Define the CodeLens results that are available for a document. -->

<!--
> **Advanced**
>
> Bind the CodeLens results to a command by responding to `codeLens/resolve`.-->

## 색상 설정

<!--
## Show Color Decorators -->

사용자에게 문서의 색상을 수정하거나 미리보기 할 수 있게 합니다.

<!--
Allow the user to preview and modify colors in the document. -->

![Type Hover](images/language-support/color-decorators.png)

#### 언어 서버 프로토콜
<!--
#### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 색상 정보를 제공한다고 알려야합니다.
<!--
In the response to the `initialize` method, your language server needs to announce that it provides color information. -->

```json
{
    ...
    "capabilities" : {
        "colorProvider" : "true"
        ...
    }
}
```

추가로, 언어 서버는 `textDocument/documentColor` 과 `textDocument/colorPresentation` 요청에 응답해야 합니다. 

<!-- In addition, your language server needs to respond to the `textDocument/documentColor` and `textDocument/colorPresentation` requests.-->

#### 직접 구현
<!--
#### Direct Implementation -->

```typescript
class GoColorProvider implements vscode.DocumentColorProvider {
    public provideDocumentColors(
        document: vscode.TextDocument, token: vscode.CancellationToken):
        Thenable<vscode.ColorInformation[]> {
    ...
    }
    public provideColorPresentations(
        color: Color, context: { document: TextDocument, range: Range }, token: vscode.CancellationToken):
        Thenable<vscode.ColorPresentation[]> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerColorProvider(
            GO_MODE, new GoColorProvider()));
    ...
}
```

> **기본**
>
> 문서의 모든 색상 참조를 반환하십시오. 지원 되는 색상 형식에 대한 표현을 제공하십시오. (예 : rgb(...), hsl(...))

> **고급**
>
> 추가 될 것이 없습니다 .


<!--
> **Basic**
>
> Return all color references in the document. Provide color presentations for the color formats supported (for example rgb(...), hsl(...)). -->

<!--
> **Advanced**
>
> Nothing additional. -->

## 에디터의 소스 코드 포맷
<!--
## Format Source Code in an Editor -->

사용자에게 전체 문서에 대한 포맷을 지원합니다. 
<!--
Provide the user with support for formatting whole documents.-->

![Document Formatting at Work](images/language-support/format-document.gif)

#### 언어 서버 프로토콜

<!-- #### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 문서 포맷을 제공한다고 알려야합니다.
<!--
In the response to the `initialize` method, your language server needs to announce that it provides document formatting.-->

```json
{
    ...
    "capabilities" : {
        "documentFormattingProvider" : "true"
        ...
    }
}
```
추가로, 언어 서버는 `textDocument/formatting` 요청에 응답해야 합니다. 
<!--
In addition, your language server needs to respond to the `textDocument/formatting` request. -->

#### 직접 구현
<!--
#### Direct Implementation -->

```typescript
class GoDocumentFormatter implements vscode.DocumentFormattingEditProvider {
    public formatDocument(document: vscode.TextDocument):
        Thenable<vscode.TextEdit[]> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerDocumentFormattingEditProvider(
            GO_MODE, new GoDocumentFormatter()));
    ...
}
```

> **기본**
>
> 포맷 지원을 제공하지 않습니다.

> **고급**
>
> 소스 코드를 포하는 최소한의 텍스트 수정을 반환해야 합니다. 이는 진단 결과와 같은 마커가 올바르게 조절되고 손실 되지 않도록 하는것에 있어 중요합니다. 

<!--
> **Basic**
>
> Don't provide formatting support. -->
<!--
> **Advanced**
>
> You should always return the smallest possible text edits that result in the source code being formatted. This is crucial to ensure that markers such as diagnostic results are adjusted correctly and are not lost. -->

## 에디터의 선택된 줄에 포맷

<!--
## Format the Selected Lines in an Editor -->

사용자에게 문서에서 선택된 범위내의 줄에 대한 포맷을 지원합니다. 

<!--
Provide the user with support for formatting a selected range of lines in a document. -->

![Document Formatting at Work](images/language-support/format-document-range.gif)

#### 언어 서버 프로토콜
<!--
#### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 선택된 줄에 대한 포맷을 제공한다고 알려야합니다.

<!--
In the response to the `initialize` method, your language server needs to announce that it provides formatting support for ranges of lines. -->

```json
{
    ...
    "capabilities" : {
        "documentRangeFormattingProvider" : "true"
        ...
    }
}
```

추가로, 언어 서버는 `textDocument/rangeFormatting` 요청에 응답해야 합니다. 

<!-- In addition, your language server needs to respond to the `textDocument/rangeFormatting` request. -->

#### 직접 구현

<!-- #### Direct Implementation -->

```typescript
class GoDocumentRangeFormatter implements vscode.DocumentRangeFormattingEditProvider{
    public provideDocumentRangeFormattingEdits(
        document: vscode.TextDocument, range: vscode.Range,
        options: vscode.FormattingOptions, token: vscode.CancellationToken):
        Thenable<vscode.TextEdit[]>;
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerDocumentRangeFormattingEditProvider(
            GO_MODE, new GoDocumentRangeFormatter()));
    ...
}
```


> **기본**
>
> 포맷 지원을 제공하지 않습니다. 

> **고급**
>
> 소스 코드를 포맷하는 최소한의 텍스트 수정을 반환해야 합니다. 이는 진단 결과와 같은 마커가 올바르게 조절되고 손실 되지 않도록 하는것에 있어 중요합니다.

<!--
> **Basic**
>
> Don't provide formatting support. -->

<!--
> **Advanced**
>
> You should always return the smallest possible text edits that result in the source code being formatted. This is crucial to ensure that markers such as diagnostic results are adjusted corrected and are not lost. -->

## 사용자 입력에 대한 포맷

<!-- 
## Incrementally Format Code as the User Types -->

사용자에게 그들이 입력 하는 대로 포맷을 지원합니다. 
<!--
Provide the user with support for formatting text as they type. -->

**주의**: 사용자 [설정](/docs/getstarted/settings) `editor.formatOnType`으로 소스 코드가 사용자 타입으로 포맷 될지 조절 합니다 .

<!--
**Note**: The user [setting](/docs/getstarted/settings) `editor.formatOnType` controls whether source code gets formatted or not as the user types. -->

![Document Formatting at Work](images/language-support/format-on-type.gif)

#### 언어 서버 프로토콜

<!-- #### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 사용자 입력으로 포맷을 제공한다고 알려야합니다. 또한 클라이언트에 어떤 문자로 포맷을 시작할지 알려야 합니다. `moreTriggerCharacters`는 선택지입니다. 

<!--
In the response to the `initialize` method, your language server needs to announce that it provides formatting as the user types. It also needs to tell the client on which characters formatting should be triggered. `moreTriggerCharacters` is optional.-->

```json
{
    ...
    "capabilities" : {
        "documentOnTypeFormattingProvider" : {
            "firstTriggerCharacter": "}",
            "moreTriggerCharacter": [";", ","]
        }
        ...
    }
}
```

추가로, 언어 서버는 `textDocument/onTypeFormatting` 요청에 응답해야 합니다. 

<!-- In addition, your language server needs to respond to the `textDocument/onTypeFormatting` request.-->

#### 직접 구현

<!-- #### Direct Implementation -->

```typescript
class GoOnTypingFormatter implements vscode.OnTypeFormattingEditProvider{
    public provideOnTypeFormattingEdits(
        document: vscode.TextDocument, position: vscode.Position,
        ch: string, options: vscode.FormattingOptions, token: vscode.CancellationToken):
        Thenable<vscode.TextEdit[]>;
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerOnTypeFormattingEditProvider(
            GO_MODE, new GoOnTypingFormatter()));
    ...
}
```

> **기본**
>
> 포맷 지원을 제공하지 않습니다. 

> **고급**
>
> 소스 코드를 포맷하는 최소한의 텍스트 수정을 반환해야 합니다. 이는 진단 결과와 같은 마커가 올바르게 조절되고 손실 되지 않도록 하는것에 있어 중요합니다.

<!-- 
> **Basic**
>
> Don't provide formatting support. -->
<!--
> **Advanced**
>
> You should always return the smallest possible text edits that result in the source code being formatted. This is crucial to ensure that markers such as diagnostic results are adjusted corrected and are not lost. -->

## 심볼 이름 변경

<!-- 
## Rename Symbols-->

사용자에게 심볼 이름을 변경하고 심볼의 모든 참조를 업데이트 하게 합니다. 

<!--
Allow the user to rename a symbol and update all references to the symbol. -->

![Type Hover](images/language-support/rename.gif)

#### 언어 서버 프로토콜
<!--
#### Language Server Protocol -->

`initialize` 메소드에 대한 응답으로, 언어 서버는 이름 변경을 제공한다고 알려야합니다.
<!--
In the response to the `initialize` method, your language server needs to announce that it provides for renaming. -->

```json
{
    ...
    "capabilities" : {
        "renameProvider" : "true"
        ...
    }
}
```

추가로, 언어 서버는 `textDocument/rename` 요청에 응답해야 합니다. 

<!--
In addition, your language server needs to respond to the `textDocument/rename` request. -->

#### 직접 구현

<!-- #### Direct Implementation -->

```typescript
class GoRenameProvider implements vscode.RenameProvider {
    public provideRenameEdits(
        document: vscode.TextDocument, position: vscode.Position,
        newName: string, token: vscode.CancellationToken):
        Thenable<vscode.WorkspaceEdit> {
    ...
    }
}

export function activate(ctx: vscode.ExtensionContext): void {
    ...
    ctx.subscriptions.push(
        vscode.languages.registerRenameProvider(
            GO_MODE, new GoRenameProvider()));
    ...
}
```

> **기본**
>
> 이름 변경 지원을 제공하지 않습니다. 

> **고급**
>
> 수행해야 하는 모든 작업 공간 목록을 반환합니다. 예를 들면 심볼에 대한 참조를 포함하는 모든 일의 편집 목록입니다.

<!--
> **Basic**
>
> Don't provide rename support.-->

<!--
> **Advanced**
>
> Return the list of all workspace edits that need to be performed, for example all edits across all files that contain references to the symbol.
-->
