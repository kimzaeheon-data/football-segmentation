# Football Segmentation

U-Net과 YOLO Seg를 비교한 축구 장면 segmentation 프로젝트입니다.

## Models
- U-Net
- YOLOv8 Segmentation

## Key Result
- U-Net: 데이터가 적은 환경에서 성능 제한
- YOLO Seg: pretrained 기반으로 더 안정적인 결과

## Tech Stack
- PyTorch
- Ultralytics YOLO
- Albumentations

## Insight
데이터가 제한된 환경에서는 pixel-level segmentation 모델(U-Net)보다  
pretrained 기반의 instance segmentation 모델(YOLO Seg)이 더 안정적인 성능을 보였다.
