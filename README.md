# Heart-rate-monitoring-using-non-contact-photoplethysmogram-from-infrared-face-video

## 요약
- 적외선 안면 비디오를 통해서 rPPG를 추출해 심박수 모니터링을 할 수 있도록 심박(심박변이도) 추정


- RGB 카메라가 아닌 IR 카메라로 찍은 비디오를 이용
- remote PPG(rPPG : 비접촉 PPG) 를 추출해 heart rate 추정

## 목표
1. 공용데이터베이스(VIPL-HR)로 기존에 있는 모델보다 성능 높은 딥러닝 모델 구축 -> 인공신경망 Termproject로 진행할 예정
2. 모니터링이 가능할 정도로의 순간 심박 추정 가능하도록
3. 자체 데이터셋을 수집해 모델 구축(ECG identity 활용, personalizedGAN 등)
