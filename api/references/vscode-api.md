---
layout: default
parent: References
title: VS Code API 
nav_order: 1
description: ""
---

# VS Code API

**VS Code API** 는 Visual Studio Code 익스텐션에서 호출 할 수있는 자바스크립트 API 세트 입니다. 이 페이지에서는, 익스텐션 저자가 사용 할 수 있는 모든 VS Code API의 목록이 나열되어 있습니다.

<!--
**VS Code API** is a set of JavaScript APIs that you can invoke in your Visual Studio Code extension. This page lists all VS Code APIs available to extension authors. -->

## API 네임스페이스 및 클래스

## API namespaces and classes

아래의 목록은 VS Code 저장소의 [`vscode.d.ts`](https://github.com/Microsoft/vscode/blob/master/src/vs/vscode.d.ts)파일에서 컴파일 된 것입니다. 

<!--
This listing is compiled from the [`vscode.d.ts`](https://github.com/Microsoft/vscode/blob/master/src/vs/vscode.d.ts) file from the VS Code repository. -->

{$ Content $}

## API 패턴

<!--
## API Patterns
-->

다음은 VS Code API에서 공통으로 쓰이는 패턴들의 일부입니다.

<!--
These are some of the common patterns we use in the VS Code API.-->

### 프로미스
<!--
### Promises -->

VS Code API는 [프로미스](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)를 사용한 비동기 작업을 나타냅니다. 익스텐션에서 __any__ 타입의 프로미스는 ES6, WinJS, A+ 등과 같이 반환 될 수 있습니다. 

<!--
The VS Code API represents asynchronous operations with [promises](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise). From extensions, __any__ type of promise can be returned, like ES6, WinJS, A+, etc.-->

특정 프로미스 라이브러리에 독립적인것은 API에서 `Thenable`-type으로 표현 됩니다. `Thenable`은 [then](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)한 메소드인 공통의 분모를 표기합니다. 

<!--
Being independent of a specific promise library is expressed in the API by the `Thenable`-type. `Thenable` represents the common denominator which is the [then](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) method. -->

대부분의 경우 프로미스 사용은 선택 사항이며 VS Code가 익스텐션을 호출하면 _result type_ 과 _result type_ 의 `Thenable`을 처리 할 수 있습니다. 프로미스 사용이 선택적인 경우, API는 `or`-type 을 반환하여 이를 나타냅니다. 

<!--
In most cases the use of promises is optional and when VS Code calls into an extension, it can handle the _result type_ as well as a `Thenable` of the _result type_. When the use of a promise is optional, the API indicates this by returning `or`-types.-->

```typescript
provideNumber(): number | Thenable<number>
```

### 취소 토큰
<!--
### Cancellation Tokens-->

때때로 작업이 완료 되지 않아, 상태가 변하는 휘발성 상태에서 작업이 시작됩니다. 예를 들어, IntelliSense 시작을 계산한 다음, 사용자는 계속하여 입력하여 해당 작업의 결과를 쓸모 없게 만듭니다.

<!--
Often operations are started on volatile state which changes before operations can finish. For instance, computing IntelliSense starts and the user continues to type making the result of that operation obsolete.-->

이같은 동작에 노출 된 API는 취소를 확인하거나 (`isCancellationRequested`), 취소가 발생 했을때 (`onCancellationRequested`) 알림을 받을 수 있는 `CancellationToken`을 전달 받습니다.  취소 토큰은 일반적으로 함수 호출의 선택사항인 마지막 매개변수입니다. 

<!--
APIs that are exposed to such behavior will get passed a `CancellationToken` on which you can check for cancellation (`isCancellationRequested`) or get notified when cancellation occurs (`onCancellationRequested`). The cancellation token is usually the last parameter of a function call and optional.-->

### 일회성
<!--
### Disposables-->

VS Code API는 VS Code에서 얻은 리소스에 [일회성 패턴](https://en.wikipedia.org/wiki/Dispose_pattern)을 사용합니다. 이는 이벤트 리스닝, 커맨드, UI와 상호작용 그리고 다양한 언어 기여에 사용 됩니다. 

<!--
The VS Code API uses the [dispose pattern](https://en.wikipedia.org/wiki/Dispose_pattern) for resources that are obtained from VS Code. This applies to event listening, commands, interacting with the UI, and various language contributions.-->

예를 들어, `setStatusBarMessage(value: string)`함수는 `dispose`를 호출 하면 메세지를 다시 제거하는 `Disposable`을 반환 합니다. 

<!--
For instance, the `setStatusBarMessage(value: string)` function returns a `Disposable` which upon calling `dispose` removes the message again. -->

### 이벤트
<!--
### Events -->

VS Code API의 이벤트는 구독을 위해 리스너 함수로 호출하는 함수로 반환 됩니다. 구독 호출은 이벤트 리스너를 제거 하는 `Disposable`을 반환합니다.

<!--
Events in the VS Code API are exposed as functions which you call with a listener-function to subscribe. Calls to subscribe return a `Disposable` which removes the event listener upon dispose. -->

```javascript
var listener = function(event) {
    console.log("It happened", event);
};

// start listening
var subscription = fsWatcher.onDidDelete(listener);

// do more stuff

subscription.dispose(); // stop listening
```

이벤트의 이름은 `on[Will|Did]VerbNoun?` 패턴을 따릅니다. 이 이름은 이벤트가 일어날 것인지 *(onWill)* 혹은 이미 일어 났는지 *(onDid)*, 어떤 일이 일어났는지 *(verb)*, 그리고 컨텍스트가 명확하지 않은 이상 컨텍스트*(noun)*를 나타냅니다.

<!--
Names of events follow the `on[Will|Did]VerbNoun?` pattern. The name signals if the event is going to happen *(onWill)* or already happened *(onDid)*, what happened *(verb)*, and the context *(noun)* unless obvious from the context.-->

VS Code API의 예시로, `window.onDidChangeActiveTextEditor`는 활성화된 텍스트 에디터가 *(noun)*, 이미 *(onDid)* 변경되었을때 *(verb)* 발생 하는 이벤트 입니다. 
<!--
An example from the VS Code API is `window.onDidChangeActiveTextEditor` which is an event fired when the active text editor *(noun)* has been (*onDid*) changed (*verb*).-->

### 엄격한 null
<!--
### Strict null-->

VS Code API는 [엄격한 null 검사](https://github.com/Microsoft/TypeScript/pull/7140)를 지원하기 때문에, `undefined`와 `null` 타입스크립트 유형을 사용합니다.

<!--
The VS Code API uses the `undefined` and `null` TypeScript types where appropriate to support [strict null checking](https://github.com/Microsoft/TypeScript/pull/7140).--?
