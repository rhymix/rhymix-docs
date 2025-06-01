레거시 전역 함수
---------------

아래의 자바스크립트 함수들은 XE 1.x에서 물려받았거나 라이믹스 초기에 추가된 것으로,
일부 오래된 코어 모듈과 서드파티 자료에서 사용하는 사례가 있으므로 하위 호환을 위해 제공합니다.
대체 불가능한 상황이 아니라면 사용을 권장하지 않습니다.


### 문서 관리 관련 함수

#### doAddDocumentCart()

```
function doAddDocumentCart(obj)
```

#### doDocumentPreview()

```
function doDocumentPreview(obj)
```

#### doDocumentSave()

```
function doDocumentSave(obj)
```

#### doDocumentLoad()

```
function doDocumentLoad(obj)
```

#### doDocumentSelect()

```
function doDocumentSelect(document_srl, module)
```


### AJAX 관련 레거시 함수

#### doCallModuleAction()

```
function doCallModuleAction(module, action, target_srl)
```

`Rhymix.ajax()`를 사용하세요.

#### exec_xml()

```
exec_xml(module, act, params, callback_success, return_fields, callback_success_arg, fo_obj)
```

`Rhymix.ajax()`를 사용하세요.

#### exec_json()

```
exec_json(action, params, callback_success, callback_error)
```

`Rhymix.ajax()`를 사용하세요.

#### exec_html()

```
exec_html()
```

`Rhymix.ajax()`를 사용하세요.

#### send_by_form()

```
send_by_form(url, params)
```

`Rhymix.ajax()`를 사용하세요.

#### arr2obj()

```
arr2obj(url, params)
```


### 쿠키 관련 함수

#### getCookie()

```
function getCookie(name)
```

라이믹스에서 제공하는 js-cookie 라이브러리를 사용하세요.

#### setCookie()

```
function setCookie(name, value, expires, path)
```

라이믹스에서 제공하는 js-cookie 라이브러리를 사용하세요.


### 기타 레거시 함수

#### doChangeLangType()

```
function doChangeLangType(obj)
```

`Rhymix.setLangType()`을 사용하세요.

#### rhymix_alert_close()

```
function rhymix_alert_close()
```

#### rhymix_alert()

```
function rhymix_alert(message, redirect_url, delay)
```

#### move_url()

```
function move_url(url, open_window)
```

#### setFixedPopupSize()

```
function setFixedPopupSize()
```

#### displayMultimedia()

```
function displayMultimedia(src, width, height, options)
```

#### transRGB2Hex()

```
function transRGB2Hex(value)
```

#### sendMailTo()

```
function sendMailTo(email_address)
```

#### viewSkinInfo()

```
function viewSkinInfo(module, skin)
```

#### xSleep()

```
function xSleep(sec)
```

#### isDef()

```
function isDef()
```

#### is_def()

```
function is_def(v)
```

#### ucfirst()

```
function ucfirst(str)
```

#### get_by_id()

```
function get_by_id(id)
```

#### GetObjLeft()

```
function GetObjLeft(obj)
```

#### GetObjTop()

```
function GetObjTop(obj)
```

#### getOuterHTML()

```
function getOuterHTML(obj)
```

#### replaceOuterHTML()

```
function replaceOuterHTML(obj, html)
```

#### toggleDisplay()

```
function toggleDisplay(id)
```

#### toggleSecuritySignIn()

```
function toggleSecuritySignIn()
```

#### completeMessage()

```
function completeMessage(ret_obj)
```

#### reloadDocument()

```
function reloadDocument()
```

#### open_calendar()

```
function open_calendar(fo_id, day_str, callback_func)
```

#### displayPopupMenu()

```
function displayPopupMenu(ret_obj, response_tags, params)
```

#### createPopupMenu()

```
function createPopupMenu()
```

#### chkPopupMenu()

```
function chkPopupMenu()
```

#### Base64.encode()

```
Base64.encode(input)
```

#### Base64.decode()

```
Base64.decode(input)
```


### 레거시 라이브러리

아래의 라이브러리는 라이믹스와 함께 배포되고, 경우에 따라 자동으로 로딩되기도 하지만, 사용을 권장하지 않습니다.

- modernizr
- x.js
- xml2json


### 프론트엔드 XE App

프론트엔드 XE App의 사용은 권장하지 않습니다.
jQuery나 일반적인 JavaScript를 사용해서 이벤트 처리를 하시기 바랍니다.
