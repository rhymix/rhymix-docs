BaseObject
----------

#### __construct()

```
public function __construct(
    int $error = 0,
    string $message = 'success'
)
```

생성자를 사용하여 에러 코드와 메시지를 생성할 수 있습니다.

```
$obj = new BaseObject(403, 'msg_error_message');
```

#### setError()

```
public function setError(int $error = 0): self
```

에러 코드를 설정합니다. 기본값은 0(에러 없음)입니다. 관례상 0이 아닌 코드는 모두 에러라고 간주합니다.

아래와 같이 에러 코드, 에러 메시지, 에러 메시지에 삽입할 변수들을 한 번에 설정할 수도 있습니다.
에러 메시지에 삽입할 변수란, 에러 메시지에 `%s`와 같은 포맷 문자열이 있을 때, 그 자리에 들어갈 값을 의미합니다.

```
public function setError(
    int $error = 0,
    string $message,
    ...$args
): self
```

이것을 이용하면 모듈의 컨트롤러 액션에서 에러 코드, 에러 메시지 등을 한 번에 설정하여 반환하는 것이 가능합니다.

```
return $this->setError(403, 'msg_error_message', $var1, $var2);
```

이 메소드는 `$this`를 반환하므로 아래와 같이 method chaining을 사용할 수도 있습니다.

```
return $this->setError(403)->setMessage('msg_error_message')->setMessageType('error');
```

#### getError()

```
public function getError(): int
```

설정된 에러 코드를 반환합니다. 기본값은 0(에러 없음)입니다.

#### setHttpStatusCode()

```
public function setHttpStatusCode(int $code = 200): self
```

HTTP 상태 코드를 설정합니다.
이 메소드는 `$this`를 반환하므로 method chaining이 가능합니다.

#### getHttpStatusCode()

```
public function getHttpStatusCode(): int
```

설정된 HTTP 상태 코드를 반환합니다. 기본값은 200입니다.

#### setMessage()

```
public function setMessage(
    string $message = 'success',
    ?string $type = null
): self
```

메시지를 설정합니다. 필요시 메시지 타입까지 한 번에 설정할 수도 있습니다.
메시지는 기본적으로 다국어 코드를 지정한다고 가정하지만, 일반 문자열을 사용할 수도 있습니다.
이 메소드는 `$this`를 반환하므로 method chaining이 가능합니다.

#### getMessage()

```
public function getMessage(): string
```

#### setMessageType()

```
public function setMessageType(string $type): self
```

메시지 타입을 설정할 수 있습니다. 사용할 수 있는 타입은 `'error'`, `'info'`, `'update'`입니다.
라이믹스에서는 실질적으로 거의 사용되지 않는 기능입니다.
이 메소드는 `$this`를 반환하므로 method chaining이 가능합니다.

#### getMessageType()

```
public function getMessageType(): string
```

메시지 타입을 반환합니다. 기본값은 'info'입니다.

#### set()

```
public function set(
    string $key,
    mixed $val
): self
```

이 오브젝트에 변수를 추가합니다. `$key`는 변수명, `$val`은 값입니다.
이 메소드는 `$this`를 반환하므로 method chaining이 가능합니다.

#### add()

```
public function add(
    string $key,
    mixed $val
): self
```

이 메소드는 `set()`의 별칭입니다. `$this`를 반환하므로 method chaining이 가능합니다.

#### sets()

```
public function sets(
    array|object $vars
): self
```

여러 개의 변수를 한 번에 추가합니다. 연관 배열이나 오브젝트를 인자로 받습니다.
이 메소드는 `$this`를 반환하므로 method chaining이 가능합니다.

#### adds()

```
public function adds(
    array|object $vars
): self
```

이 메소드는 `sets()`의 별칭입니다. `$this`를 반환하므로 method chaining이 가능합니다.

#### get()

```
public function get(string $key): mixed
```

지정한 한 개의 변수를 가져옵니다.
존재하지 않는 변수명을 지정한 경우, `null`을 반환합니다.

#### gets()

```
public function gets(...$args): object
```

지정한 여러 개의 변수를 오브젝트로 가져옵니다.
존재하지 않는 변수명을 지정한 경우, 해당 속성은 `null`로 설정됩니다.

#### getVariables()

```
public function getVariables(): array
```

지금까지 추가된 모든 변수를 연관 배열로 가져옵니다.

#### getObjectVars()

```
public function getObjectVars(): object
```

지금까지 추가된 모든 변수를 오브젝트로 가져옵니다.

#### unset()

```
public function unset(string $key): void
```

변수를 삭제합니다.

#### toBool()

```
public function toBool(): bool
```

이 오브젝트가 가진 에러 코드가 0인 경우 `true`를 반환하고, 그렇지 않으면 `false`를 반환합니다.
DB 쿼리나 데이터를 변경하는 작업이 성공했는지 확인하는 데 사용합니다.

#### toBoolean()

```
public function toBoolean(): bool
```

이 메소드는 `toBool()`의 별칭입니다.