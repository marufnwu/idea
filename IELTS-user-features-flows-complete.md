# IELTS Mock Test Platform - User Features & Flows Documentation

## Table of Contents

1. [User Personas & Roles](#user-personas--roles)
2. [Feature Access Matrix](#feature-access-matrix)
3. [Guest User Flows](#guest-user-flows)
4. [Free User Flows](#free-user-flows)
5. [Basic Subscriber Flows](#basic-subscriber-flows)
6. [Premium Subscriber Flows](#premium-subscriber-flows)
7. [Admin & Staff Flows](#admin--staff-flows)
8. [Tutor/Expert Flows](#tutorexpert-flows)
9. [B2B/Institutional Flows](#b2binstitutional-flows)
10. [Cross-Cutting Features](#cross-cutting-features)
11. [User State Management](#user-state-management)
12. [Notification Matrix](#notification-matrix)
13. [Mobile vs Web Feature Parity](#mobile-vs-web-feature-parity)

---

## User Personas & Roles

### Primary User Types

| Role ID | Name | Description | Authentication |
|---------|------|-------------|--------------|
| `guest` | Guest Visitor | Unauthenticated browser, limited preview access | None |
| `free` | Free Student | Registered user, no active subscription | Email/Social OAuth |
| `basic` | Basic Subscriber | Paid user on Basic plan ($19.99/mo) | JWT + Subscription |
| `premium` | Premium Subscriber | Paid user on Premium plan ($39.99/mo) | JWT + Subscription |
| `tutor` | Verified Tutor | Human IELTS tutor providing 1-on-1 sessions | JWT + Role Claim |
| `school_admin` | Institution Admin | Manages B2B organization accounts | JWT + Org Claim |
| `school_student` | Institution Student | User under B2B license | JWT + Org Claim |
| `content_mgr` | Content Manager | Manages question bank quality | JWT + Admin Role |
| `system_admin` | System Administrator | Full platform access | JWT + Super Admin |
| `analyst` | Data Analyst | Read-only analytics access | JWT + Analyst Role |
| `support` | Support Agent | Customer support tools | JWT + Support Role |

### Persona Details

**Persona: Sarah - The Free Student**
- Demographics: 22, university student, preparing for IELTS Academic
- Goals: Assess current level, get band score estimate
- Pain Points: Limited tests, no detailed feedback, can't afford subscription yet
- Tech Profile: Mobile-first, uses app during commute
- Conversion Trigger: Seeing detailed feedback preview after 2nd test

**Persona: David - The Basic Subscriber**
- Demographics: 28, professional, needs 7.0 for immigration
- Goals: Structured practice, track progress, identify weak areas
- Pain Points: Wants more personalized content, waiting for AI generation
- Tech Profile: Desktop for full tests, mobile for quick practice
- Upgrade Trigger: Hitting monthly test limit or wanting tutor feedback

**Persona: Emma - The Premium Subscriber**
- Demographics: 24, applying to top universities, needs 8.0+
- Goals: Perfect score, personalized study plan, speaking practice with tutor
- Pain Points: Needs advanced analysis, wants guaranteed improvement
- Tech Profile: Uses all features across devices, downloads reports
- Retention Risk: If band score plateaus despite premium features

**Persona: Mr. Chen - The Institution Admin**
- Demographics: 45, language school director, manages 200 students
- Goals: Monitor student progress, assign tests, bulk manage accounts
- Pain Points: Needs aggregated reporting, student management overhead
- Tech Profile: Desktop admin dashboard, exports reports
- Decision Factor: Bulk pricing and white-label options

---

## Feature Access Matrix

### Core Testing Features

| Feature | Guest | Free | Basic | Premium | Tutor | School Admin | School Student |
|---------|:-----:|:----:|:-----:|:-------:|:-----:|:------------:|:--------------:|
| **Test Taking** |
| Preview sample test (3 Qs) | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Full Listening test | No | Yes (3/mo) | Yes (20/mo) | Yes Unlimited | Yes | Yes | Yes |
| Full Reading test | No | Yes (3/mo) | Yes (20/mo) | Yes Unlimited | Yes | Yes | Yes |
| Full Writing test | No | Yes (3/mo) | Yes (20/mo) | Yes Unlimited | Yes | Yes | Yes |
| Full Speaking test | No | Yes (3/mo) | Yes (20/mo) | Yes Unlimited | Yes | Yes | Yes |
| Full IELTS simulation | No | Yes (3/mo) | Yes (20/mo) | Yes Unlimited | Yes | Yes | Yes |
| Module-only practice | No | Yes | Yes | Yes | Yes | Yes | Yes |
| **Question Generation** |
| Pre-generated questions | No | Yes Only | Yes | Yes | Yes | Yes | Yes |
| On-demand AI generation | No | No | Yes (5/mo) | Yes Unlimited | Yes | Yes | Yes |
| Smart routing | No | Yes | Yes | Yes | Yes | Yes | Yes |
| Difficulty selection | No | Yes Fixed | Yes | Yes | Yes | Yes | Yes |
| Academic/General choice | No | Yes | Yes | Yes | Yes | Yes | Yes |
| **Evaluation & Feedback** |
| Band score only | Yes Preview | Yes | Yes | Yes | Yes | Yes | Yes |
| Detailed AI feedback | No | No | Yes | Yes Advanced | Yes Manual | Yes | Yes |
| Section breakdown | No | No | Yes | Yes | Yes | Yes | Yes |
| Correct answer reveal | No | Yes | Yes | Yes | Yes | Yes | Yes |
| Audio snippet replay | No | Yes | Yes | Yes | Yes | Yes | Yes |
| Transcript view | No | No | Yes | Yes | Yes | Yes | Yes |
| Writing task rubric scoring | No | No | Yes | Yes (4 criteria) | Yes (Manual) | Yes | Yes |
| Speaking pronunciation analysis | No | No | Yes | Yes (Phoneme-level) | Yes (Manual) | Yes | Yes |
| **Progress & Analytics** |
| Test history | No | Last 3 | Full | Full | N/A | Org view | Own only |
| Band score trend chart | No | No | Yes 30-day | Yes Full history | N/A | Org view | Own only |
| Weak area identification | No | No | Yes | Yes (AI-powered) | Yes | Org view | Own only |
| Skill radar chart | No | No | Yes | Yes | N/A | Org view | Own only |
| Time analytics | No | No | Yes | Yes | N/A | Org view | Own only |
| Comparative percentile | No | No | Yes | Yes | N/A | Org view | Own only |
| **Study Tools** |
| Personalized study plan | No | No | No | Yes AI-generated | Yes Manual | Yes | Yes |
| Daily practice reminders | No | Yes Basic | Yes Smart | Yes AI-optimized | N/A | Yes | Yes |
| Vocabulary builder | No | No | Yes | Yes (AI-curated) | N/A | Yes | Yes |
| Grammar tips | No | No | Yes | Yes (Targeted) | N/A | Yes | Yes |
| Saved mistakes notebook | No | No | Yes | Yes | N/A | Yes | Yes |
| **Tutor Features** |
| Book 1-on-1 session | No | No | No | Yes (2/mo incl) | N/A | No | No |
| Mock interview practice | No | No | No | Yes | N/A | No | No |
| Essay review by human | No | No | No | Yes (1/mo incl) | Receives | No | No |
| Live speaking feedback | No | No | No | Yes | Provides | No | No |
| **Social & Gamification** |
| Achievement badges | No | Yes Basic | Yes All | Yes All | N/A | Yes | Yes |
| Leaderboards | No | Yes Weekly | Yes Weekly+Monthly | Yes All | N/A | Org only | Org only |
| Study streak | No | Yes | Yes | Yes | N/A | Yes | Yes |
| Share results | No | Yes Basic | Yes Detailed | Yes Detailed | N/A | Yes | Yes |
| Community forum access | No | Yes Read | Yes Post | Yes Post + Mentor | N/A | Yes | Yes |
| **Content & Reports** |
| Download PDF report | No | No | Yes | Yes (Branded) | N/A | Yes White-label | Yes |
| Download audio responses | No | No | No | Yes | N/A | No | No |
| Export progress CSV | No | No | No | Yes | N/A | Yes | No |
| **Account Management** |
| Profile customization | No | Yes | Yes | Yes | Yes | Yes | Yes |
| Notification preferences | No | Yes | Yes | Yes | Yes | Yes | Yes |
| Subscription management | No | Yes | Yes | Yes | N/A | Yes | No |
| Payment methods | No | No | Yes | Yes | N/A | Yes | No |
| **B2B/Institutional** |
| Create student accounts | No | No | No | No | N/A | Yes | No |
| Bulk test assignment | No | No | No | No | N/A | Yes | No |
| Organization analytics | No | No | No | No | N/A | Yes | No |
| White-label branding | No | No | No | No | N/A | Yes | No |
| API access | No | No | No | No | N/A | Yes | No |
| **Support** |
| Help center access | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Community support | No | Yes | Yes | Yes | Yes | Yes | Yes |
| Email support | No | No | Yes (48h) | Yes (Priority 4h) | N/A | Yes (Priority) | Yes |
| Live chat support | No | No | No | Yes 24/7 | N/A | Yes | No |
| **Admin Features** |
| User management | No | No | No | No | N/A | Yes | No |
| Question bank management | No | No | No | No | N/A | Yes | No |
| AI cost monitoring | No | No | No | No | N/A | Yes | No |
| System analytics | No | No | No | No | N/A | Yes | No |
| Content moderation | No | No | No | No | N/A | Yes | No |
| Prompt A/B testing | No | No | No | No | N/A | Yes | No |
| Financial reports | No | No | No | No | N/A | Yes | No |

---

## Guest User Flows

### FLOW G1: Landing Page to Sample Test

```
START: Anonymous user visits ielts-platform.com
    |
    v
[Landing Page - No login required]
  - Hero: "Master IELTS with AI"
  - Animated band score gauge (7.0 to 8.5)
  - Social proof: "50,000+ students improved their band"
  - Trust badges: Official IELTS alignment
  - [Start Free Practice] - Primary CTA
  - [View Pricing] - Secondary CTA
  - [Sign In] - Top right corner
    |
    v User clicks "Start Free Practice"
[Sample Test Modal - No registration required]
  "Try 3 sample questions - no account needed"
  Choose module to preview:
  [Listening] [Reading] [Writing] [Speaking]
  Backend: Serves pre-generated sample content
    - No AI generation (cost control)
    - Static content from sample bank
    - Same questions for all guests
    |
    v User selects Listening
[Sample Listening Experience (3 Questions)]
  Section 1 Preview:
  [Play Audio - 45 seconds]
  Question 1: Complete the form
  "Monthly rent: £ ______"
  [Input field - disabled after submit]
  [Submit Answer]
    |
    v User submits
[Instant Feedback (Teaser)]
  Correct! 1/1
  "Great start! Get detailed feedback on all 40 questions
   with a free account."
  [Continue to Q2] [Create Free Account]
  Backend:
    - Track guest interaction (anonymous cookie)
    - Store in Redis: guest_progress:{cookie_id}
    - TTL: 7 days
    |
    v After Q3
[Conversion Prompt]
  "You've completed the sample!"
  Your estimated band: 6.5 (based on 3 questions)
  To unlock:
    - Full 40-question tests
    - Detailed AI feedback
    - Band score prediction
    - Personalized study plan
  [Create Free Account - Takes 30 seconds]
  [Continue as Guest - Limited access]
  Backend:
    - Log conversion event
    - Store guest data for retargeting
    |
    v
END: Guest either converts or continues with limited access
```

### FLOW G2: Guest to Registration Conversion

```
Guest clicks "Create Free Account"
    |
    v
[Registration Modal (Inline, no page redirect)]
  Step 1/3: Quick Start
  "Continue your progress"
  - Your sample answers are saved!
  [Continue with Google] [Continue with Apple]
  [Continue with Facebook]
  ---------- or email ----------
  Email: [________________]
  Password: [________________]
  [Create Account]
  Backend:
    - If OAuth: Create user, link OAuth ID
    - If email: Validate format, check uniqueness
    - Hash password (bcrypt)
    - Generate JWT tokens
    - Migrate guest progress to user record
    |
    v
[Step 2/3: IELTS Profile (Optional skip)]
  "Help us personalize your experience"
  Target Band Score: [====●====] 7.0
  Test Date: [Calendar picker]
  Test Type: [o] Academic  [o] General Training
  [Skip for now] [Continue]
    |
    v
[Step 3/3: Welcome & First Test Offer]
  "Welcome, [Name]!"
  Your free plan includes:
    - 3 full tests per month
    - Basic band scores
    - Community support
  [Start Your First Full Test Now]
  [Go to Dashboard]
  Backend:
    - Create free subscription record
    - Send welcome email
    - Trigger onboarding analytics
    - Award "First Step" badge
    |
    v
END: User now has free role, redirected to dashboard
```

---

## Free User Flows

### FLOW F1: Free User Dashboard

```
User logs in (free tier)
    |
    v
[Free User Dashboard]
  Header: Welcome back, Sarah!
  Subscription Badge: FREE PLAN
  Tests remaining this month: 3/3
  [Start Test] [Progress] [Badges]
  Recent Activity:
    - Sample test completed - 2 days ago
    - Account created - 2 days ago
  [Upgrade Prompt]
    "Want detailed AI feedback on your mistakes?"
    "Upgrade to Basic for $19.99/month"
    [Compare Plans] [Upgrade Now]
  Community Highlights
  [Bottom Nav: Home | Tests | Community | Profile]
    |
    v
END: Dashboard loaded
```

### FLOW F2: Free User Taking a Test

```
Free user clicks "Start Test"
    |
    v
[Test Configuration (Free Tier Constraints Visible)]
  "Configure Your Test"
  Warning: Free Plan: Pre-generated tests only (instant start)
  Test Type:
    [o] Full IELTS Test (2h 45min)
    [o] Module Practice
      [_] Listening only
      [_] Reading only
      [_] Writing only
      [_] Speaking only
  Test Format:
    [o] Academic  [o] General Training
  Difficulty: [Auto-selected based on diagnostic]
    Band 5-6  [o] Band 6-7  [o] Band 7-8  [o] Band 8-9
    (Locked - upgrade to choose)
  [Cancel] [Start Test - Instant]
  Backend:
    - Check tests_used_this_month < 3
    - If limit reached: Show upgrade modal
    - Route to Question Bank Service (pre-gen only)
    - No on-demand option for free users
    |
    v
[Test In Progress - Listening Module]
  [Same interface as paid users]
  - Audio player
  - Question display
  - Auto-save to Redis
  - Timer running
  Backend:
    - Track time spent per question
    - Save draft answers
    - No AI involvement during test
    |
    v Test submitted
[Results Page (Free Tier - Limited)]
  Listening Test Complete!
  Your Band Score: 6.5
  Correct: 28/40 (70%)
  [Detailed Feedback Locked]
    "See exactly where you went wrong"
    - Section-by-section breakdown
    - Correct answers with explanations
    - Audio timestamps for listening
    - Time management analysis
    [Unlock with Basic Plan - $19.99/mo]
  [Continue to Reading] [View All Results]
  Backend:
    - Auto-grade (no AI cost)
    - Store band score only
    - Lock detailed_results field
    - Log "feedback_view_attempted" event
    |
    v
END: User sees score but detailed feedback is paywalled
```

### FLOW F3: Free User Hitting Monthly Limit

```
Free user attempts 4th test
    |
    v
[Limit Reached Modal]
  "You've used all 3 free tests this month"
  Your usage: 3/3 tests
  Resets on: June 1, 2026 (12 days)
  Options:
  [BASIC PLAN - $19.99/month]
    - 20 tests per month
    - Detailed AI feedback
    - Progress tracking
    [Upgrade Now]
  [PREMIUM PLAN - $39.99/month]
    - Unlimited tests
    - 1-on-1 tutor sessions
    - Personalized study plans
    [Go Premium]
  [Remind me later] [Set reminder for next month]
  Backend:
    - Block test creation: HTTP 403 + subscription error
    - Log "limit_reached" conversion event
    - Offer 20% discount if user has taken 2+ tests
    |
    v
END: User must upgrade or wait
```

---

## Basic Subscriber Flows

### FLOW B1: Basic User Onboarding After Upgrade

```
User upgrades from Free to Basic (Stripe checkout complete)
    |
    v
[Welcome to Basic Plan!]
  "Payment confirmed"
  Plan: Basic ($19.99/month)
  Next billing: June 20, 2026
  What's new for you:
    - 20 full tests per month (reset: 20/20)
    - Detailed AI feedback on every question
    - Downloadable PDF reports
    - Progress tracking & analytics
    - Email support (48h response)
  [Take Your First Basic Test Now]
  [Explore New Features]
  Backend:
    - Update subscription: status=active, plan=basic
    - Reset tests_used_this_month = 0
    - Grant access to detailed feedback
    - Send confirmation email
    - Award "Upgraded" badge
    - Trigger welcome push notification
    |
    v
[Feature Tour (First-time Basic)]
  Step 1/4: Detailed Feedback
  "Click any question to see: correct answer, explanation,
   and where in the audio to listen again"
  [Next] [Skip Tour]
  Step 2/4: Progress Dashboard
  "Track your band score improvement over time"
  Step 3/4: Weak Areas
  "We automatically identify topics you struggle with"
  Step 4/4: PDF Reports
  "Download professional reports for your tutor"
  [Finish Tour]
  Backend:
    - Store tour_completed flag
    - Track feature adoption analytics
    |
    v
END: Basic user fully onboarded
```

### FLOW B2: Basic User - Detailed Feedback Experience

```
Basic user submits Listening test
    |
    v
[Results Page (Basic Tier - Full Access)]
  Listening Test Complete!
  Your Band Score: 7.0
  [Animated gauge: 7.0/9.0]
  Performance Summary:
    - Correct: 30/40 (75%)
    - Time used: 38/40 minutes
    - Pace: Good
  Section Breakdown:
    Section 1: 8/10, 8 min, Trend: up
    Section 2: 9/10, 9 min, Trend: flat
    Section 3: 7/10, 10 min, Trend: down
    Section 4: 6/10, 11 min, Trend: down
  [View Detailed Breakdown]
    |
    v User expands Question 15
[Detailed Question Feedback]
  Question 15 (Section 2)
  Type: Multiple Choice
  Your Answer: A) Near the city center [X]
  Correct Answer: C) Close to the university [OK]
  [Audio Snippet 00:45 - 01:05]
  "...it's just a five-minute walk from the
   university campus, so very convenient for
   students..."
  [Play] [Slow 0.75x] [Transcript]
  Explanation:
  The speaker explicitly mentions proximity to the
  university ("five-minute walk from the university
  campus"). Option A (city center) is a distractor
  mentioned earlier but corrected.
  Related Vocabulary:
    - "campus" - university grounds
    - "convenient for" - indicating suitability
  [Mark for Review] [Add to Mistake Notebook]
  Backend:
    - Fetch detailed_results from PostgreSQL
    - Stream audio snippet from S3
    - Log question_view event
    - Check if user has seen similar question before
    |
    v
END: User has full feedback access
```

### FLOW B3: Basic User - Progress Analytics

```
User navigates to "Progress" tab
    |
    v
[Basic User Progress Dashboard]
  Band Score Trend (Last 30 Days)
    Listening: 7.0  up +0.5
    Reading: 6.5  flat 0.0
    Writing: 6.0  up +0.5
    Speaking: 6.5  down -0.5
    Overall: 6.5  up +0.3
  Weak Areas (AI-Identified)
    High Priority:
      - Listening Section 4 (Academic lectures) - 60%
      - Reading TF/NG questions - 55%
    Medium Priority:
      - Writing Task 1 data description - 65%
      - Speaking Part 3 abstract topics - 70%
    [Practice Weak Areas]
  Test History (Last 10 Tests)
    Date       | Type    | Overall | Time  | Action
    May 18     | Full    | 6.5     | 2h30m | [View]
    May 15     | Listen  | 7.0     | 40m   | [View]
    May 12     | Read    | 6.5     | 60m   | [View]
  [Download Progress Report PDF]
  Backend:
    - Aggregate from test_sessions + submissions
    - Calculate trends vs previous period
    - AI analysis of weak areas (cached, refreshed daily)
    - Check if user approaching test limit
    |
    v
END: Analytics displayed
```

---

## Premium Subscriber Flows

### FLOW P1: Premium Dashboard & AI Study Plan

```
Premium user logs in
    |
    v
[Premium User Dashboard]
  Header: Welcome back, Emma!
  Plan: Premium (Unlimited) | Tutor credits: 2 remaining
  Today's Focus (AI-Generated)
    "Based on your recent tests, focus on:"
    - Listening Section 4: Note-taking practice
    - Writing Task 2: Essay structure (30 min)
    - Vocabulary: Academic collocations (15 min)
    [Start Recommended Practice]
  Progress to Target
    Current: 7.5 --------●-------- Target: 8.5
    Test date: Aug 15, 2026 (87 days remaining)
    Predicted score if current pace: 8.2
    [View Study Plan]
  Quick Actions:
    [Full Test] [Module Practice] [Book Tutor]
    [Analytics] [Essay Review] [Mock Interview]
  Upcoming Tutor Session
    May 22, 2026 at 14:00 UTC - Speaking Practice
    with Tutor Michael
    [Join Session] [Reschedule]
  Backend:
    - Generate AI study plan (if not cached)
    - Check upcoming tutor bookings
    - Calculate prediction model score
    - Fetch unlimited usage status
    |
    v
```

### FLOW P2: Premium - AI Study Plan Generation

```
User clicks "View Study Plan"
    |
    v
[AI Study Plan Generator]
  Input Parameters:
    - Current level: 7.5 (from diagnostic + tests)
    - Target: 8.5
    - Target date: Aug 15, 2026
    - Days remaining: 87
    - Weak areas: Listening S4, Writing Task 2 structure
    - Available time: 2 hours/day
    - Recent performance trend: Improving +0.3/week
  [Generating personalized plan...]
  Backend to AI Orchestration Service:
    - Prompt: structured_study_plan_v3.2
    - Model: Claude Sonnet 4.5 (premium plan = best model)
    - Context: Full user history + weak areas + goals
    - Estimated generation: 8-12 seconds
  Response (JSON parsed):
    |
    v
[Personalized 12-Week Study Plan]
  Phase 1: Foundation (Weeks 1-4)
  Focus: Listening Section 4 & Writing structure
  Week 1 Schedule:
    Day      | Focus                       | Time
    Monday   | Listening S4 practice       | 45 min
               + Note-taking drills
    Tuesday  | Writing Task 2 outline      | 30 min
               + Sample essay analysis
    Wednesday| Full Listening test         | 40 min
               + Review mistakes           | 20 min
    Thursday | Vocabulary: Academic        | 30 min
               collocations
    Friday   | Writing Task 2 full essay   | 40 min
               + AI evaluation
    Saturday | Mock Speaking test          | 15 min
               + Self-review               | 15 min
    Sunday   | Rest / Light review         | 20 min
  Milestones:
    - Week 2: Achieve 8.0 in Listening
    - Week 4: Consistent 7.5+ in Writing Task 2
  [Accept Plan] [Customize] [Regenerate]
  Backend:
    - Store plan in PostgreSQL (study_plans table)
    - Cache in Redis for daily display
    - Set daily reminder notifications
    - Track plan adherence analytics
    |
    v
END: Study plan active
```

### FLOW P3: Premium - Tutor Session Booking

```
User clicks "Book Tutor"
    |
    v
[Tutor Marketplace]
  Filter: [o] All [o] Speaking [o] Writing [o] Reading
  Sort: Recommended | Rating | Price | Availability
  [Tutor Michael - 4.9 (128 reviews)]
    IELTS Examiner (8 years) | Band 9.0 native
    Specializes: Speaking, Writing
    Rate: Included in Premium (2/mo)
    Next available: Today 14:00, 16:00, 19:00
    [Book Session]
  [Tutor Sarah - 4.8 (89 reviews)]
    CELTA Certified | Band 8.5
    Specializes: Reading, Academic Writing
    Rate: Included in Premium
    Next available: Tomorrow 10:00, 15:00
    [Book Session]
  You have 2 tutor credits remaining this month
  [Purchase Additional Credits - $29.99 each]
  Backend:
    - Fetch verified tutors from database
    - Filter by availability (real-time calendar sync)
    - Check user's remaining credits
    - Calculate match score based on user's weak areas
    |
    v User selects Tutor Michael, 14:00 slot
[Session Booking Confirmation]
  Tutor: Michael
  Date: May 22, 2026
  Time: 14:00 UTC (Duration: 45 minutes)
  Type: Speaking Practice
  Focus Areas (auto-suggested from your weak areas):
    [_] Speaking Part 3 abstract topics
    [_] Fluency & coherence
    [_] Pronunciation
    [_] Custom topic: [________]
  [Confirm Booking]
  Backend:
    - Create calendar event
    - Deduct 1 tutor credit
    - Send confirmation email
    - Send reminder: 24h before, 1h before
    - Create video room link (Jitsi/Zoom)
    - Notify tutor
    |
    v
END: Session booked
```

### FLOW P4: Premium - Mock Interview Experience

```
User selects "Mock Interview" from dashboard
    |
    v
[IELTS Speaking Mock Interview Setup]
  "Simulate a real IELTS speaking test with AI"
  Configuration:
    - Duration: 11-14 minutes (authentic timing)
    - Parts: 1, 2, and 3 (full simulation)
    - Interviewer: AI with natural voice (ElevenLabs)
    - Recording: Enabled (for later review)
  Topic Selection:
    [o] Random topic (recommended)
    [o] Choose category:
      [_] Education & Work
      [_] Travel & Culture
      [_] Technology & Society
      [_] Environment & Health
  [Start Mock Interview]
  Backend:
    - Generate speaking questions (on-demand AI)
    - Pre-generate AI voice prompts
    - Initialize recording session
    - Create evaluation job queue entry
    |
    v
[Live Mock Interview Interface]
  [Camera Preview]        [Audio Level]
  AI Interviewer: "Good morning. My name is..."
  [Speaking...]
  Part 1: Introduction & Interview (4-5 min)
  Question 1/3: "Let's talk about where you live..."
  [Record Answer] [Pause] [End Interview]
  Recording: 00:23 ... [Live transcription appearing...]
  Backend:
    - Stream audio to server (WebRTC)
    - Real-time transcription (Whisper streaming)
    - Monitor audio quality
    - Track timing per part
    |
    v Interview complete
[Premium AI Evaluation Results]
  Mock Interview Complete! Duration: 13:42
  Overall Speaking Band: 7.5
  Detailed Criteria Breakdown (Official IELTS Rubric):
    Criterion          | Score | Analysis
    Fluency            | 8.0   | Natural pace, minimal
    & Coherence        |       | hesitation. Good use of
                         |       | connectors.
    Lexical Resource   | 7.5   | Good range, minor
                         |       | collocation errors.
    Grammatical Range  | 7.0   | Complex structures used
    & Accuracy         |       | but some errors in verb
                         |       | tenses.
    Pronunciation      | 7.5   | Clear, minor issues
                         |       | with word stress.
  [Play Recording] [View Phoneme Analysis]
  [Download Full Report] [Share with Tutor]
  Backend:
    - AI evaluation (Claude Sonnet 4.5)
    - Phoneme analysis (Whisper + custom model)
    - Store recording in S3 (encrypted)
    - Generate PDF report
    |
    v
END: Full premium evaluation complete
```

---

## Admin & Staff Flows

### FLOW A1: System Admin Login & Dashboard

```
Admin navigates to admin.ielts-platform.com
    |
    v
[Admin Login (Separate subdomain, enhanced security)]
  Email: [admin@platform.com]
  Password: [________________]
  2FA Code: [______] (Authy/Google Authenticator)
  [Sign In]
  Backend:
    - Validate credentials against admin_users table
    - Verify TOTP code
    - Check IP whitelist (if configured)
    - Log admin login (audit trail)
    - Issue short-lived JWT (30 min) + refresh token
    |
    v
[System Admin Dashboard]
  Platform Health
    Active Users (24h):  8,432    up 12%
    Tests Completed:     3,891    up 8%
    Revenue (MTD):      $52,400   up 15%
    AI Cost (Today):     $1,247    Warning 80% of budget
    System Status: All Operational
    API Error Rate: 0.12%
    DB Replication Lag: 23ms
  Quick Navigation:
    [User Management] [Question Bank] [AI Monitor]
    [Analytics] [Financial] [System Config]
    [Security Logs] [Announcements] [Support Tickets]
  Recent Alerts:
    Warning: AI cost at 80% - Consider enabling pre-gen priority
    Info: 3 new tutor applications pending verification
    OK: Daily backup completed successfully
    |
    v
END: Admin dashboard loaded
```

### FLOW A2: Content Manager - Question Bank Quality Review

```
Content Manager clicks "Question Bank"
    |
    v
[Question Bank Management]
  Tabs: [Overview] [Quality Review] [Pre-Gen Pool] [AI Logs]
  Quality Review Queue:
    Pending Review: 23 sets | Flagged: 7 | Approved: 4,892
    Filter: [o] All [o] Listening [o] Reading [o] Writing
            [o] Speaking  [o] Flagged  [o] Low Quality
    Sort by: Quality Score | Usage | Date | Reports
  Review Item #1:
    Set ID: listen-acad-67-2847
    Module: Listening | Type: Academic | Difficulty: Band 6-7
    Quality Score: 72/100 (Below threshold)
    Status: flagged_auto
    Issues Detected:
      Warning: Question 17: Ambiguous correct answer
        User reports: 3 | "Accepts both B and C"
      Warning: Audio quality: Background noise at 02:15
      Warning: Difficulty calibration: 90% users scored Band 8+
        (Too easy for Band 6-7 target)
    [View Full Content] [Edit] [Regenerate] [Deprecate] [Approve]
  Backend:
    - Fetch from MongoDB + PostgreSQL metadata
    - Load audio for playback
    - Show usage statistics
    - Display automated quality flags
    |
    v Manager clicks "Edit"
[Question Editor]
  Section 3, Question 17:
  Audio: [Play] [Waveform view]
  Timestamp: 02:15-02:45
  Question Text:
  "What is the professor's main concern about the proposal?"
  Current Answers:
    A) Cost [X]  B) Time [OK]  C) Feasibility [X]
  Issue: Audio actually supports both B and C
  Fix Options:
    [o] Modify correct answer to B and C
    [o] Edit question to be more specific
    [o] Regenerate audio segment
    [o] Deprecate entire question
  [Save Changes] [Request AI Regeneration] [Cancel]
  Backend:
    - Version control for question content
    - Track editor identity + timestamp
    - If regeneration: Submit to question-generation-queue
    - Update quality_score on save
    |
    v
END: Content updated
```

### FLOW A3: Support Agent - User Ticket Resolution

```
Support agent navigates to Support Tickets
    |
    v
[Support Ticket Queue]
  Filter: [o] All [o] Open [o] Pending [o] Resolved [o] Escalated
  Priority: [o] Critical [o] High [o] Medium [o] Low
  Ticket #4821 - OPEN - High Priority
    User: david.lee@email.com (Basic Plan)
    Subject: "Writing evaluation stuck for 3 hours"
    Created: 3 hours ago
    User Message:
      "I submitted my Writing Task 2 essay yesterday and it's
       still showing 'Evaluating...' for over 3 hours. This is
       very frustrating as I need feedback for my test tomorrow."
    [Internal Tools]
      [View User Profile] [View Test Session] [View Logs]
      [Impersonate User] [Refund/Compensate]
      System Status:
        - User subscription: Active (Basic)
        - Test session: #28491 - Status: evaluating
        - Evaluation job: #18932 - Status: FAILED (timeout)
        - AI provider: Claude - Status: Degraded
        - Last AI call: 3h ago, no response
        - Error: Provider timeout after 90s
    Quick Actions:
      [Retry Evaluation] [Refund Test Credit] [Escalate to Eng]
    Response to User:
      "Hi David, I sincerely apologize for the delay. Our AI
       evaluation service experienced a temporary issue. I've
       manually triggered a retry and added a free test credit
       to your account. You should receive results within 5
       minutes."
    [Send & Resolve] [Send & Keep Open] [Escalate]
  Backend:
    - Re-submit evaluation job to queue
    - Add compensation credit to user's account
    - Log support action in audit trail
    - Update ticket status
    |
    v
END: Ticket resolved
```

---

## Tutor/Expert Flows

### FLOW T1: Tutor Onboarding & Verification

```
Applicant signs up as Tutor
    |
    v
[Tutor Application Portal]
  Step 1: Credentials
    - IELTS Band Score: [9.0]
    - Teaching Certification: [CELTA/DELTA/Other]
    - Years of Experience: [8]
    - Native/Native-like: [OK]
  Step 2: Verification Documents
    - Upload IELTS certificate: [certificate.pdf]
    - Upload teaching cert: [celta.pdf]
    - ID Verification: [passport.pdf]
    - Video intro (2 min): [intro.mp4]
  Step 3: Availability & Specialization
    - Specialization: [x] Speaking [x] Writing [ ] Reading
    - Available hours: [Calendar picker]
    - Hourly rate: [$35] (platform takes 30%)
    - Languages spoken: [English, Mandarin]
  [Submit Application]
  Backend:
    - Store documents in S3 (private bucket)
    - Create tutor_application record
    - Status: pending_review
    - Notify admin team
    - Background check initiated (if required)
    |
    v Admin approves
[Tutor Dashboard]
  Welcome, Tutor Michael! Status: Verified
  This Month's Stats
    - Sessions completed: 24
    - Rating: 4.9/5.0
    - Earnings: $588 (after platform fee)
    - Upcoming sessions: 3
  Upcoming Sessions:
    - Today 14:00 - Emma (Premium) - Speaking Practice
    - Today 16:00 - James (Premium) - Writing Review
    - Tomorrow 10:00 - Lisa (Premium) - Mock Interview
  [Join Next Session] [View Calendar] [Student Notes]
  Quick Actions:
    [Set Availability] [Earnings] [Reviews] [Resources]
  Backend:
    - Fetch from tutor_sessions table
    - Calculate earnings (sessions x rate x 0.7)
    - Load student context before session
    - Check video room status
    |
    v
END: Tutor ready for sessions
```

---

## B2B/Institutional Flows

### FLOW I1: Institution Admin Onboarding

```
School director purchases Enterprise license
    |
    v
[Enterprise Onboarding]
  Organization: Chen's IELTS Academy
  License: Enterprise (200 students)
  Valid until: May 20, 2027
  Step 1: Branding Configuration
    - Organization name: [Chen's IELTS Academy]
    - Logo: [Upload logo.png]
    - Primary color: [#1E88E5]
    - Subdomain: [chens-academy.ielts-platform.com]
    - Custom domain: [ielts.chensacademy.com] (DNS setup req)
  Step 2: Student Management
    [Import from CSV] [Import from Google Classroom]
    [Add Students Manually]
    Bulk Import Preview:
      Name        | Email           | Level       | Status
      Wang Wei    | wei@student.com | Intermediate| Ready
      Li Na       | na@student.com  | Advanced    | Ready
      Zhang Ming  | ming@student.com| Beginner    | No email
    [Send Invitations] [Save Draft]
  Backend:
    - Create organization record
    - Generate student accounts (auto-password, force reset)
    - Apply white-label CSS theme
    - Configure subdomain routing
    - Send invitation emails
    - Create org_admin role for director
    |
    v
[Institution Admin Dashboard]
  Chen's IELTS Academy
  License: 200/200 students used
  Organization Performance (Last 30 Days)
    Active Students:  187/200 (94%)
    Tests Taken:        1,247
    Avg Band Score:     6.2  up +0.4 vs last month
    Completion Rate:    78%
    At-Risk Students:   12 (no activity > 14 days)
  Student Leaderboard:
    1. Wang Wei - 7.5 band (up 1.0) - 42 tests
    2. Li Na - 7.0 band (up 0.5) - 38 tests
    3. ...
  Actions:
    [Export Report] [Message All] [Assign Test]
    [Manage Students] [Settings] [Analytics]
  Backend:
    - Aggregate data across org users
    - Calculate at-risk students (ML model)
    - Generate white-label reports
    - Manage license allocation
    |
    v
END: Institution operational
```

### FLOW I2: Institution Student Experience

```
Student logs in via institution portal
    |
    v
[White-Label Student Dashboard]
  [Chen's IELTS Academy Logo]
  Welcome, Wang Wei!
  Announcement from your instructor:
    "Complete the Full Reading test by Friday. Focus on
     True/False/Not Given questions."
  Assigned Tests:
    Warning DUE: Full Reading Test (Academic)
      Deadline: May 24, 2026
      Status: Not started
      [Start Now]
  Your Progress:
    - Overall Band: 7.5 (up from 6.5 at start)
    - Class Rank: 3/187
    - Tests completed: 42/50 assigned
  [My Tests] [Class Rankings] [Messages] [Help]
  Backend:
    - Apply white-label theme (CSS injection)
    - Filter content to org-only (if configured)
    - Show instructor announcements
    - Track assignment completion
    - Calculate class rankings (privacy-compliant)
    |
    v
END: Student sees branded experience
```

---

## Cross-Cutting Features

### User Settings & Preferences Flow

```
Any authenticated user navigates to Settings
    |
    v
[User Settings Center]
  Tabs: [Profile] [Account] [Notifications] [Privacy] [Display]
  Profile Settings
    Profile Photo: [Image] [Change] [Remove]
    Full Name: [Emma Johnson]
    Email: [emma@email.com] [Verify if unverified]
    Country: [United Kingdom]
    Native Language: [English]
    Date of Birth: [1998-03-15]
    IELTS Profile:
      Target Band: [====●====] 8.5
      Target Date: [August 15, 2026]
      Test Type: [o] Academic [*] General Training
      Current Level: [Advanced]
    Weak Areas (self-reported):
      [x] Listening - Academic lectures
      [x] Writing - Task 2 structure
      [ ] Speaking - Part 3 fluency
    [Save Changes]
  Notification Preferences
    Email Notifications:
      [x] Test results ready
      [x] Weekly progress summary
      [x] Study reminder (daily at 09:00)
      [ ] Marketing & promotions
      [x] New features & updates
      [ ] Tutor session reminders
    Push Notifications (Mobile):
      [x] Study reminders
      [x] Test deadline alerts
      [x] Achievement unlocked
      [ ] Leaderboard updates
    [Save Preferences]
  Privacy & Data
    Data Visibility:
      [o] Private (only me)
      [o] Friends (share with connections)
      [*] Public (appear on leaderboards)
    Data Export: [Download My Data (GDPR)]
    Account Deletion: [Request Account Deletion]
    [Save Privacy Settings]
  Backend:
    - Update users table (PostgreSQL)
    - Invalidate cache (Redis)
    - If email changed: Re-verify required
    - If weak areas changed: Regenerate study plan
    - Log privacy changes (audit)
    |
    v
END: Settings saved
```

### Gamification & Achievement System

```
User earns achievement
    |
    v
[Achievement Unlocked!]
  [Animated badge: "First 7.0 Band" - gold medal]
  "Congratulations! You scored 7.0 in Listening - your
   first time reaching this milestone!"
  Reward: +50 XP | +1 Streak Day
  [Share on Twitter] [Share on Facebook] [Dismiss]
  Backend:
    - Insert into user_achievements table
    - Update user XP total
    - Check for chained achievements
    - Trigger push notification
    - Update leaderboard if public profile
    |
    v
[Achievement Categories]
  Band Score Milestones:
    - First 6.0, 6.5, 7.0, 7.5, 8.0, 8.5, 9.0
    - Perfect score in any module (40/40)
  Consistency:
    - 3-day streak, 7-day, 30-day, 100-day
    - Early bird (study before 8 AM)
    - Night owl (study after 10 PM)
  Module Mastery:
    - Listening Legend (50 listening tests)
    - Reading Master (50 reading tests)
    - Writing Wizard (50 writing tasks)
    - Speaking Star (50 speaking tests)
  Special:
    - First Upgrade (Basic)
    - Premium Member
    - Feedback Provider (report question issue)
    - Community Helper (answer forum question)
  Backend:
    - Check triggers after every test submission
    - Batch check daily for streaks
    - Award XP: band_milestone=100, streak=10/day, etc.
    |
    v
END: Achievement recorded
```

---

## User State Management

### User Status Lifecycle

```
                              [GUEST]
                            (anonymous)
                                |
                             Register
                                |
                                v
                              [FREE]
                            (registered)
                                |
            Upgrade to Basic    |    Upgrade to Premium
                |               |               |
                v               |               v
            [BASIC]             |           [PREMIUM]
          (subscribed)          |         (subscribed)
                |               |               |
                |    Upgrade    |    Downgrade  |
                +---------------+---------------+
                                |
                             Cancel / Expire
                                |
                                v
                            [EXPIRED]
                          (grace: 3d)
                                |
                 Renew within   |   No renewal
                    grace       |       |
                      |         |       |
                      v         |       v
                   [ACTIVE]      |    [FREE]
                 (restored)     |   (reverted)
                                |

Special States:
  [SUSPENDED]      [BANNED]         [INACTIVE]
  (payment         (ToS             (no login
   failed)          violation)       90+ days)
      |                |                |
      | Retry payment  | Appeal         | Re-engagement
      v                v                v
   [ACTIVE]        [REVIEW]         [ACTIVE]
  (restored)      (pending)       (restored)
```

### State Transition Rules

| From | To | Trigger | Backend Action |
|------|-----|---------|----------------|
| Guest | Free | Email verification complete | Create user record, assign free role |
| Free | Basic | Stripe payment success | Update subscription, reset test counter, grant features |
| Free | Premium | Stripe payment success | Update subscription, unlock all features, assign priority |
| Basic | Premium | Upgrade payment | Prorate remaining Basic time, upgrade immediately |
| Premium | Basic | Downgrade request | Schedule at period end, notify user, maintain Premium until end |
| Basic | Free | Subscription expires | Revoke features, reset to free limits, send win-back email |
| Premium | Free | Subscription expires | Revoke all premium features, preserve data (read-only history) |
| Any | Suspended | Payment failure (3 retries) | Block test creation, send payment retry email, grace period 3 days |
| Suspended | Active | Payment retry success | Restore full access, send confirmation |
| Suspended | Free | Grace period expires | Downgrade to free, preserve data |
| Any | Banned | ToS violation detected | Immediate session invalidation, block login, preserve data 30 days |
| Free | Inactive | No login 90 days | Send re-engagement email, reduce email frequency |
| Inactive | Active | User logs in | Restore normal operation, reset inactivity timer |

---

## Notification Matrix

### Notification Triggers by User Type

| Trigger | Guest | Free | Basic | Premium | School Admin | School Student |
|---------|:-----:|:----:|:-----:|:-------:|:------------:|:--------------:|
| **Test-Related** |
| Test generation complete | N/A | Yes | Yes | Yes | N/A | Yes |
| Evaluation complete | N/A | Yes | Yes | Yes | N/A | Yes |
| Test abandoned (timeout) | N/A | Yes | Yes | Yes | N/A | Yes |
| Test limit warning (2/3 used) | N/A | Yes | No | No | N/A | N/A |
| Test limit reached | N/A | Yes | No | No | N/A | N/A |
| Test limit warning (15/20) | N/A | N/A | Yes | No | N/A | N/A |
| **Study & Engagement** |
| Daily study reminder | N/A | Yes | Yes | Yes | N/A | Yes |
| Streak about to break | N/A | Yes | Yes | Yes | N/A | Yes |
| Weekly progress summary | N/A | No | Yes | Yes | N/A | Yes |
| Study plan milestone | N/A | No | No | Yes | N/A | Yes |
| Weak area practice suggestion | N/A | No | Yes | Yes | N/A | Yes |
| **Tutor & Premium** |
| Tutor session booked | N/A | N/A | N/A | Yes | N/A | No |
| Tutor session reminder (24h) | N/A | N/A | N/A | Yes | N/A | No |
| Tutor session reminder (1h) | N/A | N/A | N/A | Yes | N/A | No |
| Tutor session starting now | N/A | N/A | N/A | Yes | N/A | No |
| Essay review complete | N/A | N/A | N/A | Yes | N/A | No |
| **Subscription & Billing** |
| Subscription confirmation | N/A | N/A | Yes | Yes | Yes | N/A |
| Payment receipt | N/A | N/A | Yes | Yes | Yes | N/A |
| Upcoming renewal (7 days) | N/A | N/A | Yes | Yes | Yes | N/A |
| Payment failed | N/A | N/A | Yes | Yes | Yes | N/A |
| Subscription expired | N/A | N/A | Yes | Yes | Yes | N/A |
| **B2B/Institutional** |
| Student joined organization | N/A | N/A | N/A | N/A | Yes | N/A |
| Student test completed | N/A | N/A | N/A | N/A | Yes | N/A |
| Assignment deadline approaching | N/A | N/A | N/A | N/A | Yes | Yes |
| Weekly org report | N/A | N/A | N/A | N/A | Yes | N/A |
| **System & Security** |
| Password changed | N/A | Yes | Yes | Yes | Yes | Yes |
| New login from unknown device | N/A | Yes | Yes | Yes | Yes | Yes |
| Email verification required | N/A | Yes | Yes | Yes | Yes | Yes |
| Data export ready | N/A | Yes | Yes | Yes | Yes | Yes |
| **Marketing (Opt-in)** |
| New feature announcement | N/A | Yes | Yes | Yes | Yes | Yes |
| Special promotion | N/A | Yes | Yes | Yes | Yes | Yes |
| Win-back offer (inactive 30d) | N/A | Yes | Yes | Yes | N/A | Yes |
| Referral program invite | N/A | Yes | Yes | Yes | N/A | Yes |

### Notification Channels by Priority

| Priority | Channels | Example |
|----------|----------|---------|
| Critical | Push + Email + SMS | Payment failed, Security alert |
| High | Push + Email | Evaluation complete, Tutor session reminder |
| Medium | Email | Weekly summary, Progress milestone |
| Low | In-app only | Badge earned, Leaderboard update |
| Batch | Email digest | Daily study reminders, Marketing |

---

## Mobile vs Web Feature Parity

### Feature Availability Matrix

| Feature | Web (Desktop) | Web (Mobile) | iOS App | Android App | Notes |
|---------|:-------------:|:------------:|:-------:|:-----------:|-------|
| **Test Taking** |
| Full Listening test | Yes | Yes | Yes | Yes | Audio optimized per platform |
| Full Reading test | Yes | Yes | Yes | Yes | Scroll-optimized for mobile |
| Full Writing test | Yes | Warning Small screen | Yes | Yes | Mobile: Simplified input |
| Full Speaking test | Yes | Warning Browser mic | Yes | Yes | App: Better audio quality |
| Module practice | Yes | Yes | Yes | Yes | |
| **Audio Features** |
| Audio playback | Yes | Yes | Yes | Yes | Mobile app: Background audio |
| Audio download | Yes | No | Yes | Yes | Mobile app: Offline listening |
| Speed control (0.75x-1.5x) | Yes | Yes | Yes | Yes | |
| **Input Methods** |
| Keyboard typing | Yes | Warning Virtual | Yes | Yes | |
| Voice input (Speaking) | Yes | Yes | Yes | Yes | App: Native speech APIs |
| File upload (Writing) | Yes | Warning Limited | Yes | Yes | |
| **Offline Features** |
| Take test offline | No | No | Yes | Yes | Downloaded pre-gen content |
| Sync when online | N/A | N/A | Yes | Yes | Auto-sync on connection |
| Downloaded content | No | No | Yes | Yes | 10 test limit offline |
| **Notifications** |
| Push notifications | No | No | Yes | Yes | Rich push with actions |
| Email notifications | Yes | Yes | Yes | Yes | |
| In-app notifications | Yes | Yes | Yes | Yes | |
| **Social & Sharing** |
| Share results | Yes | Yes | Yes | Yes | Native share sheets on app |
| Community forum | Yes | Yes | Yes | Yes | Mobile-optimized UI |
| Leaderboards | Yes | Yes | Yes | Yes | |
| **Account Management** |
| Subscription mgmt | Yes | Yes | Yes | Yes | In-app purchases on mobile |
| Profile editing | Yes | Yes | Yes | Yes | |
| Payment methods | Yes | Yes | Yes | Yes | Apple Pay / Google Pay on app |
| **Premium Features** |
| Tutor video session | Yes | Warning Browser | Yes | Yes | App: Better video stability |
| Screen sharing (Tutor) | Yes | No | No | No | Desktop only |
| PDF report download | Yes | Warning Limited | Yes | Yes | Mobile: View in app, share PDF |
| **Admin Features** |
| Admin dashboard | Yes | No | No | No | Desktop only |
| Content editing | Yes | No | No | No | Desktop only |
| Analytics viewing | Yes | Warning Read-only | No | No | Mobile web: View only |

### Mobile-Specific Optimizations

```
Mobile App (iOS/Android) Special Features:

1. Native Audio Processing
   - Background audio playback
   - Audio ducking during voice recording
   - Bluetooth headphone optimization
   - AirPlay / Chromecast support

2. Offline Mode
   - Download 10 pre-generated tests
   - Store in local SQLite database
   - Auto-sync results when online
   - Queue evaluation requests

3. Push Notifications
   - Rich notifications with actions
   - "Start Test" action from reminder
   - Progress widget (iOS 16+ / Android 12+)
   - Live activities for test timer (iOS)

4. Voice & Speech
   - Native Speech-to-Text (iOS Speech / Android Speech)
   - Voice recording with noise cancellation
   - Pronunciation waveform visualization

5. Performance
   - Image caching (Glide/Picasso)
   - Lazy loading for question lists
   - Skeleton screens during loading
   - Reduced motion option (accessibility)

6. Accessibility
   - VoiceOver / TalkBack support
   - Dynamic type support
   - High contrast mode
   - Reduce transparency
```

---

## UI/UX Component Specifications by User Type

### Dashboard Component Variants

```
Dashboard Header Variants:

FREE USER:
  Welcome back!              FREE PLAN | 2/3 tests left
                                          [Upgrade]

BASIC USER:
  Welcome back!              BASIC | 12/20 tests left
                                          [Manage]

PREMIUM USER:
  Welcome back, Emma!        PREMIUM | Unlimited
  Tutor credits: 2          [Book Session] [Manage]

INSTITUTION STUDENT:
  [School Logo] Welcome, Wang!    2 Assignments
                                          [View]

TUTOR:
  Welcome, Tutor Michael!    4.9 | Next: 14:00
                              [Join Session] [Calendar]
```

### Test Button States

```
"Start Test" Button Variants by Context:

ENABLED (Free, has tests left):
  [Start Test - Instant] (Green, primary)

ENABLED (Basic, has tests left):
  [Start Test] with dropdown:
    - Instant (Pre-generated)
    - Personalized (AI-Generated)

ENABLED (Premium):
  [Start Test] with dropdown:
    - Instant (Pre-generated)
    - Personalized (AI-Generated)
    - Mock Interview (Live AI)

DISABLED (Free, limit reached):
  [Start Test - Upgrade Required] (Gray, with lock icon)
  On click: Show upgrade modal

DISABLED (Basic, limit reached):
  [Start Test - Limit Reached] (Gray)
  On click: Show upgrade to Premium modal

LOADING:
  [Generating your test... 45s remaining] (Animated)
  Progress bar + cancel option
```

---

## Permission & Access Control Implementation

### RBAC (Role-Based Access Control) Matrix

```
Resources & Actions:

Resource           | Create  | Read   | Update   | Delete
-------------------|---------|--------|----------|---------
own_profile        | -       | all    | all      | own_only
own_tests          | all     | all    | draft    | own_only
own_results        | -       | all    | -        | -
own_subscriptions  | -       | all    | cancel   | -
own_study_plan     | -       | all    | -        | -
pregen_content     | -       | all    | -        | -
ondemand_content   | basic+  | all    | -        | -
tutor_sessions     | premium | all    | reschedule| own_only
community_posts    | basic+  | all    | own_only | own_only
community_comments | all     | all    | own_only | own_only
user_management    | admin   | admin  | admin    | admin
question_bank      | admin   | admin  | admin    | admin
ai_configuration   | admin   | admin  | admin    | admin
financial_data     | admin   | admin  | -        | -
system_logs        | admin   | admin  | -        | -
org_students       | school  | school | school   | school
org_analytics      | school  | school | -        | -
org_assignments    | school  | school | school   | school

Middleware Implementation (Node.js/Express):

const permissions = {
  free: {
    tests: { create: 3, read: 'own', update: 'draft' },
    feedback: { read: 'band_only' },
    study_plan: { read: false },
    tutor: { create: false }
  },
  basic: {
    tests: { create: 20, read: 'own', update: 'draft' },
    feedback: { read: 'detailed' },
    study_plan: { read: false },
    tutor: { create: false }
  },
  premium: {
    tests: { create: Infinity, read: 'own', update: 'draft' },
    feedback: { read: 'advanced' },
    study_plan: { create: true, read: 'own', update: true },
    tutor: { create: 2 } // per month
  }
};

function checkPermission(user, resource, action) {
  const role = user.subscription.plan;
  const limit = permissions[role][resource]?.[action];

  if (limit === false) return { allowed: false, reason: 'Feature not available' };
  if (limit === Infinity) return { allowed: true };
  if (typeof limit === 'number') {
    const used = user.usage[resource] || 0;
    return { allowed: used < limit, remaining: limit - used };
  }
  return { allowed: true };
}
```

### Feature Flag System

```
Feature Flags (LaunchDarkly / Unleash):

Feature Flag       | Free   | Basic  | Premium | School
detailed_feedback  | false  | true   | true    | true
ai_study_plan      | false  | false  | true    | true
ondemand_gen       | false  | true   | true    | true
tutor_sessions     | false  | false  | true    | false
pdf_reports        | false  | true   | true    | true
advanced_analytics | false  | true   | true    | true
phoneme_analysis   | false  | false  | true    | false
mock_interview     | false  | false  | true    | false
white_label        | false  | false  | false   | true
api_access         | false  | false  | false   | true
offline_mode       | false  | false  | false   | false
beta_features      | false  | false  | true    | false

Dynamic Flags (A/B Testing):
  - new_evaluation_ui: 50% of Basic users
  - enhanced_streak: 100% of Premium users
  - community_mentor: 10% of active users
```

---

## Analytics & Tracking by User Type

### Events Tracked per Persona

**GUEST USER EVENTS:**
- page_view (landing, pricing, sample)
- sample_test_started
- sample_question_answered
- sample_test_completed
- registration_attempted
- registration_abandoned (funnel drop-off)
- conversion_prompt_viewed

**FREE USER EVENTS:**
- login / logout
- dashboard_viewed
- test_configured (module, type, difficulty)
- test_started
- test_submitted (module, duration, completion %)
- results_viewed (band_score)
- feedback_locked_viewed (paywall impression)
- upgrade_prompt_clicked
- community_forum_viewed
- settings_updated
- notification_clicked

**BASIC USER EVENTS:**
- All free events PLUS:
- detailed_feedback_expanded (question_id, time_spent)
- pdf_report_downloaded
- progress_tab_viewed
- weak_area_practice_started
- mistake_notebook_used
- test_limit_warning_seen
- downgrade_considered

**PREMIUM USER EVENTS:**
- All basic events PLUS:
- study_plan_generated
- study_plan_milestone_reached
- tutor_session_booked (tutor_id, type)
- tutor_session_joined
- tutor_session_completed (duration, rating)
- mock_interview_started
- mock_interview_completed
- phoneme_analysis_viewed
- essay_review_requested
- advanced_analytics_viewed
- prediction_model_viewed

**ADMIN EVENTS:**
- admin_login
- user_impersonated (target_user_id, reason)
- question_edited (set_id, changes)
- question_deprecated
- ai_prompt_modified
- financial_report_exported
- support_ticket_resolved
- system_config_changed

**INSTITUTION EVENTS:**
- student_invited
- student_joined
- assignment_created
- assignment_completed_by_student
- org_report_exported
- white_label_config_updated

---

## Localization & Accessibility

### Supported Languages by Feature

| Feature | UI Language | Test Content | Audio |
|---------|:-----------:|:------------:|:-----:|
| English (UK) | Yes | Yes | Yes British accent |
| English (US) | Yes | Yes | Yes American accent |
| Chinese (Simplified) | Yes | Yes | No |
| Chinese (Traditional) | Yes | Yes | No |
| Hindi | Yes | Yes | No |
| Arabic | Yes | Yes | No |
| Spanish | Yes | Yes | No |
| French | Yes | Yes | No |
| Japanese | Yes | Yes | No |
| Korean | Yes | Yes | No |
| Vietnamese | Yes | Yes | No |
| Portuguese | Yes | Yes | No |
| Russian | Yes | Yes | No |
| Turkish | Yes | Yes | No |
| German | Yes | Yes | No |

### Accessibility Features

```
Accessibility Compliance (WCAG 2.1 AA):

Visual:
  - Color contrast ratio >= 4.5:1
  - Band score colors: Red(4.0-5.5) Yellow(6.0-6.5)
    Green(7.0-8.0) Blue(8.5-9.0) - colorblind patterns
  - Font size: Base 16px, scalable to 200%
  - Screen reader optimized (ARIA labels)
  - Focus indicators visible

Audio:
  - Transcripts available for all listening audio
  - Audio descriptions for charts/images
  - Volume normalization
  - Visual waveform alternative

Motor:
  - Keyboard navigation (Tab, Enter, Space)
  - Skip links for main content
  - Extended time option (+25%, +50%)
  - Large touch targets (min 44x44dp)

Cognitive:
  - Clear, simple language in instructions
  - Progress indicators (step X of Y)
  - Error prevention (confirm before submit)
  - Consistent navigation
  - Reading time estimates

Assistive Technology Support:
  - Screen readers: NVDA, JAWS, VoiceOver, TalkBack
  - Voice control: Siri, Google Assistant
  - Switch control (iOS/Android)
  - Braille displays (basic support)
```

---

## Appendix

### User Type Quick Reference

| User Type | Identifier | JWT Claims | Database Table |
|-----------|-----------|------------|----------------|
| Guest | guest | None | N/A (cookie) |
| Free | free | role: "user", plan: "free" | users + subscriptions |
| Basic | basic | role: "user", plan: "basic" | users + subscriptions |
| Premium | premium | role: "user", plan: "premium" | users + subscriptions |
| Tutor | tutor | role: "tutor", verified: true | users + tutors |
| School Admin | school_admin | role: "org_admin", org_id | users + organization_members |
| School Student | school_student | role: "org_student", org_id | users + organization_members |
| Content Manager | content_mgr | role: "content_manager" | users + admin_users |
| System Admin | system_admin | role: "super_admin" | users + admin_users |
| Analyst | analyst | role: "analyst" | users + admin_users |
| Support | support | role: "support_agent" | users + admin_users |

### API Endpoint Access Control

```
Public Endpoints (No auth):
GET /api/v1/landing/content
GET /api/v1/pricing
GET /api/v1/sample-test/:module
POST /api/v1/auth/register
POST /api/v1/auth/login
POST /api/v1/auth/oauth/:provider

Authenticated Endpoints (Any valid JWT):
GET /api/v1/users/profile
PUT /api/v1/users/profile
GET /api/v1/users/settings
PUT /api/v1/users/settings
GET /api/v1/tests/history
GET /api/v1/community/posts

Plan-Restricted Endpoints:
POST /api/v1/tests/create          -> free:3/mo, basic:20/mo, premium:inf
GET /api/v1/results/detailed       -> basic+, premium:advanced
POST /api/v1/study-plan/generate   -> premium only
POST /api/v1/tutor/book            -> premium only
GET /api/v1/analytics/progress     -> basic+ (30d), premium (full)

Admin Endpoints (Admin JWT + 2FA):
GET /api/v1/admin/users
PUT /api/v1/admin/users/:id
GET /api/v1/admin/analytics
POST /api/v1/admin/questions/:id
GET /api/v1/admin/ai-logs
GET /api/v1/admin/financials
```

### Error Messages by User Type

| Error Scenario | Free User Message | Basic User Message | Premium User Message |
|----------------|-------------------|-------------------|---------------------|
| Test limit reached | "You've used all 3 free tests. Upgrade for more!" | "You've used 20/20 tests this month. Upgrade to Premium for unlimited tests." | "Unusual activity detected. Contact support if this is an error." |
| AI generation timeout | "Please try again. If issues persist, consider upgrading for priority generation." | "Generation is taking longer than usual. Your test will be ready soon." | "We apologize for the delay. A priority retry has been triggered." |
| Feature locked | "This feature requires Basic plan" | "This feature requires Premium plan" | "This feature is coming soon for Premium users" |
| Payment failed | N/A | "Payment failed. Please update your card to continue using Basic features." | "Payment failed. Please update your card within 3 days to avoid downgrade." |
| Tutor unavailable | N/A | N/A | "This tutor is fully booked. Here are 3 similar tutors available now." |

---

*Document Version: 1.0*
*Created: 2026-05-20*
*Status: Complete*
*Complements: AI-featured-IELTS-mock-test-system-arch-complete.md*
