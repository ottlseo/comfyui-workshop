# 2. Workflow 알아보기

## Stable Diffusion 알고리즘 구조

ComfyUI는 Stable Diffusion 알고리즘을 기반으로 이미지를 생성합니다. Stable Diffusion은 세 가지 주요 구성 요소로 이루어져 있습니다:

![Stable Diffusion 아키텍처](../.gitbook/assets/image&#32;(23).png)

### 1. Text Encoder (CLIP)

텍스트 프롬프트를 수치적 표현(임베딩)으로 변환하여 U-Net이 이해할 수 있는 형태로 만듭니다.

- **역할**: 사용자의 텍스트 설명을 의미론적 벡터로 변환
- **모델**: CLIP (Contrastive Language-Image Pre-training)
- **출력**: 텍스트 임베딩

### 2. U-Net (디노이징 네트워크)

잠재 공간(latent space)에서 노이즈를 제거하는 핵심 네트워크입니다.

- **역할**: 반복적으로 노이즈를 제거하여 의미 있는 이미지로 변환
- **입력**: 노이즈가 있는 잠재 표현 + 텍스트 임베딩
- **출력**: 디노이징된 잠재 표현

### 3. VAE (Variational Autoencoder)

이미지와 잠재 공간 간의 변환을 담당합니다.

- **Encoder**: 이미지 → 잠재 표현 (압축)
- **Decoder**: 잠재 표현 → 이미지 (복원)
- **장점**: 낮은 차원에서 작업하여 계산 효율성 향상

## 5단계 작동 순서

### 1단계: 텍스트 입력 및 인코딩

사용자가 입력한 프롬프트를 Text Encoder(CLIP)가 수치적 임베딩으로 변환합니다.

- Positive prompt: 생성하고 싶은 이미지의 특징
- Negative prompt: 제외하고 싶은 요소

### 2단계: 초기 노이즈 생성

순수한 랜덤 노이즈로 시작합니다.

- 잠재 공간에서 생성 (이미지 해상도보다 훨씬 작은 차원)
- 시드(seed) 값으로 재현 가능성 확보

### 3단계: 노이즈 스케줄 설정

노이즈를 제거하는 강도와 방식을 정의합니다.

- **Steps**: 디노이징 반복 횟수 (일반적으로 20~50)
- **Sampler**: 샘플링 알고리즘 (euler, dpm++, etc.)
- **Scheduler**: 스텝별 노이즈 제거 강도 조절

### 4단계: 반복적 샘플링 (디노이징)

U-Net이 텍스트 조건을 참고하여 점진적으로 노이즈를 제거합니다.

- 각 스텝마다 노이즈 일부 제거
- CFG(Classifier Free Guidance)로 프롬프트 준수 강도 조절
- 지정된 스텝 수만큼 반복

### 5단계: 최종 이미지 생성

VAE Decoder가 잠재 표현을 실제 이미지로 변환합니다.

- 잠재 공간 → 픽셀 공간 변환
- 최종 이미지 출력

## 이 섹션에서 다룰 내용

이 섹션에서는 Stable Diffusion의 두 가지 기본 워크플로우를 학습합니다:

### [2-1. Text-to-Image Workflow](2-1-text-to-image.md)

텍스트 프롬프트만으로 새로운 이미지를 생성하는 기본 워크플로우입니다.

- 노드 구성 방법
- 프롬프트 작성 기법
- 파라미터 조정 방법

### [2-2. Image-to-Image Workflow](2-2-image-to-image.md)

기존 이미지를 기반으로 새로운 이미지를 생성하는 워크플로우입니다.

- 이미지 로드 및 인코딩
- denoise 파라미터 조정
- 원본 이미지와 새 이미지의 균형 맞추기

## 다음 단계

[Text-to-Image Workflow](2-1-text-to-image.md)로 시작하여 ComfyUI의 기본 노드 연결과 이미지 생성 과정을 익혀보세요.
