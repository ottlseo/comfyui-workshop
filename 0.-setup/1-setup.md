# #0-1. Setup

---

## 📚 학습 목표
이 섹션을 완료하면 다음을 할 수 있습니다:
- AWS CDK를 사용하여 ECS 기반 ComfyUI 배포하기
- ComfyUI Manager를 활용하여 커스텀 노드와 모델 관리하기
- Crystools 플러그인으로 시스템 리소스 모니터링하기
- EFS 기반 모델 저장 구조 이해하기

**⏱️ 소요 시간**: 약 15분 (배포 시간 포함 20-25분)
**📊 난이도**: ⭐⭐ 초급 (AWS 기본 지식 필요)

---

## AWS CDK를 이용한 ComfyUI on ECS 배포

이 가이드에서는 AWS CDK를 사용하여 ECS 기반 ComfyUI를 배포하는 방법을 단계별로 안내합니다.

**GitHub Repository**: [https://github.com/aws-samples/cost-effective-aws-deployment-of-comfyui](https://github.com/aws-samples/cost-effective-aws-deployment-of-comfyui)

> **초보자 팁**: CDK(Cloud Development Kit)는 코드로 AWS 인프라를 관리하는 도구입니다. 복잡한 설정을 자동화하여 클릭 몇 번으로 전체 환경을 구축할 수 있습니다.

## 배포 단계

### 1단계: SageMaker Studio 접속

AWS 콘솔에서 SageMaker Studio로 이동하여 개발 환경에 접속합니다.

**무엇을 하는 단계인가요?** SageMaker Studio는 AWS에서 제공하는 클라우드 기반 개발 환경입니다. 여기서 배포 명령을 실행합니다.

> **초보자 팁**: SageMaker Studio를 찾을 수 없다면 AWS 콘솔 상단 검색창에 "SageMaker"를 입력하세요.

### 2단계: 터미널 열기

SageMaker Studio 내에서 새 터미널을 엽니다.

**무엇을 하는 단계인가요?** 터미널은 명령어를 입력하는 화면입니다. 파일 탐색기 대신 텍스트로 명령을 실행합니다.

### 3단계: 리포지토리 클론

```bash
git clone https://github.com/aws-samples/cost-effective-aws-deployment-of-comfyui.git
cd cost-effective-aws-deployment-of-comfyui
```

**무엇을 하는 단계인가요?** ComfyUI 배포에 필요한 파일들을 GitHub에서 다운로드합니다.

**예상 소요 시간**: 약 30초

### 4단계: 의존성 설치

```bash
npm install
```

**무엇을 하는 단계인가요?** 배포에 필요한 라이브러리들을 자동으로 설치합니다.

**예상 소요 시간**: 약 1-2분

> **초보자 팁**: 화면에 많은 텍스트가 빠르게 지나가는데, 이는 정상입니다. 에러 메시지가 아니라면 기다리기만 하면 됩니다.

### 5단계: CDK 부트스트랩

계정에서 CDK를 처음 사용하는 경우 부트스트랩이 필요합니다:

```bash
cdk bootstrap
```

**무엇을 하는 단계인가요?** AWS 계정에 CDK 사용을 위한 기본 설정을 초기화합니다. 계정당 한 번만 하면 됩니다.

**예상 소요 시간**: 약 2-3분

> **초보자 팁**: "already bootstrapped" 메시지가 나오면 이미 설정되어 있다는 뜻입니다. 다음 단계로 넘어가세요.

### 6단계: CDK 배포

```bash
cdk deploy
```

배포 과정에서 생성될 리소스에 대한 확인 메시지가 표시됩니다. `y`를 입력하여 진행합니다.

**무엇을 하는 단계인가요?** 실제로 ComfyUI가 작동할 클라우드 환경(ECS, ALB, EFS 등)을 생성합니다.

**예상 소요 시간**: 약 10-15분

> **초보자 팁**: 이 단계가 가장 오래 걸립니다. 커피 한 잔 하고 오세요! 중간에 창을 닫지 마세요.

### 7단계: 배포 완료 및 접속

배포가 완료되면 출력에서 ALB(Application Load Balancer) 엔드포인트 URL을 확인할 수 있습니다. 해당 URL로 접속하여 ComfyUI를 사용합니다.

**무엇을 확인하나요?**
- 터미널 출력 마지막에 `Outputs:` 섹션이 나타납니다
- `ComfyUIStack.LoadBalancerDNS = xxxxxxxx.elb.amazonaws.com` 형태의 URL을 복사합니다
- 웹 브라우저에 붙여넣고 접속합니다

> **초보자 팁**: URL이 바로 작동하지 않는다면? 컨테이너가 시작되는 데 1-2분 추가로 걸릴 수 있습니다. 잠시 후 새로고침해보세요.

### 8단계: Cognito 인증 설정

첫 접속 시 Cognito를 통한 사용자 인증이 필요합니다. 화면의 안내에 따라 사용자 계정을 생성하거나 로그인합니다.

**무엇을 하는 단계인가요?** ComfyUI 접근을 위한 사용자 계정을 만듭니다. 이메일과 비밀번호를 설정하세요.

> **초보자 팁**: 비밀번호는 최소 8자, 대소문자/숫자/특수문자 포함이어야 합니다.

## ComfyUI Manager 설정

ComfyUI Manager는 커스텀 노드와 모델을 쉽게 관리할 수 있는 필수 도구입니다.

### Manager 접근

ComfyUI 인터페이스 우측의 **Manager** 버튼을 클릭합니다.

![Manager 버튼](<../.gitbook/assets/image (78).png>)

### Custom Nodes Manager

Custom Nodes Manager를 통해 다양한 커스텀 노드를 검색하고 설치할 수 있습니다:

1. Manager 메뉴에서 **Custom Nodes Manager** 선택
2. 원하는 노드 검색
3. **Install** 버튼 클릭
4. ComfyUI 재시작

![Custom Nodes Manager](<../.gitbook/assets/image (79).png>)

### Model Manager

Model Manager를 통해 필요한 모델을 직접 다운로드하고 관리할 수 있습니다:

1. Manager 메뉴에서 **Model Manager** 선택
2. 모델 타입 선택 (Checkpoint, VAE, LoRA 등)
3. 원하는 모델 검색
4. **Download** 버튼 클릭

![Model Manager](<../.gitbook/assets/image (80).png>)

## 모델 경로 관리

### EFS 기반 모델 저장

이 배포 아키텍처는 EFS(Elastic File System)를 사용하여 모델을 영구적으로 저장합니다:

* 모델 기본 경로: `/workspace/models/`
* Checkpoints: `/workspace/models/checkpoints/`
* VAE: `/workspace/models/vae/`
* LoRA: `/workspace/models/loras/`
* Embeddings: `/workspace/models/embeddings/`

EFS를 사용하므로 컨테이너가 재시작되어도 다운로드한 모델이 유지됩니다.

## 유용한 플러그인 설치

### ComfyUI-Crystools (리소스 모니터)

시스템 리소스를 실시간으로 모니터링하는 플러그인입니다.

**왜 필요한가요?**
- GPU/CPU 사용률을 실시간으로 확인하여 워크플로우 성능을 파악할 수 있습니다
- 메모리 부족으로 인한 오류를 사전에 감지할 수 있습니다
- 어떤 노드가 자원을 많이 사용하는지 알 수 있습니다

**설치 방법**:
1. Manager → Custom Nodes Manager 열기
2. "Crystools" 검색
3. **ComfyUI-Crystools** 찾아서 Install 클릭
4. ComfyUI 재시작

![Crystools 검색](<../.gitbook/assets/image (81).png>)

설치 후 우측 패널에 CPU, 메모리, GPU 사용률이 표시됩니다.

![리소스 모니터](<../.gitbook/assets/image (82).png>)

> **초보자 팁**: GPU 사용률이 100%에 가까우면 정상입니다. 이미지 생성은 GPU를 많이 사용하는 작업입니다!

### ComfyUI-Model-Manager

모델 다운로드 및 관리를 더욱 편리하게 해주는 플러그인입니다:

1. Manager → Custom Nodes Manager 열기
2. "Model Manager" 검색
3. **ComfyUI-Model-Manager** 찾아서 Install 클릭
4. ComfyUI 재시작

![Model Manager 설치](<../.gitbook/assets/image (83).png>)

설치 후 노드 추가 메뉴에서 Model Manager 관련 노드를 사용할 수 있습니다.

![Model Manager 노드](<../.gitbook/assets/image (84).png>)

## 문제 해결 (Troubleshooting)

배포나 설치 중에 문제가 발생했나요? 아래의 일반적인 문제와 해결 방법을 확인하세요.

### CDK 배포 오류

**증상**: `cdk deploy` 실행 시 에러 발생

**해결 방법**:
1. AWS 계정에 충분한 권한이 있는지 확인
2. AWS 리전이 올바르게 설정되었는지 확인
3. 이전 배포가 있다면 `cdk destroy` 후 재시도
4. 터미널을 닫고 새로 열어서 다시 시도

```bash
# 리전 확인
aws configure get region

# 기존 스택 삭제 (필요시)
cdk destroy
```

### ComfyUI URL 접속 불가

**증상**: ALB URL로 접속했지만 페이지가 로드되지 않음

**해결 방법**:
1. 1-2분 기다린 후 새로고침 (컨테이너 시작 시간)
2. AWS 콘솔 → ECS → Clusters에서 태스크 상태 확인
3. 태스크가 RUNNING 상태인지 확인
4. CloudWatch Logs에서 에러 로그 확인

> **초보자 팁**: 브라우저 개발자 도구(F12)를 열어 Console 탭에서 에러 메시지를 확인할 수 있습니다.

### Manager 버튼이 보이지 않음

**증상**: ComfyUI 인터페이스에서 Manager 버튼을 찾을 수 없음

**해결 방법**:
1. 페이지를 새로고침 (Ctrl+F5 또는 Cmd+Shift+R)
2. 브라우저 캐시 삭제
3. 다른 브라우저로 시도 (Chrome 권장)
4. ECS 태스크 재시작

### 모델 다운로드가 실패함

**증상**: Model Manager에서 모델 다운로드 시 에러 발생

**해결 방법**:
1. 인터넷 연결 상태 확인
2. EFS 용량 확인 (AWS 콘솔 → EFS)
3. Hugging Face/CivitAI 로그인 필요 여부 확인
4. 다른 모델로 시도

---

## ✅ 설정 완료 체크리스트

모든 설정이 완료되었는지 확인하세요:

- [ ] CDK 배포가 성공적으로 완료되었다
- [ ] ALB URL로 ComfyUI 접속이 가능하다
- [ ] Cognito 인증을 통해 로그인했다
- [ ] Manager 버튼이 우측에 보인다
- [ ] ComfyUI-Crystools 플러그인을 설치했다
- [ ] 우측 패널에 리소스 모니터가 표시된다
- [ ] Model Manager를 열어볼 수 있다

모든 항목을 체크했다면, 본격적인 워크플로우 학습을 시작할 준비가 완료되었습니다!

---

## 다음 단계

설치가 완료되었습니다! 이제 [Workflow 알아보기](../basic/2-workflow-basics/) 섹션으로 이동하여 ComfyUI의 기본 워크플로우를 학습하세요.
