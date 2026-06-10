---
layout: default
title: "Language-Guided Robot Manipulation via Foundation Model Fine-Tuning"
description: "Enabling new robots to perform complex manipulation tasks through natural language instructions"
---

<div align="center" markdown="1">

# Language-Guided Robot Manipulation<br>via Foundation Model Fine-Tuning

**신규 로봇을 자연어 지시만으로 제어하는 AI 솔루션**

[㈜플레이오니 (Pleiony)](https://pleiony.com)

<!-- [![Paper](https://img.shields.io/badge/Paper-arXiv-red)](링크) -->
[![Weights](https://img.shields.io/badge/Model-Download-blue)](#model-weights)
[![License](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
[![GitHub](https://img.shields.io/badge/GitHub-pleiony-black?logo=github)](https://github.com/pleiony)

</div>

---

## 소개 (Overview)

기존 산업용 로봇은 환경이 조금만 바뀌어도 전문가가 직접 다시 티칭해야 합니다. 이로 인한 라인 중단 비용과 전문 인력 의존도는 스마트 팩토리 도입의 큰 장벽이 되어왔습니다.

본 프로젝트는 **비전-언어-행동(VLA) 파운데이션 모델을 파인튜닝**하여, 자사 로봇 NURI4s가 자연어 지시만으로 새로운 태스크에 유연하게 적응할 수 있도록 합니다.

> "빨간 블록을 집어서 접시 위에 올려줘"

위와 같은 사용자 프롬프트를 이해하고, 로봇이 즉시 행동으로 옮기는 End-to-End 제어 시스템을 구현하였습니다.

---

## 주요 기능 (Key Capabilities)

| 기능 | 설명 |
|------|------|
| **자연어 지시 이해** | 색상·위치 등 언어 명령을 실시간으로 해석하여 로봇 동작 생성 |
| **빠른 신규 로봇 적용** | LoRA 기반 효율적 파인튜닝으로 소량 데이터만으로도 신규 로봇에 빠른 적응 |
| **다양한 환경 강건성** | 조명 변화·물체 배치 변화에 강건한 동작 수행 |
| **멀티모달 인식** | 사이드·손목 카메라의 RGB 이미지와 언어 명령을 융합한 제어 |

---

## 시스템 구성 (System)

**로봇 환경**

| 항목 | 내용 |
|------|------|
| 로봇 | NURI4s |
| 태스크 | Pick-and-Place (블록을 언어 지시에 따라 목표 위치로 이동) |
| 입력 | RGB 이미지 + 자연어 명령 |
| 출력 | 로봇 엔드이펙터 실시간 제어 |

**학습 데이터**

숙련된 작업자가 직접 시연한 데이터를 수집하여 학습에 활용하였습니다.

- 총 수집 에피소드: 약 2,200건 이상 (다양한 블록 색상·배치·조명 조건)
- 데이터 형태: RGB 이미지 + 자연어 명령 + 로봇 액션이 동기화된 멀티모달 데이터

---

## 결과 (Results)

파인튜닝 + 강화학습 기반 추가 학습을 통해 아래 성능을 달성하였습니다.

| 모델 | 싱글 블록 성공률 (SR) |
|------|-------------------|
| 파인튜닝 전 (사전학습 모델) | 0% (신규 로봇 미적응) |
| 파인튜닝 후 (SFT) | 80% |
| **최종 모델 (SFT + RL 파인튜닝)** | **93.3%** |

**조명 변화 강건성**: 학습에 사용하지 않은 다양한 조명 조건(색온도 3000K–6000K, 밝기 50–100%) 에서 **18회 중 16회 성공 (88.9%)** 달성

---

## 시연 영상 (Demo)

<div align="center" markdown="1">

<iframe width="560" height="315" src="https://www.youtube.com/embed/OmYyQRj4tNA" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

[![YouTube](https://img.shields.io/badge/YouTube-시연영상-red?logo=youtube)](https://youtu.be/OmYyQRj4tNA)

</div>

---

## 공개 모델 (Model Weights)

최종 학습 모델의 가중치를 공개합니다.

| 모델 | 파라미터 | 성공률 | 다운로드 |
|------|---------|-------|---------|
| **최종 모델 (SFT + RL 파인튜닝)** | 7B | **93.3%** | [Google Drive](https://drive.google.com/drive/folders/1VhFt7aMHkC26eGsWae3HKP3Bh0sxeJbb?usp=sharing) |

> [![License](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey)](https://creativecommons.org/licenses/by-nc-nd/4.0/)  
> 본 가중치는 **CC BY-NC-ND 4.0** 라이선스 하에 공개됩니다.  
> 비상업적 용도에 한하여 사용 가능하며, 수정 및 재배포는 허용되지 않습니다.

---

## 향후 계획 (Future Work)

- 제조·물류 자동화 라인 실증 테스트
- 다양한 로봇 암(Arm) 기종으로 호환성 확장
- API 서버 기반 SaaS 서비스화

---

## 팀 (Team)

**㈜플레이오니 (Pleiony)**

| 이름 | 역할 |
|------|------|
| 연승호 | 총괄 책임 |
| 양윤석 | 모델 설계 |
| 민창기 | 모델 설계 |
| 임재규 | AI 개발 |

---

## 문의 (Contact)

- 이메일: [yearnsh@pleiony.com](mailto:yearnsh@pleiony.com)
- GitHub: [github.com/pleiony](https://github.com/pleiony)
