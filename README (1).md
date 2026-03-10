# 1. ComfyUI Workshop - basic

ComfyUI는 생성형 AI를 위한 노드 기반 인터페이스 및 추론 엔진입니다.

사용자는 노드를 통해 다양한 AI 모델과 작업을 결합하여 더 높은 수준의 맞춤형 및 제어 가능한 생성 파이프라인을 구현할 수 있습니다.

이번 워크샵에서는 AWS 환경에서 ComfyUI를 프로비저닝하고, 다양한 생성형 AI 워크플로우를 직접 구성하고 실습합니다.

---

## 🎯 대상 및 사전 준비사항

**이 워크샵은 다음과 같은 분들에게 적합합니다**:
- AI 이미지 생성에 관심이 있는 초보자
- Stable Diffusion을 처음 접해보는 분
- ComfyUI를 체계적으로 배우고 싶은 분
- AWS에서 생성형 AI를 활용하고 싶은 개발자

**사전 준비사항**:
- ✅ AWS 계정 (워크샵용)
- ✅ Chrome 또는 Firefox 브라우저
- ✅ 기본적인 컴퓨터 조작 능력 (드래그, 클릭 등)
- ❌ 프로그래밍 지식 불필요
- ❌ AI/머신러닝 배경 지식 불필요

---

## 워크샵 아젠다

| 순서  | 주제                                                                     | 내용                             | 소요 시간 | 난이도 |
| --- | ---------------------------------------------------------------------- | ------------------------------ | ----- | ----- |
| 0   | [Prerequisites](0.-setup/0-prerequisites.md)                           | ComfyUI 개념, 용어 정리, 사전 설정       | 15분   | ⭐ 입문 |
| 1   | [Setup](0.-setup/1-setup.md)                                           | ECS 기반 ComfyUI 설치 및 설정         | 15분   | ⭐⭐ 초급 |
| 2   | [Workflow 알아보기](basic/2-workflow-basics/)                              | Stable Diffusion 워크플로우 구성 및 실습 | 40분   | ⭐⭐ 초급 |
| 3   | [LoRA 실습](basic/3-lora/)                                               | SDXL LoRA 적용 및 Stacking        | 30분   | ⭐⭐ 초급 |
| 4   | [워크플로우 실습](basic/4-workflow-practices/)                                | 다양한 워크플로우 실습                   | 120분  | ⭐⭐⭐ 중급 |
| 4-1 | [ControlNet](basic/4-workflow-practices/4-1-controlnet.md)             | ControlNet 실습                  | 20분   | ⭐⭐⭐ 중급 |
| 4-2 | [Upscaler](basic/4-workflow-practices/4-2-upscaler.md)                 | CCSR / SUPIR 업스케일러             | 15분   | ⭐⭐ 초급 |
| 4-3 | [Z-Image](basic/4-workflow-practices/4-3-z-image.md)                   | Z-Image Turbo 이미지 생성           | 15분   | ⭐⭐ 초급 |
| 4-4 | [Qwen Image](basic/4-workflow-practices/4-4-qwen-image.md)             | Qwen Image Edit / Layered      | 15분   | ⭐⭐⭐ 중급 |
| 4-5 | [Image Relighting](basic/4-workflow-practices/4-5-image-relighting.md) | LBM 기반 Relighting              | 15분   | ⭐⭐⭐ 중급 |
| 4-6 | [Wan 2.2](basic/4-workflow-practices/4-6-wan22.md)                     | Wan 2.2 비디오 생성                 | 20분   | ⭐⭐⭐⭐ 고급 |
| 4-7 | [Wan 2.2 LoRA](basic/4-workflow-practices/4-7-wan22-lora.md)           | Wan 2.2 LoRA 활용                | 20분   | ⭐⭐⭐⭐ 고급 |
| 5   | [Comfy-pilot](basic/5-comfy-pilot/)                                    | (Optional) Comfy-pilot 데모      | -     | ⭐⭐⭐ 중급 |

**난이도 안내**:
- ⭐ 입문: 코딩 경험 불필요, 기본 개념 학습
- ⭐⭐ 초급: 기본 노드 조작, 간단한 워크플로우
- ⭐⭐⭐ 중급: 복잡한 노드 조합, 고급 기능 활용
- ⭐⭐⭐⭐ 고급: 여러 모델 결합, 최적화 필요

---

## 📊 워크샵 진행 흐름도

```
[0. Prerequisites]
    기본 개념 이해
         ↓
[1. Setup]
    AWS 환경 구축
         ↓
[2. Workflow 알아보기]
    첫 이미지 생성 🎨
         ↓
[3. LoRA 실습]
    스타일 적용하기
         ↓
[4. 워크플로우 실습]
    고급 기능 마스터
    ├─ 4-1. ControlNet (구도 제어)
    ├─ 4-2. Upscaler (해상도 향상)
    ├─ 4-3. Z-Image (빠른 생성)
    ├─ 4-4. Qwen Image (이미지 편집)
    ├─ 4-5. Image Relighting (조명 조정)
    ├─ 4-6. Wan 2.2 (비디오 생성)
    └─ 4-7. Wan 2.2 LoRA (비디오 스타일링)
         ↓
[5. Comfy-pilot]
    (선택) 자동화 도구 체험 🚀
```

**추천 학습 순서**:
1. 0번부터 3번까지는 순서대로 진행 (필수)
2. 4번 워크플로우 실습은 관심사에 따라 선택적으로 진행 가능
3. 이미지 생성에만 관심 있다면: 4-1, 4-2, 4-3 추천
4. 비디오 생성에 관심 있다면: 4-6, 4-7 추천
5. 이미지 편집에 관심 있다면: 4-4, 4-5 추천

---

## AWS 환경에서 ComfyUI를 활용하는 장점

1. **유연한 배포 옵션**
   * 단일 EC2부터 컨테이너화 된 ECS/EKS까지 요구사항에 맞게 선택 가능
   * 다양한 컴퓨팅 리소스를 워크로드에 맞춰 활용 가능 (CPU only / GPU)
   * Spot Instance 활용으로 비용 절감 가능
2. **사용량 기반 비용**
   * 필요할 때만 리소스를 사용하고 비용 지불
3. **운영 및 관리**
   * 통합 환경에서 중앙화 된 운영 및 모니터링 가능
   * IaC 기반 자동화 된 배포
4. **보안**
   * IAM 기반 접근 제어 및 Cognito, SAML 인증 통합
   * 네트워크 분리
   * IP 및 도메인 제한, WAF를 이용한 애플리케이션 보호

## AWS Samples

* [ComfyUI on ECS](https://github.com/aws-samples/cost-effective-aws-deployment-of-comfyui)
* [ComfyUI on EKS](https://github.com/aws-samples/comfyui-on-eks)
* [ComfyUI on SageMaker](https://github.com/aws-samples/comfyui-on-amazon-sagemaker)
