# Complete Beginner's Guide: OpenClaw in Docker with Browser Automation

> **Important Note**: "Node Host" is a concept **specific to OpenClaw**. It's not a universal term you'll find in other software. Other systems might call this "agent", "worker", "client", or "bridge" - but in OpenClaw, we call it "Node Host". This entire tutorial is about OpenClaw's specific architecture and terminology.

## How to Read This Tutorial

This tutorial is designed for someone who has **never used OpenClaw before**. Every concept is explained from scratch. If you see a term you don't understand, it will be explained before we use it.

**Tutorial Structure:**
1. First, we explain **every concept** you need to know
2. Then, we explain **why** we're doing each step
3. Finally, we show **how** to do it with commands

---

# Part 1: Understanding Every Concept

## 1.1 What is OpenClaw?

**OpenClaw** is software that lets you talk to an AI assistant through your favorite messaging apps (WhatsApp, Telegram, Discord, etc.) and have that AI do things for you - like browsing websites, running commands, editing files.

Think of it as having a personal assistant that lives on your computer and responds to your messages from anywhere.

### Real-World Example

Imagine this scenario:
1. You're at a coffee shop with just your phone
2. You send a WhatsApp message: "Check the price of Bitcoin on 3 different exchanges and tell me the average"
3. OpenClaw receives your message on your home computer
4. The AI opens websites, extracts prices, calculates average
5. It sends the result back to your WhatsApp

**That's what OpenClaw does.**

---

## 1.2 Universal vs OpenClaw-Specific Concepts

Before we dive in, it's important to understand which concepts are **universal** (used across the industry) and which are **OpenClaw-specific** (unique to this software):

### Universal Concepts (You'll See These Elsewhere)

| Concept | Where Else It's Used |
|---------|---------------------|
| Docker | Industry standard for containers |
| Container | Universal containerization concept |
| WebSocket | Standard web protocol (RFC 6455) |
| Browser Automation | Selenium, Puppeteer, Playwright |
| Browser Profile | Chrome, Firefox, Edge all have profiles |
| Chrome Extension | Chrome extension ecosystem |
| Authentication Token | Universal security concept |
| Port Binding | Networking concept |

### OpenClaw-Specific Concepts (Unique to OpenClaw)

| Concept | What It Is | Why OpenClaw Created It |
|---------|------------|------------------------|
| **Gateway** | OpenClaw's central hub | Combines WebSocket server, agent runner, session manager |
| **Node Host** | Bridge to local machine | Solves the Docker-can't-see-browser problem |
| **Agent** | OpenClaw's AI assistant | Custom implementation with tools and memory |
| **Session** | OpenClaw's conversation tracking | Specific session key format and management |
| **Workspace** | OpenClaw's file structure | Specific files like AGENTS.md, SOUL.md |
| **Device Pairing** | OpenClaw's security model | Specific approval flow for devices |
| **Control UI** | OpenClaw's dashboard | Custom web interface |

**Why This Matters:**
- Universal concepts transfer to other tools
- OpenClaw-specific concepts are unique to this system
- When you learn "Docker", that knowledge applies everywhere
- When you learn "Node Host", that's only for OpenClaw

---

## 1.3 What is a Gateway?

> **OpenClaw-Specific Concept**: While "gateway" is a common networking term, OpenClaw's Gateway is a specific program with unique features.

The **Gateway** is the heart of OpenClaw. It's a program that runs on your computer and:

- **Listens for messages** from WhatsApp, Telegram, Discord, etc.
- **Runs the AI agent** that processes your requests
- **Executes tools** like browser automation, file operations, commands
- **Sends responses** back to you

### Analogy: Gateway as a Reception Desk

Think of a hotel reception desk:
- People come in with requests (your messages)
- Receptionist coordinates everything (the Gateway)
- Receptionist calls the right department (AI, browser, file system)
- Response is delivered back to the person

**The Gateway is that reception desk for OpenClaw.**

### Gateway Technical Details

The Gateway is:
- A **WebSocket server** (listens for connections on port 18789)
- An **HTTP server** (serves the web dashboard)
- A **process manager** (runs the AI agent)
- A **session manager** (remembers conversation history)

---

## 1.3 What is Docker?

**Docker** is a way to package software so it runs the same way everywhere.

### The Problem Docker Solves

Without Docker:
- "It works on my machine" problems
- Different computers have different setups
- Installing dependencies can break other software
- Uninstalling leaves behind files

With Docker:
- Software runs in its own isolated "container"
- Same environment every time
- Easy to install, update, remove
- Doesn't affect your main system

### Docker Concepts Explained

| Term | What It Is | Simple Analogy |
|------|------------|----------------|
| **Image** | A template for creating containers | Like a recipe for a dish |
| **Container** | A running instance of an image | Like a prepared dish from the recipe |
| **Dockerfile** | Instructions to build an image | The written recipe |
| **Docker Compose** | Tool to run multiple containers | A meal plan with multiple dishes |

### Why We Use Docker for OpenClaw

1. **Isolation**: OpenClaw doesn't interfere with your other software
2. **Reproducibility**: Same setup works on any computer
3. **Easy Updates**: Pull new image, restart container
4. **Clean Removal**: Delete container, everything is gone

---

## 1.4 What is a Container?

A **container** is like a lightweight virtual machine. It's a self-contained environment with:
- Its own filesystem
- Its own installed software
- Its own network ports

### Container vs Virtual Machine

| Aspect | Virtual Machine | Container |
|--------|----------------|-----------|
| Boot time | Minutes | Seconds |
| Memory | Needs full OS | Shares host OS |
| Isolation | Complete | Process-level |
| Size | Gigabytes | Megabytes |

**Containers are lighter and faster than VMs but provide similar isolation.**

### OpenClaw in a Container

When OpenClaw runs in Docker:
- It has its own Node.js installation
- It has its own workspace directory
- It can't see your files unless you explicitly share them
- It can't use your browser unless you set up a connection

---

## 1.5 What is a Node Host?

> **OpenClaw-Specific Concept**: "Node Host" is terminology unique to OpenClaw. You won't find this term in Docker, Kubernetes, or other systems. It's OpenClaw's name for a program that bridges the Gateway to local resources.

A **Node Host** is a separate program that connects your local machine to a remote Gateway.

### The Problem: Docker Container Can't See Your Screen

When OpenClaw runs in Docker:
- It's isolated in a container
- It has no access to your monitor
- It can't launch your Chrome browser
- It can't see your screen

### The Solution: Node Host as a Bridge

A Node Host:
- Runs on your local machine (outside Docker)
- Connects to the Gateway (inside Docker) via WebSocket
- Receives commands from the Gateway
- Executes them on your local machine
- Sends results back to the Gateway

### Node Host Analogy: Remote Worker

Imagine:
- A manager (Gateway) in a main office
- A remote worker (Node Host) at home
- Manager sends tasks via chat
- Worker does the tasks on their home computer
- Worker reports back results

**The Node Host is that remote worker for OpenClaw.**

### Why OpenClaw Invented This Concept

Most AI assistants run entirely on your machine or entirely in the cloud. OpenClaw is unique because:

1. **Gateway can run anywhere** (Docker, VPS, cloud)
2. **But browser needs your machine** (for logins, visibility)
3. **Node Host bridges this gap**

Other systems solve this differently:
- **Browserless/Selenium**: Run browser on a server (headless, no logins)
- **Puppeteer/Playwright**: Run everything locally (no remote access)
- **OpenClaw's approach**: Split architecture with Node Host as the bridge

This is why Node Host is an OpenClaw-specific concept - it's the solution to OpenClaw's unique architecture problem.

### What Node Host Can Do

| Capability | Description |
|------------|-------------|
| **Browser control** | Open your Chrome, navigate, click, extract data |
| **Command execution** | Run terminal commands on your machine |
| **Screen recording** | Record your screen |
| **Camera access** | Take photos with your webcam |
| **Location access** | Get your GPS coordinates |

### Why We Need Node Host for Browser Automation

**Scenario**: You want the AI to crawl a website using YOUR browser (with your logins).

**Without Node Host:**
- Gateway in Docker can't see your browser
- Would need to run browser inside Docker (headless, no logins)
- Can't see what's happening

**With Node Host:**
- Gateway sends "open browser" command to Node Host
- Node Host launches YOUR Chrome
- You can see the browser window
- All your logins and sessions work
- AI can browse with your credentials

---

## 1.6 What is WebSocket?

**WebSocket** is a communication protocol that allows two-way communication between programs.

### Regular HTTP vs WebSocket

| HTTP | WebSocket |
|------|-----------|
| Request â Response | Two-way conversation |
| Client initiates | Either side can send |
| Short-lived | Long-lived connection |
| Like sending letters | Like a phone call |

### Why OpenClaw Uses WebSocket

The Gateway needs to:
- Receive messages from you (your request)
- Send progress updates (agent is working)
- Send the final response (here's the result)
- Send notifications (new message arrived)

WebSocket enables this real-time, two-way communication.

### WebSocket in Our Setup

```
âââââââââââââââ                âââââââââââââââ
â   Docker    â   WebSocket    â    Your     â
â   Gateway   âââââââââââââââââºâ  Node Host  â
â  (Server)   â   (Port 18789) â  (Client)   â
âââââââââââââââ                âââââââââââââââ
```

---

## 1.7 What is an Agent?

An **Agent** is the AI that processes your requests and takes actions.

### Agent vs Regular Chatbot

| Regular Chatbot | OpenClaw Agent |
|-----------------|----------------|
| Only responds with text | Can take actions |
| No memory between chats | Remembers context |
| Can't access files | Can read/write files |
| Can't browse websites | Can browse and interact |
| No tool access | Has many tools |

### What the Agent Can Do

1. **Read and write files** - Edit code, write documents
2. **Browse the web** - Open sites, click, extract data
3. **Run commands** - Execute terminal commands
4. **Search the web** - Look up information
5. **Send messages** - Reply to you on WhatsApp/Telegram/etc.

### How the Agent Works

```
Your Message â Agent receives it â Agent thinks â Agent uses tools â Agent responds
```

**Example Flow:**

1. You: "What's the weather in Tokyo?"
2. Agent thinks: "I need to search for weather"
3. Agent uses: `web_search` tool
4. Agent uses: `web_fetch` tool to get details
5. Agent responds: "It's 22Â°C and sunny in Tokyo"

---

## 1.8 What is a Session?

A **Session** is a conversation with the agent. It includes:
- All messages exchanged
- Files the agent has read
- Actions the agent has taken
- Memory of what was discussed

### Session Analogy: A Meeting

A session is like a meeting:
- Starts when you begin
- Everyone remembers what was said
- Ends when you leave
- Can be resumed later

### Session Key

Each session has a unique **session key** that identifies it:
- Main DM session: `agent:main:main`
- Group chat: `agent:main:whatsapp:group:12345`
- Cron job: `cron:daily-reminder`

### Why Sessions Matter

- **Continuity**: Agent remembers previous messages
- **Isolation**: Different chats don't mix
- **History**: You can review past conversations

---

## 1.9 What is a Workspace?

A **Workspace** is a folder where the agent works. It contains:

### Workspace Contents

```
~/.openclaw/workspace/
âââ AGENTS.md      # Instructions for the agent
âââ SOUL.md        # Agent's personality
âââ USER.md        # Information about you
âââ TOOLS.md       # Notes about tools
âââ IDENTITY.md    # Agent's name and emoji
âââ MEMORY.md      # Long-term memory
âââ memory/        # Daily memory files
    âââ 2024-01-15.md
```

### What Each File Does

| File | Purpose |
|------|---------|
| `AGENTS.md` | Rules and operating instructions |
| `SOUL.md` | Personality, tone, boundaries |
| `USER.md` | Who you are, how to address you |
| `TOOLS.md` | Custom tool instructions |
| `MEMORY.md` | Important things to remember |

### Why Workspace Matters

The workspace is the agent's "home":
- It reads instructions from these files
- It writes memories here
- It works on files here
- This is its context for every conversation

---

## 1.10 What is Browser Automation?

**Browser automation** means controlling a web browser programmatically - having software open websites, click buttons, fill forms, and extract data.

### Manual Browsing vs Automated Browsing

| Manual | Automated |
|--------|-----------|
| You click links | Script clicks links |
| You read pages | Script extracts text |
| You fill forms | Script fills forms |
| You remember data | Script saves data |

### Why Automate Browsing?

1. **Scale**: Check 100 pages instead of 1
2. **Speed**: Do in seconds what takes minutes
3. **Consistency**: Same actions every time
4. **Scheduling**: Run at 3 AM while you sleep

### OpenClaw's Browser Capabilities

| Action | What It Does |
|--------|--------------|
| `navigate` | Go to a URL |
| `click` | Click an element |
| `type` | Type text into inputs |
| `snapshot` | Read page content |
| `screenshot` | Take a picture of the page |
| `scroll` | Scroll the page |
| `wait` | Wait for something to appear |

---

## 1.11 What is a Browser Profile?

A **Browser Profile** is a separate browser instance with its own:
- Bookmarks
- History
- Cookies (login sessions)
- Extensions
- Settings

### Browser Profiles in OpenClaw

| Profile | Description |
|---------|-------------|
| `openclaw` | Clean profile, no personal data |
| `chrome` | Your existing Chrome via extension |

### When to Use Each Profile

**Use `openclaw` profile when:**
- Testing a clean browser
- Don't need your logins
- Want isolation from personal data

**Use `chrome` profile when:**
- Need your existing logins
- Want to use your extensions
- Working with sites you're already logged into

---

## 1.12 What is the Chrome Extension?

The **OpenClaw Chrome Extension** is a browser extension that lets OpenClaw control tabs in your existing Chrome browser.

### How It Works

1. You install the extension in Chrome
2. Open a tab you want OpenClaw to control
3. Click the extension icon to "attach" the tab
4. OpenClaw can now see and control that tab

### Why Use the Extension?

- Use your existing browser
- All your logins are preserved
- Can watch what the agent is doing
- Can intervene if needed

---

## 1.13 What is Device Pairing?

**Device Pairing** is a security mechanism. When a new device (like your Node Host) tries to connect to the Gateway, it must be explicitly approved.

### Why Pairing is Needed

Without pairing, anyone who knows your Gateway address could:
- Connect their device
- Run commands on your machine
- Access your browser
- Read your files

### Pairing Flow

```
1. Node Host connects to Gateway
2. Gateway creates a pending request
3. You (the owner) see the request
4. You approve or reject
5. If approved, device gets a token for future connections
```

### Pairing Commands

```bash
# See pending devices
openclaw devices list

# Approve a device
openclaw devices approve <request-id>

# Reject a device
openclaw devices reject <request-id>
```

---

## 1.14 What is Authentication Token?

A **Token** is like a password that proves you're authorized to access the Gateway.

### Token vs Password

| Aspect | Token | Password |
|--------|-------|----------|
| Length | Long random string | Usually shorter |
| Storage | File or env variable | Memory |
| Expiration | Can be set to never | Usually permanent |
| Revocation | Easy to revoke | Requires change |

### Where Tokens Are Used

1. **Gateway authentication** - Token to connect to Gateway
2. **Device authentication** - Token for paired devices
3. **Browser access** - Token for Control UI

### Token Security

- Keep tokens secret
- Don't commit to git
- Use environment variables
- Rotate if compromised

---

## 1.15 What is the Control UI?

The **Control UI** is a web dashboard for managing OpenClaw.

### Accessing Control UI

Open your browser and go to: `http://localhost:18789`

### What You Can Do in Control UI

| Tab | Function |
|-----|----------|
| **Chat** | Talk to the AI agent |
| **Channels** | Configure WhatsApp, Telegram, etc. |
| **Sessions** | View conversation history |
| **Cron** | Schedule automated tasks |
| **Nodes** | Manage connected devices |
| **Config** | Edit settings |
| **Logs** | View system logs |

### Why Control UI is Useful

- Visual interface instead of commands
- See all activity in one place
- Easy configuration
- Monitor browser actions
- Manage multiple sessions

---

## 1.16 What is Port Binding?

**Port Binding** determines who can connect to the Gateway.

### Binding Modes

| Mode | Who Can Connect |
|------|-----------------|
| `loopback` | Only your computer (localhost) |
| `lan` | Computers on your local network |
| `tailnet` | Computers on your Tailscale network |

### Why Binding Matters

- **Security**: Limit who can access your Gateway
- **Functionality**: Node Host needs to connect

### Choosing the Right Mode

```
If Gateway and Node Host are on same computer â loopback
If Node Host is on different computer (same network) â lan
If Node Host is remote (different network) â tailnet
```

---

## 1.17 What is Headless Mode?

**Headless mode** means running a browser without a visible window.

### Headed vs Headless

| Mode | Visible Window | Use Case |
|------|----------------|----------|
| Headed | Yes, you can see it | Interactive, debugging |
| Headless | No, invisible | Automation, servers |

### When to Use Each

**Headed (visible):**
- Watching what the agent does
- Debugging browser issues
- Helping with CAPTCHAs
- Using your existing sessions

**Headless (invisible):**
- Running on a server
- Background automation
- Not monitoring the agent

### For This Tutorial

We use **headed mode** because:
- You want to see the browser
- You want to use your logins
- You want to help if the agent gets stuck

---

## 1.18 What is CDP?

**CDP** (Chrome DevTools Protocol) is how OpenClaw controls Chrome/Chromium browsers.

### How CDP Works

1. OpenClaw connects to Chrome via CDP (usually port 9222)
2. Sends commands like "navigate to URL", "click element"
3. Chrome executes and responds
4. OpenClaw receives the result

### CDP Capabilities

- Navigate to URLs
- Click, type, scroll
- Read page content
- Take screenshots
- Execute JavaScript
- Monitor network requests

### CDP Ports in OpenClaw

| Profile | Default CDP Port |
|---------|------------------|
| `openclaw` | 18800 |
| `work` | 18801 |
| `chrome` | Uses extension relay |

---

# Part 2: Understanding the Architecture

Now that you understand all the concepts, let's see how they fit together.

## 2.1 Complete Architecture Diagram

```
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â                              YOUR INFRASTRUCTURE                            â
â                                                                             â
â  âââââââââââââââââââââââââââââââââââââââââââ                               â
â  â           DOCKER CONTAINER              â                               â
â  â                                         â                               â
â  â   âââââââââââââââââââââââââââââââââââ   â                               â
â  â   â          GATEWAY                â   â                               â
â  â   â                                 â   â                               â
â  â   â   â¢ Listens on port 18789      â   â                               â
â  â   â   â¢ Runs AI Agent              â   â                               â
â  â   â   â¢ Manages Sessions           â   â                               â
â  â   â   â¢ Coordinates Tools          â   â                               â
â  â   â   â¢ Handles Messages           â   â                               â
â  â   â                                 â   â                               â
â  â   â   Config: ~/.openclaw/         â   â                               â
â  â   â   Workspace: ~/.openclaw/       â   â                               â
â  â   â              workspace/         â   â                               â
â  â   ââââââââââââââââ¬âââââââââââââââââââ   â                               â
â  â                  â                       â                               â
â  â   ââââââââââââââââ¼âââââââââââââââââââ   â                               â
â  â   â        CONTROL UI               â   â                               â
â  â   â    http://localhost:18789       â   ââââââ Your Browser            â
â  â   â                                 â   â      (for management)         â
â  â   âââââââââââââââââââââââââââââââââââ   â                               â
â  â                                         â                               â
â  ââââââââââââââââââââââââ¬âââââââââââââââââââ                               â
â                         â                                                   â
â                         â WebSocket Connection                              â
â                         â (Bidirectional, Port 18789)                      â
â                         â                                                   â
â  ââââââââââââââââââââââââ¼âââââââââââââââââââ                               â
â  â          YOUR LOCAL MACHINE             â                               â
â  â                                         â                               â
â  â   âââââââââââââââââââââââââââââââââââ   â                               â
â  â   â          NODE HOST              â   â                               â
â  â   â                                 â   â                               â
â  â   â   â¢ Connects to Gateway         â   â                               â
â  â   â   â¢ Waits for commands          â   â                               â
â  â   â   â¢ Executes on local machine   â   â                               â
â  â   â   â¢ Returns results             â   â                               â
â  â   â                                 â   â                               â
â  â   â   Capabilities:                 â   â                               â
â  â   â   â¢ system.run (commands)       â   â                               â
â  â   â   â¢ browser.* (web control)     â   â                               â
â  â   â   â¢ camera.* (webcam)           â   â                               â
â  â   â   â¢ screen.* (recording)        â   â                               â
â  â   ââââââââââââââââ¬âââââââââââââââââââ   â                               â
â  â                  â                       â                               â
â  â   ââââââââââââââââ¼âââââââââââââââââââ   â                               â
â  â   â      CHROME / BRAVE BROWSER     â   â                               â
â  â   â                                 â   â                               â
â  â   â   â¢ Your existing profile       â   â                               â
â  â   â   â¢ Saved logins & sessions     â   â                               â
â  â   â   â¢ Extensions installed        â   â                               â
â  â   â   â¢ Visible window              â   â                               â
â  â   â                                 â   â                               â
â  â   â   OpenClaw Extension attached   â   â                               â
â  â   â   to specific tabs              â   â                               â
â  â   âââââââââââââââââââââââââââââââââââ   â                               â
â  â                                         â                               â
â  âââââââââââââââââââââââââââââââââââââââââââ                               â
â                                                                             â
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
```

## 2.2 How a Request Flows Through the System

Let's trace what happens when you ask the agent to crawl a website.

### Scenario: "Go to amazon.com and find the price of the first product"

```
STEP 1: You send the message
ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â  Your Phone (WhatsApp)                                           â
â  Message: "Go to amazon.com and find the price of first product" â
âââââââââââââââââââââââââââââ¬âââââââââââââââââââââââââââââââââââââââ
                            â
                            â¼
STEP 2: Message reaches Gateway
ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â  Docker Container                                                â
â  ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ  â
â  â Gateway receives message via WhatsApp connection           â  â
â  â Routes to correct agent based on sender                    â  â
â  ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ  â
âââââââââââââââââââââââââââââ¬âââââââââââââââââââââââââââââââââââââââ
                            â
                            â¼
STEP 3: Agent processes the request
ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â  Agent thinks:                                                   â
â  "I need to browse to amazon.com and extract product price"      â
â                                                                  â
â  Agent decides to use: browser tool                             â
âââââââââââââââââââââââââââââ¬âââââââââââââââââââââââââââââââââââââââ
                            â
                            â¼
STEP 4: Gateway sends browser command to Node Host
ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â  Gateway sends over WebSocket:                                   â
â  {                                                               â
â    "command": "browser.navigate",                                â
â    "params": { "url": "https://amazon.com" }                     â
â  }                                                               â
âââââââââââââââââââââââââââââ¬âââââââââââââââââââââââââââââââââââââââ
                            â
                            â¼
STEP 5: Node Host receives and executes
ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â  Node Host (on your local machine)                               â
â                                                                  â
â  1. Receives "navigate to amazon.com" command                    â
â  2. Sends to your Chrome via CDP                                 â
â  3. Chrome opens amazon.com                                      â
âââââââââââââââââââââââââââââ¬âââââââââââââââââââââââââââââââââââââââ
                            â
                            â¼
STEP 6: Browser performs the action
ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â  Your Chrome Browser                                             â
â                                                                  â
â  â¢ Opens amazon.com                                              â
â  â¢ Uses your existing login (if any)                             â
â  â¢ Page loads                                                    â
â  â¢ You see the browser window open                               â
âââââââââââââââââââââââââââââ¬âââââââââââââââââââââââââââââââââââââââ
                            â
                            â¼
STEP 7: More commands flow
ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â  Agent sends more commands:                                      â
â  â¢ browser.snapshot (read page content)                          â
â  â¢ browser.click (click on first product)                        â
â  â¢ browser.snapshot (read product page)                          â
â  â¢ Extract price from page content                               â
âââââââââââââââââââââââââââââ¬âââââââââââââââââââââââââââââââââââââââ
                            â
                            â¼
STEP 8: Response flows back
ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â  Agent formulates response:                                      â
â  "The first product is [Product Name] priced at $XX.XX"          â
â                                                                  â
â  Response goes back through:                                     â
â  Node Host â Gateway â WhatsApp â Your phone                     â
ââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
```

---

# Part 3: Why We're Doing Each Step

Now let's explain why we're setting things up this way.

## 3.1 Why Docker for the Gateway?

**Decision**: Run OpenClaw Gateway in Docker

**Reasons:**

1. **Isolation**: Gateway doesn't interfere with your other software
2. **Clean System**: No Node.js version conflicts
3. **Easy Reset**: Delete container, start fresh
4. **Server Ready**: Same setup works on VPS later

**Trade-off**: Gateway in Docker can't directly use your browser (we solve this with Node Host)

## 3.2 Why Node Host on Local Machine?

**Decision**: Run Node Host outside Docker, on your local machine

**Reasons:**

1. **Browser Access**: Node Host can launch and control your browser
2. **Your Logins**: Use your existing browser sessions
3. **Visibility**: See what the agent is doing in real-time
4. **Intervention**: Help with CAPTCHAs, login prompts

**Alternative Rejected**: Running browser inside Docker
- Headless only, can't see window
- No existing logins
- Anti-bot detection higher
- Harder to debug

## 3.3 Why Chrome Extension?

**Decision**: Use Chrome extension relay instead of separate browser profile

**Reasons:**

1. **Your Sessions**: Already logged into sites
2. **Your Extensions**: Password managers, ad blockers work
3. **Less Setup**: Don't need to configure new profile
4. **Familiar**: Use the browser you already know

**Alternative Rejected**: Separate `openclaw` browser profile
- Clean but no existing logins
- Have to log into everything again
- Miss out on password managers

## 3.4 Why WebSocket for Communication?

**Decision**: Use WebSocket between Gateway and Node Host

**Reasons:**

1. **Bidirectional**: Both sides can send messages
2. **Real-time**: No polling delay
3. **Efficient**: Single connection, not many HTTP requests
4. **Standard**: Works through firewalls, proxies

## 3.5 Why Port 18789?

**Decision**: Use port 18789 for Gateway

**Reasons:**

1. **Non-privileged**: Ports > 1024 don't require root
2. **Memorable**: Easy to remember
3. **Default**: Matches OpenClaw documentation
4. **Available**: Usually not used by other services

## 3.6 Why Device Pairing?

**Decision**: Require explicit approval for new devices

**Reasons:**

1. **Security**: Prevent unauthorized access
2. **Awareness**: Know what devices are connected
3. **Control**: Can revoke access anytime
4. **Audit**: Track connected devices

---

# Part 4: Step-by-Step Installation

Now we'll install everything, explaining each step as we go.

## Step 1: Install Docker

### What We're Installing
**Docker Desktop** - A program that runs containers on your computer.

### Why We Need It
Docker lets us run OpenClaw in an isolated environment without polluting your system.

### Installation Commands

**macOS:**
```bash
# Option 1: Download from website
# Go to: https://www.docker.com/products/docker-desktop
# Download Docker Desktop for Mac
# Install like any other Mac app

# Option 2: Using Homebrew
brew install --cask docker

# After installation:
# 1. Open Docker from Applications
# 2. Wait for it to start (whale icon in menu bar)
# 3. Docker is ready when the icon is steady
```

**Linux (Ubuntu/Debian):**
```bash
# Install Docker using the official script
curl -fsSL https://get.docker.com | sh

# Add your user to the docker group (so you don't need sudo)
sudo usermod -aG docker $USER

# Log out and log back in for group changes to take effect

# Start Docker service
sudo systemctl start docker
sudo systemctl enable docker
```

**Windows:**
```bash
# Download Docker Desktop for Windows from:
# https://www.docker.com/products/docker-desktop

# During installation:
# - Choose WSL 2 backend (better performance)
# - Restart when prompted

# After restart, Docker starts automatically
```

### Verify Docker is Working

```bash
# Check Docker version
docker --version
# Expected output: Docker version 24.x.x, build xxxxx

# Check Docker Compose
docker compose version
# Expected output: Docker Compose version v2.x.x

# Run a test container
docker run hello-world
# Expected: A welcome message from Docker
```

### What Just Happened
- Docker is now running on your computer
- You can create and run containers
- Docker Compose is installed (for managing multi-container apps)

---

## Step 2: Download OpenClaw

### What We're Doing
Downloading the OpenClaw source code from GitHub.

### Why We Need It
The source code includes:
- Docker configuration files
- Setup scripts
- Default configurations

### Commands

```bash
# Clone the repository
git clone https://github.com/openclaw/openclaw.git

# Enter the directory
cd openclaw

# See what's inside
ls -la
```

### What You'll See

```
openclaw/
âââ Dockerfile              # Instructions to build Docker image
âââ docker-compose.yml      # Configuration for running containers
âââ docker-setup.sh         # Setup script
âââ src/                    # Source code
âââ dist/                   # Compiled code
âââ ... other files
```

### What Just Happened
- Downloaded OpenClaw to your computer
- You're now in the openclaw directory
- Ready to build and run

---

## Step 3: Run the Setup Script

### What We're Doing
Running the automated setup script that builds and configures everything.

### Why We Need It
The script:
- Builds the Docker image
- Runs the onboarding wizard
- Creates configuration files
- Starts the gateway

### Command

```bash
# Make script executable (if needed)
chmod +x docker-setup.sh

# Run the setup
./docker-setup.sh
```

### What Happens During Setup

**Phase 1: Building Docker Image**
```
[+] Building 1.2m (15/15) FINISHED
 => [internal] load build definition
 => [internal] load .dockerignore
 => [internal] load metadata
 => [1/5] FROM node:22-bookworm
 => [2/5] WORKDIR /app
 => [3/5] COPY package*.json ./
 => [4/5] RUN npm install
 => [5/5] COPY . .
 => exporting to image
```

This takes a few minutes. Docker is:
- Downloading a base image (Node.js 22)
- Installing OpenClaw and dependencies
- Creating a ready-to-run image

**Phase 2: Onboarding Wizard**

The script will ask you questions:

```
? Choose your model provider:
  â¯ OpenAI (API Key)
    Anthropic (API Key)
    OpenAI Codex (OAuth)
    Google Gemini
    Custom Provider
    Skip (configure later)
```

Select your provider and follow the prompts.

```
? Enter your OpenAI API key: sk-xxxxxxxxxxxx
```

Enter your API key (it won't be shown on screen).

```
? Choose gateway binding:
  â¯ Loopback (localhost only)
    LAN (network access)
```

Choose `Loopback` for now (we'll use Node Host on the same machine).

**Phase 3: Configuration Creation**

The script creates:
- `~/.openclaw/openclaw.json` - Main configuration
- `~/.openclaw/workspace/` - Agent workspace with template files
- `.env` - Environment variables with tokens

**Phase 4: Starting the Gateway**

```
[+] Running 1/1
 â Network openclaw_default    Created
 â Container openclaw-gateway  Started
```

### Verify It Worked

```bash
# Check container is running
docker compose ps

# You should see:
# NAME                 STATUS    PORTS
# openclaw-gateway     running   0.0.0.0:18789->18789/tcp

# Check gateway health
curl http://localhost:18789/healthz
# Expected: OK
```

### What Just Happened
- OpenClaw is now running in Docker
- Gateway is listening on port 18789
- Ready to accept connections

---

## Step 4: Open the Control UI

### What We're Doing
Opening the web dashboard to verify OpenClaw is working.

### Why We Need It
The Control UI is your command center for:
- Chatting with the agent
- Configuring settings
- Viewing logs
- Managing sessions

### Commands

```bash
# Get dashboard URL with token
docker compose run --rm openclaw-cli dashboard --no-open

# This prints something like:
# http://127.0.0.1:18789/?token=xxxxx#token=xxxxx
```

Or simply open in your browser:
```
http://localhost:18789
```

### What You'll See

1. **First Time**: You'll be asked for a token
   - Get it from the command above
   - Or from `docker compose run --rm openclaw-cli config get gateway.auth.token`

2. **Control UI Tabs**:
   - **Chat**: Talk to the agent
   - **Channels**: Configure messaging apps
   - **Sessions**: View conversation history
   - **Cron**: Schedule tasks
   - **Nodes**: Manage connected devices
   - **Config**: Edit settings

3. **Pairing Required**: If connecting from a new browser, you need to approve it:
   ```bash
   docker compose run --rm openclaw-cli devices list
   docker compose run --rm openclaw-cli devices approve <request-id>
   ```

### What Just Happened
- You can now manage OpenClaw from your browser
- The Control UI is the main interface for configuration

---

## Step 5: Install OpenClaw CLI Locally

### What We're Doing
Installing the OpenClaw command-line tool on your local machine (outside Docker).

### Why We Need It
We need the CLI to run the **Node Host** - the bridge between Docker Gateway and your local browser.

### Commands

```bash
# Install using npm
npm install -g openclaw@latest

# Verify installation
openclaw --version
# Expected: 2026.x.x

# See available commands
openclaw --help
```

### What You'll See

```
openclaw <command>

Commands:
  openclaw onboard       Run the onboarding wizard
  openclaw gateway       Start the gateway
  openclaw browser       Browser control commands
  openclaw node          Node host commands
  openclaw devices       Device management
  openclaw config        Configuration management
  ...
```

### What Just Happened
- OpenClaw CLI is now installed on your machine
- You can run commands directly (not just through Docker)
- Ready to set up the Node Host

---

## Step 6: Get the Gateway Token

### What We're Doing
Retrieving the authentication token from the Gateway.

### Why We Need It
The Node Host needs to authenticate with the Gateway. The token proves it's authorized.

### Commands

```bash
# Get the token
docker compose run --rm openclaw-cli config get gateway.auth.token

# Output will be something like:
# "a1b2c3d4e5f6g7h8i9j0..."
```

### Important
- Copy this token somewhere safe
- You'll need it for the Node Host
- Don't share it with others

### What Just Happened
- You have the Gateway authentication token
- Ready to configure Node Host

---

## Step 7: Run the Node Host

### What We're Doing
Starting the Node Host program on your local machine.

### Why We Need It
The Node Host:
- Connects your local machine to the Docker Gateway
- Enables browser automation
- Executes commands on your machine

### Commands

```bash
# Run Node Host (replace TOKEN with your actual token)
openclaw node run --host 127.0.0.1 --port 18789 --token YOUR_TOKEN_HERE

# You'll see output like:
# Connecting to ws://127.0.0.1:18789...
# Waiting for pairing approval...
```

### What's Happening

1. **Node Host starts**
2. **Connects to Gateway** via WebSocket
3. **Gateway creates a pending device request**
4. **Node Host waits for your approval**

### What Just Happened
- Node Host is running and connected
- Waiting for you to approve the device

---

## Step 8: Approve the Node Device

### What We're Doing
Explicitly approving the Node Host to connect to your Gateway.

### Why We Need It
Security measure - no device can connect without your approval. This prevents unauthorized access to your machine.

### Commands

Open a new terminal (keep Node Host running in the first one):

```bash
# List pending devices
docker compose run --rm openclaw-cli devices list

# Output:
# PENDING DEVICES
# Request ID    Device ID         Role    Created
# abc123xyz     node-host-001     node    1 minute ago

# Approve using the Request ID
docker compose run --rm openclaw-cli devices approve abc123xyz

# Output:
# Device approved: node-host-001

# Verify it's now paired
docker compose run --rm openclaw-cli devices list

# Output:
# PAIRED DEVICES  
# Device ID       Role    Last Connected
# node-host-001   node    just now
```

### What Just Happened
- Node Host is now approved
- It can execute commands on your machine
- Ready to control your browser

---

## Step 9: Configure Browser

### What We're Doing
Setting up browser automation with your existing Chrome.

### Why We Need It
We want the agent to use YOUR Chrome browser (with your logins, sessions, extensions).

### Commands

```bash
# Check browser status
openclaw browser status

# If browser is disabled, enable it
docker compose run --rm openclaw-cli config set browser.enabled true --json

# Set default profile to chrome (uses your existing Chrome)
docker compose run --rm openclaw-cli config set browser.defaultProfile '"chrome"' --json

# Restart gateway to apply changes
docker compose restart openclaw-gateway
```

### Browser Profile Explanation

We chose `chrome` profile because:
- Uses your existing Chrome
- Your logins are preserved
- Your extensions work
- Can see the browser window

Alternative `openclaw` profile:
- Creates a new, clean browser
- No existing logins
- Isolated from your data

### What Just Happened
- Browser tool is enabled
- Configured to use your Chrome
- Ready for automation

---

## Step 10: Install Chrome Extension

### What We're Doing
Installing the OpenClaw extension in Chrome.

### Why We Need It
The extension lets OpenClaw control tabs in your Chrome browser. Without it, OpenClaw can't see or interact with your browser.

### Commands

```bash
# Get the path to the extension
openclaw browser extension path
# Output: /path/to/openclaw/extension

# Install the extension
openclaw browser extension install
```

### Manual Installation (if needed)

1. Open Chrome
2. Go to `chrome://extensions/`
3. Turn on "Developer mode" (top right toggle)
4. Click "Load unpacked"
5. Navigate to the extension path from the command above
6. Select the extension folder

### Verify Installation

1. You should see "OpenClaw" in your extensions list
2. The extension icon appears in your Chrome toolbar

### How to Use

1. Open a tab you want OpenClaw to control
2. Click the OpenClaw extension icon
3. The extension shows "Attached to tab"
4. OpenClaw can now control that tab

### What Just Happened
- Extension is installed in Chrome
- You can attach tabs for OpenClaw control
- Ready to test browser automation

---

# Part 5: Testing Your Setup

Now let's verify everything works.

## Test 1: Gateway Health Check

```bash
# Quick health check
curl http://localhost:18789/healthz

# Expected output:
# OK
```

**What this tests**: Gateway is running and responding.

## Test 2: Control UI Access

```bash
# Open Control UI
open http://localhost:18789
```

**What this tests**: You can access the management interface.

## Test 3: Node Host Connection

```bash
# Check node status
openclaw node status

# Expected output:
# Connected: true
# Device ID: node-host-001
# Role: node
```

**What this tests**: Node Host is properly connected to Gateway.

## Test 4: Browser Status

```bash
# Check browser status
openclaw browser status

# Expected output:
# Enabled: true
# Default Profile: chrome
```

**What this tests**: Browser tool is configured correctly.

## Test 5: Simple Browser Command

```bash
# Open a test page
openclaw browser open https://example.com

# Take a snapshot (read page content)
openclaw browser snapshot

# Expected output: The HTML/text content of the page
```

**What this tests**: Browser automation is working end-to-end.

## Test 6: Chat with Agent

1. Open Control UI: `http://localhost:18789`
2. Go to **Chat** tab
3. Send a message: "Open https://example.com and tell me what the page title is"
4. Watch the browser open and the agent respond

**What this tests**: Full end-to-end flow from message to browser action to response.

---

# Part 6: Summary

## What You Built

```
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â                     YOUR SETUP                              â
â                                                             â
â  âââââââââââââââââââââââ     âââââââââââââââââââââââââââ   â
â  â   Docker Container  â     â    Your Local Machine   â   â
â  â                     â     â                         â   â
â  â  âââââââââââââââââ  â     â  âââââââââââââââââââ   â   â
â  â  â   Gateway     ââââ¼âââââºâ  â   Node Host     â   â   â
â  â  â   (Port 18789)â  â WS  â  â                 â   â   â
â  â  âââââââââââââââââ  â     â  ââââââââââ¬âââââââââ   â   â
â  â                     â     â           â            â   â
â  â  âââââââââââââââââ  â     â  ââââââââââ¼âââââââââ   â   â
â  â  â  Control UI   â  â     â  â  Chrome Browser â   â   â
â  â  â  (Web Dashboard)â  â     â  â  (Your logins)  â   â   â
â  â  âââââââââââââââââ  â     â  âââââââââââââââââââ   â   â
â  â                     â     â                         â   â
â  âââââââââââââââââââââââ     âââââââââââââââââââââââââââ   â
â                                                             â
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
```

## What Each Component Does

| Component | Role | Why It's Needed |
|-----------|------|-----------------|
| **Docker** | Runs Gateway in isolation | Clean, reproducible environment |
| **Gateway** | Central hub for all operations | Coordinates messages, agent, tools |
| **Node Host** | Bridge to local machine | Enables browser access |
| **Chrome Extension** | Browser control interface | Lets Gateway control your Chrome |
| **Control UI** | Web dashboard | Easy management interface |

## How to Use It

1. **Keep Node Host running**: `openclaw node run ...`
2. **Open Control UI**: `http://localhost:18789`
3. **Chat with the agent**: Ask it to browse websites
4. **Attach browser tabs**: Click extension on tabs to control

## Common Commands

```bash
# Start Node Host
openclaw node run --host 127.0.0.1 --port 18789 --token TOKEN

# Check status
openclaw node status
openclaw browser status
docker compose ps

# View logs
docker compose logs -f openclaw-gateway

# Restart gateway
docker compose restart openclaw-gateway
```

## Next Steps

1. **Set up messaging channels** (WhatsApp, Telegram, Discord)
2. **Configure model provider** (OpenAI, Anthropic)
3. **Try web crawling** with the agent
4. **Explore Control UI** features

You now have a complete OpenClaw setup with browser automation capabilities!