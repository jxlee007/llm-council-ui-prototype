# LLM Council for Students![LLM Council Platform Header](header.jpg)

LLM Council for Students is a cross-platform software ecosystem (Android native, iOS/Mobile, and Web) designed to orchestrate multi-model LLM consensus operations. 
The application takes a user's prompt and processes it through a secure 3-stage asynchronous reasoning loop, allowing multiple target models to analyze, review, and synthesize optimal outputs entirely decoupled from local state.
> **💡 System Integration & Collaboration Note:**
> > While the async computation layers on the FastAPI backend were adapted and customized from a [Andrej Karpathy's idea](https://x.com/karpathy/status/1990577951671509438) foundation, the entire cross-platform frontend stack, reactive client state architecture, data streaming ingestion loop, secure cred pipeline, and multi-device persistence mappings were designed and implemented independently.
---
## 🏗️ Architectural Blueprint
The platform treats state sync and heavy LLM computation as entirely isolated modules to guarantee high throughput, offline data resilience, and fluid user interactions.

```
            ┌─────────────────────────────────────────────────┐
            │      React Native Mobile / Jetpack Compose      │
            └──────┬───────────────────────────────────┬──────┘
                   │                                   │
       (Subscribes to State Streams) │ (Triggers runCouncil Action)
                   ▼                                   ▼
            ┌──────────────────────────────────────────────┐
            │          Convex Cloud Stateful Sync          │
            └──────────────────────┬───────────────────────┘
                                   │
            (Asynchronous SSE Data │ (Node.js AES-256-GCM
                Stream Increments) │ cred Sandbox)
                                   ▼
            ┌──────────────────────────────────────────────┐
            │           FastAPI Microservice Engine        │
            └──────────────────────┬───────────────────────┘
                                   │
                                   ▼
            ┌──────────────────────────────────────────────┐
            │            OpenRouter API Gateway            │
            └──────────────────────────────────────────────┘
```

### The 3-Stage Deliberation Protocol [1349, 1425-1426]
1. **Stage 1 (Reason):** The foundational query is dispatched to up to 4 selected council models concurrently [1349, 1418, 1425-1426]. Raw responses are ingested and loaded dynamically into independent structural views for user inspection.
2. **Stage 2 (Compare):** Responses are stripped of metadata, anonymized (e.g., *Response A, Response B*), and cross-routed back to matching council members. Models evaluate and score peers blindly using a strict mathematical index to prevent model brand favoritism.
3. **Stage 3 (Result):** A designated Chairman model ingests the comprehensive transcript of individual evaluations along with the weighted aggregate peer metrics, generating a final, optimized, and synthesized response [1351, 1415-1417].

---

## 🛠️ Core Engineering Highlights

### 1. Cryptographic BYOK Sandboxing (AES-256-GCM)
To support Bring Your Own Key (BYOK) configurations safely without exposing plaintext user tokens to database logs or tracking hooks, creds undergo runtime encryption inside an isolated Node.js environment [1413-1414, 1424-1426].
* Generates custom, random 96-bit initialization vectors (IV) per mutation [1413-1414, 1424-1426].
* Enforces structural integrity using cryptographically secure authorization tags [1413-1414, 1424-1426].
* Key blocks use explicit cloud database hooks (`userActions.ts`), isolating operational memory blocks [1331-1333, 1413-1414, 1424-1426].

### 2. Asynchronous State Streaming Pipelines (SSE)
The system leverages Server-Sent Events (SSE) to map real-time tokens dynamically into client-side view states without forcing expensive network re-fetches [1335-1336, 1344-1346, 1413, 1415-1416].
* Decouples long-running LLM calculations from the client state using a streaming microservice layer [1335-1336, 1344-1346].
* Emits highly granular, progressive mutations (`stage1_complete`, `stage2_complete`, `stage3_complete`) that update database records step-by-step [1335-1336, 1344-1346].
* Uses reactive Zustand store maps on the web client to achieve uniform $O(1)$ lookups during rendering updates [1335-1336, 1344-1346].

### 3. Responsive UI Engineering & Layout Control (Jetpack Compose & NativeWind)
To eliminate the layout stutter common during high-frequency textual streaming updates, custom native layout measurement interceptors were designed [1320-1325, 1344-1346, 1414-1415, 1419-1422].
* Standardized UI layout using declarative, highly modern UI architectures (Jetpack Compose on Android native; Tailwind/NativeWind on React Native) [1320-1325, 1344-1346, 1414-1415, 1419-1422].
* Implemented strict thread-local primitive checks inside rendering loops (`useMeasuredHeight.ts`) to completely bypass redundant Composable updates and layout recalculations [1320-1325, 1344-1346].
* Integrated system-local storage sandboxes (`data_extraction_rules.xml`) to safeguard critical device configurations against unintentional data leaks during automated operating system cloud backups [1316-1317].

---

## 🚀 Tech Stack Matrix

* **Mobile Architecture:** React Native (Expo SDK 54 ecosystem), NativeWind (Tailwind CSS engine), Jetpack Compose (Android native layers) [1320-1325, 1329-1330, 1344-1346, 1420-1422].
* **State & Sync:** Convex Cloud, Zustand Reactive State Machine, React Navigation State Engine [1335-1336, 1344-1346].
* **Server Infrastructure:** FastAPI, Asynchronous HTTPX, Server-Sent Events (SSE) Protocol [1335-1336, 1344-1346].
* **Security & Testing:** AES-256-GCM Cryptography, Vitest, MockK Concurrency Test Harnesses [1335-1336, 1344-1346].

---
