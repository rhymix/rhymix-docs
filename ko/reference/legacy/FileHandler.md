FileHandler
-----------

이 클래스가 제공하는 대부분의 메소드는 XE 1.x에서 사용하던 방식과 동일하며,
라이믹스에서는 `Rhymix\Framework\Storage` 클래스로 대부분 대체할 수 있습니다.
실제로 이 클래스의 메소드 중 내부적으로 `Rhymix\Framework\Storage`의 메소드를 호출하는 경우가 많습니다.

#### getRealPath()

```
public static function getRealPath(string $source): string
```

`./`로 시작하는 상대경로에 `RX_BASEDIR` 상수를 붙여 절대경로로 변환합니다.

함수명과 달리 경로가 실제 존재하는지는 확인하지 않으며, 심볼릭 링크를 디레퍼런스하지도 않습니다.
즉, PHP의 `realpath()` 함수와는 무관하고, 단순히 문자열을 조작하는 함수입니다.

#### copyDir()

```
public static function copyDir(
    string $source_dir,
    string $target_dir,
    string $filter = ''
): void
```

디렉토리를 복사합니다. 대상 디렉토리가 존재하지 않으면 생성하며,
`$filter`를 사용하여 특정 파일을 제외할 수 있습니다.

`Rhymix\Framework\Storage::copyDirectory()`로 대체할 수 있습니다.

#### copyFile()

```
public static function copyFile(
    string $source,
    string $target,
    string $force = 'Y'
)
```

파일을 복사합니다. `$force`는 라이믹스에서는 무시합니다.

`Rhymix\Framework\Storage::copy()`로 대체할 수 있습니다.

#### readFile()

```
public static function readFile(string $filename): string|false
```

파일의 내용을 읽어 문자열로 반환합니다.

`Rhymix\Framework\Storage::read()`로 대체할 수 있습니다.

#### writeFile()

```
public static function writeFile(
    string $filename,
    string $buff,
    string $mode = 'w'
): bool
```

주어진 내용을 파일에 씁니다.

`Rhymix\Framework\Storage::write()`로 대체할 수 있습니다.

#### removeFile()

```
public static function removeFile(string $filename): bool
```

파일을 삭제합니다.

`Rhymix\Framework\Storage::delete()`로 대체할 수 있습니다.

#### rename()

```
public static function rename(
    string $source,
    string $target
): bool
```

파일명을 변경합니다.

`Rhymix\Framework\Storage::move()`로 대체할 수 있습니다.

#### moveFile()

```
public static function moveFile(
    string $source,
    string $target
): bool
```

파일을 이동합니다.

이름 변경과 이동은 파일시스템의 입장에서 동일한 작업이므로,
`rename()`과 사실상 같은 메소드입니다.

#### moveDir()

```
public static function moveDir(
    string $source_dir,
    string $target_dir
): bool
```

디렉토리를 이동합니다.

이름 변경과 이동은 파일시스템의 입장에서 동일한 작업이므로,
`rename()`과 사실상 같은 메소드입니다.

#### readDir()

```
public static function readDir(
    string $path,
    string $filter = '',
    bool $to_lower = false,
    bool $concat_prefix = false
): array
```

주어진 경로에 있는 파일 목록을 반환합니다.

`$filter`는 정규식으로, 해당 패턴과 일치하는 파일만 반환하도록 할 수 있습니다.

`$to_lower`가 `true`이면 파일명을 소문자로 변환하여 반환합니다.

`$concat_prefix`가 `true`이면 파일명 앞에 경로를 붙여 절대경로를 반환합니다.

#### makeDir()

```
public static function makeDir(string $path_string): bool
```

디렉토리를 생성합니다.

`Rhymix\Framework\Storage::createDirectory()`로 대체할 수 있습니다.

#### removeDir()

```
public static function removeDir(string $path): bool
```

디렉토리와 모든 하위 디렉토리, 파일을 삭제합니다.

`Rhymix\Framework\Storage::deleteDirectory()`로 대체할 수 있습니다.

#### removeBlankDir()

```
public static function removeBlankDir(string $path): bool
```

디렉토리가 비어 있는 경우에만 삭제합니다.

`Rhymix\Framework\Storage::deleteEmptyDirectory()`로 대체할 수 있습니다.

#### removeFilesInDir()

```
public static function removeFilesInDir(string $path): bool
```

디렉토리는 삭제하지 않고, 그 안에 있는 모든 디렉토리와 파일을 삭제합니다.

`Rhymix\Framework\Storage::deleteDirectory()`로 대체할 수 있습니다.

#### filesize()

```
public static function filesize(int $size): string
```

파일 크기를 보기 좋게 표시합니다.
예를 들어 12345678 바이트는 "12.3MB"로 표시합니다.

#### getRemoteResource()

```
public static function getRemoteResource(
    string $url,
    ?string $body = null,
    int $timeout = 3,
    string $method = 'GET',
    ?string $content_type = null,
    array $headers = [],
    array $cookies = [],
    array $post_data = [],
    array $settings = []
): ?string
```

외부 리소스를 요청하여 그 내용을 반환합니다.

`Rhymix\Framework\HTTP::request()` 메소드로 대체할 수 있습니다.

#### getRemoteFile()

```
public static function getRemoteFile(
    string $url,
    string $target_filename,
    ?string $body = null,
    int $timeout = 3,
    string $method = 'GET',
    ?string $content_type = null,
    array $headers = [],
    array $cookies = [],
    array $post_data = [],
    array $request_config = []
): bool
```

외부 리소스를 요청하고, 그 내용을 파일에 저장합니다.
용량이 큰 외부 리소스를 다운로드할 때 유용합니다.
`Rhymix\Framework\HTTP::download()` 메소드로 대체할 수 있습니다.

#### returnBytes()

```
public static function returnBytes(string $val): int
```

보기 좋게 표시한 파일 크기를 정수로 변환합니다.
예를 들어 "12.3MB"는 12300000으로 변환합니다.
유효숫자의 한계로 인해 원본 파일의 정확한 크기와 다를 수 있습니다.

#### checkMemoryLoadImage()

```
public static function checkMemoryLoadImage(array &$imageInfo): bool
```

GD 라이브러리를 사용하여 이미지 파일을 로드하기에 메모리가 충분한지 확인합니다.
실제로 이미지를 로드하기 전에 이론적으로 계산하는 것이므로, 정확하지 않을 수 있지만
이미지 변환 도중 메모리 부족으로 인한 오류를 방지하는 데 도움이 됩니다.

`$imageInfo`는 `getimagesize()` 함수로 얻은 이미지 정보 배열입니다.

#### checkImageRotation()

```
public static function checkImageRotation(string $filename): int|bool
```

이미지를 회전시켜야 하는지 확인합니다.
회전이 필요한 경우 0, 90, 180, 270 등의 각도를 반환합니다.
`exif` 확장이 설치되어 있지 않거나 회전 여부를 확인할 수 없는 이미지인 경우 `false`를 반환합니다.

#### createImageFile()

```
public static function createImageFile(
    string $source_file,
    string $target_file,
    int $resize_width = 0,
    int|string $resize_height = 0,
    string $target_type = '',
    string $thumbnail_type = 'fill',
    int $quality = 100,
    int $rotate = 0
)
```

이미지의 크기를 변환하고 화전시키는 등의 작업을 수행하여, 새로운 이미지 파일을 생성합니다.
주로 썸네일을 만들 때 사용합니다.

`$source_file`은 원본 이미지 파일의 경로이며, `$target_file`은 생성할 이미지 파일의 경로입니다.

`$resize_width`와 `$resize_height`는 생성할 이미지의 너비와 높이를 지정합니다.
`$resize_height`를 `'auto'`로 지정하면 너비에 따라 높이를 자동으로 결정합니다.

`$target_type`은 생성할 이미지의 형식으로, `'jpg'`, `'png'`, `'gif'` 등을 지정할 수 있습니다.
`'png'`를 사용하면 투명한 배경을 지원합니다.
타입을 지정하지 않으면 원본 이미지의 형식이 그대로 사용됩니다.

`$thumbnail_type`은 썸네일 생성 방식으로, 라이믹스에서는 `'fill'`이 기본값이며
그 밖에 `'crop'`, `'stretch'`, `'center'` 등을 사용할 수 있습니다.

`$quality`는 JPEG 품질을 나타내며, 0에서 100 사이의 값을 가집니다.
원본과 동일한 화질은 75 전후이니 참고하십시오.
지나치게 높은 값을 지정하면 원본보다 용량이 더 큰 썸네일이 생성될 수도 있습니다.

`$rotate`는 회전 각도로, 0, 90, 180, 270 중 하나를 지정할 수 있습니다.

이미지 변환에 성공하면 `true`를 반환하고, 실패하면 `false`를 반환합니다.

#### readIniFile()

```
public static function readIniFile(string $filename): array|false
```

ini 파일을 읽어 배열로 반환합니다.

#### writeIniFile()

```
public static function writeIniFile(
    string $filename,
    array $arr
): bool
```

배열을 ini 파일에 저장합니다.

#### openFile()

```
public static function openFile(
    string $filename,
    string $mode
): FileObject
```

파일을 열어 `FileObject` 객체를 반환합니다.
아주 오래된 버전의 XE 1.x에서 사용하던 방법으로, 권장하지 않습니다.

#### hasContent()

```
public static function hasContent(string $filename): bool
```

파일 용량이 0보다 크면 `true`를 반환합니다.

`Rhymix\Framework\Storage::getSize()` 메소드로 대체할 수 있습니다.

#### exists()

```
public static function exists(string $filename): string|false
```

주어진 경로에 파일이 존재하는 경우, 파일명을 반환하고, 그렇지 않으면 `false`를 반환합니다.

`Rhymix\Framework\Storage::exists()` 메소드로 대체할 수 있습니다.

#### isDir()

```
public static function isDir(string $path): string|false
```

주어진 경로가 디렉토리인 경우, 경로를 반환하고, 그렇지 않으면 `false`를 반환합니다.

`Rhymix\Framework\Storage::isDirectory()` 메소드로 대체할 수 있습니다.

#### isWritableDir()

```
public static function isWritableDir(string $path): bool
```

주어진 경로가 쓰기 가능한 디렉토리인 경우, `true`를 반환하고, 그렇지 않으면 `false`를 반환합니다.

`Rhymix\Framework\Storage::isDirectory()` 및
`Rhymix\Framework\Storage::isWritable()` 메소드로 대체할 수 있습니다.

#### clearStatCache()

```
public static function clearStatCache(
    string $target,
    bool $include = false
): void
```

주어진 경로에 `clearstatcache()` 함수를 호출합니다.
주어진 경로가 디렉토리인 경우, `$include`가 `true`이면 해당 디렉토리에 포함된 모든 파일에 적용합니다.

#### invalidateOpcache()

```
public static function invalidateOpcache(
    string $target,
    bool $force = true
)
```

주어진 경로에 `opcache_invalidate()` 함수를 호출합니다.
