ModuleObject
------------

`ModuleObject` 클래스는 모듈의 기본적인 동작을 정의하는 클래스입니다.
이 클래스는 모듈의 실행, 권한 검사, 템플릿 설정 등을 처리합니다.

사용자 요청을 처리하는 모든 모듈 클래스는 `ModuleObject` 클래스를 상속받아야 합니다.

`ModuleObject` 클래스는 `BaseObject` 클래스를 상속받으므로
`BaseObject`에서 제공하는 `setError()`, `setMessage()`, `set()` 등의 메소드도 사용할 수 있습니다.

[BaseObject 클래스 정보 보기](./BaseObject.md)



### 인스턴스 생성

#### __construct()

```
public function __construct(
    int $error = 0,
    string $message = 'success'
)
```

생성자는 `BaseObject`의 그것과 같습니다.
일반적으로 직접 호출할 필요는 없으며, `getInstance()` 메소드를 통해 인스턴스를 생성합니다.

#### getInstance()

```
public static function getInstance(?string $module_hint = null): static
```

모듈 클래스의 싱글턴 인스턴스를 생성합니다.
네임스페이스에 들어 있는 모듈 클래스와 그렇지 않은 모듈 클래스 모두 사용할 수 있으며,
모듈 클래스의 인스턴스를 생성하는 가장 정석적인 방법입니다.



### 응답 정보 저장

`ModuleObject` 클래스는 HTML, JSON, 리다이렉트 등 다양한 응답 정보를 선언하는 메소드를 제공합니다.
필요에 따라 사용하면 됩니다.

#### setRedirectUrl()

```
public function setRedirectUrl(
    string $url = './',
    mixed $output = null
): object
```

현재 요청 완료 후 리다이렉트할 URL을 지정합니다.

```
[예시]
$this->setHttpStatusCode(301);
$this->setRedirectUrl('https://example.com');
```

반환값은 `$output`에 오브젝트를 전달한 경우 해당 오브젝트를 반환하고,
그렇지 않으면 `$this`를 반환하여 method chaining을 지원합니다.

#### getRedirectUrl()

```
public function getRedirectUrl(): ?string
```

현재 설정된 리다이렉트 URL을 반환합니다.
설정된 URL이 없으면 `null`을 반환합니다.

#### setTemplateFile()

```
public function setTemplateFile(string $filename): self
```

HTML 응답시 사용할 템플릿 파일명을 설정합니다.
`.html`이나 `.blade.php` 확장자를 포함하여 설정하면 확장자가 일치하는 템플릿 파일만 사용하고,
확장자를 생략하면 `.html` 또는 `.blade.php` 확장자를 가진 템플릿 파일을 자동으로 찾습니다.

#### getTemplateFile()

```
public function getTemplateFile(): ?string
```

현재 설정된 템플릿 파일명을 반환합니다.

#### setTemplatePath()

```
public function setTemplatePath(string $path): self
```

HTML 응답시 사용할 템플릿 디렉토리 경로를 설정합니다.

#### getTemplatePath()

```
public function getTemplatePath(): ?string
```

현재 설정된 템플릿 디렉토리 경로를 반환합니다.

#### setEditedLayoutFile()

```
public function setEditedLayoutFile(string $filename): self
```

HTML 응답시 사용할 수정된 레이아웃 파일명을 설정합니다.
이 메소드는 레이아웃 파일을 수정한 경우에 사용됩니다.

#### getEditedLayoutFile()

```
public function getEditedLayoutFile(): ?string
```

현재 설정된 수정된 레이아웃 파일명을 반환합니다.

#### setLayoutFile()

```
public function setLayoutFile(string $filename): self
```

HTML 응답시 사용할 레이아웃 파일명을 설정합니다.

#### getLayoutFile()

```
public function getLayoutFile(): ?string
```

현재 설정된 레이아웃 파일명을 반환합니다.

#### setLayoutPath()

```
public function setLayoutPath(string $path): self
```

HTML 응답시 사용할 레이아웃 경로를 설정합니다.

#### getLayoutPath()

```
public function getLayoutPath(): ?string
```

현재 설정된 레이아웃 파일명을 반환합니다.

#### setLayoutAndTemplatePaths()

```
public function setLayoutAndTemplatePaths(
    string $type,
    object $config
): void
```

HTML 응답시 사용할 레이아웃과 템플릿 경로, 파일명 등을 `$config`를 기준으로 한 번에 설정합니다.

`$type`은 'P' 또는 'M'으로, PC용 설정을 사용할지 모바일용 설정을 사용할지 지정합니다.

`$config`는 모듈 설정 정보를 담고 있는 객체로, `layout_srl`, `skin`, `mskin` 등의 속성을 가지고 있습니다.
이 값들을 바탕으로 기본값 처리, 반응형(모바일에서 PC 레이아웃 사용 등) 설정 등을 자동으로 처리합니다.

#### copyResponseFrom()

```
public function copyResponseFrom(self $instance): void
```

다른 모듈 인스턴스의 응답을 현재 인스턴스로 복사합니다.
다른 모듈을 실행한 후 그 결과를 현재 모듈의 응답으로 사용하고자 할 때 유용합니다.



### 내부 함수

아래의 메소드들은 역사적인 이유로 `public`으로 선언되어 있지만,
일반적으로 직접 호출할 필요는 없으며
추후 `protected`로 변경되거나, 이름과 호출 방법이 변경될 수도 있으므로
신규 모듈 개발 시에는 사용하지 않는 것이 좋습니다.

#### setModule()

```
public function setModule(string $module): self
```

모듈 인스턴스 생성 과정에서 호출되는 내부 함수입니다.
모듈명을 인스턴스 내에 저장하는 역할을 합니다.

#### setModulePath()

```
public function setModulePath(string $path): self
```

모듈 인스턴스 생성 과정에서 호출되는 내부 함수입니다.
모듈 경로를 인스턴스 내에 저장하는 역할을 합니다.

#### setModuleInfo()

```
public function setModuleInfo(
    object $module_info,
    object $xml_info
): self
```

모듈 인스턴스 생성 과정에서 호출되는 내부 함수입니다.
모듈 설정과 모듈 XML 정보를 인스턴스 내에 저장하는 역할을 합니다.

#### setAct()

```
public function setAct($act): self
```

모듈 인스턴스 생성 과정에서 호출되는 내부 함수입니다.
`act`를 인스턴스 내에 저장하는 역할을 합니다.

#### setPrivileges(): bool

```
public function setPrivileges()
```

모듈 인스턴스 생성 과정에서 호출되는 내부 함수입니다.
현재 사용자가 이 모듈에 대하여 갖는 권한 정보를 인스턴스 내에 저장하고,
권한이 없는 경우 해당 에러 메시지를 설정합니다.

#### checkPermission()

```
public function checkPermission(
    ?object $grant = null,
    ?object $member_info = null,
    string|array &$failed_requirement = ''
): bool
```

`setPrivileges()` 메소드에서 호출되는 내부 함수입니다.
권한이 없는 경우, 어떤 권한이 필요한지 `$failed_requirement`에 저장합니다.
이를 바탕으로 "로그인이 필요합니다" 또는 "레벨 5 이상이어야 합니다"와 같이
사용자에게 알맞은 에러 메시지를 표시할 수 있습니다.

#### proc()

```
public function proc()
```

설정된 `act`에 해당하는 메소드를 호출하고, 그 결과에 따라 에러 코드와 메시지 등을 설정합니다.
이 메소드는 모듈의 액션을 실행하는 핵심 메소드입니다.

> [애드온 실행 시점]
> 이 메소드는 `before_module_proc` 및 `after_module_proc` 애드온 실행 시점을 포함합니다.



### Deprecated

아래의 메소드들은 하위 호환을 위해 존재하나, 더이상 사용을 권장하지 않습니다.

#### setRefreshPage()

```
public function setRefreshPage(): self
```

화면을 새로고침하도록 하는 응답을 설정합니다.

#### stop()

```
public function stop(
    string $msg_code,
    int $error_code = -1
): self
```

에러 발생시 모듈 실행을 중단하고 에러 코드와 메시지를 설정하며,
에러 화면을 표시할 수 있도록 `message` 모듈과 연동하여 템플릿 경로를 조정하는 등,
일반적인 에러 처리에 필요한 작업을 일괄 처리하는 메소드입니다.

이러한 일괄 처리는 편리한 점도 있지만, 상황에 맞는 커스터마이징이 어렵다는 단점이 있습니다.
따라서 권한이 없는 액션을 실행하려고 하는 등, 사용자를 불친절하게 응대해도 무방한 경우에 주로 사용합니다.
그러나 한 번 `stop()` 메소드를 호출하면 이후에 다른 응답을 설정할 수 없으므로,
확장성 면에서 한계가 명백하여 이제는 사용을 권장하지 않습니다.

에러 코드와 메시지의 순서가 생성자 및 `setError()` 메소드와 반대이므로 주의해야 합니다.
