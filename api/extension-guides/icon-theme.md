---
layout: default
parent: Extension Guides
title: Icon Theme
nav_order: 4
description: ""
---

# 아이콘 테마

<!--
# Icon Theme -->

Visual Studio Code는 UI를 통해 파일명 옆에 아이콘을 표시하며, 익스텐션을 통해서 사용자가 선택 할 수 있는 새로운 아이콘들을 제공 할 수 있습니다. 

<!--
Visual Studio Code displays icons next to filenames throughout its UI, and extensions can contribute new sets of file icons that users can choose from.-->

## 새 아이콘 테마 추가

<!--
## Adding a new Icon Theme -->

여러분은 원하는 아이콘(보통 SVG)과 아이콘 폰트를 이용하여 원하는 아이콘 테마를 생성 할 수 있습니다. 예시로, 아래의 2가지 빌트인 테마를 확인하십시오: [Minimal](https://github.com/Microsoft/vscode/tree/master/extensions/theme-defaults) 과 [Seti](https://github.com/Microsoft/vscode/tree/master/extensions/theme-seti).

<!--
You can create your own icon theme from icons (preferably SVG) and from icon fonts. As example, check out the two built-in themes: [Minimal](https://github.com/Microsoft/vscode/tree/master/extensions/theme-defaults) and [Seti](https://github.com/Microsoft/vscode/tree/master/extensions/theme-seti). --?>

시작하기 위해, VS Code 익스텐션을 생성하고 contribution point에 `iconTheme`를 추가
하십시오. 
<!-- To begin, create a VS Code extension and add the `iconTheme` contribution point.-->

```json
{
  "contributes": {
    "iconThemes": [
      {
        "id": "turtles",
        "label": "Turtles",
        "path": "./fileicons/turtles-icon-theme.json"
      }
    ]
  }
}
```

`id`는 아이콘 테마의 식별자이며, 현재는 내부에서만 사용 됩니다. 그러나 추후에, 설정에서 사용 될 수도 있으니 특별하지만 읽을 수 있게 생성하십시오. `label`은 아이콘 테마 드롭다운 선택지에서 보여집니다. `path`는 익스텐션내부에서 아이콘 셋을 포함하는 파일의 위치를 가르킵니다. 만약 아이콘 셋의 이름을 `*icon-theme.json`의 형식으로 지은경우, VS Code에서 완성과 호버링이 지원 될 것입니다. 

<!--
The `id` is the identifier for the icon theme. It is currently only used internally. In the future, it might be used in the settings, so make it unique but also readable. `label` is shown in the icon theme picker drop-down. The `path` points to a file in the extension that defines the icon set. If your icon set name follows the `*icon-theme.json` name scheme, you will get completion support and hovers in VS Code. -->

### 아이콘 셋 파일
<!--
### Icon Set File -->

아이콘 셋 파일은 파일 아이콘 구성과 정의를 포함하는 JSON 파일입니다. 
<!--
The icon set file is a JSON file consisting of file icon associations and icon definitions. -->

아이콘 구성은 파일 타입을('file','folder','json-file'...) 아이콘 정의에 할당 합니다. 아이콘 정의는 아이콘이 어디에 위치해 있는지를 지정합니다: 이미지 파일이거나 폰트의 글리프일 수 있습니다. 
<!--
An icon association maps a file type ('file', 'folder', 'json-file'...) to an icon definition. Icon definitions define where the icon is located: That can be an image file or also glyph in a font. -->

### 아이콘 정의
<!--
### Icon definitions -->

`iconDefinitions` 부분은 모든 정의를 포함합니다. 각각의 정의는 리퍼런스에 쓰이기 위한 id를 가지고 있습니다. 정의는 또한 여러개의 파일 구성으로 부터 리퍼런스 될 수 있습니다. 

<!--The `iconDefinitions` section contains all definitions. Each definition has an id, which will be used to reference the definition. A definition can be referenced also by more than one file association.-->

```json
{
  "iconDefinitions": {
    "_folder_dark": {
      "iconPath": "./images/Folder_16x_inverse.svg"
    }
  }
}
```

위의 예시는 `_folder_dark`라는 식별자를 포함하는 아이콘 정의입니다. 
<!--
This icon definition above contains a definition with the identifier `_folder_dark`. -->

지원되는 속성은 다음과 같습니다:

<!--
The following properties are supported: -->

- `iconPath`: svg/png를 사용하는 경우: 이미지의 경로입니다. 
- `fontCharacter`: 글리프 폰트를 사용하는 경우: 사용할 폰트의 캐릭터입니다. 
- `fontColor`: 글리프 폰트를 사용하는 경우: 글리프의 색상을 지정합니다.
- `fontSize`: 폰트를 사용하는 경우: 폰트의 크기이며, 폰트에서 지정하는 값이 기본값입니다. 부모 폰트 사이즈를 기준으로 상대 값을 가져야 합니다(예 150%)
- `fontId`: 폰트를 사용하는 경우: 폰트의 식별자입니다. 지정되지 않았다면, 폰트 지정 섹션의 첫번째 폰트가 사용됩니다. 

<!--
- `iconPath`: When using a svg/png: the path to the image.
- `fontCharacter`: When using a glyph font: The character in the font to use.
- `fontColor`: When using a glyph font: The color to use for the glyph.
- `fontSize`: When using a font: The font size. By default, the size specified in the font specification is used. Should be a relative size (e.g. 150%) to the parent font size.
- `fontId`: When using a font: The id of the font. If not specified, the first font specified in font specification section will be picked. -->

### 파일 구성
<!--
### File association -->

아이콘은 폴더, 폴더이름, 파일, 파일 익스텐션, 파일명 그리고 [language ids](/api/references/contribution-points#contributes.languages)에 사용 될수 있습니다. 
<!--
Icons can be associated to folders, folder names, files, file extensions, file names and [language ids](/api/references/contribution-points#contributes.languages).-->

추가적으로 각각의 구성은 'light' 와 'highContrast' 색상 테마를 지정 할 수 있습니다. 
<!--
Additionally each of these associations can be refined for 'light' and 'highContrast' color themes. -->

각 파일 구성은 아이콘 정의를 가르킵니다. 
<!--
Each file association points to an icon definition. -->

```json
{
  "file": "_file_dark",
  "folder": "_folder_dark",
  "folderExpanded": "_folder_open_dark",
  "folderNames": {
    ".vscode": "_vscode_folder"
  },
  "fileExtensions": {
    "ini": "_ini_file"
  },
  "fileNames": {
    "win.ini": "_win_ini_file"
  },
  "languageIds": {
    "ini": "_ini_file"
  },
  "light": {
    "folderExpanded": "_folder_open_light",
    "folder": "_folder_light",
    "file": "_file_light",
    "fileExtensions": {
      "ini": "_ini_file_light"
    }
  },
  "highContrast": {}
}
```

- `file`은 어느 익스텐션, 파일명, 혹은 language id와 일치 하지 않는 모든 파일을 위한 파일 아이콘 기본 값입니다. 현재 파일 아이콘 정의에 지정 된 모든 속성들이 상속 됩니다.(폰트 글리프에만 해당하여 폰트크기에 유용합니다.)
- `folder`는 접힌 폴더를 위한 아이콘 이며, 만약 `folderExpanded`가 설정 되지 않았을때는 확장된 폴더에도 적용 됩니다. 특정 폴더 이름을 위한 아이콘은 `folderNames`속성을 통해 구성 될 수 있습니다.
- `folderExpanded`는 확장된 폴더를 위한 아이콘입니다. 이는 옵션이며, 만약 설정 되지 않은 경우 `folder`에 설정된 아이콘이 사용 될것입니다. 
- `folderNames`는 폴더 이름에 아이콘을 설정 합니다. 구성 내용은 경로 단위를 포함하지 않은 폴더 이름입니다. 패턴이나 와일드카드(`*`)는 지원되지 않으며, 대소문자를 구분 하지 않습니다. 
- `folderNamesExpanded`는 확장된 폴더를 위한 아이콘 구성입니다. 구성 내용은 경로 단위를 포함하지 않은 폴더 이름입니다. 패턴이나 와일드카드(`*`)는 지원되지 않으며, 대소문자를 구분 하지 않습니다. 
- `rootFolder` 는 접힌 작업공간 루트 폴더를 위한 아이콘 입니다. 만약 `rootFolderExpanded`가 설정 되지 않았을 경우 확장된 루트 폴더 아이콘에도 적용됩니다. 만약 `rootFolder`가 설정 되지 않을 경우 `folder`에 설정된 아이콘이 작업공간 루트 폴더에 나타날 것입니다. 
- `rootFolderExpanded`는 확장된 루트 폴더 아이콘을 위한 구성입니다. 만약 설정 되지 않았을 경우 `rootFolder`에 적용된 아이콘이 사용 될 것입니다. 
- `languageIds`는 아이콘의 언어를 설정합니다. language id의 구성 내용은 [language contribution point](/api/references/contribution-points#contributes.languages)에 정의되어 있습니다. 파일의 언어는 파일 익스텐션과 language contribution에 정의된 파일명을 기준으로 평가 됩니다. language contribution 의 'first line match'는 적용 되지 않는것에 주의하십시오. 
- `fileExtensions`는 파일 익스텐션에 아이콘을 설정합니다. 구성 내용은 파일 익스텐션의 이름입니다. 익스텐션 이름은 점(`.`) 이후의 파일명입니다. 여러개의 점을 포함한 `lib.d.ts`와 같은 파일 이름은 `d.ts`, `ts` 와 같이 여러개의 익스텐션에서 인식될 수 있습니다. 익스텐션은 대소문자를 구분하지 않습니다. 
- `fileNames`는 파일 이름에 아이콘을 구성합니다. 구성 내용은 경로를 포함하지 않은 완전한 파일 이름 입니다. 패턴이나 와일드카드(`*`)는 지원되지 않으며, 대소문자를 구분하지 않습니다. 'fileName'은 가장 우선으로 인식되어, 일치하는 파일 익스텐션이나 language Id에도 우선으로 아이콘을 지정합니다.

<!--
- `file` is the default file icon, shown for all files that don't match any extension, filename or language id. Currently all properties defined by the definition of the file icon will be inherited (only relevant for font glyphs, useful for the fontSize).
- `folder` is the folder icon for collapsed folders, and if `folderExpanded` is not set, also for expanded folders. Icons for specific folder names can be associated using the `folderNames` property.
  The folder icon is optional. If not set, no icon will be shown for folder.
- `folderExpanded` is the folder icon for expanded folders. The expanded folder icon is optional. If not set, the icon defined for `folder` will be shown.
- `folderNames` associates folder names to icons. The key of the set is the folder name, not including any path segments. Patterns or wildcards are not supported. Folder name matching is case insensitive.
- `folderNamesExpanded` associates folder names to icons for expanded folder. The key of the set is the folder name, not including any path segments. Patterns or wildcards are not supported. Folder name matching is case insensitive.
- `rootFolder` is the folder icon for collapsed workspace root folders , and if `rootFolderExpanded` is not set, also for expanded workspace root folders. If not set, the icon defined for `folder` will be shown for workspace root folders.
- `rootFolderExpanded` is the folder icon for expanded workspace root folders. If not set, the icon defined for `rootFolder` will be shown for exanded workspace root folders.
- `languageIds` associates languages to icons. The key in the set is the language id as defined in the [language contribution point](/api/references/contribution-points#contributes.languages). The language of a file is evaluated based on the file extensions and file names as defined in the language contribution. Note that the 'first line match' of the language contribution is not considered.
- `fileExtensions` associates file extensions to icons. The key in the set is the file extension name. The extension name is a file name segment after a dot (not including the dot). File names with multiple dots such as `lib.d.ts` can match multiple extensions; 'd.ts' and 'ts'. Extensions are compared case insensitive.
- `fileNames` associates file names to icons. The key in the set is the full file name, not including any path segments. Patterns or wildcards are not supported. File name matching is case insensitive. A 'fileName' match is the strongest match, and the icon associated to the file name will be preferred over an icon of a matching fileExtension and also of a matching language Id.
-->

파일 익스텐션 은 language 보다 높은 우선순위를 가지지만 파일명에 비해서는 낮은 우선순위를 가집니다. 
<!-- A file extension match is preferred over a language match, but is weaker than a file name match.-->

`light`와 `highContrast`는 위의 목록과 동일한 속성을 가집니다. 그러나 해당하는 테마로 아이콘을 오버라이드합니다. 

<!--The `light` and the `highContrast` section have the same file association properties as just listed. They allow to override icons for the corresponding themes.-->

### 폰트 정의
<!--
### Font definitions-->

`fonts`주제는 여러분이 어떤 수의 글리프 폰트라도 사용 할 수 있게 합니다. 
여러분은 이러한 폰트들을 아이콘 정의에 나중에 리퍼런스 할 수 있습니다. 만약 아이콘 정의에서 폰트 id를 명시 하지 않은 경우, 제일 처음 선언된 폰트가 기본값으로 사용됩니다. 

<!--
The `fonts` section lets you declare any number of glyph fonts that you want to use.
You can later reference these fonts in the icon definitions. The font declared first will be used as the default if an icon definition does not specify a font id.-->

폰트 파일을 익스텐션에 복사하여 해당하는 경로를 설정하십시오.
[WOFF](https://developer.mozilla.org/docs/Web/Guide/WOFF) 폰트를 사용하는 것이 권장됩니다.

<!--
Copy the font file into your extension and set the path accordingly.
It is recommended to use [WOFF](https://developer.mozilla.org/docs/Web/Guide/WOFF) fonts.-->

- 'woff'를 포맷으로 설정하십시오.
- weight 속성 값은 [여기](https://developer.mozilla.org/docs/Web/CSS/font-weight#Values)에 설명 되어 있습니다. 
- style 속성 값은 [여기](https://developer.mozilla.org/docs/Web/CSS/@font-face/font-style#Values)에 설명 되어 있습니다. 
- size는 아이콘이 사용된 폰트 사이즈에 대해 상대적인 값을 가져야 하므로 퍼센티지 값을 사용하십시오. 

<!--
- Set 'woff' as the format.
- the weight property values are defined [here](https://developer.mozilla.org/docs/Web/CSS/font-weight#Values).
- the style property values are defined [here](https://developer.mozilla.org/docs/Web/CSS/@font-face/font-style#Values).
- the size should be relative to the font size where the icon is used. Therefore, always use percentage.
-->

```json
{
  "fonts": [
    {
      "id": "turtles-font",
      "src": [
        {
          "path": "./turtles.woff",
          "format": "woff"
        }
      ],
      "weight": "normal",
      "style": "normal",
      "size": "150%"
    }
  ],
  "iconDefinitions": {
    "_file": {
      "fontCharacter": "\\E002",
      "fontColor": "#5f8b3b",
      "fontId": "turtles-font"
    }
  }
}
```

### 파일 아이콘 테마의 폴더 아이콘
<!--
### Folder icons in File Icon Themes -->

파일 아이콘 테마는 파일 탐색기에서 폴더 아이콘으로 폴더의 열림 상태를 표현하기에 충분할 경우 기본 폴더 아이콘 (회전하는 삼각형 혹은 "twisties") 을 보이지 않게 설정 합니다. 이 기능은 파일 아이콘 테마 설정 파일의 `"hidesExplorerArrows":true`를 설정하여 사용 할 수 있습니다. 

<!--
File Icon themes can instruct the File Explorer not to show the default folder icon (the rotating triangles or "twisties") when the folder icons are good enough to indicate the expansion state of a folder. This mode is enabled by setting `"hidesExplorerArrows":true` in the File Icon theme definition file.
-->
