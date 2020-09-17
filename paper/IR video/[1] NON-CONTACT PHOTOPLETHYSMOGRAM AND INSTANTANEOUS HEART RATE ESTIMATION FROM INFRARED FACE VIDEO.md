## 용어

-   iHR : 순간 심박수

## 요약

-   최근 얼굴 영상에서 순간 심박수 추출은 많이 연구되고 있다(rPPG) -> 한계 : 조명에 민감
-   적외선 카메라를 사용하여 iHR 모니터링 가능 -> 적외선 얼굴 비디오에서 iHR을 복구하는 간단하고 원칙적인 신호 추출 방법을 제시
-   7명의 참가자 대상으로 테스트, 동시에 PPG, 심전도(ECG)와 적외선 얼굴 비디오(IR face video) 기록
-   코드 제공([https://github.com/natalialmg/IR\_iHR](https://github.com/natalialmg/IR_iHR))

## Introduction

-   최근의 적외선 얼굴 비디오를 사용하여 심박수 측정 연구의 한계 : 평균 심박수 추정(30초 이상에 걸친)으로 제한 \[14-15\]
-   이 논문 : 제어 된 모션 조건에서 기본적인 시공간 분석 및 시간-주파수 분석을 사용하여 1 초 미만의 iHR 근사값을 추출가능

## Methods

### NON-CONTACT PPG SIGNAL FROM IR VIDEO

1.  얼굴 감지하고 분리된 공간영역으로 분할
2.  각 지역의 평균 활동 취하여 이를 제거하고 더 작은 소스의 하위 집합으로 분해
3.  신호 품질 지수를 도입하여 관심있는 신호 선택해 rPPG신호 구성

-   Input IR video
    
-   Preprocessing the IR video
    
-   Low rank spatiotemporal model
    
-   Determine important sources
    
-   Reconstructing the non-contact PPG signal
    
-   Estimation of the instantaneous heart rate
    

## Experiments

-   9명의 동시 ECG, PPG 및 IR 얼굴 비디오를 획득 -> 최종 7명
-   표준 환자 모니터 사용 (Philips IntelliVue MP70 Patient 모니터) 및 Microsoft Kinect 카메라 사용
-   Kinect 카메라와 환자 모니터의 시계는 1 초의 시간 정확도로 동기화
-   피험자들은 카메라를 똑바로 쳐다보고 안정된 자세를 유지하되 그렇지 않으면 정상적으로 행동하고 눈을 깜빡이고 숨을 쉬도록 요청받음
-   순간 심박수 (iHR)는 프로세스를 사용하여 IR 비디오에서 추정
-   Ground Truth iHR은 파이썬 라이브러리 biosppy \[20\]에 구현 된 R-peak 탐지 알고리즘을 사용하여 ECG 신호에서 추출

## Results

-   각 데이터 세트에 대해 복구 된 iHR 신호 및 Ground Truth 간의 RMWSE(root mean square error) 및 상대 오차 계산
-   복구 된 비접촉 PPG (PPGIR) 및 해당 STFT 표시, 스펙트로 그램은 복구 된 IR iHR과 Ground Truth iHR 사이에 좋은 일치  

## Results

-   매트릭스 분해를 기반으로 한 간단하고 원칙적인 방법이 피험자가 상대적으로 고정되어있을 때 1 초 단위로 상대적 오류가 작은 순간 심박수를 회복하기에 충분하다는 것을 보여줌 -> 비접촉 PPG에 대한 IR의 실행 가능성을 시사
-   특히 기존 RGB 방법과 비교하여 일반적으로 IR의 저조도 및 다양한 조명 성능을 고려할 때. 적절하게하려면 추가 작업이 필요
-   motion artifacts 에 대한 강력한 수정이 필요
-   단수 벡터를 결합하여 최종 혈역학 적 추정치를 얻는 과정을 개선가능
-   근적외선 스펙트럼에서 관심있는 생물학적 과정의 흡수 곡선의 특성화에 대한 더 많은 연구가 수행되어햐 함
-   획득 한 비접촉 PPG 신호를 추가로 분석하기 위해보다 정교한 시간-주파수 표현 도구를 고려가능
-   보다 일반적인 매니 폴드 학습 알고리즘 및 매트릭스 노이즈 제거 기술을 적용하여 모션 및 시간 지연을 캡처 가능
