---
title: "01 리눅스일반"
categories: [Study, OS]
---

# 리눅스 일반

---

✓ 리눅스의 이해
.리눅스 개요, 리눅스 배포판의 특징, 오픈소스 라이선스
✓ 리눅스 설치
.리눅스 설치, 파티션과 파일 시스템, 부트 매니저

## 리눅스 개요

✓ 리눅스의 특징

- 오픈소스 소프트웨어 → 소스코드와 관련 자료가 공개
- 멀티유저(다중사용자), 멀티태스킹(다중작업) → 가상 터미널을 이용한 다중 작업
- 다중 스레드 → 동시에 여러 개의 네트워크 작업, 웹서버와 같은 인터넷 기반 서비스 등을 설치해 운영하는 데 좋은 환경 제공
- 다양한 파일 시스템 : 리눅스 파일시스템(EXT), 윈도우 파일 시스템(NTFS), 네트워크 파일 시스템(NFS)

✓ 리눅스의 장점

- POSIX(Portable Operating System Interface) 표준 기반, 유닉스와 호환 가능
- 상대적으로 안정적인 동작, 하드웨어 등 시스템 자원 효율성 → 많은 사용자가 동시에 접속해서 사용하는 서비스 운영에 유리
- IT 인프라 서비스를 위한 운영체제 - 웹 서비스, 파일 기반 서비스, 기타 서비스를 설치해서 사용할 수 있는 기반 운영체제로 많이 사용

✓ 리눅스의 단점

- 공개 운영체제로 기술 지원의 한계, 보안상 취약점 노출 가능성, 한글 지원 미흡 → 최근에는 많이 보완됨

## 리눅스 배포판의 특징

✓ 리눅스 배포판이란?

- 리눅스 커널, GNU 소프트웨어, 응용 프로그램 등을 함께 묶어서 구성한 리눅스 운영체제 → 목적에 따라 다양한 배포판이 존재

✓ 리눅스 배포판의 종류와 특징

- 슬랙웨어 리눅스(Slackware Linux) : 1992년 패트릭 볼커딩, 가장 먼저 대중화
- **데비안(Debian)** : 데비안 프로젝트, GNU의 공식 후원, **apt 패키지 매니저**를 이용한 소프트웨어 관리
- **우분투(Ubuntu)** : 데비안 GNU/리눅스에 기초, 유니티(데스크톱 환경), 사용자 편의성에 초점
- 레드햇 : 미국 래드햇사, RHEL (기업용)과 페도라로 나뉨
- RHEL (Red Hat Enterprise Linux) : 래드햇 사용 리눅스, 서브스크립션 형태의 요금, 출시 후 7년간 기술지원
- 페도라 : 래드햇과 커뮤니티의 지원으로 개발, 6개월 마다 새로운 버전, 지원 기간은 13개월
- CentoS : RHEL과 완벽하게 호환, 무료 운영체제
- 수세(SuSe) : 독일에서 출시, 롤링 릴리즈 (Rolling Release)

## 리눅스의 역사

✓ 출제 예상 문제 대비

- 켄 톰슨 (UNIX)
- 데니스 리치 (UNIX C언어로 재개발)
- 리처드 스톨먼 (GNU 프로젝트, FSF, GPL)
- 앤드루 타넨바움 (MINIX)
- 리누스 토발즈
(MINX 커널 수정, **GNU 시스템 커널**, **리눅스 커널**)

## 오픈소스 라이선스

✓ GNU, FSF, 오픈소스 소프트웨어, 오픈소스 라이선스

- GNU(GNU's Not UNIX) : 리처드 스톨만의 자유 소프트웨어 재단 프로젝트, 완전한 유닉스 호환 시스템이 목적
- FSF(Free Software Foundation) : 리처드 스톨만이 설립, 실행/복제/배포/학습/개작/개선의 자유, 유료/무료 배포
- 오픈소스 소프트웨어 (OSS) : 자유 소프트웨어 (Free Software) 대신 사용하게 된 용어, 자유와 무료의 용어 혼돈을 고려
- 오픈소스 라이선스 : OsI(Open Source Initiative)가 주도, OSS 개발자와 이용자 간 사용 방법, 조건, 범위 등 명시한 계약