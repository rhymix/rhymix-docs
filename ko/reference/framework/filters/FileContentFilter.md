Rhymix\Framework\Filters\FileContentFilter
------------------------------------------

파일 내용을 검사하는 클래스입니다.
PHP에서 인식하는 형식(`multipart/form-data`)`으로 업로드된 모든 파일은
`Context` 초기화 과정에서 이 필터를 거치게 됩니다.
검사를 통과하지 못하면 보안 정책 위반이라는 에러 메시지가 표시됩니다.

#### check()

```
public static function check(
    ?string $file = null,
    ?string $filename = null
): bool
```

파일 내용을 검사하는 주 메소드입니다.
`$file`은 검사할 파일의 서버 측 경로이며, `$filename`은 업로드시 전달받은 파일 이름입니다.

검사하는 항목은 파일 형식에 따라 다르며, 확장자보다는 실제 내용을 기준으로 판단합니다.

- XML: XXE 공격 감지
- HTML: PHP 코드 또는 server-side include의 가능성이 있는 코드 감지
- SVG: JavaScript 코드 감지, SSRF 공격 감지
