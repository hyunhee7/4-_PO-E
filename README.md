# 자율 주행 청소차 PO-E

## 개발 환경
* OS : Ubuntu16.04, Rasbian
* Virtual Enviroment : Anaconda
* Hardware : DonkeyCar, Raspberri-pi 2대, Logitech Webcam, ServoMotor
* Software : cuDnn 5.1, OpenCV 3.4
* Language : Python3.6, C++
* Library : CUDA 8.0, YOLOv3, YOLOMark, mjpg-Stream(실시간 스트리밍 기능), Keras Library(Categorical Model)

## 구현 기술
* 자율주행 : 도로 주행을 지도학습하여 Keras로 모델 생성 후 자율주행 구현
* 사물 인식 : OpenCV와 CUDA, Cudnn로 환경 설정 후 YOLO모듈 구현, YOLOMark를 이용하여 쓰레기를 학습한 뒤 주행 시 인식하도록 구현.
* 청소기 구동 : 3D프린트로 청소 차체 제작 및 Detection결과를 전달받아 모터 구현.

