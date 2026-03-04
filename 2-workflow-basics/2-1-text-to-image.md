# 2-1. Text-to-Image Workflow

Text-to-Image는 텍스트 프롬프트만으로 새로운 이미지를 생성하는 가장 기본적인 워크플로우입니다.

## 워크플로우 구성

### 1단계: 빈 캔버스 준비

ComfyUI를 열면 빈 캔버스가 표시됩니다. 우클릭 또는 스페이스바를 눌러 노드 메뉴를 열 수 있습니다.

![빈 캔버스](../.gitbook/assets/image&#32;(21)&#32;(1).png)

### 2단계: Checkpoint 로드 노드 배치

우클릭 → **Add Node** → **loaders** → **Load Checkpoint**

![노드 메뉴](../.gitbook/assets/image&#32;(24).png)

이 노드는 Stable Diffusion 모델(체크포인트)을 로드합니다.

![Load Checkpoint 노드](../.gitbook/assets/image&#32;(25).png)

### 3단계: CLIP 텍스트 인코딩 노드 배치 (2개)

우클릭 → **Add Node** → **conditioning** → **CLIP Text Encode (Prompt)**

두 개의 노드를 추가합니다:
- 하나는 **Positive Prompt** (원하는 것)
- 하나는 **Negative Prompt** (원하지 않는 것)

![CLIP Text Encode 노드](../.gitbook/assets/image&#32;(26).png)

### 4단계: 빈 잠재 이미지 노드 배치

우클릭 → **Add Node** → **latent** → **Empty Latent Image**

이 노드는 초기 노이즈를 생성하기 위한 빈 잠재 공간을 정의합니다.

![Empty Latent Image 노드](../.gitbook/assets/image&#32;(27).png)

기본 설정:
- **width**: 1024
- **height**: 1024
- **batch_size**: 1

### 5단계: KSampler 노드 배치

우클릭 → **Add Node** → **sampling** → **KSampler**

KSampler는 노이즈 제거를 수행하는 핵심 노드입니다.

![KSampler 노드](../.gitbook/assets/image&#32;(28).png)

권장 설정:
- **seed**: 0 (또는 원하는 값)
- **steps**: 20
- **cfg**: 8.0
- **sampler_name**: euler
- **scheduler**: simple
- **denoise**: 1.00

### 6단계: VAE Decode 및 이미지 미리보기 노드 배치

**VAE Decode 노드 추가:**
우클릭 → **Add Node** → **latent** → **VAE Decode**

![VAE Decode 노드](../.gitbook/assets/image&#32;(29).png)

**이미지 미리보기 노드 추가:**
우클릭 → **Add Node** → **image** → **Preview Image**

![Preview Image 노드](../.gitbook/assets/image&#32;(30).png)

### 7단계: 노드 연결 (엣지 연결)

이제 모든 노드를 올바른 순서로 연결합니다:

![노드 연결 과정](../.gitbook/assets/image&#32;(31).png)

**연결 순서:**

1. **Load Checkpoint** → **CLIP Text Encode (Positive)**
   - `CLIP` 출력 → `clip` 입력

2. **Load Checkpoint** → **CLIP Text Encode (Negative)**
   - `CLIP` 출력 → `clip` 입력

3. **Load Checkpoint** → **KSampler**
   - `MODEL` 출력 → `model` 입력

4. **CLIP Text Encode (Positive)** → **KSampler**
   - `CONDITIONING` 출력 → `positive` 입력

5. **CLIP Text Encode (Negative)** → **KSampler**
   - `CONDITIONING` 출력 → `negative` 입력

6. **Empty Latent Image** → **KSampler**
   - `LATENT` 출력 → `latent_image` 입력

7. **KSampler** → **VAE Decode**
   - `LATENT` 출력 → `samples` 입력

8. **Load Checkpoint** → **VAE Decode**
   - `VAE` 출력 → `vae` 입력

9. **VAE Decode** → **Preview Image**
   - `IMAGE` 출력 → `images` 입력

![완성된 워크플로우](../.gitbook/assets/image&#32;(32).png)

### 8단계: 체크포인트 다운로드

모델이 없는 경우 다운로드가 필요합니다.

**Manager를 통한 다운로드:**

1. 우측 **Manager** 버튼 클릭
2. **Model Manager** 선택
3. "sd_xl_base_1.0" 검색
4. **Download** 클릭

![모델 다운로드](../.gitbook/assets/image&#32;(33).png)

또는 직접 다운로드:
- [Stable Diffusion XL Base 1.0](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0)에서 다운로드
- `/workspace/models/checkpoints/` 폴더에 저장

다운로드 후 **Load Checkpoint** 노드에서 모델을 선택합니다.

![체크포인트 선택](../.gitbook/assets/image&#32;(34).png)

### 9단계: Positive Prompt 작성

**CLIP Text Encode (Positive)** 노드의 텍스트 필드에 프롬프트를 입력합니다:

```
beautiful scenery nature glass bottle landscape, purple galaxy bottle,
```

![Positive Prompt 입력](../.gitbook/assets/image&#32;(35).png)

**프롬프트 작성 팁:**
- 구체적이고 명확한 단어 사용
- 쉼표로 키워드 구분
- 형용사로 스타일 지정
- 중요한 키워드는 앞쪽에 배치

### 10단계: Negative Prompt 작성

**CLIP Text Encode (Negative)** 노드의 텍스트 필드에 제외할 요소를 입력합니다:

```
text, watermark
```

![Negative Prompt 입력](../.gitbook/assets/image&#32;(36).png)

**일반적인 Negative Prompt:**
- `text, watermark`: 텍스트/워터마크 제거
- `blurry, low quality`: 품질 저하 방지
- `deformed, distorted`: 왜곡 방지
- `extra limbs, bad anatomy`: 해부학적 오류 방지

## 실행 및 결과 확인

모든 설정이 완료되었으면 우측 상단의 **Queue Prompt** 버튼을 클릭합니다.

![Queue Prompt 버튼](../.gitbook/assets/image&#32;(37).png)

생성 과정이 진행되며 각 노드가 순차적으로 실행됩니다.

![생성 진행 중](../.gitbook/assets/image&#32;(38).png)

완료되면 **Preview Image** 노드에 생성된 이미지가 표시됩니다.

![생성된 이미지](../.gitbook/assets/image&#32;(39).png)

## 파라미터 조정

### Seed 값 변경

동일한 프롬프트로 다른 이미지를 생성하려면 **seed** 값을 변경합니다.

- 같은 seed = 같은 결과 (재현 가능)
- 다른 seed = 다른 이미지

![Seed 변경](../.gitbook/assets/image&#32;(40).png)

### Steps 조정

**steps** 값을 높이면 더 정교한 이미지를 얻을 수 있지만 생성 시간이 증가합니다.

- 낮은 값 (10~15): 빠르지만 거친 결과
- 적정 값 (20~30): 품질과 속도의 균형
- 높은 값 (40~50): 고품질이지만 느림

![Steps 조정](../.gitbook/assets/image&#32;(41).png)

### CFG Scale 조정

**cfg** (Classifier Free Guidance) 값은 프롬프트 준수 강도를 조절합니다.

- 낮은 값 (3~5): 창의적이지만 프롬프트 이탈 가능
- 적정 값 (7~9): 균형잡힌 결과
- 높은 값 (12~15): 프롬프트에 강하게 종속, 과포화 가능

![CFG 조정](../.gitbook/assets/image&#32;(42).png)

### 해상도 변경

**Empty Latent Image** 노드의 **width**와 **height**를 조정하여 해상도를 변경할 수 있습니다.

![해상도 변경](../.gitbook/assets/image&#32;(43).png)

**권장 해상도 (SDXL):**
- 1024x1024 (정사각형)
- 1152x896 (가로)
- 896x1152 (세로)
- 1216x832 (와이드)

## 워크플로우 저장

완성된 워크플로우를 저장하여 나중에 재사용할 수 있습니다:

1. 우측 상단 **Save** 버튼 클릭
2. 파일명 입력 (예: `text2img_basic.json`)
3. 저장 완료

불러오기:
1. 우측 상단 **Load** 버튼 클릭
2. 저장된 워크플로우 선택

## 다음 단계

Text-to-Image 워크플로우를 익혔다면 [Image-to-Image Workflow](2-2-image-to-image.md)로 이동하여 기존 이미지를 변형하는 방법을 학습하세요.
