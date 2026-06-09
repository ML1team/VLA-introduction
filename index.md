---
layout: default
title: "VLA-RFT: World Model-Based RL Fine-Tuning for Robot Manipulation"
description: "Reinforcement Fine-Tuning of Vision-Language-Action Models via World Model Rewards"
---

<div align="center">

# VLA-RFT: World Model-Based RL Fine-Tuning for Robot Manipulation

**Reinforcement Fine-Tuning of Vision-Language-Action Models via World Model Rewards**

[㈜플레이오니 (Pleiony)](https://pleiony.com)

<!-- 논문 링크가 생기면 아래 뱃지 활성화 -->
<!-- [![Paper](https://img.shields.io/badge/Paper-arXiv-red)](링크) -->
[![Weights](https://img.shields.io/badge/Weights-Coming%20Soon-blue)](#model-weights)
[![License](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey)](https://creativecommons.org/licenses/by-nc-nd/4.0/)

</div>

---

## 개요 (Overview)

로봇이 새로운 환경에서 새로운 태스크를 수행하려면, 방대한 데이터 재수집 없이도 빠르게 적응할 수 있어야 합니다.

본 프로젝트는 **VLA-RFT** 프레임워크를 구현하여, 사전학습된 Vision-Language-Action (VLA) 모델에 강화학습(RL) 파인튜닝을 적용합니다. 핵심 아이디어는 실제 로봇 대신 **월드 모델(VJEPA2-AC)**을 보상 신호원으로 활용하여, 추가 로봇 실험 없이도 정책 모델의 성능을 향상시키는 것입니다.

**주요 결과:**
- VLA-Adapter: SFT 기준 성공률 85% → RFT 후 **95%** (+10%p)
- OpenVLA-OFT: SFT 기준 성공률 80% → RFT 후 **85%+**
- 조명 변화 강건성 실험: 18회 중 16회 성공 (**88.9%**)

---

## 실험 설정 (Experimental Setup)

### 로봇 환경

| 항목 | 내용 |
|------|------|
| 로봇 | NURI4s |
| 태스크 | Pick-and-Place (테이블 위 블록을 지정 위치로 이동) |
| 관측 | RGB 이미지 (사이드 카메라 + 손목 카메라) |
| 행동 | 로봇 엔드이펙터 6DOF 제어 |
| 언어 지시 | "Pick up the {color} block and place it on the plate" |

### 데이터셋

자체 수집한 인간 시연 데이터로 학습하였습니다.

| 버전 | 데이터 수 | 설명 |
|------|---------|------|
| V1.0 | 4,550건 | 초기 수집 (싱글 블록) |
| V1.1 | 4,940건 | 다중 블록 추가 |
| V1.2 | 1,250건 | 다양화 수집 |
| **합계** | **10,740건** | |

- 파일 포맷: `tfrecord` (VLA 모델), `MP4 + npy` (월드 모델)

### 학습 모델

#### 정책 모델 (Policy Model)

| 모델 | 파라미터 | 설명 |
|------|---------|------|
| VLA-Adapter | 0.5B | L1 기반 VLA, LoRA 적용 |
| OpenVLA-OFT | 7B | OXE 데이터셋 사전학습 |
| Dita | 0.3B | 경량 VLA 모델 |

#### 월드 모델 (World Model)

| 모델 | 설명 |
|------|------|
| VJEPA2-AC | Meta DROID 가중치 기반 파인튜닝, RL 보상 신호 생성 |

### 학습 프레임워크

2단계 학습 파이프라인:

```
1단계 — Supervised Fine-Tuning (SFT)
  인간 시연 데이터 → VLA 모델 사전학습
  월드 모델: VJEPA2-AC 파인튜닝

2단계 — Reinforcement Fine-Tuning (RFT)
  VLA 모델을 Stochastic Policy로 확장
  월드 모델이 각 액션에 보상(Reward) 계산
  GRPO 기반 RL 파인튜닝 수행
```

---

## 결과 (Results)

### 성능 비교

#### VLA-Adapter 계열

| 모델 | 싱글 블록 SR | 비고 |
|------|------------|------|
| VLA-Adapter A | 47.5% | 초기 버전 |
| VLA-Adapter B | 80% | 데이터·설정 개선 |
| VLA-Adapter C | 85% | SFT 최종 |
| **VLA-Adapter RFT C** | **95%** | **RL 파인튜닝 후 최고 성능** |

#### OpenVLA-OFT 계열

| 모델 | 싱글 블록 SR | 비고 |
|------|------------|------|
| OpenVLA-OFT (SFT) | 80% | 파인튜닝 후 |
| **OpenVLA-OFT RFT** | **85%+** | **RL 파인튜닝 후** |

### 조명 변화 강건성 실험

VLA-Adapter RFT C 모델을 대상으로, 학습에 사용되지 않은 다양한 조명 조건에서 성능을 평가하였습니다.

| 조건 | 색온도 | 밝기 | 결과 |
|------|--------|------|------|
| 실내 조명 ON | 3000K / 4500K / 6000K | 100% | 성공 |
| 실내 조명 OFF | 3000K / 4500K / 6000K | 100% | 성공 |
| 실내 조명 OFF | 3000K / 4500K / 6000K | 50% | 일부 실패 |
| **종합** | | | **18회 중 16회 성공 (88.9%)** |

---

## 시연 영상 (Demonstrations)

VLA-Adapter RFT C 모델의 Pick-and-Place 작업 시연 영상입니다.

<div align="center">

<iframe width="560" height="315" src="https://www.youtube.com/embed/OmYyQRj4tNA" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

[![YouTube](https://img.shields.io/badge/YouTube-시연영상-red?logo=youtube)](https://youtu.be/OmYyQRj4tNA)

</div>

---

## 공개 가중치 (Model Weights)

최고 성능 모델의 가중치를 공개합니다.

| 모델 | 파라미터 | 싱글 블록 SR | 다운로드 |
|------|---------|------------|---------|
| **OpenVLA-OFT RFT A** | 7B | **85%+** | [Google Drive](https://drive.google.com/drive/folders/1VhFt7aMHkC26eGsWae3HKP3Bh0sxeJbb?usp=sharing) |

> [![License](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey)](https://creativecommons.org/licenses/by-nc-nd/4.0/)  
> 본 가중치는 **CC BY-NC-ND 4.0** 라이선스 하에 공개됩니다.  
> 비상업적 용도에 한하여 사용 가능하며, 수정 및 재배포는 허용되지 않습니다.

---

## 인용 (Citation)

```bibtex
@misc{pleiony2026vlardt,
  title     = {VLA-RFT: World Model-Based Reinforcement Fine-Tuning for Robot Manipulation},
  author    = {Pleiony},
  year      = {2026},
  url       = {https://pleiony.github.io/vla-rft}
}
```

---

## 문의

- 기관: ㈜플레이오니 (Pleiony)
- 이메일: [yearnsh@pleiony.com](mailto:yearnsh@pleiony.com)
- GitHub: [github.com/pleiony](https://github.com/pleiony)
