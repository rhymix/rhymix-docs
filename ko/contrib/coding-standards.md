코딩 규칙
---------

### 일반

PHP, HTML, XML, CSS, JS 등 모든 텍스트 파일의 문자셋은 BOM이 없는 UTF-8입니다.

줄바꿈 문자는 유닉스 방식(`LF`)을 따릅니다.

윈도우 메모장에서 편집한 파일은 위의 두 가지 규칙에 어긋납니다.
윈도우 사용자는 [Notepad++](https://notepad-plus-plus.org/) 등의 개발자용 에디터를 사용하여 편집하시기 바랍니다.

들여쓰기는 1개의 탭으로 합니다.
단, 탭 대신 스페이스를 사용하는 파일에서는 일관성 유지를 위해 4칸의 스페이스를 사용할 수 있습니다.
주석이나 긴 목록 등에서 줄을 맞추기 위해 한두 개의 스페이스를 추가하는 경우를 제외하면,
들여쓰기 목적으로 탭과 스페이스를 섞어서 사용하지 않습니다.

들여쓴 줄들 사이의 빈 줄도 들여씁니다. (에디터에서 후행 공백을 제거하지 않도록 설정하십시오.)

PHP 코드만으로 이루어진 파일은 맨 끝에 `?>` 태그를 사용하지 않습니다.

### 공백 및 줄바꿈 규칙

클래스 및 함수 선언과 `if`, `for`, `foreach`, `while` 등의 중괄호는 다음 줄에 씁니다.

    class Foo
    {                              // RIGHT
        public function bar() {    // WRONG
            
        }
    }

조건문이나 순환문 내에 하나의 명령만 있는 경우에도 반드시 중괄호를 사용합니다.
그래야 나중에 명령이 추가될 경우 수정하기 편리합니다.

    if (!$foo) return false;    // WRONG
    if (!$bar)                  // RIGHT
    {
        return true;
    }

단, 클로져는 같은 줄에 중괄호를 쓸 수 있으며,
이 경우 중괄호 앞뒤에 한 칸씩 공백을 두어 클로져가 시작되고 끝나는 지점을 찾기 쉽도록 합니다.
닫는 중괄호와 그 뒤의 기호 사이에는 공백을 두지 않습니다.

    $foo = function($bar) { return $bar + 1; };    // RIGHT
    $foo = function($bar) {                        // RIGHT
        return $bar + 1;
    };
    $foo = function($bar){return $bar + 1;};       // WRONG

자바스크립트에서는 거의 모든 함수가 클로져이며, 잘못 줄바꿈할 경우 세미콜론이 삽입되는 등 불편이 발생할 수 있으므로
중괄호를 다음 줄에 쓰지 않아도 됩니다.

    $("#foo").on("click", function() {    // OK
        if ($(this).val() === "bar") {    // OK
            $(this).val("baz");
        }
    });

함수 호출시 함수명과 괄호 사이, 괄호와 인자 사이에 공백을 두지 않습니다.
인자를 구분하는 쉼표는 뒤쪽에만 한 칸의 공백을 둡니다.

    function foobar($baz, $param)        // RIGHT
    function foobar ( $baz , $param )    // WRONG

배열 내의 요소들을 구분하는 쉼표도 뒤쪽에만 한 칸의 공백을 둡니다.

    array(1, 2, 3)    // RIGHT
	array(1,2,3)      // WRONG
	array(1 ,2 ,3)    // WRONG

`if`, `for`, `foreach`, `while` 등의 키워드 뒤에는 한 칸의 공백을 두는 것이 원칙이나,
XE 시절에 작성된 코드는 공백이 없는 것이 많으므로 이런 코드 주변에서는 공백 없이 쓸 수도 있습니다.
`==`, `!=`, `>` 등의 연산자는 항상 앞뒤에 한 칸씩 공백을 둡니다.

    if ($foo === $bar)    // RECOMMENDED
    if($foo === $bar)     // OK for XE Compatibility
    if($foo==$bar){       // WRONG

여러 줄에 걸쳐 배열을 선언할 경우, 마지막 구성요소 뒤에 쉼표를 남깁니다.
그래야 나중에 구성요소를 추가할 때 편리합니다.

    $animals = array(
        'bear',
        'cat',
        'dog',    // COMMA
    );

단, 자바스크립트 및 JSON에서는 마지막 쉼표를 반드시 삭제해야 합니다.
쉼표를 남겨둘 경우 일부 브라우저에서 오류가 발생할 수 있기 때문입니다.

    var animals = [
        'bear',
        'cat',
        'dog'    // NO COMMA
    ];

### 주석

주석은 관련 코드 윗줄에 써야 합니다.
조건문이나 루프의 경우에는 해당 키워드의 윗줄에 주석을 씁니다.

    // If foo is bar, do something.
    if ($foo->isBar())
    {
        // Note: this will do X, Y, and Z.
        $foo->doSomething();
    }
    // Otherwise, do something else.
    else
    {
        // TODO: Refactor this later.
        $foo->doSomethingElse();
    }

이전 블록의 마지막 부분에 주석을 집어넣어서는 안 됩니다.

    // RIGHT
    if ($condition)
    {
		
	}
	// RIGHT
	elseif ($foo)
	{
		// WRONG
	}
	else
	{
		
	}

모든 클래스와 함수에는 `/**`으로 시작하는 PHPDoc 방식의 주석을 붙여야 합니다.
PHPDoc 주석 작성에 어려움이 있는 경우, 다른 클래스와 함수의 주석을 참고하십시오.

    /**
     * This is the Foo class.
     */
    class Foo
    {
        /**
         * Constructor.
         *
         * @param string $member_srl
         */
        public function __construct($member_srl)
        {
            // 생략
        }
        
        /**
         * Check if this Foo is bar.
         *
         * @return bool
         */
        public function isBar()
        {
            return true;
        }
    }

불가피한 경우를 제외하면 주석은 영문으로 쓰는 것을 원칙으로 하며,
대문자로 시작하는 완전한 문장으로 이루어져야 합니다.

### 커밋 메시지

커밋 메시지는 가능하면 영문으로 작성하며, **동사원형**으로 시작하는 **현재형**, **명령형** 문장 사용을 원칙으로 합니다.

    Delete unnecessary condition     // RIGHT
    Fix #1234                        // RIGHT
    Deletes unnecessary condition    // WRONG (불필요한 3인칭 단수 동사변화)
    Fixed #1234                      // WRONG (불필요한 과거형)

이 규칙에 맞추어 영문으로 커밋 메시지를 작성하기 어려운 경우, 한글로 작성해도 무방합니다.
한글 커밋 메시지는 **어디서** **무엇을** **어떻게** 했는지 간결하고 명확하고 격식있게 표현하며,
가능하면 현재형 동사로 마치도록 합니다.

    크롬 최신 버전에서 스크립트 오류 해결    // RIGHT
    Foo 클래스에 bar() 메소드 추가           // RIGHT
    파일첨부 에러나는거 고쳤쩌염~^^          // WRONG (격식없는 표현)
    함수 개선                                // WRONG (두리뭉실한 표현)

### 기타

Rhymix의 기본 `error_reporting` 설정 하에서 어떤 에러도 발생하지 않도록 하는 것을 목표로 하지만,
선언하지 않은 변수 등 `E_NOTICE`를 일으키는 문제도 가능하면 새로 만들어내지는 않도록 합니다.

문자열과 문자열, 정수와 정수를 비교할 때는 가능하면 `==` 대신 `===`을 사용합니다.
실제 자료형이 다를 가능성이 있는 경우 `intval()`, `strval()` 등의 함수와 함께 사용합니다.
정수는 항상 64비트 범위를 가지는 것으로 가정합니다.

PHP 5.4 이상에서 지원하는 간단한 배열 문법(`[1, 2, 3]`)을 사용할 수 있으나,
복잡한 구조의 배열을 선언할 때는 이 문법이 오히려 가독성을 해칠 수 있으니 주의하시기 바랍니다.

전역 상수를 참조할 때는 `\RX_CLIENT_IP`와 같이 `\`를 앞에 붙여서,
추후 다른 네임스페이스로 코드를 이동 또는 복사하더라도 문제가 생기지 않도록 합니다.

여기에서 규정하지 않은 내용은 [PSR-1](http://www.php-fig.org/psr/psr-1/)과
[PSR-2](http://www.php-fig.org/psr/psr-2/)를 따릅니다.

composer 라이브러리를 업데이트할 때는 라이믹스에서 공식적으로 지원하는 환경(PHP 7.0 이상)에서 정상 작동하는 버전으로 고정시켜야 합니다.
라이믹스 지원 환경과 맞지 않는 라이브러리를 가져오는 것은 원칙적으로 금지되며, 불가피한 경우 해당 라이브러리를 호출하는 모든 부분에서 미리 버전을 체크하여
인클루드되는 시점에 오류가 발생하는 것을 막아야 합니다.
또한 불필요한 라이브러리가 들어오지 않도록 반드시 아래의 명령으로 업데이트합니다.

    composer update --no-dev --optimize-autoloader
