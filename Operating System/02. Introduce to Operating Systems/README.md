# 2. introduce to Operating Systems

<!--ts-->

- [2. introduce to Operating Systems](#2-introduce-to-operating-systems)
  - [1. 운영체제란 무엇인가?](#1-운영체제란-무엇인가)
  - [2. 운영체제의 분류](#2-운영체제의-분류)
  - [3. 용어 정리](#3-용어-정리)
  - [4. 운영체제의 예](#4-운영체제의-예)
  - [5. 운영체제의 구조](#5-운영체제의-구조)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->
<!-- Added by: sungminyou, at: 2022년 6월 11일 토요일 15시 37분 11초 KST -->

<!--te-->

## 1. 운영체제란 무엇인가?

컴퓨터 하드웨어 바로 위에 설치되어 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층

- 좁은 의미의 운영체제(커널)
  - 운영체제의 핵심부분으로 부팅 이후 항상 메모리에 상주하는 부분
  - 보통은 커널을 운영체제로 이야기한다.
- 넓은 의미의 운영체제
  - 커널을 포함한 각종 주변 시스템 유틸리티(메모리에 상주하지는 않는 유틸들)를 포함한 개념
  - 파일 복사 등의 부가적 프로그램

## 2. 운영체제의 분류

- 동시 작업 가능 여부에 따른 분류
  - 단일 작업(single task)
    - MS-DOS 프롬프트 상에서는 한 명령의 수행이 끝내기 전에 다른 명령을 수행시킬 수 없음
    - 기능이 적은 기기의 특수 목적의 운영체제는 현제에도 단일 작업을 지원한다.
  - 다중 작업(multi tasking)
    - UNIX, Windows 등에서는 한 명령의 수행이 끝나기 전에 다른 명령이나 프로그램을 수행할 수 있음
    - 현재의 다수의 운영체제
- 사용자의 수에 따른 분류
  - 단일 사용자(single user)
    - 여러 사용자 계정 불가능, 한 사람만 접근
    - MS-DOS, MS Window
  - 다중 사용자(multi user)
    - 여러 사용자 계정 가능, 동시 접근
    - UNIX, NT server
    - 각 사용자에 대한 파일이나 메모리를 다른 사용자가 볼 수 없게하는 보안 기능과 각 사용자 간의 자원 분배 등 추가적인 기능이 필요하다.
  - 처리 방식에 따른 분류
    - 일괄 처리(batch processing)
      - 바로 처리하지 않고 작업 요청의 일정량을 모아서 한꺼번에 처리
      - 작업이 완전 종료될 때까지 기다려야 함
      - 예시로 초기 Punch Card 처리 시스템
    - 시분할(Time sharing)
      - 현대의 처리 방식
      - 여러 작업을 수행할 때 컴퓨터 처리 능력을 일정한 시간 단위로 분할하여 사용
      - 일괄 처리 시스템에 비해 짧은 응답 시간을 갖는다
      - interactive(바로 결과가 나오는)한 방식
      - 사용자가 많아 질수록 느려진다.
      - 정확한 시간을 지키기는 힘들다.
      - 여러 사용자가 사용하는 것을 목적으로 한다.
      - 예시로 UNIX
      - 범용 운영체제가 realtime application을 지원하기 위해 어떻게 해야할지 고민해보는 것도 좋을 듯
    - 실시간(Realtime)
      - 정해진 시간 안에 어떠한 일이 반드시 종료됨이 보장되어야 하는 실시간 시스템을 위한 OS
      - 예시로 원자로/공장 제어, 미사일 제어, 반도체 장비, 로보트 제어 등 특수 목적을 갖는 것들
      - 실시간 시스템의 개념 확장
        - Hard realtime system > 시간을 엄격히 지켜야함 > 위의 예시
        - Sort realtime system > 데드라인이 있지만 엄격하지는 않음 > 스트리밍 같은 것들

## 3. 용어 정리

- multitasking
  - 여러 작업이 동시에 실행되는 것. 하나의 프로세스가 끝나기 전 다른 프로세스가 실행될 수 있다.
- multiprogramming
  - 메모리에 여러 프로세스가 올라가는 것. 멀티 태스킹과 같은 컨텍스트이지만 메모리가 강조된 표현이다.
- time sharing
  - 동시에 진행되는것 처럼 보이지만, 시간 단위로 분할되는 것 이 역시 마찬가지로 멀티 태스킹과 같은 컨텍스트 이지만 CPU의 작업이 강조된 표현이다.
- multiprocess
  - 여러 프로세스가 동시에 실행됨을 의미
- multiprocessor
  - 하나의 컴퓨터에 CPU가 여러 개 있음을 의미
  - 각 CPU에 각각 다른 작업이 들어가 있을 수 있음

## 4. 운영체제의 예

- UNIX
  - 코드의 대부분을 C언어로 구성
  - 서버를 위해 고안됨, 여러 사용자가 사용하는 환경을 고려
  - 높은 이식성(기계에 종속되있지 않다.)
  - 오픈소스
  - 최소한의 커널 구조
  - 복잡한 시스템에 맞게 확장 용이
  - 프로그램 개발에 용이
  - 다양한 버전
    - linux, solaris
- DOS
  - 단일 사용자용 운영체제, 메모리 관리 능력의 한계(최대 640KB)
- Windows
  - 다중 작업용 GUI 기반 운영 체제
- MacOS, ios, android 기타 등등

## 5. 운영체제의 구조

- cpu 스케쥴링
- 메모리 관리
- 파일 관리
- 입출력 관리
- 프로세스 관리
- 보호 시스템
- 네트워킹
- 명령어 해석기

> 출처

> ABRAHAM SILBERSCHATZ ET AL., OPERATING SYSTEM CONCEPTS, NINTH EDITION, WILEY, 2013

> 반효경, 운영체제와 정보기술의 원리, 이화여자대학교 출판부, 2008
