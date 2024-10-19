---
title: "회원 프로젝트(springboot, mysql, jpa, login)"
categories: [Study, Project]
---

# 01_Introduction

- **개발 환경**
    1. Open JDK 11(Amazon Corretto 11) 
    2. IntelliJ IDEA Community(무료버전)
    3. MySQL Server 8.0. - DB 연동, CRUD
    4. Spring Boot 2.6.12 - 메인
- **Library or Framework**
    1. Spring Data JPA - DB 작업
    2. Thymeleaf - 화면 구성
    3. lombok
    4. gradle - 빌드, 라이브러리

![사진](/assets/img/project/member/1.png)

- Metadata 부분은 프로젝트의 패키지를 설정하는 부분
- Packaging은 Jar, Java버전은 11로 선택
- Dependency는 프로젝트에 필요한 라이브러리 등을 선택하는 부분.
    - DB연동 없는 프로젝트를 만들기 위한 최소 DependencyLombokSpring WebThymeleaf
    - DB 연동하는 경우 추가할 dependencySpring Data JPA(Spring Data JPA를 쓰는 경우)MySQL Driver(mysql인 경우)

![사진](/assets/img/project/member/2.png)

- 프로젝트 Open 완료 후 화면
    - 모든 설정 완료 후 GENERATE를 클릭하면 프로젝트 파일이 .zip 파일로 다운로드 됨
    - 다운로드 된 파일 압축을 해제하고 intelliJ Community 실행, Open 클릭
    - 압축을 풀어놓은 폴더에서 build.gradle 파일 선택 후 open 클릭
    - Open as Project 클릭 후 다른 창이 출력되면 Trust Project 클릭 하면 작업창이 열림

# **03_application.yml 설정하기**

![사진](/assets/img/project/member/3.png)

- 스프링부트는 서버가 돌아가야 웹서비스를 구현할 수 있음. 서버는 기본적으로 내장되어 있는 톰캣(라이브러리를 포함함)으로 설정
- src/main/esources/application.properties 삭제하고 resources 선택 후 New → File 클릭, application.yml 생성

# **04_기본 주소 요청하기**

**HomeController.java**

```java
package com.codingrecipe.member.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {
    // 기본페이지 요청 메서드
    @GetMapping("/") // 기본 주소 요청이 오면 메서드 호출
    public String index() {
        return "index"; // 기본 요청이 왔을 때 인덱스 html 파일을 브라우저에 띄움
                        // => templates 폴더의 index.html을 view resolver가 찾아감
    }
}
```

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
    <h2>Hello Spring Boot!!</h2>
</body>
</html>
```

![사진](/assets/img/project/member/4.png)

![사진](/assets/img/project/member/5.png)

- 구글이나 네이버처럼 도메인 주소가 있는 상태가 아니기 때문에 개발하고 있는 pc가 서버가 됨
    - 톰캣의 포트번호를이 application.yml 파일에서 8081 설정. 이 정보를 가지고 우리의 서버로 접속을 한다는 개념임
    - 다른 pc에서 이 주소를 친다고 해서 뜨는 건 아님. 지금 현재 개발하고 있는 pc에서 접속이 가능함
- http://localhost:8081/ 기본 주소가 요청이 됐다고 보면 됨. 슬래시 기본 주소는 보통 주소값에 보이지는 않음
    - http://localhost:8081 여기까지가 우리 서버를 가리킨다고 보면 되고 이제 그 뒤에 특정 주소가 붙음
    기본 주소가 요청이 됐을 때 이 메서드가 호출이 되면서 인덱스 html을 브라우저에 띄워 주도록 하는 것임
    - 브라우저에 index 요걸 보여주겠다 그걸 이제 스프링이 중간에서 처리를 다 해주는 것. 그래서 이 인덱스 html에는 헬로 스프링부트라는 내용을 출력하게끔 해 놓은 것. 그래서 우리 눈에는 주소로 접속을 했더니 이 내용이 눈에 보여짐
    - 스프링 부트 프로젝트를 만들어서 기본 주소를 요청하는 메서드를 생성. 기본 주소 요청에 대해서 인덱스 html을 띄워줬고 그 인덱스 html은 어떤 내용을 썼느냐 그게 바로 브라우저에 보여짐

# **05_회원가입_회원가입 페이지 요청하기**

![사진](/assets/img/project/member/6.png)

**Index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
    <h2>Hello Spring Boot!!</h2>
    <a href="/member/save">회원가입</a> <!-- 링크를 클릭하는 것은 전부 http 메서드 중 get 메서드 -->
    <a href="/member/login">로그인</a>
</body>
</html>
```

- 모든 시작은 인덱스 html에서 시작됨
- 링크를 클릭하면 그 링크가 컨트롤러로 우리의 서버로 요청이 되고 우리 서버에 이제 컨트롤러에서 그 요청에 해당하는 페이지를 띄움. 회원가입 페이지를 요청하면 회원가입 페이지를 브라우저에 띄워주고 로그인 페이지를 요청하면 로그인 페이지를 띄움

**MemberController.java**

```java
package com.codingrecipe.member.controller;

import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller // 스프링 빈으로 등록
public class MemberController {
    // 회원가입 페이지 출력 요청
    @GetMapping("/member/save") // 링크 클릭은 http 메서드 중 get 메서드
    public String saveForm() {
        return "save"; // 작성명은 스프링이 templates 폴더에서 save 이름을 찾는다.
    }
}

```

- html에서 뭔가를 하면 보통 클라이언트의 요청이 발생했다라고 얘기를 할 수 있는데 요청이 발생을 하면 가장 먼저 받아주는 부분은 컨트롤러라고 생각을 하면 됨. 꼭 홈 컨트롤러일 필요는 없고 여러가지 컨트롤러가 있을 수 있음
- 그리고 그 중간에는 서블릿이라든지 프론트 컨트롤러라든지 그런 것들이 우리 눈에는 보이진 않지만 그런 것들이 다 스프링에 의해서 다 각자의 어떤 역할을 하고 있음
- 우리가 회원가입할 때 작성하는 내용은 컨트롤러 그러니까 결국 서버로 요청이 가야 됨. 우리가 회원가입을 하면 그 정보로 그 해당 서비스의 데이터베이스의 그 정보가 저장된다는 그런 원리라고 보면 됨

**save.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>save</title>
</head>
<body>
<!-- action 속성: form에 작성한 데이터를 어디로 보낼지 지정 -->
<form action="/action/save" method="post"> <!-- 주소 -->
    <!-- name 속성: 서버로 전송할 때 변수이름의 역할. 사용자 작성 내용은 이 변수에 담긴다. -->
    이메일: <input type="text" name="memberEmail"> <br>
    비밀번호: <input type="text" name="memberPassword"> <br>
    이름: <input type="text" name="memberName"> <br>
    <input type="submit" value="회원가입">
</form>
</body>
</html>
```

- 폼 안에서 어떤 정보를 어디론가 보낼 때 사용자의 어떤 입력을 받고자 하는 그 부분은 다 인풋 태그가 사용됨필수적으로 적어줘야하는 name 속성은 서버로 쉽게 표현하자면 이게 변수 이름이 되고, 사용자가 작성하는 값은 이 변수에 담겨서 남긴다.
- 서브밋이라는 속성 값을 주고 밸류를 주게 되면 태그는 버튼처럼 보이고, 버튼을 클릭하면 이메일, 비밀번호, 이름에 작성한 값을 가지고 action 주소로 post 방식으로 간다.

![사진](/assets/img/project/member/7.png)

![사진](/assets/img/project/member/8.png)

![사진](/assets/img/project/member/9.png)

![사진](/assets/img/project/member/10.png)

- 세이브 html에서 작성한 값을 받아주는 메서드가 아직은 없어 에러가 나는 것
- action 주소로 포스트로 보내는데 컨트롤러에서 지금 현재 주소는 존재하나, 겟으로 받는 부분만 존재함
- 회원가입 페이지(`/member/save`)를 띄워주는 메서드만 존재함. 주소는 있으나, 받는 방식

![사진](/assets/img/project/member/11.png)

- 주소는 있는데 방식이 다른 거 그런 문제 때문에 생기는 에러. 그래서 스테이터스 코드가 405라는 코드가 발생을 했음
- 주소가 없다면 404라는 주소가 뜸. 우리 컨트롤러에는 멤버의 로그인이라는 주소를 처리하는 메서드는 없기 때문에 404라는 에러가 뜸


<br>
**참고 자료**

---

- https://www.youtube.com/watch?v=RhM1bQ76Tv0&list=PLV9zd3otBRt5ANIjawvd-el3QU594wyx7
- [https://commnetall.tistory.com/106 - Intellij](https://commnetall.tistory.com/106)
- [https://velog.io/@codingrecipe/SpringBoot-Spring-Initializr로-프로젝트-만들기](https://velog.io/@codingrecipe/SpringBoot-Spring-Initializr%EB%A1%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0)
- [https://velog.io/@codingrecipe/mysql-설치하기](https://velog.io/@codingrecipe/mysql-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)
