# Sass(Syntactically Awesome StyleSheets)

Sass는 CSS 전처리기(pre-processor)로 프로젝트의 규모가 커질수록 코드의 재활용 및 가독성이 떨어지는 CSS의 문제를 해결하여 유지보수를 쉽게 할 수 있도록 여러 기능을 제공한다.

Sass는 SASS(.sass) 표기법과 SCSS(.scss) 표기법이 존재한다. 이 문서는 CSS 친화적인 SCSS 표기법을 기준으로 정리한다.

브라우저는 Sass 문법을 알지 못하기 때문에 Sass 파일을 CSS 파일로 트랜스파일링해야 하기 때문에 아래 패키지 설치가 필요하다.

```
npm i sass
```

Sass의 다양한 기능들에 대해 살펴보자.

## 변수(Variables)

Sass에서는 변수를 사용해 스타일을 저장하고 재사용할 수 있다.

변수는 $ 기호를 사용해 작성하며 스코프가 존재한다.

```scss
$primary-color: #333;

body {
  color: $primary-color;
  p {
    $point-color: #acefff;
  }
  section {
    color: $point-color; // Error
  }
}
```

<br>

## 중첩(Nesting)

Sass의 중첩 기능을 사용하면 HTML의 계층 구조를 반영한 CSS 작성이 가능하다.

& 기호를 사용해 부모 요소의 참조가 가능하다.

```scss
nav {
  ul {
    margin: 0;
    padding: 0;
  }

  li {
    display: inline-block;
  }
  a {
    font-weight: bold;
    text-decoration: none;
    &:hover {
      color: red;
    }
  }
}
```

<br>

## 모듈(Module)과 분할(Partial)

다른 Sass파일에 포함될 수 있는 작은 파일로 분할하는 것을 partial이라 하며 partial된 파일명은 \_ 기호를 붙인다. \_ 기호는 Sass에게 CSS로 트랜스파일링 하지 않도록 알려준다.

@use를 사용해 partial된 파일을 모듈로 불러올 수 있다. 이때 \_ 기호와 확장자는 생략 가능하다.

불러온 Sass 파일의 변수, 믹스인, 함수를 참조할 수 있다.

```scss
// _base.scss
$primary-color: #333;

body {
  color: $primary-color;
}

// style.scss
@use 'base';

.description {
  color: base.$primary-color; // 변수 참조
}
```

<br>

## 믹스인(Mixin)

믹스인은 재사용되는 CSS를 그룹화 할 수 있는 기능이다.

@mixin으로 선언하고 @include로 불러들일 수 있다.

매개변수를 통해 값의 전달 또한 가능하다.

```scss
@mixin circle {
  width: 50px;
  height: 50px;
  border-radius: 50%;
}

.box {
  @include circle;
  background-color: #acefff;
}

// 다음과 같이 값을 전달할 수도 있다.
@mixin circle($size) {
  width: $size;
  height: $size;
}

.box {
  @include circle(100px);
}
```

<br>

## 함수(Function)

함수는 믹스인과 유사하지만 믹스인은 CSS를 반환하고 함수는 @return을 통해 값을 반환한다는 차이가 있다. @function을 사용해 작성한다.

```scss
$primary-width: 100px;

@function box-width($n) {
  @return @n * $primary-width;
}

.box {
  width: box-width(2); // 200px
}
```

<br>

## 참고 자료

https://sass-lang.com/guide
https://poiemaweb.com/sass-basics
