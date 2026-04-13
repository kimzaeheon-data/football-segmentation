# ⚽ Football Segmentation: U-Net vs YOLO Seg

축구 경기 장면에서 다양한 객체를 분할하고,  

**U-Net**과 **YOLOv8 Segmentation** 모델의 성능을 비교한 프로젝트입니다.

## 프로젝트 개요

본 프로젝트는 축구 경기 이미지에서 골대, 심판, 광고판, 그라운드, 공, 관중, 골키퍼, 팀 선수 등 다양한 객체를 분할하는 문제를 다룹니다.

두 가지 접근을 비교했습니다.

- **U-Net**: pixel-level semantic segmentation
- **YOLOv8 Segmentation**: instance segmentation 기반 접근

작은 규모의 데이터셋 환경에서 두 모델의 성능 차이를 확인하고, 어떤 방식이 더 실용적인지 분석하는 것을 목표로 했습니다.

---

## 데이터셋

- **Dataset**: Football Semantic Segmentation Dataset
- **Images**: 100장
- **Classes**: 11개

### 클래스 목록
- Goal Bar
- Referee
- Advertisements
- Ground
- Ball
- Coaches & Officials
- Audience
- Goalkeeper B
- Goalkeeper A
- Team B
- Team A

### 데이터 특징
- 클래스 불균형이 존재함
- 공(ball)처럼 매우 작은 객체가 포함됨
- 광고판, 관중, 그라운드처럼 넓은 영역 클래스가 존재함
- 데이터 수가 적어 모델 일반화가 어려운 환경임

---

## 문제 정의

축구 경기 장면에서 다양한 객체를 **픽셀 단위로 분할**하는 문제입니다.

주요 난점은 다음과 같습니다.

- 데이터 수가 적음
- 클래스 불균형이 심함
- 공(ball)과 같은 작은 객체 탐지가 어려움
- 관중, 광고판, 필드 등 복잡한 배경 클래스가 존재함

---

## 모델

### 1) U-Net
- Encoder-Decoder 구조
- Skip Connection 사용
- Loss: CrossEntropy + Dice Loss
- 입력 크기: 256 x 512

### 2) YOLOv8 Segmentation
- Pretrained backbone 사용
- Instance segmentation 기반
- 적은 데이터에서도 상대적으로 안정적인 학습 가능

---

## 학습 설정

### U-Net
- Epochs: 30
- Batch size: 2
- Augmentation:
  - Resize
  - Horizontal Flip
  - Brightness / Contrast
  - Shift / Scale / Rotate

### YOLOv8 Segmentation
- Model: `yolov8n-seg.pt`
- Epochs: 50
- Image size: 640

---

## 결과

### 정량적 결과

| Model | Metric | Result |
|------|--------|--------|
| U-Net | Validation IoU | 약 0.50 |
| YOLOv8 Seg | mAP50 | 약 0.50 |
| YOLOv8 Seg | mAP50-95 | 약 0.33~0.37 |

YOLOv8 Segmentation은 적은 데이터 환경에서도 안정적인 객체 분할 성능을 보였고, U-Net은 데이터 부족과 클래스 불균형의 영향을 더 크게 받았다.

### 정성적 비교

| Input | Ground Truth | U-Net | YOLO |
|------|-------------|-------|------|
| ![](data_sample/input.png) | ![](data_sample/gt.png) | ![](data_sample/unet.png) | ![](data_sample/yolo.png) |

---

## 분석

### U-Net
- 전체 구조를 학습하려는 경향은 보였지만, 실제 예측에서는 클래스 구분이 불안정했음
- 데이터 수가 적고 클래스 불균형이 심한 환경에서 성능 한계가 뚜렷했음
- semantic segmentation 모델 특성상 충분한 데이터가 더 필요함

### YOLOv8 Segmentation
- 선수, 골키퍼, 광고판, 그라운드 등 주요 객체를 비교적 안정적으로 분할했음
- pretrained backbone의 이점을 바탕으로 적은 데이터에서도 강한 성능을 보였음
- instance segmentation 기반 접근이 본 데이터셋 환경에서 더 실용적이었음

---

## 핵심 인사이트

데이터가 제한된 환경에서는  
**pixel-level segmentation 모델(U-Net)** 보다  
**pretrained 기반 instance segmentation 모델(YOLO Segmentation)** 이 더 안정적인 성능을 보였다.

즉, 모델 성능은 아키텍처 자체보다도  
**데이터 규모, 사전학습 여부, 문제 구조**에 크게 영향을 받는다는 점을 확인했다.

---

## 향후 개선 방향

- U-Net에 pretrained encoder 적용
- 클래스 imbalance 보정
- 더 강한 augmentation 적용
- class-wise IoU 기반 정밀 비교
- 더 큰 축구 segmentation 데이터셋으로 확장 실험

---

## Tech Stack

- Python
- PyTorch
- Ultralytics YOLOv8
- Albumentations

## Insight
데이터가 제한된 환경에서는 pixel-level segmentation 모델(U-Net)보다  
pretrained 기반의 instance segmentation 모델(YOLO Seg)이 더 안정적인 성능을 보였다.

- OpenCV
- Matplotlib

---

## 결론

본 프로젝트를 통해 semantic segmentation과 instance segmentation의 차이를 실제 데이터셋에서 비교할 수 있었다.

특히,  
**작은 데이터셋에서는 YOLO Segmentation이 더 강한 baseline이 될 수 있으며**,  
U-Net은 더 많은 데이터와 pretrained encoder가 결합될 때 성능 향상 가능성이 크다는 점을 확인했다.
