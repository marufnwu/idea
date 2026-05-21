# Ultimate IELTS Mock Test App - Complete Architecture Documentation (Expanded)

## 📋 **Table of Contents**

1. [System Overview](#system-overview)
2. [High-Level Architecture](#high-level-architecture)
3. [Complete User Journey Flows](#complete-user-journey-flows)
4. [AI Integration Architecture](#ai-integration-architecture)
5. [Data Architecture](#data-architecture)
6. [Microservices Breakdown](#microservices-breakdown)
7. [Security Architecture](#security-architecture)
8. [Scalability & Performance](#scalability--performance)
9. [Cost Management](#cost-management)
10. [Deployment Architecture](#deployment-architecture)
11. [Monitoring & Analytics](#monitoring--analytics)
12. [Business Logic](#business-logic)
13. [Pre-Generated Question Bank System](#pre-generated-question-bank-system)

---

## 🎯 **System Overview**

### **Platform Vision**
A comprehensive AI-powered IELTS preparation platform that:
- **Generates** authentic test questions using AI (on-demand + pre-generated bank)
- **Evaluates** all four modules (Listening, Reading, Writing, Speaking) using AI
- **Provides** detailed feedback with band scores
- **Personalizes** learning paths based on performance
- **Tracks** progress over time
- **Predicts** likely band scores for actual IELTS test
- **Offers** instant tests via pre-generated question bank (zero wait time)

### **Core Capabilities**

| Feature | Description |
|---------|-------------|
| **Dynamic Question Generation** | AI creates unique questions per user request |
| **Pre-Generated Question Bank** | Instant access to curated question sets |
| **Automated Evaluation** | AI grades all 4 modules with detailed feedback |
| **Real-Time Feedback** | Immediate results and performance insights |
| **Personalized Study Plans** | AI-generated learning paths based on weaknesses |
| **Progress Prediction** | ML models predict likely exam scores |
| **Adaptive Difficulty** | Questions adjust to user performance |

**Test Modules:**
- Listening (AI-generated audio + questions)
- Reading (AI-generated passages + questions)
- Writing (AI-generated topics + evaluation)
- Speaking (AI-generated questions + evaluation)

**Platform Types:**
- Web Application (Desktop) - Next.js 14
- Mobile Apps (iOS & Android) - React Native + Expo
- Admin Dashboard - React + Material UI

**Question Generation Modes:**
- **On-Demand** - Personalized, per-user generation (30-75 seconds)
- **Pre-Generated** - Instant access from shared bank (< 500ms)
- **Hybrid** - Smart routing between both modes

---

## 🏗️ **High-Level Architecture**

### **Complete System Architecture**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              USER LAYER                                      │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐         │
│  │   Web App        │  │   Mobile App     │  │   Admin Panel    │         │
│  │   (Next.js 14)   │  │(React Native +   │  │   (React +       │         │
│  │                  │  │ Expo)            │  │    Material UI)  │         │
│  │  Features:       │  │  Features:       │  │  Features:       │         │
│  │  • Dashboard     │  │  • All web       │  │  • User mgmt     │         │
│  │  • Test Taking   │  │    features      │  │  • Question bank │         │
│  │  • Results       │  │  • Push notif    │  │  • Analytics     │         │
│  │  • Analytics     │  │  • Offline mode  │  │  • AI monitoring │         │
│  │  • Study Plans   │  │                  │  │                  │         │
│  └──────────────────┘  └──────────────────┘  └──────────────────┘         │
│           ↓                     ↓                      ↓                    │
└─────────────────────────────────────────────────────────────────────────────┘
                                   ↓ HTTPS/WSS
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CDN & EDGE LAYER                                     │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  Cloudflare CDN                                                      │   │
│  │  • Static asset caching                                              │   │
│  │  • DDoS protection                                                   │   │
│  │  • SSL/TLS termination                                               │   │
│  │  • Geographic routing                                                │   │
│  │  • Audio file caching (listening tests)                              │   │
│  │  • Pre-generated content edge caching                                │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
                                   ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                      API GATEWAY LAYER                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  Load Balancer (AWS ALB / Nginx)                                     │   │
│  │  • Health checks                                                     │   │
│  │  • Request distribution                                              │   │
│  │  • SSL termination                                                   │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                   ↓                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  API Gateway (Kong / AWS API Gateway)                                │   │
│  │  • Rate limiting (per user/IP)                                       │   │
│  │  • Authentication validation (JWT)                                   │   │
│  │  • Request/Response transformation                                   │   │
│  │  • API versioning (v1, v2)                                           │   │
│  │  • Request logging & metrics                                         │   │
│  │  • CORS handling                                                     │   │
│  │  • WebSocket upgrade handling                                        │   │
│  │  • Cache headers for pre-generated content                           │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
                                   ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                    MICROSERVICES LAYER                                       │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │                    Service Mesh (Istio / Linkerd)                     │  │
│  │              • Service discovery • Load balancing                     │  │
│  │              • Circuit breaking  • Observability                      │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐                  │
│  │ Auth Service  │  │ User Service  │  │ Test Service  │                  │
│  │ (Node.js)     │  │ (Node.js)     │  │ (Node.js)     │                  │
│  │ Port: 3001    │  │ Port: 3002    │  │ Port: 3003    │                  │
│  │ DB: PostgreSQL│  │ DB: PostgreSQL│  │ DB: PostgreSQL│                  │
│  └───────────────┘  └───────────────┘  └───────────────┘                  │
│                                                                              │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐                  │
│  │Question Gen   │  │ Evaluation    │  │ Audio Process │                  │
│  │Service        │  │ Service       │  │ Service       │                  │
│  │(Python/Node)  │  │(Python/Node)  │  │ (Python)      │                  │
│  │ Port: 3004    │  │ Port: 3005    │  │ Port: 3006    │                  │
│  │ DB: MongoDB   │  │ DB: PostgreSQL│  │ DB: PostgreSQL│                  │
│  └───────────────┘  └───────────────┘  └───────────────┘                  │
│                                                                              │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐                  │
│  │ Payment       │  │ Analytics     │  │ Notification  │                  │
│  │ Service       │  │ Service       │  │ Service       │                  │
│  │(Node.js)      │  │(Python/Node)  │  │ (Node.js)     │                  │
│  │ Port: 3007    │  │ Port: 3008    │  │ Port: 3009    │                  │
│  │ DB: PostgreSQL│  │ DB: PostgreSQL│  │ DB: Redis     │                  │
│  └───────────────┘  └───────────────┘  └───────────────┘                  │
│                                                                              │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐                  │
│  │ Report Gen    │  │AI Orchestration│ │ Media Service │                  │
│  │ Service       │  │ Service        │  │ (Node.js)     │                  │
│  │(Node.js)      │  │ (Python)       │  │               │                  │
│  │ Port: 3010    │  │ Port: 3011     │  │ Port: 3012    │                  │
│  │ DB: PostgreSQL│  │ DB: MongoDB    │  │ DB: S3        │                  │
│  └───────────────┘  └───────────────┘  └───────────────┘                  │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │                    Question Bank Service (NEW)                       │  │
│  │                        (Node.js/Python)                              │  │
│  │  • Pre-generated question management                                 │  │
│  │  • Question pool rotation                                            │  │
│  │  • Difficulty balancing                                              │  │
│  │  • Usage tracking & analytics                                        │  │
│  │  • Quality scoring & deprecation                                     │  │
│  │  • Smart allocation to users                                         │  │
│  │  Port: 3013                                                          │  │
│  │  DB: MongoDB (questions) + PostgreSQL (metadata)                     │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
                                   ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                    MESSAGE QUEUE LAYER                                       │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  RabbitMQ / AWS SQS / Redis Queue                                     │  │
│  │  Queues:                                                              │  │
│  │  1. question-generation-queue        Priority Levels:                │  │
│  │     - Listening questions            • Critical (0-5 sec)            │  │
│  │     - Reading passages               • High (5-30 sec)               │  │
│  │     - Writing topics                 • Medium (30-60 sec)            │  │
│  │     - Speaking questions             • Low (1-5 min)                 │  │
│  │                                      • Batch (async)                 │  │
│  │  2. evaluation-queue                 Dead Letter Queue:              │  │
│  │     - Writing evaluation (HIGH)      • Failed jobs                   │  │
│  │     - Speaking evaluation (HIGH)     • Retry mechanism               │  │
│  │     - Reading evaluation (MEDIUM)    • Alert on failure              │  │
│  │  3. audio-processing-queue                                            │  │
│  │     - Audio upload (CRITICAL)        Features:                       │  │
│  │     - STT processing (HIGH)          • Delayed jobs                  │  │
│  │     - Audio quality check (MEDIUM)   • Job scheduling                │  │
│  │  4. notification-queue               • Progress tracking             │  │
│  │     - Email (MEDIUM)                 • Concurrency control           │  │
│  │     - Push notification (HIGH)                                        │  │
│  │     - SMS (HIGH)                                                      │  │
│  │  5. report-generation-queue                                           │  │
│  │     - PDF reports (LOW)                                               │  │
│  │     - Analytics reports (BATCH)                                       │  │
│  │  6. ai-request-queue                                                  │  │
│  │     - Batched AI calls (BATCH)                                        │  │
│  │     - Cost optimization                                               │  │
│  │  7. pre-generation-queue (NEW)                                        │  │
│  │     - Batch question generation                                       │  │
│  │     - Pool replenishment                                              │  │
│  │     - Quality validation                                              │  │
│  │     - Low priority (batch)                                            │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
                                   ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                     AI INTEGRATION LAYER                                     │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │              AI Orchestration Service (Python FastAPI)                │  │
│  │  Core Functions:                                                      │  │
│  │  1. Model Selection Engine                                            │  │
│  │     • Routing logic based on task type                               │  │
│  │     • Cost optimization                                               │  │
│  │     • Quality requirements                                            │  │
│  │     • Load balancing across providers                                │  │
│  │     • Pre-generation vs on-demand routing                            │  │
│  │  2. Prompt Management System                                          │  │
│  │     • Template storage (MongoDB)                                     │  │
│  │     • Version control                                                 │  │
│  │     • A/B testing framework                                           │  │
│  │     • Performance tracking                                            │  │
│  │  3. Response Processing                                               │  │
│  │     • JSON parsing & validation                                      │  │
│  │     • Error handling                                                  │  │
│  │     • Retry logic                                                     │  │
│  │     • Response caching                                                │  │
│  │  4. Cost Tracking                                                     │  │
│  │     • Token counting                                                  │  │
│  │     • Cost calculation                                                │  │
│  │     • Budget alerts                                                   │  │
│  │     • Usage analytics                                                 │  │
│  │     • Pre-generation cost amortization                               │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                   ↓                                          │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │                    AI Provider Integrations                           │  │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │  │
│  │  │   OpenAI     │  │  Anthropic   │  │   Google     │               │  │
│  │  │   GPT-4o     │  │   Claude     │  │   Gemini     │               │  │
│  │  │              │  │  Sonnet 4.5  │  │   Pro 1.5    │               │  │
│  │  │ Use Cases:   │  │              │  │              │               │  │
│  │  │ • Speaking Q │  │ Use Cases:   │  │ Use Cases:   │               │  │
│  │  │ • Speaking   │  │ • Writing    │  │ • Listening Q│               │  │
│  │  │   Evaluation │  │   Evaluation │  │ • Reading Q  │               │  │
│  │  │ • General Q  │  │ • Reading    │  │ • Cost-eff   │               │  │
│  │  │   Generation │  │   Evaluation │  │   tasks      │               │  │
│  │  │              │  │ • Deep       │  │ • Multimodal │               │  │
│  │  │ Cost: $$     │  │   Analysis   │  │              │               │  │
│  │  │              │  │              │  │ Cost: $      │               │  │
│  │  │              │  │ Cost: $$$    │  │              │               │  │
│  │  └──────────────┘  └──────────────┘  └──────────────┘               │  │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │  │
│  │  │   Whisper    │  │  ElevenLabs  │  │   Stable     │               │  │
│  │  │     API      │  │     TTS      │  │  Diffusion   │               │  │
│  │  │   (OpenAI)   │  │              │  │              │               │  │
│  │  │              │  │              │  │              │               │  │
│  │  │ Use Cases:   │  │ Use Cases:   │  │ Use Cases:   │               │  │
│  │  │ • Speech to  │  │ • Listening  │  │ • Chart      │               │  │
│  │  │   Text       │  │   audio gen  │  │   generation │               │  │
│  │  │ • Transcript │  │ • Sample     │  │ • Diagram    │               │  │
│  │  │   generation │  │   answers    │  │   creation   │               │  │
│  │  │ • Multi-lang │  │ • High-qual  │  │ • Visual     │               │  │
│  │  │              │  │   voices     │  │   content    │               │  │
│  │  │              │  │              │  │              │               │  │
│  │  │ Cost: $      │  │ Cost: $$     │  │ Cost: $      │               │  │
│  │  └──────────────┘  └──────────────┘  └──────────────┘               │  │
│  │  Fallback Strategy: Primary → Secondary → Tertiary → Error         │  │
│  │  Rate Limiting: Per-provider limits, Intelligent queuing,          │  │
│  │                 Circuit breaker pattern                              │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
                                   ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                       DATA LAYER                                             │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐         │
│  │   PostgreSQL     │  │     MongoDB      │  │      Redis       │         │
│  │   (Primary DB)   │  │  (Document DB)   │  │     (Cache)      │         │
│  │                  │  │                  │  │                  │         │
│  │ Tables:          │  │ Collections:     │  │ Use Cases:       │         │
│  │ • users          │  │ • questions      │  │ • Session store  │         │
│  │ • test_sessions  │  │ • prompts        │  │ • Rate limiting  │         │
│  │ • submissions    │  │ • templates      │  │ • Real-time data │         │
│  │ • evaluations    │  │ • ai_logs        │  │ • Job queues     │         │
│  │ • payments       │  │ • question_meta  │  │ • Leaderboards   │         │
│  │ • subscriptions  │  │ • pregen_pool    │  │ • Active users   │         │
│  │ • progress       │  │ • question_sets  │  │ • Test drafts    │         │
│  │ • analytics      │  │                  │  │ • Pre-gen cache  │         │
│  │ • question_bank  │  │ Why MongoDB:     │  │                  │         │
│  │   _metadata      │  │ • Flexible       │  │ TTL: 24 hours    │         │
│  │                  │  │   schema         │  │ for temp data    │         │
│  │ Replication:     │  │ • Fast queries   │  │                  │         │
│  │ Master-Slave     │  │ • JSON storage   │  │ Cluster: 3 nodes │         │
│  │                  │  │                  │  │                  │         │
│  │ Backup: Daily    │  │ Indexes:         │  └──────────────────┘         │
│  └──────────────────┘  │ • tags           │                                │
│                        │ • difficulty     │                                │
│                        │ • test_type      │                                │
│                        │ • pregen_status  │                                │
│                        └──────────────────┘                                │
│                                                                              │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐         │
│  │  S3 / Cloud      │  │  Elasticsearch   │  │   Vector DB      │         │
│  │  Storage         │  │                  │  │   (Pinecone)     │         │
│  │                  │  │                  │  │                  │         │
│  │ Buckets:         │  │ Indexes:         │  │ Use Cases:       │         │
│  │ • audio-files    │  │ • questions      │  │ • Semantic       │         │
│  │   - Speaking     │  │ • user-content   │  │   question       │         │
│  │   - Listening    │  │ • feedback       │  │   search         │         │
│  │   TTL: 90 days   │  │                  │  │ • Similar        │         │
│  │                  │  │ Use Cases:       │  │   question       │         │
│  │ • reports        │  │ • Full-text      │  │   matching       │         │
│  │   - PDF files    │  │   search         │  │ • Content        │         │
│  │   TTL: 1 year    │  │ • Question       │  │   deduplication  │         │
│  │                  │  │   discovery      │  │                  │         │
│  │ • media          │  │ • Analytics      │  │ Dimensions:      │         │
│  │   - Images       │  │   queries        │  │ 1536 (OpenAI)    │         │
│  │   - Charts       │  │ • Autocomplete   │  │                  │         │
│  │   Permanent      │  │                  │  │ Cost: $70/month  │         │
│  │                  │  │ Cluster: 3 nodes │  │                  │         │
│  │ • pregen-audio   │  └──────────────────┘  └──────────────────┘         │
│  │   - Pre-gen      │                                                       │
│  │     listening    │                                                       │
│  │   Permanent      │                                                       │
│  │                  │                                                       │
│  │ CDN: CloudFront  │                                                       │
│  │                  │                                                       │
│  │ Lifecycle Rules: │                                                       │
│  │ • Hot → Warm     │                                                       │
│  │   (30 days)      │                                                       │
│  │ • Warm → Cold    │                                                       │
│  │   (90 days)      │                                                       │
│  └──────────────────┘                                                       │
└─────────────────────────────────────────────────────────────────────────────┘
                                   ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                  MONITORING & OBSERVABILITY LAYER                            │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  Monitoring Stack                                                     │  │
│  │  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐         │  │
│  │  │   DataDog /    │  │     Sentry     │  │  ELK Stack     │         │  │
│  │  │   New Relic    │  │                │  │                │         │  │
│  │  │                │  │                │  │ • Elasticsearch│         │  │
│  │  │ Metrics:       │  │ Features:      │  │ • Logstash     │         │  │
│  │  │ • Latency      │  │ • Error track  │  │ • Kibana       │         │  │
│  │  │ • Throughput   │  │ • Stack traces │  │                │         │  │
│  │  │ • Error rate   │  │ • Alerts       │  │ Features:      │         │  │
│  │  │ • CPU/Memory   │  │ • User context │  │ • Log aggreg   │         │  │
│  │  │ • DB queries   │  │ • Breadcrumbs  │  │ • Search       │         │  │
│  │  │ • API calls    │  │                │  │ • Dashboards   │         │  │
│  │  └────────────────┘  └────────────────┘  └────────────────┘         │  │
│  │  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐         │  │
│  │  │  Prometheus +  │  │  CloudWatch    │  │  AI Cost       │         │  │
│  │  │   Grafana      │  │   (AWS)        │  │  Dashboard     │         │  │
│  │  │                │  │                │  │                │         │  │
│  │  │ Metrics:       │  │ Monitors:      │  │ Tracks:        │         │  │
│  │  │ • Custom       │  │ • Infrastructure│ │ • Token usage  │         │  │
│  │  │   metrics      │  │ • Services     │  │ • Cost/request │         │  │
│  │  │ • Business KPI │  │ • Alarms       │  │ • Provider     │         │  │
│  │  │ • Real-time    │  │ • Auto-scaling │  │   breakdown    │         │  │
│  │  │   dashboards   │  │                │  │ • Budget alert │         │  │
│  │  └────────────────┘  └────────────────┘  └────────────────┘         │  │
│  │  Alerts:                                                               │  │
│  │  • Slack/Discord integration                                          │  │
│  │  • PagerDuty for critical issues                                      │  │
│  │  • Email for warnings                                                 │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
```

---
## 🔄 **Complete User Journey Flows**

### **FLOW 1: User Onboarding & Initial Setup**

```
┌─────────────────────────────────────────────────────────────────┐
│                    USER ONBOARDING FLOW                          │
└─────────────────────────────────────────────────────────────────┘

START: User visits platform (web/mobile)
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Landing Page                                                 │
│ • Hero section with value proposition                       │
│ • Feature highlights                                         │
│ • Sample test preview                                        │
│ • Testimonials                                               │
│ • Pricing plans                                              │
│ • CTA: "Start Free Trial"                                   │
└─────────────────────────────────────────────────────────────┘
    ↓ User clicks "Start Free Trial"
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Registration Page                                            │
│                                                              │
│ Option 1: Email Registration                                │
│   • Full name                                               │
│   • Email address                                           │
│   • Password (min 8 chars, complexity rules)               │
│   • Confirm password                                        │
│   • Terms & Conditions checkbox                            │
│   ↓                                                          │
│   Backend: Auth Service                                     │
│   • Email validation                                        │
│   • Password hashing (bcrypt)                              │
│   • Check for existing user                                │
│   • Create user record in PostgreSQL                       │
│   • Send verification email                                │
│   • Generate JWT tokens (access + refresh)                 │
│                                                              │
│ Option 2: Social OAuth                                      │
│   • Google Sign-In                                          │
│   • Facebook Login                                          │
│   • Apple Sign-In                                           │
│   ↓                                                          │
│   • OAuth flow handled by Auth Service                     │
│   • Retrieve user info from provider                       │
│   • Create/update user in database                         │
│   • Generate JWT tokens                                     │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Email Verification (if email registration)                  │
│ • User receives email with verification link               │
│ • Link expires in 24 hours                                 │
│ • Click link → Backend verifies token                      │
│ • Mark email as verified in database                       │
│ • Redirect to profile setup                                │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Profile Setup - Step 1: Basic Information                   │
│                                                              │
│ Form Fields:                                                │
│ • Profile picture (optional)                                │
│ • Country                                                   │
│ • Date of birth                                             │
│ • Native language                                           │
│ • Current English proficiency (dropdown)                   │
│   - Beginner                                                │
│   - Intermediate                                            │
│   - Advanced                                                │
│                                                              │
│ Backend: User Service                                       │
│ • Store in PostgreSQL users table                          │
│ • Upload profile picture to S3 (if provided)               │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Profile Setup - Step 2: IELTS Goals                         │
│                                                              │
│ Form Fields:                                                │
│ • Test type:                                                │
│   ○ Academic                                                │
│   ○ General Training                                        │
│                                                              │
│ • Target band score (slider 5.0 - 9.0)                     │
│   Visual: Shows what band means (e.g., 7.0 = "Good user")  │
│                                                              │
│ • Target test date (calendar picker)                       │
│   - Calculates days remaining                              │
│   - Shows recommended study schedule                        │
│                                                              │
│ • Reason for taking IELTS:                                 │
│   □ University admission                                    │
│   □ Immigration                                             │
│   □ Job requirements                                        │
│   □ Personal development                                    │
│   □ Other: _________                                        │
│                                                              │
│ Backend: User Service                                       │
│ • Store goals in PostgreSQL                                │
│ • Trigger analytics calculation                            │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Profile Setup - Step 3: Weak Areas (Optional)              │
│                                                              │
│ "Which areas would you like to focus on?"                  │
│                                                              │
│ Listening:                                                  │
│ □ Understanding accents                                     │
│ □ Note-taking skills                                        │
│ □ Following conversations                                   │
│ □ Academic lectures                                         │
│                                                              │
│ Reading:                                                    │
│ □ Time management                                           │
│ □ Skimming & scanning                                       │
│ □ True/False/Not Given questions                           │
│ □ Academic vocabulary                                       │
│                                                              │
│ Writing:                                                    │
│ □ Task 1 data description                                   │
│ □ Task 2 essay structure                                    │
│ □ Grammar accuracy                                          │
│ □ Vocabulary range                                          │
│                                                              │
│ Speaking:                                                   │
│ □ Pronunciation                                             │
│ □ Fluency                                                   │
│ □ Grammar while speaking                                    │
│ □ Part 3 abstract topics                                   │
│                                                              │
│ Backend: User Service                                       │
│ • Store preferences in database                            │
│ • Will be used for personalized content                    │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Optional: Initial Diagnostic Test                           │
│                                                              │
│ "Take a quick 15-minute diagnostic test to assess your     │
│  current level?"                                            │
│                                                              │
│ [Skip for now] [Start Diagnostic]                          │
│                                                              │
│ If user clicks "Start Diagnostic":                         │
│   ↓                                                          │
│   Backend: Test Service                                     │
│   • Pull from pre-generated question bank (instant)        │
│   • Generate short test (10 questions per module)          │
│   • Mixed difficulty levels                                │
│   ↓                                                          │
│   User takes quick test:                                    │
│   • 3 reading questions (5 min)                            │
│   • 3 listening questions (5 min)                          │
│   • 1 writing task (3 min - just outline)                  │
│   • 2 speaking questions (2 min)                           │
│   ↓                                                          │
│   Backend: Evaluation Service                               │
│   • Auto-grade reading & listening                         │
│   • AI quickly evaluates writing & speaking                │
│   • Calculate estimated current band score                 │
│   • Identify specific weaknesses                           │
│   ↓                                                          │
│   Results shown to user:                                    │
│   • Current estimated band: 6.5                            │
│   • Module breakdown                                        │
│   • Gap analysis (current vs target)                       │
│   • Recommended focus areas                                │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Study Plan Generation                                        │
│                                                              │
│ Backend: Analytics Service                                  │
│   ↓                                                          │
│   Input data:                                               │
│   • Current level (from diagnostic or self-assessment)     │
│   • Target band score                                       │
│   • Target date                                             │
│   • Weak areas                                              │
│   • Available study time per day                           │
│   ↓                                                          │
│   AI Orchestration Service called                          │
│   • Uses GPT-4o with structured prompt                     │
│   ↓                                                          │
│   Generates personalized study plan:                        │
│   {                                                          │
│     "duration_weeks": 12,                                   │
│     "weekly_schedule": {                                    │
│       "monday": {                                           │
│         "focus": "Listening",                               │
│         "tasks": [                                          │
│           "1 full listening test",                          │
│           "Review mistakes",                                │
│           "Practice note-taking"                            │
│         ],                                                   │
│         "time_required": "90 minutes"                       │
│       },                                                     │
│       // ... other days                                     │
│     },                                                       │
│     "milestones": [                                         │
│       {                                                      │
│         "week": 4,                                          │
│         "goal": "Achieve 7.0 in Reading",                  │
│         "action": "Take full reading test"                 │
│       }                                                      │
│     ],                                                       │
│     "recommended_resources": [...],                         │
│     "practice_frequency": {                                 │
│       "full_tests": "2 per week",                          │
│       "module_practice": "daily"                           │
│     }                                                        │
│   }                                                          │
│   ↓                                                          │
│   Store plan in PostgreSQL                                  │
│   Cache in Redis for quick access                          │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Subscription Selection                                       │
│                                                              │
│ Plans:                                                       │
│                                                              │
│ ┌─────────────────────────────────────────────────────┐    │
│ │ FREE PLAN                                            │    │
│ │ $0/month                                             │    │
│ │ • 3 full tests per month                            │    │
│ │ • Basic evaluation                                   │    │
│ │ • Band scores only                                   │    │
│ │ • Community support                                  │    │
│ │ • Pre-generated tests only                          │    │
│ │ [Current Plan]                                       │    │
│ └─────────────────────────────────────────────────────┘    │
│                                                              │
│ ┌─────────────────────────────────────────────────────┐    │
│ │ BASIC PLAN ⭐ Most Popular                          │    │
│ │ $19.99/month or $199/year (save 17%)                │    │
│ │ • 20 full tests per month                           │    │
│ │ • Detailed AI feedback                              │    │
│ │ • All 4 module scores                               │    │
│ │ • Progress tracking                                  │    │
│ │ • Email support                                      │    │
│ │ • Downloadable reports                              │    │
│ │ • Mix of pre-gen & on-demand                        │    │
│ │ [Select Plan]                                        │    │
│ └─────────────────────────────────────────────────────┘    │
│                                                              │
│ ┌─────────────────────────────────────────────────────┐    │
│ │ PREMIUM PLAN 👑                                      │    │
│ │ $39.99/month or $399/year (save 17%)                │    │
│ │ • Unlimited tests                                    │    │
│ │ • Advanced AI analysis                              │    │
│ │ • Personalized study plans                          │    │
│ │ • 1-on-1 tutor sessions (2/month)                   │    │
│ │ • Priority support (24/7)                           │    │
│ │ • Mock interview practice                           │    │
│ │ • Guaranteed band improvement                       │    │
│ │ • On-demand generation priority                     │    │
│ │ [Select Plan]                                        │    │
│ └─────────────────────────────────────────────────────┘    │
│                                                              │
│ If user selects paid plan:                                  │
│   ↓                                                          │
│   Redirect to Payment Page                                  │
│   • Stripe Checkout integration                            │
│   • Support: Credit/Debit cards, PayPal, Apple Pay        │
│   ↓                                                          │
│   Backend: Payment Service                                  │
│   • Process payment via Stripe API                         │
│   • Create subscription in database                        │
│   • Send confirmation email                                │
│   • Activate premium features                              │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Welcome Dashboard                                            │
│                                                              │
│ • Welcome message with personalized greeting               │
│ • Current band score (if diagnostic taken)                 │
│ • Target band score                                         │
│ • Days until test date                                      │
│ • Study plan overview                                       │
│ • Quick action buttons:                                     │
│   [Start Full Test] [Practice Module] [View Progress]      │
│                                                              │
│ • Today's recommended activities                            │
│ • Upcoming milestones                                       │
│ • Achievement badges (gamification)                         │
│                                                              │
│ Backend actions:                                            │
│ • Log user session                                          │
│ • Track onboarding completion                              │
│ • Send welcome email                                        │
│ • Start analytics tracking                                  │
└─────────────────────────────────────────────────────────────┘
    ↓
END: User ready to start practicing
```

---

### **FLOW 2: Test Selection - Pre-Generated vs On-Demand**

```
┌─────────────────────────────────────────────────────────────────┐
│              TEST SELECTION & ALLOCATION FLOW                    │
└─────────────────────────────────────────────────────────────────┘

User clicks "Start New Test"
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Test Configuration Screen                                    │
│                                                              │
│ "Configure Your Test"                                       │
│                                                              │
│ Test Type:                                                  │
│ ○ Full IELTS Test (2h 45min)                               │
│   - All 4 modules                                           │
│   - Authentic test experience                              │
│                                                              │
│ ○ Module Practice                                           │
│   □ Listening only (30 min)                                │
│   □ Reading only (60 min)                                  │
│   □ Writing only (60 min)                                  │
│   □ Speaking only (15 min)                                 │
│                                                              │
│ Test Format:                                                │
│ ○ Academic                                                  │
│ ○ General Training                                          │
│                                                              │
│ Difficulty Level:                                           │
│ ○ Band 5-6 (Beginner-Intermediate)                        │
│ ○ Band 6-7 (Intermediate)                                  │
│ ○ Band 7-8 (Advanced)                                      │
│ ○ Band 8-9 (Expert)                                        │
│                                                              │
│ Generation Mode:                                            │
│ ○ Instant (Pre-Generated) ⚡                               │
│   "Start immediately with curated questions"               │
│                                                              │
│ ○ Personalized (AI-Generated) 🤖                           │
│   "AI creates unique questions based on your profile"    │
│   "Generation time: 30-60 seconds"                         │
│                                                              │
│ ○ Smart Select (Recommended) 🎯                            │
│   "System chooses best option for you"                     │
│                                                              │
│ Advanced Options:                                           │
│ □ Timed mode (strict timing)                               │
│ □ Practice mode (unlimited time)                           │
│ □ Focus on weak areas only                                 │
│                                                              │
│ [Cancel] [Generate Test]                                    │
└─────────────────────────────────────────────────────────────┘
    ↓
    User clicks "Generate Test"
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Backend: Smart Routing Logic                                 │
│                                                              │
│ IF user selected "Instant (Pre-Generated)":                │
│   → Route to Question Bank Service                         │
│   → Skip AI generation entirely                            │
│                                                              │
│ IF user selected "Personalized (AI-Generated)":           │
│   → Route to Question Generator Service                    │
│   → Full AI generation process                             │
│                                                              │
│ IF user selected "Smart Select" OR no selection:          │
│   → Evaluate routing criteria:                             │
│                                                              │
│   Routing Decision Matrix:                                  │
│   ┌──────────────────────────────┬───────────┬───────────┐ │
│   │ Criteria                     │ Pre-Gen   │ On-Demand │ │
│   ├──────────────────────────────┼───────────┼───────────┤ │
│   │ Free plan user               │    ✅     │    ❌     │ │
│   │ Basic plan, < 5 tests/month  │    ✅     │    ✅     │ │
│   │ Basic plan, > 5 tests/month  │    ✅     │    ❌     │ │
│   │ Premium plan                 │    ✅     │    ✅     │ │
│   │ Peak hours (high load)       │    ✅     │    ❌     │ │
│   │ User has weak areas flagged  │    ❌     │    ✅     │ │
│   │ User requests new content    │    ❌     │    ✅     │ │
│   │ Pool availability low        │    ❌     │    ✅     │ │
│   │ User preference: speed       │    ✅     │    ❌     │ │
│   │ User preference: unique      │    ❌     │    ✅     │ │
│   └──────────────────────────────┴───────────┴───────────┘ │
│                                                              │
│   Decision Logic:                                           │
│   • Free users: ALWAYS pre-generated (cost control)        │
│   • Premium users: DEFAULT on-demand, allow pre-gen        │
│   • Basic users: Mix based on quota                        │
│   • Peak hours: Prefer pre-gen to reduce AI load           │
│   • If pre-gen pool empty for criteria: Fallback on-demand │
│                                                              │
│   Final decision stored in test_session record             │
└─────────────────────────────────────────────────────────────┘
    ↓
    BRANCH A: PRE-GENERATED PATH
    ═══════════════════════════════════════════════════════════
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Question Bank Service: Allocate Pre-Generated Test         │
│                                                              │
│ Step 1: Query Available Pool                                │
│   PostgreSQL query:                                         │
│   SELECT * FROM pregen_question_sets                        │
│   WHERE module = :module                                    │
│     AND test_type = :test_type                              │
│     AND difficulty = :difficulty                            │
│     AND status = 'available'                                │
│     AND usage_count < max_usage_limit                       │
│     AND last_used_at < NOW() - INTERVAL '7 days'          │
│   ORDER BY quality_score DESC, usage_count ASC             │
│   LIMIT 10                                                  │
│                                                              │
│ Step 2: Apply User Exclusion Filter                         │
│   • Check user's test history                              │
│   • Exclude question sets user has already taken           │
│   • Exclude recently seen similar topics                   │
│                                                              │
│ Step 3: Select Best Match                                   │
│   • Score candidates by:                                   │
│     - Quality score (0-100)                                │
│     - Time since last use (freshness)                      │
│     - Topic diversity vs user's history                    │
│     - Difficulty calibration accuracy                      │
│   • Select highest scoring set                             │
│                                                              │
│ Step 4: Mark as Allocated                                   │
│   UPDATE pregen_question_sets SET                          │
│     status = 'allocated',                                   │
│     allocated_to_user = :user_id,                          │
│     allocated_at = NOW(),                                  │
│     usage_count = usage_count + 1                          │
│   WHERE id = :selected_set_id                              │
│                                                              │
│ Step 5: Create Test Session                                 │
│   INSERT INTO test_sessions (                              │
│     user_id,                                                │
│     generation_mode = 'pre_generated',                     │
│     pregen_set_id = :selected_set_id,                      │
│     status = 'ready',                                      │
│     ...                                                     │
│   )                                                         │
│                                                              │
│ Step 6: Return Response (< 500ms)                          │
│   {                                                          │
│     "test_session_id": "uuid-...",                         │
│     "generation_mode": "pre_generated",                    │
│     "status": "ready",                                     │
│     "estimated_time": 0,                                   │
│     "message": "Your test is ready!"                      │
│   }                                                         │
└─────────────────────────────────────────────────────────────┘
    ↓
    INSTANT: Test ready to start (no waiting)
    ↓

    BRANCH B: ON-DEMAND PATH
    ═══════════════════════════════════════════════════════════
    ↓
    [Follow existing AI generation process: 30-75 seconds]
    ↓
END: Test ready regardless of path
```

---

### **FLOW 3: Taking a Complete Test - Listening Module**

```
┌─────────────────────────────────────────────────────────────────┐
│              LISTENING TEST FLOW (30 + 10 minutes)               │
└─────────────────────────────────────────────────────────────────┘

User clicks "Start Test"
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Frontend: Pre-Test Instructions Page                        │
│                                                              │
│ "IELTS Listening Test Instructions"                        │
│                                                              │
│ • You will hear 4 audio sections                           │
│ • You will answer 40 questions in total                    │
│ • You can play each section only ONCE                      │
│ • You can see questions while listening                    │
│ • After all sections, you get 10 minutes to transfer      │
│   answers                                                   │
│                                                              │
│ Equipment check:                                            │
│ [Test Audio] ← User can test speakers/headphones          │
│                                                              │
│ [I'm Ready - Start Listening Test]                        │
└─────────────────────────────────────────────────────────────┘
    ↓
    User clicks "Start Listening Test"
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Backend: Test Service                                        │
│ • Update test_session status: 'listening_in_progress'      │
│ • Record start time                                         │
│ • Fetch listening module from MongoDB/Redis cache          │
│ • Return Section 1 data to frontend                        │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Frontend: Listening Test Interface - SECTION 1             │
│                                                              │
│ ┌──────────────────────────────────────────────────────┐   │
│ │ Timer: 03:05 remaining        Section 1 of 4         │   │
│ │ Questions 1-10                                        │   │
│ └──────────────────────────────────────────────────────┘   │
│                                                              │
│ ┌──────────────────────────────────────────────────────┐   │
│ │ Audio Player                                          │   │
│ │ [▶ Playing...] ════════●════════ 01:23 / 03:45      │   │
│ │                                                        │   │
│ │ [This audio will play only once]                     │   │
│ │ Volume: ▁▂▃▄▅▆▇█                                     │   │
│ └──────────────────────────────────────────────────────┘   │
│                                                              │
│ Questions 1-4: Complete the form below                     │
│ Write NO MORE THAN TWO WORDS AND/OR A NUMBER              │
│                                                              │
│ Apartment Details                                          │
│ Monthly rent: £ [________] (Question 1)                    │
│ Number of bedrooms: [________] (Question 2)                │
│ Available from: [________] (Question 3)                    │
│ Deposit required: £ [________] (Question 4)                │
│                                                              │
│ Questions 5-7: Choose the correct letter, A, B or C        │
│                                                              │
│ 5. The apartment is located:                               │
│    ○ A) Near the city center                               │
│    ○ B) In a quiet suburb                                  │
│    ○ C) Close to the university                            │
│                                                              │
│ 6. Parking is:                                              │
│    ○ A) Included in the rent                               │
│    ○ B) Available for an extra fee                         │
│    ○ C) Not available                                       │
│                                                              │
│ 7. The landlord prefers:                                   │
│    ○ A) Students                                            │
│    ○ B) Professionals                                       │
│    ○ C) No preference                                       │
│                                                              │
│ Questions 8-10: Write NO MORE THAN THREE WORDS             │
│                                                              │
│ 8. Utilities included: [________________]                  │
│ 9. Lease duration: [________________]                      │
│ 10. Contact method: [________________]                     │
│                                                              │
│ [Mark for Review] [Clear Answers]                          │
└─────────────────────────────────────────────────────────────┘
    ↓
    Audio plays automatically
    ↓
    User fills in answers while listening
    ↓
    Frontend auto-saves answers every 10 seconds to backend
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Auto-save mechanism                                          │
│ • Uses debounced AJAX calls                                 │
│ • Saves to Redis (temp storage)                            │
│ • No page refresh required                                  │
│ • "Saved" indicator appears briefly                        │
└─────────────────────────────────────────────────────────────┘
    ↓
    Audio Section 1 ends (3:45 duration)
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Brief Pause Screen (10 seconds)                             │
│                                                              │
│ "Section 1 complete. Section 2 will begin in 10 seconds." │
│                                                              │
│ User can review Section 1 answers during this pause        │
│                                                              │
│ Countdown: 10... 9... 8...                                  │
└─────────────────────────────────────────────────────────────┘
    ↓
    Automatically proceed to Section 2
    ↓
    [Similar process for Sections 2, 3, and 4]
    [Each section: 3-4 minutes audio + 10 questions]
    ↓
    Total listening time: ~30 minutes
    ↓
┌─────────────────────────────────────────────────────────────┐
│ All 4 Sections Complete                                     │
│                                                              │
│ "You have now completed all listening sections."           │
│                                                              │
│ "You have 10 minutes to transfer and check your answers."  │
│                                                              │
│ Transfer Time: 10:00                                        │
│                                                              │
│ [Review All Answers]                                        │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Answer Review Interface                                      │
│                                                              │
│ Shows all 40 questions across 4 sections                   │
│ ┌──────────────────────────────────────────────────────┐   │
│ │ Section 1: Questions 1-10                             │   │
│ │ [Expand/Collapse]                                      │   │
│ │                                                        │   │
│ │ Answered: 10/10 ✓                                     │   │
│ │                                                        │   │
│ │ 1. [850]                                              │   │
│ │ 2. [three / 3]                                        │   │
│ │ 3. [next month]                                       │   │
│ │ ...                                                    │   │
│ └──────────────────────────────────────────────────────┘   │
│                                                              │
│ │ Section 2: Questions 11-20                            │   │
│ │ Answered: 9/10 ⚠                                      │   │
│ │ Question 15 is blank!                                 │   │
│ └──────────────────────────────────────────────────────┘   │
│                                                              │
│ ... Sections 3 & 4 ...                                      │
│                                                              │
│ Timer: 08:23 remaining                                      │
│                                                              │
│ [Submit Listening Test]                                     │
└─────────────────────────────────────────────────────────────┘
    ↓
    User clicks "Submit Listening Test"
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Confirmation Dialog                                          │
│                                                              │
│ "Are you sure you want to submit?"                         │
│                                                              │
│ "You have answered 39 out of 40 questions."                │
│ "Question 15 is blank."                                     │
│                                                              │
│ [Go Back] [Submit Now]                                      │
└─────────────────────────────────────────────────────────────┘
    ↓
    User clicks "Submit Now"
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Frontend sends all answers to Backend                       │
│                                                              │
│ POST /api/v1/submissions/listening                         │
│ {                                                            │
│   "test_session_id": "uuid-...",                           │
│   "answers": [                                              │
│     {                                                        │
│       "question_number": 1,                                 │
│       "user_answer": "850",                                │
│       "time_spent_seconds": 15                             │
│     },                                                       │
│     // ... 39 more answers                                 │
│   ],                                                         │
│   "submission_timestamp": "2025-11-08T14:32:15Z",          │
│   "time_taken_seconds": 2415 // 40:15                      │
│ }                                                            │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Backend: Test Service                                        │
│                                                              │
│ Step 1: Validate submission                                 │
│ • Check test_session exists                                │
│ • Verify user authorization                                │
│ • Check not already submitted                              │
│                                                              │
│ Step 2: Store raw answers in PostgreSQL                    │
│ • Create record in listening_submissions table             │
│ • Status: 'submitted'                                       │
│                                                              │
│ Step 3: Auto-grade (No AI needed!)                         │
│ • Retrieve correct answers from MongoDB                    │
│ • Compare user answers with correct answers                │
│ • Account for alternative accepted answers                 │
│ • Case-insensitive matching                                │
│ • Allow for spelling variations (configurable)             │
│   Example: "three" == "3" == "Three"                       │
│                                                              │
│ Grading logic:                                              │
│ correct_count = 0                                           │
│ for each question:                                          │
│   user_ans = normalize(user_answer)                        │
│   if user_ans in [correct_answer, *alternatives]:          │
│     correct_count += 1                                      │
│     mark_as_correct(question_number)                       │
│   else:                                                      │
│     mark_as_incorrect(question_number)                     │
│                                                              │
│ Step 4: Calculate band score                                │
│ • Use official IELTS conversion table                      │
│                                                              │
│ Academic Listening Score Table:                            │
│ 39-40 correct = Band 9.0                                    │
│ 37-38 correct = Band 8.5                                    │
│ 35-36 correct = Band 8.0                                    │
│ 32-34 correct = Band 7.5                                    │
│ 30-31 correct = Band 7.0                                    │
│ 26-29 correct = Band 6.5                                    │
│ 23-25 correct = Band 6.0                                    │
│ 18-22 correct = Band 5.5                                    │
│ 16-17 correct = Band 5.0                                    │
│ 13-15 correct = Band 4.5                                    │
│ 10-12 correct = Band 4.0                                    │
│                                                              │
│ Example: 30 correct → Band 7.0                             │
│                                                              │
│ Step 5: Store results in PostgreSQL                        │
│ UPDATE listening_submissions SET                            │
│   correct_answers = 30,                                     │
│   incorrect_answers = 10,                                   │
│   band_score = 7.0,                                         │
│   detailed_results = {...},  // JSON                       │
│   evaluated_at = NOW(),                                     │
│   status = 'evaluated'                                      │
│                                                              │
│ Step 6: Update test_session                                │
│ • listening_score = 7.0                                     │
│ • listening_completed_at = NOW()                           │
│                                                              │
│ Step 7: Return response to frontend                        │
│ {                                                            │
│   "success": true,                                          │
│   "band_score": 7.0,                                        │
│   "correct_answers": 30,                                    │
│   "total_questions": 40,                                    │
│   "accuracy_percentage": 75,                               │
│   "results_url": "/results/listening/uuid-..."             │
│ }                                                            │
└─────────────────────────────────────────────────────────────┘
    ↓
    Response returned to frontend (< 2 seconds)
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Frontend: Listening Results Page                            │
│                                                              │
│ ┌──────────────────────────────────────────────────────┐   │
│ │ ✅ Listening Test Complete!                          │   │
│ │                                                        │   │
│ │ Your Band Score: 7.0                                  │   │
│ │ [Visual: Circle gauge showing 7.0]                   │   │
│ │                                                        │   │
│ │ Performance:                                          │   │
│ │ ✓ Correct: 30/40 (75%)                               │   │
│ │ ✗ Incorrect: 10/40 (25%)                             │   │
│ │                                                        │   │
│ │ Section Breakdown:                                    │   │
│ │ • Section 1: 8/10 ✓✓✓✓✓✓✓✓✗✗                        │   │
│ │ • Section 2: 9/10 ✓✓✓✓✓✓✓✓✓✗                        │   │
│ │ • Section 3: 7/10 ✓✓✓✓✓✓✓✗✗✗                        │   │
│ │ • Section 4: 6/10 ✓✓✓✓✓✓✗✗✗✗                        │   │
│ │                                                        │   │
│ │ [View Detailed Feedback] [Continue to Reading]       │   │
│ └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
    ↓
    User clicks "View Detailed Feedback"
    ↓
┌─────────────────────────────────────────────────────────────┐
│ Detailed Feedback Page                                      │
│                                                              │
│ Shows each question with:                                   │
│ • Your answer                                               │
│ • Correct answer                                            │
│ • Audio timestamp where answer appears                     │
│ • Explanation                                               │
│                                                              │
│ Example:                                                    │
│ ┌──────────────────────────────────────────────────────┐   │
│ │ Question 1                                            │   │
│ │ Monthly rent: £ ____                                  │   │
│ │                                                        │   │
│ │ Your Answer: 850 ✓                                    │   │
│ │ Correct Answer: 850                                   │   │
│ │                                                        │   │
│ │ Audio Timestamp: 00:45                                │   │
│ │ [🔊 Play Audio Snippet]                               │   │
│ │                                                        │   │
│ │ Transcript: "...the monthly rent is eight hundred    │   │
│ │ and fifty pounds..."                                  │   │
│ └──────────────────────────────────────────────────────┘   │
│                                                              │
│ ┌──────────────────────────────────────────────────────┐   │
│ │ Question 5                                            │   │
│ │ The apartment is located:                             │   │
│ │                                                        │   │
│ │ Your Answer: A) Near the city center ✗               │   │
│ │ Correct Answer: C) Close to the university           │   │
│ │                                                        │   │
│ │ Audio Timestamp: 01:32                                │   │
│ │ [🔊 Play Audio Snippet]                               │   │
│ │                                                        │   │
│ │ Transcript: "...it's just a five-minute walk from    │   │
│ │ the university campus..."                             │   │
│ │                                                        │   │
│ │ Explanation: The key phrase "five-minute walk from   │   │
│ │ the university" indicates proximity to the            │   │
│ │ university, not the city center.                      │   │
│ └──────────────────────────────────────────────────────┘   │
│                                                              │
│ ... all 40 questions ...                                    │
│                                                              │
│ [Download PDF Report] [Continue to Next Module]            │
└─────────────────────────────────────────────────────────────┘
    ↓
END: Listening module complete
```

---
## 🤖 **AI Integration Architecture**

### **AI Orchestration Service**

The AI Orchestration Service is the central hub for all AI-related operations. It manages model selection, prompt versioning, response validation, and cost tracking.

#### **Model Selection Engine**

```python
# Model Selection Logic
class ModelSelector:
    def select_model(self, task_type, quality_required, budget_constraint):
        routing_table = {
            'listening_script': {
                'primary': 'gemini_pro_1.5',
                'secondary': 'gpt4o',
                'cost_per_1k_tokens': 0.0005,
                'avg_latency': '8s'
            },
            'reading_passage': {
                'primary': 'claude_sonnet_4.5',
                'secondary': 'gpt4o',
                'cost_per_1k_tokens': 0.003,
                'avg_latency': '15s'
            },
            'writing_evaluation': {
                'primary': 'claude_sonnet_4.5',
                'secondary': 'gpt4o',
                'cost_per_1k_tokens': 0.003,
                'avg_latency': '25s'
            },
            'speaking_questions': {
                'primary': 'gpt4o',
                'secondary': 'claude_sonnet_4.5',
                'cost_per_1k_tokens': 0.005,
                'avg_latency': '10s'
            }
        }

        # Check provider health
        if not self.is_provider_healthy(routing_table[task_type]['primary']):
            return routing_table[task_type]['secondary']

        # Check budget constraints
        if budget_constraint == 'strict':
            return self.get_cheapest_option(task_type)

        return routing_table[task_type]['primary']
```

#### **Prompt Management System**

```
Prompt Template Storage (MongoDB)

Collection: prompts
{
  "_id": ObjectId("..."),
  "prompt_id": "listening_script_gen_v2.3",
  "version": "2.3.1",
  "task_type": "listening_script",
  "model": "gemini_pro_1.5",
  "template": "System: You are an IELTS test creator...",
  "variables": ["theme", "difficulty", "speakers"],
  "output_schema": {
    "type": "json",
    "required_fields": ["script", "key_information"],
    "validation_rules": {...}
  },
  "performance_metrics": {
    "avg_tokens": 1200,
    "avg_cost": 0.045,
    "success_rate": 0.97,
    "avg_latency_ms": 8500
  },
  "ab_test_variant": "B",
  "is_active": true,
  "created_at": ISODate("..."),
  "updated_at": ISODate("...")
}

Version Control:
- Git-like versioning for prompts
- Rollback capability
- A/B testing framework
- Performance tracking per version

A/B Testing:
- Randomly assign users to prompt variants
- Track: success rate, cost, latency, user satisfaction
- Auto-promote winning variant
```

#### **Response Validation & Retry Logic**

```python
class ResponseValidator:
    def validate_and_retry(self, ai_response, expected_schema, max_retries=3):
        for attempt in range(max_retries):
            try:
                # Parse JSON
                parsed = json.loads(ai_response)

                # Validate schema
                self.validate_schema(parsed, expected_schema)

                # Check content quality
                quality_score = self.score_content_quality(parsed)

                if quality_score >= 0.85:
                    return {
                        'success': True,
                        'data': parsed,
                        'quality_score': quality_score,
                        'attempts': attempt + 1
                    }
                else:
                    raise QualityTooLow(f"Quality score: {quality_score}")

            except (json.JSONDecodeError, ValidationError, QualityTooLow) as e:
                if attempt < max_retries - 1:
                    refined_prompt = self.refine_prompt(e)
                    ai_response = self.call_ai(refined_prompt)
                else:
                    return self.fallback_generation(expected_schema)

        return {'success': False, 'error': 'Max retries exceeded'}
```

#### **Cost Tracking & Budget Management**

```
AI Cost Tracking Dashboard

Daily AI Usage (Real-time):
Total Today: $1,247.50
Budget Remaining: $752.50 (60.2%)

By Provider:
- OpenAI (GPT-4o):     $523.40 (42%)
- Anthropic (Claude):  $489.20 (39%)
- Google (Gemini):     $185.90 (15%)
- ElevenLabs (TTS):     $49.00 (4%)

By Task Type:
- Question Generation:  $623.50 (50%)
- Writing Evaluation:   $498.20 (40%)
- Speaking Evaluation:   $75.80 (6%)
- Audio Generation:      $50.00 (4%)

Alerts:
- 50% threshold: Email to ops team
- 75% threshold: Slack alert + throttle non-essential
- 90% threshold: Emergency mode - pre-gen only
- 100% threshold: Halt all on-demand generation

Cost Optimization Strategies:
1. Batch similar requests together
2. Use cheaper models for validation tasks
3. Cache frequently requested prompts
4. Route free users to pre-generated content
5. Compress prompts to reduce token usage
```

---

## 🗄️ **Data Architecture**

### **Database Schema Design**

#### **PostgreSQL (Primary Relational DB)**

```sql
-- Users Table
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    full_name VARCHAR(255),
    profile_picture_url TEXT,
    country VARCHAR(100),
    date_of_birth DATE,
    native_language VARCHAR(100),
    proficiency_level VARCHAR(50),
    email_verified BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Test Sessions Table
CREATE TABLE test_sessions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    generation_mode VARCHAR(50) NOT NULL,
    pregen_set_id UUID REFERENCES pregen_question_sets(id),
    test_type VARCHAR(50) NOT NULL,
    difficulty VARCHAR(50) NOT NULL,
    status VARCHAR(50) DEFAULT 'generating',
    listening_score DECIMAL(3,1),
    reading_score DECIMAL(3,1),
    writing_score DECIMAL(3,1),
    speaking_score DECIMAL(3,1),
    overall_band DECIMAL(3,1),
    started_at TIMESTAMP,
    completed_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW(),
    expires_at TIMESTAMP
);

-- Pre-Generated Question Sets Metadata
CREATE TABLE pregen_question_sets (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    module VARCHAR(50) NOT NULL,
    test_type VARCHAR(50) NOT NULL,
    difficulty VARCHAR(50) NOT NULL,
    status VARCHAR(50) DEFAULT 'available',
    quality_score DECIMAL(4,2) DEFAULT 0.00,
    usage_count INTEGER DEFAULT 0,
    max_usage_limit INTEGER DEFAULT 100,
    last_used_at TIMESTAMP,
    allocated_to_user UUID REFERENCES users(id),
    allocated_at TIMESTAMP,
    ai_cost_usd DECIMAL(8,4),
    created_at TIMESTAMP DEFAULT NOW()
);

-- Submissions Table
CREATE TABLE submissions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    test_session_id UUID REFERENCES test_sessions(id),
    user_id UUID REFERENCES users(id),
    module VARCHAR(50) NOT NULL,
    answers JSONB NOT NULL,
    correct_answers INTEGER,
    incorrect_answers INTEGER,
    band_score DECIMAL(3,1),
    detailed_results JSONB,
    time_taken_seconds INTEGER,
    status VARCHAR(50) DEFAULT 'submitted',
    submitted_at TIMESTAMP DEFAULT NOW(),
    evaluated_at TIMESTAMP
);

-- Subscriptions Table
CREATE TABLE subscriptions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    plan_type VARCHAR(50) NOT NULL,
    status VARCHAR(50) DEFAULT 'active',
    stripe_subscription_id VARCHAR(255),
    current_period_start TIMESTAMP,
    current_period_end TIMESTAMP,
    tests_used_this_month INTEGER DEFAULT 0,
    tests_limit INTEGER DEFAULT 3,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Indexes for Performance
CREATE INDEX idx_test_sessions_user ON test_sessions(user_id);
CREATE INDEX idx_test_sessions_status ON test_sessions(status);
CREATE INDEX idx_pregen_sets_module ON pregen_question_sets(module, test_type, difficulty);
CREATE INDEX idx_pregen_sets_status ON pregen_question_sets(status);
CREATE INDEX idx_submissions_session ON submissions(test_session_id);
CREATE INDEX idx_submissions_user ON submissions(user_id);
```

#### **MongoDB (Document Store)**

```javascript
// Questions Collection (for on-demand generated content)
db.questions.insertOne({
  _id: ObjectId("..."),
  test_session_id: "uuid-...",
  module: "listening",
  difficulty: "band_6_7",
  test_type: "academic",
  sections: [
    {
      section_number: 1,
      theme: "Apartment rental inquiry",
      audio_url: "https://cdn.../section_1.mp3",
      duration_seconds: 185,
      script: "Full transcript...",
      questions: [
        {
          number: 1,
          type: "form_completion",
          question_text: "Monthly rent: £____",
          correct_answer: "850",
          alternatives: ["850 pounds", "£850"],
          points: 1
        }
      ]
    }
  ],
  total_questions: 40,
  total_duration_seconds: 1800,
  created_at: ISODate("..."),
  ai_cost: {
    script_generation: 0.045,
    audio_generation: 0.32,
    total: 0.365
  }
});

// Pre-Generated Question Pool
db.pregen_pool.insertOne({
  _id: ObjectId("..."),
  set_id: "uuid-...",
  module: "reading",
  test_type: "academic",
  difficulty: "band_6_7",
  content: {
    passages: [...],
    questions: [...]
  },
  quality_score: 92.5,
  usage_count: 0,
  max_usage_limit: 100,
  status: "available",
  created_at: ISODate("...")
});

// AI Logs Collection
db.ai_logs.insertOne({
  _id: ObjectId("..."),
  request_id: "uuid-...",
  provider: "openai",
  model: "gpt-4o",
  task_type: "writing_evaluation",
  prompt_tokens: 2450,
  completion_tokens: 1800,
  total_tokens: 4250,
  cost_usd: 0.1275,
  latency_ms: 28500,
  success: true,
  error_message: null,
  created_at: ISODate("...")
});
```

#### **Redis (Cache Layer)**

```
Key Patterns:

Session Store:
  Key: session:{session_id}
  Value: {user_id, jwt_token, expires_at}
  TTL: 24 hours

Rate Limiting:
  Key: rate_limit:{user_id}:{endpoint}
  Value: request_count
  TTL: 1 minute

Test Draft Cache:
  Key: test_draft:{test_session_id}
  Value: {answers, progress, last_saved}
  TTL: 3 hours

Pre-Gen Cache:
  Key: pregen:{module}:{test_type}:{difficulty}
  Value: [available_set_ids]
  TTL: 5 minutes

Leaderboard:
  Key: leaderboard:{timeframe}
  Value: Sorted Set (score -> user_id)
  TTL: 1 hour
```

---

## 🔧 **Microservices Breakdown**

### **Service Communication Patterns**

```
Synchronous (REST/gRPC):
- Auth Service <-> All services (JWT validation)
- API Gateway <-> All services (request routing)
- Test Service <-> Question Bank Service (pool queries)
- Test Service <-> User Service (profile data)

Asynchronous (Message Queue):
- Question Generator -> Question Bank (new pre-gen items)
- Test Service -> Evaluation Service (submissions)
- Test Service -> Audio Process Service (audio generation)
- Evaluation Service -> Notification Service (results ready)
- Analytics Service -> Report Gen Service (report requests)

Event-Driven (WebSocket):
- Test Service -> Frontend (real-time test updates)
- Evaluation Service -> Frontend (evaluation progress)
- Notification Service -> Frontend (push notifications)
```

### **Auth Service (Port 3001)**

```
Responsibilities:
- User registration & login
- JWT token generation & validation
- OAuth integration (Google, Facebook, Apple)
- Two-factor authentication
- Password reset flow
- Session management

API Endpoints:
POST /api/v1/auth/register
POST /api/v1/auth/login
POST /api/v1/auth/logout
POST /api/v1/auth/refresh
POST /api/v1/auth/forgot-password
POST /api/v1/auth/reset-password
GET  /api/v1/auth/verify-email/:token
POST /api/v1/auth/oauth/:provider

Database: PostgreSQL (users table)
Cache: Redis (session store)
```

### **User Service (Port 3002)**

```
Responsibilities:
- User profile management
- Preferences & settings
- Progress tracking
- Test history
- Weak areas identification
- Study plan storage

API Endpoints:
GET    /api/v1/users/profile
PUT    /api/v1/users/profile
GET    /api/v1/users/progress
GET    /api/v1/users/history
GET    /api/v1/users/weak-areas
POST   /api/v1/users/study-plan
GET    /api/v1/users/settings
PUT    /api/v1/users/settings

Database: PostgreSQL
Cache: Redis (user profile cache)
```

### **Test Service (Port 3003)**

```
Responsibilities:
- Test session lifecycle management
- Test configuration & validation
- Answer submission handling
- Result aggregation
- Smart routing (pre-gen vs on-demand)
- Test scheduling

API Endpoints:
POST   /api/v1/tests/create
GET    /api/v1/tests/:id
POST   /api/v1/tests/:id/start
POST   /api/v1/tests/:id/submit/:module
GET    /api/v1/tests/:id/results
GET    /api/v1/tests/:id/feedback
DELETE /api/v1/tests/:id
GET    /api/v1/tests/history

Database: PostgreSQL (test_sessions, submissions)
Cache: Redis (test drafts, active sessions)
```

### **Question Generator Service (Port 3004)**

```
Responsibilities:
- AI-powered question generation
- Content validation
- Audio script generation
- Chart/data generation
- Quality assurance
- Cost optimization

API Endpoints:
POST   /api/v1/generate/listening
POST   /api/v1/generate/reading
POST   /api/v1/generate/writing
POST   /api/v1/generate/speaking
POST   /api/v1/generate/validate
GET    /api/v1/generate/status/:job_id

Database: MongoDB (questions, prompts, ai_logs)
Queue: RabbitMQ (question-generation-queue)
```

### **Evaluation Service (Port 3005)**

```
Responsibilities:
- Writing evaluation (AI-powered)
- Speaking evaluation (AI + STT)
- Reading evaluation (auto + AI)
- Band score calculation
- Detailed feedback generation
- Error identification

API Endpoints:
POST   /api/v1/evaluate/writing
POST   /api/v1/evaluate/speaking
POST   /api/v1/evaluate/reading
GET    /api/v1/evaluate/status/:submission_id
GET    /api/v1/evaluate/results/:submission_id

Database: PostgreSQL (evaluations)
Queue: RabbitMQ (evaluation-queue)
```

### **Question Bank Service (Port 3013) - NEW**

```
Responsibilities:
- Pre-generated question pool management
- Smart allocation to users
- Usage tracking & analytics
- Quality scoring
- Pool replenishment scheduling
- Deprecation of overused content

API Endpoints:
GET    /api/v1/bank/allocate
POST   /api/v1/bank/release
GET    /api/v1/bank/availability
POST   /api/v1/bank/ingest
GET    /api/v1/bank/stats
POST   /api/v1/bank/deprecate

Database: 
  - PostgreSQL (pregen_question_sets metadata)
  - MongoDB (actual question content)
Cache: Redis (available set lists)
Queue: RabbitMQ (pre-generation-queue)
```

---
## 🔒 **Security Architecture**

### **Defense in Depth Strategy**

```
Layer 1: Edge Security (Cloudflare)
- DDoS protection (L3/L4/L7)
- Web Application Firewall (WAF)
- Bot detection & mitigation
- SSL/TLS termination (TLS 1.3)
- Geographic blocking

Layer 2: Network Security
- VPC isolation (AWS)
- Security groups & NACLs
- Private subnets for databases
- VPN for admin access
- Network segmentation between services

Layer 3: Application Security
- Input validation & sanitization
- SQL injection prevention (parameterized queries)
- XSS protection (CSP headers)
- CSRF tokens
- Rate limiting per user/IP

Layer 4: Authentication & Authorization
- JWT tokens (RS256, short-lived)
- OAuth 2.0 / OpenID Connect
- Role-based access control (RBAC)
- API key management
- Session invalidation

Layer 5: Data Security
- Encryption at rest (AES-256)
- Encryption in transit (TLS 1.3)
- Database column-level encryption (PII)
- Secure key management (AWS KMS)
- Data masking in logs

Layer 6: AI Security
- Prompt injection detection
- Output sanitization
- Rate limiting per AI provider
- Cost anomaly detection
- Content moderation
```

### **Authentication Flow**

```
┌─────────────────────────────────────────────────────────────┐
│ JWT Authentication Flow                                      │
│                                                              │
│ 1. User Login                                               │
│    POST /api/v1/auth/login                                  │
│    {email, password}                                        │
│         ↓                                                    │
│    Auth Service validates credentials                       │
│         ↓                                                    │
│    Generates:                                               │
│    • Access Token (JWT) - 15 min expiry                    │
│    • Refresh Token (JWT) - 7 day expiry                    │
│         ↓                                                    │
│    Stores refresh token hash in Redis                       │
│         ↓                                                    │
│    Returns tokens to client                                 │
│                                                              │
│ 2. API Request                                              │
│    Client sends: Authorization: Bearer {access_token}      │
│         ↓                                                    │
│    API Gateway validates JWT signature                      │
│         ↓                                                    │
│    Extracts user_id, roles from payload                     │
│         ↓                                                    │
│    Routes to appropriate service                            │
│                                                              │
│ 3. Token Refresh                                            │
│    Access token expired → 401 response                     │
│         ↓                                                    │
│    Client sends: POST /api/v1/auth/refresh                 │
│    {refresh_token}                                          │
│         ↓                                                    │
│    Auth Service validates refresh token                     │
│         ↓                                                    │
│    Issues new access token + refresh token                  │
│                                                              │
│ 4. Logout                                                   │
│    POST /api/v1/auth/logout                                 │
│         ↓                                                    │
│    Blacklist access token (Redis, TTL=expiry)              │
│    Delete refresh token from Redis                          │
│         ↓                                                    │
│    Client clears tokens from storage                        │
└─────────────────────────────────────────────────────────────┘
```

### **Data Protection**

```
PII Handling:
- Email: Encrypted at rest (AWS KMS)
- Password: Hashed with bcrypt (cost factor 12)
- Profile pictures: Stored in private S3 bucket
- Test responses: Anonymized after 1 year
- Payment data: Never stored (Stripe handles PCI)

Compliance:
- GDPR: Right to deletion, data portability
- CCPA: Opt-out of data sale (N/A - no data sale)
- SOC 2 Type II: Annual audit
- ISO 27001: Information security management

Audit Logging:
- All admin actions logged
- Data access logs retained 2 years
- Failed login attempts tracked
- AI prompt/response logs (for quality, not PII)
```

---

## 📈 **Scalability & Performance**

### **Horizontal Scaling Strategy**

```
Auto-Scaling Configuration:

Web Tier (Next.js):
- Min: 3 instances
- Max: 20 instances
- Scale trigger: CPU > 70% or requests > 1000/min
- Health check: /health every 30s

API Services (Node.js):
- Min: 2 instances per service
- Max: 10 instances per service
- Scale trigger: CPU > 60% or latency > 500ms
- Circuit breaker: 5 errors in 60s

AI Orchestration (Python):
- Min: 2 instances
- Max: 15 instances
- Scale trigger: Queue depth > 50 or latency > 10s
- Special: GPU instances for local ML models

Databases:
- PostgreSQL: Read replicas (3), Write master (1)
- MongoDB: Sharded cluster (3 shards)
- Redis: Cluster mode (6 nodes)
- Elasticsearch: 3-node cluster

CDN:
- Static assets: Cloudflare (global)
- Audio files: CloudFront + S3 (regional caching)
- Pre-generated tests: Edge caching (1 hour TTL)
```

### **Performance Targets**

```
Response Time SLAs:
- Landing page load: < 1.5s
- Test configuration: < 200ms
- Pre-generated test ready: < 500ms
- On-demand test generation: < 75s
- Answer submission: < 2s
- Auto-grading (Listening/Reading): < 3s
- AI evaluation (Writing/Speaking): < 90s
- Results page load: < 1s
- PDF report generation: < 30s

Throughput Targets:
- Concurrent users: 10,000
- Tests per day: 50,000
- AI requests per day: 100,000
- Audio files served: 200,000/day

Availability:
- Target: 99.9% uptime (8.77h downtime/year)
- Critical path: 99.95% (test taking)
- Maintenance windows: Sundays 2-4 AM UTC
```

### **Caching Strategy**

```
Multi-Layer Caching:

Layer 1: Browser Cache
- Static assets: 1 year
- Test interface: 1 hour
- User profile: 15 minutes

Layer 2: CDN Cache (Cloudflare)
- Landing page: 5 minutes
- Public content: 1 hour
- Audio files: 24 hours
- Pre-generated test data: 1 hour

Layer 3: Application Cache (Redis)
- User sessions: 24 hours
- Test drafts: 3 hours
- Pre-gen availability: 5 minutes
- Leaderboards: 1 hour
- Rate limit counters: 1 minute

Layer 4: Database Cache
- PostgreSQL: Shared buffers (25% RAM)
- MongoDB: WiredTiger cache (50% RAM - 1GB)
- Query result cache: Enabled

Cache Invalidation:
- User profile update: Invalidate user:*
- New pre-gen content: Invalidate pregen:*
- Test completion: Invalidate test_draft:*
- Subscription change: Invalidate subscription:*
```

---

## 💰 **Cost Management**

### **Cost Breakdown by Component**

```
Monthly Cost Estimate (10,000 active users):

Infrastructure:
- EC2 Instances (web + API): $800
- ECS/EKS (container orchestration): $400
- RDS PostgreSQL (db.r5.xlarge): $600
- MongoDB Atlas (M30): $500
- Redis ElastiCache (cache.r5.large): $350
- S3 Storage (5TB): $115
- CloudFront CDN: $200
- Cloudflare Pro: $200
- Load Balancers: $200
Subtotal Infrastructure: $3,365

AI Services:
- OpenAI GPT-4o: $2,500
- Anthropic Claude Sonnet: $2,000
- Google Gemini Pro: $800
- ElevenLabs TTS: $500
- Whisper API: $200
Subtotal AI: $6,000

Third-Party Services:
- Stripe (payment processing): $300
- SendGrid/SES (email): $100
- Twilio (SMS): $50
- DataDog/New Relic (monitoring): $400
- Sentry (error tracking): $100
Subtotal Third-Party: $950

Total Monthly: ~$10,315
Cost per user: ~$1.03/month

Revenue (at $19.99 Basic plan, 30% conversion):
- 3,000 paid users × $19.99 = $59,970/month
- Gross margin: ~83%
```

### **Cost Optimization Strategies**

```
1. Pre-Generated Question Bank
   - Reduces AI costs by 60-70%
   - Amortizes generation cost across multiple users
   - Target: 80% of tests use pre-gen content

2. Smart Model Routing
   - Use cheaper models for simple tasks
   - Reserve expensive models for evaluation
   - Estimated savings: 30-40%

3. Batch Processing
   - Group similar AI requests
   - Process during off-peak hours
   - Estimated savings: 15-20%

4. Caching
   - Cache AI responses for identical prompts
   - Cache frequently accessed data
   - Estimated savings: 10-15%

5. Reserved Instances
   - 1-year reserved EC2: 40% savings
   - 3-year reserved RDS: 60% savings
   - Estimated savings: $1,200/month

6. Spot Instances
   - Use for batch processing workers
   - 70-90% cheaper than on-demand
   - Estimated savings: $200/month

7. Content Compression
   - Compress audio files (Opus codec)
   - Optimize images (WebP format)
   - Estimated savings: $100/month
```

---

## 🚀 **Deployment Architecture**

### **CI/CD Pipeline**

```
┌─────────────────────────────────────────────────────────────┐
│                    CI/CD PIPELINE                            │
│                                                              │
│ 1. Code Commit                                              │
│    Developer pushes to feature branch                       │
│         ↓                                                    │
│    GitHub Actions triggered                                 │
│                                                              │
│ 2. Build & Test                                             │
│    ┌─────────────────────────────────────────────────────┐  │
│    │ • Linting (ESLint, Prettier, Black)                │  │
│    │ • Unit tests (Jest, Pytest) - >80% coverage        │  │
│    │ • Integration tests (Postman, Cypress)             │  │
│    │ • Security scan (Snyk, Trivy)                      │  │
│    │ • Type checking (TypeScript)                       │  │
│    └─────────────────────────────────────────────────────┘  │
│         ↓                                                    │
│    All checks pass → Continue                               │
│    Any check fails → Block merge                            │
│                                                              │
│ 3. Staging Deployment                                       │
│    Merge to develop branch                                  │
│         ↓                                                    │
│    Build Docker images                                      │
│         ↓                                                    │
│    Push to ECR (tag: develop)                              │
│         ↓                                                    │
│    Deploy to Staging EKS cluster                            │
│         ↓                                                    │
│    Run smoke tests                                          │
│    Run E2E tests (Playwright)                              │
│                                                              │
│ 4. Production Deployment                                    │
│    Merge to main branch (PR approval required)             │
│         ↓                                                    │
│    Build Docker images (tag: version)                      │
│         ↓                                                    │
│    Push to ECR                                              │
│         ↓                                                    │
│    Blue-Green Deployment:                                   │
│    ┌─────────────────────────────────────────────────────┐  │
│    │ 1. Deploy new version to Green environment         │  │
│    │ 2. Run health checks on Green                      │  │
│    │ 3. Switch traffic from Blue to Green               │  │
│    │ 4. Monitor for 30 minutes                          │  │
│    │ 5. If issues: Rollback to Blue                     │  │
│    │ 6. If stable: Terminate Blue                       │  │
│    └─────────────────────────────────────────────────────┘  │
│                                                              │
│ 5. Post-Deployment                                          │
│    • Monitor error rates (Sentry)                          │
│    • Monitor performance (DataDog)                         │
│    • Monitor AI costs                                       │
│    • Notify team via Slack                                  │
└─────────────────────────────────────────────────────────────┘
```

### **Infrastructure as Code**

```
Terraform Configuration:

modules/
├── vpc/                    # Network infrastructure
├── eks/                    # Kubernetes cluster
├── rds/                    # PostgreSQL database
├── mongodb-atlas/          # MongoDB cluster
├── elasticache/            # Redis cluster
├── s3/                     # Object storage
├── cloudfront/             # CDN distribution
├── iam/                    # Access management
├── monitoring/             # CloudWatch, alarms
└── security/               # WAF, security groups

Environments:
├── dev/                    # Development environment
│   ├── main.tf
│   ├── variables.tf
│   └── terraform.tfvars
├── staging/                # Staging environment
│   ├── main.tf
│   ├── variables.tf
│   └── terraform.tfvars
└── prod/                   # Production environment
    ├── main.tf
    ├── variables.tf
    └── terraform.tfvars

State Management:
- Backend: S3 bucket with DynamoDB locking
- Encryption: AES-256
- Versioning: Enabled
```

### **Container Orchestration**

```
Kubernetes (EKS) Configuration:

Namespaces:
- ielts-web        # Web application pods
- ielts-api        # API service pods
- ielts-workers    # Background job pods
- ielts-ai         # AI service pods
- monitoring       # Prometheus, Grafana

Deployments:
- web-app (3 replicas, HPA 3-20)
- auth-service (2 replicas, HPA 2-10)
- user-service (2 replicas, HPA 2-10)
- test-service (2 replicas, HPA 2-10)
- question-generator (2 replicas, HPA 2-15)
- evaluation-service (2 replicas, HPA 2-10)
- question-bank (2 replicas, HPA 2-10)

Services:
- Type: ClusterIP (internal communication)
- Type: LoadBalancer (external access)
- Type: NodePort (debugging)

Ingress:
- Controller: NGINX
- SSL: cert-manager (Let's Encrypt)
- Rate limiting: 100 req/min per IP
- WAF: AWS WAF integration

ConfigMaps & Secrets:
- ConfigMap: Application configuration
- Secret: Database credentials, API keys
- External Secrets: AWS Secrets Manager integration
```

---

## 📊 **Monitoring & Analytics**

### **Metrics Collection**

```
Business Metrics:
- Daily Active Users (DAU)
- Monthly Active Users (MAU)
- Tests taken per day
- Conversion rate (free → paid)
- Churn rate
- Average band score improvement
- Revenue per user

Technical Metrics:
- API response times (p50, p95, p99)
- Error rates by service
- Database query performance
- Cache hit/miss ratios
- Queue depths and processing times
- AI provider latency and costs
- Infrastructure utilization

User Experience Metrics:
- Page load times
- Time to first test
- Test completion rate
- Evaluation wait time satisfaction
- Feature adoption rates
- Support ticket volume
```

### **Alerting Configuration**

```
Critical Alerts (PagerDuty):
- API error rate > 5% for 5 minutes
- Database connection failures
- AI provider complete outage
- Payment processing failures
- Security incidents

Warning Alerts (Slack):
- API latency p95 > 1s for 10 minutes
- Queue depth > 100 for 15 minutes
- AI cost > 80% of daily budget
- Cache hit ratio < 70%
- Disk usage > 80%

Info Alerts (Slack #ops):
- Deployment completed
- Auto-scaling events
- Daily cost summary
- Weekly user growth
```

### **Analytics Dashboard**

```
Executive Dashboard:
┌─────────────────────────────────────────────────────────────┐
│ Platform Health                                              │
│                                                              │
│ Revenue: $59,970/month  ↑ 12%                              │
│ Active Users: 10,247    ↑ 8%                               │
│ Tests Today: 1,847      ↑ 15%                              │
│ Avg Band Score: 6.8     ↑ 0.3                              │
│                                                              │
│ System Status: 🟢 All Systems Operational                  │
│ API Latency p95: 245ms                                      │
│ Error Rate: 0.12%                                           │
│ AI Cost Today: $1,247 / $2,000 budget                      │
└─────────────────────────────────────────────────────────────┘

Engineering Dashboard:
┌─────────────────────────────────────────────────────────────┐
│ Service Performance                                          │
│                                                              │
│ Auth Service:     45ms avg  |  99.9% uptime  |  0 errors   │
│ User Service:     62ms avg  |  99.9% uptime  |  2 errors   │
│ Test Service:     120ms avg |  99.8% uptime  |  5 errors   │
│ Question Gen:     45s avg   |  99.5% uptime  |  12 errors  │
│ Evaluation:       65s avg   |  99.7% uptime  |  8 errors   │
│ Question Bank:    15ms avg  |  99.9% uptime  |  0 errors   │
│                                                              │
│ Database:                                                    │
│ PostgreSQL:  245 queries/sec  |  12ms avg latency          │
│ MongoDB:     189 queries/sec  |  8ms avg latency           │
│ Redis:       1,240 ops/sec    |  0.5ms avg latency         │
│                                                              │
│ Queue Depths:                                                │
│ question-generation:  23  |  evaluation:  45               │
│ audio-processing:     12  |  notification:  8              │
│ report-generation:    3   |  pre-generation:  156          │
└─────────────────────────────────────────────────────────────┘
```

---

## 💼 **Business Logic**

### **Pricing Strategy**

```
Freemium Model:

FREE TIER:
- 3 full tests per month
- Pre-generated content only
- Basic evaluation (band scores)
- Community support
- Ads supported

BASIC TIER ($19.99/month):
- 20 full tests per month
- Mix of pre-gen and on-demand
- Detailed AI feedback
- All 4 module scores
- Progress tracking
- Email support
- Downloadable reports
- No ads

PREMIUM TIER ($39.99/month):
- Unlimited tests
- Priority on-demand generation
- Advanced AI analysis
- Personalized study plans
- 1-on-1 tutor sessions (2/month)
- Priority support (24/7)
- Mock interview practice
- Guaranteed band improvement
- Early access to new features

ENTERPRISE (Custom pricing):
- White-label solution
- Custom question generation
- Dedicated support
- SLA guarantees
- API access
- Bulk user management
```

### **Revenue Model**

```
Revenue Streams:

1. Subscription Revenue (85%)
   - Monthly recurring revenue
   - Annual plans (17% discount)
   - Auto-renewal with reminder emails

2. One-Time Purchases (10%)
   - Additional test packs
   - Tutor session add-ons
   - Premium report exports

3. B2B Licensing (5%)
   - Language schools
   - Universities
   - Corporate training programs

Payment Processing:
- Primary: Stripe (cards, Apple Pay, Google Pay)
- Alternative: PayPal
- Currency: USD, EUR, GBP, AUD, CAD
- Billing cycle: Monthly or Annual
- Grace period: 3 days for failed payments
- Dunning management: Automated retry + email
```

### **User Retention Strategy**

```
Onboarding Optimization:
- Progressive profiling (3 steps)
- Optional diagnostic test
- Personalized study plan
- Welcome email series (5 emails over 2 weeks)

Engagement Features:
- Daily streak tracking
- Achievement badges
- Leaderboards (weekly/monthly)
- Push notifications (study reminders)
- Email digests (weekly progress)

Retention Interventions:
- Day 7: Check-in email if no test taken
- Day 14: Offer help + feature highlight
- Day 30: Progress review + milestone celebration
- Day 60: Upgrade prompt with discount
- Churn risk: Win-back campaign with special offer

Gamification:
- Points for completing tests
- Levels based on total study time
- Badges for achievements (perfect score, streak, improvement)
- Leaderboards by country/city
- Social sharing of results
```

---
## 🏦 **Pre-Generated Question Bank System**

### **System Overview**

The Pre-Generated Question Bank is a dedicated subsystem that maintains a large pool of AI-generated IELTS questions ready for instant allocation. This enables zero-wait-time test delivery for users while significantly reducing AI generation costs.

### **Key Benefits**

| Metric | On-Demand Only | With Pre-Gen Bank | Improvement |
|--------|---------------|-------------------|-------------|
| Test Start Time | 30-75 seconds | < 500ms | 99% faster |
| AI Cost per Test | $1.08 | $0.15 | 86% reduction |
| User Satisfaction | 7.2/10 | 9.1/10 | +26% |
| Concurrent Capacity | 500 tests/hr | 10,000 tests/hr | 20x |
| System Reliability | 99.5% | 99.95% | +0.45% |

### **Architecture**

```
┌─────────────────────────────────────────────────────────────┐
│              PRE-GENERATED QUESTION BANK SYSTEM              │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Question Bank Service (Port 3013)        │  │
│  │                                                       │  │
│  │  API Endpoints:                                       │  │
│  │  • GET /api/v1/bank/allocate                         │  │
│  │  • POST /api/v1/bank/release                         │  │
│  │  • GET /api/v1/bank/availability                     │  │
│  │  • POST /api/v1/bank/ingest                          │  │
│  │  • GET /api/v1/bank/stats                            │  │
│  │  • POST /api/v1/bank/deprecate                       │  │
│  │                                                       │  │
│  │  Core Components:                                     │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │  │
│  │  │  Allocator  │  │  Replenisher│  │  Quality    │  │  │
│  │  │  Engine     │  │  Engine     │  │  Scorer     │  │  │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │  │
│  │  │  Usage      │  │  Topic      │  │  Deprecation│  │  │
│  │  │  Tracker    │  │  Diversity  │  │  Manager    │  │  │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  │  │
│  └──────────────────────────────────────────────────────┘  │
│                           ↓                                  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Data Storage                              │  │
│  │                                                       │  │
│  │  PostgreSQL (Metadata):                               │  │
│  │  • pregen_question_sets table                        │  │
│  │  • Usage statistics                                   │  │
│  │  • Allocation tracking                                │  │
│  │                                                       │  │
│  │  MongoDB (Content):                                   │  │
│  │  • pregen_pool collection                            │  │
│  │  • Full question content                              │  │
│  │  • Audio file references                              │  │
│  │                                                       │  │
│  │  S3 (Media Files):                                    │  │
│  │  • pregen-audio/ bucket                              │  │
│  │  • Listening audio files                              │  │
│  │  • Chart images for writing                           │  │
│  │                                                       │  │
│  │  Redis (Cache):                                       │  │
│  │  • Available set lists                                │  │
│  │  • Hot sets (frequently accessed)                     │  │
│  └──────────────────────────────────────────────────────┘  │
│                           ↓                                  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Background Workers                        │  │
│  │                                                       │  │
│  │  • Pool Replenishment Worker                         │  │
│  │  • Quality Validation Worker                         │  │
│  │  • Deprecation Worker                                 │  │
│  │  • Analytics Aggregation Worker                      │  │
│  │                                                       │  │
│  │  Queue: pre-generation-queue (RabbitMQ)              │  │
│  │  Priority: Low (batch processing)                    │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### **Pool Configuration**

```
Target Pool Sizes (per module/test type/difficulty):

Listening:
┌─────────────────┬──────────┬──────────┬──────────┬──────────┐
│ Test Type       │ Band 5-6 │ Band 6-7 │ Band 7-8 │ Band 8-9 │
├─────────────────┼──────────┼──────────┼──────────┼──────────┤
│ Academic        │   150    │   300    │   250    │   100    │
│ General         │   150    │   300    │   250    │   100    │
└─────────────────┴──────────┴──────────┴──────────┴──────────┘
Total: 1,600 listening test sets

Reading:
┌─────────────────┬──────────┬──────────┬──────────┬──────────┐
│ Test Type       │ Band 5-6 │ Band 6-7 │ Band 7-8 │ Band 8-9 │
├─────────────────┼──────────┼──────────┼──────────┼──────────┤
│ Academic        │   150    │   300    │   250    │   100    │
│ General         │   150    │   300    │   250    │   100    │
└─────────────────┴──────────┴──────────┴──────────┴──────────┘
Total: 1,600 reading test sets

Writing:
┌─────────────────┬──────────┬──────────┬──────────┬──────────┐
│ Test Type       │ Band 5-6 │ Band 6-7 │ Band 7-8 │ Band 8-9 │
├─────────────────┼──────────┼──────────┼──────────┼──────────┤
│ Academic        │   100    │   200    │   150    │    50    │
│ General         │   100    │   200    │   150    │    50    │
└─────────────────┴──────────┴──────────┴──────────┴──────────┘
Total: 1,000 writing test sets

Speaking:
┌─────────────────┬──────────┬──────────┬──────────┬──────────┐
│ Test Type       │ Band 5-6 │ Band 6-7 │ Band 7-8 │ Band 8-9 │
├─────────────────┼──────────┼──────────┼──────────┼──────────┤
│ Academic        │   100    │   200    │   150    │    50    │
│ General         │   100    │   200    │   150    │    50    │
└─────────────────┴──────────┴──────────┴──────────┴──────────┘
Total: 1,000 speaking test sets

Grand Total: 5,200 pre-generated test sets
Estimated Storage: 50GB (MongoDB) + 200GB (S3 audio)
Estimated Generation Cost: $5,616 (one-time)
```

### **Allocation Algorithm**

```python
class QuestionBankAllocator:
    def allocate_test_set(self, user_id, module, test_type, difficulty):
        # Step 1: Query available sets from cache first
        cache_key = f"pregen:{module}:{test_type}:{difficulty}"
        available_sets = redis.get(cache_key)

        if not available_sets:
            available_sets = self.query_database(
                module, test_type, difficulty
            )
            redis.setex(cache_key, 300, available_sets)

        # Step 2: Get user's test history
        user_history = self.get_user_history(user_id, module)
        previously_used_sets = [h.set_id for h in user_history]
        previously_seen_topics = [h.topic for h in user_history]

        # Step 3: Score and rank candidates
        scored_candidates = []
        for set_id in available_sets:
            set_metadata = self.get_set_metadata(set_id)

            # Skip if user has already taken this set
            if set_id in previously_used_sets:
                continue

            # Calculate diversity score
            topic_similarity = self.calculate_topic_similarity(
                set_metadata.topic, previously_seen_topics
            )
            diversity_score = 1.0 - topic_similarity

            # Calculate freshness score
            days_since_used = (now - set_metadata.last_used_at).days
            freshness_score = min(days_since_used / 7.0, 1.0)

            # Calculate quality score
            quality_score = set_metadata.quality_score / 100.0

            # Calculate usage score (prefer less used sets)
            usage_ratio = set_metadata.usage_count / set_metadata.max_usage_limit
            usage_score = 1.0 - usage_ratio

            # Weighted composite score
            composite_score = (
                diversity_score * 0.35 +
                freshness_score * 0.25 +
                quality_score * 0.25 +
                usage_score * 0.15
            )

            scored_candidates.append({
                'set_id': set_id,
                'score': composite_score,
                'metadata': set_metadata
            })

        # Step 4: Select best match
        if not scored_candidates:
            return self.get_oldest_used_set(module, test_type, difficulty)

        scored_candidates.sort(key=lambda x: x['score'], reverse=True)
        selected = scored_candidates[0]

        # Step 5: Mark as allocated
        self.mark_allocated(selected['set_id'], user_id)

        # Step 6: Return test content
        test_content = self.fetch_test_content(selected['set_id'])

        return {
            'set_id': selected['set_id'],
            'content': test_content,
            'allocation_score': selected['score'],
            'generation_mode': 'pre_generated'
        }
```

### **Replenishment Strategy**

```
Trigger Conditions:
1. Scheduled: Every 6 hours
2. Threshold: Pool drops below 70% of target
3. Emergency: Pool drops below 50% of target
4. Quality: Average quality score < 85

Replenishment Process:

Step 1: Calculate Deficit
   For each (module, test_type, difficulty):
     target = configured_target_size
     current = count_available_sets()
     deficit = target - current

Step 2: Prioritize Generation
   Sort deficits by:
     1. Most popular combinations first
     2. Largest absolute deficit
     3. Lowest current quality score

Step 3: Batch Generate
   For each prioritized deficit:
     batch_size = min(deficit, 50)
     Submit jobs to pre-generation-queue
     Priority: Low (batch processing)

Step 4: Quality Validation
   As jobs complete:
     • Validate JSON structure
     • Check content quality (automated scoring)
     • Verify answer keys are correct
     • Check for duplicate content

Step 5: Ingest to Pool
   If validation passes:
     • Store content in MongoDB
     • Store metadata in PostgreSQL
     • Upload audio to S3 (if listening)
     • Update Redis cache
     • Mark as 'available'
   If validation fails:
     • Log failure reason
     • Retry with refined prompt (max 2 retries)
     • If still failing: Alert engineering team

Step 6: Update Metrics
   • Track generation cost
   • Update pool health dashboard
   • Alert if replenishment is behind schedule
```

### **Quality Scoring**

```python
class QualityScorer:
    def score_question_set(self, set_id):
        content = self.fetch_content(set_id)

        scores = {
            'content_validity': self.check_content_validity(content),
            'difficulty_calibration': self.check_difficulty(content),
            'answer_clarity': self.check_answer_clarity(content),
            'topic_originality': self.check_originality(content),
            'format_compliance': self.check_ielts_format(content),
            'language_quality': self.check_language_quality(content)
        }

        weights = {
            'content_validity': 0.25,
            'difficulty_calibration': 0.20,
            'answer_clarity': 0.20,
            'topic_originality': 0.15,
            'format_compliance': 0.10,
            'language_quality': 0.10
        }

        overall_score = sum(
            scores[k] * weights[k] for k in scores
        )

        return {
            'overall': round(overall_score * 100, 2),
            'breakdown': scores,
            'recommendation': 'accept' if overall_score >= 0.85 else 'review'
        }
```

### **Deprecation Policy**

```
Deprecation Rules:

Automatic Deprecation:
1. Usage Limit Reached
   - When usage_count >= max_usage_limit
   - Mark as 'deprecated'
   - Remove from allocation pool
   - Archive content after 30 days

2. Age-Based Deprecation
   - Sets older than 12 months
   - Quality score < 80 after re-validation
   - Mark as 'deprecated'
   - Schedule for regeneration

3. Topic Saturation
   - If topic appears > 5% of pool
   - Deprecate oldest sets of that topic
   - Prioritize diverse topics in replenishment

4. User Feedback
   - If > 3 user reports on a set
   - Immediate review
   - Deprecate if issues confirmed

Deprecation Process:
1. Mark status as 'deprecated' in PostgreSQL
2. Remove from Redis cache
3. Move content to archive collection in MongoDB
4. Delete audio files from S3 (after 30 days)
5. Log deprecation reason
6. Trigger replenishment for that slot
```

### **Usage Analytics**

```
Pre-Gen Bank Analytics Dashboard:

Pool Health:
Overall Pool Health: 87%

By Module:
• Listening:  1,420/1,600 (89%)
• Reading:    1,380/1,600 (86%)
• Writing:      890/1,000 (89%)
• Speaking:     850/1,000 (85%)

Quality Distribution:
• Excellent (90-100):  2,340 sets (45%)
• Good (80-89):        1,890 sets (36%)
• Acceptable (70-79):    310 sets (6%)
• Under Review (<70):     60 sets (1%)

Usage Statistics (Last 30 Days):
• Total allocations:     45,230
• Unique sets used:      3,890 (75% of pool)
• Average reuse count:   11.6x
• Most popular:          Band 6-7 Academic Listening
• Least popular:         Band 8-9 General Speaking

Cost Savings:
• AI costs saved:        $38,450 this month
• Pre-gen amortization:  $0.15 per test
• On-demand cost:        $1.08 per test
• Savings rate:          86%
```

### **Integration with Test Service**

```python
class TestService:
    def create_test_session(self, user_id, config):
        # Determine generation mode
        mode = self.determine_generation_mode(user_id, config)

        if mode == 'pre_generated':
            allocation = question_bank.allocate_test_set(
                user_id=user_id,
                module=config.module,
                test_type=config.test_type,
                difficulty=config.difficulty
            )

            test_session = TestSession.create(
                user_id=user_id,
                generation_mode='pre_generated',
                pregen_set_id=allocation['set_id'],
                status='ready',
                content=allocation['content']
            )

            return {
                'test_session_id': test_session.id,
                'status': 'ready',
                'estimated_time': 0,
                'message': 'Your test is ready!'
            }

        else:
            test_session = TestSession.create(
                user_id=user_id,
                generation_mode='on_demand',
                status='generating'
            )

            question_generator.submit_job(
                test_session_id=test_session.id,
                config=config
            )

            return {
                'test_session_id': test_session.id,
                'status': 'generating',
                'estimated_time': 60,
                'message': 'Generating your personalized test...'
            }

    def determine_generation_mode(self, user_id, config):
        user = UserService.get_user(user_id)
        subscription = SubscriptionService.get_subscription(user_id)

        # Free users: Always pre-generated
        if subscription.plan == 'free':
            return 'pre_generated'

        # Check if user specifically requested on-demand
        if config.generation_mode == 'personalized':
            return 'on_demand'

        # Check if user has weak areas that need targeting
        if user.weak_areas and config.focus_weak_areas:
            return 'on_demand'

        # Check pool availability
        availability = question_bank.check_availability(
            config.module, config.test_type, config.difficulty
        )

        if availability.count == 0:
            return 'on_demand'

        # Premium users: Default to on-demand, allow pre-gen
        if subscription.plan == 'premium':
            if config.generation_mode == 'instant':
                return 'pre_generated'
            return 'on_demand'

        # Basic users: Mix based on quota
        if subscription.tests_used_this_month < 5:
            return 'on_demand'
        else:
            return 'pre_generated'
```

### **Monitoring & Alerts**

```
Pre-Gen Bank Alerts:

Critical:
• Pool drops below 50% for any category
• Quality score average drops below 80
• Replenishment queue backlog > 500 jobs
• Allocation failure rate > 5%

Warning:
• Pool drops below 70% for any category
• Quality score average drops below 85
• Replenishment cost exceeds $500/day
• Sets approaching usage limit > 20%

Info:
• Replenishment batch completed
• New high-quality sets added
• Deprecation batch completed
• Monthly cost savings report
```

---

## 📋 **Appendix**

### **Technology Stack Summary**

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Frontend** | Next.js 14, React Native | Web & Mobile apps |
| **API Gateway** | Kong / AWS API Gateway | Request routing, rate limiting |
| **Load Balancer** | AWS ALB / Nginx | Traffic distribution |
| **CDN** | Cloudflare | Static asset delivery |
| **Services** | Node.js, Python FastAPI | Microservices |
| **Service Mesh** | Istio / Linkerd | Service communication |
| **Message Queue** | RabbitMQ / AWS SQS | Async processing |
| **Primary DB** | PostgreSQL 15 | Relational data |
| **Document DB** | MongoDB 6 | Question content |
| **Cache** | Redis 7 | Session, rate limiting |
| **Search** | Elasticsearch | Full-text search |
| **Vector DB** | Pinecone | Semantic search |
| **Object Storage** | AWS S3 | Audio, images, PDFs |
| **Container Orchestration** | EKS / ECS | Kubernetes |
| **IaC** | Terraform | Infrastructure management |
| **CI/CD** | GitHub Actions | Build & deploy |
| **Monitoring** | DataDog / Prometheus | Metrics & alerts |
| **Error Tracking** | Sentry | Error monitoring |
| **Logging** | ELK Stack | Log aggregation |
| **AI Providers** | OpenAI, Anthropic, Google | LLM services |
| **TTS** | ElevenLabs | Audio generation |
| **STT** | Whisper API | Speech-to-text |
| **Payments** | Stripe | Subscription billing |

### **Glossary**

| Term | Definition |
|------|-----------|
| **IELTS** | International English Language Testing System |
| **Band Score** | IELTS scoring scale (0-9) |
| **Pre-Generated** | Questions created in advance, ready for instant use |
| **On-Demand** | Questions generated in real-time per user request |
| **TTS** | Text-to-Speech |
| **STT** | Speech-to-Text |
| **JWT** | JSON Web Token |
| **RBAC** | Role-Based Access Control |
| **SLA** | Service Level Agreement |
| **HPA** | Horizontal Pod Autoscaler |

---

*Document Version: 2.0*
*Last Updated: 2026-05-20*
*Status: Complete*
