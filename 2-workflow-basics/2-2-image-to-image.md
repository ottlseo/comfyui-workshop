# 2-2. Image-to-Image Workflow

Image-to-Image (img2img)는 기존 이미지를 기반으로 새로운 이미지를 생성하는 워크플로우입니다. 원본 이미지의 구조와 구성을 유지하면서 스타일이나 디테일을 변경할 수 있습니다.

## img2img 개념 이해

### 작동 원리

img2img는 다음과 같은 과정으로 작동합니다:

1. **기존 이미지를 잠재 공간으로 변환**: VAE Encoder를 사용하여 이미지를 잠재 표현으로 압축
2. **노이즈 추가**: 잠재 표현에 일정량의 노이즈를 추가
3. **디노이징**: KSampler가 프롬프트 조건에 따라 노이즈를 제거하며 이미지 변형
4. **이미지 복원**: VAE Decoder로 다시 픽셀 공간으로 변환

이 방식은 완전히 새로운 이미지를 생성하는 것이 아니라, 원본 이미지의 "뼈대"를 활용하여 프롬프트에 맞게 재해석합니다.

### txt2img와의 차이점

| 구분 | Text-to-Image | Image-to-Image |
|------|---------------|----------------|
| **시작점** | 순수 랜덤 노이즈 | 실제 이미지 (인코딩됨) |
| **초기 노드** | Empty Latent Image | Load Image + VAE Encode |
| **denoise 값** | 1.0 (100% 노이즈 제거) | 0.3~0.9 (부분 노이즈 제거) |
| **결과** | 완전히 새로운 이미지 | 원본 기반 변형 이미지 |

**핵심 차이점:**
- txt2img는 노이즈에서 시작하므로 denoise를 1.0으로 설정
- img2img는 이미지에서 시작하므로 denoise를 낮게 설정하여 원본의 구조를 일부 보존

## 워크플로우 구성

### 1단계: txt2img 워크플로우 복사

[Text-to-Image 워크플로우](2-1-text-to-image.md)를 먼저 구성하거나 저장된 워크플로우를 불러옵니다.

이 워크플로우를 기반으로 img2img로 변환할 것입니다.

### 2단계: Empty Latent Image 노드 제거

**Empty Latent Image** 노드를 삭제합니다. img2img에서는 실제 이미지를 사용하므로 더 이상 필요하지 않습니다.

노드 선택 후 **Delete** 키 또는 우클릭 → **Remove** 선택

### 3단계: 이미지 로드 노드 추가

우클릭 → **Add Node** → **image** → **Load Image**

이 노드는 변형할 원본 이미지를 불러옵니다.

**이미지 업로드 방법:**
1. **Load Image** 노드 클릭
2. 우측 패널에서 **choose file to upload** 버튼 클릭
3. 원본 이미지 선택 및 업로드

또는 이미지를 드래그 앤 드롭으로 캔버스에 직접 놓으면 자동으로 **Load Image** 노드가 생성됩니다.

### 4단계: VAE Encode 노드 추가

우클릭 → **Add Node** → **latent** → **VAE Encode**

이 노드는 픽셀 이미지를 잠재 공간 표현으로 변환합니다.

### 5단계: 노드 연결

기존 txt2img 워크플로우를 기반으로 다음과 같이 연결을 변경합니다:

**새로운 연결:**

1. **Load Image** → **VAE Encode**
   - `IMAGE` 출력 → `pixels` 입력

2. **Load Checkpoint** → **VAE Encode**
   - `VAE` 출력 → `vae` 입력

3. **VAE Encode** → **KSampler**
   - `LATENT` 출력 → `latent_image` 입력

**기존 연결 유지:**
- Load Checkpoint → KSampler (model)
- CLIP Text Encode (Positive) → KSampler (positive)
- CLIP Text Encode (Negative) → KSampler (negative)
- KSampler → VAE Decode (samples)
- Load Checkpoint → VAE Decode (vae)
- VAE Decode → Preview Image (images)

**완성된 워크플로우 구조:**

```
Load Image → VAE Encode
                ↓
Load Checkpoint → KSampler → VAE Decode → Preview Image
        ↓            ↑    ↑           ↑
       CLIP    Positive  Negative    VAE
                Prompt   Prompt
```

### 6단계: denoise 파라미터 조정

**KSampler** 노드의 **denoise** 값을 조정합니다.

**권장 범위: 0.7 ~ 0.87**

- **0.7**: 원본 이미지와 매우 유사, 미세한 변형
- **0.8**: 적정한 변형 수준 (권장)
- **0.87**: 상당한 변형, 원본 구조 일부 유지

denoise 값이 높을수록 원본에서 더 멀어지고, 낮을수록 원본과 유사합니다.

### 7단계: 프롬프트 작성

원본 이미지를 어떻게 변형할지 프롬프트를 작성합니다.

**예시 프롬프트:**

**원본 이미지:** 산 풍경 사진

**Positive Prompt:**
```
oil painting style, vibrant colors, impressionist art, mountain landscape at sunset, dramatic clouds, artistic brush strokes
```

**Negative Prompt:**
```
photorealistic, photograph, realistic, modern, text, watermark
```

이 프롬프트는 사진 스타일의 산 풍경을 인상파 유화 스타일로 변환합니다.

### 8단계: 실행 및 결과 확인

1. 우측 상단 **Queue Prompt** 버튼 클릭
2. 생성 과정 대기
3. **Preview Image**에서 결과 확인

원본 이미지의 구도와 구조를 유지하면서 프롬프트에 따라 스타일이 변경된 이미지가 생성됩니다.

## denoise 값에 따른 변화

denoise 파라미터는 img2img에서 가장 중요한 설정입니다. 다양한 값으로 실험하여 최적의 결과를 찾으세요.

### denoise = 0.3 (낮은 변형)

- **결과**: 원본과 거의 동일, 색감이나 미세한 디테일만 변경
- **사용 사례**: 색보정, 작은 스타일 변화, 노이즈 제거

```
원본 구조 보존도: ████████░░ 90%
프롬프트 반영도: ██░░░░░░░░ 20%
```

### denoise = 0.5 (중간 변형)

- **결과**: 원본의 주요 구조 유지, 디테일과 스타일 변경
- **사용 사례**: 스타일 전환, 시간대 변경, 분위기 전환

```
원본 구조 보존도: ██████░░░░ 60%
프롬프트 반영도: █████░░░░░ 50%
```

### denoise = 0.7~0.8 (권장 범위)

- **결과**: 원본의 구도와 기본 구조 유지하며 상당한 변형
- **사용 사례**: 일반적인 img2img 작업, 예술적 변환

```
원본 구조 보존도: ████░░░░░░ 40%
프롬프트 반영도: ███████░░░ 70%
```

### denoise = 0.9 (높은 변형)

- **결과**: 원본의 대략적인 구도만 유지, 대부분 새롭게 생성
- **사용 사례**: 대담한 재해석, 거의 새로운 이미지

```
원본 구조 보존도: ██░░░░░░░░ 20%
프롬프트 반영도: █████████░ 90%
```

### denoise = 1.0 (완전 변형)

- **결과**: txt2img와 동일, 원본 이미지 무시
- **사용 사례**: img2img를 사용할 이유가 없음 (txt2img 사용 권장)

```
원본 구조 보존도: ░░░░░░░░░░ 0%
프롬프트 반영도: ██████████ 100%
```

## 실전 예시

### 예시 1: 사진을 애니메이션 스타일로 변환

**원본**: 실제 도시 거리 사진

**Positive Prompt:**
```
anime style, Studio Ghibli inspired, colorful animation, detailed background, vibrant city street
```

**Negative Prompt:**
```
photorealistic, realistic, 3d render, photograph
```

**설정:**
- denoise: 0.75
- steps: 25
- cfg: 8.0

### 예시 2: 낮 풍경을 밤 풍경으로 변환

**원본**: 낮에 찍은 풍경 사진

**Positive Prompt:**
```
night scene, starry sky, moonlight, dark blue atmosphere, city lights, beautiful night landscape
```

**Negative Prompt:**
```
daytime, bright, sunny, daylight
```

**설정:**
- denoise: 0.70
- steps: 30
- cfg: 7.5

### 예시 3: 스케치를 완성된 그림으로 변환

**원본**: 간단한 연필 스케치

**Positive Prompt:**
```
detailed digital art, fully colored illustration, professional artwork, complete rendering, vibrant colors
```

**Negative Prompt:**
```
sketch, unfinished, black and white, draft, incomplete
```

**설정:**
- denoise: 0.85
- steps: 30
- cfg: 9.0

## 추가 팁

### 1. ControlNet 활용

더 정밀한 제어가 필요하면 ControlNet을 함께 사용할 수 있습니다:
- Canny edge detection으로 윤곽선 유지
- Depth map으로 깊이 정보 유지
- Pose detection으로 인물 포즈 유지

### 2. 배치 처리

동일한 이미지로 여러 변형을 생성하려면:
1. **KSampler**의 **batch_size** 증가
2. 또는 **seed**를 바꿔가며 여러 번 실행

### 3. 해상도 주의

원본 이미지 해상도와 모델의 학습 해상도가 맞지 않으면 품질 저하가 발생할 수 있습니다.

- SDXL: 1024x1024 권장
- SD 1.5: 512x512 권장

필요시 **Upscale Image** 노드로 사전 리사이징하세요.

## 참고 자료

더 자세한 img2img 기법과 고급 워크플로우는 ComfyUI Wiki를 참조하세요:

**ComfyUI Wiki - img2img:**
[https://comfyui-wiki.com/en/workflows/img2img](https://comfyui-wiki.com/en/workflows/img2img)

## 다음 단계

img2img 워크플로우를 익혔다면 다음 단계로 넘어가세요:

- **ControlNet 활용**: 더 정밀한 이미지 제어
- **LoRA 적용**: 특정 스타일이나 개념 추가
- **Upscaling**: 고해상도 이미지 생성
- **Inpainting**: 이미지의 일부분만 수정

워크플로우를 저장하고 다양한 이미지와 프롬프트로 실험해보세요\!
