포토스와이프 연동하기
---------------------

### 개요

포토스와이프 애드온은 게시판 본문 이외의 공간에서도 이미지를 갤러리 형식으로 보여주도록 고려한 기능을 포함하고 있습니다.

### 포토스와이프 적용 범위

포토스와이프 애드온은 `xe_content` 클래스를 가진 요소 내에서 이미지를 검색하고 갤러리로 나타냅니다.

코드 예시
```html
<div class="xe_content">
	<img src="https://example.com/exampel_img.jpg" alt="This is an example image" />
</div>
```

#### 이미지 제외 기능

위에서 적은 `xe_content` 클래스를 가진 요소 내 이미지라도 다음 클래스를 가지고 있다면 적용 대상에서 제외합니다.

##### 이미지 제외 클래스

* `rx-escape`
* `photoswipe-escape`


위에 적은 클래스 외에 다음 요소의 자식 요소일 때에도 포토스와이프 적용 대상에서 제외됩니다.

##### 이미지 제외 부모 요소

* `a`
* `pre`
* `xml`
* `textarea`
* `input`
* `select`
* `option`
* `code`
* `script`
* `style`
* `iframe`
* `button`
* `img`
* `embed`
* `object`
* `ins`

위 두 가지 경우에도 불구하고 다음 클래스라면 적용 대상입니다.

##### 포토스와이프 등록 클래스

* `photoswipe-images`

#### 포토스와이프 등록, 제외 예시

포토스와이프로 표시되는 이미지 예시와 제외되는 예시는 다음과 같습니다.

##### 포토스와이프로 표시되는 이미지 예시

###### xe_content 의 자식 이미지

```html
<div class="xe_content">
	<img src="https://example.com/exampel_img.jpg" alt="This is an example image" />
</div>
```

###### photoswipe-images 클래스의 이미지

```html
<div class="xe_content">
	<img src="https://example.com/exampel_img.jpg" alt="This is an example image" class="photoswipe-images" />
</div>
```

제외 대상 부모 요소인 anchor 의 자식 요소더라도 `photoswipe-images` 를 클래스로 가진다면, 포토스와이프로 이미지를 표시하려는 시도를 합니다.

```html
<div class="xe_content">
	<a href="https://example.com/this-link-will-be-ignored">
		<img src="https://example.com/exampel_img.jpg" alt="This is an example image" class="photoswipe-images" />
	</a>
</div>
```

제외 대상 클래스인 `rx-escape` 또는 `photoswipe-escape` 를 포함하더라도 `photoswipe-images` 를 클래스로 가진다면, 포토스와이프로 이미지를 표시합니다.

```html
<div class="xe_content">
	<img src="https://example.com/exampel_img.jpg" alt="This is an example image" class="photoswipe-images rx-escape" />
</div>
```


##### 포토스와이프로 표시되지 않는 이미지 예시

###### 이미지 제외 클래스를 가진 경우

```html
<div class="xe_content">
	<img src="https://example.com/exampel_img.jpg" alt="This is an example image" class="rx-escape" />
</div>
```


```html
<div class="xe_content">
	<img src="https://example.com/exampel_img.jpg" alt="This is an example image" class="photoswipe-escape" />
</div>
```

###### 이미지 제외 부모 요소가 있는 경우

```html
<div class="xe_content">
	<a href="https://example.com/this-link-will-not-be-ignored">
		<img src="https://example.com/exampel_img.jpg" alt="This is an example image" />
	</a>
</div>
```