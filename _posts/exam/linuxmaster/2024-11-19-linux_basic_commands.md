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


<br>
**참고 자료**

---

- <https://www.youtube.com/watch?v=9I3kcxhAY7Q&t=3233s>