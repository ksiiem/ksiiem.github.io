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
**✓ passwd - 사용자 패스워드 변경**

- /etc/shadow 파일에 패스워드 변경 및 저장
- passwd <-옵션> [계정명]
    - 옵션 : -S (상태표시), -d (삭제), -l (잠금), -u (잠금해제)
- 상태표시 정보 : **PS** (정상), **NP** (패스워드 없음), **LK** (잠금 혹은 패스워드 없는 상태)
    
    ```bash
    [LinuxMaster] #passwd -S chris
    chris LK 2023-01-08 0 999997-1 (비밀 번호 잠금.)
    [LinuxMaster] #passwd chris
    ...
    passwd: 모든 인증 토큰이 성공적으로 업데이트 되었습니다.
    [LinuxMaster] #passwd -S chris
    Chris PS 2023-01-08 0 999997-1 (비밀번호 설정, SHA512 암호화. )
    ```
    
    - #passwd -S chris : chris 계정의 상태 정보를 확인
    - #passwd chris : 패스워드 설정 후, 상태 정보 확인
        - 기본적으로 계정만 만든 상태에서는 패스워드가 없었는데,  패스워드 수정 passwd 명령의 결과로 업데이트 표시됨
        - 두 번째 passwd -S chris 명령에서는 비밀번호도 설정되어 있고, 암호화도 잘 되어있음

✓ su - 사용자 전환 (switch user)

- 로그아웃 없이 다른 사용자의 계정으로 로그인, 다른 사용자의 권한을 획득
    - 보통 다른 사용자의 계정. 일반적으로 루트와 같은 관리자 계정을 갖고 특정한 명령을 실행하기 위해 사용하기도 함
    - 아무런 옵션을 주지 않으면 기본적으로 루트 계정으로 로그인하려고 함
- su <-옵션> [계정명] <셸변수>
    - 옵션 : **-** 혹은 **-l** 혹은 login (사용자 환경변수 적용), -s (쉘 지정), -c (계정 변환없이 지정한 명령어 실행)
        - -c 옵션은 실제 계정을 전환하는 게 아닌, 전환했다 치고 해당 계정의 권한으로 명령만 실행하는 옵션
    
    ```bash
    [LinuxMaster] #su chris
    [LinuxMaster] #pwd
    /root
    [LinuxMaster] #exit
    exit
    [LinuxMaster) #su - chris
    [chris@118-27-119-212 ~] $ pwd
    /CHRIS
    ```
    
    - #su - chris : 크리스 계정으로 로그인 → 크리스에게 로그인 시 설정되어 있는 여러가지 환경 설정 값들이 적용된 상태에서 사용자 계정이 전환이 됨

✓ 사용자 설정 관련 환경 구성 파일

- **/etc/default/useradd** : 사용자를 추가할 때 기본 설정 정보로 활용
    - SKEL : 사용자 홈 디렉터리로 복사될 초기 환경 설정 파일이 저장된 디렉터리 지정
- 텍스트 편집기 (vi, nano 등) 혹은 'useradd - D 옵션'으로 변경
    
    ```bash
    [LinuxMaster] #cat /etc/default/useradd
    # useradd defaults file
    GROUP=100
    HOME=/home
    INACTIVE=-1
    SHELL=/bin/bash
    SKEL=/etc/skel
    ...
    ```
    
    - SKEL 변수는 사용자가 만들어질 때 사용자 홈 디렉토리로 복사해야 될( 기본적으로 저장해놓을) 설정 파일들이 위치해 있는 디렉토리 이름을 저장해주는 값
    - 편집을 하는 것이므로 텍스트 편집기나 useradd -D 옵션으로 변경 가능
- /etc/ passwd : 사용자 계정 정보 저장
    - **파일 형식)** username : password : uid : gid : comment : home-directory : shell
        - 최근에는 보안 등의 이슈로 코멘트는 사용하지 않기도 함
        - **구성 필드 기억**
    - password : 패스워드 → **pwconv 실행** (패스워드 필드는 보통 **x로 표시**, **/etc/shadow에 패스워드**를 암호화해서 **기록**), pwunconv로 비활성화
- **/etc/shadow** : **사용자 패스워드 암호화 파일**
    - 파일 형식) username : password: lastchange : mindays : maxdays : warndays : inactive : expire : flag
        - lastchange : **1970년 1월 1일** 기준 최근 비밀번호 변경일
        - mindays : 비밀번호 변경 후 재설정 가능한 최소 기간
        - inactive : 비밀번호 만료 후 계정 사용 불가능 까지의 유예 기간
        - expire : 계정 사용 정지 날짜 (1970년 1월 1일 기준으로 이후의 날짜 수)
        - flag : 기본 0으로 설정
- /etc/**login.defs** : 사용자 계정 설정 시 기본값
    - 예) MAIL_DIR (메일 디렉터리), PASS_MAX_DAYS (패스워드 변경 없이 사용할 수 있는 최대 일 수), PASS_MIN_LEN (패스워드 최소 길이), CREATE_HOME (홈 디렉터리 생성 여부) 등
    
    ```bash
    [LinuxMaster] #cat /etc/login.defs
    ...
    # PASS_MAX_DAYS Maximum number of days a password may be used.
    # PASS_MIN_DAYS Minimum number of days allowed between password changes.
    ...
    PASS_MAX_DAYS 99999
    PASS_MIN_DAYS O
    ...
    ```
    

✓ usermod - 사용자 계정 정보 수정

- 사용자 홈 디렉터리, 그룹, 유효기간, 셸 등 정보를 변경
- usermod <-옵션> [계정명]
    - 옵션 : -u (새로운 UID), -g (새로운 GID), -G (새로운 보조 그룹), -d (새로운 홈 디렉터리), -s (새로운 셀), c (새로운 주석), -| (새로운 계정명)
    
    ```bash
    /[LinuxMaster] #usermod - h
    Usage: usermod [options] LOGIN
    
    Options:
     - c, --comment COMMENT    new value of the GECOS field
    ```
    
    - -h 옵션은 간단하게 명령의 특징과 사용할 수 있는 옵션들을 한눈에 볼 수 있음

✓ userdel - 사용자 계정 정보 삭제

- /etc/passwd, /etc/shadow, /etc/group에서 사용자 정보 삭제
- usermod <-옵션> [계정명]
    - 옵션 : **-r** (사용자의 메일 파일과 홈 디렉토리 모두 삭제)
        - 사용자 계정 뿐만 아니라 관련된 것들 모두 삭제
    
    ```bash
    [LinuxMaster| #userdel -h
    Usage: userdel [options] LOGIN
    
    Options:
    -f, --force      force some actions that would fail otherwise
                  e.g. removal of user still logged in
                  or files, even if not owned by the user
    ... 
    ```
    

✓ chage - 패스워드 만료 정보 변경

- chage 〈-옵션〉 [계정명]
    - 옵션 : -l (계정 정보 표시), -m (최소 사용 일자), -M (사용 가능 일자), -E (만기일), -W (만기 전 지정 날짜 부터 경고)

```bash
[LinuxMaster] #chage -l chris
마지막으로 암호를 바꾼 날       : 1월 08, 2023
암호 만료               : 안함
암호가 비활성화 기간          :안함
계정 만료               : 안함
암호를 바꿀 수 있는 최소 날 수 :0
암호를 바꿔야 하는 최대 날 수 : 99999
암호 만료 예고를 하는 날 수 :7
...
```
<br>
**참고 자료**

---

- <https://www.youtube.com/watch?v=9I3kcxhAY7Q&t=3233s>