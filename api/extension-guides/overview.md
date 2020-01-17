---
layout: default
parent: Extension Guides
title: Overview
nav_order: 1
description: ""
---

# 익스텐션 가이드
<!--
# Extension Guides -->

[Hello World](/api/get-started/your-first-extension) 예제에서  Visual Studio Code 익스텐션 API의 기본을 배우셨다면, 실제로 쓰이는 익스텐션을 작성해 볼 시간입니다. [Extension Capabilities](/api/extension-capabilities/overview) 주제에서 익스텐션에서 어떤 일을 **가능** 한 지에 대해 설명한다면, 이번 주제에서는 자세한 코드 가이드와 예시를 통해 특정 VS Code API를 이용한 **방법**을 설명 하고 있습니다. 

<!--
Once you have learned the basics of Visual Studio Code Extension API in the [Hello World](/api/get-started/your-first-extension) sample, it's time to build some real-world extensions. While the [Extension Capabilities](/api/extension-capabilities/overview) section offers high-level overviews of what extension **can** do, this section contains a list of detailed code guides and samples that explains **how** to use a specific VS Code API. -->

각각의 가이드-예시 조합마다, 여러분은 다음을 확인 할 수 있습니다:
<!-- 
In each guide-sample combo, you can expect to find: -->

- 커멘트로 설명이 잘 되어있는 소스 코드.
- GIF 혹은 이미지로 제공되는 익스텐션의 사용 예시.
- 예시 익스텐션을 실행시키는 방법.
- 사용되는 VS Code API 리스트.
- 사용되는 Contribution Points 리스트.
- 예시와 유사한 실제 익스텐션.
- API 컨셉에 대한 설명.

<!--
- Thoroughly commented source code.
- A gif or image showing the usage of the sample extension.
- Instructions for running the sample extension.
- Listing of VS Code API being used.
- Listing of Contribution Points being used.
- Real-world extensions resembling the sample.
- Explanation of API concepts.
-->

## 가이드 & 예시

<!-- ## Guides & Samples-->

다음은 가이드와 예시의 목록들입니다. 각각의 가이드는 해당하는 샘플 코드와 표기하고 있지만, 몇몇 샘플은 아직 해당하는 가이드가 없습니다. 
<!--
Here is a list of guides and samples. While each guide comes with sample code, some samples do not have a matching guide yet. -->

각 샘플은 하나의 [VS Code API](/api/references/vscode-api) 혹은 [Contribution Point](/api/references/contribution-points)를 사용 예를 설명 하고 있습니다. 

<!--
Each sample illustrates one [VS Code API](/api/references/vscode-api) usage or a [Contribution Point](/api/references/contribution-points). -->

| 예시 | VS Code Website의 가이드 | API & Contribution |
| --- | --- | --- |
| [웹 뷰](https://github.com/Microsoft/vscode-extension-samples/tree/master/webview-sample) | [/api/extension-guides/webview](https://code.visualstudio.com/api/extension-guides/webview) | [window.createWebviewPanel](https://code.visualstudio.com/api/references/vscode-api#window.createWebviewPanel)<br>[window.registerWebviewPanelSerializer](https://code.visualstudio.com/api/references/vscode-api#window.registerWebviewPanelSerializer) |
| [상태 바](https://github.com/Microsoft/vscode-extension-samples/tree/master/statusbar-sample) | N/A | [window.createStatusBarItem](https://code.visualstudio.com/api/references/vscode-api#window.createStatusBarItem)<br>[StatusBarItem](https://code.visualstudio.com/api/references/vscode-api#StatusBarItem) |
| [트리 뷰](https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample) | [/api/extension-guides/tree-view](https://code.visualstudio.com/api/extension-guides/tree-view) | [window.createTreeView](https://code.visualstudio.com/api/references/vscode-api#window.createTreeView)<br>[window.registerTreeDataProvider](https://code.visualstudio.com/api/references/vscode-api#window.registerTreeDataProvider)<br>[TreeView](https://code.visualstudio.com/api/references/vscode-api#TreeView)<br>[TreeDataProvider](https://code.visualstudio.com/api/references/vscode-api#TreeDataProvider)<br>[contributes.views](https://code.visualstudio.com/api/references/contribution-points#contributes.views)<br>[contributes.viewsContainers](https://code.visualstudio.com/api/references/contribution-points#contributes.viewsContainers) |
| [작업 제공자](https://github.com/Microsoft/vscode-extension-samples/tree/master/task-provider-sample) | [/api/extension-guides/task-provider](https://code.visualstudio.com/api/extension-guides/task-provider) | [tasks.registerTaskProvider](https://code.visualstudio.com/api/references/vscode-api#tasks.registerTaskProvider)<br>[Task](https://code.visualstudio.com/api/references/vscode-api#Task)<br>[ShellExecution](https://code.visualstudio.com/api/references/vscode-api#ShellExecution)<br>[contributes.taskDefinitions](https://code.visualstudio.com/api/references/contribution-points#contributes.taskDefinitions) |
| [다중 루트](https://github.com/Microsoft/vscode-extension-samples/tree/master/basic-multi-root-sample) | N/A | [workspace.getWorkspaceFolder](https://code.visualstudio.com/api/references/vscode-api#workspace.getWorkspaceFolder)<br>[workspace.onDidChangeWorkspaceFolders](https://code.visualstudio.com/api/references/vscode-api#workspace.onDidChangeWorkspaceFolders) |
| [완성 제공자](https://github.com/Microsoft/vscode-extension-samples/tree/master/completions-sample) | N/A | [languages.registerCompletionItemProvider](https://code.visualstudio.com/api/references/vscode-api#languages.registerCompletionItemProvider)<br>[CompletionItem](https://code.visualstudio.com/api/references/vscode-api#CompletionItem)<br>[SnippetString](https://code.visualstudio.com/api/references/vscode-api#SnippetString) |
| [파일 시스템 제공자](https://github.com/Microsoft/vscode-extension-samples/tree/master/fsprovider-sample) | N/A | [workspace.registerFileSystemProvider](https://code.visualstudio.com/api/references/vscode-api#workspace.registerFileSystemProvider) |
| [Editor Decoractor Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/decorator-sample) | N/A | [TextEditor.setDecorations](https://code.visualstudio.com/api/references/vscode-api#TextEditor.setDecorations)<br>[DecorationOptions](https://code.visualstudio.com/api/references/vscode-api#DecorationOptions)<br>[DecorationInstanceRenderOptions](https://code.visualstudio.com/api/references/vscode-api#DecorationInstanceRenderOptions)<br>[ThemableDecorationInstanceRenderOptions](https://code.visualstudio.com/api/references/vscode-api#ThemableDecorationInstanceRenderOptions)<br>[window.createTextEditorDecorationType](https://code.visualstudio.com/api/references/vscode-api#window.createTextEditorDecorationType)<br>[TextEditorDecorationType](https://code.visualstudio.com/api/references/vscode-api#TextEditorDecorationType)<br>[contributes.colors](https://code.visualstudio.com/api/references/contribution-points#contributes.colors) |
| [I18n](https://github.com/Microsoft/vscode-extension-samples/tree/master/i18n-sample) | N/A |  |
| [터미널](https://github.com/Microsoft/vscode-extension-samples/tree/master/terminal-sample) | N/A | [window.createTerminal](https://code.visualstudio.com/api/references/vscode-api#window.createTerminal)<br>[window.onDidChangeActiveTerminal](https://code.visualstudio.com/api/references/vscode-api#window.onDidChangeActiveTerminal)<br>[window.onDidCloseTerminal](https://code.visualstudio.com/api/references/vscode-api#window.onDidCloseTerminal)<br>[window.onDidOpenTerminal](https://code.visualstudio.com/api/references/vscode-api#window.onDidOpenTerminal)<br>[window.Terminal](https://code.visualstudio.com/api/references/vscode-api#window.Terminal)<br>[window.terminals](https://code.visualstudio.com/api/references/vscode-api#window.terminals) |
| [Vim](https://github.com/Microsoft/vscode-extension-samples/tree/master/vim-sample) | N/A | [commands](https://code.visualstudio.com/api/references/vscode-api#commands)<br>[StatusBarItem](https://code.visualstudio.com/api/references/vscode-api#StatusBarItem)<br>[window.createStatusBarItem](https://code.visualstudio.com/api/references/vscode-api#window.createStatusBarItem)<br>[TextEditorCursorStyle](https://code.visualstudio.com/api/references/vscode-api#TextEditorCursorStyle)<br>[window.activeTextEditor](https://code.visualstudio.com/api/references/vscode-api#window.activeTextEditor)<br>[Position](https://code.visualstudio.com/api/references/vscode-api#Position)<br>[Range](https://code.visualstudio.com/api/references/vscode-api#Range)<br>[Selection](https://code.visualstudio.com/api/references/vscode-api#Selection)<br>[TextEditor](https://code.visualstudio.com/api/references/vscode-api#TextEditor)<br>[TextEditorRevealType](https://code.visualstudio.com/api/references/vscode-api#TextEditorRevealType)<br>[TextDocument](https://code.visualstudio.com/api/references/vscode-api#TextDocument) |
| [소스 컨트롤](https://github.com/Microsoft/vscode-extension-samples/tree/master/source-control-sample) | https://code.visualstudio.com/api/extension-guides/scm-provider | [workspace.workspaceFolders](https://code.visualstudio.com/api/references/vscode-api#workspace.workspaceFolders)<br>[SourceControl](https://code.visualstudio.com/api/references/vscode-api#SourceControl)<br>[SourceControlResourceGroup](https://code.visualstudio.com/api/references/vscode-api#SourceControlResourceGroup)<br>[scm.createSourceControl](https://code.visualstudio.com/api/references/vscode-api#scm.createSourceControl)<br>[TextDocumentContentProvider](https://code.visualstudio.com/api/references/vscode-api#TextDocumentContentProvider)<br>[contributes.menus](https://code.visualstudio.com/api/references/contribution-points#contributes.menus) |
| [API에 커멘트](https://github.com/Microsoft/vscode-extension-samples/tree/master/comment-sample) | N/A |  |
| [문서 수정](https://github.com/Microsoft/vscode-extension-samples/tree/master/document-editing-sample) | N/A | [commands](https://code.visualstudio.com/api/references/vscode-api#commands) |

<!--
| Sample | Guide on VS Code Website | API & Contribution |
| --- | --- | --- |
| [Webview Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/webview-sample) | [/api/extension-guides/webview](https://code.visualstudio.com/api/extension-guides/webview) | [window.createWebviewPanel](https://code.visualstudio.com/api/references/vscode-api#window.createWebviewPanel)<br>[window.registerWebviewPanelSerializer](https://code.visualstudio.com/api/references/vscode-api#window.registerWebviewPanelSerializer) |
| [Status Bar Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/statusbar-sample) | N/A | [window.createStatusBarItem](https://code.visualstudio.com/api/references/vscode-api#window.createStatusBarItem)<br>[StatusBarItem](https://code.visualstudio.com/api/references/vscode-api#StatusBarItem) |
| [Tree View Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample) | [/api/extension-guides/tree-view](https://code.visualstudio.com/api/extension-guides/tree-view) | [window.createTreeView](https://code.visualstudio.com/api/references/vscode-api#window.createTreeView)<br>[window.registerTreeDataProvider](https://code.visualstudio.com/api/references/vscode-api#window.registerTreeDataProvider)<br>[TreeView](https://code.visualstudio.com/api/references/vscode-api#TreeView)<br>[TreeDataProvider](https://code.visualstudio.com/api/references/vscode-api#TreeDataProvider)<br>[contributes.views](https://code.visualstudio.com/api/references/contribution-points#contributes.views)<br>[contributes.viewsContainers](https://code.visualstudio.com/api/references/contribution-points#contributes.viewsContainers) |
| [Task Provider Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/task-provider-sample) | [/api/extension-guides/task-provider](https://code.visualstudio.com/api/extension-guides/task-provider) | [tasks.registerTaskProvider](https://code.visualstudio.com/api/references/vscode-api#tasks.registerTaskProvider)<br>[Task](https://code.visualstudio.com/api/references/vscode-api#Task)<br>[ShellExecution](https://code.visualstudio.com/api/references/vscode-api#ShellExecution)<br>[contributes.taskDefinitions](https://code.visualstudio.com/api/references/contribution-points#contributes.taskDefinitions) |
| [Multi Root Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/basic-multi-root-sample) | N/A | [workspace.getWorkspaceFolder](https://code.visualstudio.com/api/references/vscode-api#workspace.getWorkspaceFolder)<br>[workspace.onDidChangeWorkspaceFolders](https://code.visualstudio.com/api/references/vscode-api#workspace.onDidChangeWorkspaceFolders) |
| [Completion Provider Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/completions-sample) | N/A | [languages.registerCompletionItemProvider](https://code.visualstudio.com/api/references/vscode-api#languages.registerCompletionItemProvider)<br>[CompletionItem](https://code.visualstudio.com/api/references/vscode-api#CompletionItem)<br>[SnippetString](https://code.visualstudio.com/api/references/vscode-api#SnippetString) |
| [File System Provider Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/fsprovider-sample) | N/A | [workspace.registerFileSystemProvider](https://code.visualstudio.com/api/references/vscode-api#workspace.registerFileSystemProvider) |
| [Editor Decoractor Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/decorator-sample) | N/A | [TextEditor.setDecorations](https://code.visualstudio.com/api/references/vscode-api#TextEditor.setDecorations)<br>[DecorationOptions](https://code.visualstudio.com/api/references/vscode-api#DecorationOptions)<br>[DecorationInstanceRenderOptions](https://code.visualstudio.com/api/references/vscode-api#DecorationInstanceRenderOptions)<br>[ThemableDecorationInstanceRenderOptions](https://code.visualstudio.com/api/references/vscode-api#ThemableDecorationInstanceRenderOptions)<br>[window.createTextEditorDecorationType](https://code.visualstudio.com/api/references/vscode-api#window.createTextEditorDecorationType)<br>[TextEditorDecorationType](https://code.visualstudio.com/api/references/vscode-api#TextEditorDecorationType)<br>[contributes.colors](https://code.visualstudio.com/api/references/contribution-points#contributes.colors) |
| [I18n Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/i18n-sample) | N/A |  |
| [Terminal Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/terminal-sample) | N/A | [window.createTerminal](https://code.visualstudio.com/api/references/vscode-api#window.createTerminal)<br>[window.onDidChangeActiveTerminal](https://code.visualstudio.com/api/references/vscode-api#window.onDidChangeActiveTerminal)<br>[window.onDidCloseTerminal](https://code.visualstudio.com/api/references/vscode-api#window.onDidCloseTerminal)<br>[window.onDidOpenTerminal](https://code.visualstudio.com/api/references/vscode-api#window.onDidOpenTerminal)<br>[window.Terminal](https://code.visualstudio.com/api/references/vscode-api#window.Terminal)<br>[window.terminals](https://code.visualstudio.com/api/references/vscode-api#window.terminals) |
| [Vim Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/vim-sample) | N/A | [commands](https://code.visualstudio.com/api/references/vscode-api#commands)<br>[StatusBarItem](https://code.visualstudio.com/api/references/vscode-api#StatusBarItem)<br>[window.createStatusBarItem](https://code.visualstudio.com/api/references/vscode-api#window.createStatusBarItem)<br>[TextEditorCursorStyle](https://code.visualstudio.com/api/references/vscode-api#TextEditorCursorStyle)<br>[window.activeTextEditor](https://code.visualstudio.com/api/references/vscode-api#window.activeTextEditor)<br>[Position](https://code.visualstudio.com/api/references/vscode-api#Position)<br>[Range](https://code.visualstudio.com/api/references/vscode-api#Range)<br>[Selection](https://code.visualstudio.com/api/references/vscode-api#Selection)<br>[TextEditor](https://code.visualstudio.com/api/references/vscode-api#TextEditor)<br>[TextEditorRevealType](https://code.visualstudio.com/api/references/vscode-api#TextEditorRevealType)<br>[TextDocument](https://code.visualstudio.com/api/references/vscode-api#TextDocument) |
| [Source Control Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/source-control-sample) | https://code.visualstudio.com/api/extension-guides/scm-provider | [workspace.workspaceFolders](https://code.visualstudio.com/api/references/vscode-api#workspace.workspaceFolders)<br>[SourceControl](https://code.visualstudio.com/api/references/vscode-api#SourceControl)<br>[SourceControlResourceGroup](https://code.visualstudio.com/api/references/vscode-api#SourceControlResourceGroup)<br>[scm.createSourceControl](https://code.visualstudio.com/api/references/vscode-api#scm.createSourceControl)<br>[TextDocumentContentProvider](https://code.visualstudio.com/api/references/vscode-api#TextDocumentContentProvider)<br>[contributes.menus](https://code.visualstudio.com/api/references/contribution-points#contributes.menus) |
| [Commenting API Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/comment-sample) | N/A |  |
| [Document Editing Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/document-editing-sample) | N/A | [commands](https://code.visualstudio.com/api/references/vscode-api#commands) |
-->

## 언어 익스텐션
<!--
## Language Extension Samples --?

아래는 [언어 익스텐션](/api/language-extensions/overview)의 예시 입니다:
<!-- These samples are [Language Extensions](/api/language-extensions/overview) samples: -->

| 예시 | VS Code Website 의 가이드 |
| --- | --- |
| [Snippet](https://github.com/Microsoft/vscode-extension-samples/tree/master/snippet-sample) | [/api/language-extensions/snippet-guide](https://code.visualstudio.com/api/language-extensions/snippet-guide)  
| [contributes.snippets](https://code.visualstudio.com/api/references/contribution-points#contributes.snippets) |
| [언어 구성](https://github.com/Microsoft/vscode-extension-samples/tree/master/language-configuration-sample) | [/api/language-extensions/language-configuration-guide](https://code.visualstudio.com/api/language-extensions/language-configuration-guide)
| [contributes.languages](https://code.visualstudio.com/api/references/contribution-points#contributes.languages) |
| [LSP](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-sample) | [/api/language-extensions/language-server-extension-guide](https://code.visualstudio.com/api/language-extensions/language-server-extension-guide) | |
| [LSP 로그 스트리밍](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-log-streaming-sample)| N/A |  |
| [LSP 다중 루트 서버](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-multi-server-sample) | https://github.com/Microsoft/vscode/wiki/Adopting-Multi-Root-Workspace-APIs#language-client--language-server |  |

<!--
| Sample                                                                                                                           | Guide on VS Code Website                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Snippet Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/snippet-sample)                               | [/api/language-extensions/snippet-guide](https://code.visualstudio.com/api/language-extensions/snippet-guide)                                     | [contributes.snippets](https://code.visualstudio.com/api/references/contribution-points#contributes.snippets) |
| [Language Configuration Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/language-configuration-sample) | [/api/language-extensions/language-configuration-guide](https://code.visualstudio.com/api/language-extensions/language-configuration-guide)       | [contributes.languages](https://code.visualstudio.com/api/references/contribution-points#contributes.languages) |
| [LSP Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-sample)                                       | [/api/language-extensions/language-server-extension-guide](https://code.visualstudio.com/api/language-extensions/language-server-extension-guide) |  |
| [LSP Log Streaming Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-log-streaming-sample)           | N/A                                                                                                                                               |  |
| [LSP Multi Root Server Sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/lsp-multi-server-sample)        | https://github.com/Microsoft/vscode/wiki/Adopting-Multi-Root-Workspace-APIs#language-client--language-server                                      |  |
-->
