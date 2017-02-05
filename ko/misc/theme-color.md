레이아웃에서 주제 색상 지정하기
-------------------------------

### 개요
라이믹스에서 추가되는 기본 스킨들에는 주제 색상 (Primary color) 개념을 도입했습니다. 주제 색상은 레이아웃 설정을 통해서 지정할 수 있습니다. 레이아웃 제작자와 호환 레이아웃을 사용하는 사용자에게 도움을 주기 위해서 매뉴얼을 작성합니다.


### 지정하는 방법
#### 사용자
* 주제 색상을 지원하는 레이아웃에서 주제 색상을 선택합니다.
  * 선택하는 방법은 드롭다운 메뉴나 컬러 피커가 있습니다.

#### 개발자
레이아웃 설정에서 `primary_color` 또는 `customized_primary_color` 항목을 지정합니다.

* `primary_color` 는 `type=select` 인 `extra_vars` 입니다.
* `primary_color` 의 값은 6자리 또는 16진법 색상 코드(HEXcode) 또는 Material design guide 의 색상 중에서 가능합니다. Material design 색상은 아래 값으로 지정될 수 있습니다.
  * `red`
  * `crimson`
  * `pink`
  * `purple`
  * `deep-purple`
  * `indigo`
  * `deep-blue`
  * `blue`
  * `light-blue`
  * `cyan`
  * `teal`
  * `green`
  * `light-green`
  * `lime`
  * `yellow`
  * `amber`
  * `orange`
  * `deep-orange`
  * `brown`
  * `grey`
  * `blue-grey`
  * `black`
  * `white`
* `customized_primary_color` 는 `type=colorpicker` 인 `extra_vars` 입니다.
* `primary_color` 의 값은 6자리 16진법 색상 코드(HEXcode) 입니다.

### 레이아웃 info.xml 예시

```xml
<?xml version="1.0" encoding="UTF-8"?>
<layout version="0.2">
	<title xml:lang="ko">네모의 꿈-예시</title>
	<title xml:lang="en">Rectangular World-Example</title>
	<description xml:lang="ko">깔끔한 면과 그림자 레이아웃</description>
	<description xml:lang="en">Simple rectangular planes and shadows</description>
	<version>0.0.1</version>
	<date>2017-02-05</date>
	<author email_address="misol.kr@gmail.com" link="https://github.com/misol">
		<name xml:lang="ko">misol</name>
		<name xml:lang="en">misol</name>
	</author>
	<menus>
		<menu name="GNB" maxdepth="2" default="true">
			<title xml:lang="ko">메뉴</title>
			<title xml:lang="en">Global Navigation Menu</title>
		</menu>
	</menus>
	<extra_vars>
		<var name="primary_color" type="select">
			<title xml:lang="ko">중심 색상</title>
			<title xml:lang="en">Primary color</title>
			<description xml:lang="ko">분위기를 형성하는데 사용되는 중심 색상입니다.</description>
			<description xml:lang="en">Please select the mood color you want.</description>
			<options value="red">
				<title xml:lang="ko">붉은 색</title>
				<title xml:lang="en">Red</title>
			</options>
			<options value="crimson">
				<title xml:lang="ko">크림슨</title>
				<title xml:lang="en">Crimson</title>
			</options>
			<options value="pink">
				<title xml:lang="ko">분홍</title>
				<title xml:lang="en">Pink</title>
			</options>
			<options value="purple">
				<title xml:lang="ko">보라</title>
				<title xml:lang="en">Purple</title>
			</options>
			<options value="deep-purple">
				<title xml:lang="ko">진보라</title>
				<title xml:lang="en">Deep Purple</title>
			</options>
			<options value="indigo">
				<title xml:lang="ko">인디고</title>
				<title xml:lang="en">Indigo</title>
			</options>
			<options value="deep-blue">
				<title xml:lang="ko">짙은 파랑</title>
				<title xml:lang="en">Deep Blue</title>
			</options>
			<options value="blue">
				<title xml:lang="ko">파랑</title>
				<title xml:lang="en">Blue</title>
			</options>
			<options value="light-blue">
				<title xml:lang="ko">밝은 파랑</title>
				<title xml:lang="en">Light Blue</title>
			</options>
			<options value="cyan">
				<title xml:lang="ko">시안</title>
				<title xml:lang="en">Cyan</title>
			</options>
			<options value="teal">
				<title xml:lang="ko">틸</title>
				<title xml:lang="en">Teal</title>
			</options>
			<options value="green">
				<title xml:lang="ko">초록</title>
				<title xml:lang="en">Green</title>
			</options>
			<options value="light-green">
				<title xml:lang="ko">연한 초록</title>
				<title xml:lang="en">Light Green</title>
			</options>
			<options value="lime">
				<title xml:lang="ko">라임</title>
				<title xml:lang="en">Lime</title>
			</options>
			<options value="yellow">
				<title xml:lang="ko">노랑</title>
				<title xml:lang="en">Yellow</title>
			</options>
			<options value="amber">
				<title xml:lang="ko">앰버</title>
				<title xml:lang="en">Amber</title>
			</options>
			<options value="orange">
				<title xml:lang="ko">주황</title>
				<title xml:lang="en">Orange</title>
			</options>
			<options value="deep-orange">
				<title xml:lang="ko">진한 주황</title>
				<title xml:lang="en">Deep Orange</title>
			</options>
			<options value="brown">
				<title xml:lang="ko">갈색</title>
				<title xml:lang="en">Brown</title>
			</options>
			<options value="grey">
				<title xml:lang="ko">회색</title>
				<title xml:lang="en">Grey</title>
			</options>
			<options value="blue-grey">
				<title xml:lang="ko">푸른 회색</title>
				<title xml:lang="en">Blue Grey</title>
			</options>
			<options value="customized">
				<title xml:lang="ko">커스텀</title>
				<title xml:lang="en">Customized</title>
			</options>
		</var>
		<var name="customized_primary_color" type="colorpicker">
			<title xml:lang="ko">커스텀 중심 색상</title>
			<title xml:lang="en">Customized primary color</title>
			<description xml:lang="ko">분위기를 형성하는데 사용되는 중심 색상을 위에서 선택하지 않고 Hex 코드로 직접 입력 합니다. (기본 값: #f44336) 위 항목에서 '커스텀' 항목을 선택해야 적용됩니다.</description>
			<description xml:lang="en">Please type the mood color you want, if there is no choice before. Insert your color in hex code. (dafault value:  #f44336) To use this option, you have to select 'Customized' for the options before.</description>
		</var>
	</extra_vars>
</layout>
```
