# Project 1. 자동화 도구 비교 구현

## 1. 프로젝트 개요

본 프로젝트에서는 동일한 이메일 관리 자동화 워크플로우를 **Make**와 **Zapier**에서 각각 구현하고 두 도구의 차이를 비교하였다.

Gmail로 새로운 이메일이 수신되면 AI가 이메일 내용을 요약하고 중요도를 분류한다. 이후 이메일 제목, 요약, 수신일, 중요도를 Notion 데이터베이스에 저장하며, 중요도가 높은 이메일은 Discord로 별도의 알림을 전송하도록 구성하였다.

---

## 2. 사용한 도구

### 자동화 도구

- Make
- Zapier

### 연동 서비스

- Gmail
- OpenAI
- Notion
- Discord

---

## 3. 자동화 워크플로우

두 도구에서 동일한 구조의 이메일 자동화 워크플로우를 구현하였다.

```text
Gmail에서 새로운 이메일 감지
        ↓
OpenAI가 이메일 내용 요약 및 중요도 분류
        ↓
중요도에 따른 조건 분기
   ├─ 중요도 높음
   │      ├─ Notion에 이메일 정보 저장
   │      └─ Discord 알림 전송
   │
   └─ 중요도 보통 또는 낮음
          └─ Notion에 이메일 정보 저장
```

### Trigger

- Gmail에 새로운 이메일이 수신되면 자동화가 시작된다.

### Action

- OpenAI가 이메일 내용을 요약한다.
- OpenAI가 이메일 중요도를 분류한다.
- 이메일 정보를 Notion에 저장한다.
- 중요도가 높은 경우 Discord 메시지를 전송한다.

### 조건 분기

- 중요도 `높음`: Notion 저장 및 Discord 알림 전송
- 중요도 `보통/낮음`: Notion 저장

---

## 4. 구현 과정 요약

### 4.1 Make

Make에서는 Gmail, OpenAI, JSON Parse, Router, Notion, Discord 모듈을 연결하여 자동화를 구현하였다.

먼저 Gmail 모듈에서 새로운 이메일을 감지하고 이메일 제목과 본문을 OpenAI 모듈로 전달하였다. OpenAI는 이메일 내용을 요약하고 중요도를 `높음`, `보통`, `낮음` 중 하나로 분류하였다.

OpenAI의 결과는 JSON Parse 모듈을 사용하여 요약과 중요도 데이터로 분리하였다. 이후 Router를 이용하여 중요도에 따라 실행 경로를 나누었다.

중요도가 높은 경우 이메일 정보를 Notion에 저장하고 Discord 알림을 전송하였다. 중요도가 보통이거나 낮은 경우에는 Discord 알림 없이 Notion에만 저장하도록 구성하였다.

### Make 구성

```text
Gmail
→ OpenAI
→ JSON Parse
→ Router
   ├─ 중요도 높음 → Notion → Discord
   └─ 중요도 보통/낮음 → Notion
```

---

### 4.2 Zapier

Zapier에서도 Gmail, ChatGPT, 조건 분기, Notion, Discord를 연결하여 Make와 동일한 자동화 구조를 구현하였다.

Gmail에서 새로운 이메일을 감지한 뒤 이메일 제목과 본문을 ChatGPT 단계로 전달하였다. ChatGPT는 이메일 내용을 요약하고 중요도를 분류하였다.

Zapier에서는 ChatGPT의 결과를 다음 단계의 데이터 필드로 직접 사용할 수 있었기 때문에 별도의 JSON Parse 단계를 추가하지 않았다.

이후 조건 분기 기능을 사용하여 중요도가 높은 이메일은 Notion에 저장한 뒤 Discord 알림을 전송하고, 중요도가 보통이거나 낮은 이메일은 Notion에만 저장하도록 구성하였다.

### Zapier 구성

```text
Gmail
→ ChatGPT
→ 조건 분기
   ├─ 중요도 높음 → Notion → Discord
   └─ 중요도 보통/낮음 → Notion
```

---

## 5. 워크플로우 구성 화면

### 5.1 Make 워크플로우

> Make의 전체 워크플로우가 보이는 구성 화면 캡처를 삽입한다.  
> Gmail, OpenAI, JSON Parse, Router, Notion, Discord 모듈과 분기 구조가 한 화면에 보이도록 한다.

```markdown
![Make 워크플로우 구성 화면](./images/make_workflow.png)
```

---

### 5.2 Zapier 워크플로우

> Zapier의 전체 워크플로우가 보이는 구성 화면 캡처를 삽입한다.  
> Gmail, ChatGPT, 조건 분기, Notion, Discord 단계가 보이도록 한다.

```markdown
![Zapier 워크플로우 구성 화면](./images/zapier_workflow.png)
```

---

## 6. 실행 결과

### 6.1 Make 실행 결과

> Make에서 자동화가 정상적으로 실행된 전체 결과 화면을 삽입한다.  
> 가능하면 중요도가 높은 이메일이 Notion에 저장되고 Discord 알림까지 전송된 결과가 함께 확인되도록 한다.

```markdown
![Make 실행 결과](./images/make_result.png)
```

---

### 6.2 Zapier 실행 결과

> Zapier에서 자동화가 정상적으로 실행된 전체 결과 화면을 삽입한다.  
> 가능하면 중요도가 높은 이메일이 Notion에 저장되고 Discord 알림까지 전송된 결과가 함께 확인되도록 한다.

```markdown
![Zapier 실행 결과](./images/zapier_result.png)
```

---

