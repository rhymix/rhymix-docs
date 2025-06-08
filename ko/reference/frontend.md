프론트엔드 라이브러리
------------------

이 문서는 라이믹스에서 기본 제공하는 프론트엔드(JavaScript) 라이브러리와 함수들을 소개합니다.
아래의 라이브러리와 함수들은 라이믹스로 제작한 페이지라면 어디에서나 별도의 import 과정 없이 사용할 수 있습니다.


### 공용 라이브러리

아래의 라이브러리들은 코어에서 관리하므로 다른 버전을 중복으로 로딩하지 않도록 주의하시기 바랍니다.

- jQuery
  - 관리자 설정에 따라 2.x 또는 3.x 버전이 제공됩니다.
  - `$` 전역변수를 사용할 수 있습니다.
- js-cookie
  - `Cookies` 전역변수 또는 `Rhymix.cookie` 속성으로 참조할 수 있습니다.
- URI.js
  - `URI` 전역변수 또는 `Rhymix.URI` 속성으로 참조할 수 있습니다.


### Rhymix 오브젝트

`Rhymix` 오브젝트는 XE 1.x에서 `XE` 전역변수가 갖던 역할을 물려받습니다.

대부분의 메소드는 현재 접속 환경과 라이믹스 설정 상태에 대한 정확한 정보를 제공하는 것이 목적으로,
매우 간단한 구조를 띠고 있습니다.

AJAX 및 모달창 관련 메소드는 라이믹스 2.1.24부터 새로 추가된 기능으로,
앞으로 이와 같은 메소드들이 점진적으로 더 추가될 예정입니다.

아래의 설명에서는 파라미터와 반환값의 타입을 명시하기 위한 목적으로 일부 TypeScript 문법을 사용하였으나,
실제 코드는 바닐라 JavaScript와 jQuery를 활용하여 작성되어 있으며,
라이믹스에서 프론트엔드 코드 작성시 TypeScript를 사용할 필요는 없습니다.

#### Rhymix.isMobile()

```
Rhymix.isMobile(): boolean
```

현재 접속한 환경이 모바일 기기인 경우 `true`, 그렇지 않으면 `false`를 반환합니다.

#### Rhymix.getColorScheme()

```
Rhymix.getColorScheme(): 'light' | 'dark'
```

현재 다크모드가 선택되어 있는 경우 `'dark'`, 그렇지 않으면 `'light'`를 반환합니다.

#### Rhymix.setColorScheme()

```
Rhymix.setColorScheme(color_scheme: 'light' | 'dark'): void
```

색상 모드를 변경합니다. `'light'` 또는 `'dark'`를 넘길 수 있습니다.
라이믹스 기준을 따르는 레이아웃과 스킨이라면 즉시 변경되나,
에디터 등 일부 요소는 새로고침 후에 반영될 수도 있습니다.

#### Rhymix.detectColorScheme()

```
Rhymix.detectColorScheme(): void
```

색상 모드를 자동으로 감지하여 적용합니다.
이 함수는 페이지 로딩시 자동으로 호출됩니다.

#### Rhymix.getLangType()

```
Rhymix.getLangType(): string
```

현재 사용중인 언어를 반환합니다. 예) 한국어 `'ko'`

#### Rhymix.setLangType()

```
Rhymix.setLangType(lang_type: string): void
```

언어를 변경합니다. 즉시 반영되지는 않을 수도 있습니다.
언어 변경 후 새로고침을 원하시면 `location.reload()`를 사용하세요.

#### Rhymix.getCSRFToken()

```
Rhymix.getCSRFToken(): string
```

현재 사용중인 CSRF 토큰값을 반환합니다.

#### Rhymix.setCSRFToken()

```
Rhymix.setCSRFToken(token: string): void
```

CSRF 토큰값을 특정한 문자열로 변경합니다.

#### Rhymix.getRewriteLevel()

```
Rhymix.getRewriteLevel(): 0 | 1 | 2
```

관리자가 설정한 짧은주소 레벨을 반환합니다.

- 0: 짧은주소 사용 안 함
- 1: XE와 호환되는 형태만 사용
- 2: 모든 형태 사용

#### Rhymix.getBaseUrl()

```
Rhymix.getBaseUrl(): string
```

현재 사이트가 설치된 경로 중 origin을 제외한 부분을 반환합니다.
서버단의 `RX_BASEURL`에 해당하며, 대부분의 사이트에서는 `/`이지만,
하위 폴더에 설치한 경우 `/rhymix/`와 같은 형태로 나올 수도 있습니다.

#### Rhymix.getDefaultUrl()

```
Rhymix.getDefaultUrl(): string
```

현재 사이트의 전체 주소를 반환합니다.

#### Rhymix.getCurrentUrl()

```
Rhymix.getCurrentUrl(): string
```

현재 페이지의 전체 주소를 반환합니다.
짧은주소를 적용하지 않고 모든 파라미터를 풀어놓은 형태로 제공됩니다.

#### Rhymix.getCurrentUrlPrefix()

```
Rhymix.getCurrentUrlPrefix(): string
```

현재 페이지의 `mid` 값을 반환합니다.
빈 값일 수도 있습니다.

#### Rhymix.isCurrentUrl()

```
Rhymix.isCurrentUrl(url: string): boolean
```

주어진 URL이 현재 페이지 주소와 동일한 것으로 판단될 경우 `true`, 그렇지 않으면 `false`를 반환합니다.
상대경로도 정확하게 인식하며, URL 뒤에 붙은 `#hash` 부분은 제외하고 비교하는 등,
단순히 문자열을 비교하는 것보다 편리한 기능을 제공합니다.

#### Rhymix.isSameOrigin()

```
Rhymix.isSameOrigin(url1: string, url2: string): boolean
```

두 URL이 동일한 origin에 소속되어 있는 경우 `true`, 그렇지 않으면 `false`를 반환합니다.
origin을 기준으로 판단하므로 `Rhymix.isSameHost()`보다 엄격합니다.

#### Rhymix.isSameHost()

```
Rhymix.isSameHost(url: string): boolean
```

두 URL의 도메인이 동일한 경우 `true`, 그렇지 않으면 `false`를 반환합니다.
프로토콜(http, https) 및 포트를 비교하지 않으므로
실제 브라우저가 cross-origin 요청을 구분하는 기준과 일치하지는 않으나,
동일한 사이트의 내부 URL인지 판단하는 용도로 활용할 수 있습니다.
(권장하지는 않습니다. XE에서 사용하던 레거시 함수입니다.)

#### Rhymix.redirectToUrl()

```
Rhymix.redirectToUrl(url: string): void
```

주어진 URL로 페이지를 전환합니다.
단, `Rhymix.isCurrentUrl()`의 기준으로 현재 페이지와 동일한 URL인 것으로 판단하는 경우,
페이지 전환 대신 새로고침합니다.

#### Rhymix.openWindow()

```
Rhymix.openWindow(
	url: string,
	target: string,
	features: string
): void
```

새 창을 열고, 새 창에 포커스를 줍니다. XE 1.x의 `winopen()`과 호환됩니다.
`window.open()`과 동일하나, reverse tabnabbing 공격에 대한 방어 수단이 추가되었습니다.

#### Rhymix.openPopup()

```
Rhymix.openPopup(
	url: string,
	target: string
): void
```

정해진 규격의 팝업창을 엽니다. XE 1.x의 `popopen()`과 호환됩니다.

#### Rhymix.modal.open()

```
Rhymix.modal.open(id: string): void
```

`id`라는 id를 가진 요소를 모달창으로 엽니다.
이 요소는 `active`라는 클래스를 추가했을 때 화면에 표시되어야 합니다.
이 방법으로 열린 모달창은 뒤로가기를 누르면 닫힙니다.

#### Rhymix.modal.openIframe()

```
Rhymix.modal.openIframe(
	url: string,
	target: string
): void
```

주어진 URL을 iframe에 열고, 이 iframe을 모달창으로 엽니다.
웹뷰앱처럼 실제 팝업창을 열기 어려운 환경에서 손쉽게 팝업창을 대체할 수 있습니다.
뒤로가기를 누르면 iframe이 닫힙니다.

#### Rhymix.modal.close()

```
Rhymix.modal.close(id: string): void
```

위의 방법으로 연 모달창을 닫으려면 이 함수를 호출하여야 합니다.
히스토리 처리를 위해 필요하며, iframe인 경우 `id`는 필요하지 않습니다.

#### Rhymix.ajax()

```
Rhymix.ajax(
	action: string | null,
	params: {} | FormData,
	success?: (data?: {}, xhr?: jqXHR): void,
	error?: (data?: {}, xhr?: jqXHR): void,
): void
```

특정 모듈의 act에 AJAX POST 요청을 하기 위한 표준 함수입니다.

과거 `exec_xml()`, `exec_json()` 등이 하던 역할을 라이믹스 2.1.24부터 이 함수로 모두 통일하고,
레거시 함수를 사용하는 코드는 점진적으로 바꾸어 나갈 예정입니다.

- `action`은 `module.act` 형태로 작성합니다. 예) `document.procDocumentVoteUp`
- `params`는 딕셔너리로 작성합니다. 예) `{ target_srl: document_srl }`
  - 폼에서 추출한 데이터이거나, 파일 업로드를 포함하는 경우 `FormData` 오브젝트를 넘길 수 있습니다.
    이 때 `action`은 `null`이어야 하며, `module`과 `act`는 `FormData`에서 추출하여 사용하게 됩니다.
- `success`는 요청 성공시 호출할 콜백 함수입니다.
  - 2개의 파라미터를 가질 수 있습니다. `data`는 반환된 데이터, `xhr`은 `jqXHR` 오브젝트를 받습니다.
- `error`는 요청 실패시 호출할 콜백 함수입니다.
  - 2개의 파라미터를 가질 수 있습니다. `data`는 반환된 데이터, `xhr`은 `jqXHR` 오브젝트를 받습니다.
  - 에러 콜백 함수가 `false`를 반환하지 않으면 에러 메시지 표시, 콘솔 로그 등이 그대로 진행됩니다.
    따라서 에러 메시지 표시를 막고 싶다면 `false`를 반환하여야 합니다.
  - 통신 오류뿐 아니라 라이믹스 모듈이나 애드온 등이 반환한 오류도 에러 콜백 함수로 전달됩니다.

기존의 `exec_xml()`, `exec_json()` 함수와 다른 점은 아래와 같습니다.

- 모든 데이터는 JSON 형태로 제출합니다. (단, `FormData`를 제출하는 경우 제외)
- 모든 응답은 JSON 형태로 받습니다.
- 403, 404, 500 등 HTTP 상태 코드와 관계없이, response body가 정상적인 JSON 형태를 띠고 있다면
  해당 JSON에 담긴 상태값 및 에러 메시지에 따라 성공 또는 실패 여부를 판단합니다.
  단, 정상적인 JSON 응답이 아니라면 에러 콜백 함수가 호출됩니다.
- `-1000` 미만의 `error` 값을 별도로 처리하지 않고, `0`이 아닌 경우 모두 에러로 취급합니다.
- 에러 발생시 나타나던 `AJAX communication error` 에러 메시지가 `AJAX error`로 단순화되었고,
  디버깅에 도움을 줄 수 있는 경로 정보를 조금 더 자세히 표시합니다.

#### Rhymix.ajaxForm()

```
Rhymix.ajaxForm(
	form: HTMLFormElement,
	success?: (data?: {}, xhr?: jqXHR): void,
	error?: (data?: {}, xhr?: jqXHR): void,
): void
```

일반 HTML 폼을 페이지 전환 없이 AJAX로 제출합니다.
주어진 폼의 내용을 직렬화하여 `Rhymix.ajax()`로 전달하며, 이후 과정은 `Rhymix.ajax()`와 같습니다.

`<form>` 태그에 `rx_ajax` 클래스를 추가하면 `submit` 이벤트 발생시 자동으로 이 함수를 거치게 되므로,
페이지 전환 (새로고침) 없는 폼 제출 및 에러 처리를 쉽게 구현할 수 있습니다.

#### Rhymix.checkboxToggleAll()

```
Rhymix.checkboxToggleAll(name: string): void
```

`XE.checkboxToggleAll()` 함수를 `Rhymix` 오브젝트로 옮겨온 것입니다.
주어진 이름을 가진 체크박스를 모두 선택하거나 해제합니다.
게시판에서 문서 관리에 내부적으로 사용합니다.
그 밖의 코드에서 직접 호출하는 것은 권장하지 않습니다.

#### Rhymix.displayPopupMenu()

```
Rhymix.displayPopupMenu(
	ret_obj: {},
	response_tags: {},
	params: {}
): void
```

`XE.displayPopupMenu()` 함수를 `Rhymix` 오브젝트로 옮겨온 것입니다.
회원, 문서, 댓글 등의 팝업 레이어를 표시하는 데 내부적으로 사용합니다.
그 밖의 코드에서 직접 호출하는 것은 권장하지 않습니다.

#### Rhymix.filesizeFormat()

```
Rhymix.filesizeFormat(size: int): string
```

파일 크기를 보기 쉽게 표현합니다.
백엔드에서 사용하는 `FileHandler::filesize()` 매소드와 같은 역할입니다.

#### Rhymix.lang()

```
Rhymix.lang(
	key: string,
	val?: string
): void
```

다국어 코드를 저장하거나 불러옵니다.
백엔드의 `lang()` 함수와 같은 역할입니다.

- `val`이 주어진 경우, `key`라는 코드에 `val`이라는 내용을 저장합니다.
- `val`이 주어지지 않은 경우, `key`라는 코드에 저장된 내용을 반환합니다.

백엔드에서 갖고 있는 모든 다국어 코드를 프론트엔드에서 자동으로 사용할 수 있는 것은 아니므로,
사용할 다국어 코드를 미리 저장해 두어야 그 페이지에서 불러쓸 수 있습니다.

기존 방식대로 `xe.lang.key = val;` 이라고 쓰는 것도 동일한 효과입니다.
(`xe.lang` 변수는 `Rhymix` 오브젝트 내부의 자료 구조로 맵핑되어 있습니다.)


### XE 전역 변수

구 버전에 존재하던 `XE` 전역 변수는 라이믹스 2.1.24부터 `Rhymix` 오브젝트에 대한 alias입니다.

예를 들어 `XE.filesizeFormat()`를 호출하면 `Rhymix.filesizeFormat()`를 호출하는 것과 같습니다.

XML 필터 등에서 사용하는 소문자 `xe` 전역 변수와는 다르니 주의하세요.


### 기타 함수

- [전역 함수](frontend/global_functions.md)
- [레거시 전역 함수](frontend/legacy_functions.md)
- [String 확장](frontend/string_extensions.md)
