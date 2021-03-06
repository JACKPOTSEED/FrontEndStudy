# Sass Basic
	
* *이문서는 [sass-guidelin](http://sass-guidelin.es/ko/)의 내용을 포함하고 있습니다.*


 ###Preprocessing (전처리)

Sass(Syntactically Awesome Stylesheets)는 CSS 상위에 있는 메타언어(meta-language)로 보다 간결하고 격식을 갖춘 CSS 문법을 사용합니다.
소프트웨어 개발 원칙인 DRY(Don’t Repeat Yourself)를 적용해서 더 빠르고 효율적으로 코드를 작성하고 쉽게 유지보수 하도록 해줍니다.-단 가이드 라인에서 말하길 DRY 보다도 KISS 원칙 (Keep It Simple Stupid)을 우선 할 수 있습니다.- 또한 Sass는 CSS 전처리기(pre-processor)입니다. 스타일시트와 브라우저에서 해석할 CSS파일 중간에 위치하는 하나의 계층으로 Sass 문법으로 작성된 코드를 일반적으로 사용하는 CSS 코드로 컴파일해 줍니다.

Sass 파일을 CSS 코드로 컴파일 하는 가장 기본적인 방법은 `터미널`을 사용하는 것인데, Sass 설치 후에 터미널에서 `sass input.scss output.css`를 실행하면 개별 파일이나 전체 디렉토리를 변환할 수 있습니다 . 또,`--watch`명령어를 추가하면 폴더나 디렉토리를 실시간으로 감시할 수 있습니다.    
아래는 Sass의 전체 디렉토리를 실행하는 예시입니다.    

```SCSS

sass --watch app/sass:public/stylesheets

```

<br>
 ###Variables (변수)

Sass는 stylesheet 전반에 걸쳐 재사용 할 정보를 저장하는 방법으로 `변수`를 사용합니다.
이 변수에는 색상, 글꼴 등의 그 어떤 Css라도 저장할 수 있으며, 변수를 사용할 때는 `$` 기호를 이용합니다.

```SCSS

// _style.scss
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}

```
```css

/*style.css*/
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}

```
단, 적절한 이유 없이는 변수를 선언하면 안됩니다. CSS에는 변수 이름에 대한 유효범위가 없기 때문에, 충돌의 위험성이 아주 높습니다. 

<br>
[변수선언 가이드라인 참조](http://sass-guidelin.es/ko/#section-63) 


<br>
 ### Nesting (중첩)

Sass를 이용하면 HTML의 div 안에 div를 넣는다던지 하는 중첩 된 계층구조처럼 CSS선택자를 중첩할 수 있습니다.    
단, 지나친 중첩은 지양해야합니다. 지나친 중첩시 CSS로 컴파일시 문제가 발생할 수 있으며, 문서의 가독성을 떨어뜨릴 수 있습니다.    
아래는 적합하게 중첩된 *navigation style* 예입니다.

```SCSS

// _style.scss
nav {
	ul {
		margin: 0;
		padding: 0;
		list-style: none;
	}

	li { display: inline-block; }

	a {
		display: block;
		padding: 6px 12px;
		text-decoration: none;
	}
}

```
nav안에 ul, li, a selector가 있는 것을 알 수 있습니다.    
이런 경우에 중첩 방식을 사용하면  CSS를 정리하고 더 읽기 쉽게 만들 수있습니다.

```css
/*styel.css*/
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  display: inline-block;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}

```

<br>
[중첩선언 가이드라인 참조](http://sass-guidelin.es/ko/#nesting)


<br>
 ### Partials

Sass를 작성 시 CSS의 작은 조각파일을 포함하는 partial Sass파일을 만들어 다른 Sass파일에 포함 시킬 수 있습니다. 

```SCSS

// _reset.scss

html,
body,
ul,
ol {
  margin: 0;
  padding: 0;
}
```
```SCSS

// _base.scss

@import 'reset';

body {
  font: 100% Helvetica, sans-serif;
  background-color: #efefef;
}

```
Sass 파일명 앞에 `_`를 붙이면 CSS로 변환되지 않습니다. 이점을 이용해 가이드라인에서는 `main.scss`를 제외한 모든 Sass파일명 앞에는 `_`를 사용할 것을 권장하고 있습니다.   

<br>
[패턴 가이드라인 참조 ](http://sass-guidelin.es/ko/#section-54)


<br>
 ### Import (불러오기)

CSS에서 @import로 다른 파일을 연결시킬 수 있습니다. 
SASS에서도 @import를 사용할 수 있는데, CSS의 @import와는 문법이나 작동 방식이 다릅니다.

`.sass` , `.scss` 모두 확장자를 생략하고 호출가능합니다. 물론 확장자를 붙여도 됩니다.
확장자 없이 파일 이름만으로 가져오기를 하면 네 가지 파일이 있는지 검색하여 가져옵니다.
예를 들어

```SCSS

@import "inc";

```
는 다음 파일이 있는지 검색합니다.

* inc.scss
* inc.sass
* _inc.scss
* _inc.sass

네 개 중 하나만 존재하면 그 파일을 가져오고, 여러 파일이 존재하면 변환 시 에러가 납니다.    
SASS 파일을 가져왔다면, 가져온 파일에 있는 변수와 Mixin을 사용할 수 있습니다.

```SCSS 

// _rounded.scss

@mixin rounded($side, $radius: 10px) {
  border-#{$side}-radius: $radius;
  -moz-border-radius-#{$side}: $radius;
  -webkit-border-#{$side}-radius: $radius;
}

``` 
```SCSS 

/// _style.scss
@import "rounded";
 
nav li { @include rounded(top); }
footer { @include rounded(top, 5px); }
sidebar { @include rounded(left, 8px); }

```
```css

/* style.css */
 
nav li {
  border-top-radius: 10px;
  -moz-border-radius-top: 10px;
  -webkit-border-top-radius: 10px; }
 
footer {
  border-top-radius: 5px;
  -moz-border-radius-top: 5px;
  -webkit-border-top-radius: 5px; }
 
sidebar {
  border-left-radius: 8px;
  -moz-border-radius-left: 8px;
  -webkit-border-left-radius: 8px; }
```

 ### Mixins

믹스인은 Sass 언어 전체에서 가장 많이 사용되는 기능 중 하나이며, 재사용성과 DRY 컴퍼넌트의 핵심입니다.   
Mixin은 스타일시트 내내 재사용하고 싶은 style을 정의할 수 있습니다. 
믹스인은 CSS 규칙과 Sass 문서에서 허용되는 거의 모든 것을 포함할 수 있습니다. 
함수처럼 매개변수를 취할 수도 있고, vendor prefixes도 간단히 정의할 수 있습니다.
아래는  border-radius 의 정의 예시입니다.

```SCSS 

/// _style.scss
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}

.box { @include border-radius(10px); }

```
mixin을 사용하려면 먼저 @mixin 지시어를 선언하고 이름을 지정합니다.   
예시에선 border-radius 라고 이름을 지정했습니다.    
자바스크립트 함수 선언문과 흡사한 구조인데, 이름 뒤의 소괄호 안에는 `$radius`라는 변수를 선언했습니다.    

이제 실제 `.box`라는 클래스에 border-radius 속성을 사용하기 위해, 정의한 `border-radiu` mixin을 호출합니다. 호출 할 때는 `@include` 뒤에 지정한 mixin 이름을 선언하고, 소괄호 안에는 값을 선언합니다.
아래는 Css로 변환된 결과값입니다.

```css

/* style.css */
 
.box {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  border-radius: 10px;
}

```
모든것을 mixin으로 선언할수 있다고 해서 과하게 선언하는 것은 좋지않습니다.  선언할때는 20줄 이내로 간단히 정의하길 권장합니다. 예시의 vendor prefixes의 경우에도 _Autoprefixer_를 사용하면 항상 최신 정보를 반영해주고, Sass의 코드를 줄여 줄수 있으니, 수동으로 vendor prefixes를 정의하는것은 권장하지 않습니다. 

[mixin 가이드라인 참조 ](http://sass-guidelin.es/ko/#mixins

### Extend/Inheritance (확장/상속)
`@extend`를 사용하면 미리 정의된 다른 Css 선언을 상속 받을 수 있습니다. 아래는 .success, .error, .warning 등의 메세지 속성을 `@extend`를 사용하여 간단히 정의한 예시입니다. 

```SCSS 

/// _style.scss
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}

.warning {
  @extend .message;
  border-color: yellow;
}

```
이 지시어는 중복해서 선언해야하는 클래스 명을 보다 간결하게 유지할 수 있습니다. 

```css

/* style.css */
 
.message, .success, .error, .warning {
  border: 1px solid #cccccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}

```
이 방식은 알아는 두되, 되도록 사용을 자제하길 권장합니다. 
상속에 상속, 또 그 상속안에 상속을 해버린다거나 하는 경우엔 코드가 되려 더 복잡해질 뿐더러, 본래 지정하고자 했던 속성을 찾아가기 더 어려워질 수도 있습니다. 

또 `@extend`는 `@media` block 안에서는 제대로 작동하지 않습니다. 
[extend 가이드라인 참조 ](http://sass-guidelin.es/ko/#extend)


<br>
### Operators (연산)
Sass에서는 `+, -, *, /, %`등의 수학의 사칙연산과 (>, <, ==, >-, <=, !=)등의 비교연산,  문자접합 등을 지원합니다.
아래는 width값을 알아내기 위한 간단한 연산 예문입니다.

```SCSS 

/// _style.scss
.container { width: 100%; }


article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complimentary"] {
  float: right;
  width: 300px / 960px * 100%;
}

```
연산을 사용하면 모든 계산된 수치값을 지정할 필요 없이 값들을 유동적으로 뽑아낼 수 있습니다.
예시문에서는 `container`클래스의 `width`속성만 변경한다면 `article,aside`의 값도 자동으로 반영되어 변경됩니다.
유지보수가 훨씬 간단해질 수 있습니다.

```css

/* style.css */
 
.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 62.5%;
}

aside[role="complimentary"] {
  float: right;
  width: 31.25%;
}

```






