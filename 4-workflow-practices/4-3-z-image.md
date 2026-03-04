# #4-3. Z-Image Turbo 실습

## Z-Image Turbo 소개

Z-Image Turbo는 알리바바 그룹의 Tongyi-MAI에서 개발한 고성능 Text-to-Image AI 모델입니다.

### 주요 특징

- **모델 크기**: 6B 파라미터
- **아키텍처**: Lumina 기반
- **최적화**: 16GB VRAM에서 원활하게 동작
- **성능**: 빠른 속도와 높은 품질의 이미지 생성
- **통합**: ComfyUI 공식 워크플로우 템플릿 제공

### 장점

- ✅ 중급 사양 GPU에서도 실행 가능
- ✅ 빠른 생성 속도 (약 15~30초)
- ✅ 고품질 이미지 출력
- ✅ 간단한 설정 및 사용

## 모델 준비

Z-Image Turbo를 사용하기 위해 3개의 모델 파일이 필요합니다.

| 모델 | 용도 | 저장 경로 | 다운로드 링크 |
|------|------|-----------|---------------|
| qwen_3_4b.safetensors | Text Encoder | ComfyUI/models/text_encoders/ | [다운로드](https://huggingface.co/Comfy-Org/z_image_turbo/blob/main/split_files/text_encoders/qwen_3_4b.safetensors) |
| z_image_turbo_bf16.safetensors | Diffusion Model | ComfyUI/models/diffusion_models/ | [다운로드](https://huggingface.co/Comfy-Org/z_image_turbo/blob/main/split_files/diffusion_models/z_image_turbo_bf16.safetensors) |
| ae.safetensors | VAE | ComfyUI/models/vae/ | [다운로드](https://huggingface.co/Comfy-Org/z_image_turbo/blob/main/split_files/vae/ae.safetensors) |

### 수동 다운로드 방법

각 링크에서 파일을 다운로드한 후 해당 경로에 저장합니다.

```bash
# 예시 (Linux/Mac)
cd ComfyUI/models/text_encoders/
wget https://huggingface.co/Comfy-Org/z_image_turbo/resolve/main/split_files/text_encoders/qwen_3_4b.safetensors

cd ../diffusion_models/
wget https://huggingface.co/Comfy-Org/z_image_turbo/resolve/main/split_files/diffusion_models/z_image_turbo_bf16.safetensors

cd ../vae/
wget https://huggingface.co/Comfy-Org/z_image_turbo/resolve/main/split_files/vae/ae.safetensors
```

## 실습: Z-Image Turbo 이미지 생성

### Step 1: 워크플로우 템플릿 로드

**방법 1: ComfyUI 내장 템플릿 사용**

1. ComfyUI 좌측 상단 메뉴(C 버튼) 클릭
2. **템플릿 탐색** 선택
3. "Z-Image Turbo" 검색 및 선택
4. 템플릿 로드

**방법 2: JSON 워크플로우 다운로드**

1. GitHub에서 워크플로우 JSON 다운로드
   - URL: https://github.com/Comfy-Org/workflow_templates/blob/main/templates/image_z_image_turbo.json
2. ComfyUI에서 Load 버튼 → JSON 파일 선택

### Step 2: 모델 자동 다운로드

1. 워크플로우 로드 시 **모델 다운로드 다이얼로그** 표시
2. **Download** 버튼 클릭 → 자동으로 필요한 모델 다운로드
3. 모델이 이미 다운로드되어 있으면 다이얼로그 닫기

### Step 3: 모델 확인

각 모델 로드 노드에서 올바른 모델이 선택되었는지 확인합니다.

- **Text Encoder Loader**: qwen_3_4b.safetensors
- **Diffusion Model Loader**: z_image_turbo_bf16.safetensors
- **VAE Loader**: ae.safetensors

### Step 4: 프롬프트 입력

프롬프트 입력 노드에 원하는 이미지 설명을 입력합니다.

**예시 프롬프트**:
```
A cat astronaut floating in space, detailed fur, stars in background, high quality, 4k
```

```
A futuristic cityscape at sunset, neon lights, cyberpunk style, detailed architecture
```

```
Portrait of a young woman with long hair, soft lighting, professional photography, bokeh background
```

### Step 5: 실행

1. **Queue Prompt** 버튼 클릭
2. 생성 대기 (약 15~30초 소요)
3. 결과 확인

### Step 6: 파라미터 조정 (선택사항)

- **Steps**: 생성 스텝 수 (기본값: 8, 범위: 4~20)
- **CFG Scale**: 프롬프트 가이던스 강도 (기본값: 7.0)
- **Seed**: 랜덤 시드 값 (재현성을 위해 고정 가능)
- **Resolution**: 출력 해상도 (512x512, 768x768 등)

## 저사양을 위한 양자화 모델

VRAM이 부족한 경우 FP8 양자화 버전을 사용할 수 있습니다.

### 양자화 모델 장점

- ✅ 낮은 VRAM 사용량 (약 30~40% 감소)
- ✅ 더 빠른 생성 속도 (약 7~8초)
- ✅ 품질 손실 최소화

### 다운로드 링크

**CivitAI**:
- https://civitai.com/models/2169712/z-image-turbo-quantized-for-low-vram

**Hugging Face**:
- https://huggingface.co/drbaph/Z-Image-Turbo-FP8

### 사용 방법

1. 양자화 모델 다운로드
2. `ComfyUI/models/diffusion_models/` 경로에 저장
3. Diffusion Model Loader 노드에서 FP8 모델 선택
4. 나머지 워크플로우는 동일

## 트러블슈팅

### 문제: 모델이 로드되지 않음

**해결책**:
- 모델 파일 경로 확인
- 파일 이름이 정확한지 확인
- ComfyUI 재시작

### 문제: VRAM 부족 오류

**해결책**:
- FP8 양자화 모델 사용
- 해상도 낮추기 (512x512 사용)
- 다른 프로그램 종료하여 VRAM 확보

### 문제: 생성 속도가 느림

**해결책**:
- Steps 수 줄이기 (4~6 스텝 사용)
- FP8 양자화 모델 사용
- GPU 드라이버 최신 버전 확인

## 참조

- God Logger 블로그: https://god-logger.tistory.com/237
- ComfyUI Workflow Templates: https://github.com/Comfy-Org/workflow_templates
- Hugging Face Model Hub: https://huggingface.co/Comfy-Org/z_image_turbo
