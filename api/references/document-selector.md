---
layout: default
parent: References
title: Document Selectors
nav_order: 8
description: ""
---

# 문서 선택기
<!--
# Document Selectors-->
익스텐션은 기능을 언어, 파일 형식 및 위치별로 문서 선택기에 기반하여 필터링 할 수 있습니다. 이 주제에서는 문서 선택기, 문서 체계, 그리고 익스텐션 작성자가 알고 있어야 할 것에 대해 설명합니다.
<!--
Extensions can filter their features based on document selectors by language, file type, and location. This topic discusses document selectors, document schemes, and what extensions authors should be aware about.-->
## 디스크에 없는 텍스트 문서
<!--
## Text documents not on disk-->
모든 텍스트 문서들이 디스크에 저장되어 있지 않습니다. 예: 새로 작성된 문서. 문서의 형식이 지정되지 않으면 문서 선택기는 **모든** 형식에 적용됩니다. [DocumentFilter](/api/references/vscode-api#DocumentFilter) `scheme` 속성을 사용하여 특정 형식으로 좁힐 수 있습니다. 예를 들면, 디스크에 저장된 TypeScript 파일의 경우 `{ scheme: 'file', language: 'typescript' }` 입니다.
<!--
Not all text documents are stored on disk, for example, newly created documents. Unless specified, a document selector applies to **all** document types. Use the [DocumentFilter](/api/references/vscode-api#DocumentFilter) `scheme` property to narrow down on certain schemes, for example `{ scheme: 'file', language: 'typescript' }` for TypeScript files that are stored on disk.-->

## 문서 선택기
<!--
# Document selectors-->
Visual Studio Code 익스텐션 API는 IntelliSense와 같은 언어별 기능을 [DocumentSelector](/api/references/vscode-api#DocumentSelector) 형식을 통해 결합합니다. 기능을 특정 언어로 좁히는 쉬운 메커니즘입니다.
<!--
The Visual Studio Code extension API combines language-specific features, like IntelliSense, with document selectors through the [DocumentSelector](/api/references/vscode-api#DocumentSelector) type. They are an easy mechanism to narrow down functionality to a specific language.-->

아래 Snippet은 TypeScript 파일에 [HoverProvider](/api/references/vscode-api#HoverProvider)를 등록하며 문서 선택기는 `typescript` 언어 식별자 문자열입니다.
<!--
The snippet below registers a [HoverProvider](/api/references/vscode-api#HoverProvider) for TypeScript files and the document selector is the `typescript` language identifier string.-->

```ts
vscode.languages.registerHoverProvider('typescript', {
  provideHover(doc: vscode.TextDocument) {
    return new vscode.Hover('For *all* TypeScript documents.');
  }
});
```
문서 선택기는 단순한 언어 식별자 이상일 수 있으며 더 복잡한 선택기는 [DocumentFilter](/api/references/vscode-api#DocumentFilter)를 사용하여 `scheme`과 `pattern` 경로 글로브 패턴을 통한 파일 위치를 기반으로 필터링할 수 있습니다.
<!--
A document selector can be more than just a language identifier and more complex selectors can use a [DocumentFilter](/api/references/vscode-api#DocumentFilter) to filter based on the `scheme` and file location through a `pattern` path glob-pattern:-->

```ts
vscode.languages.registerHoverProvider(
  { pattern: '**/test/**' },
  {
    provideHover(doc: vscode.TextDocument) {
      return new vscode.Hover('For documents inside `test`-folders only');
    }
  }
);
```
다음 스니펫은 `scheme`필터를 사용하여 언어 식별자와 결합합니다. `untitled` 체계는 아직 디스크에 저장되지 않은 새 파일을 위한 것입니다.
<!--
The next snippet uses the `scheme` filter and combines it with a language identifier. The `untitled` scheme is for new files that have not yet been saved to disk.-->

```ts
vscode.languages.registerHoverProvider(
  { scheme: 'untitled', language: 'typescript' },
  {
    provideHover(doc: vscode.TextDocument) {
      return new vscode.Hover('For new, unsaved TypeScript documents only');
    }
  }
);
```

## 문서 체계
<!--
## Document scheme-->
문서의 체계는 종종 간과되지만 중요한 정보입니다. 대부분의 문서는 디스크에 저장되며 익스텐션 작성자는 일반적으로 디스크의 파일로 작업한다고 가정합니다. 간단한 `typescript` 선택기로 예를 들면, 가정은 **디스크에 있는 Typescript 파일**이 됩니다. 하지만 그 가정이 너무 느슨하고 `{ scheme: 'file', language: 'typescript' }` 와 같은 명확한 선택기가 사용되어야 할 시나리오가 있습니다.
<!--
The `scheme` of a document is often overlooked but is an important piece of information. Most documents are saved on disk and extension authors typically assume they are working with a file on disk. For example with a simple `typescript` selector, the assumption is **TypeScript files on disk**. However, there are scenarios where that assumption is too lax and a more explicit selector like `{ scheme: 'file', language: 'typescript' }` should be used.-->

기능의 중요성은 기능이 디스크에서 파일을 읽거나 쓰는 데 의존할 때 나타납니다. 아래의 snippet을 확인하십시오:
<!--
The importance of this comes into play when features rely on reading/writing files from/to disk. Check out the snippet below:-->

```ts
// 👎 too lax
vscode.languages.registerHoverProvider('typescript', {
  provideHover(doc: vscode.TextDocument) {
    const { size } = fs.statSync(doc.uri.fsPath); // ⚠️ what about 'untitled:/Untitled1.ts' or others?
    return new vscode.Hover(`Size in bytes is ${size}`);
  }
});
```
위의 HoverProvider는 디스크에 있는 문서의 크기를 표시하려고 하지만 문서가 실제로 디스크에 저장되어 있는지 확인하는데 실패합니다. 예를 들어, 문서가 새로 작성하여 아직 저장되지 않았을 수 있습니다. 올바른 방법은 제공자가 디스크의 파일로만 작업할 수 있음을 VS Code에 알리는 것입니다.

<!--
The hover provider above wants to display the size of a document on disk but it fails to check whether the document is actually stored on disk. For example, it could be newly created and not yet saved. The correct way would be to tell VS Code that the provider can only work with files on disk.-->

```ts
// 👍 only works with files on disk
vscode.languages.registerHoverProvider(
  { scheme: 'file', language: 'typescript' },
  {
    provideHover(doc: vscode.TextDocument) {
      const { size } = fs.statSync(doc.uri.fsPath);
      return new vscode.Hover(`Size in bytes is ${size}`);
    }
  }
);
```

## 요약
<!--
## Summary-->
문서는 일반적으로 파일 시스템에 저장되지만 항상 그런 것은 아닙니다. 제목없는 문서, Git이 사용하는 캐시된 문서, FTP와 같은 원격 소스의 문서 등이 있습니다. 기능이 디스크 액세스에 의존하는 경우, `file` 체계와 함께 문서 선택기를 사용해야합니다.
<!--
Documents are usually stored on the file system, but not always: there are untitled documents, cached documents that Git uses, documents from remote sources like FTP, and so forth. If your feature relies on disk access, make sure to use a document selector with the `file` scheme.-->

## 다음 단계들
<!--
## Next steps-->
VS 코드 확장성 모델에 대한 자세한 내용은 다음 항목을 읽어보십시오.
<!--
To learn more about VS Code extensibility model, try these topic:-->
- [Extension Manifest File](/api/references/extension-manifest) - VS Code package.json 익스텐션 매니페스트 파일 참조
- [Contribution Points](/api/references/contribution-points) - VS Code 기여 포인트 참조
<!--
- [Extension Manifest File](/api/references/extension-manifest) - VS Code package.json extension manifest file reference
- [Contribution Points](/api/references/contribution-points) - VS Code contribution points reference-->
