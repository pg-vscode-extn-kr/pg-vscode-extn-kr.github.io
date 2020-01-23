---
layout: default
parent: Extension Guides
title: Task Provider
nav_order: 8
description: ""
---

# 작업 제공자

<!-- # Task Provider -->

사용자는 보통 [작업](/docs/editor/tasks)을 Visual Studio Code의 `tasks.json` 파일에 정의합니다. 한편, 어떤 작업들은 소프트웨어 개발 도중 자동적으로 VS Code 익스텐션 작업 제공자에 의해 인식됩니다. VS Code에서 **Tasks: Run Task** 커맨드가 실행될때, 모든 활성 작업 제공자는 사용자가 실행 가능한 작업을 지시합니다. `tasks.json`파일은 사용자가 특정 폴더나 작업공간에 수동으로 작업을 정의해야하지만, 작업 제공자는 작업공간에 대하여 세부정보를 감지하고 해당하는 VS Code 작업을 자동으로 생성합니다. 예를 들어, 작업 제공자는 `make`나 `Rakefile`과 같은 특정 빌드 파일이 존재 하는지를 확인하여 빌드 작업을 생성 합니다. 이번 주제에서는 익스텐션이 어떻게 작업을 자동으로 인식하고 사용자에게 제공하는지 설명합니다.

<!--
Users normally define [tasks](/docs/editor/tasks) in Visual Studio Code in a `tasks.json` file. However, there are some tasks during software development that can be automatically detected by a VS Code extension with a Task Provider. When the **Tasks: Run Task** command is run from VS Code, all active Task Providers contribute tasks that the user can run. While the `tasks.json` file lets the user manually define a task for a specific folder or workspace, a Task Provider can detect details about a workspace and then automatically create a corresponding VS Code Task. For example, a Task Provider could check if there is a specific build file, such as `make` or `Rakefile`, and create a build task. This topic describes how extensions can auto-detect and provide tasks to end-users. -->

This guide teaches you how to build a Task Provider that auto-detects tasks defined in [Rakefiles](https://ruby.github.io/rake/). The complete source code is at: https://github.com/Microsoft/vscode-extension-samples/tree/master/task-provider-sample.

## 작업 정의 
<!--
## Task Definition -->

시스템에서 작업을 특별하게 식별하기 위하여, 익스텐션의 작업 지시는 작업을 식별하는 속성을 지정해야만 합니다. Rake 예시에서 작업 정의는 다음과 같습니다 :

<!--
To uniquely identify a task in the system, an extension contributing a task needs to define the properties that identify a task. In the Rake example, the task definition looks like this: -->

```json
"taskDefinitions": [
    {
        "type": "rake",
        "required": [
            "task"
        ],
        "properties": {
            "task": {
                "type": "string",
                "description": "The Rake task to customize"
            },
            "file": {
                "type": "string",
                "description": "The Rake file that provides the task. Can be omitted."
            }
        }
    }
]
```

이는 `rake` 작업에 대하여 작업 정의를 지시합니다. 작업 정의는 `task`와 `file`의 2가지 속성을 가집니다. `task`는 Rake 작업의 이름이며 `file`은 작업을 포함하는 `Rakefile`의 위치를 가르킵니다. `task`는 필수 속성이며 `file` 속성은 옵션입니다. 만약 `file`속성이 생략 되었을 경우, 작업공간 폴더의 루트에 있는 `Rakefile`이 대신 사용 될 것입니다.  

<!--
This contributes a task definition for `rake` tasks. The task definition has two attributes `task` and `file`. `task` is the name of the Rake task and `file` points to the `Rakefile` that contains the task. The `task` property is required, the `file` property is optional. If the `file` attribute is omitted, the `Rakefile` in the root of the workspace folder is used. -->


## 작업 제공자 
<!--
## Task provider -->

코드 완성을 지원하는 언어 제공 익스텐션과 유사하게, 익스텐션은 가능한 모든 작업을 계산할 작업 제공자를 등록할 수 있습니다. 이는 다음 코드 snippet처럼 `vscode.task` 네임스페이스를 이용하여 가능합니다:

<!--
Analogous to language providers that let extensions support code completion, an extension can register a task provider to compute all available tasks. This is done using the `vscode.tasks` namespace as shown in the following code snippet: -->

```ts
import * as vscode from 'vscode';

let rakePromise: Thenable<vscode.Task[]> | undefined = undefined;
const taskProvider = vscode.tasks.registerTaskProvider('rake', {
  provideTasks: () => {
    if (!rakePromise) {
      rakePromise = getRakeTasks();
    }
    return rakePromise;
  },
  resolveTask(_task: vscode.Task): vscode.Task | undefined {
		const task = _task.definition.task;
		// A Rake task consists of a task and an optional file as specified in RakeTaskDefinition
		// Make sure that this looks like a Rake task by checking that there is a task.
		if (task) {
			// resolveTask requires that the same definition object be used.
			const definition: RakeTaskDefinition = <any>_task.definition;
			return new vscode.Task(definition, definition.task, 'rake', new vscode.ShellExecution(`rake ${definition.task}`));
		}
		return undefined;  }
});
```

`provideTask`처럼, `resolveTask` 메소드는 VS Code에 의해 호출되어 익스텐션으로 부터 작업을 받습니다. `resolveTask`는 `provideTasks` ***이후에*** 호출되며, 이는 `provideTasks`로 부터 작업을 제공 받지 않는 제공자의 퍼포먼스 향상을 위하여 의도 된 것입니다. 사용자가 개인 작업 제공자를 멈출 수 있게 설정하는 것은 좋은 예시이며, 이는 일반적입니다. 이로 인해 사용자는 특정 제공자로 부터 받은 작업이 느리다는 것을 알 때 해당 제공자를 중지 할 것입니다. 이 경우, 사용자는 여전히 `tasks.json`에 있는 공급자의 몇몇 작업을 참조 할 수 있습니다. 만약 `resolveTask`가 구현되지 않았을경우, `tasks.json`의 작업이 생성 되지 않았다는 경고가 나타날 것입니다. `resolveTask`를 통해 익스텐션은 `tasks.json`에 정의 된 작업들을 제공 할 수 있습니다. 

<!--
Like `provideTasks`, the `resolveTask` method is called by VS Code to get tasks from the extension. `resolveTask` is called ***after*** `provideTasks`, and is intended to provide an optional performance increase for providers that implement it but don't provide tasks from `provideTasks`. It is good practice to have a setting that allows users to turn off individual task providers, so this is common. A user might notice that tasks from a specific provider are slower to get and turn off the provider. In this case, the user might still reference some of the tasks from this provider in their `tasks.json`. If `resolveTask` is not implemented, then there will be a warning that the task in their `tasks.json` was not created. With `resolveTask` an extension can still provide a task for the task defined in `tasks.json`. -->

`getRakeTasks`구현은 다음을 행합니다:

<!-- The `getRakeTasks` implementation does the following: -->

- `rake -AT -f Rakefile` 커맨드를 사용하여 `Rakefile`에 정의된 모든 rake 작업을 목록으로 만듬.
- stdio 출력을 파싱
- 모든 작업 목록에 대하여, `vscode.Task`구현을 생성.

<!--
- Lists all rake tasks defined in a `Rakefile` using the `rake -AT -f Rakefile` command.
- Parses the stdio output.
- For every listed task, creates a `vscode.Task` implementation. -->

Rake 작업 인스턴스화에는 `package.json`파일에 정의된 작업정의가 필요하기 때문에, VS Code는 아래와 같은 타입스크립트 인터페이스를 사용하여 구조를 정의 할 것입니다: 

<!--
Since a Rake task instantiation needs a task definition as defined in the `package.json` file, VS Code also defines the structure using a TypeScript interface like this: -->

```typescript
interface RakeTaskDefinition extends vscode.TaskDefinition {
  /**
   * The task name
   */
  task: string;

  /**
   * The rake file containing the task
   */
  file?: string;
}
```

출력이 `compile`이라는 작업에서 나온다고 할 때, 해당 작업 생성은 다음과 같습니다:
<!--
Assuming that the output comes from a task called `compile`, the corresponding task creation then looks like this: -->

```typescript
let task = new vscode.Task(
  { type: 'rake', task: 'compile' },
  'compile',
  'rake',
  new vscode.ShellExecution('rake compile')
);
```

출력 안의 모든 작업 목록에 대하여, 해당 VS Code 작업이 위의 패턴을 사용하여 생성되고 `getRakeTasks` 호출로 인한 모든 작업의 배열이 반환됩니다.

<!--
For every task listed in the output, a corresponding VS Code task is created using the above pattern and then returns the array of all tasks from the `getRakeTasks` call. -->

`ShellExecution`은 각 OS에 해당하는 쉘의 `rake compile` 커맨드를 실행합니다 (예를 들어 Windows의 경우는 PowerShell에서, Ubuntu는 bash에서 실행됩니다). 만약 작업이 프로세스를 직접 실행해야 한다면 ( 쉘을 생성하지 않고 ), `vscode.ProcessExecution`이 사용 될 수 있습니다. `ProcessExecution`은 익스텐션이 프로세스에 전달된 변수를 완전히 제어 할 수 있다는 장점이 있습니다. `ShellExecution`을 사용하는것은 쉘 커맨드 인터프리테이션을 (bash에서의 wildcard 확장같은) 사용 할 수 있게 합니다. 만약 `ShellExecution`이 하나의 커맨드 라인에서 생성되었다면, 익스텐션은 적절한 처리가 된 커맨드를 필요로 합니다.(예를 들면 공백을 다루거나 따옴표 같은) 

<!-- 
The `ShellExecution` executes the `rake compile` command in the shell that is specific for the OS (for example under Windows the command would be executed in PowerShell, under Ubuntu it'd be executed in bash). If the task should directly execute a process (without spawning a shell), `vscode.ProcessExecution` can be used. `ProcessExecution` has the advantage that the extension has full control over the arguments passed to the process. Using `ShellExecution` makes use of the shell command interpretation (like wildcard expansion under bash). If the `ShellExecution` is created with a single command line, then the extension needs to ensure proper quoting and escaping (for example to handle whitespace) inside the command.-->

## 커스텀실행

<!--
## CustomExecution -->

일반적인 경우, 단순한 `ShellExecution`이나 `ProcessExecution`을 사용하는것이 좋습니다. 하지만, 만약 여러분의 작업이 실행 중에 있어 많은 저장 상태를 요구하거나, 분리된 스크립트나 프로세스로서 작동하지 않거나, 많은 양의 출력을 처리해야 한다면 `CustomExecution`을 사용해야 합니다. `CustomExecution`은 보통 복잡한 빌드 시스템에 쓰입니다. 이는 작업이 할 수 있는 것에 굉장한 유연성을 허용하지만 동시에, 작업 제공자가 어떤 프로세스 관리와, 출력 파싱에도 책임이 있다는 것을 의미합니다. 또한 작업 제공자는 `Pseudoterminal` 을 구현해야하고 이를 `CustomExecution`콜백으로부터 리턴해야합니다. 

<!--
In general, it is best to use a `ShellExecution` or `ProcessExecution` because they are simple. However, if your task requires a lot of saved state between runs, doesn't work well as a separate script or process, or requires extensive handling of output a `CustomExecution` might be a good fit. Existing uses of `CustomExecution` are usually for complex build systems. A `CustomExecution` has only a callback which is executed at the time that the task is run. This allows for greater flexibility in what the task can do, but it also means that the task provider is responsible for any process management and output parsing that needs to happen. The task provider is also responsible for implementing `Pseudoterminal` and returning it from the `CustomExecution` callback. -->

```typescript
return new vscode.Task(definition, vscode.TaskScope.Workspace, `${flavor} ${flags.join(' ')}`,
  CustomBuildTaskProvider.CustomBuildScriptType, new vscode.CustomExecution(async (): Promise<vscode.Pseudoterminal> => {
    // When the task is executed, this callback will run. Here, we setup for running the task.
    return new CustomBuildTaskTerminal(this.workspaceRoot, flavor, flags, () => this.sharedState, (state: string) => this.sharedState = state);
  }));
```

`Pseudoterminal`의 구현을 포함한 완전한 예시는 https://github.com/Microsoft/vscode-extension-samples/tree/master/task-provider-sample/src/customTaskProvider.ts 에 있습니다. 

<!--
The full example, including the implementation of `Pseudoterminal` is at https://github.com/Microsoft/vscode-extension-samples/tree/master/task-provider-sample/src/customTaskProvider.ts -->
