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

# **06_회원가입_입력한 정보 컨트롤러로 전달하기**

![사진](/assets/img/project/member/12.png)

![사진](/assets/img/project/member/13.png)

- 메서드가 호출되면서 콘솔에 찍힌 프린트 문으로 요청이 제대로 가고있음
- save.html에서 입력한 값을 MemberController.java에서 써야 DB로 전달을 해줄 수 있음

![사진](/assets/img/project/member/14.png)

- @RequestParam을 이용해서 “”에 name 값을 적어준다. “”에 담겨온 값을 String memberEmail에 옮겨담는다.
    - 네임 속성은 서버로 전송할 때 변수 이름과 같은 역할을 한다. save.html에서 보냈으니까 MemberController.java 입장에서는 받아야 한다. 그냥 받는 것이 아니라 이제 전달해
    온 걸 받는다는 개념

![사진](/assets/img/project/member/15.png)

- 메서드 호출됐고, save.html에서 입력한 aaa라는 값이 컨트롤러로 잘 넘어오는게 확인이 된다.

**MemberController.java**

```java
package com.codingrecipe.member.controller;

import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller // 스프링 빈으로 등록
public class MemberController {
    // 회원가입 페이지 출력 요청
    @GetMapping("/member/save") // 링크 클릭은 http 메서드 중 get 메서드
    public String saveForm() {
        return "save"; // 작성명은 스프링이 templates 폴더에서 save 이름을 찾는다.
    }

    @PostMapping("/member/save")
    public String save(@RequestParam("memberEmail") String memberEmail,
                       @RequestParam("memberPassword") String memberPassword,
                       @RequestParam("memberName") String memberName) {
        System.out.println("MemberController.save"); // soutm: 현재 메서드를 프린트 문으로 작성. 메서드 호출 확인
        System.out.println("memberEmail = " + memberEmail + ", memberPassword = " + memberPassword + ", memberName = " + memberName);
                                                     // soutp: 매개변수를 자동완성으로 프린트 문으로 만들어주는 단축키
        return "index";
    }
}

```

# **07_회원가입_DB 연동하기**

![사진](/assets/img/project/member/16.png)

![사진](/assets/img/project/member/17.png)

- member 패키지에서 service, repository, dto, entity 패키지 생성
- 컨트롤러는 컨트롤러끼리 서비스는 서비스끼리 이렇게 모아 놓는 것이 일반적인 코딩 방식이기 때문에 패키지를 구분을 해서 사용

**memberDTO.java**

```java
package com.codingrecipe.member.dto;

import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Getter // 롬복 라이브러리가 제공해주는 어노테이션
@Setter
@NoArgsConstructor
@ToString
public class MemberDTO {
    private Long id;
    private String memberEmail;
    private String memeberPassword;
    private String memberName;
}

```

- dto라는 클래스는 이제 회원 정보에 필요한 내용들을 필드로 정의를 할 것. 그 필드에 대해서 보통 일반적으로 코딩하는 규칙은 이제 이 클래스의 필드를 모두 다 프라이빗으로 감싸 감춘다라고 많이 표현을 함
- 그래서 이 필드를 사용하기 위해서는 게터 또는 세터 이런 메서드를 이용해서 간접적으로 필드 값을 사용하도록 함
- 보통은 필드를 선언하고 우리가 게터, 세터를 각각에 대해서 만들어 줘야 되는데, 이 롬복이라는 라이브러리는 어노테이션만 하나 붙여줌으로 인해서 이 각각의 필드에 대한 게터, 세터를 자동으로 만들어 주는 역할을 함
- NoArgsConstructor라는 것은 기본 생성자를 자동으로 만들어주는 기능이 있음. 또 비슷한 것으로 AllArgsConstructor는 필드를 모두 다 매개변수로 하는 생성자를 만들어줌
- dto 객체가 가지고 있는 어떤 필드 값을 출력할 때 보통 toString을 많이 사용을 하는데 toString 메서드를 자동으로 만들어주는 롬복 어노테이션임
- 이렇게 어노테이션만 잘 붙여주고 그 다음에 필드를 선언을 해주면 필요한 것들이 다 생긴다 이렇게 생각을 하면 됨

**MemberDTO.java**

```java
package com.codingrecipe.member.controller;

import com.codingrecipe.member.dto.MemberDTO;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller // 스프링 빈으로 등록
public class MemberController {
    // 회원가입 페이지 출력 요청
    @GetMapping("/member/save") // 링크 클릭은 http 메서드 중 get 메서드
    public String saveForm() {
        return "save"; // 작성명은 스프링이 templates 폴더에서 save 이름을 찾는다.
    }

    @PostMapping("/member/save")
    public String save(**@ModelAttribute MemberDTO MemberDTO**) { // 스프링이 제공해주는 어노테이션
        System.out.println("MemberController.save"); // soutm: 현재 메서드를 프린트 문으로 작성. 메서드 호출 확인
        **System.out.println("MemberDTO = " + MemberDTO);** // soutp: 매개변수를 자동완성으로 프린트 문으로 만들어주는 단축키
        return "index";
    }
}

```

- 그래서 다시 컨트롤러(MemberController)로 돌아오면 우리가 각각의 파라미터를 String 변수(String memberEmail, String memberPassword, String memberName)로 받아서 서비스 클래스로 넘겨주고 서비스 클래스에서는 레파지토리로 넘겨주고 레파지토리에서는 데이터베이스로 넘기는 그런 작업을 해야만 우리의 웹페이지의 회원가입 페이지에서 사용자가 입력한 그 회원 정보가 데이터베이스로 저장이 된다 그런 과정을 지금 진행 중에 있음

![사진](/assets/img/project/member/18.png)

- dto의 이메일, 패스워,드 네임이 필드 값이 잘 담겨서 옴. 이제 이게 가능한 이유는 중간에 스프링이 우리가 매개변수로 dto클래스 타입으로 매개변수를 정해주면 네임 속성과  dto의 필드가 동일하다면 알아서 스프링이 dto의 세터 메서드들을 각각 호출하면서 작성한 값을 알아서 담아줌
- ModelAttribute도 생략은 가능함. 오히려 명시적으로 이것을 썼다는 것이 더 보기도 쉽고 ModelAttribute가 갖는 또 여러 가지 기능들이 있어 쓰는 게 좋음
- 파라미터가 많아질수록 코드 양이라든지,그리고 또 많아지면 분명히 놓치는 것도 생기기도 하고 그렇기 때문에 훨씬 더 좋음
- dto 객체에 save.html에서 작성한 값을 제대로 전달을 받고 있음

```java
// 예시
MemberService memberService = new MemberService();
memberService.save();
```

- 전달 받은 값을 서비스클래스, 이제 계속 DB까지 전달하는 그 과정을 처리를 해야 함
MemberService 객체를 하나 만들어서 그 MemberService가 가지고 있는 어떤 특정 메서드들을 이렇게 호출하는 방식을 스프링에서는 이런 식의 객체 생성 방식을 잘 쓰지 않는다.
- 아예 안 쓰는 건 아니지만, 이런 컨트롤러 클래스에서 서비스 클래스로 뭔가를 넘길 때 메서드를 호출할 때는 @Controller, @Service, @Repository 이런 거를 붙여 준다는게 스프링이 관리하는 객체로 등록을 한다고 했었는데,
- 스프링이 관리하는 객체를 편하게 쓸 수 있는 방법이 있음. 그래서 그거를 객체를 주입 받는다라고 하는데, 객체 주입 방식은 크게 3가지가 있고 여기서는 생성자 주입이라는 방식을 사용을 하려고 함

**MemberController.java**

```java
package com.codingrecipe.member.controller;

import com.codingrecipe.member.dto.MemberDTO;
import com.codingrecipe.member.service.MemberService;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller // 스프링 빈으로 등록
@RequiredArgsConstructor // 롬복이 제공하는 어노테이션
public class MemberController {
    // 생성자 주입
    **private final MemberService memberService;** // 이 필드를 매개변수로 하는 컨트롤러(MemberController) 생성자를 @@RequiredArgsConstructor가 만들어줌
                                               // 이 클래스(MemberController)에 대한 객체를 스프링 빈으로 등록을 할 때 자동적으로 이 서비스 클래스(MemberService)에 대한 객체를 주입 받는다.

    // 회원가입 페이지 출력 요청
    @GetMapping("/member/save") // 링크 클릭은 http 메서드 중 get 메서드
    public String saveForm() {
        return "save"; // 작성명은 스프링이 templates 폴더에서 save 이름을 찾는다.
    }

    @PostMapping("/member/save")
    public String save(@ModelAttribute MemberDTO MemberDTO) { // 스프링이 제공해주는 어노테이션
        System.out.println("MemberController.save"); // soutm: 현재 메서드를 프린트 문으로 작성. 메서드 호출 확인
        System.out.println("MemberDTO = " + MemberDTO); // soutp: 매개변수를 자동완성으로 프린트 문으로 만들어주는 단축키
        **memberService.save(MemberDTO);**
        return "index";
    }
}

```

- 주입을 받는다라는 건 컨트롤러 클래스가 서비스 클래스의 자원. 즉, 메서드나 필드나 이런 것들을 사용할
수 있게 되는 권한이 생긴다라고 생각을 하면 됨

![사진](/assets/img/project/member/19.png)

- 클릭을 해보면 서비스 클래스에 세이브라는 메서드가 아까 우리가 호출했던 방식을 따르는 형태로 만들어진다.

**MemberService.java**

```java
package com.codingrecipe.member.service;

import com.codingrecipe.member.dto.MemberDTO;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

@Service // 스프링이 관리해주는 객체 즉, 스프링 빈으로 등록을 시킴
**@RequiredArgsConstructor**
public class MemberService {
    **private final MemberRepository memberRepository;** 

    **public void save(MemberDTO memberDTO) {
    }**
}

```

- 그러면 서비스 클래스에서는 리파지토리 클래스를 호출을 해야 되는데 마찬가지로 똑같은 방식을 사용한다. @RequiredArgsConstructor를 쓰고,  프라이빗 파이널 멤버 지금 멤버 리파지토리를 아직 많이 들지 않아서 빨간색이 뜬다.
- 리파지토리를 만들기 전에 작업을 해야 될 부분이 있다.

**build.gradle**

```groovy
plugins {
	id 'java'
	id 'org.springframework.boot' version '3.3.4'
	id 'io.spring.dependency-management' version '1.1.6'
}

group = 'com.codingrecipe'
version = '0.0.1-SNAPSHOT'

java {
	toolchain {
		languageVersion = JavaLanguageVersion.of(17)
	}
}

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'

	**implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	runtimeOnly 'mysql:mysql-connector-java'**

	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

tasks.named('test') {
	useJUnitPlatform()
}

```

- 스프링 데이터 jpa라는 디펜던시와 mysql이라는 디펜던시 두 가지를 추가를 할 것이다. 새로운 디펜던시를 추가를 하게 되면 인텔리제이에서는 코끼리 모양 아이콘이 하나가 뜬다. 반드시 클릭해서 라이브러리를 업데이트를 해야한다. 그래야 우리가 사용하고자 하는 디펜던시에 대한 라이브러리를 이 gradle이 다운로드를 받아서 우리가 라이브러리를 사용을 할 수 있게 세팅을 해준다.
- db를 사용해야 되기 때문에 이제 곧 db와 관계된 세팅이 좀 추가가 된다라고 보면 된다.

```yaml
server:
  port: 8081

# database 연동 설정
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    #    각자 PC에 만들어놓은 Database이름을 써야 한다.
    url: jdbc:mysql://localhost:3306/**db_codingrecipe**?serverTimezone=Asia/Seoul&characterEncoding=UTF-8
    #    mysql에 생성한 사용자 계정 정보를 써야 한다.
    username: **user_codingrecipe**
    password: **1234**
  thymeleaf:
    cache: false

  # spring data jpa 설정
  jpa:
    database-platform: org.hibernate.dialect.MySQL5InnoDBDialect
    open-in-view: false
    show-sql: true  # 콘솔 창에서 메시지들을 처리되는 과정들을 본다.
    hibernate:
      ddl-auto: update
```

- appplication.yaml를 이제 조금 더 써야 한다. 처음에 이 부분이 db와 관계된 부분을 할 때 좀 사용이 될거다라고 말했는데, 이 부분도 일단 각자 PC 그 mysql이 분명히 깔려 있어야 되고, mysql에서 데이터베이스를 생성을 해야 하는데 그 데이터베이스 이름을 여기(db_codingrecipe) 쓴다.
- username과 password는 mysql에 생성한 사용자 계정 정보를 써야 한다.

```sql
// 데이터베이스 생성 명령어 create database 데이터베이스이름;
create database db_codingrecipe;

// 사용자 생성 명령어
create user user_codingrecipe@localhost identified by '1234';
grant all privileges on db_codingrecipe.* to user_codingrecipe@localhost;
```

- mysql를 설치하고 루트 계정으로 접속을 해서 데이터베이스도 만들고, 사용자도 만들고 이런 작업들을 할 수가 있다. 데이터베이스를 만드는 명령어와 사용자를 생성하는 명령어는 이렇게 하면 된다.
    - 계정의 이름 골뱅이 앞부분만. user_codingrecipe 이 부분이 어떻게 보면 아이디 같은 뭐 유저네임 같은 거라고 보면 된다.
    - @localhost 서버의 위치 주소를 정하는 건데 대부분이 각자 개인 pc로 하는 경우가 많을 것이다. 그래서 그냥 골뱅이 로컬 호스트로 정하면 큰 문제 없이 사용을 할 수 있다.
    - identified by 뒤에 붙어 있는요 부분이 비밀번호다.
    - 계정과 비밀번호를 이렇게 동시에 지정을 한다고 보면 되고, 마지막으로 하나 더 해야 되는 것은 새로 만든 사용자(user_codingrecipe)가 이 데이터베이스(db_codingrecipe)를 사용할 수 있게끔 데이터베이스는 권한 설정하는 부분들이 있는데, 권한을 읽기 쓰기 권한 뭐 이런 것들을 다 구체적으로 원래 구분을 해서 쓰지만 지금 우리는 연습을 하는 것이기 때문에 모든 권한을 부여하는 형식으로 사용자에게 이 데이터베이스에 모든 권한을 준다는 명령어라고 보면 된다.

![사진](/assets/img/project/member/20.png)

- 각각을 실행을 해야 하는데 보통은 컨트롤 엔터키로 실행이 된다.
    
    

![사진](/assets/img/project/member/21.png)

- 접속 정보를 추가하는 부분에서 유저 네임을 입력하고, Connection Name는 보여지는 이름,  알아보기 쉬운 어떤 구분용 이름이라고 보면 된다. 이렇게 쓴 다음에 Test Connection을 클릭을 하게 되면 비밀번호를 물어본다. Save password in keychain 체크박스해서 비밀번호 입력하면 그 다음부터는 비밀번호를 묻지 않는다.

![사진](/assets/img/project/member/22.png)

- 디폴트 스키마라는 부분은 이 계정으로 접속을 했을 때 기본적으로 사용하는 데이터베이스가 뭐냐를 여기서 설정을 할 수 있다. 그래서 여기에 우리가 조금 전에 만들었던 DB 언더바 코딩 레시피 이런 데이터베이스 이름을 써주면 하나의 작업을 줄일 수 있다.

```sql
use db_codingrecipe
```

- mysql은 데이터베이스를 항상 사용을 먼저 해준다는 명령을 실행을 해 줘야 한다. 그래야 셀렉트 쿼리를 수행한다든지 크리에이트 쿼리를 수행하는 이런 것들이 이제 가능하다. 그런데 아까 디폴트 스키마에다가 DB 언더바 코딩 레시피 이런 데이터베이스 이름을 써 놓게 되면 작업을 생략을 해도 된다. 무조건 db_codingrecipe가 사용이 된다라고 mysql workbench가 자동 설정을 해주기 때문에 생략이 가능하다.

![사진](/assets/img/project/member/23.png)

```java
package com.codingrecipe.member.repository;
import org.springframework.data.jpa.repository.JpaRepository;

public interface MemberRepository extends JpaRepository<MemberEntity, Long> {
}

```

- 리파짓토리 패키지에 MemberRepository 인터페이스를 하나 추가

![사진](/assets/img/project/member/24.png)

```java
package com.codingrecipe.member.entity;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;

@Entity // 엔티티라는 걸 정의함. Entity 클래스 사용 가능
@Setter // 롬복에서 제공하는 게터, 세터
@Getter
@Table(name = "member_table") // db에 실제로 이 클래스에 정의한 대로 테이블이 생성이 된다. 생성되었을 때의 테이블 이름을 정하는 부분
public class MemberEntity {

    @Id // pk 지정(우리가 보통 테이블을 정의할 때는 pk, 프라이머리키 컬럼이 있다. PK를 정의할 때 이 Id룰 씀)
    @GeneratedValue(strategy = GenerationType.IDENTITY) // mysql에서는 얘가 auto_increment를 지정해주는 부분임. 오라클 같은 경우는 시퀀스 역할
    private Long id;

    @Column(unique = true) // 일반 컬럼 지정, 유니크 제약조건 추가
    private String memberEmail;

    @Column
    private String memeberPassword;

    @Column
    private String memberName;
}

```
- 엔티티 패키지에 MemberEntity 클래스 추가
    - 엔티티 클래스가 일종의 이제 테이블 역할을 하게 된다. 당연히 테이블이 있고 자바 클래스나 이제 그 메소드를 이용해서 어떤 쿼리를 쓰는 그리고 쿼리를 전송해서 테이블과의 그 어떤 crud 작업을 한다 이렇게 생각을 하게 되는데, 스프링 데이터 jpa라는 것은 데이터베이스의 테이블을 일종의 자바 객체처럼 이제 활용을 할 수 있도록 해주는게 가장 큰 특징이다.
    - 테이블 이름 member_table에 이제 이런 형식을 갖는 컬럼들이 생성이 돼서 이 스프링 데이터 jpa가 어떤 데이터베이스 테이블을 알아서 db에다가 만들어 준다. application.yaml에 우리가 아까 url에 정의한 데이터베이스 db_codingrecipe에다가

![사진](/assets/img/project/member/25.png)

- MemberRepository 인터페이스에서 MemberEntity 클래스 임포트

**MemberRepository.java**

```java
package com.codingrecipe.member.repository;

import com.codingrecipe.member.entity.MemberEntity;
import org.springframework.data.jpa.repository.JpaRepository;

public interface MemberRepository extends JpaRepository<MemberEntity, Long> {
}
```

![사진](/assets/img/project/member/26.png)

- MemberService 클래스에서 MemberRepository 클래스 임포트

**MemberService.java**

```java
package com.codingrecipe.member.service;

import com.codingrecipe.member.dto.MemberDTO;
import com.codingrecipe.member.repository.MemberRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

@Service // 스프링이 관리해주는 객체 즉, 스프링 빈으로 등록을 시킴
@RequiredArgsConstructor
public class MemberService {
    private final MemberRepository memberRepository;

    public void save(MemberDTO memberDTO) {
    }
}
```

**참고 자료**

---

- <https://www.youtube.com/watch?v=RhM1bQ76Tv0&list=PLV9zd3otBRt5ANIjawvd-el3QU594wyx7>
- [https://commnetall.tistory.com/106 - Intellij](https://commnetall.tistory.com/106)
- [https://velog.io/@codingrecipe/SpringBoot-Spring-Initializr로-프로젝트-만들기](https://velog.io/@codingrecipe/SpringBoot-Spring-Initializr%EB%A1%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0)
- [https://velog.io/@codingrecipe/mysql-설치하기](https://velog.io/@codingrecipe/mysql-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)
