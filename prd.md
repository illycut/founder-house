# 🧠 Product Requirements Document v1.1

### Project: **Black Ops VC – Chief of Staff Agent (CoSA)**

**Version:** 1.1
**Owner:** James Norman (Managing Partner)
**Date:** October 2025
**Last Updated:** October 30, 2025

---

## 1. Executive Summary

The **Black Ops Chief of Staff Agent (CoSA)** is an AI-powered operational system designed to streamline LP fundraising for Black Operator Ventures Fund II. It automates relationship intelligence, follow-up management, and personalized outreach—functioning as James Norman's always-on fundraising partner.

**Primary Goal:** Enable James to maintain 3x more active LP relationships while reducing context-switching time by 70%.

---

## 2. Core Problem Statement

**Current State:**
- LP relationships require manual research, context recall, and timely follow-ups
- No systematic way to identify warm introduction paths through professional networks
- Meeting notes and email threads scattered across tools (Fireflies, Gmail, Calendar)
- LPs fall through cracks when follow-ups exceed 40 days
- Fundraising cycles (8-18 months) require consistent engagement that's hard to maintain manually

**Desired State:**
- Automated LP intelligence gathering and profile enrichment
- Proactive follow-up recommendations with draft messages ready for review
- Network mapping to surface introduction paths and backchannels
- Zero LPs ignored beyond preferred follow-up windows
- James spends time on high-value conversations, not administrative overhead

---

## 3. Success Criteria (Phase 1 - Month 2)

| Metric | Target | Measurement |
|--------|--------|-------------|
| LP call auto-summarization | 100% within 24 hours | Fireflies → LP profile updates |
| AI-drafted follow-ups sent per week | 10+ reviewed/sent by James | Email tracking |
| LPs exceeding 40-day follow-up window | 0 (with valid next action) | CRM monitoring |
| Time saved on meeting prep | 70% reduction | Self-reported |
| Network connection visibility | 100% of LP orgs mapped | LinkedIn data cross-reference |

---

## 4. User Personas & Priorities

### Primary User
**James Norman** (Managing Partner)
- **Goals:** Close Fund II first close faster, maintain warm relationships with 50+ LP prospects
- **Pain Points:** Forgetting context between conversations, slow manual follow-up drafting
- **Success Looks Like:** "I never miss a follow-up, and every email feels personalized"

### Secondary Users
**Partners** (recipients of weekly retros)
- **Goals:** Stay informed on LP pipeline progress
- **Needs:** Friday summaries of completed meetings with recordings

---

## 5. Core Objectives

1. **Automate LP Intelligence**
   - Ingest and enrich LP profiles from multiple sources
   - Surface talking points, shared connections, and alignment with fund thesis

2. **Streamline Follow-Up Management**
   - Track interaction cadence (1 day post-meeting, 10 days post-materials, 25 days = stalled)
   - Generate follow-up reminders with draft emails ready for review

3. **Enable Network-Driven Outreach**
   - Map professional connections (1st and 2nd degree) to LP organizations
   - Identify warm introduction paths and backchannels

4. **Orchestrate Communication Workflows**
   - Draft personalized emails in James's tone (75-150 words, warm/professional)
   - Summarize Fireflies transcripts into actionable LP profile updates
   - Generate weekly digests (Sunday: week-ahead + follow-ups, Friday: retro)

5. **Maintain LP Pipeline Visibility**
   - Track stage progression (Prospect → Outreach → In Discussion → Verbal Interest → Committed)
   - Flag stalled opportunities (>25 days without engagement)

---

## 6. Functional Requirements

### 6.1 Data Model & Schema

**Organizations Table**
```
- id (uuid, primary key)
- name (text)
- owner (text) - GP assigned
- priority (enum: High/Medium/Low)
- stage (enum: Prospect/Outreach/In Discussion/Verbal Interest/Committed/Stalled/Pass)
- investor_type (enum: FO/Foundation/Endowment/Pension)
- check_size_min (integer)
- check_size_max (integer)
- thesis (text) - Investment focus areas
- fund_size (integer)
- location (text)
- upcoming_meeting (date)
- last_interaction (timestamp)
- days_since_contact (integer, computed)
- tags (array) - e.g., ["diverse managers", "black equity", "regional focus"]
- misc_notes (text)
```

**Contacts Table**
```
- id (uuid, primary key)
- first_name (text)
- last_name (text)
- email (text, unique)
- organization_id (uuid, foreign key → Organizations)
- position (text)
- location (text)
- timezone (text)
- linkedin_url (text)
- image_url (text)
- top_of_mind (text) - AI-generated context
- misc_notes (text)
```

**Backchannels Table** (relational)
```
- id (uuid, primary key)
- contact_id (uuid, foreign key → Contacts)
- backchannel_contact_id (uuid, foreign key → Contacts)
- relationship_type (text) - e.g., "former colleague", "mutual friend"
- notes (text)
```

**Interactions Table** (new)
```
- id (uuid, primary key)
- organization_id (uuid, foreign key)
- interaction_type (enum: Email/Call/Meeting/Deck View)
- date (timestamp)
- summary (text) - AI-generated
- participants (array of contact_ids)
- fireflies_url (text, nullable)
- email_thread_id (text, nullable)
- next_action (text)
- next_action_date (date)
```

**Network Connections Table** (new)
```
- id (uuid, primary key)
- gp_email (text) - james@blackopsvc.com or partner emails
- connection_name (text)
- connection_email (text)
- connection_linkedin (text)
- connection_company (text)
- relationship_strength (enum: 1st degree/2nd degree)
- last_updated (timestamp) - for monthly refresh tracking
```

**Reminders Table**
```
- id (uuid, primary key)
- organization_id (uuid, foreign key)
- reminder_date (date)
- reminder_type (enum: Follow-up/Meeting Prep/Custom)
- message (text)
- status (enum: Pending/Completed/Dismissed)
- draft_email (text, nullable) - AI-generated draft
```

---

### 6.2 Intelligence Layer

**LLM Configuration**
- **Model:** Llama 3 8B via Replicate API
- **Cost:** ~$0.10 per 1M tokens (estimated $50-150/month at scale)
- **Use Cases:**
  - Email draft generation
  - Meeting summary extraction
  - LP fit scoring
  - Network connection reasoning

**Vector Store**
- **Technology:** Supabase pgvector extension
- **Embeddings:** Text-embedding-ada-002 via OpenAI (or all-MiniLM-L6-v2 for cost savings)
- **Indexed Content:**
  - LP organization descriptions + thesis
  - Historical email threads
  - Meeting transcripts
  - Fund thesis + deck content

**Agent Capabilities**

1. **LP Fit Scoring Agent**
   - **Inputs:** Organization profile, fund thesis, check size requirements
   - **Outputs:** Fit score (0-100) + reasoning
   - **Logic:**
     - ✅ Check size matches fund minimum (HIGH weight)
     - ✅ Thesis alignment with early-stage software (MEDIUM weight)
     - ✅ DEI/Black founder focus (MEDIUM weight)
     - ✅ Recent fund activity (last 2 years) (LOW weight)
     - ❌ Sole focus on healthcare/FDA regulated (EXCLUSION)

2. **Email Drafting Agent**
   - **Style:** Warm, professional, 75-150 words
   - **Structure:**
     1. Brief personal touch or context reference
     2. Black Ops update (1 sentence)
     3. Clear call-to-action with timing suggestion
   - **Training Data:** 3 sample emails provided (Dale, Dana, Alexis examples)

3. **Meeting Summarization Agent**
   - **Input:** Fireflies transcript
   - **Output:** Structured summary stored in Interactions table
     - Key discussion points (3-5 bullets)
     - Next steps identified
     - Sentiment/interest level (High/Medium/Low)
     - Suggested follow-up timeline

4. **Network Mapping Agent**
   - **Input:** LP organization name + Network Connections table
   - **Output:** Introduction paths ranked by strength
     - 1st degree: "You know [Name] at [Org]"
     - 2nd degree: "Your connection [X] knows [Name] at [Org]"
   - **Display:** In LP profile dashboard with connection visualization

5. **Follow-Up Recommendation Engine**
   - **Logic:**
     - Stage = "Outreach" + days_since_contact > 1 → Draft follow-up
     - Stage = "In Discussion" + days_since_contact > 10 → Draft follow-up
     - Stage = "Verbal Interest" + days_since_contact > 10 → Draft follow-up
     - days_since_contact > 25 → Flag as "Stalled" + draft re-engagement
     - Active Reminder due within 7 days → Include in weekly digest

---

### 6.3 Integrations

**Gmail API (via cos@blackopsvc.com service account)**
- **Read Access:**
  - Scan sent/received emails for LP-related threads
  - Match email addresses to Contacts table
  - Extract engagement history (last email date, summary)
- **Write Access:**
  - Schedule approved draft emails for sending
- **Security:** OAuth 2.0 with read/compose scopes only

**Google Calendar API**
- **Read Access:**
  - All GP calendars shared with cos@blackopsvc.com
  - Extract upcoming meetings with LP contacts
  - Calculate days_since_contact from last meeting
- **Meeting Detection Logic:**
  - External attendee email matches Contacts table
  - Meeting duration >20 minutes
  - Attendee not @blackopsvc.com domain

**Fireflies.ai API**
- **Read Access:**
  - Fetch transcripts for meetings with LP contacts
  - Match meeting participants to Contacts via email
- **Trigger:** Agent checks for new transcripts daily at 8am PT
- **Processing:** Auto-summarize → update Interactions table

**LinkedIn Data (Manual Upload)**
- **Frequency:** Monthly CSV upload by James + partners
- **Fields Required:** Name, Email, Company, Position, Connection Date
- **Processing:**
  - Deduplicate across GP networks
  - Cross-reference with Organizations table
  - Store in Network Connections table with relationship_strength

---

### 6.4 User Interfaces

**1. Web Dashboard (React + Tailwind)**

**Home View:**
- **Today's Actions Panel**
  - LPs requiring follow-up (sorted by days overdue)
  - Upcoming meetings this week (with context cards)
  - Draft emails pending review (click to approve/edit/schedule)

**LP Pipeline View:**
- Kanban board: Prospect → Outreach → In Discussion → Verbal Interest → Committed
- Filters: Investor Type, Priority, Check Size, Tags
- Quick actions: "Generate Follow-Up", "View Network Path", "Schedule Meeting"

**LP Profile Detail View:**
- **Organization Info:** Name, Type, Check Size, Thesis, Stage, Priority
- **Key Contacts:** List with roles, LinkedIn links, last interaction
- **Network Connections:** Visual map showing "You know [X] who knows [Y]"
- **Engagement History:** Timeline of emails, calls, meetings with AI summaries
- **Meeting Notes:** Embedded Fireflies summaries with full recording links
- **Next Actions:** Upcoming reminders + suggested follow-up timeline
- **Quick Draft:** Button to generate contextualized email based on last interaction

**2. Email Notifications**

**Sunday 7pm PT → james@blackopsvc.com**
```
Subject: Week of [Date] - LP Prep & Follow-Ups

UPCOMING MEETINGS THIS WEEK
• Monday 10am - Acme Foundation (Sarah Chen) - Stage: In Discussion, Check: $500K-$1M
  Context: Follow-up from Sept deck review. They asked about portfolio diversity metrics.
  
• Wednesday 2pm - Hamilton Family Office (Lauren Jacobson) - Stage: Outreach, Check: $1M-$2M
  Context: First meeting via Michael Rodriguez intro. Focus on impact thesis alignment.

FOLLOW-UPS NEEDED (40+ days or reminder due)
• Dale Roberts (XYZ Foundation) - Last contact: 52 days ago - Stage: In Discussion
  Draft ready: "Hi Dale, checking in before holidays. Let's catch up on Fund II progress..."
  [Review Draft] [Schedule Send]

• Metro Pension Fund - Reminder set for Nov 2 - Stage: Verbal Interest  
  Draft ready: "Following up on our conversation about Q1 commitment timing..."
  [Review Draft] [Schedule Send]
```

**Friday 12pm PT → partners@blackopsvc.com**
```
Subject: Week of [Date] - LP Meeting Retro

MEETINGS COMPLETED THIS WEEK

📞 Tuesday - Acme Foundation (Sarah Chen)
Summary: Strong interest in Fund II structure. Requested LP reference list and side letter terms. Concern about fund size relative to check ($750K target). Next step: Send references by Friday.
Sentiment: High Interest
[View Full Recording] [View Notes]

📞 Thursday - New Endowment Group (Michael Torres)  
Summary: Early-stage conversation. Aligned on thesis but endowment committee doesn't meet until Q1. Asked to stay in touch monthly. Next step: Add to Jan follow-up list.
Sentiment: Medium Interest  
[View Full Recording] [View Notes]

PIPELINE SNAPSHOT
• In Discussion: 8 LPs ($6.5M potential)
• Verbal Interest: 3 LPs ($2M potential)
• This week: +2 new outreach, +1 moved to Verbal Interest
```

**3. Slack Integration (Optional Phase 2)**
- Post daily digest to #lp-pipeline channel
- Interactive commands: `/cosa draft [LP name]`, `/cosa summarize [meeting]`

---

### 6.5 Key Workflows

**Workflow 1: Post-Meeting Follow-Up**
1. Meeting occurs with LP contact (detected via Calendar API)
2. Agent checks Fireflies for transcript (next morning at 8am PT)
3. If transcript found:
   - Generate summary → store in Interactions table
   - Update Organization.last_interaction timestamp
   - Calculate next_action_date based on stage (1/10/10 day rules)
   - Create Reminder with draft email
4. James receives notification in Sunday digest or on-demand via dashboard

**Workflow 2: Manual Follow-Up Request**
1. James opens LP profile in dashboard
2. Clicks "Generate Follow-Up Email"
3. Agent retrieves:
   - Last 3 interactions from Interactions table
   - Organization context (thesis, stage, check size)
   - Network connections for personalization
4. LLM generates draft email (75-150 words, James's tone)
5. James reviews in dashboard:
   - "Send Now" → Sent via Gmail API
   - "Schedule Send [Date/Time]" → Queued
   - "Edit" → Manual revision → Save as approved draft

**Workflow 3: Weekly Digest Generation**
1. **Sunday 7pm PT cron job:**
   - Query Organizations with upcoming_meeting between [Today] and [+7 days]
   - Query Organizations with days_since_contact > 40 OR active Reminder due within 7 days
   - For each follow-up needed: Generate draft email if not already exists
   - Compile email with sections (Upcoming Meetings, Follow-Ups Needed)
   - Send to james@blackopsvc.com

2. **Friday 12pm PT cron job:**
   - Query Interactions where date between [Monday] and [Friday]
   - Group by organization
   - Include Fireflies recording links
   - Calculate pipeline snapshot stats
   - Send to partners@blackopsvc.com

**Workflow 4: Network Connection Discovery**
1. Monthly: James uploads LinkedIn connections CSV
2. Agent processes:
   - Parse CSV → deduplicate → store in Network Connections table
   - For each Organization in CRM:
     - Match organization.name or contacts.company to connection_company
     - Identify 1st degree connections
     - Flag potential warm intro paths
3. Display in LP profile: "🔗 You know [Name] at [Org]" or "🔗 [Your Connection] knows [Name] at [Org]"

**Workflow 5: Stalled LP Re-Engagement**
1. Daily cron checks: Organizations where days_since_contact > 25
2. If stage != "Pass" and stage != "Stalled":
   - Update stage to "Stalled"
   - Generate re-engagement email draft
   - Add to next Sunday digest with "STALLED" flag
3. James reviews draft and decides: Re-engage, Move to Pass, or Custom Action

---

## 7. Technical Architecture

### 7.1 System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND                              │
│  React + Tailwind (Vercel Hosting)                          │
│  - LP Dashboard    - Profile Views    - Draft Review        │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      │ HTTPS/REST API
                      │
┌─────────────────────▼───────────────────────────────────────┐
│                   BACKEND LAYER                              │
│  Supabase Edge Functions (Node.js)                          │
│  - API Routes    - Auth    - Cron Jobs                      │
└─────────┬───────────────────┬───────────────────────────────┘
          │                   │
          │                   │
┌─────────▼─────────┐  ┌──────▼──────────────────────────────┐
│   SUPABASE DB     │  │      AI ORCHESTRATION               │
│   (PostgreSQL)    │  │  - Replicate (Llama 3 8B)           │
│   - Organizations │  │  - pgvector (embeddings)            │
│   - Contacts      │  │  - LangChain agent logic            │
│   - Interactions  │  └─────────────────────────────────────┘
│   - Network Conn. │
│   - Reminders     │
└───────────────────┘
          │
          │
┌─────────▼─────────────────────────────────────────────────┐
│                  INTEGRATIONS                              │
│  - Gmail API (via cos@blackopsvc.com)                     │
│  - Google Calendar API (shared calendars)                 │
│  - Fireflies API (transcript fetch)                       │
│  - LinkedIn CSV Upload (manual, monthly)                  │
└────────────────────────────────────────────────────────────┘
```

### 7.2 Tech Stack Summary

| Layer | Technology | Cost (Monthly) | Notes |
|-------|-----------|----------------|-------|
| **Frontend** | React + Tailwind on Vercel | $0 (Hobby) | Upgrade to $20 if custom domain needed |
| **Backend** | Supabase Edge Functions | $25 (Pro tier) | Includes PostgreSQL + Auth |
| **Database** | Supabase PostgreSQL + pgvector | Included | 8GB storage, 5GB bandwidth |
| **LLM** | Llama 3 8B via Replicate | $50-150 | Pay-per-token, ~500K tokens/month est. |
| **Email** | Gmail API | $0 | Standard Google Workspace account |
| **Calendar** | Google Calendar API | $0 | Included with Workspace |
| **Transcripts** | Fireflies.ai | Existing | Assume already subscribed |
| **Monitoring** | Supabase Logs + Sentry | $0 (free tier) | Error tracking |
| **Total** | | **$75-195/month** | Well within $200-500 budget |

### 7.3 Security & Compliance

**Authentication:**
- Supabase Auth with email/password for dashboard access
- Row-level security policies (RLS) on all database tables
- API keys stored in environment variables (never committed to repo)

**Data Privacy:**
- All LP data encrypted at rest (Supabase default)
- TLS 1.3 for data in transit
- No LP data sent to OpenAI API (all inference via self-hosted Llama 3 on Replicate)
- Gmail OAuth limited to read/compose scopes (no delete/admin access)

**Audit Trail:**
- All AI-generated emails logged in Interactions table with `ai_generated: true` flag
- Email send actions logged with timestamp + user_id
- Draft edits tracked in revision history (for learning loop)

**Compliance:**
- No GDPR concerns (all LPs are institutional, not EU-based)
- No PII beyond business contact info (standard CRM use case)
- Fireflies transcripts stored with consent (assumed in business context)

---

## 8. Non-Functional Requirements

| Requirement | Specification | Priority |
|-------------|---------------|----------|
| **Latency** | <3s for dashboard loads, <10s for email draft generation | High |
| **Availability** | 99% uptime (Vercel + Supabase SLA) | Medium |
| **Scalability** | Support 500 LP orgs, 2K contacts, 10K interactions | High |
| **Auditability** | All AI actions logged with timestamps | High |
| **Maintainability** | Codebase documented, modular functions | Medium |
| **Cost Efficiency** | Stay within $200-500/month operational budget | High |

---

## 9. Implementation Roadmap

### Phase 1: MVP Foundation (Weeks 1-4)

**Week 1: Database & Auth Setup**
- [ ] Set up Supabase project + database schema (Organizations, Contacts, Interactions, Network Connections, Reminders)
- [ ] Enable Row Level Security (RLS) policies
- [ ] Create Supabase Auth user for James
- [ ] Deploy basic React frontend (Vercel) with login

**Week 2: Core Integrations**
- [ ] Integrate Gmail API (cos@blackopsvc.com service account)
- [ ] Integrate Google Calendar API (read shared calendars)
- [ ] Build Fireflies API connector (fetch transcripts)
- [ ] Create CSV upload endpoint for LinkedIn connections

**Week 3: AI Agent Development**
- [ ] Set up Replicate API + Llama 3 8B endpoint
- [ ] Build Email Drafting Agent (with 3 sample emails for tone training)
- [ ] Build Meeting Summarization Agent (Fireflies → structured summary)
- [ ] Implement LP Fit Scoring logic

**Week 4: Dashboard & Workflows**
- [ ] Build LP Pipeline view (Kanban board)
- [ ] Build LP Profile detail page with engagement history
- [ ] Implement "Generate Follow-Up Email" button → draft review interface
- [ ] Create cron jobs for Sunday/Friday email digests
- [ ] **🎯 LAUNCH MVP to James for testing**

### Phase 2: Intelligence & Automation (Weeks 5-8)

**Week 5: Network Mapping**
- [ ] Build Network Mapping Agent (1st/2nd degree connection discovery)
- [ ] Display connection paths in LP profile view
- [ ] Add "Request Intro" workflow (draft email to mutual connection)

**Week 6: Follow-Up Engine**
- [ ] Implement automatic stalled LP detection (>25 days)
- [ ] Build Follow-Up Recommendation Engine (1/10/10 day rules)
- [ ] Auto-generate follow-up drafts for stalled LPs

**Week 7: Engagement History Automation**
- [ ] Auto-detect LP-related emails (Gmail scan)
- [ ] Extract email summaries and store in Interactions
- [ ] Display email thread timeline in LP profile

**Week 8: Refinement & Learning**
- [ ] Implement draft edit tracking (learn from James's revisions)
- [ ] Add bulk actions (approve 5 drafts at once)
- [ ] Optimize LLM prompts based on feedback
- [ ] **🎯 PHASE 2 COMPLETE**

### Phase 3: Advanced Features (Weeks 9-12)

**Week 9: Deck Tracking & Analytics**
- [ ] Integrate DocSend API (track deck views if available)
- [ ] Build fundraising dashboard (committed vs. target, stage distribution)
- [ ] Add LP engagement heatmap (frequency of interactions)

**Week 10: Multi-Partner Support**
- [ ] Extend dashboard to support multiple GP logins
- [ ] Add LP assignment/ownership features
- [ ] Partner-specific follow-up queues

**Week 11: Voice Interface (Stretch)**
- [ ] Integrate Whisper API for voice notes → LP context
- [ ] "Hey CoSA, brief me on today's LP calls" voice command

**Week 12: Polish & Optimization**
- [ ] Performance optimization (reduce LLM API costs via caching)
- [ ] Mobile-responsive dashboard
- [ ] Onboarding documentation for team
- [ ] **🎯 PHASE 3 COMPLETE - FULL SYSTEM OPERATIONAL**

---

## 10. Key Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| **LLM hallucinations in email drafts** | High - could send incorrect info to LPs | Require James review before send; never auto-send |
| **Gmail API rate limits** | Medium - could delay email sending | Implement exponential backoff + queue system |
| **LinkedIn data staleness** | Low - connections become outdated | Monthly refresh reminder + last_updated tracking |
| **Fireflies transcript access issues** | Medium - missing meeting context | Fallback to manual note entry if API fails |
| **Cost overrun on Replicate API** | Medium - exceeds budget | Set usage alerts at $100/month, optimize prompt length |
| **Data privacy concerns from LPs** | Low - institutional investors understand CRM use | Ensure no third-party data sharing (keep Llama inference internal) |

---

## 11. Success Metrics (90-Day Review)

| Metric | Baseline (Current) | Target (90 Days) | Measurement Method |
|--------|-------------------|------------------|-------------------|
| Active LP relationships maintained | ~15-20 | 50+ | Organizations with <40 day last_interaction |
| Hours/week on LP admin tasks | ~10 hours | <3 hours | Self-reported time tracking |
| Follow-ups missed (>40 days) | ~30% of pipeline | <5% | CRM audit |
| Email drafts accepted without edit | N/A | >60% | Draft approval tracking |
| Fund II first close timeline | TBD | Accelerated by 60 days | Date of first close vs. original target |
| LP conversion rate (Outreach → Committed) | TBD | +20% vs. Fund I | Historical comparison |

---

## 12. Future Enhancements (Phase 4+)

**Quarter 2 Roadmap:**
- **Sentiment Analysis:** Track LP enthusiasm over time based on email/call tone
- **Competitive Intelligence:** Auto-scan for news about LP portfolio activity
- **LP Clustering:** Group LPs by persona (impact-first, returns-focused, emerging manager program)
- **Document Synthesis:** Auto-generate quarterly LP update newsletters
- **Side Letter Management:** Track custom LP terms and flag conflicts
- **Wire Reconciliation:** Match bank transfers to commitments (stretch goal, low priority)

**Long-Term Vision:**
- **Autonomous Outreach Mode:** Agent independently sends pre-approved message types (e.g., holiday greetings, portfolio updates)
- **Multi-Fund Support:** Extend to Fund III and SPVs
- **LP Co-Investment Portal:** Self-service portal for LPs to opt into deals
- **Voice Interface:** "Hey CoSA, who should I call today?" with smart prioritization

---

## 13. Appendix

### A. Sample Email Tone Analysis

**Characteristics of James's email style:**
1. **Opening:** Personal reference or time acknowledgment ("It's been a while", "Thanks for thinking to connect")
2. **Context:** Brief Black Ops update (1 sentence: fundraising progress, founder work)
3. **Value Prop:** Materials shared (deck, AGM recordings) with links
4. **CTA:** Specific timing suggestion ("late next week", "top of May", "before the holidays")
5. **Tone:** Warm but not overly casual, professional with personality
6. **Length:** 60-120 words typically

**Agent Training Prompt:**
```
You are drafting a follow-up email to an LP on behalf of James Norman, Managing Partner at Black Operator Ventures. Your style is warm, professional, and concise (75-150 words). 

Structure:
1. Personal opening that references last interaction or shared context
2. Brief Black Ops update (1 sentence) about fundraising progress or founder work
3. Clear call-to-action with specific timing for next conversation
4. Sign off with "Cheers" or "Look forward to hearing from you"

Avoid:
- Overly formal language ("I hope this email finds you well")
- Buzzwords or jargon
- Multiple asks in one email
- Paragraphs longer than 3 sentences

Context for this email:
[LP Name], [Last Interaction Summary], [Days Since Contact], [Current Stage], [Org Check Size]
```

### B. Network Connection Discovery Example

**Input:** Organization = "Hamilton Family Office"
**LinkedIn Connections CSV includes:**
- Michael Rodriguez | Founding Partner | Hamilton Family Office
- Sarah Lee | Investment Analyst | Hamilton Family Office

**Agent Output:**
```
🔗 Your Network at Hamilton Family Office:
1st Degree Connections:
• Michael Rodriguez (Founding Partner) - Connected since 2022
• Sarah Lee (Investment Analyst) - Connected since 2024

Suggested Action: Reach out to Michael for a warm intro to Lauren Jacobson (GP).
```

### C. Follow-Up Cadence Rules

| Stage | Trigger | Action | Timeline |
|-------|---------|--------|----------|
| **Outreach** | Meeting completed | Draft follow-up email | 1 day |
| **In Discussion** | Materials sent | Draft follow-up email | 10 days |
| **Verbal Interest** | Positive signal received | Draft check-in email | 10 days |
| **Any Stage** | No interaction | Flag as "Stalled" + re-engagement draft | 25 days |
| **Stalled** | Still no response | Move to "Pass" or manual outreach decision | 60 days |

### D. LP Fit Scoring Rubric

```python
def calculate_lp_fit_score(org):
    score = 0
    
    # Check size match (40 points)
    if org.check_size_min >= FUND_MINIMUM and org.check_size_max <= FUND_MAXIMUM:
        score += 40
    elif org.check_size_min >= FUND_MINIMUM * 0.5:
        score += 20
    
    # Thesis alignment (30 points)
    if "early-stage software" in org.thesis.lower():
        score += 15
    if "venture capital" in org.thesis.lower():
        score += 15
    
    # DEI/Black founder focus (20 points)
    if any(tag in ["diverse managers", "black equity", "impact"] for tag in org.tags):
        score += 20
    
    # Recent fund activity (10 points)
    if org.last_fund_commitment_year >= 2023:
        score += 10
    
    # Exclusions
    if org.thesis.lower() == "healthcare only" or "FDA" in org.thesis:
        score = 0  # Hard pass
    
    return min(score, 100)  # Cap at 100
```

---

## 14. Approval & Sign-Off

**Prepared By:** AI Product Architect (Claude)
**Reviewed By:** James Norman (Managing Partner, Black Ops VC)
**Approved for Development:** [Pending]
**Target MVP Launch Date:** Week of November 25, 2025

---

**Questions or feedback?** Contact james@blackopsvc.com
