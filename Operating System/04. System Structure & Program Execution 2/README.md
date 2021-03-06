# 4. System Structure & Program Execution 2

<!--ts-->

- [4. System Structure &amp; Program Execution 2](#4-system-structure--program-execution-2)
  - [1. 동기식 입출력과 비동기식 입출력](#1-동기식-입출력과-비동기식-입출력)
  - [2. DMA(Direct Memory Access)](#2-dmadirect-memory-access)
  - [3. 서로 다른 입출력 명령어](#3-서로-다른-입출력-명령어)
  - [4. 저장장치 계층 구조](#4-저장장치-계층-구조)
  - [5. 프로그램의 실행(메모리 load)](#5-프로그램의-실행메모리-load)
  - [6. 커널 주소 공간의 내용](#6-커널-주소-공간의-내용)
  - [7. 사용자 프로그램이 사용하는 함수](#7-사용자-프로그램이-사용하는-함수)
  - [8. 프로그램의 실행](#8-프로그램의-실행)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->
<!-- Added by: sungminyou, at: 2022년 6월 11일 토요일 15시 36분 28초 KST -->

<!--te-->

## 1. 동기식 입출력과 비동기식 입출력

![1](https://user-images.githubusercontent.com/48282185/170663596-230d0d5e-f5cd-43b2-a101-fb38adab811f.png)

1. 동기식 입출력(synchronous IO)

- IO장치까지 직접가서 결과를 보고 오는 것을 동기식 입출력이라고 부른다.
- IO작업이 완료되기 전에 다음 instruction을 실행하지 않고 대기
- IO 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 어플리케이션에 넘어감
- 구현 방법1(IO완료될 때까지 CPU를 가지고 있음)
  - IO가 끝날 때까지 CPU를 낭비시킴, IO작업 동안 대기하기때문
  - 매시점 하나의 IO만 일어날 수 있음, CPU가 대기 중이기 때문에 다른 요청을 처리할 수 없음
- 구현 방법2(IO요청 후 다른 프로세스에 CPU를 넘김)
  - IO가 완료될 때까지 해당 프로세스에게서 CPU를 빼앗음
  - IO처리를 기다리는 줄에 그 프로세스를 줄 세움
  - 다른 프로그램에 CPU제어를 넘김

1. 비동기식 입출력(asynchronous IO)

- IO작업이 완료되기 전에 다음 instruction을 실행함
- IO가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 어플리케이션에 즉시 넘어감

1. 두 경우 모두 IO의 완료는 인터럽트로 알려줌
1. sync는 시간적으로 동작을 맞춘다는 것을 의미

## 2. DMA(Direct Memory Access)

- 빠른 입출력 장치(인터럽트를 더 빈번히 발생시킴)를 메모리에 가까운 속도로 처리하기 위해 사용
- CPU의 중재 없이 device controller가 device의 buffer storage의 내용을 메모리에 block단위로 직접 전송
- 바이트 단위가 아니라 block단위로 인터럽트를 발생시켜 CPU의 인터럽트 당함 횟수를 줄인다.

## 3. 서로 다른 입출력 명령어

![2](https://user-images.githubusercontent.com/48282185/170663582-c87a7fae-e355-44a0-a861-ede2f5491465.png)

- IO를 수행하는 special instruction에 의해
  - memory 접근 instruction(load instruction)
  - device 접근 instruction(각 장치에 주소가 있다)
  - 일반적인 구현 방식
- Memory Mapped IO에 의해
  - IO장치들에 메모리 주소를 매겨서 메모리 접근 instruction을 통해 device에 접근

## 4. 저장장치 계층 구조

![3](https://user-images.githubusercontent.com/48282185/170663490-54f1d9ef-f679-45d6-8d9c-53995e8b26cc.png)

- 속도
- 용량에 따른 가격
- 휘발성 / 비휘발성
- primary는 CPU가 직접 접근이 가능한 것을 의미
  - CPU가 직접 접근이 가능하려면 Byte단위로 접근이 가능해야함
- secondary는 CPU가 직접 접근이 불가능한 것을 의미
- cache메모리는 memory와 CPU의 속도차이를 완충하기 위해 필요
  - cache는 보통 재사용을 위해 사용

## 5. 프로그램의 실행(메모리 load)

![4](https://user-images.githubusercontent.com/48282185/170663483-1a366ad4-ea1d-4c2d-a6a8-1091ce8f695c.png)

- 어떤 프로그램을 실행시키면 해당 프로그램의 독자적 주소공간(address space)이 생기고, 이 주소공간은 stack, data 그리고 code로 구성된다.
  - code
    - 프로그램의 기계어 코드가 담겨있음
  - data
    - 프로그램의 전역변수 등, 프로그램이 사용하는 자료구조가 담겨있음
  - stack
    - 코드가 함수구조로 되어있기 때문에 함수를 호출하고 리턴할 때 데이터를 쌓고 꺼내는 용도로 사용됨
- 프로그램이 실행된다고 해서 그 프로그램의 주소 공간을 모두 메모리에 올리는 것은 비효율적이다. 그래서 당장 필요한 부분만 메모리에 올리고, 그렇지 않은 부분은 디스크의 swap area에 보관해둔다.
- 실제 프로그램의 독자적 주소공간이 실재하는 것이 아니라 메모리 여기저기에 흩어져있기도하고, 디스크의 swap area에 있기도 하기 때문에 연속된 것이 아니다. 그래서 가상으로 존재하는 연속적인 공간이라는 뜻으로 virtual memory라고 부른다.
- disk는 swap area와 file system 두 가지 용도를 가지고 있고, 관리하는 방법도 다르다. swap area의 경우 프로그램이 종료되면 의미 없어지는 데이터이고 file system은 영구적으로 보관되야하는 데이터이다.
- physical memory와 virtual memory 사이의 주소 변환을 address translation이라고 부르고, 이 작업을 담당하는 주소 변환 layer가 존재한다. OS가 직접하는 것이 아니라 하드웨어의 서포트를 받는다.

## 6. 커널 주소 공간의 내용

![5](https://user-images.githubusercontent.com/48282185/170663476-ba9b4145-581d-4953-81b2-46f2ec94b6ea.png)

- data
  - 운영체제가 사용하는 여러 자료구조들이 정의되어있음
  - 각 자원들을 관리하기 위해 각 종류마다 자료구조를 만들어 관리한다.
  - 프로세스를 관리하기 위한 각 프로세스마다 자료구조(PCB)를 만들어 관리한다.
- stack
  - 운영체제도 함수구조로 코드가 짜여 있기때문에 스택영역이 필요하다.
  - 여러 사용자 어플리케이션들이 커널을 부르기 때문에 어떤 어플리케이션이 실행되는지 알 필요가 있다. 그래서 각 어플리케이션마다 커널 스택을 따로 둘 필요가 있다.

## 7. 사용자 프로그램이 사용하는 함수

- 어떤 언어를 쓰던 어떤 프로그램이던 함수 형태이다.
- 기계어에서도 함수에 해당하는 부분이 어디부터 어디까지인지 유지가 된다.
- 함수는 프로세스 주소공간에 code영역에 들어가게 된다.
- 함수
  - 사용자 정의 함수
    - 자신의 프로그램에서 정의한 함수
  - 라이브러리 함수
    - 자신의 프로그램에서 정의하지 않고 미리 구현된 것을 갖다 쓴 함수
    - 자신의 프로그램의 실행 파일에 포함되어 있다.
  - 커널 함수
    - 운영체제 프로그램의 함수
    - 커널 함수의 호출(시스템 콜)

## 8. 프로그램의 실행

![6](https://user-images.githubusercontent.com/48282185/170663462-46f11baa-8461-4c49-921e-a836887c69d3.png)

- 다른 프로그램의 개입이 없다고 가정

> 출처

> ABRAHAM SILBERSCHATZ ET AL., OPERATING SYSTEM CONCEPTS, NINTH EDITION, WILEY, 2013

> 반효경, 운영체제와 정보기술의 원리, 이화여자대학교 출판부, 2008
