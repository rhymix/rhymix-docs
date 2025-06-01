
레거시 클래스
--------------

레거시(legacy) 클래스는 XE 1.x부터 존재해 온 전역 클래스들을 뜻합니다.
레거시 클래스는 `Rhymix\Framework`와 함께 코어의 중요한 한 축을 담당하며,
특히 HTTP 요청을 받아서 처리하는 기본적인 플로우에 여전히 크게 관여하고 있습니다.

라이믹스는 레거시 클래스의 기능을 점진적으로 `Rhymix\Framework`로 이전 및 대체하려고 하지만, 긴급한 사안은 아닙니다.
대체 여부 및 우선순위에 따라 레거시 클래스는 크게 세 가지로 분류됩니다.

### 적극 사용중인 레거시 클래스

아래의 클래스들은 라이믹스 실행 과정에 적극 관여하므로 제거하기 곤란합니다.
계속 유지보수할 뿐 아니라, 기능이 추가되기도 합니다.

| 클래스                | 설명                                                                       |
|----------------------|---------------------------------------------------------------------------|
| BaseObject           | 모듈, DB 쿼리, 이벤트 핸들러 등의 반환값을 담는 데 사용                            |
| Context              | 요청에 대한 전반적인 환경 관리, 전역변수 관리, 응답 작성을 위한 다양한 메소드 제공       |
| FrontEndFileHandler  | HTML 응답에 사용할 CSS, JS 리소스 관리                                        |
| ModuleHandler        | 모듈 라이프사이클 관리                                                        |
| ModuleObject         | 모든 모듈 클래스의 기초가 되는 클래스                                            |
|----------------------|---------------------------------------------------------------------------|

(자세한 정보 추가 예정)

### 대체 예정인 레거시 클래스

아래의 클래스들이 수행하는 역할은 `Rhymix\Framework`로 대체하는 중이거나, 대체할 예정입니다.
이후에는 하위호환성을 위한 껍데기만 남게 될 가능성이 높습니다.

| 클래스                | 설명                                                                       |
|----------------------|---------------------------------------------------------------------------|
| DisplayHandler       | 응답 렌더링                                                                 |
| - HTMLDisplayHandler | HTML 응답 렌더링                                                            |
| - JSONDisplayHandler | JSON 응답 렌더링                                                            |
| - RawDisplayHandler  | 형식이 정해지지 않은 응답 렌더링                                                |
| - XMLDisplayHandler  | XML 응답 렌더링                                                             |
| FileHandler          | `Rhymix\Framework\Storage`로 대부분 대체 가능하나, 일부 기능 유지중              |
| Mobile               | `Rhymix\Framework\UA`로 대부분 대체 가능하나, 일부 기능 유지중                   |
| PageHandler          | DB 쿼리 결과 중 페이지네이션 표현에 사용                                         |
| Validator            | XE 1.x 방식의 XML 룰셋 적용 (백엔드 검증)                                      |
| XmlJsFilter          | XE 1.x 방식의 XML 필터 적용 (프론트엔드 검증)                                   |
|----------------------|---------------------------------------------------------------------------|

### 대체 완료된 레거시 클래스

아래의 클래스들은 `Rhymix\Framework` 클래스로 이미 대체되었거나, 라이믹스에서 하는 역할이 없습니다.
현재 남아 있는 것은 혹시 모를 서드파티 자료를 위한 하위호환성 보장용 껍데기일 뿐입니다.
추후 완전히 제거되거나 역할이 더욱 축소될 수 있으므로 사용을 권장하지 않습니다.

| 클래스                | 설명                                                                       |
|----------------------|---------------------------------------------------------------------------|
| CacheHandler         | `Rhymix\Framework\Cache`                                                  |
| DB                   | `Rhymix\Framework\DB`                                                     |
| EditorHandler        | 미사용                                                                     |
| EmbedFilter          | `Rhymix\Framework\Filters\MediaFilter`                                    |
| FileObject           | 미사용                                                                     |
| GeneralXmlParser     | 미사용                                                                     |
| Handler              | 미사용                                                                     |
| IpFilter             | `Rhymix\Framework\Filters\IpFilter`                                       |
| JSCallbackDisplayHandler | 미사용                                                                 |
| Mail                 | `Rhymix\Framework\Mail`                                                   |
| Password             | `Rhymix\Framework\Password`                                               |
| Purifier             | `Rhymix\Framework\Filters\HTMLFilter`                                     |
| Security             | `Rhymix\Framework\Security`                                               |
| TemplateHandler      | `Rhymix\Framework\Template`                                               |
| UploadFileFilter     | `Rhymix\Framework\Filters\FileContentFilter`                              |
| VirtualXMLDisplayHandler | 미사용                                                                 |
| WidgetHandler        | 미사용                                                                     |
| XEHttpRequest        | 미사용                                                                     |
| XeXmlParser          | 미사용                                                                     |
| XmlGenerator         | 미사용                                                                     |
| XmlLangParser        | `Rhymix\Framework\Parsers\LangParser`                                     |
|----------------------|---------------------------------------------------------------------------|
