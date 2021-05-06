---
title: "[React] CSS Module"
date: 2021-1-21 22:17:00 +0900
categories:
  - react
toc: true
classes: wide
---

# CSS Module

CSS Module은 CSS를 불러와서 사용할 때 클래스 이름을 고유한 값, 즉 `[파일 이름]_[클래스 이름]__[해시값]` 형태로 자동으로 만들어서, 컴포넌트 스타일 클래스 이름이 중첩되는 현상을 방지해 주는 기술입니다. 확장자를 **.module.css** 로 지정하기만 하면 알아서 CSS Module이 적용됩니다.

```css
/* CSSModule.module.css */

.wrapper {
  background: black;
  padding: 1rem;
  color: white;
  font-size: 2rem;
}

:global .something {
  font-weight: 800;
  color: aqua;
}
```

<br>

.module.css 파일의 각 클래스는 파일을 불러오는 컴포넌트 내부에서만 작동하기 때문에 클래스 이름을 지을 때 고유성에 대해 고민하지 않아도 됩니다. 또한 `:global` 을 사용하면 해당 클래스를 웹 페이지 전역에서 사용할 수 있습니다.

```jsx
// CSSModule.js

import React from "react";
import styles from "./CSSModule.module.css";

const CSSModule = () => {
  console.log(styles);

  return (
    <div className={styles.wrapper}>
      <span className="something">CSS Module!</span> 테스트 입니다.
    </div>
  );
};

export default CSSModule;
```

CSSModule이 적용된 스타일 파일을 불러오면 객체 하나를 전달받습니다. 여기선 styles라는 이름으로 받았습니다. 이 객체에는 파일에서 사용된 클래스 이름과 해당 이름을 고유화한 값이 키-값 형태로 들어 있습니다. styles 객체를 로그에 찍어보면 다음과 같이 나옵니다.

![http://dl.dropbox.com/s/9up7ploxvafpklb/React-CSS%20Module-1.png](http://dl.dropbox.com/s/9up7ploxvafpklb/React-CSS%20Module-1.png)

작성된 각각의 클래스들에 `[파일 이름]_[클래스 이름]__[해시값]` 의 형태로 값이 주어지는 것을 확인할 수 있습니다. 고유 클래스 이름을 사용하려면 중괄호를 이용해 { styles.wrapper } 와 같이 작성해주고, 전역으로 선언된 클래스 이름은 문자열로 지정해줍니다.

<br>

두 개 이상의 클래스 명을 주고 싶을 땐 다음과 같이 사용합니다.

```jsx
<div className={`${styles.wrapper} ${styles.inverted}`}><div>
```

<br>

# classnames 라이브러리

classnames는 CSS 클래스를 **조건부**로 설정하거나 엘리먼트에 **여러 클래스를 적용**할 때 유용합니다.

먼저 yarn을 통해 라이브러리를 설치합니다.

```bash
$ yarn add classnames
```

<br>

classnames 사용법은 다음과 같습니다.

```jsx
import classNames from "classnames";

classNames("one", "two"); // 'one two'
classNames("one", { two: true }); // 'one two'
classNames("one", { two: false }); // 'one'
classNames("one", ["two", "three"]); // 'one two three'

const myClass = "hello";
classNames("one", myClass, { myCondition: true });
("one hello myCondition");
```

<br>

classnames 라이브러리에 있는 `bind` 함수를 사용하면, 클래스를 넣어줄 때마다 **styles.[클래스 이름]** 형태를 사용할 필요가 없습니다. 미리 styles에서 받아온 후 bind 함수를 거쳐 변수에 저장해두고, 변수명을 통해 다음과 같은 **cx('클래스 이름1', '클래스 이름2')** 형태로 사용할 수 있습니다. 아래는 bind를 사용한 예시입니다. 불러오는 파일의 path가 classnames가 아닌 `classnames/bind` 인 점을 유의해야 합니다.

```jsx
import React from "react";
import styles from "./CSSModule.module.css";
import classNames from "classnames/bind";

const cx = classNames.bind(styles);

const CSSModule = () => {
  console.log(styles);

  return (
    <div className={cx("wrapper")}>
      {/* <div className={styles.wrapper}> */}
      <span className="something">CSS Module!</span> 테스트 입니다.
    </div>
  );
};

export default CSSModule;
```

<br>

css 뿐만 아니라 scss 확장자를 갖는 파일도 앞에 .module을 붙이면 CSS Module을 사용할 수 있습니다.

출처 - [리액트를 다루는 기술](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791160508796&orderClick=LEa&Kc=)