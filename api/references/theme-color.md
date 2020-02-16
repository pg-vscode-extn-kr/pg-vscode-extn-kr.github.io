---
layout: default
parent: References
title: Theme Color
nav_order: 6
description: ""
---

# 테마 색상

<!--
# Theme Color -->

`workbench.colorCustomizations` 사용자 [설정](/docs/getstarted/settings)을 통해 활성화된 Visual Studio Code의 [색상 테마](/docs/getstarted/themes)를 커스텀 할 수 있습니다.

<!--
You can customize your active Visual Studio Code [color theme](/docs/getstarted/themes) with the `workbench.colorCustomizations` user [setting](/docs/getstarted/settings). -->

```json
{
  "workbench.colorCustomizations": {
    "activityBar.background": "#00AA00"
  }
}
```

**주의**: 만약 기존의 색상 테마를 사용하려는 경우, [색상 테마](/docs/getstarted/themes)를 참조하여 **Preferences: Color Theme** 드롭다운을 통해 (`kb(workbench.action.selectTheme)`) 활성 색상 테마를 설정하는 방법을 배우십시오.

<!--
**Note**: If you want to use an existing color theme, see [Color Themes](/docs/getstarted/themes) where you'll learn how to set the active color theme through the **Preferences: Color Theme** dropdown (`kb(workbench.action.selectTheme)`). -->

## 색상 포맷
<!--
## Color formats -->

색상 값은 투명값을 위한 알파 채널을 포함한 RGB 색상 모델로 정의 할 수 있습니다. 포맷으로는 다음 16진법 표기법이 지원됩니다 : `#RGB`, `#RGBA`, `#RRGGBB` 및 `#RRGGBBAA`. R(빨간색), G(녹색), B(파란색), 그리고 A(알파)는 16진수 문자 (0-9, a-f 혹은 A-F) 입니다. 3자리 표기법은 (`#RGB`) 6자리 표기법의 짧은 버전이고, (`#RRGGBB`) 4자리 RGB 표기법은 (`#RGBA`) 8자리 표기법의 (`#RRGGBBAA`) 짧은 버전입니다. 예를 들어 `#e35f`는 `#ee3355ff`와 같은 색상을 표현합니다.

<!--
Color values can be defined in the RGB color model with an alpha channel for transparency. As format, the following hexadecimal notations are supported: `#RGB`, `#RGBA`, `#RRGGBB` and `#RRGGBBAA`. R (red), G (green), B (blue), and A (alpha) are hexadecimal characters (0-9, a-f or A-F). The three-digit notation (`#RGB`) is a shorter version of the six-digit form (`#RRGGBB`) and the four-digit RGB notation (`#RGBA`) is a shorter version of the eight-digit form (`#RRGGBBAA`). For example `#e35f` is the same color as `#ee3355ff`. -->

만약 알파값이 정의 되지 않은 경우, 기본값인 `ff`로 (불투명, 투명도 없음) 설정 됩니다. 만약 알파가 `00`로 설정 되는 경우, 색상은 완전히 투명해집니다.

<!--
If no alpha value is defined, it defaults to `ff` (opaque, no transparency). If alpha is set to `00`, the color is fully transparent. -->

다른 주석 범위를 포함하지 않기 위해서, 일부 색상은 불투명 하지 않아야 합니다. 색상 설명을 확인하여 어떤 색상들이 이에 해당하는지 확인하십시오.

<!--
Some colors should not be opaque in order to not cover other annotations. Check the color descriptions to see to which colors this applies. -->

## 대비 색상
<!--
## Contrast colors -->

대비 색상은 일반적으로 고대비 테마에서만 설정됩니다. 만약 설정된 경우, UI에서 항목 주위에 테두리를 추가하여 대비를 높입니다.

<!--
The contrast colors are typically only set for high contrast themes. If set, they add an additional border around items across the UI to increase the contrast. -->

- `contrastActiveBorder`: 더 큰 대비를 위해 다른 엘리멘트와 구분하기 위한 활성 엘리멘트 주위의 추가 경계 입니다.
- `contrastBorder`: 엘리먼트 주변에 여분에 추가 경계를 두어, 다른 엘리멘트와 구분하여 대비를 높입니다.

<!--
- `contrastActiveBorder`: An extra border around active elements to separate them from others for greater contrast.
- `contrastBorder`: An extra border around elements to separate them from others for greater contrast. -->

## 기본 색상
<!--
## Base colors -->


- `focusBorder`: 포커스된 엘리먼트의 전체 테두리 색입니다. 이는 컴포넌트에 의해 재정의되지 않는 경우에만 사용됩니다.
- `foreground`: 전체 전경색입니다. 이는 컴포넌트에 의해 재정의되지 않는 경우에만 사용됩니다.
- `widget.shadow`: 에디터 내부의 Find/Replace와 같은 위젯의 그림자 색상입니다.
- `selection.background`: 워크벤치에서 선택된 텍스트의 배경색입니다. (입력 필드 혹은 텍스트 영역의 경우로, 에디터나 터미널에는 적용되지 않습니다.
- `descriptionForeground`: 레이블과 같은 추가 정보를 제공하는 설명 텍스트의 전경색상입니다.
- `errorForeground`: 에러 메시지의 전체 전경색상입니다. (이는 컴포넌트에 의해 재정의되지 않는 경우에만 사용됩니다.)
- `icon.foreground`: 워크벤치에서의 아이콘의 기본 색상입니다.

<!--
- `focusBorder`: Overall border color for focused elements. This color is only used if not overridden by a component.
- `foreground`: Overall foreground color. This color is only used if not overridden by a component.
- `widget.shadow`: Shadow color of widgets such as Find/Replace inside the editor.
- `selection.background`: Background color of text selections in the workbench (for input fields or text areas, does not apply to selections within the editor and the terminal).
- `descriptionForeground`: Foreground color for description text providing additional information, for example for a label.
- `errorForeground`: Overall foreground color for error messages (this color is only used if not overridden by a component).
- `icon.foreground`: The default color for icons in the workbench.
-->

## 창 테두리
<!--
## Window border -->

VS Code 창 테두리를 위한 색상 테마 입니다.

<!--
The theme colors for VS Code window border. -->

- `window.activeBorder`: 활성화된 (포커싱) 창의 테두리 색상입니다.
- `window.inactiveBorder`: 비활성화된 (언포커싱) 창의 테두리 색상입니다.

<!--
- `window.activeBorder`: Border color for the active (focused) window.
- `window.inactiveBorder`: Border color for the inactive (unfocused) windows. -->

## 텍스트 색상
<!--
## Text colors -->

웰컴 페이지와 같은 텍스트 문서 내부의 색상입니다. 
<!--
Colors inside a text document, such as the welcome page. -->

- `textBlockQuote.background`: 블록 인용부호의 배경색입니다.
- `textBlockQuote.border`: 블록 인용부호의 테두리색입니다.
- `textCodeBlock.background`: 코드블록의 배경색입니다.
- `textLink.activeForeground`: 링크를 마우스로 클릭하거나, 위에 두었을때의 전경색입니다.
- `textLink.foreground`: 텍스트링크의 전경색입니다.
- `textPreformat.foreground`: 포맷이 지정된 텍스트 세그먼트의 전경색입니다.
- `textSeparator.foreground`: 텍스트 구분 기호의 색입니다.

<!--
- `textBlockQuote.background`: Background color for block quotes in text.
- `textBlockQuote.border`: Border color for block quotes in text.
- `textCodeBlock.background`: Background color for code blocks in text.
- `textLink.activeForeground`: Foreground color for links in text when clicked on and on mouse hover.
- `textLink.foreground`: Foreground color for links in text.
- `textPreformat.foreground`: Foreground color for preformatted text segments.
- `textSeparator.foreground`: Color for text separators. -->

## 버튼 색상

<!--
## Button control -->

새 창 탐색기의 **Open Folder** 같은 버튼 위젯을 위한 색입니다.
<!--
A set of colors for button widgets such as **Open Folder** button in the Explorer of a new window. -->

![button control](images/theme-color/button.png)


- `button.background`: 버튼 배경색입니다.
- `button.foreground`: 버튼 전경색입니다.
- `button.hoverBackground`: 버튼위에 마우스를 두었을때 나타나는 배경색입니다.
- `checkbox.background`: 체크박스 위젯의 배경색입니다.
- `checkbox.foreground`: 체크박스 위젯의 전경색입니다.
- `checkbox.border`: 체크박스 위젯의 테두리색입니다.

<!--
- `button.background`: Button background color.
- `button.foreground`: Button foreground color.
- `button.hoverBackground`: Button background color when hovering.
- `checkbox.background`: Background color of checkbox widget.
- `checkbox.foreground`: Foreground color of checkbox widget.
- `checkbox.border`: Border color of checkbox widget. -->

## 드롭다운 설정

<!--
## Dropdown control -->

통합 터미널 혹은 출력 패널과 같은 모든 드롭다운 위젯의 색상 세트입니다. macOS에서는 현재 지원되지 않음에 주의하십시오.

<!--
A set of colors for all Dropdown widgets such as in the Integrated Terminal or the Output panel. Note that the 
Dropdown control is not used on macOS currently.-->

![drop down control](images/theme-color/Dropdown.png)

- `dropdown.background`: 드롭다운 배경색입니다.
- `dropdown.listBackground`: 드롭다운 목록의 배경색입니다.
- `dropdown.border`: 드롭다운의 테두리색입니다.
- `dropdown.foreground`: 드롭다운의 전경색입니다.

<!--
- `dropdown.background`: Dropdown background.
- `dropdown.listBackground`: Dropdown list background.
- `dropdown.border`: Dropdown border.
- `dropdown.foreground`: Dropdown foreground. -->

## 입력 설정

<!--
## Input control -->

서치 뷰 또는 Find/Replace 대화상자와 같은 입력 컨트롤의 색상입니다.
<!--
Colors for input controls such as in the Search view or the Find/Replace dialog. -->

![input control](images/theme-color/input.png)


- `input.background`: 입력상자 배경
- `input.border`: 입력상자 테두리색
- `input.foreground`: 입력상자 전경색
- `input.placeholderForeground`: 입력상자의 placeholder 전경색
- `inputOption.activeBackground`: 입력필드에서 활성화된 옵션의 배경색
- `inputOption.activeBorder`: 입력필드에서 활성화된 옵션의 테두리색
- `inputValidation.errorBackground`: 오류 심각도에 대한 유효성 검사 배경색
- `inputValidation.errorForeground`: 오류 심각도에 대한 유효성 검사 전경색
- `inputValidation.errorBorder`: 오류 심각도에 대한 유효성 검사 테두리색
- `inputValidation.infoBackground`: 정보 심각도에 대한 유효성 검사 배경색
- `inputValidation.infoForeground`: 정보 심각도에 대한 유효성 검사 전경색
- `inputValidation.infoBorder`: 정보 심각도에 대한 유효성 검사 테두리색
- `inputValidation.warningBackground`: 정보 경고를 위한 유효성 검사 배경색
- `inputValidation.warningForeground`: 경고 심각도에 대한 유효성 검사 전경색
- `inputValidation.warningBorder`: 경고 심각도에 대한 유효성 검사 테두리색

<!--
- `input.background`: Input box background.
- `input.border`: Input box border.
- `input.foreground`: Input box foreground.
- `input.placeholderForeground`: Input box foreground color for placeholder text.
- `inputOption.activeBackground`: Background color of activated options in input fields.
- `inputOption.activeBorder`: Border color of activated options in input fields.
- `inputValidation.errorBackground`: Input validation background color for error severity.
- `inputValidation.errorForeground`: Input validation foreground color for error severity.
- `inputValidation.errorBorder`: Input validation border color for error severity.
- `inputValidation.infoBackground`: Input validation background color for information severity.
- `inputValidation.infoForeground`: Input validation foreground color for information severity.
- `inputValidation.infoBorder`: Input validation border color for information severity.
- `inputValidation.warningBackground`: Input validation background color for information warning.
- `inputValidation.warningForeground`: Input validation foreground color for warning severity.
- `inputValidation.warningBorder`: Input validation border color for warning severity.-->

## 스크롤바 설정
<!--
## Scrollbar control -->

- `scrollbar.shadow`: 뷰가 스크롤 되었음을 나타내는 스크롤 막대 슬라이더 그림자
- `scrollbarSlider.activeBackground`: 클릭시 나타나는 스크롤바 슬라이더 배경색
- `scrollbarSlider.background`: 스크롤바 슬라이더 배경색
- `scrollbarSlider.hoverBackground`: 마우스를 위에 두었을때 나타나는 스크롤바 슬라이더 배경색

<!--
- `scrollbar.shadow`: Scrollbar slider shadow to indicate that the view is scrolled.
- `scrollbarSlider.activeBackground`: Scrollbar slider background color when clicked on.
- `scrollbarSlider.background`: Scrollbar slider background color.
- `scrollbarSlider.hoverBackground`: Scrollbar slider background color when hovering. -->

## 뱃지
<!--
## Badge -->

뱃지는 검색 결과의 개수와 같은 작은 정보 레이블입니다. 
<!--
Badges are small information labels, for example, search results count. -->

- `badge.foreground`: 뱃지 전경색
- `badge.background`: 뱃지 배경색

<!--
- `badge.foreground`: Badge foreground color. 
- `badge.background`: Badge background color.-->

## 프로그레스 바

<!--
## Progress bar -->

- `progressBar.background`: 장시간 실행되는 작업의 프로그레스 바 배경색
<!--
- `progressBar.background`: Background color of the progress bar shown for long running operations. -->

## 리스트와 트리

<!--
## Lists and trees -->

파일 탐색이와 같은 목록과 트리를 위한 색상입니다. 활성화된 리스트/트리는 키보드 포커스를 갖고 있으며, 비활성화의 경우는 그렇지 않습니다.

<!--
Colors for list and trees like the File Explorer. An active list/tree has keyboard focus, an inactive does not.-->

- `list.activeSelectionBackground`: 활성된 리스트/트리의 선택된 항목의 리스트/트리의 배경색
- `list.activeSelectionForeground`: 활성된 리스트/트리의 선택된 항목의 리스트/트리의 전경색
- `list.dropBackground`: 마우스로 항목을 움직일때, 리스트/트리의 배경색 
- `list.focusBackground`: 리스트/트리가 활성화 되어있을때, 포커스된 항목의 리스트/트리 배경색
- `list.focusForeground`: 리스트/트리가 활성화 되어있을때, 포커스된 항목의 리스트/트리 전경색. 활성된 리스트/ 트리는 키보드 포커스를 갖고 있으며, 비활성화의 경우는 그렇지 않습니다.
- `list.highlightForeground`: 리스트/트리 내부를 검색할때 일치되는 강조의 전경색
- `list.hoverBackground`: 마우스를 아이템 위에 두었을때의 리스트/트리 배경색
- `list.hoverForeground`: 마우스를 아이템 위에 두었을때의 리스트/트리 전경색
- `list.inactiveSelectionBackground`: 리스트/트리가 비활성화 상태일때 선택된 항목의 리스트/트리 배경색
- `list.inactiveSelectionForeground`: 리스트/트리가 비활성화 상태일때 선택된 항목의 리스트/트리 전경색. 활성된 리스트/ 트리는 키보드 포커스를 갖고 있으며, 비활성화의 경우는 그렇지 않습니다.
- `list.inactiveFocusBackground`: 리스트가 비활성화 상태일때 포커스가 있는 항목의 배경색. 활성된 리스트는 키보드 포커스를 갖고 있으며, 비활성화의 경우는 그렇지 않습니다. 현재는 리스트에만 지원됩니다.
- `list.invalidItemForeground`: 유효하지 않은 항목의 리스트/트리 전경색, 예를 들면 탐색기에서 확인 할수 없는 루트가 있습니다.
- `list.errorForeground`: 에러를 포함하는 리스트 항목의 전경색
- `list.warningForeground`: 경고가 포함된 리스트 항목의 전경색
- `listFilterWidget.background`: 리스트/트리 안에서 검색할때 입력된 텍스트의 리스트/트리 필터 배경색
- `listFilterWidget.outline`: 리스트/트리 안에서 검색할때 입력된 텍스트의 리스트/트리 필터 위젯의 윤곽선색
- `listFilterWidget.noMatchesOutline`: 리스트/ 트리 내부에서 검색할때 입력된 텍스트와 일치하는 항목이 없을때 리스트/트리 필터 위젯의 윤곽선 색상.
- `list.filterMatchBackground`: 리스트와 트리에서 필터링된 일치 항목의 배경색
- `list.filterMatchBorder`: 리스트와 트리에서 필터링된 일치 항목의 테두리색
- `tree.indentGuidesStroke`: 들여쓰기 가이드를 위한 트리 위젯의 선 색상

<!--
- `list.activeSelectionBackground`: List/Tree background color for the selected item when the list/tree is active.
- `list.activeSelectionForeground`: List/Tree foreground color for the selected item when the list/tree is active.
- `list.dropBackground`: List/Tree drag and drop background when moving items around using the mouse.
- `list.focusBackground`: List/Tree background color for the focused item when the list/tree is active.
- `list.focusForeground`: List/Tree foreground color for the focused item when the list/tree is active. An active list/tree has keyboard focus, an inactive does not.
- `list.highlightForeground`: List/Tree foreground color of the match highlights when searching inside the list/tree.
- `list.hoverBackground`: List/Tree background when hovering over items using the mouse.
- `list.hoverForeground`: List/Tree foreground when hovering over items using the mouse.
- `list.inactiveSelectionBackground`: List/Tree background color for the selected item when the list/tree is inactive.
- `list.inactiveSelectionForeground`: List/Tree foreground color for the selected item when the list/tree is inactive. An active list/tree has keyboard focus, an inactive does not.
- `list.inactiveFocusBackground`: List background color for the focused item when the list is inactive. An active list has keyboard focus, an inactive does not. Currently only supported in lists.
- `list.invalidItemForeground`: List/Tree foreground color for invalid items, for example an unresolved root in explorer.
- `list.errorForeground`: Foreground color of list items containing errors.
- `list.warningForeground`: Foreground color of list items containing warnings.
- `listFilterWidget.background`: List/Tree Filter background color of typed text when searching inside the list/tree.
- `listFilterWidget.outline`: List/Tree Filter Widget's outline color of typed text when searching inside the list/tree.
- `listFilterWidget.noMatchesOutline`: List/Tree Filter Widget's outline color when no match is found of typed text when searching inside the list/tree.
- `list.filterMatchBackground`: Background color of the filtered matches in lists and trees.
- `list.filterMatchBorder`: Border color of the filtered matches in lists and trees.
- `tree.indentGuidesStroke`: Tree Widget's stroke color for indent guides.-->

## 액티비티 바
<!--
## Activity Bar-->

액티비티 바는 워크 벤치의 가장 왼쪽 혹은 오른쪽에 표시되며, 사이드 바 뷰를 빠르게 이동 할 수 있게 합니다.
<!--
The Activity Bar is displayed either on the far left or right of the workbench and allows fast switching between views of the Side Bar. -->


- `activityBar.background`: 액티비티 바 배경색
- `activityBar.dropBackground`: 액티비티 바 항목에 대한 드래그 앤 드롭 피드백 색상
- `activityBar.foreground`: 액티비티 바 전경색 (예를 들어 아이콘에 쓰인).
- `activityBar.inactiveForeground`: 비활성화 상태에서의 액티비티 바 항목 전경색
- `activityBar.border`: 사이드 바가 있는 액티비티 바 테두리 색
- `activityBarBadge.background`: 액티비티 알림 뱃지 배경색
- `activityBarBadge.foreground`: 액티비티 알림 뱃지 전경색
- `activityBar.activeBorder`: 액티비티 바 활성 표시기 테두리 색
- `activityBar.activeBackground`: 활성화된 엘리먼트에 대한 액티비티 바 옵션 배경색
- `activityBar.activeFocusBorder`: 활성화된 항목에 대한 액티비티 바 포커스 테두리 색

<!--
- `activityBar.background`: Activity Bar background color.
- `activityBar.dropBackground`: Drag and drop feedback color for the Activity Bar items.
- `activityBar.foreground`: Activity Bar foreground color (for example used for the icons).
- `activityBar.inactiveForeground`: Activity Bar item foreground color when it is inactive.
- `activityBar.border`: Activity Bar border color with the Side Bar.
- `activityBarBadge.background`: Activity notification badge background color.
- `activityBarBadge.foreground`: Activity notification badge foreground color.
- `activityBar.activeBorder`: Activity Bar active indicator border color.
- `activityBar.activeBackground`: Activity Bar optional background color for the active element.
- `activityBar.activeFocusBorder`: Activity bar focus border color for the active item.
-->

## 사이드 바

<!--
## Side Bar -->

사이드 바는 탐색기나 검색 같은 뷰를 포함합니다.

<!--
The Side Bar contains views like the Explorer and Search.-->


- `sideBar.background`: 사이드 바 배경색.
- `sideBar.foreground`: 사이드 바 전경색. 사이드 바는 탐색기나 검색 같은 뷰를 포함합니다.
- `sideBar.border`: 에디터를 분리하는 쪽의 사이드 바 테두리 색.
- `sideBar.dropBackground`: 사이드 바 섹션에 대한 드래그 앤 드롭 피드백 색상. 이 색상은 사이드 바 섹션이 여전히 보일 수 있도록 투명해야 합니다. 사이드 바는 탐색기나 검색 같은 뷰를 포함합니다.

- `sideBarTitle.foreground`: 사이드 바 제목 전경색
- `sideBarSectionHeader.background`: 사이드 바 섹션 헤더 배경색
- `sideBarSectionHeader.foreground`: 사이드 바 섹션 세더 전경색
- `sideBarSectionHeader.border`: 사이드 바 섹션 헤더 테두리 색
<!--
- `sideBar.background`: Side Bar background color.
- `sideBar.foreground`: Side Bar foreground color. The Side Bar is the container for views like Explorer and Search.
- `sideBar.border`: Side Bar border color on the side separating the editor.
- `sideBar.dropBackground`: Drag and drop feedback color for the side bar sections. The color should have transparency so that the side bar sections can still shine through. The side bar is the container for views like explorer and search. -->

<!--
- `sideBarTitle.foreground`: Side Bar title foreground color.
- `sideBarSectionHeader.background`: Side Bar section header background color.
- `sideBarSectionHeader.foreground`: Side Bar section header foreground color.
- `sideBarSectionHeader.border`: Side bar section header border color.
-->

## 미니맵
<!--
## Minimap -->

미니맵은 현재 파일의 최소화 된 버전을 보여줍니다.
<!--
The Minimap shows a minified version of the current file. -->


- `minimap.findMatchHighlight`: 파일에서 검색한 결과에 일치되는 강조색
- `minimap.selectionHighlight`: 에디터 선택의 강조색
- `minimap.errorHighlight`: 에디터 내부의 에러 강조색
- `minimap.warningHighlight`: 에디터 내부의 경고 강조색

- `minimapGutter.addedBackground`: 추가 된 컨텐츠를 위한 미니맵 거터 색상
- `minimapGutter.modifiedBackground`: 수정 된 컨텐츠를 위한 미니맵 거터 색상
- `minimapGutter.deletedBackground`: 삭제 된 컨텐츠를 위한 미니맵 거터 색상

<!--
- `minimap.findMatchHighlight`: Highlight color for matches from search within files.
- `minimap.selectionHighlight`: Highlight color for the editor selection.
- `minimap.errorHighlight`: Highlight color for errors within the editor.
- `minimap.warningHighlight`: Highlight color for warnings within the editor.
-->
<!--
- `minimapGutter.addedBackground`: Minimap gutter color for added content.
- `minimapGutter.modifiedBackground`: Minimap gutter color for modified content.
- `minimapGutter.deletedBackground`: Minimap gutter color for deleted content.
-->

## 에디터 그룹, 탭
<!--
## Editor Groups & Tabs -->

에디터 그룹은 에디터들을 담는 컨테이너 입니다. 다수의 에디터 그룹이 존재 할 수 있습니다. 탭은 에디터를 담는 컨테이너 입니다. 한개의 에디터 그룹에서 여러개의 탭이 열릴 수 있습니다.

<!--
Editor Groups are the containers of editors. There can be many editor groups. A Tab is the container of an editor. Multiple Tabs can be opened in one editor group. -->

- `editorGroup.border`: 에디터 그룹을 구분 짓기 위한 색상.

<!-- - `editorGroup.border`: Color to separate multiple editor groups from each other. -->

  ![editorGroup.border](images/theme-color/editorGroup-border.gif)

- `editorGroup.dropBackground`: 에디터를 드래그 할 때의 배경색

<!--
- `editorGroup.dropBackground`: Background color when dragging editors around. -->

  ![editorGroup.dropBackground](images/theme-color/editorGroup-dropbackground.gif)

- `editorGroupHeader.noTabsBackground`: 탭이 비활성화 된 경우의 에디터 그룹 제목 헤더의 배경색 (`"workbench.editor.showTabs": false`로 설정)

<!--
- `editorGroupHeader.noTabsBackground`: Background color of the editor group title header when Tabs are disabled (set `"workbench.editor.showTabs": false`). -->

  ![editorGroupHeader.noTabsBackground](images/theme-color/editorgroupheader-notabsbackground.gif)

- `editorGroupHeader.tabsBackground`: 탭 컨테이너의 배경색
<!--
- `editorGroupHeader.tabsBackground`: Background color of the Tabs container. -->

  ![editorGroupHeader.tabsBackground](images/theme-color/editorgroupheader-tabsbackground.gif)

- `editorGroupHeader.tabsBorder`: 탭이 활성화 되었을때의 에디터 그룹 제목 헤더의 테두리 색
<!--
- `editorGroupHeader.tabsBorder`: Border color of the editor group title header when tabs are enabled. -->

  ![editorGroupHeader.tabsBorder](images/theme-color/editorgroupheader-tabsborder.gif)


- `editorGroup.emptyBackground`: 빈 에디터 그룹의 배경색.
- `editorGroup.focusedEmptyBorder`: 포커스 된 빈 에디터 그룹의 테두리색.
- `tab.activeBackground`: 활성 그룹의 활성 탭 배경색.
- `tab.unfocusedActiveBackground`: 비활성 에디터 그룹의 활성 탭 배경색.
- `tab.activeForeground`: 활성 그룹의 활성 탭 전경색.
- `tab.border`: 탭을 구분하기 위한 테두리색.
- `tab.activeBorder`: 활성 탭의 하단 테두리 색.
- `tab.unfocusedActiveBorder`: 비활성 에디터 그룹의 활성 탭 하단 테두리 색.
- `tab.activeBorderTop`: 활성 탭의 상단 테두리 색.
- `tab.unfocusedActiveBorderTop`: 비활성 에디터 그룹의 활성 탭 상단 테두리 색.
- `tab.inactiveBackground`: 비활성 탭 배경색.
- `tab.inactiveForeground`: 활성 그룹의 비활성 탭 전경색.
- `tab.unfocusedActiveForeground`: 비활성 에디터 그룹의 활성 탭 전경색.
- `tab.unfocusedInactiveForeground`: 비활성 에디터 그룹의 비활성 탭 전경색.
- `tab.hoverBackground`: 마우스를 위로 두었을때의 탭 배경색
- `tab.unfocusedHoverBackground`: 포커스 되지 않은 그룹의 마우스를 위로 두었을때의 탭 배경색
- `tab.hoverBorder`: 마우스를 위로 두었을때의 강조되는 탭의 테두리 색
- `tab.unfocusedHoverBorder`: 포커스 되지 않은 그룹의 마우스를 위로 두었을때의 강조되는 탭의 테두리 색
- `tab.activeModifiedBorder`: 활성 그룹에서 수정된 (dirty) 활성 그룹의 활성 탭 테두리 색.
- `tab.inactiveModifiedBorder`: 활성 그룹에서 수정된 (dirty) 비활성 탭 상단의 테두리 색.
- `tab.unfocusedActiveModifiedBorder`: 포커스 되지 않은 그룹에서 수정된 (dirty) 활성 탭 상단의 테두리 색.
- `tab.unfocusedInactiveModifiedBorder`: 포커스 되지 않은 그룹에서 수정된 (dirty) 비활성 탭 상단의 테두리 색.
- `editorPane.background`: 가운데 정렬된 에디터 레이아웃의 양측에 보이는 에디터 창의 배경색.

<!--
- `editorGroup.emptyBackground`: Background color of an empty editor group.
- `editorGroup.focusedEmptyBorder`: Border color of an empty editor group that is focused.
- `tab.activeBackground`: Active Tab background color in an active group.
- `tab.unfocusedActiveBackground`: Active Tab background color in an inactive editor group.
- `tab.activeForeground`: Active Tab foreground color in an active group.
- `tab.border`: Border to separate Tabs from each other.
- `tab.activeBorder`: Bottom border for the active tab.
- `tab.unfocusedActiveBorder`: Bottom border for the active tab in an inactive editor group.
- `tab.activeBorderTop`: Top border for the active tab.
- `tab.unfocusedActiveBorderTop`: Top border for the active tab in an inactive editor group
- `tab.inactiveBackground`: Inactive Tab background color.
- `tab.inactiveForeground`: Inactive Tab foreground color in an active group.
- `tab.unfocusedActiveForeground`: Active tab foreground color in an inactive editor group.
- `tab.unfocusedInactiveForeground`: Inactive tab foreground color in an inactive editor group.
- `tab.hoverBackground`: Tab background color when hovering
- `tab.unfocusedHoverBackground`: Tab background color in an unfocused group when hovering
- `tab.hoverBorder`: Border to highlight tabs when hovering
- `tab.unfocusedHoverBorder`: Border to highlight tabs in an unfocused group when hovering
- `tab.activeModifiedBorder`: Border on the top of modified (dirty) active tabs in an active group.
- `tab.inactiveModifiedBorder`: Border on the top of modified (dirty) inactive tabs in an active group.
- `tab.unfocusedActiveModifiedBorder`: Border on the top of modified (dirty) active tabs in an unfocused group.
- `tab.unfocusedInactiveModifiedBorder`: Border on the top of modified (dirty) inactive tabs in an unfocused group.
- `editorPane.background`: Background color of the editor pane visible on the left and right side of the centered editor layout. -->

## 에디터 색상

<!--
## Editor colors -->

가장 두드러진 에디터 색상은 구문 강조에 쓰이는 토큰 색상이며 설치된 언어 문법을 기반으로 작동합니다. 이 색상들은 생상 테마에 의해 정의되지만 `editor.tokenColorCustomizations` 설정으로 커스텀 할 수도 있습니다. 색상 테마를 업데이트 하거나 가능한 토큰 타입에 대한 자세한 내용은 [색상 테마 커스터마이징](/docs/getstarted/themes#_customizing-a-color-theme)을 참조하십시오.

<!--
The most prominent editor colors are the token colors used for syntax highlighting and are based on the language grammar installed. These colors are defined by the Color Theme but can also be customized with the `editor.tokenColorCustomizations` setting. See [Customizing a Color Theme](/docs/getstarted/themes#_customizing-a-color-theme) for details on updating a Color Theme and the available token types. -->

다른 모든 에디터 색상은 아래 목록과 같습니다:

<!--
All other editor colors are listed here: -->

- `editor.background`: 에디터 배경색.
- `editor.foreground`: 에디터 기본 전경색.
- `editorLineNumber.foreground`: 에디터 줄 번호 색상
- `editorLineNumber.activeForeground`: 활성 에디터 줄 번호 색상
- `editorCursor.background`: 에디터 커서의 배경색. 블록 커서로 겹친 문자의 색상을 커스터마이즈 할 수 있습니다.
- `editorCursor.foreground`: 에디터 커서의 색상.

<!--
- `editor.background`: Editor background color.
- `editor.foreground`: Editor default foreground color.
- `editorLineNumber.foreground`: Color of editor line numbers.
- `editorLineNumber.activeForeground`: Color of the active editor line number.
- `editorCursor.background`: The background color of the editor cursor. Allows customizing the color of a character overlapped by a block cursor.
- `editorCursor.foreground`: Color of the editor cursor. -->

한글자 이상의 단어를 선택할때, 선택 색상이 표시됩니다. 선택 된 부분 외에도 동일한 내용을 가진 모든 컨텐츠가 강조 표시 됩니다. 

<!--
Selection colors are visible when selecting one or more characters. In addition to the selection also all regions with the same content are highlighted. -->

![selection highlight](images/theme-color/selectionhighlight.png)

- `editor.selectionBackground`: 에디터의 선택 색상.
- `editor.selectionForeground`: 선택된 텍스트의 고대비 색상.
- `editor.inactiveSelectionBackground`: 비활성 에디터의 선택 색상. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editor.selectionHighlightBackground`: 선택된 내용과 같은 부분의 색상. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editor.selectionHighlightBorder`: 선택된 내용과 같은 부분의 테두리 색상.

<!--
- `editor.selectionBackground`: Color of the editor selection.
- `editor.selectionForeground`: Color of the selected text for high contrast.
- `editor.inactiveSelectionBackground`: Color of the selection in an inactive editor. The color must not be opaque so as not to hide underlying decorations.
- `editor.selectionHighlightBackground`: Color for regions with the same content as the selection. The color must not be opaque so as not to hide underlying decorations.
- `editor.selectionHighlightBorder`: Border color for regions with the same content as the selection.
-->

단어 강조 색상은 커서가 기호나 단어 안에 있을때 보여집니다. 파일 유형에 사용 가능한 언어 지원에 따라 일치하는 모든 참조 및 선언 부분이 강조 표시되고, 읽기와 쓰기 액세스가 다른 색상으로 표시 됩니다. 만약 문서 기호 언어 지원이 가능하지 않은 경우, 단어 강조 표시로 돌아갑니다.

<!--
Word highlight colors are visible when the cursor is inside a symbol or a word. Depending on the language support available for the file type, all matching references and declarations are highlighted and read and write accesses get different colors. If document symbol language support is not available, this falls back to word highlighting.
-->

![occurrences](images/theme-color/occurrences.png)

- `editor.wordHighlightBackground`: 변수를 읽는 것과 같은, 읽기 액세스중의 기호 배경색. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editor.wordHighlightBorder`: 변수를 읽는 것과 같은, 읽기 액세스중의 기호 테두리 색.
- `editor.wordHighlightStrongBackground`: 변수에 쓰는 것과 같은, 쓰기 액세스 중의 기호 배경색. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editor.wordHighlightStrongBorder`: 변수에 쓰는 것과 같은, 쓰기 액세스 중의 기호 테두리 색.

<!--
- `editor.wordHighlightBackground`: Background color of a symbol during read-access, for example when reading a variable. The color must not be opaque so as not to hide underlying decorations.
- `editor.wordHighlightBorder`: Border color of a symbol during read-access, for example when reading a variable.
- `editor.wordHighlightStrongBackground`: Background color of a symbol during write-access, for example when writing to a variable. The color must not be opaque so as not to hide underlying decorations.
- `editor.wordHighlightStrongBorder`: Border color of a symbol during write-access, for example when writing to a variable.
-->

찾기 색상은 찾기/바꾸기 대화상자의 현재 찾는 문자열에 따라 다릅니다.

<!--
Find colors depend on the current find string in the Find/Replace dialog. -->

![Find matches](images/theme-color/findmatches.png)


- `editor.findMatchBackground`: 현재 검색과 일치하는 색상.
- `editor.findMatchHighlightBackground`: 다른 검색과 일치하는 색상.  뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editor.findRangeHighlightBackground`: 검색을 제한하는 범위의 색상 (찾기 위젯에서 'Find in Selection'을 사용). 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editor.findMatchBorder`: 현재 검색과 일치하는 테두리 색.
- `editor.findMatchHighlightBorder`: 다른 검색과 일치하는 테두리 색.
- `editor.findRangeHighlightBorder`: 검색을 제한하는 범위의 테두리 색 (찾기 위젯에서 'Find in Selection'을 사용). 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.

<!--
- `editor.findMatchBackground`: Color of the current search match.
- `editor.findMatchHighlightBackground`: Color of the other search matches. The color must not be opaque so as not to hide underlying decorations.
- `editor.findRangeHighlightBackground`: Color the range limiting the search (Enable 'Find in Selection' in the find widget). The color must not be opaque so as not to hide underlying decorations.
- `editor.findMatchBorder`: Border color of the current search match.
- `editor.findMatchHighlightBorder`: Border color of the other search matches.
- `editor.findRangeHighlightBorder`: Border color the range limiting the search (Enable 'Find in Selection' in the find widget). -->

검색 에디터 색상은 검색 에디터에서 결과를 강조합니다. 이는 동일한 에디터에서 다른 일치하는 클래스를 더 잘 구분하기 위해 다른 찾기에 대한 일치와 별도로 설정 할 수 있습니다. 

<!--
Search Editor colors highlight results in a Search Editor. This can be configured separately from other find matches in order to better differentiate between different classes of match in the same editor. -->

![Search Editor Matches](images/theme-color/searchEditorMatches.png)

- `searchEditor.findMatchBackground`: 에디터의 결과 색.
- `searchEditor.findMatchBorder`: 에디터의 결과의 테두리 색.

<!--
- `searchEditor.findMatchBackground`: Color of the editor's results.
- `searchEditor.findMatchBorder`: Border color of the editor's results. -->

말풍선 강조 표시는, 말풍선이 표시되는 기호 뒤에 표시됩니다.

<!--
The hover highlight is shown behind the symbol for which a hover is shown. -->

![Hover Highlight](images/theme-color/hoverhighlight.png)

- `editor.hoverHighlightBackground`: 말풍선이 표시되는 단어 아래에 강조되는 색상. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.

<!--
- `editor.hoverHighlightBackground`: Highlight below the word for which a hover is shown. The color must not be opaque so as not to hide underlying decorations. -->

현재 줄은 일반적으로 배경 강조 혹은 테두리로 나타납니다. (둘다는 아님)

<!--
The current line is typically shown as either background highlight or a border (not both).-->

![Line Highlight](images/theme-color/line.png)

- `editor.lineHighlightBackground`: 커서 위치에서 강조되는 라인의 배경색.
- `editor.lineHighlightBorder`: 커서 위치에서 선 주위의 테두리에 대한 배경색.

<!--
- `editor.lineHighlightBackground`: Background color for the highlight of line at the cursor position.
- `editor.lineHighlightBorder`: Background color for the border around the line at the cursor position. -->

링크 색상은 링크를 클릭 할 때 보여집니다.

<!--
The link color is visible when clicking on a link. -->

![Link](images/theme-color/link.png)

- `editorLink.activeForeground`: 활성 링크의 색.
<!--
- `editorLink.activeForeground`: Color of active links. -->

검색 결과를 선택 할때 범위 강조표시가 나타납니다.
<!-- The range highlight is visible when selecting a search result. -->

![Range Highlight](images/theme-color/rangehighlight.png)

- `editor.rangeHighlightBackground`: 강조된 범위의 배경색, 빠른 열기, 파일에서 심볼 검색, 찾기 기능에서 사용됩니다. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editor.rangeHighlightBorder`: 강조된 범위 주변의 테두리 배경색.

<!--
- `editor.rangeHighlightBackground`: Background color of highlighted ranges, used by Quick Open, Symbol in File and Find features. The color must not be opaque so as not to hide underlying decorations.
- `editor.rangeHighlightBorder`: Background color of the border around highlighted ranges.
-->

**Go to Definition**과 같은 커맨드를 통해 심볼을 탐색할때 강조가 표시 됩니다.

<!--
The symbol highlight is visible when navigating to a symbol via a command such as **Go to Definition**. -->


- `editor.symbolHighlightBackground`: 강조된 심볼의 배경색. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editor.symbolHighlightBorder`: 강조된 심볼 주변의 테두리 배경색.

<!--
- `editor.symbolHighlightBackground`: Background color of highlighted symbol. The color must not be opaque so as not to hide underlying decorations.
- `editor.symbolHighlightBorder`: Background color of the border around highlighted symbols. -->

에디터의 공백을 확인하기 위해, **Toggle Render Whitespace**를 사용하십시오.
<!--
To see the editor white spaces, enable **Toggle Render Whitespace**. -->

- `editorWhitespace.foreground`: 에디터의 공백 문자 색상.
<!--
- `editorWhitespace.foreground`: Color of whitespace characters in the editor. -->

에디터의 들여쓰기 가이드를 보기 위해, `"editor.renderIndentGuides": true`로 설정하십시오.

<!--
To see the editor indent guides, set `"editor.renderIndentGuides": true`. -->

- `editorIndentGuide.background`: 에디터 들여쓰기 가이드의 색상.
- `editorIndentGuide.activeBackground`: 활성 에디터 들여쓰기 가이드의 색상.

<!--
- `editorIndentGuide.background`: Color of the editor indentation guides.
- `editorIndentGuide.activeBackground`: Color of the active editor indentation guide. -->


에디터의 눈금을 보기 위해, `"editor.rulers"`를 통해 위치를 정의하십시오.

<!--
To see editor rulers, define their location with `"editor.rulers"` -->

- `editorRuler.foreground`: 에디터 눈금의 색상.
<!--
- `editorRuler.foreground`: Color of the editor rulers. -->

CodeLens:

![Code Lenses](images/theme-color/codelens.png)

- `editorCodeLens.foreground`: 에디터 CodeLens의 전경색.
<!--
- `editorCodeLens.foreground`: Foreground color of an editor CodeLens. -->

Lightbulb:

- `editorLightBulb.foreground`: Lightbulb 액션 아이콘의 색상.
- `editorLightBulbAutoFix.foreground`: Lightbulb 자동 기능 액션 아이콘의 색상.

<!--
- `editorLightBulb.foreground`: The color used for the lightbulb actions icon. 
- `editorLightBulbAutoFix.foreground`: The color used for the lightbulb auto fix actions icon. -->

괄호 일치:

<!--
Bracket matches: -->

![Bracket colors](images/theme-color/bracket-colors.png)

- `editorBracketMatch.background`: 일치하는 대괄호 뒤의 배경색.
- `editorBracketMatch.border`: 일치하는 대괄호 상자의 색상.

<!--
- `editorBracketMatch.background`: Background color behind matching brackets. 
- `editorBracketMatch.border`: Color for matching brackets boxes.-->

개요 눈금:
<!--
Overview ruler: -->

이 눈금은 에디터의 우측 가장자리에 있는 스크롤 막대 아래에 있으며, 에디터의 장식에 대한 개요을 제공합니다.

<!--
This ruler is located beneath the scroll bar on the right edge of the editor and gives an overview of the decorations in the editor. -->


- `editorOverviewRuler.border`: 개요 눈금의 테두리 색.
- `editorOverviewRuler.findMatchForeground`: 검색 일치에 대한 개요 눈금 마커 색상. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.- `editorOverviewRuler.rangeHighlightForeground`: 빠른 열기, 파일의 심볼, 찾기 기능과 같이 강조된 범위의 개요 눈금 마커 색상. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editorOverviewRuler.selectionHighlightForeground`: 선택 강조를 위한 개요 눈금 마커 색상. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editorOverviewRuler.wordHighlightForeground`: 심볼 강조를 위한 개요 눈금 마커 색상. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editorOverviewRuler.wordHighlightStrongForeground`: 쓰기 액세스 중의 심볼 강조를 위한 개요 눈금 마커 색상. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editorOverviewRuler.modifiedForeground`: 수정 된 내용을 위한 개요 눈금 마커 색상.
- `editorOverviewRuler.addedForeground`: 추가 된 내용을 위한 개요 눈금 마커 색상.
- `editorOverviewRuler.deletedForeground`: 삭제 된 내용을 위한 개요 눈금 마커 색상.
- `editorOverviewRuler.errorForeground`: 에러를 위한 개요 눈금 마커 색상.
- `editorOverviewRuler.warningForeground`: 경고를 위한 개요 눈금 마커 색상.
- `editorOverviewRuler.infoForeground`: 정보를 위한 개요 눈금 마커 색상.
- `editorOverviewRuler.bracketMatchForeground`: 일치하는 대괄호를 위한 개요 눈금 마커 색상.

<!--
- `editorOverviewRuler.border`: Color of the overview ruler border.
- `editorOverviewRuler.findMatchForeground`: Overview ruler marker color for find matches. The color must not be opaque so as not to hide underlying decorations.
- `editorOverviewRuler.rangeHighlightForeground`: Overview ruler marker color for highlighted ranges, like by the Quick Open, Symbol in File and Find features. The color must not be opaque so as not to hide underlying decorations.
- `editorOverviewRuler.selectionHighlightForeground`: Overview ruler marker color for selection highlights. The color must not be opaque so as not to hide underlying decorations.
- `editorOverviewRuler.wordHighlightForeground`: Overview ruler marker color for symbol highlights. The color must not be opaque so as not to hide underlying decorations.
- `editorOverviewRuler.wordHighlightStrongForeground`: Overview ruler marker color for write-access symbol highlights. The color must not be opaque so as not to hide underlying decorations.
- `editorOverviewRuler.modifiedForeground`: Overview ruler marker color for modified content.
- `editorOverviewRuler.addedForeground`: Overview ruler marker color for added content.
- `editorOverviewRuler.deletedForeground`: Overview ruler marker color for deleted content.
- `editorOverviewRuler.errorForeground`: Overview ruler marker color for errors.
- `editorOverviewRuler.warningForeground`: Overview ruler marker color for warnings.
- `editorOverviewRuler.infoForeground`: Overview ruler marker color for infos.
- `editorOverviewRuler.bracketMatchForeground`: Overview ruler marker color for matching brackets.
-->

에러 및 경고:
<!--
Errors and warnings:-->


- `editorError.foreground`: 에디터의 에러 전경색.
- `editorError.border`: 에디터에서 에러 박스의 테두리 색상.
- `editorWarning.foreground`: 에디터에서 경고 전경색.
- `editorWarning.border`: 에디터에서 경고 박스의 테두리 색상.
- `editorInfo.foreground`: 에디터의 정보 전경색.
- `editorInfo.border`: 에디터에서 정보 박스의 테두리 색상.
- `editorHint.foreground`: 에디터의 힌트 전경색.
- `editorHint.border`: 에디터에서 힌트 박스의 테두리 색상.
- `problemsErrorIcon.foreground`: 문제 에러 아이콘에 쓰이는 색상.
- `problemsWarningIcon.foreground`: 문제 경고 아이콘에 쓰이는 색상.
- `problemsInfoIcon.foreground`: 문제 정보 아이콘에 쓰이는 색상.

<!--
- `editorError.foreground`: Foreground color of error squiggles in the editor.
- `editorError.border`: Border color of error boxes in the editor.
- `editorWarning.foreground`: Foreground color of warning squiggles in the editor.
- `editorWarning.border`: Border color of warning boxes in the editor.
- `editorInfo.foreground`: Foreground color of info squiggles in the editor.
- `editorInfo.border`: Border color of info boxes in the editor.
- `editorHint.foreground`: Foreground color of hints in the editor.
- `editorHint.border`: Border color of hint boxes in the editor.
- `problemsErrorIcon.foreground`: The color used for the problems error icon.
- `problemsWarningIcon.foreground`: The color used for the problems warning icon.
- `problemsInfoIcon.foreground`: The color used for the problems info icon.
-->

미사용 소스 코드:
<!--
Unused source code: -->


- `editorUnnecessaryCode.border`: 에디터에 불필요한(안쓰인) 소스코드의 테두리 색상.
- `editorUnnecessaryCode.opacity`: 에디터에 불필요한(안쓰인) 소스코드의 투명도. 예를 들어, `"#000000c0"` 는 75%의 투명도를 렌더링 할 것입니다. 고 대비 테마의 경우, `"editorUnnecessaryCode.border"` 테마 색상을 사용하여 불필요한 코드를 페이드 아웃하는 대신 밑줄을 칩니다.

<!--
- `editorUnnecessaryCode.border`: Border color of unnecessary (unused) source code in the editor.
- `editorUnnecessaryCode.opacity`: Opacity of unnecessary (unused) source code in the editor. For example, `"#000000c0"` will render the code with 75% opacity. For high contrast themes, use the `"editorUnnecessaryCode.border"` theme color to underline unnecessary code instead of fading it out. -->

글리프 여백과 줄번호를 포함하는 거터:
<!--
The gutter contains the glyph margins and the line numbers: -->


- `editorGutter.background`: 에디터 거터의 배경색. 거터에는 글리프 여백과 줄번호를 포함합니다.
- `editorGutter.modifiedBackground`: 수정 된 줄의 에디터 거터의 배경색.
- `editorGutter.addedBackground`: 추가 된 줄의 에디터 거터의 배경색.
- `editorGutter.deletedBackground`: 삭제 된 줄의 에디터 거터의 배경색.
- `editorGutter.commentRangeForeground`: 주석 범위의 에디터 거터 장식 색상.

<!--
- `editorGutter.background`: Background color of the editor gutter. The gutter contains the glyph margins and the line numbers.
- `editorGutter.modifiedBackground`: Editor gutter background color for lines that are modified.
- `editorGutter.addedBackground`: Editor gutter background color for lines that are added.
- `editorGutter.deletedBackground`: Editor gutter background color for lines that are deleted.
- `editorGutter.commentRangeForeground`: Editor gutter decoration color for commenting ranges. -->

## Diff 에디터 색상
<!--
## Diff editor colors -->

삽입 혹은 삭제된 텍스트 색상을 위해, 배경이나 테두리 색 중에서 한가지를 사용하십시오.
<!--
For coloring inserted and removed text, use either a background or a border color but not both. -->


- `diffEditor.insertedTextBackground`: 삽입된 텍스트의 배경색. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `diffEditor.insertedTextBorder`: 삽입된 텍스트의 테두리 색상.
- `diffEditor.removedTextBackground`: 삭제된 텍스트의 배경색. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `diffEditor.removedTextBorder`: 삭제된 텍스트의 테두리 색상.
- `diffEditor.border`: 2개의 텍스트 에디터 간의 테두리 색상.

<!--
- `diffEditor.insertedTextBackground`: Background color for text that got inserted. The color must not be opaque so as not to hide underlying decorations.
- `diffEditor.insertedTextBorder`: Outline color for the text that got inserted.
- `diffEditor.removedTextBackground`: Background color for text that got removed. The color must not be opaque so as not to hide underlying decorations.
- `diffEditor.removedTextBorder`: Outline color for text that got removed.
- `diffEditor.border`: Border color between the two text editors. -->

## 에디터 위젯 색상
<!--
## Editor widget colors -->

에디터 위젯은 에디터 컨텐츠 앞에 보입니다. 예시로는 검색/바꾸기 대화상자, 제안 위젯 그리고 에디터 말풍선이 있습니다.

<!--
The Editor widget is shown in front of the editor content. Examples are the Find/Replace dialog, the suggestion widget, and the editor hover. -->


- `editorWidget.foreground`: 검색/바꾸기 같은 에디터 위젯의 전경색.
- `editorWidget.background`: 검색/바꾸기 같은 에디터 위젯의 배경색.
- `editorWidget.border`: 에디터 위젯의 테두리 색, 이는 자체 테두리 색을 포함하지 않는 경우 작동합니다.
- `editorWidget.resizeBorder`: 에디터 위젯의 리사이즈 바 테두리 색. 이 색상은 위젯이 리사이즈를 선택하고 위젯에 의해 색상이 덮어쓰기 되지 않는 경우에 쓰입니다. 

<!--
- `editorWidget.foreground`: Foreground color of editor widgets, such as find/replace.
- `editorWidget.background`: Background color of editor widgets, such as Find/Replace.
- `editorWidget.border`: Border color of the editor widget unless the widget does not contain a border or defines its own border color.
- `editorWidget.resizeBorder`: Border color of the resize bar of editor widgets. The color is only used if the widget chooses to have a resize border and if the color is not overridden by a widget. -->


- `editorSuggestWidget.background`: 제안 위젯의 배경색.
- `editorSuggestWidget.border`: 제안 위젯의 테두리색.
- `editorSuggestWidget.foreground`: 제안 위젯의 전경색.
- `editorSuggestWidget.highlightForeground`: 제안 위젯에서 일치 하는 내용에 대한 강조색.
- `editorSuggestWidget.selectedBackground`: 제안 위젯에서 선택된 항목에 대한 배경색.

<!--
- `editorSuggestWidget.background`: Background color of the suggestion widget.
- `editorSuggestWidget.border`: Border color of the suggestion widget.
- `editorSuggestWidget.foreground`: Foreground color of the suggestion widget.
- `editorSuggestWidget.highlightForeground`: Color of the match highlights in the suggestion widget.
- `editorSuggestWidget.selectedBackground`: Background color of the selected entry in the suggestion widget.-->


- `editorHoverWidget.foreground`: 에디터 말풍선의 전경색.
- `editorHoverWidget.background`: 에디터 말풍선의 배경색.
- `editorHoverWidget.border`: 에디터 말풍선의 테두리색.
- `editorHoverWidget.statusBarBackground`: 에디터 말풍선 상태 표시줄의 배경색.

<!--
- `editorHoverWidget.foreground`: Foreground color of the editor hover.
- `editorHoverWidget.background`: Background color of the editor hover.
- `editorHoverWidget.border`: Border color of the editor hover.
- `editorHoverWidget.statusBarBackground`: Background color of the editor hover status bar.
-->

디버그 익셉션 위젯은 디버그가 익셉션에서 중지 될 때 에디터에 표시되는 픽 뷰 입니다.

<!--The Debug Exception widget is a peek view that shows in the editor when debug stops at an exception.-->

- `debugExceptionWidget.background`: 익셉션 위젯의 배경색.
- `debugExceptionWidget.border`: 익셉션 위젯의 테두리색.

<!--
- `debugExceptionWidget.background`: Exception widget background color. 
- `debugExceptionWidget.border`: Exception widget border color.-->

에디터 마커 뷰는 에디터에서 경고나 에러를 탐색할때 나타납니다. (**Go to Next Error or Warning** 커맨드 사용)
<!--
The editor marker view shows when navigating to errors and warnings in the editor (**Go to Next Error or Warning** command). -->

- `editorMarkerNavigation.background`: 에디터 마커 탐색 위젯의 배경 색상.
- `editorMarkerNavigationError.background`: 에디터 마커 탐색 위젯의 에러 색상.
- `editorMarkerNavigationWarning.background`: 에디터 마커 탐색 위젯의 경고 색상.
- `editorMarkerNavigationInfo.background`: 에디터 마커 탐색 위젯의 정보 색상.

<!--
- `editorMarkerNavigation.background`: Editor marker navigation widget background.
- `editorMarkerNavigationError.background`: Editor marker navigation widget error color.
- `editorMarkerNavigationWarning.background`: Editor marker navigation widget warning color.
- `editorMarkerNavigationInfo.background`: Editor marker navigation widget info color.-->

## 픽 뷰 색상

<!--
## Peek view colors -->

픽뷰는 에디터 내부에서 참조나 선언을 뷰 형태로 보여줍니다.

<!--
Peek views are used to show references and declarations as a view inside the editor. -->

![Peek view](images/theme-color/peek-view.png)


- `peekView.border`: 픽 뷰 테두리와 화살표의 색상.
- `peekViewEditor.background`: 픽 뷰 에디터의 배경색.
- `peekViewEditorGutter.background`: 픽 뷰 에디터의 거터 배경색.
- `peekViewEditor.matchHighlightBackground`: 픽 뷰 에디터의 일치되는 부분에 대한 강조색.
- `peekViewEditor.matchHighlightBorder`: 픽 뷰 에디터의 일치되는 부분에 대한 강조 테두리색.
- `peekViewResult.background`: 픽 뷰 결과 목록의 배경색.
- `peekViewResult.fileForeground`: 픽 뷰 결과 목록의 파일 노드의 전경색.
- `peekViewResult.lineForeground`: 픽 뷰 결과 목록의 라인 노드의 전경색.
- `peekViewResult.matchHighlightBackground`: 픽 뷰 결과 목록의 일치되는 부분에 대한 강조색.
- `peekViewResult.selectionBackground`: 픽 뷰 결과 목록의 선택된 항목에 대한 배경색.
- `peekViewResult.selectionForeground`: 픽 뷰 결과 목록의 선택된 항목에 대한 전경색.
- `peekViewTitle.background`: 픽 뷰 제목 부분의 배경색.
- `peekViewTitleDescription.foreground`: 픽 뷰 제목 정보의 색상.
- `peekViewTitleLabel.foreground`: 픽 뷰 제목의 색상.

<!--
- `peekView.border`: Color of the peek view borders and arrow.
- `peekViewEditor.background`: Background color of the peek view editor.
- `peekViewEditorGutter.background`: Background color of the gutter in the peek view editor.
- `peekViewEditor.matchHighlightBackground`: Match highlight color in the peek view editor.
- `peekViewEditor.matchHighlightBorder`: Match highlight border color in the peek view editor.
- `peekViewResult.background`: Background color of the peek view result list.
- `peekViewResult.fileForeground`: Foreground color for file nodes in the peek view result list.
- `peekViewResult.lineForeground`: Foreground color for line nodes in the peek view result list.
- `peekViewResult.matchHighlightBackground`: Match highlight color in the peek view result list.
- `peekViewResult.selectionBackground`: Background color of the selected entry in the peek view result list.
- `peekViewResult.selectionForeground`: Foreground color of the selected entry in the peek view result list.
- `peekViewTitle.background`: Background color of the peek view title area.
- `peekViewTitleDescription.foreground`: Color of the peek view title info.
- `peekViewTitleLabel.foreground`: Color of the peek view title. -->

## 병합 충돌

<!-- ## Merge conflicts-->

병합 충돌 장식은 에디터가 특별한 diff 범위를 포함하고 있을때 나타납니다.

<!--
Merge conflict decorations are shown when the editor contains special diff ranges. -->

![Merge ranges](images/theme-color/merge-ranges.png)


- `merge.currentHeaderBackground`: 인라인 병합 충돌의 현재 헤더 배경색. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `merge.currentContentBackground`: 인라인 병합 충돌의 현재 내용 배경색. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `merge.incomingHeaderBackground`: 인라인 병합 충돌의 이후 헤더 배경색.뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `merge.incomingContentBackground`: 인라인 병합 충돌의 이후 내용 배경색. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `merge.border`: 인라인 병합 충돌의 헤더와 스플리터의 테두리색.
- `merge.commonContentBackground`: 인라인 병합 충돌의 공통 이전 내용의 배경색. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `merge.commonHeaderBackground`: 인라인 병합 충돌의 공통 이전 헤더의 배경색. 뒤의 내용을 숨기지 않기 위해 불투명해서는 안됩니다.
- `editorOverviewRuler.currentContentForeground`: 인라인 병합 충돌의 현재 개요 눈금 전경색.
- `editorOverviewRuler.incomingContentForeground`: 인라인 병합 충돌의 이후 개요 눈금 전경색.
- `editorOverviewRuler.commonContentForeground`: 인라인 병합 충돌의 현재 공통 이전 내용의 개요 눈금 전경색.

<!--
- `merge.currentHeaderBackground`: Current header background in inline merge conflicts. The color must not be opaque so as not to hide underlying decorations.
- `merge.currentContentBackground`: Current content background in inline merge conflicts. The color must not be opaque so as not to hide underlying decorations.
- `merge.incomingHeaderBackground`: Incoming header background in inline merge conflicts. The color must not be opaque so as not to hide underlying decorations.
- `merge.incomingContentBackground`: Incoming content background in inline merge conflicts. The color must not be opaque so as not to hide underlying decorations.
- `merge.border`: Border color on headers and the splitter in inline merge conflicts.
- `merge.commonContentBackground`: Common ancestor content background in inline merge-conflicts. The color must not be opaque so as not to hide underlying decorations.
- `merge.commonHeaderBackground`: Common ancestor header background in inline merge-conflicts. The color must not be opaque so as not to hide underlying decorations.
- `editorOverviewRuler.currentContentForeground`: Current overview ruler foreground for inline merge conflicts.
- `editorOverviewRuler.incomingContentForeground`: Incoming overview ruler foreground for inline merge conflicts.
- `editorOverviewRuler.commonContentForeground`: Common ancestor overview ruler foreground for inline merge conflicts.
-->

## 패널 색상
<!--
## Panel colors -->

패널은 에디터 영역 아래에서 보이며, 출력이나 통합 터미널 같은 뷰를 포함합니다.
<!--
Panels are shown below the editor area and contain views like Output and Integrated Terminal. -->

- `panel.background`: 패널 배경색
- `panel.border`: 에디터와 구분하기 위한 패널의 테두리색.
- `panel.dropBackground`: 패널 제목 항목을 위한 드래그 앤 드롭 피드백 색상. 이 색상은 패널 항목이 비춰보일 수 있도록 투명해야 합니다.
- `panelTitle.activeBorder`: 활성 패널 제목을 위한 테두리색.
- `panelTitle.activeForeground`: 활성 패널을 위한 제목 색.
- `panelTitle.inactiveForeground`: 비활성화 패널을 위한 제목 색.
- `panelInput.border`: 패널의 입력을 위한 입력상자 테두리 색.

<!--
- `panel.background`: Panel background color.
- `panel.border`: Panel border color to separate the panel from the editor.
- `panel.dropBackground`: Drag and drop feedback color for the panel title items. The color should have transparency so that the panel entries can still shine through.
- `panelTitle.activeBorder`: Border color for the active panel title.
- `panelTitle.activeForeground`: Title color for the active panel.
- `panelTitle.inactiveForeground`: Title color for the inactive panel.
- `panelInput.border`: Input box border for inputs in the panel. -->

### 미리보기
<!--
### Preview -->

- `imagePreview.border`: 이미지 미리보기의 이미지 테두리 색상.
<!-- - `imagePreview.border`: Border color for image in image preview.-->

## 상태 표시줄 색상
<!--
## Status Bar colors -->

상태 표시줄은 작업벤치 하단에 나타납니다.
<!--
The Status Bar is shown in the bottom of the workbench. -->

- `statusBar.background`: 표준 상태 표시줄 배경색상.
- `statusBar.foreground`: 상태 표시줄 전경색상.
- `statusBar.border`: 상태 표시줄과 에디터를 구분짓는 상태 표시줄 테두리 색상.
- `statusBar.debuggingBackground`: 프로그램이 디버깅 중일때의 상태 표시줄 배경색상.
- `statusBar.debuggingForeground`: 프로그램이 디버깅 중일때의 상태 표시줄 전경색상.
- `statusBar.debuggingBorder`: 프로그램이 디버깅 중일때의 상태 표시줄과 에디터를 구분 짓는 상태 표시줄 테두리 색상.
- `statusBar.noFolderForeground`: 열린 폴더가 없을때의 상태 표시줄 전경색상.
- `statusBar.noFolderBackground`: 열린 폴더가 없을때의 상태 표시줄 배경색상.
- `statusBar.noFolderBorder`: 열린 폴더가 없을때의 상태 표시줄과 에디터를 구분 짓는 상태 표시줄 테두리 색상.
- `statusBarItem.activeBackground`: 클릭 중일때의 상태 표시줄 항목 배경색상.
- `statusBarItem.hoverBackground`: 마우스를 위에 두었을때의 상태 표시줄 항목 배경색상.
- `statusBarItem.prominentForeground`: 상태 표시줄의 눈에 잘 띄는 항목 전경색상.
- `statusBarItem.prominentBackground`: 상태 표시줄의 눈에 잘 띄는 항목 배경색상.
- `statusBarItem.prominentHoverBackground`: 마우스를 위에 두었을때의 상태 표시줄의 눈에 잘 띄는 항목 배경색상.
- `statusBarItem.remoteBackground`: 상태 표시줄의 원격 표시기의 배경색상.
- `statusBarItem.remoteForeground`: 상태 표시줄의 원격 표시기의 전경색상.

<!--
- `statusBar.background`: Standard Status Bar background color.
- `statusBar.foreground`: Status Bar foreground color.
- `statusBar.border`: Status Bar border color separating the Status Bar and editor.
- `statusBar.debuggingBackground`: Status Bar background color when a program is being debugged.
- `statusBar.debuggingForeground`: Status Bar foreground color when a program is being debugged.
- `statusBar.debuggingBorder`: Status Bar border color separating the Status Bar and editor when a program is being debugged.
- `statusBar.noFolderForeground`: Status Bar foreground color when no folder is opened.
- `statusBar.noFolderBackground`: Status Bar background color when no folder is opened.
- `statusBar.noFolderBorder`: Status Bar border color separating the Status Bar and editor when no folder is opened.
- `statusBarItem.activeBackground`: Status Bar item background color when clicking.
- `statusBarItem.hoverBackground`: Status Bar item background color when hovering.
- `statusBarItem.prominentForeground`: Status Bar prominent items foreground color.
- `statusBarItem.prominentBackground`: Status Bar prominent items background color.
- `statusBarItem.prominentHoverBackground`: Status Bar prominent items background color when hovering.
- `statusBarItem.remoteBackground`: Background color for the remote indicator on the status bar.
- `statusBarItem.remoteForeground`: Foreground color for the remote indicator on the status bar.
-->

중요한 항목은 다른 상태 표시줄 항목과 차별화 되어 중요함을 나타냅니다. 한가지 예시로 **Toggle Tab Key Moves Focus** 커맨드 변경 모드 표시기 입니다.

<!--
Prominent items stand out from other Status Bar entries to indicate importance. One example is the **Toggle Tab Key Moves Focus** command change mode indicator. -->

## 제목 표시줄 색상

<!--
## Title Bar colors -->

- `titleBar.activeBackground`: 창이 활성화 되었을때의 제목 표시줄 배경색상. 
- `titleBar.activeForeground`: 창이 활성화 되었을때의 제목 표시줄 전경색상. 
- `titleBar.inactiveBackground`: 창이 비활성화 되었을때의 제목 표시줄 배경색상. 
- `titleBar.inactiveForeground`: 창이 활성화 되었을때의 제목 표시줄 전경색상. 
- `titleBar.border`: 제목 표시줄 테두리 색상.

<!--
- `titleBar.activeBackground`: Title Bar background when the window is active.
- `titleBar.activeForeground`: Title Bar foreground when the window is active.
- `titleBar.inactiveBackground`: Title Bar background when the window is inactive.
- `titleBar.inactiveForeground`: Title Bar foreground when the window is inactive.
- `titleBar.border`: Title bar border color. -->

## 메뉴 표시줄 색상

<!--
## Menu Bar colors -->


- `menubar.selectionForeground`: 메뉴 표시줄에서 선택된 메뉴의 전경색상.
- `menubar.selectionBackground`: 메뉴 표시줄에서 선택된 메뉴의 배경색상.
- `menubar.selectionBorder`: 메뉴 표시줄에서 선택된 메뉴의 테두리 색상.
- `menu.foreground`: 메뉴 항목의 전경색상.
- `menu.background`: 메뉴 항목의 배경색상.
- `menu.selectionForeground`: 메뉴에서 선택된 메뉴 항목의 전경색상.
- `menu.selectionBackground`: 메뉴에서 선택된 메뉴 항목의 배경색상.
- `menu.selectionBorder`: 메뉴에서 선택된 메뉴 항목의 테두리색상.
- `menu.separatorBackground`: 메뉴에서 메뉴 항목을 구분짓는 색상.
- `menu.border`: 메뉴의 테두리 색상.

<!--
- `menubar.selectionForeground`: Foreground color of the selected menu item in the menubar.
- `menubar.selectionBackground`: Background color of the selected menu item in the menubar.
- `menubar.selectionBorder`: Border color of the selected menu item in the menubar.
- `menu.foreground`: Foreground color of menu items.
- `menu.background`: Background color of menu items.
- `menu.selectionForeground`: Foreground color of the selected menu item in menus.
- `menu.selectionBackground`: Background color of the selected menu item in menus.
- `menu.selectionBorder`: Border color of the selected menu item in menus.
- `menu.separatorBackground`: Color of a separator menu item in menus.
- `menu.border`: Border color of menus.
-->

## 알림 색상

<!-- ## Notification colors-->

**주의:** 아래의 색상은 VS Code 버전 1.21 이상에서만 적용됩니다.
<!--
**Note:** The colors below only apply for VS Code versions 1.21 and higher. -->

알림은 작업공간의 우측 하단에서 위로 올라오며 나타납니다.
<!--
Notification toasts slide up from the bottom-right of the workbench. -->

![Notification Toasts](images/theme-color/notification-toast.png)

한번 알림 센터에서 열린 이후에는, 헤더를 포함한 목록으로 표시됩니다:

<!--
Once opened in the Notification Center, they are displayed in a list with a header: -->

![Notification Center](images/theme-color/notification-center.png)


- `notificationCenter.border`: 알림 센터의 테두리 색상.
- `notificationCenterHeader.foreground`: 알림 센터 헤더의 전경색상.
- `notificationCenterHeader.background`: 알림 센터 헤더의 배경색상.
- `notificationToast.border`: 알림 토스트의 테두리 색상.
- `notifications.foreground`: 알림의 전경색상.
- `notifications.background`: 알림의 배경색상.
- `notifications.border`: 알림 센터에서 다른 알림과 구분 짓는 알림의 테두리 색상.
- `notificationLink.foreground`: 알림 링크의 전경색상.
- `notificationsErrorIcon.foreground`: 알림 에러 아이콘의 색상.
- `notificationsWarningIcon.foreground`: 알림 경고 아이콘의 색상.
- `notificationsInfoIcon.foreground`: 알림 정보 아이콘의 색상.

<!--
- `notificationCenter.border`: Notification Center border color.
- `notificationCenterHeader.foreground`: Notification Center header foreground color.
- `notificationCenterHeader.background`: Notification Center header background color.
- `notificationToast.border`: Notification toast border color.
- `notifications.foreground`: Notification foreground color.
- `notifications.background`: Notification background color.
- `notifications.border`: Notification border color separating from other notifications in the Notification Center.
- `notificationLink.foreground`: Notification links foreground color.
- `notificationsErrorIcon.foreground`: The color used for the notification error icon.
- `notificationsWarningIcon.foreground`: The color used for the notification warning icon.
- `notificationsInfoIcon.foreground`: The color used for the notification info icon.
-->

만약 VS Code가 1.21 (2018 2월) 릴리즈 이전 버전이라면, 아래는 이전의 색상입니다 (현재 지원하지 않음):

<!--
If you target VS Code versions before the 1.21 (February 2018) release, these are the old (no longer supported) colors: -->

- `notification.background`
- `notification.foreground`
- `notification.buttonBackground`
- `notification.buttonForeground`
- `notification.buttonHoverBackground`
- `notification.errorBackground`
- `notification.errorForeground`
- `notification.infoBackground`
- `notification.infoForeground`
- `notification.warningBackground`
- `notification.warningForeground`

## 익스텐션

<!--
## Extensions -->

- `extensionButton.prominentForeground`: 익스텐션 뷰 버튼의 전경색상. (예를 들어 **Install** 버튼)
- `extensionButton.prominentBackground`: 익스텐션 뷰 버튼의 배경색상. 
- `extensionButton.prominentHoverBackground`: 익스텐션 뷰 버튼의 말풍선 배경색상. 
- `extensionBadge.remoteBackground`: 익스텐션 뷰의 원격 뱃지 배경색상.
- `extensionBadge.remoteForeground`: 익스텐션 뷰의 원격 뱃지 전경색상.

<!--
- `extensionButton.prominentForeground`: Extension view button foreground color (for example **Install** button).
- `extensionButton.prominentBackground`: Extension view button background color.
- `extensionButton.prominentHoverBackground`: Extension view button background hover color.
- `extensionBadge.remoteBackground`: Background color for the remote badge in the extensions view.
- `extensionBadge.remoteForeground`: Foreground color for the remote badge in the extensions view.
-->

## 빠른 선택

<!-- ## Quick picker -->

- `pickerGroup.border`: 빠른 선택 (빠른 열기)의 그룹 테두리 색상.
- `pickerGroup.foreground`: 빠른 선택 (빠른 열기)의 그룹 레이블 색상.
- `quickInput.background`: 빠른 입력 배경색상. 빠른 입력 위젯은 색상 테마 선택자 같은 뷰를 담는 컨테이너 입니다.
- `quickInput.foreground`: 빠른 입력 전경색상. 빠른 입력 위젯은 색상 테마 선택자 같은 뷰를 담는 컨테이너 입니다.

<!--
- `pickerGroup.border`: Quick picker (Quick Open) color for grouping borders.
- `pickerGroup.foreground`: Quick picker (Quick Open) color for grouping labels.
- `quickInput.background`: Quick input background color. The quick input widget is the container for views like the color theme picker.
- `quickInput.foreground`: Quick input foreground color. The quick input widget is the container for views like the color theme picker.
-->

## 통합 터미널 색상
<!--
## Integrated Terminal colors -->


- `terminal.background`: 통합 터미널의 뷰포트 배경색상.
- `terminal.border`: 터미널 내에서 분할 창을 구분하는 테두리의 색상. 기본값은 panel.border 입니다.
- `terminal.foreground`: 통합 터미널의 기본 전경색상.
- `terminal.ansiBlack`: 터미널의 'Black' ANSI 색상.
- `terminal.ansiBlue`: 터미널의 'Blue' ANSI 색상.
- `terminal.ansiBrightBlack`: 터미널의 'BrightBlack' ANSI 색상.
- `terminal.ansiBrightBlue`: 터미널의 'BrightBlue' ANSI 색상.
- `terminal.ansiBrightCyan`: 터미널의 'BrightCyan' ANSI 색상.
- `terminal.ansiBrightGreen`: 터미널의 'BrightGreen' ANSI 색상.
- `terminal.ansiBrightMagenta`: 터미널의 'BrightMagenta' ANSI 색상.
- `terminal.ansiBrightRed`: 터미널의 'BrightRed' ANSI 색상.
- `terminal.ansiBrightWhite`: 터미널의 'BrightWhite' 색상.
- `terminal.ansiBrightYellow`: 터미널의 'BrightYellow' ANSI 색상.
- `terminal.ansiCyan`: 터미널의 'Cyan' ANSI 색상.
- `terminal.ansiGreen`: 터미널의 'Green' ANSI 색상.
- `terminal.ansiMagenta`: 터미널의 'Magenta' ANSI 색상.
- `terminal.ansiRed`: 터미널의 'Red' ANSI 색상.
- `terminal.ansiWhite`: 터미널의 'White' ANSI 색상.
- `terminal.ansiYellow`: 터미널의 'Yellow' ANSI 색상.
- `terminal.selectionBackground`: 터미널의 선택 배경색상. 
- `terminalCursor.background`: 터미널 커서의 배경색상. 블록 커서를 통해 커스터마이즈 할 수 있습니다.
- `terminalCursor.foreground`: 터미널 커서의 전경색상.

<!--
- `terminal.background`: The background of the Integrated Terminal's viewport.
- `terminal.border`: The color of the border that separates split panes within the terminal. This defaults to panel.border.
- `terminal.foreground`: The default foreground color of the Integrated Terminal.
- `terminal.ansiBlack`: 'Black' ANSI color in the terminal.
- `terminal.ansiBlue`: 'Blue' ANSI color in the terminal.
- `terminal.ansiBrightBlack`: 'BrightBlack' ANSI color in the terminal.
- `terminal.ansiBrightBlue`: 'BrightBlue' ANSI color in the terminal.
- `terminal.ansiBrightCyan`: 'BrightCyan' ANSI color in the terminal.
- `terminal.ansiBrightGreen`: 'BrightGreen' ANSI color in the terminal.
- `terminal.ansiBrightMagenta`: 'BrightMagenta' ANSI color in the terminal.
- `terminal.ansiBrightRed`: 'BrightRed' ANSI color in the terminal.
- `terminal.ansiBrightWhite`: 'BrightWhite' ANSI color in the terminal.
- `terminal.ansiBrightYellow`: 'BrightYellow' ANSI color in the terminal.
- `terminal.ansiCyan`: 'Cyan' ANSI color in the terminal.
- `terminal.ansiGreen`: 'Green' ANSI color in the terminal.
- `terminal.ansiMagenta`: 'Magenta' ANSI color in the terminal.
- `terminal.ansiRed`: 'Red' ANSI color in the terminal.
- `terminal.ansiWhite`: 'White' ANSI color in the terminal.
- `terminal.ansiYellow`: 'Yellow' ANSI color in the terminal.
- `terminal.selectionBackground`: The selection background color of the terminal.
- `terminalCursor.background`: The background color of the terminal cursor. Allows customizing the color of a character overlapped by a block cursor.
- `terminalCursor.foreground`: The foreground color of the terminal cursor.
-->

## 디버그

<!--
## Debug -->

- `debugToolBar.background`: 디버그 툴바 배경색상.
- `debugToolBar.border`: 디버그 툴바 테두리색상.
- `editor.stackFrameHighlightBackground`: 에디터의 상단 스택 프레임 강조의 배경색.
- `editor.focusedStackFrameHighlightBackground`: 에디터의 포커스 된 스택 프레임 강조의 배경색.
<!--
- `debugToolBar.background`: Debug toolbar background color.
- `debugToolBar.border`: Debug toolbar border color.
- `editor.stackFrameHighlightBackground`: Background color of the top stack frame highlight in the editor.
- `editor.focusedStackFrameHighlightBackground`: Background color of the focused stack frame highlight in the editor.
-->

## 시작 페이지
<!--
## Welcome page -->

- `welcomePage.background`: 시작 페이지의 배경색상.
- `welcomePage.buttonBackground`: 시작페이지 버튼의 배경색상.
- `welcomePage.buttonHoverBackground`: 시작페이지 버튼의 말풍선 배경 색상.
- `walkThrough.embeddedEditorBackground`: Interactive Playground에 임베드 된 에디터의 배경색상.

<!--
- `welcomePage.background`: Background color for the Welcome page.
- `welcomePage.buttonBackground`: Background color for the buttons on the Welcome page.
- `welcomePage.buttonHoverBackground`: Hover background color for the buttons on the Welcome page.
- `walkThrough.embeddedEditorBackground`: Background color for the embedded editors on the Interactive Playground. -->

## 깃 색상

<!--
## Git colors -->

- `gitDecoration.addedResourceForeground`: 추가 된 깃 리소스의 색상.파일 레이블과 SCM 뷰릿에 사용됩니다.
- `gitDecoration.modifiedResourceForeground`: 수정 된 깃 리소스의 색상.파일 레이블과 SCM 뷰릿에 사용됩니다.
- `gitDecoration.deletedResourceForeground`: 삭제 된 깃 리소스의 색상.파일 레이블과 SCM 뷰릿에 사용됩니다.
- `gitDecoration.untrackedResourceForeground`: 추적되지 않은 깃 리소스의 색상.파일 레이블과 SCM 뷰릿에 사용됩니다.
- `gitDecoration.ignoredResourceForeground`: 무시된 깃 리소스의 색상.파일 레이블과 SCM 뷰릿에 사용됩니다.
- `gitDecoration.conflictingResourceForeground`: 충돌하는 깃 리소스의 색상.파일 레이블과 SCM 뷰릿에 사용됩니다.
- `gitDecoration.submoduleResourceForeground`: 하위 모듈 리소스의 색상.

<!--
- `gitDecoration.addedResourceForeground`: Color for added Git resources. Used for file labels and the SCM viewlet.
- `gitDecoration.modifiedResourceForeground`: Color for modified Git resources. Used for file labels and the SCM viewlet.
- `gitDecoration.deletedResourceForeground`: Color for deleted Git resources. Used for file labels and the SCM viewlet.
- `gitDecoration.untrackedResourceForeground`: Color for untracked Git resources. Used for file labels and the SCM viewlet.
- `gitDecoration.ignoredResourceForeground`: Color for ignored Git resources. Used for file labels and the SCM viewlet.
- `gitDecoration.conflictingResourceForeground`: Color for conflicting Git resources. Used for file labels and the SCM viewlet.
- `gitDecoration.submoduleResourceForeground`: Color for submodule resources. -->

## 에디터 설정 색상

<!--
## Settings Editor colors -->

**주의:** 이 색상들은 GUI 설정 에디터용이며, `Preferences: Open Settings (UI)` 커맨드를 통해 열 수 있습니다.

<!--
**Note:** These colors are for the GUI settings editor which can be opened with the `Preferences: Open Settings (UI)` command. -->

- `settings.headerForeground`: 활성 제목이나 섹션 헤더를 위한 전경색상.
- `settings.modifiedItemIndicator`: 수정된 설정을 나타내는 줄의 색상.
- `settings.dropdownBackground`: 드롭다운 배경색상.
- `settings.dropdownForeground`: 드롭다운 전경색상.
- `settings.dropdownBorder`: 드롭다운 테두리 색상.
- `settings.dropdownListBorder`: 드롭다운 목록의 테두리 색상.
- `settings.checkboxBackground`: 체크박스 배경색상.
- `settings.checkboxForeground`: 체크박스 전경색상.
- `settings.checkboxBorder`: 체크박스 테두리색상.
- `settings.textInputBackground`: 텍스트 입력 상자 배경색상.
- `settings.textInputForeground`: 텍스트 입력 상자 전경색상.
- `settings.textInputBorder`: 텍스트 입력 상자 테두리색상.
- `settings.numberInputBackground`: 숫자 입력 상자 배경색상.
- `settings.numberInputForeground`: 숫자 입력 상자 전경색상.
- `settings.numberInputBorder`: 숫자 입력 상자 테두리색상.

<!--
- `settings.headerForeground`: The foreground color for a section header or active title.
- `settings.modifiedItemIndicator`: The line that indicates a modified setting.
- `settings.dropdownBackground`: Dropdown background.
- `settings.dropdownForeground`: Dropdown foreground.
- `settings.dropdownBorder`: Dropdown border.
- `settings.dropdownListBorder`: Dropdown list border.
- `settings.checkboxBackground`: Checkbox background.
- `settings.checkboxForeground`: Checkbox foreground.
- `settings.checkboxBorder`: Checkbox border.
- `settings.textInputBackground`: Text input box background.
- `settings.textInputForeground`: Text input box foreground.
- `settings.textInputBorder`: Text input box border.
- `settings.numberInputBackground`: Number input box background.
- `settings.numberInputForeground`: Number input box foreground.
- `settings.numberInputBorder`: Number input box border. -->

## 브레드크럼
<!--
## Breadcrumbs -->

브레드크럼 탐색을 위한 테마색상입니다:

<!--
The theme colors for breadcrumbs navigation: -->

- `breadcrumb.foreground`: 브레드크럼 항목의 색상.
- `breadcrumb.background`: 브레드크럼 항목의 배경색상.
- `breadcrumb.focusForeground`: 포커스된 브레드크럼 항목의 색상.
- `breadcrumb.activeSelectionForeground`: 선택된 브레드크럼 항목의 색상.
- `breadcrumbPicker.background`: 브레드크럼 항목 선택기의 배경색상.

<!--
- `breadcrumb.foreground`: Color of breadcrumb items.
- `breadcrumb.background`: Background color of breadcrumb items.
- `breadcrumb.focusForeground`: Color of focused breadcrumb items.
- `breadcrumb.activeSelectionForeground`: Color of selected breadcrumb items.
- `breadcrumbPicker.background`: Background color of breadcrumb item picker. -->

## 스니펫
<!--
## Snippets -->

스니펫을 위한 테마 색상입니다:
<!--
The theme colors for snippets: -->

- `editor.snippetTabstopHighlightBackground`: 스니펫 탭스톱의 강조 배경색상.
- `editor.snippetTabstopHighlightBorder`: 스니펫 탭스톱의 강조 테두리 색상.
- `editor.snippetFinalTabstopHighlightBackground`: 마지막 스니펫 탭스톱의 강조 배경색상.
- `editor.snippetFinalTabstopHighlightBorder`: 마지막 스니펫 탭스톱의 강조 테두리 색상.

<!--
- `editor.snippetTabstopHighlightBackground`: Highlight background color of a snippet tabstop.
- `editor.snippetTabstopHighlightBorder`: Highlight border color of a snippet tabstop.
- `editor.snippetFinalTabstopHighlightBackground`: Highlight background color of the final tabstop of a snippet.
- `editor.snippetFinalTabstopHighlightBorder`: Highlight border color of the final tabstop of a snippet. -->

## 심볼 아이콘

<!--
## Symbol Icons -->

아웃라인 뷰, 브레드크럼 탐색 그리고 제안 위젯에서 나타나는 심볼 아이콘을 위한 테마 색상입니다:

<!--
The theme colors for symbol icons that appears in the Outline view, breadcrumb navigation, and suggest widget: -->


- `symbolIcon.arrayForeground`: 배열 심볼의 전경색상.
- `symbolIcon.booleanForeground`: 불리안 심볼의 전경색상.
- `symbolIcon.classForeground`: 클래스 심볼의 전경색상.
- `symbolIcon.colorForeground`: 색상 심볼의 전경색상.
- `symbolIcon.constantForeground`: 상수 심볼의 전경색상.
- `symbolIcon.constructorForeground`: 생성자 심볼의 전경색상.
- `symbolIcon.enumeratorForeground`: 열거자 심볼의 전경색상.
- `symbolIcon.enumeratorMemberForeground`: 열거자 구성원 심볼의 전경색상.
- `symbolIcon.eventForeground`: 이벤트 심볼의 전경색상.
- `symbolIcon.fieldForeground`: 필드 심볼의 전경색상.
- `symbolIcon.fileForeground`: 파일 심볼의 전경색상.
- `symbolIcon.folderForeground`: 폴더 심볼의 전경색상.
- `symbolIcon.functionForeground`: 함수 심볼의 전경색상.
- `symbolIcon.interfaceForeground`: 인터페이스 심볼의 전경색상.
- `symbolIcon.keyForeground`: 키 심볼의 전경색상.
- `symbolIcon.keywordForeground`: 키워드 심볼의 전경색상.
- `symbolIcon.methodForeground`: 메소드 심볼의 전경색상.
- `symbolIcon.moduleForeground`: 모듈 심볼의 전경색상.
- `symbolIcon.namespaceForeground`: 네임스페이스 심볼의 전경색상.
- `symbolIcon.nullForeground`: null 심볼의 전경색상.
- `symbolIcon.numberForeground`: 숫자 심볼의 전경색상.
- `symbolIcon.objectForeground`: 오브젝트 심볼의 전경색상.
- `symbolIcon.operatorForeground`: 연산자 심볼의 전경색상.
- `symbolIcon.packageForeground`: 패키지 심볼의 전경색상.
- `symbolIcon.propertyForeground`: 속성 심볼의 전경색상.
- `symbolIcon.referenceForeground`: 참조 심볼의 전경색상.
- `symbolIcon.snippetForeground`: 스니펫 심볼의 전경색상.
- `symbolIcon.stringForeground`: 문자열 심볼의 전경색상.
- `symbolIcon.structForeground`: 구조체 심볼의 전경색상.
- `symbolIcon.textForeground`: 텍스트 심볼의 전경색상.
- `symbolIcon.typeParameterForeground`: 타입 매개변수 심볼의 전경색상.
- `symbolIcon.unitForeground`: 단위 심볼의 전경색상.
- `symbolIcon.variableForeground`: 변수 심볼의 전경색상.

<!--
- `symbolIcon.arrayForeground`: The foreground color for array symbols.
- `symbolIcon.booleanForeground`: The foreground color for boolean symbols.
- `symbolIcon.classForeground`: The foreground color for class symbols.
- `symbolIcon.colorForeground`: The foreground color for color symbols.
- `symbolIcon.constantForeground`: The foreground color for constant symbols.
- `symbolIcon.constructorForeground`: The foreground color for constructor symbols.
- `symbolIcon.enumeratorForeground`: The foreground color for enumerator symbols.
- `symbolIcon.enumeratorMemberForeground`: The foreground color for enumerator member symbols.
- `symbolIcon.eventForeground`: The foreground color for event symbols.
- `symbolIcon.fieldForeground`: The foreground color for field symbols.
- `symbolIcon.fileForeground`: The foreground color for file symbols.
- `symbolIcon.folderForeground`: The foreground color for folder symbols.
- `symbolIcon.functionForeground`: The foreground color for function symbols.
- `symbolIcon.interfaceForeground`: The foreground color for interface symbols.
- `symbolIcon.keyForeground`: The foreground color for key symbols.
- `symbolIcon.keywordForeground`: The foreground color for keyword symbols.
- `symbolIcon.methodForeground`: The foreground color for method symbols.
- `symbolIcon.moduleForeground`: The foreground color for module symbols.
- `symbolIcon.namespaceForeground`: The foreground color for namespace symbols.
- `symbolIcon.nullForeground`: The foreground color for null symbols.
- `symbolIcon.numberForeground`: The foreground color for number symbols.
- `symbolIcon.objectForeground`: The foreground color for object symbols.
- `symbolIcon.operatorForeground`: The foreground color for operator symbols.
- `symbolIcon.packageForeground`: The foreground color for package symbols.
- `symbolIcon.propertyForeground`: The foreground color for property symbols.
- `symbolIcon.referenceForeground`: The foreground color for reference symbols.
- `symbolIcon.snippetForeground`: The foreground color for snippet symbols.
- `symbolIcon.stringForeground`: The foreground color for string symbols.
- `symbolIcon.structForeground`: The foreground color for struct symbols.
- `symbolIcon.textForeground`: The foreground color for text symbols.
- `symbolIcon.typeParameterForeground`: The foreground color for type parameter symbols.
- `symbolIcon.unitForeground`: The foreground color for unit symbols.
- `symbolIcon.variableForeground`: The foreground color for variable symbols.-->

## 디버그 아이콘

<!--
## Debug Icons -->

- `debugIcon.breakpointForeground`: 중지점 아이콘 색상.
- `debugIcon.breakpointDisabledForeground`: 비활성된 중지점 아이콘 색상.
- `debugIcon.breakpointUnverifiedForeground`: 미검증된 중지점 아이콘 색상.
- `debugIcon.breakpointCurrentStackframeForeground`: 현재 중지점 스택 프레임 아이콘 색상.
- `debugIcon.breakpointStackframeForeground`: 모든 중지점 스택 프레임 아이콘 색상.
- `debugIcon.startForeground`: 디버그 툴바의 디버깅 시작 아이콘 색상. 
- `debugIcon.pauseForeground`: 디버그 툴바의 일시중지 아이콘 색상. 
- `debugIcon.stopForeground`: 디버그 툴바의 정지 아이콘 색상. 
- `debugIcon.disconnectForeground`: 디버그 툴바의 연결해제 아이콘 색상. 
- `debugIcon.restartForeground`: 디버그 툴바의 재시작 아이콘 색상. 
- `debugIcon.stepOverForeground`: 디버그 툴바의 단계 진행 아이콘 색상. 
- `debugIcon.stepIntoForeground`: 디버그 툴바의 단계로 이동 아이콘 색상. 
- `debugIcon.stepOutForeground`: 디버그 툴바의 건너뛰기 아이콘 색상. 
- `debugIcon.continueForeground`: 디버그 툴바의 이어하기 아이콘 색상. 
- `debugIcon.stepBackForeground`: 디버그 툴바의 단계 취소 아이콘 색상. 

<!--
- `debugIcon.breakpointForeground`: Icon color for breakpoints.
- `debugIcon.breakpointDisabledForeground`: Icon color for disabled breakpoints.
- `debugIcon.breakpointUnverifiedForeground`: Icon color for unverified breakpoints.
- `debugIcon.breakpointCurrentStackframeForeground`: Icon color for the current breakpoint stack frame.
- `debugIcon.breakpointStackframeForeground`: Icon color for all breakpoint stack frames.
- `debugIcon.startForeground`: Debug toolbar icon for start debugging.
- `debugIcon.pauseForeground`: Debug toolbar icon for pause.
- `debugIcon.stopForeground`: Debug toolbar icon for stop.
- `debugIcon.disconnectForeground`: Debug toolbar icon for disconnect.
- `debugIcon.restartForeground`: Debug toolbar icon for restart.
- `debugIcon.stepOverForeground`: Debug toolbar icon for step over.
- `debugIcon.stepIntoForeground`: Debug toolbar icon for step into.
- `debugIcon.stepOutForeground`: Debug toolbar icon for step over.
- `debugIcon.continueForeground`: Debug toolbar icon for continue.
- `debugIcon.stepBackForeground`: Debug toolbar icon for step back.
-->

## 익스텐션 색상
<!--
## Extension colors -->

색상 id는 [색상 기여 포인트](/api/references/contribution-points#contributes.colors)를 통해 익스텐션을 통해 제공 될 수도 있습니다. 이 색상들은 `workbench.colorCustomizations` 설정 및 색상 테마 정의 파일에서 코드 완성을 사용 할때도 나타납니다. 사용자는 [익스텐션 기여](/docs/editor/extension-gallery#_extension-details)탭에서 익스텐션이 정의한 색상을 확인 할 수 있습니다.

<!--
Color ids can also be contributed by extensions through the [color contribution point](/api/references/contribution-points#contributes.colors). These colors also appear when using code complete in the `workbench.colorCustomizations` settings and the color theme definition file. Users can see what colors an extension defines in the [extension contributions](/docs/editor/extension-gallery#_extension-details) tab.
-->
