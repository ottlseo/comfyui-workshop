# ComfyUI Workshop

ComfyUI는 생성형 AI를 위한 노드 기반 인터페이스 및 추론 엔진입니다.

사용자는 노드를 통해 다양한 AI 모델과 작업을 결합하여 더 높은 수준의 맞춤형 및 제어 가능한 생성 파이프라인을 구현할 수 있습니다.

<figure><img src=".gitbook/assets/image (21).png" alt=""><figcaption><p>ComfyUI Interface</p></figcaption></figure>

이번 워크샵에서는 AWS 환경에서 ComfyUI를 프로비저닝하고, 다양한 생성형 AI 워크플로우를 직접 구성하고 실습합니다.

## 워크샵 아젠다

| 순서 | 주제 | 내용 | 소요 시간 |
|------|------|------|-----------|
| 0 | [Prerequisites](0-prerequisites/README.md) | ComfyUI 개념, 용어 정리, 사전 설정 | 15분 |
| 1 | [Setup](1-setup/README.md) | ECS 기반 ComfyUI 설치 및 설정 | 15분 |
| 2 | [Workflow 알아보기](2-workflow-basics/README.md) | Stable Diffusion 워크플로우 구성 및 실습 | 40분 |
| 3 | [LoRA 실습](3-lora/README.md) | SDXL LoRA 적용 및 Stacking | 30분 |
| 4 | [워크플로우 실습](4-workflow-practices/README.md) | 다양한 워크플로우 실습 | 120분 |
| 4-1 | [ControlNet](4-workflow-practices/4-1-controlnet.md) | ControlNet 실습 | 20분 |
| 4-2 | [Upscaler](4-workflow-practices/4-2-upscaler.md) | CCSR / SUPIR 업스케일러 | 15분 |
| 4-3 | [Z-Image](4-workflow-practices/4-3-z-image.md) | Z-Image Turbo 이미지 생성 | 15분 |
| 4-4 | [Qwen Image](4-workflow-practices/4-4-qwen-image.md) | Qwen Image Edit / Layered | 15분 |
| 4-5 | [Image Relighting](4-workflow-practices/4-5-image-relighting.md) | LBM 기반 Relighting | 15분 |
| 4-6 | [Wan 2.2](4-workflow-practices/4-6-wan22.md) | Wan 2.2 비디오 생성 | 20분 |
| 4-7 | [Wan 2.2 LoRA](4-workflow-practices/4-7-wan22-lora.md) | Wan 2.2 LoRA 활용 | 20분 |
| 5 | [Comfy-pilot](5-comfy-pilot/README.md) | (Optional) Comfy-pilot 데모 | - |

## AWS 환경에서 ComfyUI를 활용하는 장점

1. **유연한 배포 옵션**
   - 단일 EC2부터 컨테이너화 된 ECS/EKS까지 요구사항에 맞게 선택 가능
   - 다양한 컴퓨팅 리소스를 워크로드에 맞춰 활용 가능 (CPU only / GPU)
   - Spot Instance 활용으로 비용 절감 가능
2. **사용량 기반 비용**
   - 필요할 때만 리소스를 사용하고 비용 지불
3. **운영 및 관리**
   - 통합 환경에서 중앙화 된 운영 및 모니터링 가능
   - IaC 기반 자동화 된 배포
4. **보안**
   - IAM 기반 접근 제어 및 Cognito, SAML 인증 통합
   - 네트워크 분리
   - IP 및 도메인 제한, WAF를 이용한 애플리케이션 보호

## AWS Samples

- [ComfyUI on ECS](https://github.com/aws-samples/cost-effective-aws-deployment-of-comfyui)
- [ComfyUI on EKS](https://github.com/aws-samples/comfyui-on-eks)
- [ComfyUI on SageMaker](https://github.com/aws-samples/comfyui-on-amazon-sagemaker)
