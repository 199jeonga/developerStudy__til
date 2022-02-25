# css, scss, sass

npm start를 했을 떄 실제 import가 어떻게 진행될까?

컴포넌트별로 style파일이 관리되는 것(스코핑)이 아니기 때문에 className관리에 신경을 써야 한다.

- BEM방식 ( block - element - modifier)

하지만 sass 를 사용하면 css의 수동적인 className작성의 단점을 극복할 수 있다. 그 전에 모듈을 설치해야함! → `npm i sass`