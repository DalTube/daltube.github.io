---
title: ES6
date: 2024-11-20 00:00:00
categories: [javascript, ES6]
tags:
  [
    javascript
  ]
---

# ES6
---
ES6(ECMAScript 6)는 2015년에 도입된 자바스크립트의 6번째 표준안이다. 현대적인 코드를 사용하면, 코드가 간결해지고 생산성이 향상된다. 이것이 ES6를 사용해야 하는 이유다.

#### **Module**
---
**Javascript를 파일 단위로 분리된 코드 덩어리**를 일컫습니다. 여기서 Javascript파일은 **특정한 기능**을 가진 여러 개의 **함수**와 **변수**들의 **집합체**입니다.

#### **필요성***
---
1. 코드 베이스를 분리할 수 있으며, 이를 통해 코드를 구조적으로 관리
2. 코드를 재사용 가능. 모듈화(modularize)
3. 코드의 함수와 변수중 일부만 외부에서 사용하도록 노출
4. 해당 모듈이 참조하고 있는 다른 모듈에 대한 종속성(Dependency)을 관리하는 역할

#### **ES6 Module 사용**
---
1. 화살표 함수 
```javascript
/** 화살표 함수 export **/

// 모듈을 호출했을 때, addArrowFunction 키 값에는 addArrowFunction 변수 함수가 가지고 있는 값이 할당된다.
export const addArrowFunction = (a, b) => {
  return a + b;
}

/** 화살표 함수 import **/

import { addArrowFunction } from './math.js'

console.log(addArrowFunction(5, 3));

// Print: 8
```

2. 익명 함수
```javascript
/** 익명 함수 export **/

// 모듈을 호출했을 때, addAnonymousFunction 키 값에는 (a,b){return a + b;} 익명 함수가 할당된다.
export const addAnonymousFunction = function (a, b) {
  return a + b;
}

/** 익명 함수 import **/

import { addAnonymousFunction } from './math.js'

console.log(addAnonymousFunction(9, 3));

// Print: 12
```

3. export default Object
```javascript
/** export default Object **/

// 모듈을 호출했을 때, add 키 값에는 add 함수가 들어가는 방법이다.
const defaultObject = {
  add: add,
}

export default defaultObject;

/** import default Object, 모듈 전체 가져오기 **/

import * as math from './math.js'

console.log(math.default.add(13, 8));

// Print: 21

/** import default Object, 모듈 전체 가져오기 **/

import { default as defaultObject } from './math.js'

console.log(defaultObject.add(17, 2));

// Print: 19
```

4. export default Function

```javascript
/** export default Function **/

// 모듈을 호출했을 때, defaultAddFunction 함수가 들어가는 방법이다.
const defaultAddFunction = function (a, b) {
  return a + b;
}

export default defaultAddFunction;

/** import default Function, 모듈 전체 가져오기 **/
import * as math from './math.js'

console.log(math.default(20, 11));

// Print: 31


/** import default Function, default 모듈만 가져오기 **/
import { default as defaultAddFunction } from './math.js'

console.log(defaultAddFunction(12, 16));

// Print: 28
```

5. export default 익명 함수
```javascript
/** 익명 함수 export default **/

// 모듈을 호출했을 때, 익명 함수가 반환되는 방법이다.
export default function (a, b) {
  return a + b;
}

/** default 익명 함수 import **/

import math from './math.js'

console.log(math(2, 1));

// Print: 3
```