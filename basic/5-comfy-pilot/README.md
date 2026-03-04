# #5. Demos - Comfypilot

> 이 섹션은 선택 사항입니다.

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
