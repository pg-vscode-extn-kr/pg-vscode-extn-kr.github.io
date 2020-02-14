---
layout: default
parent: References
title: Commands
nav_order: 5
description: ""
---

# 빌트인 커맨드

<!--
# Built-in Commands -->

이 문서에서는 Visual Studio Code에서 `vscode.commands.executeCommand` API를 통해 사용 할 수 있는 커맨드의 목록을 나열합니다.  
<!--
This document lists a subset of Visual Studio Code commands that you might use with `vscode.commands.executeCommand` API.-->

[커맨드 가이드](/api/extension-guides/command)에서, 커맨드 API를 사용하는 방법을 참조하십시오.
<!--
Read the [Commands Guide](/api/extension-guides/command) for how to use the commands API. -->

다음은 VS Code에서 새로운 폴더를 여는 방법의 한가지 예입니다:

<!--
The following is a sample of how to open a new folder in VS Code: -->

```javascript
let uri = Uri.file('/some/path/to/folder');
let success = await commands.executeCommand('vscode.openFolder', uri);
```

## 커맨드
<!--
## Commands-->

`vscode.executeWorkspaceSymbolProvider` - 모든 작업공간 심볼제공자를 실행합니다.

<!--
`vscode.executeWorkspaceSymbolProvider` - Execute all workspace symbol provider. -->

- _query_ - 문자열을 검색합니다.
- _(returns)_ - SymbolInformation과 DocumentSymbol 인스턴스 배열로 이뤄진 프로미스 입니다.

<!--
- _query_ - Search string
- _(returns)_ - A promise that resolves to an array of SymbolInformation and DocumentSymbol instances.-->

`vscode.executeDefinitionProvider` - 모든 정의제공자를 실행합니다.

<!--
`vscode.executeDefinitionProvider` - Execute all definition provider.-->

- _uri_ - 텍스트 문서의 URI입니다.
- _position_ - 심볼의 위치입니다.
- _(returns)_ - 위치 인스턴스 배열로 이뤄진 프로미스 입니다.

<!--
- _uri_ - Uri of a text document
- _position_ - Position of a symbol
- _(returns)_ - A promise that resolves to an array of Location instances. -->

`vscode.executeDeclarationProvider` - 모든 선언제공자를 실행합니다.

<!--
`vscode.executeDeclarationProvider` - Execute all declaration provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _position_ - 심볼의 위치입니다.
- _(returns)_ - 위치 인스턴스 배열로 이뤄진 프로미스 입니다.

<!--
- _uri_ - Uri of a text document
- _position_ - Position of a symbol
- _(returns)_ - A promise that resolves to an array of Location-instances. -->

`vscode.executeTypeDefinitionProvider` - 모든 타입 정의제공자를 실행합니다.

<!--
`vscode.executeTypeDefinitionProvider` - Execute all type definition providers. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _position_ - 심볼의 위치입니다.
- _(returns)_ - 위치 인스턴스 배열로 이뤄진 프로미스 입니다.

<!--
- _uri_ - Uri of a text document
- _position_ - Position of a symbol
- _(returns)_ - A promise that resolves to an array of Location instances. -->

`vscode.executeImplementationProvider` - 모든 구현제공자를 실행합니다.

<!-- `vscode.executeImplementationProvider` - Execute all implementation providers.-->

- _uri_ - 텍스트 문서의 URI입니다.
- _position_ - 심볼의 위치입니다.
- _(returns)_ - 위치 인스턴스 배열로 이뤄진 프로미스 입니다.

<!--
- _uri_ - Uri of a text document
- _position_ - Position of a symbol
- _(returns)_ - A promise that resolves to an array of Location instances. -->

`vscode.executeHoverProvider` - 모든 말풍선 제공자를 실행합니다.

<!--
`vscode.executeHoverProvider` - Execute all hover provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _position_ - 심볼의 위치입니다.
- _(returns)_ - 말풍선 인스턴스 배열로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _position_ - Position of a symbol
- _(returns)_ - A promise that resolves to an array of Hover instances. -->

`vscode.executeDocumentHighlights` - 모든 문서강조 제공자를 실행합니다.

<!-- `vscode.executeDocumentHighlights` - Execute document highlight provider.-->

- _uri_ - 텍스트 문서의 URI입니다.
- _position_ - 텍스트 문서에서의 위치 입니다.
- _(returns)_ - 문서강조 인스턴스 배열로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _position_ - Position in a text document
- _(returns)_ - A promise that resolves to an array of DocumentHighlight instances. -->

`vscode.executeReferenceProvider` - 참조제공자를 실행합니다.
<!--
`vscode.executeReferenceProvider` - Execute reference provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _position_ - 텍스트 문서에서의 위치 입니다.
- _(returns)_ - 위치 인스턴스 배열로 이뤄진 프로미스 입니다.
<!--
- _uri_ - Uri of a text document
- _position_ - Position in a text document
- _(returns)_ - A promise that resolves to an array of Location instances.
-->

`vscode.executeDocumentRenameProvider` - 이름변경 제공자를 실행합니다.

<!--
`vscode.executeDocumentRenameProvider` - Execute rename provider. -->

- _uri_ - 텍스트 문서의 URI 입니다.
- _position_ - 텍스트 문서에서의 위치입니다.
- _newName_ - 새로운 심볼의 이름입니다.
- _(returns)_ - WorkspaceEdit으로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _position_ - Position in a text document
- _newName_ - The new symbol name
- _(returns)_ - A promise that resolves to a WorkspaceEdit. -->

`vscode.executeSignatureHelpProvider` - 서명도움말 제공자를 실행합니다..
<!--
`vscode.executeSignatureHelpProvider` - Execute signature help provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _position_ - 텍스트 문서에서의 위치입니다.
- _triggerCharacter_ - (선택적) 사용자가 `,` 나 `(`와 같은 문자를 입력할때 서명도움말을 실행하도록 합니다.
- _(returns)_ - 서명도움말로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _position_ - Position in a text document
- _triggerCharacter_ - (optional) Trigger signature help when the user types the character, like `,` or `(`
- _(returns)_ - A promise that resolves to SignatureHelp. -->

`vscode.executeDocumentSymbolProvider` - 문서 심볼제공자를 실행합니다.
<!--
`vscode.executeDocumentSymbolProvider` - Execute document symbol provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _(returns)_ - SymbolInformation 과 DocumentSymbol 인스턴스 배열로 구성된 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _(returns)_ - A promise that resolves to an array of SymbolInformation and DocumentSymbol instances. -->

`vscode.executeCompletionItemProvider` - 완성항목 제공자를 실행합니다.

<!--
`vscode.executeCompletionItemProvider` - Execute completion item provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _position_ - 텍스트 문서에서의 위치입니다.
- _triggerCharacter_ - (선택적) 사용자가 `,` 나 `(`와 같은 문자를 입력할때 완성을 실행하도록 합니다.`
- _itemResolveCount_ - (선택적) 완성 해야할 횟수 입니다. (너무 많은 수는 완성 속도를 늦춥니다)
- _(returns)_ - 완성목록 인스턴스로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _position_ - Position in a text document
- _triggerCharacter_ - (optional) Trigger completion when the user types the character, like `,` or `(`
- _itemResolveCount_ - (optional) Number of completions to resolve (too large numbers slow down completions)
- _(returns)_ - A promise that resolves to a CompletionList instance. -->

`vscode.executeCodeActionProvider` - 코드 액션 제공자를 실행합니다.
<!--
`vscode.executeCodeActionProvider` - Execute code action provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _range_ - 텍스트 문서에서의 범위입니다.
- _(returns)_ - 커맨드 인스턴스 배열로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _range_ - Range in a text document
- _(returns)_ - A promise that resolves to an array of Command instances. -->

`vscode.executeCodeLensProvider` - CodeLens 제공자를 실행합니다.
<!--
`vscode.executeCodeLensProvider` - Execute CodeLens provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _itemResolveCount_ - (선택적) 해결, 반환되어야할 렌즈의 수입니다. 해결된 것만 반환하며, 성능에 영향을 미칩니다.
- _(returns)_ - CodeLens 인스턴스 배열로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _itemResolveCount_ - (optional) Number of lenses that should be resolved and returned. Will only return resolved lenses, will impact performance)
- _(returns)_ - A promise that resolves to an array of CodeLens instances.
-->

`vscode.executeFormatDocumentProvider` - 문서 포맷 제공자를 실행합니다.
<!--
`vscode.executeFormatDocumentProvider` - Execute document format provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _options_ - 포맷 옵션입니다.
- _(returns)_ - TextEdits 배열로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _options_ - Formatting options
- _(returns)_ - A promise that resolves to an array of TextEdits. -->

`vscode.executeFormatRangeProvider` - 범위 포맷 제공자를 실행합니다.
<!--
`vscode.executeFormatRangeProvider` - Execute range format provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _range_ - 텍스트 문서에서의 범위입니다.
- _options_ - 포맷 옵션입니다.
- _(returns)_ - TextEdits 배열로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _range_ - Range in a text document
- _options_ - Formatting options
- _(returns)_ - A promise that resolves to an array of TextEdits. -->

`vscode.executeFormatOnTypeProvider` - 문서 포맷 제공자를 실행합니다.

<!--
`vscode.executeFormatOnTypeProvider` - Execute document format provider. -->


- _uri_ - 텍스트 문서의 URI입니다.
- _position_ - 텍스트 문서에서의 위치입니다.
- _ch_ - 입력받을 문자 입니다.
- _options_ - 포맷 옵션입니다.
- _(returns)_ - TextEdits 배열로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _position_ - Position in a text document
- _ch_ - Character that got typed
- _options_ - Formatting options
- _(returns)_ - A promise that resolves to an array of TextEdits. -->

`vscode.executeLinkProvider` - 문서 링크 제공자를 실행합니다.

<!-- `vscode.executeLinkProvider` - Execute document link provider.-->

- _uri_ - 텍스트 문서의 URI입니다.
- _(returns)_ - DocumentLink 인스턴스 배열로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _(returns)_ - A promise that resolves to an array of DocumentLink instances. -->

`vscode.executeDocumentColorProvider` - 문서색상 제공자를 실행합니다.
<!--
`vscode.executeDocumentColorProvider` - Execute document color provider. -->

- _uri_ - 텍스트 문서의 URI입니다.
- _(returns)_ - ColorInformation 오브젝트 배열로 이뤄진 프로미스입니다.

<!--
- _uri_ - Uri of a text document
- _(returns)_ - A promise that resolves to an array of ColorInformation objects. -->

`vscode.executeColorPresentationProvider` - 색상 프레젠테이션 제공자를 실행합니다.

<!-- `vscode.executeColorPresentationProvider` - Execute color presentation provider.-->

- _color_ - 표시 및 삽입할 색상입니다.
- _context_ - URI 와 범위가 있는 컨텍스트 오브젝트입니다.
- _(returns)_ - ColorPresentation 오브젝트 배열로 이뤄진 프로미스입니다.

<!--
- _color_ - The color to show and insert
- _context_ - Context object with uri and range
- _(returns)_ - A promise that resolves to an array of ColorPresentation objects. -->

`vscode.openFolder` - newWindow 인수에 따라, 폴더나 작업공간을 현재 창 혹은 새 창에서 엽니다.

<!--
`vscode.openFolder` - Open a folder or workspace in the current window or new window depending on the newWindow argument. -->

- _uri_ - (선택적) 열고자 하는 폴더나 작업공간 파일의 URI입니다. 주어지지 않은 경우 기본대화상자를 통해 사용자에게 폴더를 요구할 것입니다.
- _newWindow_ - (선택적) 폴더/작업공간을 새 창에서 열지를 정합니다. 기본값은 현재 창입니다.

<!--
- _uri_ - (optional) Uri of the folder or workspace file to open. If not provided, a native dialog will ask the user for the folder
- _newWindow_ - (optional) Whether to open the folder/workspace in a new window or the same. Defaults to opening in the same window.
-->

newWindow 매개변수가 true로 설정되어 있지 않으면, 현재 창에서 열리게 되어 현재 익스텐션 호스트 프로세스가 종료되고 주어진 폴더/작업공간에서 새로운 프로세스가 실행됩니다.

<!--
Note that opening in the same window will shutdown the current extension host process and start a new one on the given folder/workspace unless the newWindow parameter is set to true. -->

`vscode.diff` - diff 에디터에서, 제공된 리소스를 열어 내용을 비교합니다.

<!--
`vscode.diff` - Opens the provided resources in the diff editor to compare their contents. -->

- _left_ - diff 에디터의 좌측 리소스입니다.
- _right_ - diff 에디터의 우측 리소스입니다.
- _title_ - (선택적) 사람이 읽을수 있는 diff 에디터의 제목입니다.
- _options_ - (선택적) 에디터의 옵션입니다. vscode.TextDocumentShowOptions 를 참조하십시오.

<!--
- _left_ - Left-hand side resource of the diff editor
- _right_ - Right-hand side resource of the diff editor
- _title_ - (optional) Human readable title for the diff editor
- _options_ - (optional) Editor options, see vscode.TextDocumentShowOptions
-->

`vscode.open` - 에디터에서 제공된 리소스를 엽니다.
<!--
`vscode.open` - Opens the provided resource in the editor. -->

- _resource_ - 열고자 하는 리소스 입니다.
- _columnOrOptions_ - (선택적) 옵션을 열거나 편집할 행입니다, vscode.TextDocumentShowOptions를 참조하십시오.

<!--
- _resource_ - Resource to open
- _columnOrOptions_ - (optional) Either the column in which to open or editor options, see vscode.TextDocumentShowOptions -->

텍스트나 바이너리 파일, 혹은 http(s) url일 수 있습니다. 텍스트 파일을 열기 위한 옵션을 더 조절하기 위해서는 vscode.window.showTextDocument를 대신 사용하십시오.

<!--
Can be a text or binary file, or a http(s) url. If you need more control over the options for opening a text file, use vscode.window.showTextDocument instead. -->

`vscode.removeFromRecentlyOpened` - 최근에 열린 목록에서 주어진 경로를 가진 항목을 제거합니다.

<!--
`vscode.removeFromRecentlyOpened` - Removes an entry with the given path from the recently opened list. -->

- _path_ - 최근 열린것에서 제거할 경로입니다.
<!--
- _path_ - Path to remove from recently opened. -->

`vscode.setEditorLayout` - 에디터 레이아웃을 설정합니다.
<!--
`vscode.setEditorLayout` - Sets the editor layout. -->

- _layout_ - 설정할 에디터 레이아웃입니다.
<!--
- _layout_ - The editor layout to set. -->

레이아웃은 초기(선택적) 방향 (0 = 가로, 1 = 세로)과 에디터 그룹 배열을 가진 오브젝트로 설명됩니다. 각 에디터 그룹은 크기와 방향에 수직하게 배치되는 다른 에디터 그룹 배열을 가질 수 있습니다. 만약 에디터 그룹 크기가 제공되는 경우, 그 합은 행과 열마다 1이어야 합니다. 2x2 그리드의 예시 :  `{ orientation: 0, groups: [{ groups: [{}, {}], size: 0.5 }, { groups: [{}, {}], size: 0.5 }] }`

<!--
The layout is described as object with an initial (optional) orientation (0 = horizontal, 1 = vertical) and an array of editor groups within. Each editor group can have a size and another array of editor groups that will be laid out orthogonal to the orientation. If editor group sizes are provided, their sum must be 1 to be applied per row or column. Example for a 2x2 grid: `{ orientation: 0, groups: [{ groups: [{}, {}], size: 0.5 }, { groups: [{}, {}], size: 0.5 }] }` -->

`cursorMove` - 커서를 뷰의 논리적 위치로 이동합니다.
<!--
`cursorMove` - Move cursor to a logical position in the view -->

- _Cursor move argument object_

  이 인수를 통해 전달되는 특성-값 쌍입니다:
  <!--
  Property-value pairs that can be passed through this argument: -->

  - 'to': 커서를 이동할 위치를 제공하는 필수적인 논리적 위치 값입니다.
  <!--
  - 'to': A mandatory logical position value providing where to move the cursor. -->
    ```
    'left', 'right', 'up', 'down'
    'wrappedLineStart', 'wrappedLineEnd', 'wrappedLineColumnCenter'
    'wrappedLineFirstNonWhitespaceCharacter', 'wrappedLineLastNonWhitespaceCharacter'
    'viewPortTop', 'viewPortCenter', 'viewPortBottom', 'viewPortIfOutside'
    ```
  - 'by': 움직임의 단위 입니다. 기본값은 `to` 값을 기반으로 계산됩니다.
  <!--
  - 'by': Unit to move. Default is computed based on 'to' value. -->
    ```
    'line', 'wrappedLine', 'character', 'halfLine'
    ```
    
  - 'value': 이동할 단위 입니다. 기본값은 '1'입니다.
  - 'select': 'true'인 경우 선택합니다. 기본값은 'false'입니다.
 <!--
  - 'value': Number of units to move. Default is '1'. 
  - 'select': If 'true' makes the selection. Default is 'false'.-->

`editorScroll` - 주어진 방향으로 에디터를 스크롤합니다.
<!--`editorScroll` - Scroll editor in the given direction-->

- _Editor scroll argument object_

   이 인수를 통해 전달되는 특성-값 쌍입니다:
  <!--Property-value pairs that can be passed through this argument:-->

  - 'to': 필수인 방향의 값입니다.
  <!--
  - 'to': A mandatory direction value.-->
    ```
    'up', 'down'
    ```
  - 'by': 움직임의 단위입니다. 기본값은 'to' 를 기준으로 계산됩니다.
  <!-- - 'by': Unit to move. Default is computed based on 'to' value. -->
    ```
    'line', 'wrappedLine', 'page', 'halfPage'
    ```
  - 'value': 이동할 단위 입니다. 기본값은 '1'입니다.
  - 'revealCursor': 'true'인경우 커서가 뷰 포트 외부에 있을때 커서가 표시됩니다.
  
  <!--
  - 'value': Number of units to move. Default is '1'.
  - 'revealCursor': If 'true' reveals the cursor if it is outside view port. -->

`revealLine` - 주어진 논리적 위치에서 주어진 줄을 표시합니다.
<!--`revealLine` - Reveal the given line at the given logical position-->

- _Reveal line argument object_

  이 인수를 통해 전달되는 특성-값 쌍입니다:
  <!--Property-value pairs that can be passed through this argument:-->

  - 'lineNumber': 필수인 줄 번호 값입니다.
  <!--
  - 'lineNumber': A mandatory line number value. -->
  - 'at': 나타낼 줄을 표시하는 논리적위치 입니다.
  <!-- - 'at': Logical position at which line has to be revealed .-->
    ```
    'top', 'center', 'bottom'
    ```

`editor.unfold` - 에디터의 컨텐츠를 펼칩니다.
<!-- `editor.unfold` - Unfold the content in the editor-->

- _Unfold editor argument_

  이 인수를 통해 전달되는 특성-값 쌍입니다:
  <!-- Property-value pairs that can be passed through this argument:-->

  - 'levels': 펼칠 레벨의 수 입니다. 설정되지 않은 경우 기본값은 1입니다.
  - 'direction': 'up'인경우 주어진 레벨 수만큼 위로 펼치고, 그렇지 않은 경우 아래로 펼칩니다.
  - 'selectionLines': 펼침 액션을 적용할 에디터 선택의 시작 줄 (0-기반)입니다. 설정하지 않으면 활성화된 선택이 사용됩니다.

  <!--   
  - 'levels': Number of levels to unfold. If not set, defaults to 1.
  - 'direction': If 'up', unfold given number of levels up otherwise unfolds down.
  - 'selectionLines': The start lines (0-based) of the editor selections to apply the unfold action to. If not set, the active selection(s) will be used.
  -->
  
`editor.fold` - 에디터의 컨텐츠를 접습니다.
<!--
`editor.fold` - Fold the content in the editor -->

- _Fold editor argument_

  이 인수를 통해 전달되는 특성-값 쌍입니다:  
  <!-- Property-value pairs that can be passed through this argument: -->

  - 'levels': 접을 레벨의 수 입니다. 기본값은 1 입니다.
  - 'direction': 'up' 일 경우 주어진 레벨수 만큼 위로 접고, 그렇지 않을 경우 아래로 접습니다.
  - 'selectionLines': 접기 액션을 적용할 에디터 선택의 시작 줄 (0-기반)입니다. 설정하지 않으면 활성화된 선택이 사용됩니다.
  <!--
  - 'levels': Number of levels to fold. Defaults to 1.
  - 'direction': If 'up', folds given number of levels up otherwise folds down.
  - 'selectionLines': The start lines (0-based) of the editor selections to apply the fold action to. If not set, the active selection(s) will be used. -->

`editor.action.showReferences` - 파일의 위치에 참조를 표시합니다.
<!--
`editor.action.showReferences` - Show references at a position in a file -->

- _uri_ - 참조를 표시할 텍스트 문서입니다.
- _position_ - 보여줄 위치입니다.
- _locations_ - 위치 배열입니다.

<!--
- _uri_ - The text document in which to show references
- _position_ - The position at which to show
- _locations_ - An array of locations. -->

`moveActiveEditor` - 활성화된 에디터의 탭이나 그룹으로 이동합니다.
<!--
`moveActiveEditor` - Move the active editor by tabs or groups -->

- _Active editor move argument_

  인수의 특성입니다:
  <!-- Argument Properties:-->

  - 'to': 이동할 곳을 제공하는 문자열 값입니다.
  - 'by': 움직일 단위를 제공하는 문자열 값입니다. (by tab 혹은 by group).
  - 'value': 이동할 위치 수 또는 절대 위치를 제공하는 숫자 값입니다.
  <!--
  - 'to': String value providing where to move.
  - 'by': String value providing the unit for move (by tab or by group).
  - 'value': Number value providing how many positions or an absolute position to move.
  -->
  
## 심플 커맨드

<!--
## Simple commands -->

매개변수를 요구하지 않는 심플 커맨드는, 기본 `keybindings.json`파일의 키보드 단축키 목록에서 찾을 수 있습니다. 설정 되지 않은 커맨드는 파일 맨 아래의 주석 블록에 나열 됩니다.

<!--
Simple commands that do not require parameters can be found in the Keyboard Shortcuts list in the default `keybindings.json` file. The unbound commands are listed in a comment block at the bottom of the file. -->

`keybindings.json`을 검토하기 위해:
<!--
To review `keybindings.json`: -->

Windows, Linux: **File** > **Preferences** > **Keyboard Shortcuts** > `keybindings.json` 링크
<!--
Windows, Linux: **File** > **Preferences** > **Keyboard Shortcuts** > `keybindings.json` link -->

macOS:
**Code** > **Preferences** > **Keyboard Shortcuts** > `keybindings.json` 링크

<!--
macOS:
**Code** > **Preferences** > **Keyboard Shortcuts** > `keybindings.json` link -->
