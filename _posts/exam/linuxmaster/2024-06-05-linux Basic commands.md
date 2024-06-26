---
title: "[Linux] 기본 명령어"
categories: [Study, OS]
---

## **리눅스 기본 명령어**

---

이 명령어들은 리눅스 시스템을 효율적으로 관리하고 운영하는 데 필수적이다. 실무에서 자주 사용되므로 익숙해지면 업무 효율성을 크게 높일 수 있다.

## **시스템 종료 및 재부팅**

---

- **`shutdown`**, **`halt`**, **`init 0`**, **`poweroff`** : 시스템 종료
- **`reboot`**, **`init 6`**, **`shutdown -r now`** : 시스템 재부팅

## **사용자 권한 관리**

---

- **`sudo`** : 다른 사용자가 super user 권한으로 실행
- **`su`** : 사용자의 권한을 root로 변경

## **디렉토리 및 파일 관리**

---

- **`pwd`** : 현재 위치한 디렉토리 확인
- **`cd`** : 디렉토리 이동
- **`ls`** : 현재 디렉토리 내의 파일 및 폴더 목록 표시
- **`mkdir`** : 디렉토리 생성
- **`rmdir`** : 디렉토리 삭제
- **`touch`** : 파일 생성 (크기 0)
- **`cp`** : 파일 복사 (디렉토리 내부까지 복사 시, **`cp -R`**)
- **`mv`** : 파일 이동
- **`rm`** : 파일 삭제 (디렉토리 삭제 시, **`rm -R`** 사용)
- **`cat`** : 파일 내용 출력
- **`more`** : 파일 내용을 화면 단위로 출력
- **`less`** : **`more`**보다 개선된 파일 내용 출력
- **`find`** : 특정 파일 찾기
- **`grep`** : 특정 패턴으로 파일 내용 검색
- **`>>`** : 리다이렉션 (파일 끼워넣기 등)
- **`file`** : 파일 종류 확인
- **`which`** : 특정 명령어의 위치 찾기

## **네트워크 관리**

---

- **`ping`** : 네트워크 상태 점검 및 도메인 IP 확인
- **`ifconfig`** : IP 확인 및 설정
- **`netstat`** : 네트워크 상태 확인
- **`nbstat`** : IP 충돌 시 충돌된 컴퓨터 찾기
- **`traceroute`** : 목적지까지 경로 추적
- **`route`** : 라우팅 테이블 상태 확인

## **시간 관리**

---

- **`clock`** : 시간 조절 명령어
- **`date`** : 시간 및 날짜 출력 및 변경

## **패키지 관리**

---

- **`rpm`** : rpm 패키지 설치, 삭제 및 관리
- **`yum`** : 의존성 해결 패키지 관리자 (다른 필요한 rpm 패키지까지 자동 다운로드)

## **시스템 정보**

---

- **`free`** : 시스템 메모리 정보 출력
- **`ps`** : 현재 실행 중인 프로세스 목록 출력
- **`pstree`** : 트리 형식으로 프로세스 출력
- **`top`** : 실시간 시스템 운용 상황 모니터링
- **`kill`** : 특정 프로세스에 signal 보내기
- **`killall`** : 특정 프로세스 모두 종료
- **`killall5`** : 모든 프로세스 종료 (사용 자제)

## **파일 압축 및 권한 관리**

---

- **`tar`**, **`gzip`** 등 : 파일 압축 및 해제
- **`chmod`** : 파일 및 디렉토리 권한 수정
- **`chown`** : 파일 및 디렉토리 소유자 및 소유 그룹 수정
- **`chgrp`** : 파일 및 디렉토리 소유 그룹 수정
- **`umask`** : 파일 생성 시 기본 권한 값 설정

## **작업 예약**

---

- **`at`** : 지정된 시간에 하나의 작업 수행
- **`crontab`** : 반복 작업 예약 (디스크 최적화를 위한 반복적 로그 파일 삭제 등)

## **사용자 및 그룹 관리**

---

- **`useradd`** : 새로운 사용자 계정 생성
- **`passwd`** : 사용자 계정 비밀번호 설정
- **`userdel`** : 사용자 계정 삭제
- **`usermod`** : 사용자 계정 수정
- **`groupadd`** : 그룹 생성
- **`groupdel`** : 그룹 삭제
- **`groups`** : 현재 사용자가 속한 그룹 확인
- **`newgrp`** : 현재 속한 그룹 변경

## **메시지 및 통신**

---

- **`mesg`** : 메시지 응답 가능 및 불가 설정
- **`talk`** : 로그인한 사용자 간 대화
- **`wall`** : 시스템에 로그인한 모든 사용자에게 메시지 전송
- **`write`** : 특정 사용자에게 메시지 전송

## **기타**

---

- **`dd`** : 블럭 단위로 파일 복사 및 변환