---
layout: default
parent: Language Extensions
title: Language Server Extension Guide
nav_order: 6
description: ""
---

# 언어 서버 익스텐션 가이드

<!--
# Language Server Extension Guide -->

[프로그래밍 언어 기능](/api/language-extensions/programmatic-language-features) 주제에서 봤듯이, `languages.*` API를 이용하여 언어 기능을 바로 구현하는것이 가능합니다. 언어 서버 익스텐션은 이런 언어 지원을 구현하는 또 다른 방법입니다. 
<!--
As you have seen in the [Programmatic Language Features](/api/language-extensions/programmatic-language-features) topic, it's possible to implement Language Features by directly using `languages.*` API. Language Server Extension, however, provides an alternative way of implementing such language support. -->

이번 주제에선:

<!-- This topic: -->

- 언어 서버 익스텐션의 장점을 설명합니다.
- [`Microsoft/vscode-languageserver-node`](https://github.com/Microsoft/vscode-languageserver-node) 라이브러리를 사용한 언어 서버 작성을 설명 합니다. [lsp-sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-sample)에 있는 코드를 바로 참조 할 수도 있습니다.

<!--
- Explains the benefits of Language Server Extension.
- Walks you through building a Language Server using the [`Microsoft/vscode-languageserver-node`](https://github.com/Microsoft/vscode-languageserver-node) library. You can also jump directly to the code in [lsp-sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-sample). -->

## 언어 서버를 사용하는 이유

<!--
## Why Language Server? -->

언어 서버는 다양한 프로그래밍 언어 편집에 도움을 주는 Visual Studio Code 익스텐션의 특수한 종류입니다. 언어 서버를 통해 자동완성, 에러 확인 (진단), 정의로 이동 그리고 많은 VS Code에서 지원하는 [언어 기능](/api/language-extensions/programmatic-language-features)을 이용 할 수 있습니다. 

<!--
Language Server is a special kind of Visual Studio Code extension that powers the editing experience for many programming languages. With Language Servers, you can implement autocomplete, error-checking (diagnostics), jump-to-definition, and many other [language features](/api/language-extensions/programmatic-language-features) supported in VS Code.
-->

하지만 VS Code에서 지원하는 언어 기능을 구현하는 동안, 3가지 공통 문제점이 발견 되었습니다.

<!--
However, while implementing support for language features in VS Code, we found three common problems: -->

첫번째로, 언어 서버는 대개 자신들의 프로그래밍 언어로 구현됩니다, 그렇기 때문에 이를 Node.js 런타임을 사용하는 VS Code에 통합하는 것은 쉽지 않습니다.

<!--
First, Language Servers are usually implemented in their native programming languages, and that presents a challenge in integrating them with VS Code, which has a Node.js runtime.-->

그 다음으로, 언어 기능의 리소스 소모를 많이 요구 할 수도 있습니다. 예를 들어, 파일을 정확하게 검증 하기 위해서, 언어 서버는 많은 양의 파일을 파싱하고, 추상 구문 트리를 작성하여 정적 프로그램 분석을 실행합니다. 이러한 작동은 CPU 와 메모리 사용량의 증가를 유도 할 수 있기 때문에 VS Code의 퍼포먼스가 영향을 받지 않는다는 것을 확인 해야 합니다. 

<!--
Additionally, language features can be resource intensive. For example, to correctly validate a file, Language Server needs to parse a large amount of files, build up Abstract Syntax Trees for them and perform static program analysis. Those operations could incur significant CPU and memory usage and we need to ensure that VS Code's performance remains unaffected. -->

마지막으로, 다양한 언어 도구를 다양한 코드 에디터에서 통합하는것은 상당한 노력을 필요로 합니다. 언어 도구의 관점에서, 다른 API를 가진 코드 에디터에 적응해야 하고, 코드 에디터의 관점에서는, 언어 도구에서 동일한 API 형태를 기대 할 수 없습니다. 이는 `N` 코드 에디터에서의 `M` 언어 지원이 `M * N` 만큼의 작업을 필요로 하게 합니다. 

<!--
Finally, integrating multiple language toolings with multiple code editors could involve significant effort. From language toolings' perspective, they need to adapt to code editors with different APIs. From code editors' perspective, they cannot expect any uniform API from language toolings. This makes implementing language support for `M` languages in `N` code editors the work of `M * N`.-->

이 문제들을 해결하기 위해서, 마이크로소프트는 언어 도구와 코드 에디터간의 통신을 표준화 하는 [언어 서버 프로토콜](https://microsoft.github.io/language-server-protocol)을 명시했습니다. 이 방법으로, 언어 서버는 어느 언어로도 구현 될 수 있으며, 코드 에디터와 언어 서버 프로토콜을 이용한 통신을 하여, 퍼포먼스 비용을 피하며 자체 프로세스에서 실행 할 수 있게 됩니다. 더 나아가, 모든 LSP를 준수하는 언어 도구는 여러개의 LSP 준수 코드 에디터에 통합 될 수 있고, 모든 LSP 준수 코드 에디터는 여러개의 LSP 준수 언어 도구를 선택 할 수 있습니다. LSP 는 언어 도구 제공자와 코드 에디터 공급자에 대하여 모두에게 이득입니다.

<!--
To solve those problems, Microsoft specified [Language Server Protocol](https://microsoft.github.io/language-server-protocol) which standardizes the communication between language tooling and code editor. This way, Language Servers can be implemented in any language and run in their own process to avoid performance cost, as they communicate with the code editor through the Language Server Protocol. Furthermore, any LSP-compliant language toolings can integrate with multiple LSP-compliant code editors, and any LSP-compliant code editors can easily pick up multiple LSP-compliant language toolings. LSP is a win for both language tooling providers and code editor vendors! -->

![LSP Languages and Editors](images/language-server-extension-guide/lsp-languages-editors.png)

이번 가이드에서는: 

<!--
In this guide, we will: -->

- VS Code에서 제공된 [Node SDK](https://github.com/Microsoft/vscode-languageserver-node)를 이용하여 언어 서버 익스텐션을 작성하는 방법을 설명합니다. 
- 언어 서버 익스텐션을 실행, 디버그, 로그, 테스트 하는 방법을 설명합니다.
- 언어 서버에 관한 고급 주제들을 설명합니다. 

<!--
- Explain how to build a Language Server extension in VS Code using the provided [Node SDK](https://github.com/Microsoft/vscode-languageserver-node).
- Explain how to run, debug, log, and test the Language Server extension.
- Point you to some advanced topics on Language Servers. -->

## 언어 서버 구현

<!--
## Implementing a Language Server -->

### 개요

<!--
### Overview -->

VS Code에서, 언어 서버는 2가지 부분으로 구성됩니다:

<!--
In VS Code, a language server has two parts: -->

- 언어 클라이언트 : 자바스크립트 / 타입스크립트로 작성된 일반적인 VS Code 익스텐션. 이 익스텐션은 모든 [VS Code Namespace API](/api/references/vscode-api)에 액세스 할 수 있습니다. 
- 언어 서버 : 분리된 프로세스 에서 실행되는 언어 분석 도구입니다. 

<!--
- Language Client: A normal VS Code extension written in JavaScript / TypeScript. This extension has access to all [VS Code Namespace API](/api/references/vscode-api).
- Language Server: A language analysis tool running in a separate process. -->

위에서 설명한대로 언어 서버를 분리된 프로세스에서 실행하는 것은 2가지 장점이 있습니다:

<!--
As briefly stated above there are two benefits of running the Language Server in a separate process:-->

- 언어 서버 프로토콜에 따라 언어 클라이언트와 통신 할 수 있는 한, 분석 도구는 모든 언어로 구현될 수 있습니다. 
- 언어 분석 도구가 CPU 와 메모리 사용량이 높기 때문에, 분리된 프로세스에서 실행 하는것은 퍼포먼스 비용을 피할 수 있게 합니다. 

<!--
- The analysis tool can be implemented in any languages, as long as it can communicate with the Language Client following the Language Server Protocol.
- As language analysis tools are often heavy on CPU and Memory usage, running them in separate process avoids performance cost. -->

다음은 2개의 언어 서버 익스텐션을 실행하는 VS Code에 대한 설명입니다. HTML 언어 클라이언트와 PHP 언어 클라이언트는 타입스크립트로 작성된 일반적인 VS Code 익스텐션입니다. 그들은 각자 해당하는 언어 서버를 인스턴스화하고 LSP를 통하여 통신합니다. PHP 언어 서버가 PHP로 작성되었지만 LSP를 통해 PHP 언어 클라이언트와 통신 할 수 있습니다.

<!--
Here is an illustration of VS Code running two Language Server extensions. The HTML Language Client and PHP Language Client are normal VS Code extensions written in TypeScript. Each of them instantiates a corresponding Language Server and communicates with them through LSP. Although the PHP Language Server is written in PHP, it can still communicate with the PHP Language Client through LSP. -->

![LSP Illustration](images/language-server-extension-guide/lsp-illustration.png)

이 가이드는 여러분에게 [Node SDK](https://github.com/Microsoft/vscode-languageserver-node)를 이용하여 언어 클라이언트 / 서버 작성 하는 방법을 가르칠 것입니다. 문서의 남은 부분은 여러분이 VS Code [익스텐션 API](/api)에 익숙하다고 가정합니다. 

<!--
This guide will teach you how to build a Language Client / Server using our [Node SDK](https://github.com/Microsoft/vscode-languageserver-node). The remaining document assumes that you are familiar with VS Code [Extension API](/api). -->

### 예시 LSP - 간단한 텍스트 파일을 위한 언어 서버 

<!--
### LSP Sample - A simple Language Server for plain text files -->

텍스트 파일에 대하여 자동완성과 진단을 구현하는, 간단한 언어 서버 익스텐션을 작성해보겠습니다. 클라이언트 / 서버 간의 구성 동기화 또한 다룹니다. 

<!--
Let's build a simple Language Server extension that implements autocomplete and diagnostics for plain text files. We will also cover the syncing of configurations between Client / Server. -->

코드를 바로 확인 하길 선호한다면:

<!--
If you prefer to jump right into the code:-->

- **[lsp-sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-sample)**: 많은 설명이 추가된, 이번 가이드의 소스 코드 입니다. 
- **[lsp-multi-server-sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-multi-server-sample)**:  VS Code의 [멀티 루트 작업공간](/docs/editor/multi-root-workspaces) 기능을 지원하기 위해 작업 공간 폴더마다 다른 서버 인스턴스를 시작하는 **lsp-sample**의 고급 버전입니다. 

<!--
- **[lsp-sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-sample)**: Heavily documented source code for this guide.
- **[lsp-multi-server-sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-multi-server-sample)**: A heavily documented, advanced version of **lsp-sample** that starts a different server instance per workspace folder to support the [multi-root workspace](/docs/editor/multi-root-workspaces) feature in VS Code.
-->

예시를 열기 위해 [Microsoft/vscode-extension-samples](https://github.com/Microsoft/vscode-extension-samples) 저장소를 클론하십시오:

<!--
Clone the repository [Microsoft/vscode-extension-samples](https://github.com/Microsoft/vscode-extension-samples) and open the sample: -->

```bash
> git clone https://github.com/Microsoft/vscode-extension-samples.git
> cd vscode-extension-samples/lsp-sample
> npm install
> npm run compile
> code .
```

위의 모든 디펜던시를 설치한 다음 클라이언트와 서버 코드를 모두 포함한 **lsp-sample** 작업공간을 여십시오. 아래는 **lsp-sample**의 구조에 대한 간단한 개요입니다.

<!--
The above installs all dependencies and opens the **lsp-sample** workspace containing both the client and server code. Here is a rough overview of the structure of **lsp-sample**: -->

```
.
├── client // Language Client
│   ├── src
│   │   ├── test // End to End tests for Language Client / Server
│   │   └── extension.ts // Language Client entry point
├── package.json // The extension manifest
└── server // Language Server
    └── src
        └── server.ts // Language Server entry point
```

### '언어 클라이언트'에 대한 설명

<!--
### Explaining the 'Language Client' -->

언어 클라이언트의 수용을 설명하는, `/package.json`을 먼저 확인하겠습니다. 3가지 흥미로운 섹션이 있습니다:

<!--
Let's first take a look at `/package.json`, which describes the capabilities of the Language Client. There are three interesting sections: -->

첫번째로 [`activationEvents`](/api/references/activation-events)를 확인하십시오:

<!--
First look the [`activationEvents`](/api/references/activation-events): -->

```json
"activationEvents": [
    "onLanguage:plaintext"
]
```

이 섹션은 VS Code가 텍스트 파일이 열리는 즉시 (예들들어 `.txt`확장자를 갖는 파일) 익스텐션을 활성화하도록 합니다.

<!--
This section tells VS Code to activate the extension as soon as a plain text file is opened (for example a file with the extension `.txt`). -->

다음으로 [`configuration`](/api/references/contribution-points#contributes.configuration) 섹션을 확인하십시오:

<!--
Next look at the [`configuration`](/api/references/contribution-points#contributes.configuration) section: -->

```json
"configuration": {
    "type": "object",
    "title": "Example configuration",
    "properties": {
        "languageServerExample.maxNumberOfProblems": {
            "scope": "resource",
            "type": "number",
            "default": 100,
            "description": "Controls the maximum number of problems produced by the server."
        }
    }
}
```

이 섹션은 VS Code의 `configuration` 설정을 작성합니다. 예시는 이러한 설정이 시작할때와 설정이 변경될때마다 언어 서버에 전송되는 방법을 설명합니다. 

<!--
This section contributes `configuration` settings to VS Code. The example will explain how these settings are sent over to the language server on startup and on every change of the settings.-->

실제 언어 클라이언트 코드와 해당하는 `package.json` 은 `/client` 폴더에 위치합니다. `/client/pakcage.json`에 대한 흥미로운 부분은 `vscode`익스텐션 호스트 API와 `vscode-languageclient`라이브러리에 대한 의존성을 추가하는 것입니다:

<!--
The actual Language Client code and the corresponding `package.json` is in the `/client` folder. The interesting part in the `/client/package.json` file is that it adds a dependency to the `vscode` extension host API and the `vscode-languageclient` library:-->

```json
"dependencies": {
    "vscode": "^1.1.18",
    "vscode-languageclient": "^4.1.4"
}
```

언급 한대로, 클라이언트는 일반적인 VS Code 익스텐션으로 구현되어있으며, 모든 VS Code namespace API에 액세스 할 수 있습니다. 

<!--
As mentioned, the client is implemented as a normal VS Code extension, and it has access to all VS Code namespace API. -->

아래는 **lsp-sample**의 항목에 해당하는 extension.ts 파일의 내용입니다. 

<!--
Below is the content of the corresponding extension.ts file, which is the entry of the **lsp-sample** extension: -->

```typescript
import * as path from 'path';
import { workspace, ExtensionContext } from 'vscode';

import {
  LanguageClient,
  LanguageClientOptions,
  ServerOptions,
  TransportKind
} from 'vscode-languageclient';

let client: LanguageClient;

export function activate(context: ExtensionContext) {
  // The server is implemented in node
  let serverModule = context.asAbsolutePath(path.join('server', 'out', 'server.js'));
  // The debug options for the server
  // --inspect=6009: runs the server in Node's Inspector mode so VS Code can attach to the server for debugging
  let debugOptions = { execArgv: ['--nolazy', '--inspect=6009'] };

  // If the extension is launched in debug mode then the debug server options are used
  // Otherwise the run options are used
  let serverOptions: ServerOptions = {
    run: { module: serverModule, transport: TransportKind.ipc },
    debug: {
      module: serverModule,
      transport: TransportKind.ipc,
      options: debugOptions
    }
  };

  // Options to control the language client
  let clientOptions: LanguageClientOptions = {
    // Register the server for plain text documents
    documentSelector: [{ scheme: 'file', language: 'plaintext' }],
    synchronize: {
      // Notify the server about file changes to '.clientrc files contained in the workspace
      fileEvents: workspace.createFileSystemWatcher('**/.clientrc')
    }
  };

  // Create the language client and start the client.
  client = new LanguageClient(
    'languageServerExample',
    'Language Server Example',
    serverOptions,
    clientOptions
  );

  // Start the client. This will also launch the server
  client.start();
}

export function deactivate(): Thenable<void> {
  if (!client) {
    return undefined;
  }
  return client.stop();
}
```

### '언어 서버'에 대한 설명

<!--
### Explaining the 'Language Server' -->

> **주의:** 깃허브 저장소에서 클론된 '서버' 구현은 최종 결과물이 있습니다. 진행을 위해서 새로운 `server.ts`를 생성하거나, 클론된 버전의 내용을 수정하십시오. 

<!--
> **Note:** The 'Server' implementation cloned from the GitHub repository has the final walkthrough implementation. To follow the walkthrough, you can create a new `server.ts` or modify the contents of the cloned version.-->

예시에서, 서버는 타입스크립트로 구현되고 Node.js를 통해 실행 됩니다. VS Code가 Node.js 런타임을 포함하기 때문에, 런타임을 위한 특정한 요구사항이 없는 이상, 이를 추가로 제공 할 필요는 없습니다. 

<!--
In the example, the server is also implemented in TypeScript and executed using Node.js. Since VS Code already ships with a Node.js runtime, there is no need to provide your own, unless you have specific requirements for the runtime. -->

언어 서버를 위한 소스 코드는 `/server`에 위치합니다. 서버의 `package.json` 파일의 흥미로운 부분은 :

<!--
The source code for the Language Server is at `/server`. The interesting section in the server's `package.json` file is: -->

```json
"dependencies": {
    "vscode-languageserver": "^4.1.3"
}
```

이는 `vscode-languageserver` 라이브러리를 호출 합니다.

<!--
This pulls in the `vscode-languageserver` library. -->

아래는 VS Code에서 서버로 텍스트 문서의 내용을 항상 전송하여 동기화 하는 텍스트 문서 관리자를 사용하는 서버 구현 입니다. 

<!--
Below is a server implementation that uses the provided simple text document manager that synchronizes text documents by always sending the file's full content from VS Code to the server. -->

```typescript
import {
  createConnection,
  TextDocuments,
  TextDocument,
  Diagnostic,
  DiagnosticSeverity,
  ProposedFeatures,
  InitializeParams,
  DidChangeConfigurationNotification,
  CompletionItem,
  CompletionItemKind,
  TextDocumentPositionParams
} from 'vscode-languageserver';

// Create a connection for the server. The connection uses Node's IPC as a transport.
// Also include all preview / proposed LSP features.
let connection = createConnection(ProposedFeatures.all);

// Create a simple text document manager. The text document manager
// supports full document sync only
let documents: TextDocuments = new TextDocuments();

let hasConfigurationCapability: boolean = false;
let hasWorkspaceFolderCapability: boolean = false;
let hasDiagnosticRelatedInformationCapability: boolean = false;

connection.onInitialize((params: InitializeParams) => {
  let capabilities = params.capabilities;

  // Does the client support the `workspace/configuration` request?
  // If not, we will fall back using global settings
  hasConfigurationCapability =
    capabilities.workspace && !!capabilities.workspace.configuration;
  hasWorkspaceFolderCapability =
    capabilities.workspace && !!capabilities.workspace.workspaceFolders;
  hasDiagnosticRelatedInformationCapability =
    capabilities.textDocument &&
    capabilities.textDocument.publishDiagnostics &&
    capabilities.textDocument.publishDiagnostics.relatedInformation;

  return {
    capabilities: {
      textDocumentSync: documents.syncKind,
      // Tell the client that the server supports code completion
      completionProvider: {
        resolveProvider: true
      }
    }
  };
});

connection.onInitialized(() => {
  if (hasConfigurationCapability) {
    // Register for all configuration changes.
    connection.client.register(DidChangeConfigurationNotification.type, undefined);
  }
  if (hasWorkspaceFolderCapability) {
    connection.workspace.onDidChangeWorkspaceFolders(_event => {
      connection.console.log('Workspace folder change event received.');
    });
  }
});

// The example settings
interface ExampleSettings {
  maxNumberOfProblems: number;
}

// The global settings, used when the `workspace/configuration` request is not supported by the client.
// Please note that this is not the case when using this server with the client provided in this example
// but could happen with other clients.
const defaultSettings: ExampleSettings = { maxNumberOfProblems: 1000 };
let globalSettings: ExampleSettings = defaultSettings;

// Cache the settings of all open documents
let documentSettings: Map<string, Thenable<ExampleSettings>> = new Map();

connection.onDidChangeConfiguration(change => {
  if (hasConfigurationCapability) {
    // Reset all cached document settings
    documentSettings.clear();
  } else {
    globalSettings = <ExampleSettings>(
      (change.settings.languageServerExample || defaultSettings)
    );
  }

  // Revalidate all open text documents
  documents.all().forEach(validateTextDocument);
});

function getDocumentSettings(resource: string): Thenable<ExampleSettings> {
  if (!hasConfigurationCapability) {
    return Promise.resolve(globalSettings);
  }
  let result = documentSettings.get(resource);
  if (!result) {
    result = connection.workspace.getConfiguration({
      scopeUri: resource,
      section: 'languageServerExample'
    });
    documentSettings.set(resource, result);
  }
  return result;
}

// Only keep settings for open documents
documents.onDidClose(e => {
  documentSettings.delete(e.document.uri);
});

// The content of a text document has changed. This event is emitted
// when the text document first opened or when its content has changed.
documents.onDidChangeContent(change => {
  validateTextDocument(change.document);
});

async function validateTextDocument(textDocument: TextDocument): Promise<void> {
  // In this simple example we get the settings for every validate run.
  let settings = await getDocumentSettings(textDocument.uri);

  // The validator creates diagnostics for all uppercase words length 2 and more
  let text = textDocument.getText();
  let pattern = /\b[A-Z]{2,}\b/g;
  let m: RegExpExecArray;

  let problems = 0;
  let diagnostics: Diagnostic[] = [];
  while ((m = pattern.exec(text)) && problems < settings.maxNumberOfProblems) {
    problems++;
    let diagnostic: Diagnostic = {
      severity: DiagnosticSeverity.Warning,
      range: {
        start: textDocument.positionAt(m.index),
        end: textDocument.positionAt(m.index + m[0].length)
      },
      message: `${m[0]} is all uppercase.`,
      source: 'ex'
    };
    if (hasDiagnosticRelatedInformationCapability) {
      diagnostic.relatedInformation = [
        {
          location: {
            uri: textDocument.uri,
            range: Object.assign({}, diagnostic.range)
          },
          message: 'Spelling matters'
        },
        {
          location: {
            uri: textDocument.uri,
            range: Object.assign({}, diagnostic.range)
          },
          message: 'Particularly for names'
        }
      ];
    }
    diagnostics.push(diagnostic);
  }

  // Send the computed diagnostics to VS Code.
  connection.sendDiagnostics({ uri: textDocument.uri, diagnostics });
}

connection.onDidChangeWatchedFiles(_change => {
  // Monitored files have change in VS Code
  connection.console.log('We received an file change event');
});

// This handler provides the initial list of the completion items.
connection.onCompletion(
  (_textDocumentPosition: TextDocumentPositionParams): CompletionItem[] => {
    // The pass parameter contains the position of the text document in
    // which code complete got requested. For the example we ignore this
    // info and always provide the same completion items.
    return [
      {
        label: 'TypeScript',
        kind: CompletionItemKind.Text,
        data: 1
      },
      {
        label: 'JavaScript',
        kind: CompletionItemKind.Text,
        data: 2
      }
    ];
  }
);

// This handler resolves additional information for the item selected in
// the completion list.
connection.onCompletionResolve(
  (item: CompletionItem): CompletionItem => {
    if (item.data === 1) {
      item.detail = 'TypeScript details';
      item.documentation = 'TypeScript documentation';
    } else if (item.data === 2) {
      item.detail = 'JavaScript details';
      item.documentation = 'JavaScript documentation';
    }
    return item;
  }
);

/*
connection.onDidOpenTextDocument((params) => {
    // A text document got opened in VS Code.
    // params.uri uniquely identifies the document. For documents store on disk this is a file URI.
    // params.text the initial full content of the document.
    connection.console.log(`${params.textDocument.uri} opened.`);
});
connection.onDidChangeTextDocument((params) => {
    // The content of a text document did change in VS Code.
    // params.uri uniquely identifies the document.
    // params.contentChanges describe the content changes to the document.
    connection.console.log(`${params.textDocument.uri} changed: ${JSON.stringify(params.contentChanges)}`);
});
connection.onDidCloseTextDocument((params) => {
    // A text document got closed in VS Code.
    // params.uri uniquely identifies the document.
    connection.console.log(`${params.textDocument.uri} closed.`);
});
*/

// Make the text document manager listen on the connection
// for open, change and close text document events
documents.listen(connection);

// Listen on the connection
connection.listen();
```

### 간단한 검증 추가
<!--
### Adding a Simple Validation -->

서버에 문서 검증을 추가 하기 위해, 텍스트 문서 관리자에 텍스트 문서 의 내용이 바뀔때마다 호출 되는 리스너를 추가할 것입니다. 문서를 검증하기 위한 최적의 시간을 결정하는 것은 서버가 결정합니다. 예시 구현에서는 서버는 텍스트 문서를 검증하고, ALL CAPS를 사용하는 모든 단어를 표기 합니다. 해당하는 코드 snippet은 이와 같습니다:

<!--
To add document validation to the server, we add a listener to the text document manager that gets called whenever the content of a text document changes. It is then up to the server to decide when the best time is to validate a document. In the example implementation, the server validates the plain text document and flags all occurrences of words that use ALL CAPS. The corresponding code snippet looks like this: -->

```typescript
// The content of a text document has changed. This event is emitted
// when the text document first opened or when its content has changed.
documents.onDidChangeContent(async (change) => {
    // In this simple example we get the settings for every validate run.
    let settings = await getDocumentSettings(textDocument.uri);

    // The validator creates diagnostics for all uppercase words length 2 and more
    let text = textDocument.getText();
    let pattern = /\b[A-Z]{2,}\b/g;
    let m: RegExpExecArray;

    let problems = 0;
    let diagnostics: Diagnostic[] = [];
    while ((m = pattern.exec(text))) {
        problems++;
        let diagnostic: Diagnostic = {
            severity: DiagnosticSeverity.Warning,
            range: {
                start: textDocument.positionAt(m.index),
                end: textDocument.positionAt(m.index + m[0].length)
            },
            message: `${m[0]} is all uppercase.`,
            source: 'ex'
        };
        if (hasDiagnosticRelatedInformationCapability) {
            diagnostic.relatedInformation = [
                {
                    location: {
                        uri: textDocument.uri,
                        range: Object.assign({}, diagnostic.range)
                    },
                    message: 'Spelling matters'
                },
                {
                    location: {
                        uri: textDocument.uri,
                        range: Object.assign({}, diagnostic.range)
                    },
                    message: 'Particularly for names'
                }
            ];
        }
        diagnostics.push(diagnostic);
    }

    // Send the computed diagnostics to VS Code.
    connection.sendDiagnostics({ uri: textDocument.uri, diagnostics });
}
```

### 진단 팁 및 요령

<!-- ### Diagnostics Tips and Tricks -->

- 시작과 끝 위치가 동일하다면, VS Code는 해당 위치에서 단어를 강조 할 것입니다.
- 줄의 마지막까지 밑줄을 표기하려면, 끝 위치의 문자를 Number.MAX_VALUE로 설정하십시오.

<!--
- If the start and end positions are the same, VS Code will underline with a squiggle the word at that position.
- If you want to underline with a squiggle until the end of the line, then set the character of the end position to Number.MAX_VALUE.
-->

언어 서버를 실행하기 위해, 다음을 실행 하십시오: 
<!--
To run the Language Server, do the following: -->

- `kb(workbench.action.tasks.build)`를 눌러 빌드 작업을 시작하십시오. 작업은 클라이언트와 서버 둘 다 컴파일합니다.  
- 디버그 viewlet 을 열고, `Launch Client`실행 구성을 선택한뒤, `Start Debugging` 버튼을 눌러 익스텐션 코드를 실행하는 VS Code의 추가 `Extension Development Host` 인스턴스를 실행하십시오. 
- 루트 폴더에 text.txt 파일을 생성하고 아래 내용을 복사 하십시오:

<!--
- press `kb(workbench.action.tasks.build)` to start the build task. The task compiles both the client and the server.
- open the debug viewlet, select the `Launch Client` launch configuration, and press the `Start Debugging` button to launch an additional `Extension Development Host` instance of VS Code that executes the extension code.
- Create a test.txt file in the root folder and paste the following content:-->

```bash
TypeScript lets you write JavaScript the way you really want to.
TypeScript is a typed superset of JavaScript that compiles to plain JavaScript.
ANY browser. ANY host. ANY OS. Open Source.
```

`Extension Development Host` 인스턴스는 다음과 같습니다: 

<!--
The `Extension Development Host` instance will then look like this: -->

![Validating a text file](images/language-server-extension-guide/validation.png)

### 클라이언트와 서버 디버그

<!--
### Debugging both Client and Server -->

클라이언트 코드를 디버그 하는것은 일반 익스텐션을 디버그 하는것 처럼 쉽습니다. 클라이언트 코드 안에 중지점을 설정하고 `kb(workbench.action.debug.start)`를 눌러 익스텐션을 디버그 하십시오. 

<!--
Debugging the client code is as easy as debugging a normal extension. Set a breakpoint in the client code and debug the extension by pressing `kb(workbench.action.debug.start)`. -->

![Debugging the client](images/language-server-extension-guide/debugging-client.png)

익스텐션(클라이언트) 에서 실행되는 `LanguageClient`에 의해 서버가 시작되므로, 실행중인 서버에 디버거를 연결해야 합니다. 이를 위해 디버그 뷰로 전환한 다음 구성 설정 `Attach to Server`를 선택한 뒤 `kb(workbench.action.debug.start)`를 누르십시오. 디버거가 서버에 연결 될 것입니다.

<!--
Since the server is started by the `LanguageClient` running in the extension (client), we need to attach a debugger to the running server. To do so, switch to the Debug view and select the launch configuration `Attach to Server` and press `kb(workbench.action.debug.start)`. This will attach the debugger to the server. -->

![Debugging the server](images/language-server-extension-guide/debugging-server.png)

### 언어 서버를 위한 로그 지원

<!--
### Logging Support for Language Server -->

`vscode-languageclient`를 클라이언트 구현에 사용한다면, 언어 클라이언트 / 서버 간의 커뮤니케이션을 언어 클라이언트의 `name`채널에 기록 하도록 `[langId].trace.server`를 설정 할 수 있습니다. 

<!--
If you are using `vscode-languageclient` to implement the client, you can specify a setting `[langId].trace.server` that instructs the Client to log communications between Language Client / Server to a channel of the Language Client's `name`. -->

**lsp-sample**에 대하여, 이 설정을 `"languageServerExample.trace.server": "verbose"`로 설정할 수 있습니다. 이제 "Language Server Example" 채널을 확인하면 로그를 확인 할 수 있습니다 : 

<!--
For **lsp-sample**, you can set this setting: `"languageServerExample.trace.server": "verbose"`. Now head to the channel "Language Server Example". You should see the logs: -->

![LSP Log](images/language-server-extension-guide/lsp-log.png)

언어 서버의 내용이 많으므로 (상용 서버는 5초 동안 5천줄의 로그를 생성), 언어 클라이언트 / 서버간의 커뮤니케이션을 시각화 하고 필터 할 수 있는 도구를 제공합니다. 채널의 모든 로그를 파일로 저장하고, [https://microsoft.github.io/language-server-protocol/inspector](https://microsoft.github.io/language-server-protocol/inspector)의 [Language Server Protocol Inspector](https://github.com/Microsoft/language-server-protocol-inspector)를 이용하여 해당 파일을 불러올 수 있습니다.  

<!--
As Language Servers can be chatty (5 seconds of real-world usage can produce 5000 lines of log), we also provide a tool to visualize and filter the communication between Language Client / Server. You can save all logs from the channel into a file, and load that file with the [Language Server Protocol Inspector](https://github.com/Microsoft/language-server-protocol-inspector) at [https://microsoft.github.io/language-server-protocol/inspector](https://microsoft.github.io/language-server-protocol/inspector).-->

![LSP Inspector](images/language-server-extension-guide/lsp-inspector.png)

### 서버 구성 설정
<!--
### Using Configuration Settings in the Server -->

익스텐션의 클라이언트 부분을 작성할때, 보고 되는 문제의 최대 수를 조절하는 설정을 이미 정의 했습니다. 또한 서버 측에서 클라이언트로부터 설정을 읽는 코드를 작성 했습니다:

<!--
When writing the client part of the extension, we already defined a setting to control the maximum numbers of problems reported. We also wrote code on the server side to read these settings from the client: -->

```typescript
function getDocumentSettings(resource: string): Thenable<ExampleSettings> {
  if (!hasConfigurationCapability) {
    return Promise.resolve(globalSettings);
  }
  let result = documentSettings.get(resource);
  if (!result) {
    result = connection.workspace.getConfiguration({
      scopeUri: resource,
      section: 'languageServerExample'
    });
    documentSettings.set(resource, result);
  }
  return result;
}
```

해야 할것은 서버 쪽에서 구성 변경 사항을 들어 설정이 변경 된 경우, 열린 텍스트 문서를 다시 검증 하는 것입니다. 문서 변경 이벤트 처리의 검증 로직을 재사용 할 수 있도록, 코드를 `validateTextDocument` 함수로 추출하고 `maxNumberOfProblems` 변수를 준수하도록 수정합니다:

<!--
The only thing we need to do now is to listen to configuration changes on the server side and if a settings changes, revalidate the open text documents. To be able to reuse the validate logic of the document change event handling, we extract the code into a `validateTextDocument` function and modify the code to honor a `maxNumberOfProblems` variable: -->

```typescript
async function validateTextDocument(textDocument: TextDocument): Promise<void> {
  // In this simple example we get the settings for every validate run.
  let settings = await getDocumentSettings(textDocument.uri);

  // The validator creates diagnostics for all uppercase words length 2 and more
  let text = textDocument.getText();
  let pattern = /\b[A-Z]{2,}\b/g;
  let m: RegExpExecArray;

  let problems = 0;
  let diagnostics: Diagnostic[] = [];
  while ((m = pattern.exec(text)) && problems < settings.maxNumberOfProblems) {
    problems++;
    let diagnostic: Diagnostic = {
      severity: DiagnosticSeverity.Warning,
      range: {
        start: textDocument.positionAt(m.index),
        end: textDocument.positionAt(m.index + m[0].length)
      },
      message: `${m[0]} is all uppercase.`,
      source: 'ex'
    };
    if (hasDiagnosticRelatedInformationCapability) {
      diagnostic.relatedInformation = [
        {
          location: {
            uri: textDocument.uri,
            range: Object.assign({}, diagnostic.range)
          },
          message: 'Spelling matters'
        },
        {
          location: {
            uri: textDocument.uri,
            range: Object.assign({}, diagnostic.range)
          },
          message: 'Particularly for names'
        }
      ];
    }
    diagnostics.push(diagnostic);
  }

  // Send the computed diagnostics to VS Code.
  connection.sendDiagnostics({ uri: textDocument.uri, diagnostics });
}
```

구성 변경 처리는 구성 변경에 대한 알림 핸들러를 연결에 추가하여 수행 됩니다. 해당하는 코드는 이와 같습니다:

<!--
The handling of the configuration change is done by adding a notification handler for configuration changes to the connection. The corresponding code looks like this: -->

```typescript
connection.onDidChangeConfiguration(change => {
  if (hasConfigurationCapability) {
    // Reset all cached document settings
    documentSettings.clear();
  } else {
    globalSettings = <ExampleSettings>(
      (change.settings.languageServerExample || defaultSettings)
    );
  }

  // Revalidate all open text documents
  documents.all().forEach(validateTextDocument);
});
```

클라이언트를 다시 시작하고 최대 1개만 보고 하도록 설정을 변경하면 다음과 같은 검증을 수행합니다. 

<!--
Starting the client again and changing the setting to maximum report 1 problem results in the following validation:-->

![Maximum One Problem](images/language-server-extension-guide/validationOneProblem.png)

### 추가 언어 기능 추가 

<!--
### Adding additional Language Features -->

언어 서버에서 일반적으로 구현하는 흥미로운 기능은 문서의 검증입니다. 그런 의미에서 linter는 언어 서버로 간주되고, VS Code linter는 언어 서버로 구현됩니다 (예를 위하여, [eslint](https://github.com/Microsoft/vscode-eslint) 와 [jshint](https://github.com/Microsoft/vscode-jshint) 를 참조하십시오). 그러나 언어 서버에는 다른 것도 있습니다. 언어 서버는 코드완성, 모든 참조 검색 혹은 정의로 이동을 제공 할 수 있습니다. 아래의 예시 보드는 서버에 코드 완성 기능을 추가합니다. 이는 '타입스크립트'와 '자바스크립트'를 지원합니다. 

<!--
The first interesting feature a language server usually implements is validation of documents. In that sense, even a linter counts as a language server and in VS Code linters are usually implemented as language servers (see [eslint](https://github.com/Microsoft/vscode-eslint) and [jshint](https://github.com/Microsoft/vscode-jshint) for examples). But there is more to language servers. They can provide code completion, Find All References, or Go To Definition. The example code below adds code completion to the server. It proposes the two words 'TypeScript' and 'JavaScript'. -->

```typescript
// This handler provides the initial list of the completion items.
connection.onCompletion(
  (_textDocumentPosition: TextDocumentPositionParams): CompletionItem[] => {
    // The pass parameter contains the position of the text document in
    // which code complete got requested. For the example we ignore this
    // info and always provide the same completion items.
    return [
      {
        label: 'TypeScript',
        kind: CompletionItemKind.Text,
        data: 1
      },
      {
        label: 'JavaScript',
        kind: CompletionItemKind.Text,
        data: 2
      }
    ];
  }
);

// This handler resolves additional information for the item selected in
// the completion list.
connection.onCompletionResolve(
  (item: CompletionItem): CompletionItem => {
    if (item.data === 1) {
      item.detail = 'TypeScript details';
      item.documentation = 'TypeScript documentation';
    } else if (item.data === 2) {
      item.detail = 'JavaScript details';
      item.documentation = 'JavaScript documentation';
    }
    return item;
  }
);
```

`data`필드는 리졸버 핸들러에서 완료된 항목을 식별하기 위해 쓰입니다. 데이터 속성은 프로토콜에 투명합니다. 기본 메세지 전달 프로토콜은 JSON기반이기 때문에, 데이터 필드는 JSON과 직렬화 할 수 있는 데이터만 보유 해야 합니다.

<!--
The `data` fields are used to uniquely identify a completion item in the resolve handler. The data property is transparent for the protocol. Since the underlying message passing protocol is JSON-based, the data field should only hold data that is serializable to and from JSON. -->

이제 빠진 것은 VS Code에 서버가 코드 완성 요청을 지원한다는 것을 알리는 것입니다. 이를 위해 초기화 핸들러에서 해당 기능에 플래그를 지정하십시오:

<!--
All that is missing is to tell VS Code that the server supports code completion requests. To do so, flag the corresponding capability in the initialize handler: -->

```typescript
connection.onInitialize((params): InitializeResult => {
    ...
    return {
        capabilities: {
            ...
            // Tell the client that the server supports code completion
            completionProvider: {
                resolveProvider: true
            }
        }
    };
});
```

아래의 스크린샷은 텍스트 파일에서 코드 완성을 실행한 결과 입니다:
<!--
The screenshot below shows the completed code running on a plain text file: -->

![Code Complete](images/language-server-extension-guide/codeComplete.png)

### 언어 서버 테스트

<!--
### Testing The Language Server -->

언어 서버의 높은 품질을 위해, 좋은 기능 테스트를 작성 해야 합니다. 언어 서버를 테스트 하는 방법에는 2가지 일반적인 방법이 있습니다:

<!--
To create a high-quality Language Server, we need to build a good test suite covering its functionalities. There are two common ways of testing Language Servers: -->

- 단위 테스트 : 언어 서버로 전송되는 모든 정보를 예시로, 특정 기능을 테스트를 원할때 유용합니다. VS Code의 [HTML](https://github.com/Microsoft/vscode-html-languageservice) / [CSS](https://github.com/Microsoft/vscode-css-languageservice) / [JSON](https://github.com/Microsoft/vscode-json-languageservice) 언어 서버는 테스트를 위해 이 방법을 사용합니다. LSP npm 모듈 또한 이를 사용합니다. [이곳](https://github.com/Microsoft/vscode-languageserver-node/blob/master/protocol/src/test/connection.test.ts)에서 npm 프로토콜 모듈을 사용하여 작성된 단위 테스트를 참조하십시오. 
- End-to-End 테스트 : 이는 [VS Code 익스텐션 테스트](/api/working-with-extensions/testing-extension)와 유사합니다. 이 방법의 장점은 작업공간으로 VS Code를 인스턴스화 하고, 파일을 열어, 언어 서버/클라이언트를 활성화 한후 [VS Code 커맨드](/api/references/commands)를 통해 실행 한다는 것입니다. 이는 예시를 작성하기 어려운 파일, 설정, 혹은 의존성(`node_modules`와 같은)을 가지고 있을때 우수합니다. 유명한 [Python](https://github.com/Microsoft/vscode-python) 익스텐션이 이 방법을 테스트에 사용합니다. 

<!--
- Unit Test: This is useful if you want to test specific functionalities in Language Servers by mocking up all the information being sent to it. VS Code's [HTML](https://github.com/Microsoft/vscode-html-languageservice) / [CSS](https://github.com/Microsoft/vscode-css-languageservice) / [JSON](https://github.com/Microsoft/vscode-json-languageservice) Language Servers take this approach to testing. The LSP npm modules itself use the approach. See [here](https://github.com/Microsoft/vscode-languageserver-node/blob/master/protocol/src/test/connection.test.ts) for some unit test written using the npm protocol module.
- End-to-End Test: This is similar to [VS Code extension test](/api/working-with-extensions/testing-extension). The benefit of this approach is that it runs the test by instantiating a VS Code instance with a workspace, opening the file, activating the Language Client / Server, and running [VS Code commands](/api/references/commands). This approach is superior if you have files, settings, or dependencies (such as `node_modules`) which are hard or impossible to mock. The popular [Python](https://github.com/Microsoft/vscode-python) extension takes this approach to testing.-->

모든 선택한 테스트 프레임워크에서 단위테스트를 수행 할 수 있습니다. 여기서는 언어 서버 익스텐션을 위한 End-to-End 테스팅을 하는 방법에 대해 설명합니다.

<!--
It is possible to do Unit Test in any testing framework of your choice. Here we describe how to do End-to-End testing for Language Server Extension. -->

`.vscode/launch.json`을 열어, `E2E` 테스트 목표를 확인하십시오: 

<!-- Open `.vscode/launch.json`, and you can find a `E2E` test target:-->

```json
{
  "name": "Language Server E2E Test",
  "type": "extensionHost",
  "request": "launch",
  "runtimeExecutable": "${execPath}",
  "args": [
    "--extensionDevelopmentPath=${workspaceRoot}",
    "--extensionTestsPath=${workspaceRoot}/client/out/test",
    "${workspaceRoot}/client/testFixture"
  ],
  "stopOnEntry": false,
  "sourceMaps": true,
  "outFiles": ["${workspaceRoot}/client/out/test/**/*.js"]
}
```

디버그 대상을 실행하면, `client/testFixture`를 활성 작업 공간으로 사용하는 VS Code 인스턴스를 실행 할 것입니다. VS Code는 이후 `client/src/test`에 있는 모든 테스트를 실행합니다. 디버깅 팁으로, `client/src/test` 내부에 타입스크립트 파일에 중단점을 설정할 수 있습니다. 

<!--
If you run this debug target, it will launch a VS Code instance with `client/testFixture` as the active workspace. VS Code will then proceed to execute all tests in `client/src/test`. As a debugging tip, you can set breakpoints in TypeScript files in `client/src/test` and they will be hit. -->

`completion.test.ts` 파일을 확인해보면:
<!--
Let's take a look at the `completion.test.ts` file: -->

```ts
import * as vscode from 'vscode';
import * as assert from 'assert';
import { getDocUri, activate } from './helper';

describe('Should do completion', () => {
  const docUri = getDocUri('completion.txt');

  it('Completes JS/TS in txt file', async () => {
    await testCompletion(docUri, new vscode.Position(0, 0), {
      items: [
        { label: 'JavaScript', kind: vscode.CompletionItemKind.Text },
        { label: 'TypeScript', kind: vscode.CompletionItemKind.Text }
      ]
    });
  });
});

async function testCompletion(
  docUri: vscode.Uri,
  position: vscode.Position,
  expectedCompletionList: vscode.CompletionList
) {
  await activate(docUri);

  // Executing the command `vscode.executeCompletionItemProvider` to simulate triggering completion
  const actualCompletionList = (await vscode.commands.executeCommand(
    'vscode.executeCompletionItemProvider',
    docUri,
    position
  )) as vscode.CompletionList;

  assert.equal(actualCompletionList.items.length, expectedCompletionList.items.length);
  expectedCompletionList.items.forEach((expectedItem, i) => {
    const actualItem = actualCompletionList.items[i];
    assert.equal(actualItem.label, expectedItem.label);
    assert.equal(actualItem.kind, expectedItem.kind);
  });
}
```

이 테스트에서 : 

- 익스텐션을 활성화.
- URI와 완성을 작동할 위치를 사용하여 `vscode.executeCompletionItemProvider` 커맨드를 실행.
- 반환 완료 항목을 예상 완료 항목과 비교하십시오.


<!--
In this test, we: -->
<!--
- Activate the extension.
- Run the command `vscode.executeCompletionItemProvider` with a URI and a position to simulate completion trigger.
- Assert the returned completion items against our expected completion items.
-->


`activate(docURI)` 함수에 대하여 조금 더 알아 보겠습니다. 이는 `client/src/test/helper.ts`에 정의되어 있습니다:

<!--
Let's dive a bit deeper into the `activate(docURI)` function. It is defined in `client/src/test/helper.ts`: -->

```ts
import * as vscode from 'vscode';
import * as path from 'path';

export let doc: vscode.TextDocument;
export let editor: vscode.TextEditor;
export let documentEol: string;
export let platformEol: string;

/**
 * Activates the vscode.lsp-sample extension
 */
export async function activate(docUri: vscode.Uri) {
  // The extensionId is `publisher.name` from package.json
  const ext = vscode.extensions.getExtension('vscode.lsp-sample');
  await ext.activate();
  try {
    doc = await vscode.workspace.openTextDocument(docUri);
    editor = await vscode.window.showTextDocument(doc);
    await sleep(2000); // Wait for server activation
  } catch (e) {
    console.error(e);
  }
}

async function sleep(ms: number) {
  return new Promise(resolve => setTimeout(resolve, ms));
}
```

활성화 부분에서 : 

<!-- In the activation part, we: -->

- `package.json`에 정의 되어있는, `{publisher.name}.{extensionId}`를 이용하여 익스텐션 정보를 확보.
- 특정 문서를 열고, 활성된 텍스트 에디터에 보임.
- 2초 간 대기, 이를 통해 언어 서버가 인스턴스 되었다는 것을 확인.

<!--
- Get the extension using the `{publisher.name}.{extensionId}`, as defined in `package.json`.
- Open the specified document, and show it in the active text editor.
- Sleep for 2 seconds, so we are sure the Language Server is instantiated.-->

준비가 된 이후, 각 언어 기능에 대항다는 [VS Code 커맨드](/api/references/commands)를 실행하고, 반환된 결과와 비교 할 수 있습니다. 

<!--
After the preparation, we can run the [VS Code Commands](/api/references/commands) corresponding to each language feature, and assert against the returned result.-->

방금 구현한 진한 기능을 다루는 테스트가 한가지 더 있습니다. 이를 `client/src/test/diagnostics.test.ts`에서 확인하십시오. 

<!--
There is one more test that covers the diagnostics feature that we just implemented. Check it out at `client/src/test/diagnostics.test.ts`. -->

## 고급 주제
<!--
## Advanced Topics -->

여태까지, 가이드에서 다룬것은 : 

<!--
So far, this guide covered: -->

- 언어 서버와 언어 서버 프로토콜에 대한 개요
- VS Code의 언어 서버 익스텐션의 구조
- **lsp-sample** 익스텐션과, 이를 개발/디버그/조사/테스트 하는 방법입니다. 

<!--
- A brief overview of Language Server and Language Server Protocol.
- Architecture of a Language Server extension in VS Code
- The **lsp-sample** extension, and how to develop/debug/inspect/test it.-->

이 가이드에 적합하지 않은 고급 주제가 몇 가지 있습니다. 언어 서버 개발에 대한 추가 공부를 위해 이러한 리소스에 대한 링크를 포함시키겠습니다. 

<!--
There are some more advanced topics we could not fit in to this guide. We will include links to these resources for further studying of Language Server development. -->

### 추가 언어 서버 기능들
<!--
### Additional Language Server features -->

아래의 언어 기능들은 코드 완성과 같이 현재 언어 서버에서 지원 됩니다. 

<!--
The following language features are currently supported in a language server along with code completions: -->


- _Document Highlights_: 문서의 모든 '동일한' 심볼을 강조합니다.
- _Hover_: 문서의 선택된 심볼에 대해 호버링 도움말을 제공합니다.
- _Signature Help_: 문서의 선택된 심볼에 대한 시그니쳐 도움말을 제공합니다. 
- _Goto Definition_: 문서의 선택된 심볼에 대한 정의로 이동을 제공합니다. 
- _Goto Type Definition_: 문서의 선택된 심볼에 대한 타입/인터페이스 정의 지원으로 이동을 제공합니다. 
- _Goto Implementation_: 문서의 선택된 심볼에 대한 구현 정의로 이동을 제공합니다. 
- _Find References_: 문서의 선택된 심볼에 대하여 모든 프로젝트에 대한 참조를 검색합니다. 
- _List Document Symbols_: 문서의 선택된 심볼에 대한 목록을 제공합니다. 
- _List Workspace Symbols_: 모든 프로젝트의 심볼에 대한 목록을 제공합니다. 
- _Code Actions_: 주어진 문서와 범위에 대하여 실행할 커맨드을 수행합니다. (일반적으로 beautify/refactor)
- _CodeLens_: 주어진 문서에 대해 CodeLens 통계치를 계산합니다. 
- _Document Formatting_: 이는 전체 문서 포맷, 문서 범위 및 타입에 대한 포맷을 포함합니다.
- _Rename_: 프로젝트 전체의 심볼 이름 변경.
- _Document Links_: 문서 내의 링크를 계산, 연결합니다. 
- _Document Colors_: 문서 내부의 색상을 계산, 연결하여 에디터에 색상 선택기를 제공합니다. 

<!--
- _Document Highlights_: highlights all 'equal' symbols in a text document.
- _Hover_: provides hover information for a symbol selected in a text document.
- _Signature Help_: provides signature help for a symbol selected in a text document.
- _Goto Definition_: provides go to definition support for a symbol selected in a text document.
- _Goto Type Definition_: provides go to type/interface definition support for a symbol selected in a text document.
- _Goto Implementation_: provides go to implementation definition support for a symbol selected in a text document.
- _Find References_: finds all project-wide references for a symbol selected in a text document.
- _List Document Symbols_: lists all symbols defined in a text document.
- _List Workspace Symbols_: lists all project-wide symbols.
- _Code Actions_: compute commands to run (typically beautify/refactor) for a given text document and range.
- _CodeLens_: compute CodeLens statistics for a given text document.
- _Document Formatting_: this includes formatting of whole documents, document ranges and formatting on type.
- _Rename_: project-wide rename of a symbol.
- _Document Links_: compute and resolve links inside a document.
- _Document Colors_: compute and resolve colors inside a document to provide color picker in editor.
-->

[프로그래밍 언어 기능](/api/language-extensions/programmatic-language-features) 주제에서 위의 각 언어 기능을 설명하고 언어 서버 프로토콜을 통해서나 익스텐션에서 확장성 API를 직접 이용하여 기능들을 구현 하는 방법을 제공합니다. 

<!--
The [Programmatic Language Features](/api/language-extensions/programmatic-language-features) topic describes each of the language features above and provides guidance on how to implement them either through the language server protocol or by using the extensibility API directly from your extension.-->

### 증분 텍스트 문서 동기화
<!--
### Incremental Text Document Synchronization-->

이 예제에서는 `vscode-languageserver`모듈에서 제공된 간단한 텍스트 문서 관리자를 이용하여 VS Code와 언어 서버 간에 문서를 동기화 합니다. 

<!--
The example uses the simple text document manager provided by the `vscode-languageserver` module to synchronize documents between VS Code and the language server. -->

이는 두가지 단점을 가지고 있습니다:

<!-- This has two drawbacks:-->

- 반복적으로 서버에 문서의 전체 내용을 전송하므로 데이터 전달이 많습니다. 
- 기존 언어 라이브러리를 사용하는 경우, 일반적으로 불필요한 파싱과 추상 구문 트리 작성을 위해 증분 문서 업데이트를 지원합니다. 

<!--
- Lots of data transfer since the whole content of a text document is sent to the server repeatedly.
- If an existing language library is used, such libraries usually support incremental document updates to avoid unnecessary parsing and abstract syntax tree creation. -->

따라서 이 프로토콜은 증분 문서 동기화도 지원합니다. 

<!-- The protocol therefore supports incremental document synchronization as well.-->

증분 문서 동기화를 사용하기 위해, 서버는 3가지 알림 핸들러를 설치해야 합니다:

<!--
To make use of incremental document synchronization, a server needs to install three notification handlers:-->

- _onDidOpenTextDocument_: VS Code에서 문서가 열릴때 호출됩니다. 
- _onDidChangeTextDocument_: VS Code에서 문서가 바뀌었을때 호출됩니다. 
- _onDidCloseTextDocument_: VS Code에서 문서가 닫혔을때 호출됩니다. 

<!--
- _onDidOpenTextDocument_: is called when a text document is opened in VS Code.
- _onDidChangeTextDocument_: is called when the content of a text document changes in VS Code.
- _onDidCloseTextDocument_: is called when a text document is closed in VS Code.
-->

아래는 이러한 알림 핸들러를 연결하고 초기화시 올바른 기능을 반환하는 방법을 보여주는 코드 snippet입니다:

<!--
Below is a code snippet that illustrates how to hook these notification handlers on a connection and how to return the right capability on initialize: -->

```typescript
connection.onInitialize((params): InitializeResult => {
    ...
    return {
        capabilities: {
            // Enable incremental document sync
            textDocumentSync: TextDocumentSyncKind.Incremental,
            ...
        }
    };
});

connection.onDidOpenTextDocument((params) => {
    // A text document was opened in VS Code.
    // params.uri uniquely identifies the document. For documents stored on disk, this is a file URI.
    // params.text the initial full content of the document.
});

connection.onDidChangeTextDocument((params) => {
    // The content of a text document has change in VS Code.
    // params.uri uniquely identifies the document.
    // params.contentChanges describe the content changes to the document.
});

connection.onDidCloseTextDocument((params) => {
    // A text document was closed in VS Code.
    // params.uri uniquely identifies the document.
});
```

### 언어 기능 구현을 위해 VS Code API를 직접 사용

<!--
### Using VS Code API directly to implement Language Features -->

언어 서버가 많은 장점을 가지고 있지만, VS Code의 편집 기능을 확장하는 유일한 방법은 아닙니다. 문서 타입에 대한 간단한 언어 기능을 추가 하려는 경우, `vscode.languages.register[LANGUAGE_FEATURE]Provider` 를 사용하는것을 고려해보십시오. 

<!--
While Language Servers have many benefits, they are not the only option for extending the editing capabilities of VS Code. In the cases when you want to add some simple language features for a type of document, consider using `vscode.languages.register[LANGUAGE_FEATURE]Provider` as an option. -->

이것은 `vscode.languages.registerCompletionItemProvider`를 사용하여 텍스트 파일에 대해 snippet을 완성시키는 기능을 추가한 [`completions-sample`](https://github.com/Microsoft/vscode-extension-samples/tree/master/completions-sample) 예시 입니다. 

<!--
Here is a [`completions-sample`](https://github.com/Microsoft/vscode-extension-samples/tree/master/completions-sample) using `vscode.languages.registerCompletionItemProvider` to add a few snippets as completions for plain text files.-->

VS Code API의 사용을 설명하는 더 많은 예시를 [https://github.com/Microsoft/vscode-extension-samples](https://github.com/Microsoft/vscode-extension-samples)에서 확인 할 수 있습니다. 

<!--
More samples illustrating the usage of VS Code API can be found at [https://github.com/Microsoft/vscode-extension-samples](https://github.com/Microsoft/vscode-extension-samples).-->

### 언어 서버용 오류 허용 파서

<!--
### Error Tolerant Parser for Language Server -->

대부분의 경우, 에디터의 모드는 불완전하고 구문상 옳지 않지만, 개발자는 여전히 자동완성과 다른 언어 기능이 작동하길 기대합니다. 그러므로 에러 허용 파서가 언어 서버에 필수적입니다 : 파서는 의미있는 AST를 부분적으로 완성된 코드로 부터 생성하며, 언어 서버는 AST를 기반으로 언어 기능을 제공합니다.

<!--
Most of the time, the code in the editor is incomplete and syntactically incorrect, but developers would still expect autocomplete and other language features to work. Therefore, an error tolerant parser is necessary for a Language Server: The parser generates meaningful AST from partially complete code, and the Language Server provides language features based on the AST. -->

VS Code에서 PHP 지원을 개선할때, 공식 PHP 파서가 에러를 허용하지 않아 언어 서버에서 직접 재사용 될 수 없음을 발견 했습니다. 따라서 우리는 
[Microsoft/tolerant-php-parser](https://github.com/Microsoft/tolerant-php-parser)를 작업 했으며, 에러 허용 파서를 구현하려는 언어 서버 작성자에게 도움이 될 수 있는 자세한 [기록](https://github.com/Microsoft/tolerant-php-parser/blob/master/docs/HowItWorks.md)을 남겨두었습니다.  

<!--
When we were improving PHP support in VS Code, we realized the official PHP parser is not error tolerant and cannot be reused directly in the Language Server. Therefore, we worked on [Microsoft/tolerant-php-parser](https://github.com/Microsoft/tolerant-php-parser) and left detailed [notes](https://github.com/Microsoft/tolerant-php-parser/blob/master/docs/HowItWorks.md) that might help Language Server authors who need to implement an error tolerant parser.-->

## 자주 나오는 질문

<!--
## Common questions -->

### 서버에 연결할 때, "cannot connect to runtime proceess (timeout after 5000ms)"가 확인됩니다.

<!--
### When I try to attach to the server, I get "cannot connect to runtime process (timeout after 5000 ms)"? -->

서버가 실행중이지 않은 상태에서 디버거를 연결하려 할때 해당 timeout 에러를 확인 할 수 있습니다. 클라이언트는 언어 서버를 시작하므로, 작동중인 서버를 갖기 위해 클라이언트가 시작 되었는지 확인하십시오. 또한 서버 시작을 방해하는 클라이언트의 중단점을 비활성화 해야 할 수도 있습니다. 

<!--
You will see this timeout error if the server isn't running when you try to attach the debugger. The client starts the language server so make sure you have started the client in order to have a running server. You may also need to disable your client breakpoints if they are interfering with starting the server. -->

### 이 가이드와 [LSP Specification](https://microsoft.github.io/language-server-protocol/)를 읽었지만 여전히 해결 되지 않은 문제가 있습니다, 어디서 도움을 받을 수 있나요?

<!--
### I have read through this guide and the [LSP Specification](https://microsoft.github.io/language-server-protocol/), but I still have unresolved questions. Where can I get help? -->

https://github.com/Microsoft/language-server-protocol 에 이슈를 작성하십시오. 

<!-- Please open an issue at https://github.com/Microsoft/language-server-protocol.-->
