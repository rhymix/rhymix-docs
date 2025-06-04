ModuleHandler
-------------

이 클래스가 제공하는 대부분의 `public` 메소드는 요청 라이프사이클 과정에서 자동으로 호출되므로,
사용자가 직접 호출할 필요는 없습니다.

트리거를 실행할 때 `triggerCall()` 메소드를 사용하는 것이
서드파티 자료(애드온, 모듈 등)에서 `ModuleHandler`와 상호작용하는 가장 일반적인 방법입니다.

#### __construct()

```
public function __construct(
    string $module = '',
    string $act = '',
    string $mid = '',
    string $document_srl = '',
    string $module_srl = ''
)
```

모듈 핸들러를 초기화합니다.
비어 있는 값은 이후 다른 메소드에서 채워집니다.

> [애드온 실행 시점]
> 이 메소드는 `before_module_init` 애드온 실행 시점을 포함합니다.

#### init()

```
public function init(): bool
```

모듈 초기화

#### procModule()

```
public function procModule(): ModuleObject
```

모듈 실행

#### displayContent()

```
public function displayContent(?ModuleObject $oModule = null): void
```

모듈 실행 결과 출력

#### procCommandLineArguments()

```
public static function procCommandLineArguments(array $args): void
```

PHP-CLI에서 라이믹스를 실행한 경우, 요청한 스크립트를 찾아서 실행합니다.
이 메소드는 CLI 모드에서만 사용되며, 웹 요청에서는 사용되지 않습니다.

`modules/foo/scripts/bar.php` 경로에 스크립트를 작성했다면
터미널에서 `php index.php foo.bar` 명령으로 실행할 수 있습니다.

#### getModulePath()

```
public static function getModulePath(string $module): string
```

주어진 모듈의 설치 경로를 반환합니다.
모듈 경로는 `./modules/모듈명/'`의 형태로 작성되며, 실제 존재 여부를 확인하지 않습니다.
라이믹스에서 내부적으로 사용하는 대부분의 경로 변수와 마찬가지로, `./`로 시작하고 `/`로 끝납니다.

#### getModuleInstance()

```
public static function getModuleInstance(
    string $module,
    string $type = 'view',
    string $kind = ''
): ?ModuleObject
```

모듈 클래스의 인스턴스를 생성하여 반환합니다.
`getController()`, `getModel()` 등의 레거시 전역 함수를 호출했을 때,
공통적인 처리는 이 메소드가 담당합니다.

#### triggerCall()

```
public static function triggerCall(
    string $trigger_name,
    string $called_position,
    mixed &$obj
): BaseObject
```

트리거를 호출합니다.
다른 용어로, 이벤트를 발생시키고 그 이벤트에 연결된 리스너들을 실행합니다.

`$trigger_name`은 이벤트 이름으로, 임의의 문자열을 사용할 수 있으나
일반적으로 `모듈명.메소드명`의 형태를 사용합니다.

`$called_position`은 일반적으로 `before` 또는 `after`입니다.

`$obj`는 이벤트에 전달할 데이터로,
대개 오브젝트를 전달하지만 `display` 등 일부 이벤트에서는 문자열을 사용하기도 합니다.
이 경우에도 오브젝트와 마찬가지로 참조로 전달되므로, 전달된 문자열을 수정하면 원본도 수정됩니다.
