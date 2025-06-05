Rhymix\Framework\Filters\FilenameFilter
---------------------------------------

파일명과 경로를 필터링하는 클래스입니다.

#### clean()

```
public static function clean(string $filename): string
```

파일명에서 보안상 위험하거나 오작동을 일으킬 수 있는 특수 문자를 제거하거나 안전한 문자로 치환합니다.

#### cleanPath()

```
public static function cleanPath(string $path): string
```

경로를 정리합니다. `./`, `../`, 후행 슬래시 등을 제거합니다.
윈도우 경로는 `\\`를 `/`로 변환한 후 정리합니다.

#### isDirectDownload()

```
public static function isDirectDownload(
    string $filename,
    bool $include_multimedia = true
): bool
```

직접 다운로드 가능한 확장자인지 확인합니다.

직접 다운로드 가능한 확장자란, 이러한 형식의 파일을 일반적인 에디터에서 업로드했을 때
`./files/attach/images/` 경로에 저장되고, 그 경로가 본문에 그대로 삽입되어
서버 측에서 PHP를 거치지 않고 직접 다운로드할 수 있는 파일을 말합니다.
대개 이미지나 비디오, 오디오 파일이 여기에 해당됩니다.

반면, 직접 다운로드할 수 없는 확장자를 가진 파일을 업로드하면
`./files/attach/binaries/` 경로에 저장되고,
`file` 모듈의 `procFileDownload` 액션을 통해서만 다운로드할 수 있습니다.
대개 문서 파일이나 압축 파일이 여기에 해당됩니다.

확장자는 대소문자를 구분하지 않습니다. 기본적으로 모든 멀티미디어 파일 확장자를 허용합니다.
`$include_multimedia`가 `false`인 경우, 이미지 파일만 허용됩니다.
