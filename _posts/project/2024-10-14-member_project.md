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


<br>
**참고 자료**

---

- https://www.youtube.com/watch?v=RhM1bQ76Tv0&list=PLV9zd3otBRt5ANIjawvd-el3QU594wyx7
- [https://commnetall.tistory.com/106 - Intellij](https://commnetall.tistory.com/106)
- [https://velog.io/@codingrecipe/SpringBoot-Spring-Initializr로-프로젝트-만들기](https://velog.io/@codingrecipe/SpringBoot-Spring-Initializr%EB%A1%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0)
- [https://velog.io/@codingrecipe/mysql-설치하기](https://velog.io/@codingrecipe/mysql-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)
