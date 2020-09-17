## Abstract

-   딥 러닝 방법을 사용하여 데이터 기반 HR 추정 모델을 구축
-   얼굴 비디오에서 원격 HR 추정을위한 RhythmNet을 소개

## Method

-   RhythmNet의 Framework
    -   얼굴 랜드 마크 감지를 수행 -> 관심 영역 (ROI) 얼굴 영역
    -   이 영역에서 공간-시간지도가 HR 신호의 새로운 저수준 표현으로 계산
    -   CNN-RNN 모델 : 공간-시간 맵에서 HR 로의 매핑을 학습하도록 설계

**A. Face Detection, Landmark Detection and Segmentation**

-   SeetaFace(오픈 소스) -> 81개 얼굴 랜드 마크 감지 (매 프레임 마다 진행) : 더욱 안정적인 랜드 마크 위치를 얻기 위해 프레임에 걸쳐 81 개의 얼굴 랜드 마크에 이동 평균 필터도 적용
-   심장 박동으로 인한 색상 변화를 포함하는 모든 유익한 부분을 최대한 활용하기 위해 추가 처리를 위해 얼굴 전체를 사용
-   눈 중심점을 사용하여 얼굴 정렬을 수행 -> 너비 w (w는 바깥 쪽 볼 테두리 지점 사이의 수평 거리)와 높이 1.2 \* h (h는 턱 위치와 눈썹 중앙 사이 수직 거리)로 얼굴 경계 상자를 정의
-   얼굴 아닌 영역 제거하기 위해 skin segmentation이 정의된 ROI에 적용
-   resize하면 HR 신호에 노이즈가 발생할 수 있으므로 추가 처리를 위해 얼굴 랜드 마크를 기반으로 자른 원래 얼굴 영역을 사용

**B. Spatial-temporal Map for Representing HR Signals**

-   얼굴 영상에서 rPPG 기반 HR 추정에 유일하게 유용한 정보는 심장 박동으로 인한 혈관 내 혈액의 부피 및 산소 포화도 변화로 인한 피부색 변화 -> 한계 : 진폭이 작고, 움직임, 조명, 센서 소음에 영향 받을 수 있다
-   이전(대부분) : 평균풀링표현 : HR 신호의 SNR을 개선하기 위해 얼굴 전체의 RGB 채널의 평균 픽셀 값을 HR 신호 표현으로 사용
-   시간-공간 맵 : 후속 네트워크에 대한 입력을 심장 리듬 신호만큼 구체적으로 강제가능, CNN을 활용하여 최종 HR 추정 작업에 대한 정보 표현을 학습 가능
-   min-max normalization \[0 ~ 255\]
-   color space selection : YUV
-   partially masked spatial temporal maps as augmented data : randomly mask a small part of the spatial-temporal maps partially masked

**C. Temporal Modeling for HR measurement**

1.  백복 CNN : RESNET18 -> feature 추출
2.  1계층 GRU
3.  fully connected layer for regress HR value
4.  RNN : stable HR

## Experiments

**A. Database and Experimental Setting**

-   Metrics
    -   Mean, std of HR error, mean absolute HR error (MAE), the root mean squared HR error (RMSE), the mean of error rate percentage (MER), and Pearson’s correlation coefﬁcients r
-   temporal sliding window
-   The number of the ROI blocks : 25 (5x5 grids)
-   six adjacent estimated HRs are used to compute the Lsmooth
-   balance parameter λ
-   Adam solver, initial learning rate of 0.001, epoch number to 50

**B. Intra-database Testing**  
**1) Experiments on Color Face Videos**

-   최근 딥러닝 방식
    -   \[38\] Z. A. Carreira J, “Quo vadis, action recognition? a new model and the kinetics dataset,” Proc. IEEE CVPR, pp. 6299–6308, 2017.
    -   \[26\] Chen and D. Mcduff, “Deepphys: Video-based physiological measurement using convolutional attention networks,” Proc. ECCV, pp. 356– 373, 2018.
    -   한계 : NIR 비디오에서 작동될 수 없다.
-   딥러닝 사용한 모델이 기존 방법보다 좋고, 특히 RhythmNet 은 데이터 수 작을 떄 robust

**2) Experiment on NIR Face Video**

-   채널 1개 -> 1 channel spatial-temporal map
-   성능 : RGB face videos >>> NIRS face videos
-   한계 : face detector 이 NIR비디오에서 작동안함
