# ImageEnhance-website
영상 화질 개선 모델 + 웹 사이트

![Uploading image.png…]()



## 프로젝트 소개
비전 트랜스포머(Vistion Transformer; ViT)를 기반으로 하여 영상 화질을 개선하고
해당 모델에 대해 테스트 해보기 위해 제작한 웹 사이트입니다.

## 프로잭트 진행 기간
23.02. ~ 23.06.

### 사용된 기술 및 도구
- **python**:  프로그래밍 언어
- **Streamlit**:  웹 사이트 제작 및 사용자 인터페이스 개발을 위한 프레임워크
- **FastAPI**:  웹 서버 개발을 위한 프레임워크
- **OpenCV**:  이미지 처리 라이브러리
- **Pytorch**:  딥러닝 프레임워크

## 주요 내용
**1. 모델**
1. 비전 트랜스포머와 색상 변환 함수를 통해 전역적 조정을 수행합니다.
2. U-Net 구조 기반의 네트워크를 통해 지역적 조정을 수행합니다.
3. Adobe_Fivek dataset과 LOL dataset을 활용하여 각각 학습합니다.

**2. 웹 사이트**
1. 웹 사이트에 이미지를 업로드 -> 서버로 전송합니다.
2. 이미지의 히스토그램 분석 후 Adobe_Fivek dataset과 유사한지, LOL dataset과 유사한지 판단합니다.
3. 판단 후 해당 모델을 이용하여 색상 조정 및 화질 개선된 이미지를 출력합니다.
4. Streamlit의 SELECT BOX에서 "Another Model"을 선택하여 나머지 모델로도 테스트 가능합니다.

   (기본값은 "Recommended Model"로 설정되어 있습니다.)


---


### 1. 모델 ###
- #### Global Enhancement ####
  - 입력 영상 I를 256×256의 크기로 조정한 뒤, 3개의 convolution layer를 통해 3 채널의 256×256 크기의 컬러 입력 영상을 64 채널의 64×64의 특징 맵으로 변환합니다. 
  - ViT 모델과 색상 변환 함수를 통해 입력 영상의 전역적 조정을 수행합니다.
  - self-attention mechanism을 통해 영상 전체에 대한 특성을 파악할 수 있으며 출력으로 768 차원의 특징을 획득합니다.
  - 이는 시그모이드 함수와 정규화 과정을 거친 뒤 R,G,B 채널에 대한 색상 변환 함수를 통해 전역적 개선 영상을 획득합니다.

    
- #### Local Enhancement ####
  - U-Net 기반 네트워크를 통해 입력 영상의 지역적 조정을 수행합니다. 
  - 인코더-디코더 기반 모델인 U-Net 구조를 통해 정보의 손실로 최소화하여 지역적 개선 영상을 획득합니다.

    
- #### 학습 #### 
  - 모델은 구조는 동일하지만 하이퍼파라미터를 달리 하여 두 데이터셋에 대해 학습합니다.

  -> 파일경로: project_model_Adobe5k, project_model_LOL


- #### 테스트 ####
  - train 코드와 test 코드가 각기 다른 파일로 구성되어 있습니다.

  -> 파일경로: project_test/main.py
  


### 2. 웹 사이트 ###
- Streamlit 코드: project_test/streamlit/main.py
- FastAPI 코드: project_test/fast_api_main.py

- #### 이미지 업로드 ####
  - 모든 입력 이미지는 현재 시각으로 구분하여 서버에 저장됩니다.
  - 
