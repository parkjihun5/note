## OpenAI Chat Completions API
openai의 api key를 .env의 OPENAI_API_KEY에 등록하여 사용

### REST API 요청
라이브러리 없이 직접 HTTP 통신을 통해 api를 호출

### OpenAI SDK를 활용한 요청
공식 라이브러리를 사용하여 생산성을 높이는 표준 방식
    pip install openai 를 통해 설치

### System Prompt 비교
동일한 질문에 대해 AI의 페르소나에 따라 답변이 어떻게 달라지는지 확인해 보자

code
---

user_input = "아침 일찍 일어나는 습관의 장점에 대해 말해줘."

personas = {
    "열정적인 셰프": "당신은 요리에 인생을 건 셰프입니다. 인생의 모든 이치를 요리 과정과 재료에 비유하여 설명하세요.",
    "엄격한 헬스 트레이너": "당신은 매우 엄격한 운동 전문가입니다. 강한 어조로 자기관리를 강조하며 답변하세요.",
    "지혜로운 판다": "당신은 대나무 숲에 사는 느긋하고 지혜로운 판다입니다. 느릿느릿하고 평화로운 말투로 조언을 건네세요."
}

for name, prompt in personas.items():
    print(f"--- [{name}] 버전 ---")
    response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "system", "content": prompt},
            {"role": "user", "content": user_input}
        ]
    )
    print(response.choices[0].message.content)
    print("\n")

---

### Temperature 비교
동일한 질문에 대해 temperature에 따라 답변이 어떻게 달라지는지 확인해 보자

code
---

creative_topic = "운동화 브랜드의 새로운 슬로건을 5개 제안해줘. 단, '속도'나 '승리' 같은 뻔한 단어는 제외하고 아주 기발하게 작성해줘."
temperatures = [0.3, 0.8, 1.0, 1.3, 1.5, 1.6, 1.8]

for t in temperatures:
    print(f"### 설정값 (Temperature): {t} ###")
    response = client.chat.completions.create(
        model=model,
        messages=[{"role": "user", "content": creative_topic}],
        temperature=t,
        max_completion_tokens=200, 
        timeout=15.0
    )
    print(response.choices[0].message.content)
    print("=" * 50)

---

### messages 배열을 활용한 대화 맥락 유지 (Context Window)
Chat Completions API는 상태를 저장하지 않는(Stateless) 방식이므로, 이전 대화 내역을 리스트에 계속 누적해서 보내야 함

code
---

def chat_without_memory(user_input):
    
    response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "user", "content": user_input}
        ]
    )
    
    answer = response.choices[0].message.content
    
    return answer

print("Q1: 내 이름은 XXX이야.")
print(f"A1: {chat_without_memory('내 이름은 XXX이야')}\n")

print("Q2: 내 이름이 뭐라고?")
print(f"A2: {chat_without_memory('내 이름이 뭐라고?')}")

---

### Structured Outputs (구조화된 출력)
모델의 답변을 산순히 텍스트로 받는 것이 아니라, JSON 형태로 고정하여 받을 수 있음
웹 서비스의 백엔드에서 데이터를 바로 처리해야 할 때 필수적인 기능
여기서는 JSON mode(json_object)로 json format을 활용하지만, 이후에는 pydantic 라이브러리를 활용한 JSON Scheme 방식을 통해 명확한 json 응답 형식을 지정

### streaming (실시간 응답 처리)
stream=True 설정을 통해 활성화
서버는 SSE(Server-Sent Events) 프로토콜을 사용하여 응답을 끊지 않고 조각(Chunk) 단위로 지속적으로 전송
응답 객체는 제너레이터 형식으로, for 루프를 사용해 활용할 수 있음