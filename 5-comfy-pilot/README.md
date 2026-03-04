# #5. Comfy-pilot 데모 (Optional)

> 이 섹션은 선택 사항입니다.

## Comfy-pilot이란?

Comfy-pilot은 ComfyUI 워크플로우를 프로그래밍 방식으로 관리하고 실행할 수 있는 도구입니다.

ComfyUI의 워크플로우를 API로 호출하거나, 배치 처리, 자동화된 파이프라인 구성 등을 지원합니다.

## 주요 기능

1. **워크플로우 API 호출**: ComfyUI 워크플로우를 REST API로 실행
2. **배치 처리**: 여러 프롬프트나 이미지를 일괄 처리
3. **파이프라인 자동화**: 여러 워크플로우를 연결하여 자동화된 생성 파이프라인 구성
4. **결과 관리**: 생성된 이미지/비디오의 체계적인 관리

## 데모 시나리오

### 시나리오 1: 프롬프트 배치 처리

여러 프롬프트를 준비하고 한 번에 이미지를 생성하는 데모입니다.

```python
# 예시: 여러 프롬프트로 이미지 일괄 생성
prompts = [
    "a serene mountain landscape at sunset",
    "a futuristic city skyline at night",
    "a cozy coffee shop interior, warm lighting"
]

for prompt in prompts:
    result = comfy_pilot.run_workflow(
        workflow="text-to-image",
        prompt=prompt
    )
```

### 시나리오 2: 워크플로우 체이닝

이미지 생성 → 업스케일링 → 스타일 변환을 자동으로 연결하는 데모입니다.

```
[Text-to-Image] → [Upscaler] → [Style Transfer] → [Output]
```

## 참고 자료

- 데모는 워크샵 진행자가 라이브로 시연합니다
- 상세한 사용법은 프로젝트 문서를 참고하세요
