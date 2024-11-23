---
title: "02 리눅스 기본 명령어"
categories: [Study, OS]
---

# 리눅스 기본 명령어

---

✓ 사용자 생성 및 계정 관리

- 사용자를 등록하고, 계정을 만들어야 로그인을 해서 사용할 수 있음

✓ 디렉터리 및 파일 관련 명령어

- 기본적인 디렉토리(폴더)와 관련된 명령어

✓ 리다이렉션과 정규 표현식

## 기본 명령어

✓ which - **명령어 경로** 확인

- **PATH 환경변수의 경로에 따라** 사용자가 실행할 명령어의 위치 혹은 alias를 표시
    - 파라미터로 전달하는 그 명령어가 어느 디렉토리에 있는지를 확인하고 출력해줌
    - PATH는 리눅스 시스템이 명령을 찾기 위해서 이 명령어는 어디 있을까? 디렉토리를 하나씩 방문하면서 확인해갈 텐데 이 방문하는 경로들을 담아놓은 환경변수임
    - 환경변수는 운영체제가 특정한 기능이나 동작을 수행하기 위해서 참고하는 일종의 운영체제를 위한 변수임(값을 담아놓는 저장소)
- which [명령어]
    
    ```bash
    [LinuxMaster] #which bash 
    /usr/bin/bash
    [LinuxMaster] #which pwd 
    /usr/bin/pwd
    ```
    

✓ whereis - 대상 명령어의 **절대 경로**, 소스 파일, 설정 파일, 메뉴얼 등 정보를 표시

- 부가적인 조금 더 많은 정보를 표시
- Whereis (-옵션〉 [명령어]
    - 옵션: -b (바이너리만 검색), -m (지정한 메뉴얼 섹션만 검색), -M (메뉴얼 페이지 위치 제한), -u (특정 파일 제외)
    
    ```bash
    [LinuxMaster] #whereis find
    find: /us/bin/find/usr/share/man/man1p/find.1p.gz /us/share/man/man1/find.1.gz
    ```
    

✓ alias - 명령어의 별칭 만들기

- 명령어에 별칭을 부여하여 자주 사용하는 명령을 편리하게 사용
- alias [별칭 = '명령']
    
    ```bash
    
    [LinuxMaster] #alias showall='ls-al'
    [LinuxMaster] #showall
    합계 1968
    dr-xr-x---. 10 root root 4096 1월 5 13:28.
    dr-xr-xr-x. 19 root root 4096 9월 702:19..
    drwx------. 3 root root 4096 3월 9 2021 .ansible
    -rw-------. 1 root root 15821 1월 5 15:44 .bash_history
    -rw-r--r--. 1 root root 18 12월 29 2013. bash_logout
    ...
    ```
    
    - ls 옵션은 현재 내 폴더에 있는 목록을 보여줌
    - -al 옵션은 숨긴 파일까지 포함한 전체 내용을 리스트 형식 자세히 보여주라는 명령어
    - showall 이라는 별칭을 실행하면 구성되어 있는 ls-al 명령이 실행됨

✓ unalias - 명령어의 별칭 삭제하기

- 만들어 놓은 별칭을 삭제
- unalias [별칭]
    - unalias -a : 설정해 놓았던 별칭을 모두 삭제
    
    ```bash
    [LinuxMaster] #unalias showall
    [LinuxMaster] #showall
    -bash: showall: command not found
    ```
    

✓ **PATH 환경 변수** - 명령어(프로그램)가 위치한 경로를 찾는 순서 지정

- PATH 환경 변수에 지정된 디렉터리를 순서대로 탐색하여 명령어(프로그램)을 찾고 실행
    - PATH 환경변수는 경로들을 모아놓은 것
- 확인 : echo $PATH
    - echo는 뒤에 나오는 문자열 등을 화면에 출력
    - 뒤에 $가 붙어있음. 자신이 원하는 환경변수를 만들고 값을 설정한 후 사용 가능하여 다양한 환경변수를 지정하여 사용 → 환경변수 안의 실제 값을 참조하고 싶은 경우 환경변수 앞에 $를 붙임
- 설정: export PATH = [설정값:설정값:•••]
    - ~/.bash_profile에 기본값 설정. (예) export PATH=$PATH:/var/myfile
        - /var/myfile 파일의 디렉토리 안에서도 명령어가 있는지 없는지 찾겠다.
        - 이미 지정되어 있는 PATH의 값 → 현재 지정되어 있는 PATH 뒤에다가 새로운 내가 원하는 디렉토리를 추가해주는 효과
        - ~: 사용자의 home 폴더. 그 안에는 .bash_profile 사용자 설정 파일 안에 기본으로 이런 PATH와 관련된 환경변수들이 세팅되어 있음 → 로그인 하는 순간 이런 환경변수들이 적용되어 사용할 수 있게 됨

※ 기타 환경 변수의 확인 및 설정 방법도 동일함

```bash
[LinuxMaster] #echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
[LinuxMaster] #export MYVAR=LinuxMaster
[LinuxMaster] #echo $MYVAR
LinuxMaster
```

- #export MYVAR=LinuxMaster : MYVAR 환경변수에 값을 세팅
- #echo $MYVAR : $는 값을 참고 →LinuxMaster 출력
✓ man - 리눅스 명령어의 메뉴얼(manual) 확인

- 리눅스에서 사용할 수 있는 명령어들의 메뉴얼을 확인
- man <섹션><-옵션〉 [명령어]
    - 섹션 : 1 (일반), 2 (시스템 호출), 3 (C 표준 함수), 4 (장치/특수 파일), 5 (특정한 파일), 6 (게임/화면보호기), 7 (리눅스 파일 표준, 프로토콜, 시그널 목록), 8 (시스템 관리 명령어, 데몬 정보), 9 (커널 관리 정보)
    - -h (사용법 표시), -a (모든 페이지 표시), -w (검색 문자의 메뉴얼 페이지의 위치 표시), -f (키워드 검색하여 페이지 표시)
        - man의 옵션자체가 궁금해서 도움말을 보는 -h 옵션
        - 키워드를 검색해서 관련된 그 키워드가 있는지를 살펴보는 -f 옵션
    
    ```bash
    [LinuxMaster] #man find
    FIND(1)              General Commands Manual             FIND(1)
    NAME
    	find - search for files in a directory hierarchy
    ...
    ```
    
    - 파일을 검색할 수 있는 명령어인 find의 메뉴얼을 표시해주게 됨

✓ apropos - 명령어 메뉴얼에서 지정한 키워드가 포함된 것을 검색

- **whatis 데이터베이스를 참조**하여 지정한 문자열을 포함한 것을 검색하여 표시
- apropos [문자열]
    
    ```bash
    [LinuxMaster] #apropos find 
    ffs (3)        - find first bit set in a word
    ffsl (3)       - find first bit set in a word
    ffsll (3)      - find first bit set in a word
    tfind (3)      - manage a binary tree
    ttyslot (3)    - find the slot of the current user's terminal in some file
    ```
    
    - whatis 데이터베이스를 참조하여 find 문자열이 포함되어 있는 것들을 검색해서 보여줌

✓ manpath - man 명령이 참조하는 메뉴얼의 경로 표시

- man 명령이 페이지를 찾기 위해 탐색하는 경로 정보를 표시
    - man 명령어도 어딘가에서 데이터를 보고 참고해서 화면에 표시해 주는 것
    - 리눅스에 기본적으로 설치되어 있는 프로그램이 아니라 인터넷으로부터 새로운 프로그램을 다운 받아서 프로그램을 설치할 경우, 해당 프로그램에 대한 메뉴얼도 함께 설치하게 되는 경우도 있는데, 메뉴얼 파일의 위치를 찾고자 할 때
- manpath
    
    ```bash
    [LinuxMaster] #manpath
    /usr/local/share/man:/usr/share/man/overrides:/usr/share/man
    ```
    

✓ info - 리눅스 명령어의 사용법, 옵션 등 표시

- 리눅스 명령어의 사용법, 옵션 등을 표시. man 명령보다 제한적인 기능 제공
- info [명령어]

```bash
[LinuxMaster] #info find
File: find.info, Node: Invoking find, Next: Invoking locate, Up: Reference

8.1 Invoking 'find'
ニニニニニニニニニニニニニニニニニニニ

	find [-H] [-L] [-P] [-D DEBUGOPTIONS] [-OLEVEL] [FILE...] [EXPRESSION]
...
```

✓ whatis - 명령어의 기능을 **간략히 표시**

- **whatis 데이터베이스에서 키워드를 검색**하여 일치할 경우 명령어의 기능을 표시
- whatis [명령어]
    
    ```bash
    [LinuxMaster] #whatis find
    find (1p)    - find files
    find (1)     - search for files in a directory hierarchy
    ```
    

## 사용자 생성 및 계정 관리

✓ useradd, adduser - 사용자 계정 생성

- 사용자 계정과 관련 정보를 생성
- **useradd 〈-옵션〉 [계정명]**
    - 옵션 : -s (로그인 셸 지정), -d (홈 디렉터리 지정), -f (패스워드 만료 후 계정 영구 정지까지의 기간), -e (계정의 유효기간, 임시계정 생성), -c (/etc/passwd에 사용자 설명 추가), -G (소속할 그룹 추가)
        - 해당 사용자를 로그인할 때, 특정한 쉘을 사용, 홈 디렉토리 경로, 패스워드 만료기간 등 지정 가능
- 기본 홈 디렉터리 : **/home/계정명**
    - 일반 사용자의 경우
- 계정 정보 파일 :  **/etc/passwd, /etc/shadow, /etc/group**에 정보 추가
    
    ```bash
    [LinuxMaster] #useradd - d /CHRIS chris
    ```
    
    - 사용자를 생성하게 되면 그 사용자를 관리하기 위한 다양한 정보들이 리눅스 설정 파일에 추가됨
    - /etc/passwd는 사용자가 로그인할 때 패스워드를 점검하고 사용된 환경(예. 사용자는 어떤 셸을 쓰는가)에서의 내용들을 기록
    - /etc/shadow는 일상적으로 사용자 비밀번호를 암호화해서 저장하고 관리하기 위해서 함께 사용하는 설정파일

<br>
**참고 자료**

---

- <https://www.youtube.com/watch?v=9I3kcxhAY7Q&t=3233s>