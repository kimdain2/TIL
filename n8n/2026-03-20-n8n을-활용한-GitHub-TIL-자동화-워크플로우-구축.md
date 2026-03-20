### Context
* 디스코드(Discord)를 통해 입력된 단편적인 메모를 AI 에이전트가 구조화된 기술 문서(TIL)로 변환하고, 이를 GitHub 저장소에 자동으로 기록하는 워크플로우를 구축함.
* 수동으로 이루어지던 문서 작성, 커밋, 푸시 및 README 색인 업데이트 과정을 n8n을 통해 자동화하여 지식 관리의 효율성을 극대화하는 것이 목표임.

### Core
* **워크플로우 설계 구조**
    * **Discord Trigger**: 'On Message' 이벤트를 감지하여 사용자가 입력한 텍스트 데이터를 수신함.
    * **AI Agent Node**: `LLM Chain` 또는 `AI Agent` 노드를 활용하여 입력된 메시지를 사전에 정의된 TIL Markdown 템플릿으로 변환함.
    * **GitHub Node (File Push)**: 변환된 내용을 바탕으로 `YYYY-MM-DD-제목.md` 형태의 파일명을 생성하고, 지정된 디렉토리에 파일을 생성(Create/Update)함.
    * **GitHub Node (README Update)**: 기존 `README.md` 파일을 호출(Get)한 뒤, `Code Node`를 사용하여 새로운 문서의 링크를 인덱스 리스트 최상단에 추가하고 다시 업데이트함.
    * **Discord Node (Notification)**: 모든 프로세스가 완료되면 사용자에게 성공 메시지와 커밋 주소를 발신함.

* **기술적 핵심 설정**
    * **Expression**: 노드 간 데이터 전달 시 `{{ $json.content }}`와 같은 표현식을 사용하여 이전 노드의 출력값을 참조함.
    * **Binary Property**: GitHub 노드에서 파일을 생성할 때 변환된 텍스트 데이터를 `Binary` 데이터로 변환하여 처리해야 함.

### Insight
* n8n 워크플로우 구축의 핵심은 각 노드의 **입력(Input)**과 **출력(Output)** 데이터 구조(Schema)를 정확히 파악하는 것임.
* 이전 노드가 내뱉는 JSON 객체의 키(Key) 값을 다음 노드에서 어떻게 호출하는지만 이해하면 복잡한 로직도 손쉽게 연결할 수 있음.
* 단순한 API 연결을 넘어 AI 에이전트와 버전 관리 시스템(GitHub)을 결합함으로써 개인 학습 파이프라인의 '완전 자동화' 가능성을 확인했음.
* 후츠릿 강사의 교육을 통해 노드 기반 자동화 툴의 강력한 생산성을 경험할 수 있었음.

**출처:** [n8n GitHub Node Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.github/), [n8n AI Agent Node Guide](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.aichatagent/)