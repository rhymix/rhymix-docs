String 확장
------------

라이믹스는 자바스크립트의 `String` 타입을 확장하여 몇 가지 편의 기능을 제공합니다.

#### String.prototype.getQuery()

```
String.prototype.getQuery(key)
```

문자열이 URL인 경우, 쿼리스트링 부분에서 특정 파라미터의 값을 추출합니다.

#### String.prototype.setQuery()

```
String.prototype.setQuery(key, val)
```

문자열이 URL인 경우, 쿼리스트링에 파라미터를 추가합니다.
이미 존재하는 파라미터인 경우, 주어진 값으로 대체합니다.

#### String.prototype.escape()

```
String.prototype.escape(double_escape)
```

문자열에 포함된 `<`, `>`, `&`, `"`, `'` 특수문자를 HTML 엔티티로 인코딩합니다.
백엔드에서 사용하는 `escape()` 함수와 같은 기능입니다.

`double_escape`를 `false`로 할 경우, 이미 인코딩된 것은 다시 인코딩하지 않습니다.

#### String.prototype.unescape()

```
String.prototype.unescape()
```

위의 `escape()` 함수로 인코딩된 것을 디코딩합니다.
PHP의 `htmlspecialchars_decode()` 함수와 같은 기능입니다.

#### String.prototype.stripTags()

```
String.prototype.stripTags()
```

문자열에서 HTML 태그로 보이는 부분을 제거합니다.
PHP의 `strip_tags()` 함수와 같은 기능입니다.

#### String.prototype.trim()

```
String.prototype.trim()
```

문자열의 앞뒤에서 공백을 제거합니다.
PHP의 `trim()` 함수와 같은 기능입니다.

이 메소드는 구형 브라우저를 위한 polyfill입니다.
대부분의 브라우저와 자바스크립트 엔진은 이미 문자열에 `trim()` 메소드가 존재합니다.
이 경우 라이믹스에서는 `trim()` 메소드를 추가하거나 대체하지 않습니다.
