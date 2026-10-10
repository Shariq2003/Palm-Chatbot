<div align="center">

# 🌴 PaLM Chatbot

**A conversational AI chatbot built with Angular Material on the front and an Express server talking to Google's PaLM 2 (`chat-bison-001`) model on the back.**

[![Live Demo](https://img.shields.io/badge/Live_Demo-palm--chatbot.vercel.app-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://palm-chatbot.vercel.app)

![Angular](https://img.shields.io/badge/Angular_16-DD0031?style=flat-square&logo=angular&logoColor=white)
![Angular Material](https://img.shields.io/badge/Angular_Material-3F51B5?style=flat-square&logo=angular&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=flat-square&logo=express&logoColor=white)
![Google PaLM](https://img.shields.io/badge/Google_PaLM_2-4285F4?style=flat-square&logo=google&logoColor=white)

[Features](#-features) · [Architecture](#%EF%B8%8F-architecture) · [Getting Started](#-getting-started) · [Customise](#-customise-the-bot) · [Structure](#-project-structure)

</div>

---

> [!NOTE]
> Google has since retired the PaLM API in favour of Gemini. The code here is kept as a reference for building an Angular ↔ LLM chat app; to run it today, swap the `DiscussServiceClient` call in `server/index.js` for the Gemini SDK (`@google/generative-ai`).

## ✨ Features

- 💬 **Chat-style UI** with user and bot message bubbles (Angular Material cards)
- ⏳ **Typing indicator** — a "…" placeholder while the model is thinking
- 🧠 **Conversation memory** — the server sends the full message history with every request
- 🎭 **Prompt context & few-shot examples** to steer the bot's tone
- ↩️ **Line-break pipe** so multi-paragraph answers render nicely
- 🧩 Standalone components (no NgModules)

## 🏗️ Architecture

```mermaid
sequenceDiagram
    actor U as User
    participant A as Angular app<br/>(ChatService)
    participant S as Express server<br/>:3000
    participant P as Google PaLM 2<br/>chat-bison-001
    U->>A: Type a message
    A->>A: Show user bubble + "…" placeholder
    A->>S: POST /api/chatbot { message }
    S->>S: Append to conversation history
    S->>P: generateMessage(context, examples, messages)
    P-->>S: Candidate reply
    S-->>A: { agent: "chatbot", message }
    A-->>U: Replace "…" with the reply
```

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- Angular CLI: `npm install -g @angular/cli`
- A Google AI API key

<details open>
<summary><b>1️⃣ Server (<code>server/</code>)</b></summary>

```bash
cd server
npm install
npm install cors          # used by index.js
echo "API_KEY=your_google_ai_key" > .env
npm start                 # http://localhost:3000
```

</details>

<details open>
<summary><b>2️⃣ Angular app (repo root)</b></summary>

```bash
npm install
ng serve                  # http://localhost:4200
```

The app calls `http://localhost:3000/api/chatbot` (see [`src/app/chat.service.ts`](src/app/chat.service.ts)).

</details>

## 🎨 Customise the bot

<details>
<summary><b>Change its personality</b></summary>

In [`server/index.js`](server/index.js):

```js
const CONTEXT = "Respond to all questions with atleast 300 words";

const EXAMPLES = [
  {
    input:  { content: "What is the capital of California?" },
    output: { content: "If the capital of California is what you seek,\nSacramento is where you ought to peek." },
  },
];
```

Edit `CONTEXT` for the system-style instruction and `EXAMPLES` for few-shot demonstrations of the tone you want.

</details>

## 📁 Project Structure

<details>
<summary><b>Expand tree</b></summary>

```text
Palm-Chatbot/
├── src/app/
│   ├── app.component.*          # chat UI
│   ├── chat.service.ts          # HTTP call to the server
│   ├── chat-content.interface.ts
│   └── line-break.pipe.ts
└── server/
    └── index.js                 # Express + PaLM DiscussServiceClient
```

</details>

---

<div align="center">

Built by **[Shariq](https://github.com/Shariq2003)** · [LinkedIn](https://www.linkedin.com/in/shariq-sd)

⭐ If you like this project, consider giving it a star!

</div>
