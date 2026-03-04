# 1. Setup - ECS 기반 ComfyUI 설치

## AWS CDK를 이용한 ComfyUI on ECS 배포

이 가이드에서는 AWS CDK를 사용하여 ECS 기반 ComfyUI를 배포하는 방법을 단계별로 안내합니다.

**GitHub Repository**: [https://github.com/aws-samples/cost-effective-aws-deployment-of-comfyui](https://github.com/aws-samples/cost-effective-aws-deployment-of-comfyui)

## 배포 단계

### 1단계: SageMaker Studio 접속

AWS 콘솔에서 SageMaker Studio로 이동하여 개발 환경에 접속합니다.

### 2단계: 터미널 열기

SageMaker Studio 내에서 새 터미널을 엽니다.

### 3단계: 리포지토리 클론

```bash
git clone https://github.com/aws-samples/cost-effective-aws-deployment-of-comfyui.git
cd cost-effective-aws-deployment-of-comfyui
```

### 4단계: 의존성 설치

```bash
npm install
```

### 5단계: CDK 부트스트랩

계정에서 CDK를 처음 사용하는 경우 부트스트랩이 필요합니다:

```bash
cdk bootstrap
```

### 6단계: CDK 배포

```bash
cdk deploy
```

배포 과정에서 생성될 리소스에 대한 확인 메시지가 표시됩니다. `y`를 입력하여 진행합니다.

### 7단계: 배포 완료 및 접속

배포가 완료되면 출력에서 ALB(Application Load Balancer) 엔드포인트 URL을 확인할 수 있습니다. 해당 URL로 접속하여 ComfyUI를 사용합니다.

### 8단계: Cognito 인증 설정

첫 접속 시 Cognito를 통한 사용자 인증이 필요합니다. 화면의 안내에 따라 사용자 계정을 생성하거나 로그인합니다.

## ComfyUI Manager 설정

ComfyUI Manager는 커스텀 노드와 모델을 쉽게 관리할 수 있는 필수 도구입니다.

### Manager 접근

ComfyUI 인터페이스 우측의 **Manager** 버튼을 클릭합니다.

![Manager 버튼](../.gitbook/assets/image&#32;(78).png)

### Custom Nodes Manager

Custom Nodes Manager를 통해 다양한 커스텀 노드를 검색하고 설치할 수 있습니다:

1. Manager 메뉴에서 **Custom Nodes Manager** 선택
2. 원하는 노드 검색
3. **Install** 버튼 클릭
4. ComfyUI 재시작

![Custom Nodes Manager](../.gitbook/assets/image&#32;(79).png)

### Model Manager

Model Manager를 통해 필요한 모델을 직접 다운로드하고 관리할 수 있습니다:

1. Manager 메뉴에서 **Model Manager** 선택
2. 모델 타입 선택 (Checkpoint, VAE, LoRA 등)
3. 원하는 모델 검색
4. **Download** 버튼 클릭

![Model Manager](../.gitbook/assets/image&#32;(80).png)

## 모델 경로 관리

### EFS 기반 모델 저장

이 배포 아키텍처는 EFS(Elastic File System)를 사용하여 모델을 영구적으로 저장합니다:

- 모델 기본 경로: `/workspace/models/`
- Checkpoints: `/workspace/models/checkpoints/`
- VAE: `/workspace/models/vae/`
- LoRA: `/workspace/models/loras/`
- Embeddings: `/workspace/models/embeddings/`

EFS를 사용하므로 컨테이너가 재시작되어도 다운로드한 모델이 유지됩니다.

## 유용한 플러그인 설치

### ComfyUI-Crystools (리소스 모니터)

시스템 리소스를 실시간으로 모니터링하는 플러그인입니다:

1. Manager → Custom Nodes Manager 열기
2. "Crystools" 검색
3. **ComfyUI-Crystools** 찾아서 Install 클릭
4. ComfyUI 재시작

![Crystools 검색](../.gitbook/assets/image&#32;(81).png)

설치 후 우측 패널에 CPU, 메모리, GPU 사용률이 표시됩니다.

![리소스 모니터](../.gitbook/assets/image&#32;(82).png)

### ComfyUI-Model-Manager

모델 다운로드 및 관리를 더욱 편리하게 해주는 플러그인입니다:

1. Manager → Custom Nodes Manager 열기
2. "Model Manager" 검색
3. **ComfyUI-Model-Manager** 찾아서 Install 클릭
4. ComfyUI 재시작

![Model Manager 설치](../.gitbook/assets/image&#32;(83).png)

설치 후 노드 추가 메뉴에서 Model Manager 관련 노드를 사용할 수 있습니다.

![Model Manager 노드](../.gitbook/assets/image&#32;(84).png)

## 다음 단계

설치가 완료되었습니다! 이제 [Workflow 알아보기](../2-workflow-basics/) 섹션으로 이동하여 ComfyUI의 기본 워크플로우를 학습하세요.
