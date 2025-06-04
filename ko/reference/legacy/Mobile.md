Mobile
------

접속자가 모바일 기기를 사용하는지 판단하는 클래스입니다.

#### getInstance()

```
public function getInstance(): self
```

이 클래스의 인스턴스를 생성하여 반환합니다.
아래의 모든 메소드는 `static`으로 사용할 수 있으므로, 인스턴스 생성은 불필요합니다.
이 메소드는 하위 호환성을 위해 남아 있을 뿐입니다.

#### isFromMobilePhone()

```
public static function isFromMobilePhone(): bool
```

접속자의 User-Agent, 쿠키, 시스템 설정 등을 종합하여 현재 접속 기기를 모바일로 취급할지 판단합니다.
쿠키나 URL 파라미터를 사용하여 모바일 또는 PC로 강제 설정할 수 있으므로,
실제 화면 크기에 맞지 않는 결과가 나올 수도 있지만,
그것은 사용자의 명시적인 설정에 따른 것이므로 오류로 보지 않습니다.

#### isMobileCheckByAgent()

```
public static function isMobileCheckByAgent(): bool
```

현재 접속 기기가 모바일 기기(태블릿 포함)인지 확인합니다.
쿠키나 URL 파라미터가 아닌 User-Agent만을 기반으로 판단합니다.

#### isMobilePadCheckByAgent()

```
public static function isMobilePadCheckByAgent(): bool
```

현재 접속 기기가 태블릿인지 확인합니다.
쿠키나 URL 파라미터가 아닌 User-Agent만을 기반으로 판단합니다.

#### setMobile()

```
public static function setMobile(bool $ismobile): void
```

현재 접속자를 모바일 또는 PC로 강제 분류합니다.
`true`로 설정하면 모바일로, `false`로 설정하면 PC로 간주합니다.

#### isMobileEnabled()

```
public static function isMobileEnabled(): bool
```

시스템 설정에서 "모바일 뷰 사용"이 활성화되어 있는지 확인합니다.
