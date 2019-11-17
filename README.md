# 자율 주행 도로 청소차 PO-E

## 개발 환경
* OS : Ubuntu16.04, Rasbian
* Virtual Enviroment : Anaconda
* Hardware : DonkeyCar, Raspberri-pi 2대, Logitech Webcam, ServoMotor
* Software : cuDnn 5.1, OpenCV 3.4
* Language : Python3.6, C++
* Library : CUDA 8.0, YOLOv3, YOLOMark, mjpg-Stream(실시간 스트리밍 기능), Keras Library(Categorical Model)

## 구현 기술
### 자율주행 
* 자율 주행 RC카를 통해 자율 주행을 구현한다. 
* 라즈베리 파이(1번)를 통해 차체 전방의 도로를 인식하고 서버에서 모델링을 통해 훈련된 결과를 전송하여 자율주행을 구현

### 사물 인식 
* 라즈베리 파이(2번)를 통해 도로 위에 뿌려져 있는 쓰레기들을 인식하고 인식한 결과를 토대로 청소기의 구동 여부를 결정한다.
* OpenCV와 CUDA, Cudnn로 환경 설정 후 YOLO모듈 구현 및 YOLOMark를 이용하여 쓰레기를 학습한 뒤 주행 시 인식하도록 구현.

### 청소기 구동
* 물체 인식을 통한 결과를 받는다. 만약 도로 위에 인식된 물체가 쓰레기로 판정 될 경우 청소기에 장착된 모터를 돌려 청소기를 가동시킨다. 
* 3D프린트로 청소 차체 제작 및 Detection결과를 전달받아 모터 구현.

## 프로젝트 일정
![Main](https://github.com/hyunhee7/4-_PO-E/blob/master/screenshot/%EC%9D%BC%EC%A0%95.png)

## PO-E 구조도
![struct](https://github.com/hyunhee7/4-_PO-E/blob/master/screenshot/structure.png)

## 자율주행 모델 구축 및 훈련
  1. Linux에 DonkeyCar 파일 install
  
  2. Raspberry pi에 os설치
  
  3. Raspberry pi 통신을 위한 인터넷 설정 (공유기 설치)
  
  4. Raspberry pi 초기설정
  
  5. donkey car app 설정(raspberry pi)
  
  6. 운전 및 학습
    - git clone https://github.com/autorope/donkeycar 
    
    - OpenCV, CuDNN, CUDA 설치
    
    - <설정한 IP주소>:8887 로 접속 후 운전 확인 
    
    - 조이스틱으로 운전하여 Webcam으로 Data수집 (약 20,000장 사진 수집)
    
    - rsync -r pi@<your_pi_ip_address>:~/mycar/data/  ~/mycar/data/ 로 수집한 사진을 서버(웍)로 이동
    
    - python ~/mycar/manage.py train --model ~/mycar/models/<파일이름>.h5 로 모델 생성. 
      이때, myconfig.py에서 다양한 모델을 생성해볼 수 있다.

## Object Detection
  1. 파이 내 Real-time detection 시도 (하드웨어 성능으로 인한 실패)
  
  2. mjpg-Streamer를 이용한 서버와의 HTTP 통신
  
    - git clone https://github.com/jacksonliam/mjpg-streamer.git
  
  3. YOLOMark를 통한 MODEL 학습
  
    - git clone https://github.com/AlexeyAB/Yolo_mark
  
    - Detection할 이미지 파일에 Bounding-Box를 그려서 Labeling 작업을 해야한다. PO-E는 수집한 약 1000장의 trash이미지로 수행하였다.
    
    - DataSet을 모두 만들면 yolov3.cfg 파일 생성 후 class, filters 등의 설정작업 수행
    
    - darknet convolution layer 다운로드 후, 학습 실행 (GPU 2개를 이용해 학습함)
    
  4. 모델 학습 결과
  
      ![res](https://github.com/hyunhee7/4-_PO-E/blob/master/screenshot/trash_detection.jpg)


## 모터 구동 및 쓰레기 수집
