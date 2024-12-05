---
title: 포트폴리오
author: yeonwlee
date: 2024-12-05 22:36:00 +0900
categories: [Blogging]
pin: true
tags: [포트폴리오]
---


- [1. AI 직원과 AI 통역가를 통해 언어의 한계를 뛰어넘은 메타버스 컨벤션](#1-ai-직원과-ai-통역가를-통해-언어의-한계를-뛰어넘은-메타버스-컨벤션)
- [2. 스크린샷을 DB화, 검색 및 요약 기능 제공](#2-스크린샷을-db화-검색-및-요약-기능-제공)
- [3. 고양이 꼬리의 모양을 통해 감정을 분석](#3-고양이-꼬리의-모양을-통해-감정을-분석)
- [4. 영문 텍스트에서 MBTI 추측](#4-영문-텍스트에서-mbti-추측)


---

<br>

## 1. AI 직원과 AI 통역가를 통해 언어의 한계를 뛰어넘은 메타버스 컨벤션
[노션 링크 - 메타 컨벤션](https://amethyst-leather-286.notion.site/2024-10-01-2024-12-16-11052bb25d1b80ecac18dd664b58bc26)

**기간**:  2024.10.01~2024.12.16(약 10주)

**기술 스택**: Python, PyTorch, LangChain, FAISS, Docker, Docker Compose, AWS,..
- **Python**: 데이터 처리 및 서버 구현
- **PyTorch**: 추천 시스템 구현시, 트랜스포머 계열 임베딩 모델 사용
- **LangChain**: LLM과의 인터페이스 구축
- **FAISS**: 대용량 벡터 검색을 위한 라이브러리, 빠른 유사도 검색을 위해 선택
- **Docker & Docker Compose**: 마이크로서비스 컨테이너화 및 관리
- **AWS (EC2)**: 클라우드 환경에서의 서비스 배포 및 스케일링

**맡은 역할**: 기업 데이터 크롤링, 추천 시스템, AI 직원과 바이어의 대화 요약 시스템

**Github**: [https://github.com/meta-convention-mtvs/chat-server](https://github.com/meta-convention-mtvs/chat-server)



## 2. 스크린샷을 DB화, 검색 및 요약 기능 제공
[노션 링크 - 스크린샷 기반 개인화DB](https://amethyst-leather-286.notion.site/2024-07-29-2024-08-20-DB-3e846bc4e8774593974b01e80aef05d8)

**기간**:  2024.07.29~2024.08.20(약 3주)

**기술 스택**: Python, PyTorch, OpenCV, Paddle ocr, LangChain, FAISS, Gradio,..
- **Python**: 데이터 처리 및 모델 사용
- **PyTorch**: 트랜스포머 계열 모델을 활용하여 텍스트 임베딩 및 자연어 처리 작업 수행
- **OpenCV**: 이미지 전처리 작업에 사용
- **Paddle ocr:** OCR 처리에 사용
- **LangChain:** LLM과의 인터페이스 구축에 사용
- **FAISS**: 대용량 벡터 검색을 위한 라이브러리, 빠른 유사도 검색을 위해 선택
- **Gradio:**  사용자 인터페이스 구축에 사용. 팀원들이 구현한 앱과 기능을 연결하여 사용자가 쉽게 접근할 수 있도록 지원

**맡은 역할**: 스크린샷 이미지 전처리, 여러 모델을 활용한 기초적 구성 구현(전원 실행), 팀원들이 구현한 Gradio 앱과 기능 간의 연결


## 3. 고양이 꼬리의 모양을 통해 감정을 분석
[노션 링크 - cattail language](https://amethyst-leather-286.notion.site/2024-07-15-2024-07-26-cattail-language-6c2022b3f4994905b015ca9a8d20922f)

**기간**:  2024.07.15~2024.07.26(약 2주)

**기술 스택**: python, pytorch, optuna,..
- **Python**: 데이터 처리 및 모델 사용
- **PyTorch**: 딥러닝 모델 사용
- **Optuna**: 하이퍼 파라미터 튜닝
  
**맡은 역할**: 데이터 수집(크롤링, 이미지 생성), 모델 개발(모델 선택, 하이퍼 파라미터 튜닝)


## 4. 영문 텍스트에서 MBTI 추측
[노션 링크 - 글에서 네가 보여](https://amethyst-leather-286.notion.site/2024-07-02-2024-07-12-6aa8f790f3884b5abdb49573dd0ebeed)

**기간**: 2024.07.02~2024.07.12(약 2주)

**기술 스택**: python, pandas, pytorch, sklearn, optuna,..
- **Python**: 데이터 처리 및 모델 사용
- **Pandas**: 데이터 전처리 및 분석
- **Sklearn**: TF-IDF 벡터화, 모델 구현, 평가 지표 계산 등 여러 작업에 활용
- **Optuna:** 하이퍼파라미터 튜닝
  
**맡은 역할**: 모델 개발(데이터 전처리, 모델 선택, 하이퍼파라미터 튜닝), 발표

