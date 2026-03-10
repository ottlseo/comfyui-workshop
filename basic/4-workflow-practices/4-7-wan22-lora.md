# 4-7. Wan 2.2 LoRA 실습

## 학습 목표
이 실습을 완료하면 다음을 할 수 있습니다:
- 비디오 생성에서 LoRA의 역할과 효과 이해하기
- Lightning LoRA로 비디오 생성 속도를 크게 향상시키기
- LoRA 가중치를 조정하여 최적의 결과 얻기
- 여러 LoRA를 조합하여 스타일과 속도를 동시에 제어하기

## 소요 시간
약 20분

## 난이도
★★★ (고급)

> **주의**: 이 실습은 4-6 Wan 2.2 실습을 먼저 완료한 후 진행하세요. Wan 2.2 기본 워크플로우가 구성되어 있어야 합니다.

## Wan 2.2 LoRA 소개

비디오 생성 모델에 LoRA(Low-Rank Adaptation)를 적용하여 스타일과 모션을 더욱 정밀하게 제어할 수 있습니다.

> **초보자를 위한 비유**: Lightning LoRA는 비디오 생성의 **고속도로**와 같습니다. 일반 도로(기본 모델)로는 20-30분 걸리던 길을 4-5분 만에 갈 수 있게 해주죠. 품질은 거의 그대로 유지하면서 말이에요!

### 주요 기능

- **Lightning LoRA**: 4스텝 가속으로 빠른 비디오 생성
- **스타일 제어**: 특정 영상 스타일 적용 (애니메이션, 실사 등)
- **모션 제어**: 특정 모션 패턴 강화
- **커뮤니티 LoRA**: 다양한 커뮤니티 제작 LoRA 활용 가능

## 사용 가능한 LoRA

| LoRA 이름 | 설명 | 용도 |
|----------|------|-----|
| Wan2.2-T2V-A14B-4steps-lora | 4스텝 가속 Lightning LoRA | 빠른 생성 |
| 커뮤니티 스타일 LoRA | 특정 영상 스타일 | 스타일 특화 |
| 모션 특화 LoRA | 특정 모션 패턴 | 모션 제어 |

## 실습: Lightning LoRA로 빠른 비디오 생성

### Step-by-step 진행

1. **기본 워크플로우 준비**
   - 4-6에서 구성한 Wan 2.2 T2V 워크플로우를 기반으로 시작

2. **Load LoRA 노드 추가**
   - 워크플로우 캔버스에서 더블클릭
   - "Load LoRA" 검색 및 추가

3. **Lightning LoRA 다운로드**
   - Model Manager에서 "Wan2.2-T2V-A14B-4steps-lora-rank64-V1" 검색
   - 또는 Hugging Face에서 직접 다운로드

4. **LoRA 파일 배치**
   - 다운로드한 LoRA를 `ComfyUI/models/loras/` 디렉토리에 저장

5. **노드 연결**
   - UNet 모델 출력 → Load LoRA 노드 입력
   - Load LoRA 노드 출력 → KSampler 입력
   - 기존 UNet → KSampler 연결 제거

6. **LoRA 선택**
   - Load LoRA 노드에서 Lightning LoRA 파일 선택
   - Strength: 1.0 유지

7. **KSampler 파라미터 조정**
   - Steps: 4로 변경 (기본 20-30에서 감소)
   - CFG: 1.0~2.0으로 낮춤 (기본 7.0에서 감소)

8. **프롬프트 입력**
   - 원하는 비디오 설명 프롬프트 입력
   - 예: "A sports car driving on a coastal highway at sunset, cinematic"

9. **실행**
   - 실행 버튼 클릭
   - 일반 버전 대비 생성 시간 크게 단축

10. **결과 비교**
    - Lightning LoRA 사용 시 vs 미사용 시 속도 및 품질 비교
    - 4스텝으로도 충분한 품질 확인

## LoRA 가중치 조정 팁

### Lightning LoRA
- **Strength**: 1.0 권장
- 너무 낮으면 가속 효과 감소
- 너무 높으면 아티팩트 발생 가능

### 스타일 LoRA
- **Strength**: 0.5~0.8 권장
- 자연스러운 스타일 융합
- 원본 모델의 특성과 균형 유지

### 여러 LoRA 조합
- 가중치 합이 1.0 이하 권장
- 예: Lightning (1.0) + Style (0.5) = 합산 고려
- 너무 많은 LoRA 조합 시 충돌 가능

## 고급 활용

### 커스텀 LoRA 찾기
- Hugging Face에서 "wan2.2 lora" 검색
- CivitAI 등 커뮤니티 플랫폼 활용
- 라이선스 확인 필수

### LoRA 학습 (고급)
- 특정 스타일이나 피사체 학습 가능
- Kohya LoRA 학습 스크립트 활용
- 비디오 데이터셋 준비 필요

## 실습 결과 예시

### Lightning LoRA 사용 전
- Steps: 20
- 생성 시간: ~5분
- 품질: 높음

### Lightning LoRA 사용 후
- Steps: 4
- 생성 시간: ~1분
- 품질: 높음 (거의 차이 없음)

### 효율성 향상
- **속도**: 약 5배 향상
- **VRAM**: 유사
- **품질**: 90% 이상 유지

## 진행 확인

다음 항목을 완료했는지 체크하세요:

- [ ] Lightning LoRA 파일을 다운로드하고 올바른 경로에 배치했다
- [ ] Load LoRA 노드를 추가하고 워크플로우에 연결했다
- [ ] KSampler의 Steps를 4로, CFG를 낮게 조정했다
- [ ] Lightning LoRA 사용 전/후의 생성 시간을 비교했다
- [ ] LoRA 가중치를 조정하며 최적의 설정을 찾았다

> **초보자 팁**: Lightning LoRA는 "빠른 프로토타이핑"에 최적입니다. 여러 아이디어를 빠르게 테스트해보고, 최종 결과물은 일반 모델로 생성하는 전략도 좋습니다. 하지만 대부분의 경우 Lightning LoRA의 품질도 충분히 훌륭합니다!

## 참조

- [ComfyUI Wiki - Wan 2.2](https://comfyui-wiki.com/ko/tutorial/advanced/video/wan2.2/wan2-2)
- [Hugging Face - Wan 2.2 LoRA](https://huggingface.co/models?search=wan2.2+lora)
