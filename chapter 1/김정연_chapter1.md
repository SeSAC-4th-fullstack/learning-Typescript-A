[Chapter1] 자바스크립트에서 타입스크립트로 
======================

# 1. 자바스크립트의 역사 
현재 자바스크립트는 ECMA 스크립트의 새로운 버전을 매년 출시하며, 브라우저, 임베디드 애플리케이션 그리고 서버 런타입을 포함한 다양한 환경에서 새로운 버전과 이전 버전과의 호환성을 수십년동안 유지했습니다. 오늘날 자바스크립트는 놀라운 성장을 이뤘습니다.
## 1.1. 바닐라 자바스크립트의 함정
중요한 언어 확장이나 프레임워크 없이 자바스크립트를 사용하는것을 바닐라 라고 부릅니다. 함정을 이해해야하는데, 이러한 약점은 프로젝트 규모가 커질수록 장기화되고 더욱 드러납니다.
### 1.1.1. 값 비싼 자유
자바스크립트를 이용하는 개발자들의 가장 큰 불만은 코드를 구성하는 방법에 제한이 없습니다. 이러한 자유 덕분에 프로젝트를 자바스크립트로 시작하면 재미있습니다. 그러나 파일이 점점 늘어 날수록 얼마나 훼손 될수 있는지 명확해집니다. 다음은 가상의 페인팅 애플리케이션 코드입니다.
```javascript
    function paintPainting(painter,painting) {
        return painter
            .prepare()
            .paint(painting, painter.ownMaterials)
            .finish();
    }
``` 
어떠한 맥락도 없이 코드를 읽게되면, paintPainting 함수를 호출하는 방법에 대해서만 생각할것입니다. 
다른 언어는 컴파일러가 충돌될 수 있다고 판단하면 코드 실행을 거부할 수 있습니다. 하지만, 자바스크립트는 충돌 가능성을 먼저 인지하고 실행하는 동적 타입 언어로 그렇지 않습니다.

### 1.1.2. 부족한 문서
자바스크립트 언어 사양에는 함수의 매개변수, 함수 반환, 변수 또는 다른 구성 요소의 의미를 설명하는 표준화된 내용이 업습니다.
따라서 블록 주석으로 함수와 변수를 설명하는 JSDoc 표준을 선택했습니다.
```javascript
    /**
    * Hello
    * Hi
    */
``` 
JSDoc 은 다음과 같은 곳에서 사용하기 불편합니다.
* 코드가 잘못되는 것을 막을 수 없다.
* 이전에 정확했더라도 코드 리팩토링 중에 생긴 변경 사항과 관련된 유효하지 않은 모든 주석을 찾기란 쉽지 않다. 
* 복잡한 객체를 설명할때는 다루기 어렵고 관계 정의시 다수의 독립형 주석이 필요하다.

### 1.1.3. 부족한 개발자 도구
1. 타입을 식별하는 내장된 방법을 제공하지 않아, 코드가 JSDoc 에서 쉽게 분리되어 대규모 변경을 자동화하거나 통찰력을 얻기 힘들다.
2. C#이나 자바와 같은 타입이 지정된 언어에서 클래스 멤버 이름을 변경하거나 인수의 타입이 선언된 곳에 바로 이동할 수 있는 기능이 있다.

****
## 1.2. 타입스크립트
타입 스크립트는 2010년대에 마이크로소프트에서 만들어져 오픈 소스화 되었습니다.
타입스크립트는 네가지로 설명됩니다.
* 프로그래밍 언어 : 자바스크립트의 모든 구문과 타입을 정의하여 새로운 타입스크립트 고유 구문이 포함된 언어
* 타입 검사기 : 자바스크립트 및 타입스크립트로 작성된 일련의 파일에서 생성된 구성요소(변수, 함수 ...) 를 이해하고 잘못 구성된 부분을 알려주는 프로그램
* 컴파일러 : 타입 검사기를 실행하고 문제를 보고 후 이에 대응하는 자바스크립트 코드를 생성하는 프로그램
* 언어 서비스 : 타입 검사기를 통해 VS Code 와 같은 편집기에 개발자에게 유용한 유틸리티 제공법을 알려주는 프로그램
****
## 1.3. 타입스크립트 시작하기
```javascript
    const firstName = 'Hi';
    const nameLength = firstName.length();
``` 
타입스크립트의 경우 간단한 오류를 미리 편집기에서 알려주므로, 자바스크립트처럼 발생할때까지 기다리는것보다 유용합니다.

### 1.3.1. 제한을 통한 자유
타입스크립트를 시작시 매개변수와 변수에 제공되는 값을 타입으로 지정할 수 있습니다.
개발자는 처음에 특정 영역이 제한적으로 작동하는 방법을 코드에 명시적으로 작성해야한다고 생각합니다. 

### 1.3.2. 정확한 문서화
다음과 같은 코드를 처음읽게 되면, Painter에 적어도 세가지 속성이 있고, 그 중 두가지는 메서드라고 이해합니다. 타입스크립트는 구문을 적용해 객체의 형태를 설명하고 어떻게 보이는지 설명합니다.
```javascript
    interface Painter {
        finish() : boolean;
        ownMaterials : Material[];
        paint(painting: string, materials: Materials[]): boolean;
    }
    fucntion paintPainting(painter: Painter, painting: string): boolean{ /* ... */}
``` 
### 1.3.3. 강력한 개발자 도구
VS Code 에서 문자열 같은 객체의 내장 코드를 작성할때 '자동 완성' 을 제안하는 것을 이미 알고 있습니다. 타입스크립트는 모든 내장 코드를 제안합니다.

### 1.3.4. 구문 컴파일하기
타입스크립트 구문을 입력하면 타입을 검사 후, 작성된 코드에 해당하는 자바스크립트를 내보냅니다. [**타입스크립트 플레이그라운드**](https://www.typescriptlang.org/play?#code/PTAEHUFMBsGMHsC2lQBd5oBYoCoE8AHSAZVgCcBLA1UABWgEM8BzM+AVwDsATAGiwoBnUENANQAd0gAjQRVSQAUCEmYKsTKGYUAbpGF4OY0BoadYKdJMoL+gzAzIoz3UNEiPOofEVKVqAHSKymAAmkYI7NCuqGqcANag8ABmIjQUXrFOKBJMggBcISGgoAC0oACCbvCwDKgU8JkY7p7ehCTkVDQS2E6gnPCxGcwmZqDSTgzxxWWVoASMFmgYkAAeRJTInN3ymj4d-jSCeNsMq-wuoPaOltigAKoASgAywhK7SbGQZIIz5VWCFzSeCrZagNYbChbHaxUDcCjJZLfSDbExIAgUdxkUBIursJzCFJtXydajBBCcQQ0MwAUVWDEQC0gADVHBQGNJ3KAALygABEAAkYNAMOB4GRonzFBTBPB3AERcwABS0+mM9ysygc9wASmCKhwzQ8ZC8iHFzmB7BoXzcZmY7AYzEg-Fg0HUiQ58D0Ii8fLpDKZgj5SWxfPADlQAHJhAA5SASPlBFQAeS+ZHegmdWkgR1QjgUrmkeFATjNOmGWH0KAQiGhwkuNok4uiIgMHGxCyYrA4PCCJSAA) 를 사용하세요.
****
## 1.4. 로컬에서 시작하기
* 타입스크립트 설치
```javascript
    npm i -g typescript
``` 
* 버전 확인
```javascript
    tsc --version
```
* 시작 (tsconfig.json)
```javascript
    tsc --init
```
* index 파일 추가
```javascript
    tsc index.ts
```

### 1.4.1 편집기 기능
tsconfig.json 파일 생성시 편집기가 해당 폴더를 타입스크립트 프로젝트로 인식합니다. 예를 들어 VSCode에서 폴더를 열면 타입스크립트 코드를 분석하는데 tsconfig.json을 따르게됩니다.
****
## 1.5. 타입스크립트의 오해
1. 잘못된 코드 해결책
    * 타입스크립트는 자바스크립트 코드를 구조화하는데 도움이 되지만, 타입 안정성 강화를 제외하고 해당 구조가 어떻게 보여야하는지 강요하지 않는다.(좋은 특징!)
    * 타입스크립트는 사용했던 아키텍처 패턴 중 무엇을 사용해도 지원한다. 
    * 특정 라이브러리나 프레임워크와도 연관이 없다.
2. 자바스크립트의 확장
    * 타입스크립트의 설계목표
        * 현재와 미래의 ECMA스크립트 제안에 맞춤
        * 모든 자바스크립트 코드의 런타임 동작을 유지
3. 자바스크립트보다 느림
    * 타입스크립트는 코드를 빌드하는데 시간이 조금 걸린다.
    * Node.js 와 같은 환경에서 실행되기 전에 자바스크립트로 컴파일 되어야한다.
    * 빌드 파이프라인은 대부분 성능 저하를 무시하도록 설정한다.
4. 진화가 끝남
    * 웹 커뮤니티는 타입스크립트에 버그 수정과 기능 추가를 지속적으로 요구한다.