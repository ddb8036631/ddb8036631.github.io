---
title: "[모던 JavaScript 튜토리얼] 1. 엄격 모드(use strict)"
date: 2021-4-6 02:43:00 +0900
categories:
  - 모던 JavaScript 튜토리얼
classes: wide
---

ECMAScript5(ES5)가 2009년에 등장했다. ES5에서는 새로운 기능이 추가되고, 기능 중 일부가 변경됐다.

기존 기능을 변경했기 때문에, 하위 호환성 문제가 발생했다. `use strict` 라는 특별한 지시자를 사용해 엄격 모드(strict mode)를 활성화 했을 때만 이 변경사항이 활성화되게 했다.

# use strict

```jsx
"use strict";

```

js 파일 내 최상단에 위의 문구를 적으면, 파일의 코드엔 ES5 이상의 기능이 사용된다.

ES6 이상의 `모듈` 과 `클래스` 에는 use strict 가 자동으로 적용된다. 따라서, 파일 내에서 클래스와 모듈을 사용한다면, 따로 적을 필요가 없다.
