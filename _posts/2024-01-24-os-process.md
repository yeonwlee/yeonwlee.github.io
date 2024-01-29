---
title: 운영체제 - 프로세스
author: yeonwlee
date: 2024-01-24 00:00:00 +0900
categories: [Computer Science, OS]
tags: [CS, 운영체제, 프로세스]
---

<br>

- [1. 프로세스(Process)](#1-프로세스process)
- [2. 프로세스의 구성 요소](#2-프로세스의-구성-요소)
  - [2.1 프로세스 메모리(Process Memory)](#21-프로세스-메모리process-memory)
    - [코드 섹션 (Code Section 또는 Text Section):](#코드-섹션-code-section-또는-text-section)
    - [데이터 섹션 (Data Section):](#데이터-섹션-data-section)
    - [스택 (Stack):](#스택-stack)
    - [힙 (Heap):](#힙-heap)
  - [2.2 프로세스 제어 블록(Process Control Block, PCB)](#22-프로세스-제어-블록process-control-block-pcb)
    - [프로세스 식별자(Process ID)](#프로세스-식별자process-id)
    - [프로세스 상태(Process State)](#프로세스-상태process-state)
    - [프로그램 카운터(Program Counter):](#프로그램-카운터program-counter)
    - [레지스터 상태:](#레지스터-상태)
    - [프로세스 우선순위(Process Priority):](#프로세스-우선순위process-priority)
    - [프로세스 계정 정보(Process Accounting Information):](#프로세스-계정-정보process-accounting-information)
    - [메모리 관리 정보(Memory Management Information):](#메모리-관리-정보memory-management-information)
    - [입출력 상태 및 정보(I/O Status and Information):](#입출력-상태-및-정보io-status-and-information)
- [3. 프로세스의 상태](#3-프로세스의-상태)
  - [생성 (Creation) 상태](#생성-creation-상태)
  - [준비 (Ready) 상태](#준비-ready-상태)
  - [실행 (Running) 상태](#실행-running-상태)
  - [대기 (Waiting 또는 Blocked) 상태](#대기-waiting-또는-blocked-상태)
  - [종료 (Termination) 상태:](#종료-termination-상태)

<br>

## 1. 프로세스(Process)

> 컴퓨터에서 실행되고 있는 프로그램, 프로그램이 실제로 동작하는 상태
> 라고 볼 수 있다.

## 2. 프로세스의 구성 요소

### 2.1 프로세스 메모리(Process Memory)

#### 코드 섹션 (Code Section 또는 Text Section):

프로그램의 실행 코드가 저장되는 영역. 읽기 전용 영역이며, CPU는 해당 위치에 저장된 명령어를 읽어서 실행한다.

#### 데이터 섹션 (Data Section):

정적인 데이터와 전역 변수들이 저장되는 영역. 읽기/쓰기가 이뤄질 수 있으며, 상수나 초기화된 변수들을 포함한다.

#### 스택 (Stack):

함수 호출과 관련된 지역 변수, 반환 주소, 함수 호출 시 사용되는 파라미터 등이 저장되는 영역. 스택은 후입선출(LIFO) 구조를 가지고 있어서 가장 최근에 호출된 함수가 가장 먼저 종료된다. 스택은 주로 임시 데이터를 저장하고 함수 호출을 관리하는 데 사용된다.

#### 힙 (Heap):

동적으로 할당되는 메모리가 저장되는 영역으로, 프로세스 실행 중에 동적으로 할당된 데이터가 저장된다. 힙은 개발자가 메모리를 직접 할당하고 해제할 수 있는 공간이다.

### 2.2 프로세스 제어 블록(Process Control Block, PCB)

프로세스에 대한 정보를 저장하는 데이터 구조.
각 프로세스마다 하나의 PCB가 할당되며, 프로세스의 상태 및 실행에 필요한 모든 정보를 추적한다.
PCB는 프로세스의 상태 변화에 따라 업데이트 되며, 스케줄러 및 다른 운영체제의 서비스가 이 정보를 활용해 프로세스를 관리한다.

PCB에는 아래와 같은 정보들이 포함되어 있다.

#### 프로세스 식별자(Process ID)

각 프로세스는 고유한 식별자를 가지며, 이는 시스템 내에서 프로세스를 식별하는 데 사용된다.

#### 프로세스 상태(Process State)

현재 프로세스의 상태. 이를 기반으로 스케줄러가 프로세스를 관리한다.

#### 프로그램 카운터(Program Counter):

다음에 실행될 명령어의 주소를 가리키는 레지스터 값. 프로세스가 다시 실행될 때 이 위치에서부터 명령어가 실행된다.

#### 레지스터 상태:

프로세스가 일시 중단되거나 다시 실행될 때 레지스터 값이 PCB로 저장되어 관리된다.

#### 프로세스 우선순위(Process Priority):

프로세스의 중요도나 우선순위를 나타내는 값. 스케줄러는 이 값을 기반으로 CPU를 할당하는 우선순위를 결정한다.

#### 프로세스 계정 정보(Process Accounting Information):

프로세스의 실행 시간, CPU 사용량 등과 같은 성능 관련 정보를 저장.

#### 메모리 관리 정보(Memory Management Information):

프로세스가 사용하는 메모리 영역에 대한 정보를 포함하며 코드 섹션, 데이터 섹션, 스택, 힙 등의 정보가 여기에 기록된다.

#### 입출력 상태 및 정보(I/O Status and Information):

프로세스가 사용하는 입출력 장치와 관련된 상태 및 정보를 저장한다. 프로세스가 대기 상태로 전환되었을 때 이 정보는 입출력 완료를 기다리는 동안 사용된다.

## 3. 프로세스의 상태

#### 생성 (Creation) 상태

프로세스가 생성되고, 초기화 작업이 이루어지는 단계.
이 단계에서는 PCB(Process Control Block)가 생성되고 초기 상태로 설정된다.

#### 준비 (Ready) 상태

프로세스가 실행을 기다리며, CPU를 할당받을 준비가 된 상태.

#### 실행 (Running) 상태

CPU를 할당받아 명령어를 실행하는 상태. 프로세스가 CPU에서 명령어를 수행하는 상태.

#### 대기 (Waiting 또는 Blocked) 상태

프로세스가 어떤 이벤트를 기다리는 동안에 있는 상태.

#### 종료 (Termination) 상태:

프로세스가 작업을 마치고 종료되거나, 다른 이유로 인해 종료된 상태.
종료된 프로세스는 시스템에서 제거된다.
