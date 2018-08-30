# SASS compile 방법

## 설치

> npm install -g node-sass

## Compile 방법

* Syntax

> node-sass [option] &lt;inputFilePath> [outpuFilePath]

* 예제 - 출력경로하 하나일 때

> node-sass scss/main.scss css/main.css

* 예제 - 출력 경로가 여럿일 때

> node-sass scss/main.scss css/main.css public/style.css

* --watch or -w 를 사용하여 런타임 중 파일을 감시하여 저장 시 자동으로 변경 사항을 컴파일한다.

> node-sass --watch scss/main.css css/main.css

* **VSCode 사용자**는 extension인 'live sass compiler'를 설치하면 위의 '--watch' option을 쓸 필요없이 지정한 경로에 자동으로 .css 를 update해 준다.