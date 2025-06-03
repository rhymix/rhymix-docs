FrontEndFileHandler
-------------------

HTML 응답에서 사용하는 CSS 및 JavaScript 애셋을 관리하는 클래스입니다.
라이믹스는 각각의 템플릿에서 애셋 링크를 직접 출력하지 않고 `FrontEndFileHandler` 클래스를 사용하여 관리하도록 함으로써
아래와 같은 장점을 얻습니다.

- 중복 로딩을 방지합니다.
- 코어, 레이아웃, 스킨, 위젯 등에서 로딩한 애셋의 로딩 순서를 표준화합니다.
- 스크립트 자동 압축, 합치기 등의 정책을 일괄 적용할 수 있습니다.
- 다른 자료가 로딩한 애셋을 제거하거나 대체하는 등, 다양한 부가기능의 개발을 지원합니다.

`FrontEndFileHandler` 클래스는 싱글턴으로 구현되어 있으며,
`getInstance()` 메서드를 통해 인스턴스를 가져올 수 있으나
일반적으로 인스턴스를 직접 다루기보다는 `Context` 클래스를 통해 상호작용하게 됩니다.

#### getInstance()

```
public static function getInstance(): self
```

싱글턴 인스턴스를 반환합니다.

#### loadFile()

```
public function loadFile(array $args): void
```

CSS (LESS, SCSS 포함), JS 애셋을 로딩합니다.

`$args` 매개변수는 파일의 종류에 따라 다르게 사용됩니다. 파일 확장자를 통해 CSS와 JS를 구분합니다.

CSS 파일의 경우:

```
$args[0]: 로딩할 파일의 상대경로 (내부 파일인 경우) 또는 URL (외부 리소스인 경우)
$args[1]: media
$args[2]: 출처 정보 ('layout', 'skin', 'widget' 등)
$args[3]: 정렬 순서
$args[4]: LESS나 SCSS에서 사용할 변수의 배열
```

JS 파일의 경우:

```
$args[0]: 로딩할 파일의 상대경로 (내부 파일인 경우) 또는 URL (외부 리소스인 경우)
$args[1]: 실행할 위치('head' 또는 'body') 또는 type=module인 경우 `module'
$args[2]: 사용하지 않음
$args[3]: 정렬 순서
```

#### unloadFile()

```
public function unloadFile(
    string $fileName,
    string $unused = '',
    string $media = 'all'
): void
```

주어진 값과 일치하는 속성을 가진 애셋을 삭제합니다.

#### unloadAllFiles()

```
public function unloadAllFiles(string $type = 'all'): void
```

지금까지 로딩된 모든 애셋을 삭제합니다.

`$type` 매개변수는 `'css'`, `'js'`, `'all'` 중 하나를 선택할 수 있으며, 기본값은 `'all'`입니다.

#### getCssFileList()

```
public function getCssFileList(bool $finalize = false): array
```

로딩된 CSS 파일 목록을 반환합니다.

`$finalize`가 참인 경우 압축, 합치기 등의 최종 처리를 거친 후 압축된 목록을 반환합니다.
그렇지 않은 경우, 로딩된 CSS 파일 목록을 그대로 반환합니다.

#### getJsFileList()

```
public function getJsFileList(
    string $type = 'head',
    bool $finalize = false
): array
```

로딩된 JS 파일 목록을 반환합니다.

`$type` 매개변수는 `'head'`, `'body'` 중 하나를 선택할 수 있으며, 기본값은 `'head'`입니다.

`$finalize`가 참인 경우 압축, 합치기 등의 최종 처리를 거친 후 압축된 목록을 반환합니다.
그렇지 않은 경우, 로딩된 JS 파일 목록을 그대로 반환합니다.

#### startLog()

```
public function startLog(): void
```

로깅을 시작하고, 로그를 초기화합니다.

#### endLog()

```
public function endLog(): array
```

로깅을 종료하고, 그 동안 기록된 로그를 반환합니다.

#### isSsl()

```
public static function isSsl(): bool
```

`RX_SSL` 상수의 값을 반환합니다. 하위 호환을 위해 존재하는 메소드입니다.
