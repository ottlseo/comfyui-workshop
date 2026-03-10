# #5. Demos - Comfypilot

## 학습 목표
이 데모를 완료하면 다음을 할 수 있습니다:
- Comfy-pilot의 개념과 활용 방법 이해하기
- Claude Code와 ComfyUI의 통합 환경 경험하기
- 자연어로 워크플로우를 생성하고 편집하는 방법 익히기

## 소요 시간
약 10분 (선택사항)

## 난이도
★☆☆ (체험용 데모)

> **주의**: 이 섹션은 **선택 사항**입니다. Comfy-pilot은 고급 사용자를 위한 도구이며, 기본 ComfyUI 워크플로우 학습에는 필수가 아닙니다. 워크플로우 자동화에 관심이 있거나, AI 어시스턴트로 작업하고 싶을 때 시도해보세요.

## Comfy-pilot이란?

**Comfy Pilot**은 Claude Code와 ComfyUI를 연결하는 **MCP 서버 + 내장 터미널** 커스텀 노드입니다. ComfyUI 안에서 자연어로 워크플로우를 생성/편집/실행할 수 있게 해줍니다.

**핵심 기능:**

* **MCP 서버**: Claude Code가 워크플로우를 직접 조회, 편집, 실행
* **내장 터미널**: ComfyUI 안에 xterm.js 기반 Claude Code 터미널 탑재
* **이미지 뷰잉**: Claude가 Preview/Save Image 노드의 출력을 직접 확인
* **그래프 편집**: 노드 생성/삭제/이동/연결을 프로그래밍 방식으로 처리
* **모델 다운로드**: HuggingFace, CivitAI에서 모델 직접 다운로드

**사용 예시:**

* "SDXL + ControlNet 워크플로우 만들어줘"
* "프리뷰 이미지 보고 디테일 높여줘"
* "FLUX schnell 모델 다운받고 워크플로우 세팅해줘"

### Comfy-pilot이 유용한 경우

- **워크플로우 자동화**: 반복적인 노드 배치를 자동화하고 싶을 때
- **빠른 실험**: 다양한 모델과 설정을 빠르게 테스트하고 싶을 때
- **학습 보조**: AI가 워크플로우를 설명하고 추천해주길 원할 때
- **모델 관리**: 여러 모델을 자동으로 다운로드하고 관리하고 싶을 때

> **초보자 팁**: Comfy-pilot은 ComfyUI에 어느 정도 익숙해진 후에 시도하세요. 기본 워크플로우를 손으로 구성하는 경험이 있어야 AI 어시스턴트의 도움을 효과적으로 활용할 수 있습니다.

***

#### 설치 방법

**방법 1 - CLI (권장):**

```bash
comfy node install comfy-pilot
```

**방법 2 - ComfyUI Manager:**

1. ComfyUI에서 **Manager** → **Install Custom Nodes** 클릭
2. `Comfy Pilot` 검색
3. **Install** 클릭
4. ComfyUI 재시작

**방법 3 - Git Clone:**

```bash
cd ~/Documents/ComfyUI/custom_nodes && git clone https://github.com/ConstantineB6/comfy-pilot.git
```

> Claude Code CLI가 없으면 자동 설치됩니다.



## 데모

{% embed url="https://private-user-images.githubusercontent.com/90444271/550379895-325b1194-2334-48a1-94c3-86effd1fef02.mp4?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NzI2MDYyNzUsIm5iZiI6MTc3MjYwNTk3NSwicGF0aCI6Ii85MDQ0NDI3MS81NTAzNzk4OTUtMzI1YjExOTQtMjMzNC00OGExLTk0YzMtODZlZmZkMWZlZjAyLm1wND9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjAzMDQlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwMzA0VDA2MzI1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTRkMTA0MzdkNzM0MDgzNTAyOGJjZWZjZWYzM2RlOTU4ODdlYjg4ZTRlYTRiODdkMmJhMWIyMGQ2NDRkZDgyY2ImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.Mo34xMUkIWVYGsFz9arsQyEhK9RY6SEp7t66avS73Lo" %}
