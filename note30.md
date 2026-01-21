## OpenAI Responses API

### Chat Completions vs Responses 차이점
상태 유지 방식
    Chat Completions는 매번 전체 대화 내역을 리스트로 묶어 보내야 하지만, Responses API는 previous_response_id를 통해 서버 측에서 이전 응답을 참조하는 상태 중심(Statusful) 상호작용을 지원
입력 파라미터
    기존의 messages 필드가 input 필드로 대체
    input은 단순 문자열이나 이미지, 파일 등 다양한 형태를 포함하는 배열로 구성될 수 있음
시스템 메세지
    role: system 방식 대신 instructions라는 별도의 최상위 파라미터를 사용하여 모델의 페르소나와 지침을 설정
응답 구조
    결과 데이터가 choices 대신 output 배열에 담겨 반환
내장 도구
    별도의 세션 관리 없이도 웹 검색(Web Search) 등의 도구를 더욱 직접적으로 활용할 수 있음

### REST API 요청
라이브러리 없이 직접 HTTP 통신을 통해 /v1/responses 엔드포인트를 호출

### OpenAI SDK를 활용한 요청
client.responses 리소스를 사용하여 효율적으로 응답을 생성

### Instructions (System Pormpt) 비교
기존의 system role 메세지 대신 instructions 파라미터에 지침을 입력

### input
input에는 text뿐만 아니라 메시지 객체, 이미지, 파일 등이 들어갈 수 있음  
메시지 객체는 다음과 같은 형식을 가짐
    {
    'content' : 컨텐트에 대한 text 또는 list,
    'role' : 'user' / 'assistant' / 'system' / 'developer' 중 하나가 들어감
    'type' : 'message'
    }


### Temperature 비교
무작위성을 조절하는 temperature 파라미터는 동일하게 사용 가능

### previous_response_id를 활용한 대화 맥락 유지
과거 응답의 ID를 전달하여 이전 대화 내용을 자동으로 참조

### Structured Outputs (JSON 형식 출력)
text 파라미터의 format 설정을 통해 JSON 출력을 강제할 수 있음

### Streaming (실시간 응답 처리)
stream=True 설정을 통해 생성되는 즉시 데이터를 수신

### 비동기 요청
AsyncOpenAI 클라이언트를 사용하여 병렬로 여러 응답을 처리

### 추가 관리 기능 (조회, 삭제, 압축)
생성된 응답을 ID로 조회하거나 삭제하고, 긴 대화의 컨텍스트를 압축하는 기능을 제공