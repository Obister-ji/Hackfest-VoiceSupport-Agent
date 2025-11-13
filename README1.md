# 🔥 AI Voice Support Assistant — Powered by FlameAI

A fully production-ready **real-time voice customer support system** powered by:

- **Agora Conversational AI** (ASR + voice pipeline)  
- **ElevenLabs TTS**  
- **Groq Llama 3.1 LLM**  
- **N8N Workflow Engine**  
- **Supabase Knowledge Base + Logging**

Unlike basic chatbots, this system uses a **multi-stage intelligent pipeline** with memory, knowledge search, strict input security, and real-time streaming. It is designed for **FlameAI’s high-quality customer support, sales automation, and booking flows**.

---

## 🧠 How This AI Support Agent Works (And Why It’s Better)

Most AI support bots simply forward text to an LLM.  
**This system does much more.**  

### 🔥 1. Real Voice Intelligence (Not Just Text-to-AI)
Our system handles:
- Live voice input  
- Real-time transcription  
- AI reasoning  
- High-quality TTS output  
- Continuous interactive streaming  

Using Agora's low-latency Real-Time Engagement stack.

---

### 🔒 2. Enterprise-Grade Input Security
Every message passes through a **full security validation layer**:

- HTML/script sanitization  
- JavaScript protocol removal  
- Malicious code detection (eval/exec/shell)  
- Spam detection (excess URLs, repeated characters)  
- Emoji-only or gibberish filtering  
- Message length validation  

Most bots **don’t have any of these** protections.

---

### 🧩 3. AI With Memory + Knowledge Base
The AI agent uses:

- **10-message sliding memory window**  
- **Supabase vector search knowledge base**  
- **FlameAI-trained system prompt**  
- **Groq Llama 3.1 8B Instant (super fast)**  

This makes responses:
- Consistent  
- Context-aware  
- Company-specific  
- Accurate  
- Fast  

---

### 🔁 4. Dual Response Formats (Automatic Detection)
The agent outputs based on caller preference:
- **SSE (Server-Sent Events)** for streaming voice apps  
- **OpenAI-compatible JSON** for API apps  

Automatically detected through:
- Query params (`?format=json`)  
- Request body (`stream: false`)  
- Custom headers  

Works across:
- Websites  
- Mobile apps  
- Voice bots  
- WhatsApp bots  
- IVR systems  

---

### 📊 5. Full Conversation Logging
Every interaction is logged with:
- User message  
- AI response  
- Session ID  
- Response time  
- Knowledge base used  
- Error details  
- Timestamp  

Stored in Supabase for analytics & debugging.

---

## 🧩 Full System Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   React Frontend│    │  Node.js Server │    │  Agora AI Agent │
│                 │    │                 │    │                 │
│  - Voice UI     │◄──►│  - Token Gen    │◄──►│  - ASR/TTS      │
│  - Audio Stream │    │  - Agent Mgmt   │    │  - LLM Pipeline │
│  - Status Ind.  │    │  - API Proxy    │    │  - Voice Synth  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                                ▼
                       ┌─────────────────────────────┐
                       │       N8N WORKFLOW          │
                       │                             │
                       │  - Input Validation         │
                       │  - Security Filtering       │
                       │  - AI Processing (Groq)     │
                       │  - Knowledge Base Search    │
                       │  - JSON / SSE Formatting    │
                       │  - Logging (Supabase)       │
                       └─────────────────────────────┘
```

---

## ⚙️ N8N Workflow — Detailed Breakdown

### 1. 📨 Input Reception
Webhook receives POST requests from Agora.

Extracts:
- user message  
- session ID  
- user ID  
- past messages  

---

### 2. 📄 Format Detection
Decides between:
- **SSE streaming** (default)  
- **OpenAI-style JSON** (via query/body/header)  

---

### 3. 🛡️ Security & Validation Layer  
Blocks:
- HTML/script tags  
- `javascript:` URLs  
- `eval`, `exec`, shells  
- Malformed inputs  
- Spam patterns  
- Too-long or too-short text  
- Emoji-only content  

If invalid → error branch triggers.

---

### 4. 🤖 AI Processing
Valid messages go to the Groq-hosted Llama 3.1:

- Custom FlameAI system prompt  
- Vector search through Supabase KB  
- 10-message session memory  
- Fast inference via Groq  

---

### 5. 🧱 Response Formatting
Switch node outputs:
- SSE streaming payload  
- OpenAI-compatible JSON  

---

### 6. 📤 Response Delivery + Logging
Sends reply to caller, then logs:

- Input  
- Output  
- Model used  
- Response time  
- Session ID  
- Errors  

Stored in `conversation_logs` table.

---

## 🗄️ Database Schema (Supabase)
`conversation_logs` table fields:

- session_id  
- user_message  
- ai_response  
- response_time_ms  
- knowledge_base_used  
- error  
- error_message  
- timestamp  

---

## 🏗️ Frontend Architecture

### Features:
- Real-time voice UI  
- Microphone visualizer  
- Connection status indicator  
- Live agent state display  
- Start/End conversation controls  

### Main Components:
- App.tsx  
- useAgora.ts  
- ControlButton.tsx  
- StatusIndicator.tsx  
- MicIcon.tsx  
- PhoneIcon.tsx  

---

## 🖥️ Backend (Node.js)

Endpoints:

```
POST /generate-token  
POST /start-agent  
POST /stop-agent  
```

Handles:
- Agora token generation  
- Agent session management  
- Proxy to N8N workflow  

---

## 🛠️ Setup & Installation

### 1. Clone Repo
```bash
git clone <repository-url>
cd ai-voice-support-assistant
npm install
```

### 2. Configure Environment

#### Update `constants.ts`
```ts
export const AGORA_APP_ID = 'your_agora_app_id';
export const BACKEND_URL = 'http://localhost:3000';
```

#### Update `server.js`
```js
const APP_ID = 'your_agora_app_id';
const APP_CERTIFICATE = 'your_app_certificate';
const CUSTOMER_ID = 'your_customer_id';
const CUSTOMER_SECRET = 'your_customer_secret';

const ELEVENLABS_API_KEY = 'sk_your_api_key';
const ELEVENLABS_VOICE_ID = 'your_voice_id';

const N8N_WEBHOOK_URL = 'https://your-n8n.com/webhook/voice-input';
```

---

## ▶️ Start the System

### Backend
```bash
node server.js
```

### Frontend
```bash
npm run dev
```

Open:  
**http://localhost:5173**

---

## 🎯 Usage

1. Click **Start Conversation**  
2. Grant microphone access  
3. Speak naturally  
4. Watch real-time status indicators  
5. End conversation anytime  

---

## 🔧 Troubleshooting

### Agent not speaking?
- Invalid ElevenLabs key or voice ID  

### "Failed to fetch" error?
- Backend not running  
- Incorrect BACKEND_URL  

### Agora issues?
- Wrong App ID/Certificate  

### N8N workflow issues?
- Wrong webhook URL  
- Execution errors  

---

## 🔒 Security Best Practices

- Never commit API keys  
- Use HTTPS  
- Apply rate limiting  
- Validate all inputs  
- Secure CORS  

---

## 🚀 Deployment

### Production Deployment
1. Move secrets to environment variables  
2. Build frontend:
   ```bash
   npm run build
   ```
3. Serve static files with Nginx  
4. Deploy backend + N8N  

---

## 🤝 Contributing

- Fork  
- Create branch  
- Submit PR  

---

## 📄 License  
MIT License
