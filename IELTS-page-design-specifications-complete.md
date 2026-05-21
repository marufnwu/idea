# IELTS Mock Test Platform - Page Design Specifications

## Document Information
- **Version**: 1.0
- **Date**: 2026-05-21
- **Status**: Draft for Development
- **Scope**: All user-facing and admin-facing pages

---

## Table of Contents

1. [Page Inventory & URL Structure](#1-page-inventory--url-structure)
2. [Global Layout System](#2-global-layout-system)
3. [Public Pages](#3-public-pages)
4. [Authentication Pages](#4-authentication-pages)
5. [Student Dashboard Pages](#5-student-dashboard-pages)
6. [Test Experience Pages](#6-test-experience-pages)
7. [Results & Feedback Pages](#7-results--feedback-pages)
8. [Progress & Analytics Pages](#8-progress--analytics-pages)
9. [Study Tools Pages](#9-study-tools-pages)
10. [Tutor Pages](#10-tutor-pages)
11. [Community Pages](#11-community-pages)
12. [Account & Settings Pages](#12-account--settings-pages)
13. [Subscription & Billing Pages](#13-subscription--billing-pages)
14. [Admin Portal Pages](#14-admin-portal-pages)
15. [Institution Portal Pages](#15-institution-portal-pages)
16. [Support Pages](#16-support-pages)
17. [Notification & Communication Pages](#17-notification--communication-pages)
18. [Error & Utility Pages](#18-error--utility-pages)
19. [Component Library Reference](#19-component-library-reference)
20. [Responsive Breakpoints](#20-responsive-breakpoints)

---

## 1. Page Inventory & URL Structure

### URL Routing Map

| Page Name | URL Pattern | Auth Required | User Types | Layout Type |
|-----------|-------------|---------------|------------|-------------|
| **Public Pages** |
| Landing Page | `/` | No | All | Marketing |
| Pricing | `/pricing` | No | All | Marketing |
| Features | `/features` | No | All | Marketing |
| Sample Test | `/sample-test/:module` | No | Guest | Test |
| About | `/about` | No | All | Marketing |
| Blog | `/blog` | No | All | Marketing |
| Blog Post | `/blog/:slug` | No | All | Marketing |
| Help Center | `/help` | No | All | Marketing |
| Help Article | `/help/:category/:article` | No | All | Marketing |
| Contact | `/contact` | No | All | Marketing |
| Terms | `/terms` | No | All | Minimal |
| Privacy | `/privacy` | No | All | Minimal |
| **Auth Pages** |
| Login | `/login` | No | All | Auth |
| Register | `/register` | No | Guest | Auth |
| OAuth Callback | `/auth/callback/:provider` | No | Guest | Auth |
| Forgot Password | `/forgot-password` | No | All | Auth |
| Reset Password | `/reset-password/:token` | No | All | Auth |
| Verify Email | `/verify-email/:token` | No | Free+ | Auth |
| **Student Pages** |
| Dashboard | `/dashboard` | Yes | Free, Basic, Premium | App |
| Test Configuration | `/test/configure` | Yes | Free, Basic, Premium | App |
| Test Interface | `/test/session/:sessionId` | Yes | Free, Basic, Premium | Test |
| Test Results | `/test/results/:sessionId` | Yes | Free, Basic, Premium | App |
| Detailed Feedback | `/test/feedback/:sessionId` | Yes | Basic, Premium | App |
| Test History | `/tests/history` | Yes | Free, Basic, Premium | App |
| Progress Overview | `/progress` | Yes | Basic, Premium | App |
| Weak Areas | `/progress/weak-areas` | Yes | Basic, Premium | App |
| Skill Radar | `/progress/skills` | Yes | Basic, Premium | App |
| Time Analytics | `/progress/time` | Yes | Basic, Premium | App |
| Study Plan | `/study-plan` | Yes | Premium | App |
| Study Plan Detail | `/study-plan/week/:weekId` | Yes | Premium | App |
| Vocabulary Builder | `/tools/vocabulary` | Yes | Basic, Premium | App |
| Mistake Notebook | `/tools/mistakes` | Yes | Basic, Premium | App |
| Grammar Tips | `/tools/grammar` | Yes | Basic, Premium | App |
| Mock Interview Setup | `/speaking/mock-setup` | Yes | Premium | App |
| Mock Interview Live | `/speaking/mock-live/:sessionId` | Yes | Premium | App |
| Mock Interview Results | `/speaking/mock-results/:sessionId` | Yes | Premium | App |
| **Tutor Pages** |
| Tutor Marketplace | `/tutors` | Yes | Premium | App |
| Tutor Profile | `/tutors/:tutorId` | Yes | Premium | App |
| Book Session | `/tutors/:tutorId/book` | Yes | Premium | App |
| My Sessions | `/tutors/my-sessions` | Yes | Premium | App |
| Session Room | `/session/live/:sessionId` | Yes | Premium, Tutor | Video |
| Essay Review Request | `/tutors/essay-review` | Yes | Premium | App |
| **Community Pages** |
| Community Home | `/community` | Yes | Free, Basic, Premium | App |
| Forum Category | `/community/:category` | Yes | Free, Basic, Premium | App |
| Forum Thread | `/community/thread/:threadId` | Yes | Free, Basic, Premium | App |
| New Thread | `/community/new-thread` | Yes | Basic, Premium | App |
| Leaderboard | `/community/leaderboard` | Yes | Free, Basic, Premium | App |
| User Profile Public | `/u/:username` | Yes | All | App |
| **Account Pages** |
| Profile Settings | `/settings/profile` | Yes | All | App |
| Account Settings | `/settings/account` | Yes | All | App |
| Notification Settings | `/settings/notifications` | Yes | All | App |
| Privacy Settings | `/settings/privacy` | Yes | All | App |
| Display Settings | `/settings/display` | Yes | All | App |
| Subscription Management | `/settings/subscription` | Yes | Basic, Premium | App |
| Payment Methods | `/settings/payment-methods` | Yes | Basic, Premium | App |
| Billing History | `/settings/billing` | Yes | Basic, Premium | App |
| Data Export | `/settings/data-export` | Yes | All | App |
| Delete Account | `/settings/delete-account` | Yes | All | App |
| **Subscription Pages** |
| Checkout | `/checkout/:plan` | Yes | Free | Minimal |
| Checkout Success | `/checkout/success` | Yes | Free | Minimal |
| Checkout Cancel | `/checkout/cancel` | Yes | Free | Minimal |
| Upgrade Prompt | `/upgrade` | Yes | Free, Basic | App |
| Plan Comparison | `/plans` | No | All | Marketing |
| **Admin Pages** |
| Admin Login | `admin.` domain | No | Admin | Admin Auth |
| Admin Dashboard | `/admin/dashboard` | Yes + 2FA | Admin | Admin |
| User Management | `/admin/users` | Yes + 2FA | Admin | Admin |
| User Detail | `/admin/users/:userId` | Yes + 2FA | Admin | Admin |
| Question Bank | `/admin/questions` | Yes + 2FA | Content Mgr | Admin |
| Question Editor | `/admin/questions/:setId` | Yes + 2FA | Content Mgr | Admin |
| AI Monitor | `/admin/ai-monitor` | Yes + 2FA | Admin | Admin |
| Analytics | `/admin/analytics` | Yes + 2FA | Admin, Analyst | Admin |
| Financial Reports | `/admin/financials` | Yes + 2FA | Admin | Admin |
| Support Tickets | `/admin/support` | Yes + 2FA | Support | Admin |
| Ticket Detail | `/admin/support/:ticketId` | Yes + 2FA | Support | Admin |
| System Config | `/admin/system` | Yes + 2FA | Admin | Admin |
| Security Logs | `/admin/security` | Yes + 2FA | Admin | Admin |
| Announcements | `/admin/announcements` | Yes + 2FA | Admin | Admin |
| Tutor Applications | `/admin/tutors/applications` | Yes + 2FA | Admin | Admin |
| **Institution Pages** |
| Institution Dashboard | `/institution/dashboard` | Yes | School Admin | App |
| Student Management | `/institution/students` | Yes | School Admin | App |
| Student Detail | `/institution/students/:studentId` | Yes | School Admin | App |
| Bulk Import | `/institution/students/import` | Yes | School Admin | App |
| Assignments | `/institution/assignments` | Yes | School Admin | App |
| Create Assignment | `/institution/assignments/new` | Yes | School Admin | App |
| Org Analytics | `/institution/analytics` | Yes | School Admin | App |
| Org Reports | `/institution/reports` | Yes | School Admin | App |
| Branding Settings | `/institution/branding` | Yes | School Admin | App |
| License Management | `/institution/license` | Yes | School Admin | App |
| API Keys | `/institution/api` | Yes | School Admin | App |
| Institution Student Dashboard | `/student/dashboard` | Yes | School Student | App |
| Institution Tests | `/student/tests` | Yes | School Student | App |
| **Support Pages** |
| Contact Support | `/support/contact` | Yes | All | App |
| My Tickets | `/support/tickets` | Yes | All | App |
| Ticket Detail | `/support/tickets/:ticketId` | Yes | All | App |
| FAQ | `/support/faq` | No | All | Marketing |
| **Notification Pages** |
| Notification Center | `/notifications` | Yes | All | App |
| Notification Preferences | `/settings/notifications` | Yes | All | App |
| **Error Pages** |
| 404 Not Found | `*` | No | All | Minimal |
| 403 Forbidden | `/403` | No | All | Minimal |
| 500 Error | `/500` | No | All | Minimal |
| Maintenance | `/maintenance` | No | All | Minimal |
| Browser Not Supported | `/unsupported` | No | All | Minimal |

---

## 2. Global Layout System

### 2.1 Layout Types

#### Marketing Layout
```
[Announcement Bar - conditional]
[Navigation Bar]
  Logo | Links (Features, Pricing, About) | [Login] [Get Started]
[Hero Section]
[Main Content Area]
[Footer]
  Columns: Product, Resources, Company, Legal
  Bottom: Copyright, Social Links, Language Selector
```

#### Application Layout (App Shell)
```
[Top Navigation Bar]
  Logo | [Search] | [Notifications] | [User Menu]
[Left Sidebar - collapsible]
  Navigation Menu (icon + label)
  Plan Badge (bottom)
[Main Content Area]
  [Breadcrumb]
  [Page Header]
  [Content]
[Right Panel - conditional]
  Contextual info, quick actions
[Bottom Navigation - mobile only]
  Home | Tests | Community | Profile
```

#### Test Layout (Immersive)
```
[Minimal Header]
  Logo (small) | Test Info | Timer | [Pause] [Exit]
[Full Screen Content]
  Question area | Media player | Input area
[Bottom Bar]
  Navigation dots | Prev/Next | Submit
[Modal Overlays]
  Confirm exit, Time warning, Submit confirmation
```

#### Admin Layout
```
[Top Bar]
  Logo | Instance Name | [Alerts] | [Admin Profile]
[Left Sidebar]
  Admin Navigation (grouped by function)
[Main Content]
  [Page Title + Actions]
  [Tabs/Filters]
  [Data Table / Cards / Charts]
[Right Drawer - conditional]
  Details panel, edit forms
```

#### Auth Layout
```
[Centered Card]
  Logo (centered, large)
  [Form Area]
  [Social Login Buttons]
  [Footer Links]
[Background]
  Subtle pattern or gradient
[No navigation]
```

#### Minimal Layout
```
[Centered Content]
  Logo
  [Message/Content]
  [Action Button]
[No sidebar, minimal header]
```

### 2.2 Global Components

#### Top Navigation Bar (App)
- **Left**: Hamburger menu (mobile), Logo, Search bar (collapsible)
- **Center**: Page title (mobile only)
- **Right**: 
  - Test credits badge (Basic)
  - Tutor credits badge (Premium)
  - Notification bell (with unread count badge)
  - User avatar dropdown
    - Profile, Settings, Subscription, Help, Logout
- **Height**: 64px desktop, 56px mobile
- **Behavior**: Fixed top, z-index 50, shadow on scroll

#### Left Sidebar Navigation
- **Sections**:
  - **Main**: Dashboard, Start Test, Progress, Study Plan (Premium)
  - **Tools**: Vocabulary, Mistakes, Grammar
  - **Community**: Forum, Leaderboard
  - **Support**: Help, Contact
  - **Account**: Settings, Subscription
- **Collapsed State**: Icons only, 64px width
- **Expanded State**: Icons + labels, 240px width
- **Active State**: Left border accent, background highlight
- **Plan Badge**: Bottom sticky, color-coded (Free=gray, Basic=blue, Premium=gold)

#### Notification Bell Dropdown
- **Trigger**: Bell icon with red dot/badge
- **Panel**: 400px width, max 500px height, scrollable
- **Sections**: Today, Earlier, Unread filter
- **Item Structure**: Icon, Title, Message, Time, [Dismiss]
- **Empty State**: "No new notifications" illustration
- **Footer**: [Mark all read] [View all notifications]

#### User Avatar Dropdown
- **Header**: Avatar + Name + Email + Plan badge
- **Menu Items**:
  - Profile Settings
  - Account & Billing
  - Notification Preferences
  - Display Settings
  - Divider
  - Help Center
  - What's New (changelog)
  - Divider
  - Logout

#### Footer (Marketing)
- **Columns**:
  - Product: Features, Pricing, Testimonials, API
  - Resources: Blog, Help Center, Guides, Webinars
  - Company: About, Careers, Press, Contact
  - Legal: Terms, Privacy, Cookies, Compliance
- **Bottom Bar**: 
  - Copyright text
  - Social icons (Twitter, Facebook, LinkedIn, YouTube)
  - Language selector dropdown
  - App store badges

### 2.3 Breadcrumb System
- **Format**: Home > Section > Page > [Current]
- **Behavior**: Clickable ancestors, current page plain text
- **Hidden on**: Mobile (replaced by back button), Dashboard home
- **Variants**:
  - Test flow: Dashboard > Tests > Listening Test > Results
  - Admin: Admin > Users > User Detail
  - Community: Community > Speaking Tips > Thread Title

---

## 3. Public Pages

### 3.1 Landing Page (`/`)

**Page Purpose**: Convert visitors to registered users

**Layout**: Marketing Layout

**Sections**:

#### Hero Section
- **Background**: Gradient or subtle animated pattern
- **Content**:
  - H1: "Master IELTS with AI-Powered Practice"
  - Subtitle: "Get instant band scores, detailed feedback, and personalized study plans"
  - CTA Group: [Start Free Practice] (primary, large) + [View Pricing] (secondary)
  - Social proof: "Join 50,000+ students" with avatar stack
- **Right/Visual**: Animated band score gauge (6.0 → 9.0), or product screenshot
- **Below fold**: Trust badges (Official IELTS alignment, Security, Reviews)

#### Features Grid
- **Layout**: 3-4 column grid (2 on mobile)
- **Cards**:
  - AI-Powered Evaluation (icon: brain/robot)
  - Realistic Test Simulation (icon: clipboard)
  - Detailed Feedback (icon: chart)
  - Personalized Study Plans (icon: calendar)
  - Expert Tutors (icon: video)
  - Progress Tracking (icon: trend-up)
- **Each Card**: Icon, Title, 2-line description, [Learn more] link

#### How It Works
- **Layout**: Horizontal timeline (vertical on mobile)
- **Steps**:
  1. Take a test (icon: play)
  2. Get instant scoring (icon: check-circle)
  3. Review AI feedback (icon: message-square)
  4. Follow study plan (icon: target)
- **Connector**: Animated line between steps

#### Test Preview Section
- **Heading**: "Try a Sample Test — No Account Required"
- **Cards**: 4 module cards (Listening, Reading, Writing, Speaking)
  - Each: Module icon, 3-question preview badge, [Try Now] button
- **Behavior**: Opens sample test modal/overlay

#### Pricing Teaser
- **Heading**: "Choose Your Plan"
- **Cards**: Free, Basic, Premium (side by side)
  - Highlighted: Basic or Premium (recommended badge)
  - Feature comparison (3-5 key features)
  - Price display
  - [Get Started] / [Go Premium] buttons
- **Note**: Full comparison links to `/pricing`

#### Social Proof / Testimonials
- **Layout**: Carousel or grid
- **Elements**: Student photo, name, target band, quote, star rating
- **Stats**: "50,000+ tests taken", "Average improvement +1.5 band", "4.8/5 rating"

#### FAQ Accordion
- **Items**: 5-6 common questions
  - "Is this official IELTS?"
  - "How accurate is the AI scoring?"
  - "Can I get a refund?"
  - "What modules are supported?"
  - "Is there a mobile app?"

#### Final CTA Section
- **Background**: Primary brand color
- **Content**: "Ready to improve your IELTS score?"
- **Buttons**: [Start Free Practice] + [Compare Plans]

**Navigation**: Marketing nav, no sidebar
**Footer**: Full marketing footer
**SEO**: Meta title, description, structured data

---

### 3.2 Pricing Page (`/pricing`)

**Page Purpose**: Convert visitors to paying customers

**Layout**: Marketing Layout

**Sections**:

#### Page Header
- H1: "Simple, Transparent Pricing"
- Subtitle: "Choose the plan that fits your IELTS goals"
- Toggle: Monthly / Annual (save 20% badge)

#### Plan Comparison Table
- **Cards Layout**: 3-4 cards side by side
  - **Free**: $0, basic features, [Get Started]
  - **Basic**: $19.99/mo, most popular badge, [Start Basic]
  - **Premium**: $39.99/mo, best value badge, [Go Premium]
  - **Enterprise**: Custom pricing, [Contact Sales]

- **Each Card**:
  - Plan name + badge
  - Price (large) + "/month"
  - Annual price (strikethrough monthly if annual selected)
  - [CTA Button] (color-coded)
  - Feature list (checkmarks + text)
    - Tests per month
    - AI feedback level
    - Study plan
    - Tutor sessions
    - Support level
  - "What's included" expander (mobile)

#### Detailed Feature Matrix
- **Table**: Features as rows, Plans as columns
- **Categories**: Test Taking, Evaluation, Study Tools, Support, Extras
- **Check/X icons** for availability
- **Sticky header** on scroll

#### FAQ Section
- Plan-specific questions
- Billing, cancellation, upgrade/downgrade policies

#### Trust Signals
- "Cancel anytime" badge
- "Secure payment" icons (Stripe, SSL)
- "14-day money-back guarantee"
- Student success stats

#### Bottom CTA
- "Still have questions? [Contact Sales] or [Chat with Support]"

**States**:
- Loading: Skeleton cards
- Error: "Unable to load pricing. Please refresh."
- Promo: Banner with coupon code input

---

### 3.3 Sample Test Modal/Page (`/sample-test/:module`)

**Page Purpose**: Allow guests to try platform without registration

**Layout**: Test Layout (simplified)

**Components**:

#### Pre-Test Screen
- **Header**: "Sample Test — 3 Questions"
- **Module Info**: Icon, Name, Duration estimate
- **Instructions**: 3 bullet points about the sample
- **Warning**: "This is a preview. Create a free account for full tests."
- **Button**: [Start Sample] (primary)
- **Secondary**: [Create Free Account] (to convert early)

#### Question Interface
- **Progress**: "Question 1 of 3" + progress bar
- **Timer**: Not shown (sample has no time limit)
- **Content Area**: 
  - Audio player (Listening) or Passage text (Reading)
  - Question text
  - Answer input (varies by question type)
- **Navigation**: [Previous] [Next] [Submit]
- **Banner**: Sticky top — "Create an account to save progress" [Sign Up]

#### Feedback Screen (per question)
- **Result**: Correct/Incorrect badge
- **Correct Answer**: Revealed immediately
- **Explanation**: 2-3 sentences
- **Upgrade Prompt**: "Get detailed explanations for all 40 questions" [Upgrade]

#### Completion Screen
- **Header**: "Sample Complete!"
- **Score**: "You got 2/3 correct"
- **Estimated Band**: "Estimated Band: 6.5" (based on sample)
- **Upgrade Card**:
  - "Unlock full tests with detailed AI feedback"
  - Feature bullets
  - [Create Free Account] (primary)
  - [Continue as Guest] (secondary, returns to landing)

**Backend**: Static content, no AI generation, cookie-based progress

---

### 3.4 Help Center (`/help`)

**Page Purpose**: Self-service support and documentation

**Layout**: Marketing Layout with search-focused header

**Sections**:

#### Search Hero
- **H1**: "How can we help?"
- **Search Bar**: Large, centered, placeholder "Search for articles..."
- **Popular Searches**: Pill buttons below

#### Category Grid
- **Categories** (6 cards):
  - Getting Started (icon: rocket)
  - Taking Tests (icon: clipboard)
  - Understanding Scores (icon: bar-chart)
  - Subscriptions & Billing (icon: credit-card)
  - Account & Security (icon: shield)
  - Tutors & Sessions (icon: users)
- **Each**: Icon, Title, Article count

#### Popular Articles
- **List**: 10 articles with view counts
- **Each**: Title, category tag, "Updated 2 days ago"

#### Contact Options
- **Cards**: 
  - Email Support (Basic+)
  - Live Chat (Premium)
  - Community Forum (Free+)
  - Book a Demo (Enterprise)

#### Article Page (`/help/:category/:article`)
- **Breadcrumb**: Help > Category > Article Title
- **Layout**: Article content (max-width 720px, centered)
- **Sidebar**: Table of contents (sticky, scroll spy)
- **Footer**: Was this helpful? [Yes] [No] + Related articles
- **Actions**: Share, Print, [Contact Support if not resolved]

---

## 4. Authentication Pages

### 4.1 Login Page (`/login`)

**Page Purpose**: Authenticate existing users

**Layout**: Auth Layout

**Components**:

#### Auth Card
- **Logo**: Centered, 48px height
- **Header**: "Welcome back" / "Sign in to your account"

#### Form Fields
- Email: [________________] (validation: email format)
- Password: [________________] (toggle visibility icon)
- [x] Remember me (checkbox)
- [Forgot password?] link
- [Sign In] button (full width, primary)

#### Social Login
- Divider: "or continue with"
- Buttons: [Google] [Apple] [Facebook]
  - Icon + brand name
  - Outlined style

#### Footer
- "Don't have an account? [Sign up]"
- "By signing in, you agree to [Terms] and [Privacy]"

#### States
- **Loading**: Button shows spinner, disabled
- **Error**: Inline message "Invalid email or password"
- **Success**: Redirect to dashboard or intended destination
- **Session Expired**: Banner "Your session expired. Please sign in again."

**Background**: Subtle gradient, abstract shapes
**No navigation bar**
**SEO**: Noindex

---

### 4.2 Registration Page (`/register`)

**Page Purpose**: Create new user account

**Layout**: Auth Layout

**Components**:

#### Progress Indicator (Step 1 of 3)
- **Steps**: Account → Profile → Welcome
- **Visual**: Horizontal progress dots with labels

#### Step 1: Account Creation
- **Header**: "Create your free account"
- **Social Buttons**: [Google] [Apple] [Facebook] (primary method emphasis)
- **Divider**: "or sign up with email"
- **Fields**:
  - Full Name: [________________] (required)
  - Email: [________________] (validation, uniqueness check)
  - Password: [________________] (strength meter below)
    - Weak/Medium/Strong indicator
    - Requirements: 8+ chars, 1 number, 1 symbol
  - Confirm Password: [________________]
- [x] I agree to [Terms] and [Privacy] (required)
- [x] Send me study tips (optional, default checked)
- [Create Account] button

#### Step 2: IELTS Profile (Optional)
- **Header**: "Help us personalize your experience"
- **Subtext**: "You can skip this and set it up later"
- **Fields**:
  - Target Band Score: Slider (4.0 to 9.0, step 0.5)
  - Test Date: Date picker (future dates only)
  - Test Type: [Academic] [General Training] (radio cards)
  - Current Level: [Beginner] [Intermediate] [Advanced] (radio cards)
- [Skip for now] [Continue]

#### Step 3: Welcome
- **Header**: "Welcome, {Name}!"
- **Content**:
  - Plan summary card (Free plan features)
  - "First Step" achievement badge (animated)
- **CTA**: [Start Your First Test] (primary, large)
- **Secondary**: [Go to Dashboard]

#### States
- **Email Exists**: "An account with this email already exists. [Sign in]"
- **Weak Password**: Inline validation, block submission
- **OAuth Linking**: If email exists with different provider, offer linking
- **Success**: Auto-login, redirect to Step 3 or dashboard

**Conversion Tracking**: 
- Registration start
- Step completion rates
- Drop-off points

---

### 4.3 Forgot Password (`/forgot-password`)

**Layout**: Auth Layout (narrow card)

**Components**:
- Header: "Reset your password"
- Subtext: "Enter your email and we'll send you a reset link"
- Email field
- [Send Reset Link] button
- Success state: "Check your email for reset instructions" + illustration
- Error: "We couldn't find an account with that email"
- Footer: [Back to sign in]

---

### 4.4 Reset Password (`/reset-password/:token`)

**Layout**: Auth Layout

**Components**:
- Header: "Create new password"
- New password field + strength meter
- Confirm password field
- [Reset Password] button
- Success: "Password updated! [Sign in with new password]"
- Invalid token: "This link has expired. [Request new link]"

---

## 5. Student Dashboard Pages

### 5.1 Dashboard Home (`/dashboard`)

**Page Purpose**: Central hub for all user activities

**Layout**: App Layout

**User Variants**:

#### Free User Dashboard

**Header Section**:
- Welcome message: "Welcome back, {Name}!"
- Plan badge: "FREE PLAN" (gray)
- Tests remaining: "3/3 tests this month" (progress ring)
- Reset date: "Resets on June 1"

**Quick Actions Row**:
- [Start Test] (primary, large)
- [View Progress] (disabled or hidden)
- [Community] (outline)

**Main Content Grid** (2 columns desktop, 1 mobile):

**Left Column**:
- **Recent Activity Card**:
  - Title: "Recent Activity"
  - List: Last 3 activities (test completed, badge earned, etc.)
  - Empty state: "No tests yet. Take your first test!"
  - [View All History]

- **Upgrade Prompt Card** (prominent):
  - Background: Subtle gradient or border accent
  - Title: "Unlock Detailed Feedback"
  - Bullets: AI explanations, progress tracking, PDF reports
  - [Upgrade to Basic — $19.99/mo]
  - [Compare Plans]

**Right Column**:
- **Stats Overview** (simplified):
  - Tests taken: 0
  - Best band: —
  - Study streak: 0 days

- **Community Highlights**:
  - Title: "From the Community"
  - 2-3 recent forum posts
  - [Join Discussion]

- **Daily Tip Card**:
  - Random IELTS tip
  - [More Tips]

**Bottom Section**:
- **Achievement Preview**: Locked achievements with "Upgrade to unlock" overlay

---

#### Basic User Dashboard

**Header Section**:
- Welcome message + name
- Plan badge: "BASIC" (blue)
- Tests remaining: "12/20 tests this month"
- Next billing date

**Quick Actions**:
- [Start Test] with dropdown:
  - Full IELTS Test
  - Module Practice
  - AI-Generated Test (5/mo limit indicator)

**Main Content**:

**Left Column**:
- **Progress Chart Card**:
  - Title: "Your Progress"
  - Mini line chart (last 30 days)
  - Overall band trend
  - [View Full Analytics]

- **Recent Tests Card**:
  - Last 5 tests table
  - Columns: Date, Type, Band, Actions
  - [View All]

- **Weak Areas Card**:
  - Title: "Focus Areas"
  - 2-3 weak areas with progress bars
  - [Practice Weak Areas]

**Right Column**:
- **Study Streak Card**:
  - Calendar heatmap (last 30 days)
  - Current streak: "5 days"
  - [View Streak Details]

- **Mistake Notebook Preview**:
  - "You have 12 saved mistakes"
  - [Review Mistakes]

- **Upgrade Teaser** (subtle):
  - "Want unlimited tests + tutor sessions?"
  - [Explore Premium]

---

#### Premium User Dashboard

**Header Section**:
- Welcome message + name
- Plan badge: "PREMIUM" (gold/gradient)
- "Unlimited tests"
- Tutor credits: "2 sessions remaining this month"

**Quick Actions** (4-5 buttons):
- [Full Test]
- [Module Practice]
- [Book Tutor]
- [Mock Interview]
- [Analytics]

**Today's Focus Card** (AI-generated, prominent):
- Title: "Today's Focus"
- Content: Personalized recommendations
  - "Listening Section 4 practice (45 min)"
  - "Writing Task 2 outline (30 min)"
- [Start Recommended Practice]
- [View Full Study Plan]

**Progress to Target**:
- Visual: Progress bar or gauge
- Current: 7.5 → Target: 8.5
- Days remaining: 87
- Predicted score: 8.2 (if current pace continues)
- [Adjust Target]

**Upcoming Tutor Session** (conditional):
- Card with tutor photo, name, date/time
- [Join Session] or [Reschedule]
- Countdown timer if within 1 hour

**Main Grid**:

**Left Column**:
- **Band Score Trend**: Full chart with module breakdown
- **Recent Activity**: Tests + tutor sessions + achievements
- **AI Insights**: "Based on your last 3 tests..." tip

**Right Column**:
- **Study Plan Progress**: Weekly milestone tracker
- **Vocabulary Builder**: Word of the day + streak
- **Achievements**: Recent unlocks + next targets
- **Community**: Mentions, replies, trending topics

---

#### Institution Student Dashboard (`/student/dashboard`)

**Header**:
- Organization logo (white-label)
- Welcome: "Welcome, {Name}!"
- Class rank: "3/187"

**Announcements Banner** (conditional):
- Instructor message
- [Dismiss]

**Assigned Tests Card** (priority):
- Title: "Assigned Tests"
- List with deadlines
  - Overdue badge (red)
  - Due soon badge (orange)
  - Completed badge (green)
- [Start Test] per item

**Progress Card**:
- Overall band: 7.5 (up from 6.5)
- Tests completed: 42/50 assigned
- Class average comparison

**Class Rankings**:
- Mini leaderboard (top 5)
- "You are #3" highlight
- [View Full Rankings]

**Navigation**: Institution-specific sidebar (branded colors)

---

### 5.2 Test History (`/tests/history`)

**Page Purpose**: Browse and review all past tests

**Layout**: App Layout

**Components**:

#### Page Header
- Title: "Test History"
- Subtitle: "Review your past tests and track improvement"
- Actions: [Filter] [Sort] [Export] (Basic+)

#### Filter Bar
- **Module**: All, Listening, Reading, Writing, Speaking
- **Type**: All, Full Test, Module Practice, Mock Interview
- **Date Range**: Last 7 days, 30 days, 90 days, Custom
- **Band Score**: Range slider (4.0 - 9.0)
- **Clear Filters** button

#### Data Table
- **Columns**:
  - Date (sortable)
  - Test Type (icon + label)
  - Module(s)
  - Overall Band (color-coded badge)
  - Duration
  - Status (Completed, Abandoned, Evaluating)
  - Actions: [View Results] [Retake] [Delete] (own only)
- **Row Style**: Hover highlight, band score color indicator
- **Pagination**: 20 per page, infinite scroll option

#### Empty States
- **No tests**: "You haven't taken any tests yet. [Start Your First Test]"
- **No results for filters**: "No tests match your filters. [Clear Filters]"
- **Free user limit**: "Showing last 3 tests. [Upgrade] to see full history."

#### Mobile View
- Card list instead of table
- Each card: Date, type, band (large), [View]

---

## 6. Test Experience Pages

### 6.1 Test Configuration (`/test/configure`)

**Page Purpose**: Set up test parameters before starting

**Layout**: App Layout (centered content, max-width 800px)

**Components**:

#### Page Header
- Title: "Configure Your Test"
- Step indicator: "Step 1 of 2" (Configure → Test)

#### Plan Notice Banner (conditional)
- **Free**: "Free Plan: Pre-generated tests only. 3 tests per month."
- **Basic**: "Basic Plan: 20 tests/mo. 5 AI-generated tests/mo."
- **Premium**: "Premium Plan: Unlimited tests. AI-generated available."

#### Test Type Selection
- **Options** (radio cards):
  - **Full IELTS Test**: "All 4 modules — 2h 45min" (recommended badge)
  - **Module Practice**: "Focus on one module"
    - Sub-options appear: Listening, Reading, Writing, Speaking
  - **Mock Interview**: "Speaking simulation with AI" (Premium only)
- **Visual**: Icon + title + description + estimated time

#### Test Format
- **Type**: [Academic] [General Training] (radio buttons)
- **Disabled/Forced**: Based on user profile setting

#### Difficulty Selection
- **Free**: "Auto-selected based on your level" (disabled, info text)
- **Paid**: Slider or radio cards
  - Band 5-6, Band 6-7, Band 7-8, Band 8-9
  - "Recommended: Band 6-7" (based on history)

#### Question Source
- **Free**: "Pre-generated questions" (only option)
- **Basic/Premium**:
  - [o] Instant Start (Pre-generated)
  - [o] AI-Generated (Personalized to your weak areas)
    - Basic: "3 remaining this month"
    - Premium: "Unlimited"

#### Summary Card
- **Content**: Test type, format, difficulty, estimated duration, question source
- **Warning**: "Ensure you have uninterrupted time"

#### Actions
- [Cancel] (secondary)
- [Start Test] (primary, large)
  - Free limit reached: Disabled + "Upgrade required" tooltip

#### Backend Checks
- Validate test credits before allowing start
- If limit reached: Show upgrade modal instead of starting

---

### 6.2 Test Interface (`/test/session/:sessionId`)

**Page Purpose**: Active test taking environment

**Layout**: Test Layout (immersive, full focus)

**Components**:

#### Top Bar (Fixed, 48px)
- **Left**: 
  - Platform logo (small, 24px)
  - Test name: "Academic Listening — Band 6-7"
- **Center**:
  - Module timer: "38:42 remaining" (red if < 5 min)
  - Section indicator: "Section 2 of 4"
- **Right**:
  - [Pause] button (icon)
  - [Exit] button (icon, with confirmation)

#### Main Content Area
- **Split View** (Listening/Reading):
  - **Left Panel** (60%): 
    - Audio player (Listening) or Passage text (Reading)
    - Audio controls: Play/Pause, Progress bar, Volume, Speed (0.75x-1.5x)
  - **Right Panel** (40%):
    - Question text
    - Answer input area
    - Question type indicator (Multiple choice, Fill in blank, etc.)
- **Full Width** (Writing/Speaking):
  - Writing: Prompt top, text area bottom, word counter
  - Speaking: Prompt card, recording controls, waveform visualization

#### Question Navigation (Bottom Bar, 56px)
- **Left**: [Previous] button
- **Center**: Question number pills (1-40)
  - States: Current (filled), Answered (checkmark), Unanswered (outline), Flagged (flag icon)
  - Click to jump (with confirmation if time running)
- **Right**: [Next] or [Submit Section]

#### Question Types UI

**Multiple Choice**:
- Radio buttons: A, B, C, D
- Option text
- Selected state: Filled radio + highlight

**Fill in the Blank**:
- Inline input fields within text
- Character limit indicator
- Auto-save indicator ("Saved")

**Matching**:
- Drag and drop or dropdown selectors
- Two-column layout

**True/False/Not Given**:
- Three large button options
- Selected state

**Writing Task**:
- Prompt box (scrollable)
- Text area with formatting toolbar (minimal: bold, italic, undo)
- Word count: "245 / 250 words" (color change near limit)
- Timer: "20:00 remaining" for task

**Speaking Task**:
- Part indicator: "Part 2: Long Turn"
- Prompt card with bullet points
- Preparation timer (1 minute for Part 2)
- Recording button: Large circular, pulse animation when recording
- Waveform visualization
- Time elapsed: "00:45 / 02:00"

#### Modal Overlays

**Pause Modal**:
- "Test Paused"
- Timer paused
- [Resume] [Exit Test]
- Warning: "You can pause for up to 10 minutes total"

**Exit Confirmation**:
- "Are you sure you want to exit?"
- "Your progress will be saved but this test will count as used."
- [Cancel] [Exit and Save] [Exit and Discard]

**Time Warning**:
- 5 minutes left: Banner "5 minutes remaining"
- 1 minute left: Banner + sound (if enabled)
- Time up: Auto-submit modal

**Submit Confirmation**:
- "Ready to submit?"
- Unanswered questions count: "You have 3 unanswered questions."
- Review option: [Review Unanswered] [Submit Now]
- Post-submit: "Submitting..." spinner

#### Auto-Save
- Indicator: "All changes saved" (bottom left)
- Backend: Save to Redis every 30 seconds + on every input change

#### Accessibility
- Keyboard navigation: Tab, Enter, Space, Arrow keys
- Screen reader labels for all inputs
- High contrast mode support

---

## 7. Results & Feedback Pages

### 7.1 Test Results (`/test/results/:sessionId`)

**Page Purpose**: Display overall test performance

**Layout**: App Layout

**User Variants**:

#### Free User Results

**Header Section**:
- Title: "Listening Test Complete!"
- Time: "Completed in 38 minutes"

**Score Display** (centered, prominent):
- Large band score: "6.5" (animated counter)
- Gauge visualization: 6.5/9.0
- Correct count: "28/40 (70%)"

**Performance Summary**:
- Section breakdown (simplified):
  - Section 1: 8/10
  - Section 2: 9/10
  - Section 3: 7/10
  - Section 4: 6/10
- Time used: "38/40 minutes"

**Locked Content Teaser**:
- Card with lock icons:
  - "Detailed AI Feedback — Upgrade to Basic"
  - "Section-by-section analysis"
  - "Correct answers with explanations"
  - "Audio timestamps for review"
- [Upgrade to Basic — $19.99/mo]
- [Maybe Later]

**Actions**:
- [Take Another Test]
- [View All Results]
- [Share Result] (basic text only)

**Upgrade Prompt Modal** (auto-triggered after 3 seconds):
- Same content as teaser but modal overlay
- Closeable

---

#### Basic User Results

**Header**: Same as Free

**Score Display**: Same

**Detailed Breakdown** (expandable sections):
- **Section 1**: 8/10, 8 min, Trend: ↑
  - Expandable to question list
- **Section 2**: 9/10, 9 min, Trend: →
- **Section 3**: 7/10, 10 min, Trend: ↓
- **Section 4**: 6/10, 11 min, Trend: ↓

**Question List**:
- Table: Question #, Your Answer, Correct, Status
- [View Details] per row → links to detailed feedback

**Actions**:
- [View Detailed Feedback] (primary)
- [Download PDF Report]
- [Add to Mistake Notebook]
- [Retake Test]

**Related**:
- "Practice weak areas from this test"
- [Start Weak Area Practice]

---

#### Premium User Results

**Header**: Same + "AI Analysis Complete"

**Score Display**: 
- Overall band + individual module bands
- Predicted score trajectory

**AI Insights Card**:
- "Based on this test, here are 3 key insights:"
- Bullet points with recommendations
- [View Full Study Plan]

**Criteria Breakdown** (Writing/Speaking):
- Rubric table:
  - Criterion | Score | Analysis
  - Task Response | 7.0 | "Good development, but..."
  - Coherence | 7.5 | "Well-organized..."
  - Lexical Resource | 6.5 | "Adequate range..."
  - Grammar | 6.5 | "Some errors in..."

**Actions**:
- [View Detailed Feedback]
- [Download Full Report]
- [Share with Tutor]
- [Book Review Session]

---

### 7.2 Detailed Feedback (`/test/feedback/:sessionId`)

**Page Purpose**: Per-question analysis and learning

**Layout**: App Layout

**Components**:

#### Page Header
- Title: "Detailed Feedback"
- Subtitle: "Listening Test — Band 6.5"
- Breadcrumb: Results > Detailed Feedback
- Actions: [Back to Results] [Download PDF]

#### Question Navigator (Sticky Sidebar or Top Bar)
- Grid: 1-40 question pills
- Color coding: Correct (green), Incorrect (red), Partial (yellow)
- Current question highlighted
- Filter: [All] [Incorrect only] [Flagged]

#### Question Detail Card (Main Content)

**Header**:
- Question #15 of 40
- Type badge: "Multiple Choice"
- Section badge: "Section 2"
- Difficulty: "Band 6-7"

**Your Performance**:
- Your Answer: "A) Near the city center" (red X)
- Correct Answer: "C) Close to the university" (green check)
- Time spent: "45 seconds"

**Media Section** (Listening only):
- Audio snippet player: Timestamp 00:45 - 01:05
- Controls: Play, Pause, Speed (0.75x, 1x, 1.25x)
- [Show Transcript] toggle
- Transcript text with key sentence highlighted

**Explanation**:
- Heading: "Why C is correct"
- Paragraph explanation
- Key vocabulary list with definitions
- Related grammar point (if applicable)

**Common Mistakes**:
- "Why students choose A: The city center is mentioned as a distractor..."

**Actions**:
- [Play Audio Again]
- [Add to Mistake Notebook]
- [Mark for Review]
- [Previous] [Next] navigation

#### Side Panel (Right)
- **Question Statistics**:
  - "% of users got this correct"
  - "You took 45s (avg: 38s)"
- **Related Questions**: Links to similar questions
- **Vocabulary**: Key words from this question

#### Empty/Locked States
- **Free user**: "Upgrade to Basic to see detailed feedback" with feature list
- **Loading**: Skeleton cards for explanation
- **Error**: "Failed to load feedback. [Retry]"

---

## 8. Progress & Analytics Pages

### 8.1 Progress Overview (`/progress`)

**Page Purpose**: Visualize long-term improvement

**Layout**: App Layout

**Access**: Basic, Premium (Free sees upgrade prompt)

**Components**:

#### Page Header
- Title: "Your Progress"
- Subtitle: "Track your IELTS journey"
- Actions: [Date Range] [Export] (Premium)

#### Summary Cards Row (4 cards)
- **Current Overall Band**: Large number, trend arrow (↑/↓/→), change amount
- **Tests Taken**: Total count, this month count
- **Study Streak**: Current streak, longest streak
- **Time Invested**: Total hours, this week hours

#### Band Score Trend Chart (Main)
- **Type**: Line chart with multiple series
- **Series**: Overall, Listening, Reading, Writing, Speaking
- **X-axis**: Dates (last 30/90/365 days)
- **Y-axis**: Band score (4.0 - 9.0)
- **Interactive**: Hover for exact scores, toggle series on/off
- **Annotations**: Test dates, milestone markers
- **Empty**: "Take more tests to see your trend. [Start Test]"

#### Module Breakdown Section
- **4 Column Grid** (1 column mobile):
  - **Listening Card**:
    - Current band: 7.0
    - Trend chart (mini sparkline)
    - Tests taken: 12
    - Weak area: "Section 4"
    - [Practice Listening]
  - **Reading Card**: Same structure
  - **Writing Card**: Same structure
  - **Speaking Card**: Same structure

#### Recent Milestones
- Timeline: "May 18 — First 7.0 in Listening"
- "May 12 — 7-day study streak"
- "April 30 — Completed 10th test"

#### Comparison Section (Premium)
- **Percentile**: "You score better than 68% of Academic test takers"
- **Target Progress**: Visual progress to goal

---

### 8.2 Weak Areas (`/progress/weak-areas`)

**Page Purpose**: Identify and address specific weaknesses

**Layout**: App Layout

**Components**:

#### Page Header
- Title: "Weak Areas"
- Subtitle: "Focus your practice where it matters most"

#### AI Analysis Card (Premium)
- "AI-Powered Analysis (updated daily)"
- Last updated timestamp
- [Refresh Analysis]

#### Priority Matrix
- **High Priority** (red):
  - Listening Section 4 (Academic lectures) — 60% accuracy
  - Reading TF/NG questions — 55% accuracy
  - [Practice This]
- **Medium Priority** (yellow):
  - Writing Task 1 data description — 65%
  - Speaking Part 3 abstract topics — 70%
  - [Practice This]
- **Improving** (green):
  - Listening Section 1 — 85% (was 70%)
  - [Keep Practicing]

#### Each Weak Area Card
- **Header**: Topic name, priority badge, accuracy percentage
- **Trend Chart**: Mini bar chart (last 5 attempts)
- **Details**:
  - "You miss these questions because..."
  - Common error pattern
  - Suggested resource
- **Actions**:
  - [Practice 5 Questions] (AI-generated)
  - [View Related Lessons]
  - [Add to Study Plan]

#### Empty State
- "Great job! No significant weak areas identified. [Take More Tests]"

---

### 8.3 Skill Radar (`/progress/skills`)

**Page Purpose**: Visual skill balance visualization

**Layout**: App Layout

**Components**:
- **Radar Chart**: 6-8 axes (Listening S1-S4, Reading types, Writing tasks, Speaking parts)
- **Current vs Target**: Two overlays
- **Legend**: Click to toggle
- **Insights**: "Your Listening S4 is 2 bands below your Listening S1"
- **Recommendations**: "Focus on academic vocabulary to improve Section 4"

---

### 8.4 Time Analytics (`/progress/time`)

**Page Purpose**: Analyze time management

**Layout**: App Layout

**Components**:
- **Time Distribution Chart**: Pie or stacked bar (per module, per section)
- **Pace Analysis**: "You spend too much time on Section 3 questions"
- **Comparison**: Your time vs recommended time
- **Trend**: Time per test over last 10 tests
- **Tips**: "Try the 20-40-60-80 rule for Listening sections"

---

## 9. Study Tools Pages

### 9.1 Study Plan (`/study-plan`)

**Page Purpose**: AI-generated personalized study schedule

**Layout**: App Layout

**Access**: Premium only (Free/Basic see upgrade prompt)

**Components**:

#### Page Header
- Title: "Your Study Plan"
- Subtitle: "AI-generated based on your goals and weak areas"
- Actions: [Regenerate] [Customize] [Export]
- Last generated: "May 20, 2026"

#### Plan Overview Card
- **Target**: 8.5 band by August 15, 2026
- **Current**: 7.5 band
- **Days Remaining**: 87
- **Weekly Commitment**: 14 hours
- **Progress**: "Week 2 of 12 — 16% complete"

#### Phase Tabs
- **Phase 1: Foundation** (Weeks 1-4) — Active
- **Phase 2: Improvement** (Weeks 5-8)
- **Phase 3: Mastery** (Weeks 9-12)

#### Weekly Schedule (Current Week)
- **Table/Grid**:
  - Day | Focus | Activities | Time | Status
  - Monday | Listening S4 | Practice test + note-taking | 45 min | [Done]
  - Tuesday | Writing Task 2 | Outline + essay | 30 min | [In Progress]
  - Wednesday | Vocabulary | Academic collocations | 30 min | [Pending]
  - ...
- **Status**: Checkbox or badge (Done, In Progress, Missed, Pending)
- **Actions**: [Mark Complete] [Reschedule] [Skip]

#### Daily Detail (Expanded Row)
- Specific tasks with links
- Resources: "Use Cambridge IELTS 17 Test 2"
- AI tips: "Focus on identifying speaker attitudes"

#### Milestones
- Week 2: "Achieve 8.0 in Listening"
- Week 4: "Consistent 7.5+ in Writing Task 2"
- Progress bars per milestone

#### Empty/Loading States
- **Generating**: "AI is creating your personalized plan..." spinner + progress
- **No plan**: "Create your first study plan" button
- **Expired**: "Your test date has passed. [Set New Goal]"

---

### 9.2 Vocabulary Builder (`/tools/vocabulary`)

**Page Purpose**: Learn and review IELTS vocabulary

**Layout**: App Layout

**Components**:

#### Page Header
- Title: "Vocabulary Builder"
- Stats: "Words learned: 127 | Streak: 5 days"
- Actions: [Review All] [Import List]

#### Word of the Day Card (prominent)
- Word: "Sustainable"
- Phonetic: /səˈsteɪnəbl/
- Audio pronunciation button
- Definition: "Able to be maintained at a certain rate or level"
- IELTS context: "Commonly used in Writing Task 2 environment topics"
- Example sentence
- [Add to My List] [Next Word]

#### My Vocabulary Lists
- **Tabs**: All | Learning | Mastered | Difficult
- **List View**:
  - Word | Definition | Frequency | Last Reviewed | Mastery
  - Sortable columns
- **Card View** (mobile): Flip cards (front: word, back: definition)

#### Study Mode
- **Flashcards**: Flip animation, mark as known/learning/difficult
- **Quiz Mode**: Multiple choice definitions
- **Spelling Mode**: Type the word from definition

#### Categories
- Academic collocations
- Topic-specific (Environment, Education, Technology)
- Band 7+ advanced vocabulary
- Recent test words (from your mistakes)

---

### 9.3 Mistake Notebook (`/tools/mistakes`)

**Page Purpose**: Review and learn from past errors

**Layout**: App Layout

**Components**:

#### Page Header
- Title: "Mistake Notebook"
- Subtitle: "Learn from your errors"
- Count: "12 saved mistakes"
- Actions: [Review Session] [Export]

#### Filter Bar
- Module: All, Listening, Reading, Writing, Speaking
- Question Type: All, Multiple Choice, TFNG, etc.
- Date: All, Last 7 days, Last 30 days
- Mastery: All, Still Learning, Mastered

#### Mistake Cards
- **Each Card**:
  - Question preview (truncated)
  - Your wrong answer (red)
  - Correct answer (green)
  - Source: "Listening Test — May 18"
  - Date added
  - Tags: Question type, topic
  - Mastery status: "Reviewed 3 times — 80% retention"
  - Actions: [View Full Question] [Remove] [Add to Review Queue]

#### Review Session
- [Start Review] button
- Mode: Spaced repetition
- Shows mistakes based on forgetting curve

#### Empty State
- "No mistakes saved yet. They'll appear here when you add them from test feedback."

---

### 9.4 Grammar Tips (`/tools/grammar`)

**Page Purpose**: Targeted grammar improvement

**Layout**: App Layout

**Components**:
- **Header**: "Grammar Tips"
- **Categories**: Articles, Tenses, Conditionals, Relative Clauses, etc.
- **For You** (AI-curated): Based on your writing errors
- **Tip Cards**:
  - Title: "Using Articles with Uncountable Nouns"
  - Explanation with examples
  - IELTS relevance: "Common in Writing Task 2"
  - [Mark as Learned] [Practice Exercise]
- **Exercises**: Interactive mini-quizzes

---

## 10. Tutor Pages

### 10.1 Tutor Marketplace (`/tutors`)

**Page Purpose**: Browse and book IELTS tutors

**Layout**: App Layout

**Access**: Premium only

**Components**:

#### Page Header
- Title: "Tutor Marketplace"
- Subtitle: "Book 1-on-1 sessions with IELTS experts"
- Credits badge: "2 sessions remaining this month"
- [Purchase More Credits — $29.99]

#### Filter & Search Bar
- **Search**: By name or keyword
- **Filters**:
  - Module: Speaking, Writing, Reading, All
  - Availability: Today, This Week, Anytime
  - Rating: 4.5+, 4.7+, 4.9+
  - Price: Included in Premium, Additional credits
  - Native Speaker: Toggle
- **Sort**: Recommended, Rating, Price, Availability

#### Tutor Cards Grid
- **Each Card**:
  - Profile photo (circle, 80px)
  - Name + Verified badge (checkmark)
  - Rating: "4.9 (128 reviews)" (stars)
  - Badges: "IELTS Examiner", "Band 9.0", "8 years exp"
  - Specialization tags: Speaking, Writing
  - Rate: "Included in Premium" or "$35/hr"
  - Next available: "Today 14:00, 16:00, 19:00"
  - Bio: 2-line preview
  - [View Profile] [Book Now]

#### Empty State
- "No tutors match your filters. [Clear Filters]"

---

### 10.2 Tutor Profile (`/tutors/:tutorId`)

**Page Purpose**: Detailed tutor information and booking

**Layout**: App Layout

**Components**:

#### Header Section
- Cover photo (optional)
- Profile photo (large, 120px)
- Name + Verification badges
- Rating (large) + Review count
- [Book Session] (primary, sticky on scroll)

#### Info Tabs
- **Overview**:
  - Bio (full text)
  - Video introduction (if available)
  - Specializations
  - Languages spoken
  - Teaching certificates (viewable images)
  - Experience timeline
- **Availability**:
  - Calendar view (week view)
  - Clickable time slots
  - Timezone selector
- **Reviews**:
  - Sort: Recent, Highest, Lowest
  - Review cards: Student name, rating, date, comment
  - Response from tutor (if any)
- **Pricing**:
  - Included sessions (Premium)
  - Additional session cost
  - Cancellation policy

#### Booking Panel (Right sidebar or bottom)
- Selected date/time display
- Session type: Speaking Practice / Writing Review / Mock Interview
- Focus areas (checkboxes from user's weak areas)
- Custom topic input
- [Confirm Booking]
- Cancellation policy note

---

### 10.3 Session Room (`/session/live/:sessionId`)

**Page Purpose**: Video session interface for tutoring

**Layout**: Video Layout (full screen, minimal UI)

**Components**:

#### Video Area
- **Main**: Tutor video (large)
- **Picture-in-picture**: Student video (bottom right, draggable)
- **Background**: Blur or virtual background options

#### Control Bar (Bottom)
- [Mute/Unmute] microphone
- [Enable/Disable] camera
- [Share Screen] (tutor only)
- [Chat] toggle
- [Record] (if enabled)
- [End Session] (red, with confirmation)

#### Side Panel (Collapsible)
- **Chat Tab**:
  - Text messages
  - File sharing
  - Links shared
- **Notes Tab**:
  - Shared notepad (collaborative)
- **Materials Tab**:
  - Uploaded files
  - Screen captures

#### Session Info Header
- Tutor name + session type
- Timer: "Session time: 00:23:45"
- Connection quality indicator

#### Pre-Screen
- Camera preview
- Audio test
- [Join Session]

#### Post-Session
- Rating: 1-5 stars
- Feedback text area
- "Would you book again?"
- [Submit Feedback]

---

### 10.4 Essay Review Request (`/tutors/essay-review`)

**Page Purpose**: Submit writing for human review

**Layout**: App Layout

**Components**:
- **Header**: "Essay Review by Expert Tutor"
- **Credits**: "1 review included this month"
- **Form**:
  - Task Type: Task 1 or Task 2
  - Prompt: Text area or select from past tests
  - Essay Text: Paste or upload
  - Word count display
  - Focus areas: Task Response, Coherence, Vocabulary, Grammar
  - Preferred tutor: [Any] or [Select from list]
  - Deadline: [Standard 48h] [Express 24h]
- [Submit for Review]
- **Status Page**:
  - Submitted → Assigned to Tutor → In Review → Complete
  - Estimated completion time
  - [View Review] when complete

---

## 11. Community Pages

### 11.1 Community Home (`/community`)

**Page Purpose**: Forum and community engagement

**Layout**: App Layout

**Components**:

#### Page Header
- Title: "Community"
- Subtitle: "Learn together with fellow IELTS students"
- Actions: [New Thread] (Basic+), [Search]

#### Category Grid
- **Cards**:
  - Speaking Tips (icon: mic, thread count)
  - Writing Feedback (icon: pen)
  - Reading Strategies (icon: book)
  - Listening Practice (icon: headphones)
  - Study Groups (icon: users)
  - General Discussion (icon: message)
- **Each**: Icon, Title, Description, Thread count, Last activity

#### Trending Threads
- List: Title, Author, Replies, Views, Last reply time
- [View All]

#### Your Activity
- Threads you started
- Threads you replied to
- Unread replies

---

### 11.2 Forum Thread (`/community/thread/:threadId`)

**Page Purpose**: View and participate in discussion

**Layout**: App Layout

**Components**:

#### Thread Header
- Breadcrumb: Community > Category > Thread Title
- Title: H1
- Tags: Category badges
- Author info: Avatar, name, level badge, post date
- [Follow Thread] [Share]

#### Post Content
- Formatted text (markdown support)
- Embedded images (if any)
- Like count + [Like] button

#### Replies Section
- Sort: Oldest, Newest, Most Liked
- **Each Reply**:
  - Author info (avatar, name, role badge)
  - Post date
  - Content
  - Like button + count
  - [Reply] (nested threading, 2 levels max)
  - [Report] (dropdown)
- **Mentor Badge**: Tutor/Expert replies highlighted

#### Reply Composer
- Text area with markdown toolbar
- [Post Reply] (Basic+)
- **Free user**: "Upgrade to Basic to post replies. [Upgrade]"

#### Related Threads
- "You might also like..."
- 3 thread cards

---

### 11.3 Leaderboard (`/community/leaderboard`)

**Page Purpose**: Gamification and motivation

**Layout**: App Layout

**Components**:

#### Tabs
- **Global**: All users
- **Weekly**: This week's rankings
- **Monthly**: This month's rankings
- **Institution** (School Student): Org-only

#### Rankings Table
- **Columns**: Rank, User, Tests Taken, Avg Band, Streak, XP
- **Highlight**: Current user's row (sticky if outside top 10)
- **Top 3**: Special styling (gold, silver, bronze)
- **Avatars**: Clickable to public profile

#### User Card (Hover)
- Mini profile popup
- Quick stats
- [View Profile]

---

## 12. Account & Settings Pages

### 12.1 Profile Settings (`/settings/profile`)

**Page Purpose**: Manage personal and IELTS profile

**Layout**: App Layout (Settings sub-layout with left menu)

**Components**:

#### Settings Navigation (Left Sidebar)
- Profile (active)
- Account & Security
- Notifications
- Privacy
- Display
- Subscription & Billing (if applicable)
- Data Export
- Delete Account

#### Profile Form
- **Profile Photo**:
  - Current photo (circle, 100px)
  - [Change] [Remove]
  - Upload modal: Crop, max 2MB
- **Personal Info**:
  - Full Name: [________________]
  - Email: [________________] [Verify button if unverified]
  - Country: [Dropdown]
  - Native Language: [Dropdown]
  - Date of Birth: [Date picker]
- **IELTS Profile**:
  - Target Band Score: Slider (4.0 - 9.0)
  - Target Date: Date picker
  - Test Type: [Academic] [General Training]
  - Current Level: [Beginner] [Intermediate] [Advanced]
- **Weak Areas** (self-reported):
  - Checkboxes: Listening S4, Writing Task 2, Speaking Part 3, etc.
- [Save Changes] button

#### Validation
- Email uniqueness check on blur
- Date validation (target date must be future)
- Success toast: "Profile updated successfully"

---

### 12.2 Account Settings (`/settings/account`)

**Components**:
- **Password Section**:
  - Current password: [________________]
  - New password: [________________]
  - Confirm: [________________]
  - [Change Password]
- **Two-Factor Authentication**:
  - Status: Enabled/Disabled
  - [Setup 2FA] or [Disable]
  - QR code modal for setup
- **Connected Accounts**:
  - Google: Connected [Disconnect]
  - Apple: Not connected [Connect]
  - Facebook: Connected [Disconnect]
- **Login History**:
  - Table: Device, Location, IP, Time
  - [Revoke All Sessions]

---

### 12.3 Notification Settings (`/settings/notifications`)

**Components**:
- **Email Notifications**:
  - [x] Test results ready
  - [x] Weekly progress summary
  - [x] Study reminder (daily at [09:00] time picker)
  - [ ] Marketing & promotions
  - [x] New features & updates
  - [x] Tutor session reminders
- **Push Notifications** (Mobile app only, web push):
  - [x] Study reminders
  - [x] Test deadline alerts
  - [x] Achievement unlocked
  - [ ] Leaderboard updates
- **In-App Notifications**:
  - [x] All activity
  - [x] Mentions only
- **Digest Mode**:
  - [o] Real-time [o] Daily digest [o] Weekly digest
- [Save Preferences]

---

### 12.4 Privacy Settings (`/settings/privacy`)

**Components**:
- **Profile Visibility**:
  - [o] Private (only me)
  - [o] Friends (share with connections)
  - [o] Public (appear on leaderboards)
- **Data Usage**:
  - [x] Allow AI training on my anonymized data
  - [x] Show me personalized recommendations
- **Data Export**:
  - [Download My Data (GDPR)] button
  - Status: "Preparing export..." / "Ready — download link valid for 7 days"
- **Account Deletion**:
  - Warning card (red border)
  - [Request Account Deletion]
  - Confirmation modal: Type "DELETE" to confirm
  - 30-day grace period notice

---

### 12.5 Display Settings (`/settings/display`)

**Components**:
- **Theme**:
  - [o] Light [o] Dark [o] System default
- **Language**:
  - UI Language: [Dropdown — 15 languages]
  - Test Content Language: [Dropdown]
- **Accessibility**:
  - [x] Reduce motion
  - [x] High contrast mode
  - Font size: [Small] [Default] [Large] [Extra Large]
  - [x] Screen reader optimizations
- **Test Interface**:
  - [x] Show timer
  - [x] Sound notifications
  - [x] Auto-save indicator
  - [ ] Extended time (+25%)

---

## 13. Subscription & Billing Pages

### 13.1 Subscription Management (`/settings/subscription`)

**Page Purpose**: Manage plan, billing, and upgrades

**Layout**: App Layout

**Components**:

#### Current Plan Card
- **Plan Name**: Basic / Premium / Free
- **Badge**: Active, Cancelled, Expired, Grace Period
- **Price**: $19.99/month
- **Billing Cycle**: Monthly / Annual
- **Next billing date**: June 20, 2026
- **Payment method**: Visa ending in 4242

#### Plan Features List
- Checklist of included features
- Usage stats: "12/20 tests used this month"

#### Actions
- [Upgrade Plan] (if Free or Basic)
- [Downgrade Plan] (if Premium → Basic)
- [Cancel Subscription] (with confirmation modal)
- [Change Billing Cycle] (Monthly ↔ Annual)

#### Cancellation Flow
- Step 1: "We're sorry to see you go"
  - Reason dropdown: Too expensive, Not using enough, Found alternative, Missing features, Other
  - Comment text area
- Step 2: Retention offer (conditional)
  - "How about 50% off for 3 months?"
  - [Accept Offer] [Continue Cancelling]
- Step 3: Confirm
  - "You'll lose access on [date]"
  - [Keep Subscription] [Confirm Cancellation]

---

### 13.2 Payment Methods (`/settings/payment-methods`)

**Components**:
- **Saved Cards**:
  - Card icon (Visa/MC/Amex)
  - Last 4 digits
  - Expiry date
  - Default badge
  - [Set Default] [Remove]
- **Add New Card**:
  - Card number, expiry, CVC, name
  - Stripe Elements integration
  - [Save Card]
- **Alternative Methods**:
  - PayPal [Connect]
  - Apple Pay [Setup]
  - Google Pay [Setup]

---

### 13.3 Billing History (`/settings/billing`)

**Components**:
- **Table**: Date, Description, Amount, Status, Invoice
- **Filter**: Date range, Status
- **Actions**: [Download Invoice] (PDF)
- **Empty**: "No billing history yet."

---

### 13.4 Checkout (`/checkout/:plan`)

**Layout**: Minimal Layout (no sidebar)

**Components**:
- **Order Summary** (left):
  - Plan name, price, billing cycle
  - Features list
  - Total
- **Payment Form** (right):
  - Card details (Stripe)
  - Billing address (auto-filled from profile)
  - [x] I agree to terms
  - [Pay $19.99] button
  - Secure payment badges
- **Success**: "Payment successful! [Go to Dashboard]"
- **Error**: Stripe error message + [Retry]

---

## 14. Admin Portal Pages

### 14.1 Admin Dashboard (`/admin/dashboard`)

**Page Purpose**: Platform health and overview

**Layout**: Admin Layout

**Components**:

#### Top Stats Row (6 cards)
- Active Users (24h): 8,432 ↑12%
- Tests Completed (24h): 3,891 ↑8%
- Revenue (MTD): $52,400 ↑15%
- AI Cost (Today): $1,247 ⚠️ 80% of budget
- API Error Rate: 0.12%
- DB Replication Lag: 23ms

#### Charts Section
- **User Growth**: Line chart (7/30/90 days)
- **Revenue Breakdown**: Stacked bar by plan
- **Test Volume**: Area chart by module
- **AI Cost Trend**: Line chart with budget line

#### Alerts Feed
- Warning: AI cost at 80%
- Info: 3 new tutor applications
- OK: Daily backup completed
- [View All Alerts]

#### Quick Actions
- [User Management]
- [Question Bank]
- [AI Monitor]
- [Support Tickets]
- [System Config]

#### Recent Activity
- Table: Action, User, Time, IP
- Admin log entries

---

### 14.2 User Management (`/admin/users`)

**Components**:
- **Filter Bar**:
  - Search: Name, email, user ID
  - Plan: All, Free, Basic, Premium, Institution
  - Status: Active, Suspended, Banned, Inactive
  - Date range
- **Data Table**:
  - Checkbox (bulk actions)
  - User ID
  - Name + Avatar
  - Email
  - Plan (badge)
  - Tests taken
  - Last active
  - Status
  - Actions: [View] [Edit] [Suspend] [Impersonate] [Delete]
- **Bulk Actions**: Change plan, Send email, Export CSV
- **Pagination**: 50/100/200 per page

#### User Detail (`/admin/users/:userId`)
- **Profile Card**: Avatar, name, email, plan, join date
- **Tabs**:
  - **Overview**: Stats, recent activity, devices
  - **Tests**: Full test history table
  - **Billing**: Subscriptions, payments, invoices
  - **Activity Log**: Login history, actions
  - **Support**: Tickets raised
  - **Notes**: Admin-only notes (editable)
- **Actions**: [Edit User] [Reset Password] [Impersonate] [Suspend] [Delete]

---

### 14.3 Question Bank (`/admin/questions`)

**Components**:
- **Tabs**: Overview | Quality Review | Pre-Gen Pool | AI Logs
- **Stats Cards**: Total sets, Pending review, Flagged, Approved
- **Filter**: Module, Type, Difficulty, Quality score, Status
- **Table**:
  - Set ID
  - Module/Type
  - Difficulty
  - Quality Score (color-coded)
  - Usage count
  - Status (Approved, Pending, Flagged, Deprecated)
  - Last modified
  - Actions: [View] [Edit] [Regenerate] [Deprecate]

#### Question Editor (`/admin/questions/:setId`)
- **Split View**:
  - Left: Question list navigator
  - Center: Editor
    - Audio player (Listening)
    - Passage text (Reading)
    - Question text
    - Answer options
    - Correct answer selector
    - Explanation text area
    - Difficulty slider
  - Right: Metadata & Tools
    - Set ID, Module, Type
    - Quality score
    - Usage stats
    - Flags & Issues
    - [Save] [Save & Approve] [Regenerate with AI]
- **Version Control**: History dropdown, diff view

---

### 14.4 AI Monitor (`/admin/ai-monitor`)

**Components**:
- **Cost Dashboard**:
  - Today/This Month/This Year spend
  - Budget progress bar
  - Cost by model (Claude, GPT, Whisper, etc.)
  - Cost by endpoint (evaluation, generation, study plan)
- **Performance Metrics**:
  - Avg response time
  - Error rate
  - Queue depth
  - Timeout rate
- **Live Activity**:
  - Table: Request ID, User, Type, Model, Start time, Duration, Cost, Status
  - [Cancel Job] [Retry Failed]
- **Configuration**:
  - Model selection per task
  - Timeout settings
  - Rate limits
  - Fallback rules

---

### 14.5 Support Tickets (`/admin/support`)

**Components**:
- **Queue Stats**: Open, Pending, Resolved, Escalated counts
- **Filter**: Priority, Status, Category, Assignee, Date
- **Table**:
  - Ticket ID
  - Subject
  - User (name + plan)
  - Priority (Critical/High/Medium/Low badge)
  - Status
  - Created
  - Assignee
  - Actions: [View] [Assign to me] [Escalate]

#### Ticket Detail (`/admin/support/:ticketId`)
- **Header**: Subject, ID, Status, Priority
- **User Context Card**: Profile summary, plan, recent tests, account status
- **Internal Tools**: [View User Profile] [Impersonate] [View Logs] [Refund/Compensate]
- **Conversation Thread**:
  - User messages (left, white)
  - Agent messages (right, blue)
  - System events (center, gray)
  - Attachments
- **Reply Composer**:
  - Text area with templates
  - [Send & Resolve] [Send & Keep Open] [Escalate to Engineering]
- **Ticket Actions**:
  - Change status
  - Change priority
  - Assign to agent
  - Add internal note
  - Link to related ticket

---

### 14.6 System Config (`/admin/system`)

**Components**:
- **Feature Flags**: Toggle table (flag name, description, status per plan)
- **API Configuration**: Endpoints, keys, rate limits
- **Email Templates**: List + editor
- **Maintenance Mode**: Toggle + message editor
- **Backup Status**: Last backup, next scheduled, [Run Now]
- **Environment Info**: Version, build date, commit hash

---

## 15. Institution Portal Pages

### 15.1 Institution Dashboard (`/institution/dashboard`)

**Page Purpose**: Organization-level overview

**Layout**: App Layout (branded)

**Components**:
- **Branded Header**: Org logo, name
- **License Status**: "200/200 students used"
- **Date Range Filter**: Last 7/30/90 days

#### Stats Row
- Active Students: 187/200
- Tests Taken: 1,247
- Avg Band Score: 6.2 ↑+0.4
- Completion Rate: 78%
- At-Risk Students: 12

#### Charts
- **Student Activity**: Bar chart (active per day)
- **Band Distribution**: Histogram
- **Module Performance**: Radar chart

#### Student Leaderboard
- Top 10 students
- Columns: Rank, Name, Band, Tests, Improvement
- [View Full Rankings]

#### Actions
- [Export Report]
- [Message All]
- [Assign Test]
- [Manage Students]

---

### 15.2 Student Management (`/institution/students`)

**Components**:
- **Actions Bar**: [Add Student] [Bulk Import] [Export CSV]
- **Filter**: Status, Band range, Activity level, Assignment status
- **Table**:
  - Name + Email
  - Status: Active, Invited, Inactive
  - Joined date
  - Tests taken
  - Avg band
  - Last active
  - Assignments completed
  - Actions: [View] [Edit] [Reset Password] [Deactivate]
- **Bulk Actions**: Assign test, Send message, Export

#### Student Detail (`/institution/students/:studentId`)
- **Profile**: Name, email, join date, last login
- **Progress**: Band trend chart, test history
- **Assignments**: List with status
- **Activity**: Login times, time spent
- **Notes**: Instructor notes (editable)
- [Message Student] [Assign Test] [View Full Report]

---

### 15.3 Bulk Import (`/institution/students/import`)

**Components**:
- **Instructions**: CSV format template, required fields
- [Download Template]
- **Upload Area**: Drag & drop or file picker
- **Preview Table**: Parsed data validation
  - Valid rows (green check)
  - Invalid rows (red X, error message)
- **Field Mapping**: Map CSV columns to system fields
- [Send Invitations] [Save Draft]
- **Progress**: Uploading → Validating → Creating accounts → Sending emails

---

### 15.4 Assignments (`/institution/assignments`)

**Components**:
- **List View**: 
  - Assignment name
  - Type (Full test, Module)
  - Due date
  - Students assigned
  - Completion rate
  - Status (Active, Closed, Draft)
- [Create Assignment]

#### Create Assignment
- **Form**:
  - Name: [________________]
  - Description: Text area
  - Test Type: Full / Module / Specific set
  - Assign to: [All students] [Select students] [Groups]
  - Due date: Date + time picker
  - Allow late submissions: Toggle
  - Notify students: Toggle
- [Create] [Save as Draft] [Cancel]

---

### 15.5 Branding Settings (`/institution/branding`)

**Components**:
- **Organization Info**:
  - Name: [________________]
  - Subdomain: [________________].ielts-platform.com
  - Custom domain: [________________] (DNS instructions)
- **Visual**:
  - Logo upload (light + dark mode versions)
  - Primary color: Color picker
  - Secondary color: Color picker
  - Favicon upload
- **Preview**: Live preview of student dashboard
- [Save Changes] [Reset to Default]

---

### 15.6 Organization Analytics (`/institution/analytics`)

**Components**:
- **Date Range**: Custom picker
- **Report Types**: Overview, Student Progress, Test Performance, Engagement
- **Charts**:
  - Student growth
  - Test completion rate over time
  - Average band improvement
  - Module comparison
  - Time-of-day activity heatmap
- **Data Table**: Sortable, exportable
- [Export PDF Report] [Export CSV Data]

---

## 16. Support Pages

### 16.1 Contact Support (`/support/contact`)

**Page Purpose**: Create new support ticket

**Layout**: App Layout

**Components**:
- **Header**: "Contact Support"
- **Category Selection**:
  - [Technical Issue] [Billing Question] [Feature Request] [Account Help] [Report Bug]
- **Form**:
  - Subject: [________________]
  - Description: Text area (rich text optional)
  - Priority: [Low] [Medium] [High] (default Medium)
  - Attachments: File upload (max 5 files, 10MB each)
- **Context Auto-filled**:
  - Browser info, OS, account info
  - [x] Include diagnostic data
- [Submit Ticket]
- **Success**: "Ticket #4821 created. We'll respond within 48 hours."

---

### 16.2 My Tickets (`/support/tickets`)

**Components**:
- **Tabs**: Open | Resolved | All
- **List**:
  - Ticket ID, Subject, Category, Status, Last update, [View]
- **Status Badges**: Open (blue), Pending (yellow), Resolved (green), Escalated (red)
- **Empty**: "No tickets yet. [Contact Support]"

#### Ticket Detail (`/support/tickets/:ticketId`)
- **Header**: Subject, ID, Status, Created date
- **Conversation Thread**: Same as admin view but read-only for user
- **Reply Box**: User can add follow-up messages
- **Actions**: [Close Ticket] [Reopen] (if resolved)

---

## 17. Notification & Communication Pages

### 17.1 Notification Center (`/notifications`)

**Page Purpose**: View and manage all notifications

**Layout**: App Layout

**Components**:
- **Header**: "Notifications"
- **Tabs**: All | Unread | Mentions | System
- **Filter**: Date range, Type
- **Bulk Actions**: [Mark all read] [Clear all]
- **List**:
  - **Each Item**:
    - Icon (varies by type)
    - Title (bold if unread)
    - Message preview
    - Time
    - [Dismiss] (hover)
    - Click action: Navigate to relevant page
  - **Grouped**: Today, Yesterday, Earlier
- **Empty**: "No notifications" illustration
- **Settings Link**: [Notification Preferences]

---

## 18. Error & Utility Pages

### 18.1 404 Not Found

**Layout**: Minimal Layout

**Components**:
- Illustration: Lost/confused character or abstract shape
- Title: "Page Not Found"
- Message: "The page you're looking for doesn't exist or has been moved."
- [Go to Dashboard] [Go Back]
- Search bar: "Search for what you need..."
- Popular links: Dashboard, Tests, Help

---

### 18.2 403 Forbidden

**Components**:
- Illustration: Lock or barrier
- Title: "Access Denied"
- Message: "You don't have permission to access this page."
- [Go to Dashboard]
- If plan issue: "This feature requires Basic plan. [Upgrade]"

---

### 18.3 500 Error

**Components**:
- Illustration: Broken robot or warning
- Title: "Something Went Wrong"
- Message: "We're experiencing technical difficulties. Please try again later."
- [Refresh Page] [Contact Support]
- Error ID (for support reference): "Error #a1b2c3d4"

---

### 18.4 Maintenance Mode

**Components**:
- Illustration: Tools or construction
- Title: "We'll Be Back Soon"
- Message: "Scheduled maintenance in progress. Expected completion: 14:00 UTC"
- [Refresh] button
- Status page link (if external)

---

### 18.5 Browser Not Supported

**Components**:
- Title: "Browser Not Supported"
- Message: "Please upgrade to a modern browser for the best experience."
- Recommended browsers: Chrome, Firefox, Safari, Edge (with download links)
- [Continue Anyway] (at own risk warning)

---

## 19. Component Library Reference

### 19.1 Button Variants

| Variant | Style | Usage |
|---------|-------|-------|
| Primary | Filled, brand color | Main CTAs: Start Test, Submit, Upgrade |
| Secondary | Outlined, brand color | Alternative actions: Cancel, Back |
| Tertiary | Text only, brand color | Low priority: Skip, Learn more |
| Danger | Filled, red | Destructive: Delete, Cancel subscription |
| Ghost | Text, gray | Subtle: Dismiss, Close |
| Success | Filled, green | Positive: Mark complete, Save |
| Loading | Primary + spinner | Async actions |
| Disabled | Grayed out | Unavailable actions |

### 19.2 Form Components

**Text Input**:
- Label + required indicator
- Placeholder text
- Helper text below
- Error state: Red border + message
- Success state: Green checkmark
- Icon support (left/right)

**Select/Dropdown**:
- Native select (mobile)
- Custom dropdown (desktop)
- Searchable (for long lists)
- Multi-select (tags)

**Radio Cards**:
- Large touch targets
- Icon + title + description
- Selected: Border accent + checkmark

**Toggle Switch**:
- On/Off states
- Label right or left
- Disabled state

**Slider**:
- Min/max labels
- Step indicators
- Value tooltip on drag

### 19.3 Data Display Components

**Stat Card**:
- Label (small, gray)
- Value (large, bold)
- Trend (arrow + %)
- Icon (optional)

**Progress Ring**:
- SVG circle
- Percentage in center
- Color by status

**Badge**:
- Small pill
- Colors: Gray (neutral), Blue (info), Green (success), Yellow (warning), Red (danger), Purple (premium)

**Avatar**:
- Sizes: XS (24px), S (32px), M (40px), L (48px), XL (80px)
- Fallback: Initials on colored background
- Status dot: Online (green), Away (yellow), Offline (gray)

**Empty State**:
- Illustration (120px)
- Title: "No [items] yet"
- Description: 1-2 sentences
- CTA button (if applicable)

**Skeleton Loader**:
- Animated pulse
- Mimics content shape
- Used for tables, cards, charts

### 19.4 Feedback Components

**Toast/Notification**:
- Position: Top-right (desktop), bottom (mobile)
- Types: Success, Error, Warning, Info
- Auto-dismiss: 5 seconds
- Action button support

**Modal/Dialog**:
- Overlay: Black 50% opacity
- Panel: Centered, max-width 500px (small), 800px (large)
- Header: Title + [X]
- Content: Scrollable if needed
- Footer: Action buttons (right-aligned)
- Backdrop click to close (optional)

**Banner**:
- Full width, top of page
- Types: Info, Warning, Error, Success
- Dismissible [X]
- Can be sticky

**Tooltip**:
- Hover trigger
- Position: Top, bottom, left, right
- Max-width 200px
- Used for icons, truncated text

### 19.5 Navigation Components

**Breadcrumb**:
- Separator: "/" or ">"
- Current page: Plain text (not link)
- Max items: 4 (collapse middle if more)

**Pagination**:
- Previous/Next arrows
- Page numbers (max 7 visible)
- Ellipsis for gaps
- "Page X of Y" text
- Items per page selector

**Tabs**:
- Horizontal (default) or vertical
- Active: Underline or background
- Badge support on labels
- Scrollable on mobile

**Accordion**:
- Expand/collapse
- Single or multiple open
- Icon indicator: Chevron

---

## 20. Responsive Breakpoints

### Breakpoint Definitions

| Name | Width | Target |
|------|-------|--------|
| Mobile | < 640px | Phones |
| Tablet | 640px - 1024px | Tablets, small laptops |
| Desktop | 1024px - 1440px | Standard laptops |
| Wide | > 1440px | Large monitors |

### Layout Adaptations

**Mobile (< 640px)**:
- Sidebar becomes bottom navigation
- Tables become card lists
- Multi-column becomes single column
- Horizontal scroll for chart legends
- Modals become bottom sheets
- Test interface: Full screen, swipe between questions
- Font size: Base 14px

**Tablet (640px - 1024px)**:
- Collapsible sidebar (icons only default)
- 2-column grids
- Split views maintained but narrower
- Touch targets: Min 44x44px

**Desktop (1024px+)**:
- Full sidebar expanded
- Multi-column layouts
- Hover states active
- Context menus on right-click
- Keyboard shortcuts enabled

### Touch vs Mouse

**Touch Optimizations**:
- Larger buttons (min 48px)
- Swipe gestures for navigation
- Pull-to-refresh
- Bottom sheets instead of dropdowns
- Native date/time pickers

**Mouse Optimizations**:
- Hover tooltips
- Right-click context menus
- Drag and drop
- Precision inputs (sliders, color pickers)
- Keyboard shortcuts

---

## Appendix A: Page State Matrix

| Page | Empty | Loading | Error | Success | Partial |
|------|-------|---------|-------|---------|---------|
| Dashboard | No data cards | Skeleton | "Failed to load" | N/A | Free limits |
| Test History | "No tests" | Skeleton table | "Try again" | N/A | N/A |
| Test Interface | N/A | "Loading test..." | "Failed to start" | N/A | N/A |
| Results | N/A | "Calculating..." | "Evaluation failed" | Score display | Free locks |
| Progress | "Take tests" | Skeleton charts | "Failed to load" | N/A | N/A |
| Study Plan | "Create plan" | "Generating..." | "Failed" | Plan display | N/A |
| Tutors | "No tutors" | Skeleton cards | "Failed" | N/A | N/A |
| Community | "No threads" | Skeleton | "Failed" | N/A | N/A |
| Admin Users | "No users" | Skeleton table | "Failed" | N/A | N/A |
| Support | "No tickets" | Skeleton | "Failed" | "Ticket created" | N/A |

## Appendix B: Navigation Quick Reference

### Main Navigation (Left Sidebar)

**Student Menu**:
1. Dashboard (home icon)
2. Start Test (play icon)
3. My Tests (clipboard icon)
4. Progress (trending-up icon)
5. Study Plan (calendar icon) — Premium
6. Vocabulary (book icon) — Basic+
7. Mistakes (alert-circle icon) — Basic+
8. Tutors (users icon) — Premium
9. Community (message-square icon)
10. Leaderboard (trophy icon)
11. Settings (gear icon)

**Admin Menu**:
1. Dashboard (bar-chart icon)
2. Users (users icon)
3. Question Bank (database icon)
4. AI Monitor (cpu icon)
5. Analytics (pie-chart icon)
6. Financials (dollar-sign icon)
7. Support (life-buoy icon)
8. System (settings icon)
9. Tutors (user-check icon)
10. Announcements (bell icon)

**Institution Menu**:
1. Dashboard (home icon)
2. Students (users icon)
3. Assignments (clipboard icon)
4. Analytics (bar-chart icon)
5. Reports (file-text icon)
6. Branding (palette icon)
7. License (key icon)
8. API (code icon)

---

*Document Version: 1.0*
*Created: 2026-05-21*
*Status: Complete*
*Complements: IELTS-user-features-flows-complete.md*
