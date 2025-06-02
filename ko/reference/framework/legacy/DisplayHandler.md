DisplayHandler
--------------

#### printContent()

```
public function printContent(ModuleObject &$oModule): void
```

주어진 모듈 오브젝트가 가진 정보를 기반으로
HTML, JSON, XML 등의 응답을 생성하고 출력합니다.

> [애드온 실행 시점]
> 이 메소드는 `before_display_content` 애드온 실행 시점을 포함합니다.

#### getDebugInfo()

```
public static function getDebugInfo(string &$output = null): string
```

디버그 정보를 가공하여 출력물에 덧붙이거나 파일에 저장합니다.
