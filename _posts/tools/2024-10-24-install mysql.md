---
title: "[MySQL]설치 및 실행(mac OS)"
categories: [Tools, "DB"]
---

1. 홈페이지에 접속하여 무료 버전인 mysql community(GPL) Downloads를 클릭한다.
    
    <https://dev.mysql.com/downloads/>
    
2.  Mac 사용자는 Mysql community Server, Mysql Workbench를 각각 다운받아 설치하면 된다. 윈도우 사용자는 MySQL Installer for Windows를 클릭한다.
    
    !![image.png](/assets/img/tools/1.png)
    
3. 다운로드 페이지에서 아래에 있는 것으로 다운 받는다.
    
    ![image.png](/assets/img/tools/2.png)
    
    ![image.png](/assets/img/tools/3.png)
    
4. 아래 화면에서 로그인을 해도 되고, 빨간 네모로 표시한 부분을 클릭하면 바로 다운로드가 시작된다.
    
    ![image.png](/assets/img/tools/4.png)
    
    ![image.png](/assets/img/tools/5.png)
    
5. 비밀번호를 설정하는 화면이다. 어떤 값으로 하든 잘 기억하자
    
    ![image.png](/assets/img/tools/6.png)
    
    ![image.png](/assets/img/tools/7.png)
    
6. 모든 설치가 완료되고 MySQL Workbench 실행 화면
    
    ![image.png](/assets/img/tools/8.png)
    
7. root계정 클릭 후 비번 입력하여 작업창 들어가면 완료!
    
    ![image.png](/assets/img/tools/9.png)
    
    ![image.png](/assets/img/tools/10.png)
    

**[root 계정으로 로그인]**

```bash
cd /usr/local/mysql/bin
```

```bash
./mysql -uroot -p
```

![image.png](/assets/img/tools/11.png)

```bash
		sudo /usr/local/mysql/support-files/mysql.server start
```

![image.png](/assets/img/tools/12.png)

**[계정 생성]**

![image.png](/assets/img/tools/13.png)


<br>
**참고 자료**

---

- [https://losskatsu.github.io/database/mysql-install-mac/#31-접속하기](https://losskatsu.github.io/database/mysql-install-mac/#31-%EC%A0%91%EC%86%8D%ED%95%98%EA%B8%B0)
- [https://velog.io/@codingrecipe/mysql-설치하기](https://velog.io/@codingrecipe/mysql-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)

