# #4-1. ControlNet 실습

## ControlNet이란?

ControlNet은 참조 이미지를 AI가 이해할 수 있는 지시로 변환하는 **"번역 도우미"**입니다. 이미지의 구조, 포즈, 윤곽선 등을 정밀하게 제어할 수 있게 해줍니다.

### ControlNet의 핵심 개념

- **구조 제어**: 참조 이미지의 구조를 유지하면서 새로운 이미지 생성
- **다양한 제어 방식**: 엣지, 깊이, 포즈, 라인 아트 등 다양한 방식으로 제어
- **유연한 강도 조절**: 제어의 강도를 0.0~1.0 사이에서 조절 가능

## ControlNet V1.1 주요 제어 유형

| 유형 | 모델 | 설명 | 주요 용도 |
|-----|------|------|----------|
| **Canny** | control_v11p_sd15_canny | 엣지(경계선) 감지 | 정밀한 라인 아트, 건축물 |
| **Depth** | control_v11f1p_sd15_depth | 깊이 맵 추출 | 전경/배경 깊이 제어 |
| **OpenPose** | control_v11p_sd15_openpose | 인체 포즈 인식 | 인물 자세 제어 |
| **Lineart** | control_v11p_sd15_lineart | 라인 아트 추출 | 애니메이션 스타일 |
| **Scribble** | control_v11p_sd15_scribble | 낙서/스케치 제어 | 대략적 윤곽선 |
| **Softedge** | control_v11p_sd15_softedge | 부드러운 엣지 | 자연스러운 경계 |
| **Seg** | control_v11p_sd15_seg | 의미론적 세그먼테이션 | 영역별 제어 |
| **Normal** | control_v11p_sd15_normalbae | 표면 법선 맵 | 조명/질감 제어 |

## 모델 준비

### 1. 체크포인트 모델
- **DreamShaper 8**: https://civitai.com/models/4384?modelVersionId=128713
- 저장 경로: `ComfyUI/models/checkpoints/`

### 2. ControlNet 모델
- **control_v11p_sd15_openpose.pth**: https://huggingface.co/lllyasviel/ControlNet-v1-1/resolve/main/control_v11p_sd15_openpose.pth
- 저장 경로: `ComfyUI/models/controlnet/`

### 3. 플러그인 설치
- **ComfyUI ControlNet Auxiliary Preprocessors**
- Comfy Manager → Custom Nodes Manager에서 검색하여 설치

## 실습 1: 전처리기 없이 ControlNet 사용

이미 전처리된 OpenPose 이미지를 직접 사용하는 방법입니다.

### Step 1: 기본 워크플로우 준비
1. 기본 txt2img 워크플로우 로드
2. Load Checkpoint, CLIP Text Encode, KSampler, VAE Decode 노드 확인

### Step 2: Load ControlNet Model 노드 추가
1. 캔버스에서 더블클릭 → "Load ControlNet Model" 검색
2. 노드 추가 후 `control_v11p_sd15_openpose.pth` 선택

### Step 3: Apply ControlNet 노드 추가
1. 캔버스에서 더블클릭 → "Apply ControlNet" 검색
2. 노드 추가

### Step 4: 연결 설정
1. **CLIP Text Encode(positive)의 CONDITIONING 출력** → **Apply ControlNet의 conditioning 입력**
2. **Load ControlNet Model의 CONTROL_NET 출력** → **Apply ControlNet의 control_net 입력**
3. **Load Image의 IMAGE 출력** → **Apply ControlNet의 image 입력**
4. **Apply ControlNet의 CONDITIONING 출력** → **KSampler의 positive 입력**

### Step 5: 참조 이미지 로드
1. **Load Image** 노드 추가
2. OpenPose 전처리된 참조 이미지 업로드
   - 예: 흰색 배경에 스켈레톤 라인이 그려진 이미지

### Step 6: 강도(strength) 조정
- Apply ControlNet 노드의 `strength` 값 조정:
  - **1.0**: 최대 제어 (참조 이미지를 거의 그대로 따름)
  - **0.7**: 강한 제어 (일반적인 권장값)
  - **0.5**: 중간 제어
  - **0.3**: 약한 제어 (힌트만 제공)

### Step 7: 프롬프트 입력 및 실행
```
Positive: a beautiful woman in elegant dress, standing pose, high quality, detailed face
Negative: ugly, deformed, bad anatomy, blurry
```

Queue Prompt 클릭하여 실행

## 실습 2: 전처리기 사용 ControlNet

일반 사진을 OpenPose로 자동 변환하여 사용하는 방법입니다.

### Step 1: 플러그인 설치 확인
1. Comfy Manager → Custom Nodes Manager
2. "ComfyUI ControlNet Auxiliary Preprocessors" 검색
3. 설치되지 않았다면 Install 클릭
4. ComfyUI 재시작

### Step 2: 일반 인물 사진 준비
1. **Load Image** 노드 추가
2. 일반 인물 사진 업로드 (포즈가 명확한 사진)

### Step 3: OpenPose Preprocessor 노드 추가
1. 캔버스에서 더블클릭
2. "OpenPose Pose" 검색 (또는 "DWPose Estimator")
3. 노드 추가

### Step 4: 전처리기 연결
1. **Load Image의 IMAGE 출력** → **OpenPose Pose의 image 입력**
2. **OpenPose Pose의 IMAGE 출력** → **Apply ControlNet의 image 입력**

### Step 5: 전처리기 설정
OpenPose Pose 노드 설정:
- `detect_hand`: hand (손 감지 활성화)
- `detect_body`: enable (신체 감지 활성화)
- `detect_face`: enable (얼굴 감지 활성화)

### Step 6: Preview Image로 전처리 결과 확인
1. **Preview Image** 노드 추가
2. **OpenPose Pose의 IMAGE 출력** → **Preview Image의 images 입력** 연결
3. Queue Prompt 실행하여 전처리 결과 확인
4. 포즈 스켈레톤이 제대로 추출되었는지 확인

### Step 7: 최종 이미지 생성
1. 전처리 결과가 만족스러우면 Apply ControlNet 연결 유지
2. 프롬프트 입력:
```
Positive: 1girl, beautiful face, elegant outfit, high quality, detailed, professional photography
Negative: ugly, deformed, bad anatomy, extra limbs, blurry, low quality
```
3. Queue Prompt 실행

## 다양한 ControlNet 활용 예시

### 1. Canny Edge Control
**용도**: 정밀한 라인 아트, 건축물 스케치

**모델**: `control_v11p_sd15_canny.pth`

**프롬프트 예시**:
```
Positive: modern architecture building, glass facade, blue sky, professional photography
Negative: blurry, distorted, low quality
```

**전처리기**: Canny Edge Preprocessor
- `low_threshold`: 100
- `high_threshold`: 200

### 2. Depth Control
**용도**: 깊이감 있는 이미지 생성, 전경/배경 분리

**모델**: `control_v11f1p_sd15_depth.pth`

**프롬프트 예시**:
```
Positive: fantasy landscape, distant mountains, foreground flowers, magical atmosphere
Negative: flat, 2d, no depth
```

**전처리기**: Depth Anything Preprocessor 또는 MiDaS Depth

### 3. Lineart Control
**용도**: 애니메이션 스타일, 만화 라인 아트

**모델**: `control_v11p_sd15_lineart.pth`

**프롬프트 예시**:
```
Positive: anime girl, detailed lineart, clean lines, manga style
Negative: messy lines, blurry, photo realistic
```

**전처리기**: Lineart Preprocessor
- `coarse`: disable (정밀한 라인)

### 4. Scribble Control
**용도**: 대략적인 스케치로 이미지 생성

**모델**: `control_v11p_sd15_scribble.pth`

**프롬프트 예시**:
```
Positive: fantasy creature, dragon, detailed scales, epic composition
Negative: blurry, low quality, distorted
```

**전처리기**: Scribble Preprocessor 또는 직접 그린 스케치 사용

## ControlNet 고급 기법

### 1. 멀티 ControlNet (여러 ControlNet 동시 사용)
OpenPose + Canny를 함께 사용하여 포즈와 윤곽선을 동시에 제어할 수 있습니다.

**워크플로우**:
1. 첫 번째 Apply ControlNet (OpenPose)
2. 두 번째 Apply ControlNet (Canny)
3. 첫 번째 출력 → 두 번째 conditioning 입력
4. 두 번째 출력 → KSampler

### 2. ControlNet 강도 실험
동일한 참조 이미지로 강도를 바꿔가며 테스트:

| Strength | 효과 |
|----------|------|
| 0.3 | 참조 이미지의 힌트만 약하게 적용 |
| 0.5 | 중간 정도 제어 |
| 0.7 | 강한 제어 (권장) |
| 1.0 | 참조 이미지를 거의 그대로 따름 |

### 3. 전처리기 파라미터 조정
각 전처리기는 세밀한 파라미터 조정이 가능합니다:

**Canny**:
- `low_threshold`: 낮을수록 더 많은 엣지 감지
- `high_threshold`: 높을수록 주요 엣지만 감지

**OpenPose**:
- `detect_hand`: 손 포즈 감지 on/off
- `detect_face`: 얼굴 랜드마크 감지 on/off

## 실습 프롬프트 모음

### 인물 포즈 제어 (OpenPose)
```
Positive: professional model, fashion photography, elegant pose, high quality, detailed face, studio lighting
Negative: ugly, deformed, bad anatomy, extra limbs, blurry, amateur
```

### 건축물 제어 (Canny)
```
Positive: modern skyscraper, glass and steel, urban architecture, blue hour, professional photography, sharp details
Negative: blurry, distorted, low quality, old building
```

### 풍경 깊이 제어 (Depth)
```
Positive: epic mountain landscape, misty valleys, foreground trees, golden hour lighting, cinematic, highly detailed
Negative: flat, 2d, no depth, blurry
```

### 애니메이션 스타일 (Lineart)
```
Positive: anime character, clean lineart, manga style, detailed eyes, expressive, high quality
Negative: messy lines, blurry, photo realistic, western cartoon
```

## 문제 해결

### Q: 전처리기가 검색되지 않아요
**A**: ComfyUI ControlNet Auxiliary Preprocessors 플러그인을 설치하고 ComfyUI를 재시작하세요.

### Q: ControlNet이 너무 강하게 적용돼요
**A**: Apply ControlNet의 strength 값을 0.5~0.7 정도로 낮춰보세요.

### Q: OpenPose가 포즈를 제대로 인식하지 못해요
**A**: 참조 이미지에서 인물의 포즈가 명확하게 보이는지 확인하고, 전처리기의 detect_body/hand/face 설정을 조정해보세요.

### Q: 생성된 이미지가 참조 이미지와 너무 달라요
**A**: strength 값을 높이거나 (0.8~1.0), cfg_scale을 조정해보세요.

## 추가 학습 자료

- ComfyUI Wiki ControlNet 가이드: https://comfyui-wiki.com/ko/tutorial/advanced/how-to-install-and-use-controlnet-models-in-comfyui
- ControlNet 공식 GitHub: https://github.com/lllyasviel/ControlNet
- Hugging Face ControlNet 모델: https://huggingface.co/lllyasviel/ControlNet-v1-1

## 다음 단계

ControlNet을 마스터했다면 다음 주제로 넘어갑니다:
- **IP-Adapter**: 스타일 참조 이미지 활용
- **Inpainting**: 이미지 부분 수정
- **Upscaling**: 고해상도 이미지 생성
- **AnimateDiff**: 동영상 생성
