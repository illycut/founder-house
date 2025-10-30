# 🏃 Black Ops CoSA - Sprint Plan

**Version:** 1.0  
**Last Updated:** October 30, 2025  
**Scrum Master:** TBD  
**Product Owner:** James Norman  
**Development Team:** Black Ops VC Engineering

---

## Table of Contents
1. [Sprint Overview](#sprint-overview)
2. [Team Capacity & Velocity](#team-capacity--velocity)
3. [Sprint 1: Foundation & Auth (Weeks 1-2)](#sprint-1-foundation--auth-weeks-1-2)
4. [Sprint 2: Core CRM & AI Email (Weeks 3-4)](#sprint-2-core-crm--ai-email-weeks-3-4)
5. [Sprint 3: Meeting Intelligence & Dashboard (Weeks 5-6)](#sprint-3-meeting-intelligence--dashboard-weeks-5-6)
6. [Sprint 4: Network Mapping & Gmail Integration (Weeks 7-8)](#sprint-4-network-mapping--gmail-integration-weeks-7-8)
7. [Sprint 5: Follow-Up Automation & Polish (Weeks 9-10)](#sprint-5-follow-up-automation--polish-weeks-9-10)
8. [Sprint 6: Analytics & Multi-Partner (Weeks 11-12)](#sprint-6-analytics--multi-partner-weeks-11-12)
9. [Risk Management](#risk-management)
10. [Sprint Ceremonies Schedule](#sprint-ceremonies-schedule)

---

## Sprint Overview

### Project Timeline
- **Start Date:** November 4, 2025
- **MVP Launch:** November 25, 2025 (Sprint 2 completion)
- **Phase 2 Complete:** December 23, 2025 (Sprint 4 completion)
- **Phase 3 Complete:** January 20, 2026 (Sprint 6 completion)

### Sprint Cadence
- **Sprint Duration:** 2 weeks
- **Total Sprints:** 6
- **Sprint Start:** Monday 9am PT
- **Sprint End:** Friday 5pm PT (2 weeks later)

### Definition of Ready (DoR)
A story is ready for sprint when:
- [ ] Acceptance criteria clearly defined
- [ ] Dependencies identified and resolved (or have mitigation plan)
- [ ] Story points estimated by team
- [ ] Technical approach discussed
- [ ] Design mockups available (if UI work)

### Definition of Done (DoD)
A story is complete when:
- [ ] Code written and committed to repo
- [ ] Unit tests written (>80% coverage for critical functions)
- [ ] Code reviewed and approved by peer
- [ ] Deployed to staging environment
- [ ] Manual QA testing completed
- [ ] Product Owner acceptance obtained
- [ ] Documentation updated

---

## Team Capacity & Velocity

### Team Composition
**Development Team:**
- Developer 1: Full-stack (Frontend + Backend) - 40 hours/week
- Developer 2: Full-stack (Backend + AI) - 40 hours/week

**Supporting Roles:**
- James Norman: Product Owner (5-10 hours/week for reviews/feedback)
- QA: Manual testing by team (no dedicated QA)

### Velocity Calculation
- **Available Hours per Sprint:** 80 hours (2 devs × 40 hours/week × 2 weeks ÷ 2 weeks)
- **Planned Velocity:** 40 story points per sprint (assumes 2 hours per point)
- **Velocity Buffer:** 20% for meetings, debugging, unforeseen issues
- **Target Commitment:** 32 story points per sprint

### Capacity Adjustments
- **Sprint 1:** Reduced to 25 points (setup overhead, learning curve)
- **Sprint 2-6:** Full 32 points (team at steady state)

---

## Sprint 1: Foundation & Auth (Weeks 1-2)

**Dates:** November 4 - November 15, 2025  
**Goal:** Establish technical foundation with database, auth, and deployment pipeline  
**Commitment:** 25 story points

### Sprint Objectives
1. ✅ Supabase database fully configured and deployed
2. ✅ User authentication working end-to-end
3. ✅ Frontend deployed to production with CI/CD
4. ✅ Replicate API integrated and tested

### Stories in Sprint

| Story ID | Story Title | Points | Assignee | Status |
|----------|-------------|--------|----------|--------|
| 1.1 | Set up Supabase project and database schema | 5 | Dev 2 | 🔴 Not Started |
| 1.2 | Implement authentication system | 3 | Dev 1 | 🔴 Not Started |
| 1.3 | Deploy frontend to Vercel | 2 | Dev 1 | 🔴 Not Started |
| 1.4 | Set up Replicate API for Llama 3 8B | 3 | Dev 2 | 🔴 Not Started |
| 2.1 | Build Organizations CRUD interface | 8 | Dev 1 | 🔴 Not Started |
| 2.2 | Build Contacts CRUD interface | 5 | Dev 2 | 🔴 Not Started |

**Total:** 26 points

### Daily Standup Schedule
- **Time:** 9:30am PT daily
- **Duration:** 15 minutes
- **Format:** 
  - What did I complete yesterday?
  - What will I work on today?
  - Any blockers?

### Sprint Ceremonies

#### Sprint Planning (Nov 4, 9am-11am PT)
**Agenda:**
1. Review sprint goal and capacity
2. Product Owner presents priority stories
3. Team estimates and commits to stories
4. Break down stories into tasks
5. Assign initial ownership

**Preparation Required:**
- Dev 2: Research Supabase best practices for RLS policies
- Dev 1: Review React + Tailwind component library options
- Both: Review database schema from PRD

#### Mid-Sprint Check-in (Nov 11, 2pm PT)
**Agenda:**
1. Review progress on stories (burndown)
2. Identify any blockers or risks
3. Adjust sprint scope if needed

#### Sprint Review (Nov 15, 2pm-3pm PT)
**Agenda:**
1. Demo completed stories to Product Owner
2. Stories to demo:
   - Login flow working
   - Create/view/edit organization
   - Create/view/edit contact
   - Llama 3 8B test prompt working
3. Gather feedback for next sprint

#### Sprint Retrospective (Nov 15, 3pm-4pm PT)
**Agenda:**
1. What went well?
2. What could be improved?
3. Action items for next sprint

### Dependencies & Risks

**Dependencies:**
- Supabase account provisioned (pre-work before sprint starts)
- Vercel account set up
- Replicate API key obtained
- GitHub repo created

**Risks:**
| Risk | Impact | Mitigation |
|------|--------|------------|
| Supabase learning curve steeper than expected | Medium | Dev 2 to complete Supabase tutorial over weekend before sprint |
| RLS policies complex to implement correctly | Medium | Start with permissive policies, harden in Sprint 2 |
| Vercel deployment issues | Low | Have backup deployment to Railway/Render |

### Success Metrics
- [ ] All team members can log in to dashboard
- [ ] Can create 10 test organizations with contacts
- [ ] Database schema matches PRD exactly
- [ ] Llama 3 8B responds to test prompts in <5 seconds

---

## Sprint 2: Core CRM & AI Email (Weeks 3-4)

**Dates:** November 18 - November 29, 2025  
**Goal:** Complete MVP with LP profiles, email drafting, and basic meeting intelligence  
**Commitment:** 32 story points

### Sprint Objectives
1. ✅ LP profile page fully functional
2. ✅ AI email drafting working end-to-end (generate → review → approve → send)
3. ✅ Fireflies integration auto-summarizing meetings
4. ✅ **MVP LAUNCH** - James can use system for real LP outreach

### Stories in Sprint

| Story ID | Story Title | Points | Assignee | Status |
|----------|-------------|--------|----------|--------|
| 2.3 | Build LP profile detail page | 8 | Dev 1 | 🔴 Not Started |
| 3.1 | Create email drafting agent prompt template | 3 | Dev 2 | 🔴 Not Started |
| 3.2 | Build "Generate Follow-Up Email" feature | 5 | Dev 2 | 🔴 Not Started |
| 3.3 | Build draft review and approval interface | 8 | Dev 1 | 🔴 Not Started |
| 3.4 | Implement scheduled email sending via Gmail API | 8 | Dev 2 | 🔴 Not Started |

**Total:** 32 points

### Key Milestones
- **Day 3 (Nov 20):** LP profile page UI complete
- **Day 5 (Nov 22):** First AI-generated email draft working
- **Day 8 (Nov 27):** Gmail API sending test email successfully
- **Day 10 (Nov 29):** **MVP LAUNCH** - James sends first AI-drafted email to real LP

### Sprint Ceremonies

#### Sprint Planning (Nov 18, 9am-11am PT)
**Agenda:**
1. Celebrate Sprint 1 completion
2. Review MVP definition with Product Owner
3. Discuss email tone requirements (James to provide 3 more sample emails)
4. Commit to stories and assign tasks

**Preparation Required:**
- James: Provide 3 additional sample emails for tone training
- Dev 2: Set up Gmail API sandbox environment
- Dev 1: Design LP profile wireframes

#### Mid-Sprint Check-in (Nov 25, 2pm PT)
**Focus:** Ensure email drafting is on track for launch

#### Sprint Review (Nov 29, 2pm-3pm PT)
**CRITICAL DEMO:**
1. James logs in
2. Opens LP profile for real LP
3. Clicks "Generate Follow-Up"
4. Reviews draft, makes minor edit
5. Approves and schedules send
6. Email successfully sent via Gmail

#### Sprint Retrospective (Nov 29, 3pm-4pm PT)
**Special Focus:** What do we need to support James post-launch?

### Dependencies & Risks

**Dependencies:**
- Gmail API OAuth set up for cos@blackopsvc.com
- Fireflies API key obtained
- James provides sample emails by Nov 18

**Risks:**
| Risk | Impact | Mitigation |
|------|--------|------------|
| Gmail API rate limits during testing | Medium | Use sandbox mode, request rate limit increase proactively |
| AI-generated drafts don't match James's tone | High | Build in edit workflow, iterate on prompts daily |
| Fireflies API unstable | Medium | Have manual note entry as fallback |

### Success Metrics
- [ ] James sends 5 AI-drafted emails to real LPs by Dec 1
- [ ] >50% of drafts approved without edits
- [ ] Zero email send failures
- [ ] James reports "this saves me 2+ hours this week"

---

## Sprint 3: Meeting Intelligence & Dashboard (Weeks 5-6)

**Dates:** December 2 - December 13, 2025  
**Goal:** Auto-summarize meetings and build operational dashboard with weekly digests  
**Commitment:** 32 story points

### Sprint Objectives
1. ✅ Fireflies transcripts automatically summarized
2. ✅ Dashboard shows "Today's Actions" and pipeline kanban
3. ✅ Sunday and Friday weekly emails working
4. ✅ James has complete visibility into follow-up queue

### Stories in Sprint

| Story ID | Story Title | Points | Assignee | Status |
|----------|-------------|--------|----------|--------|
| 4.1 | Integrate Fireflies API | 3 | Dev 2 | 🔴 Not Started |
| 4.2 | Build meeting summarization agent | 5 | Dev 2 | 🔴 Not Started |
| 4.3 | Auto-detect LP meetings from calendar | 8 | Dev 2 | 🔴 Not Started |
| 4.4 | Display meeting notes in LP profile | 5 | Dev 1 | 🔴 Not Started |
| 7.1 | Build pipeline kanban board | 8 | Dev 1 | 🔴 Not Started |
| 7.2 | Build "Today's Actions" dashboard panel | 8 | Dev 1 | 🔴 Not Started |

**Total:** 37 points (over capacity - will defer 7.2 if needed)

### Key Milestones
- **Day 3 (Dec 4):** Fireflies integration fetching transcripts
- **Day 5 (Dec 6):** First meeting auto-summarized
- **Day 7 (Dec 9):** Kanban board functional
- **Day 10 (Dec 13):** First Sunday digest sent

### Sprint Ceremonies

#### Sprint Planning (Dec 2, 9am-11am PT)
**Agenda:**
1. Review post-MVP feedback from James
2. Prioritize meeting intelligence vs. dashboard features
3. Discuss Google Calendar access and permissions

**Preparation Required:**
- Dev 2: Test Fireflies API with sample transcripts
- Dev 1: Review kanban board UI libraries (react-beautiful-dnd)

#### Mid-Sprint Check-in (Dec 9, 2pm PT)
**Focus:** Ensure weekly digest logic is correct

#### Sprint Review (Dec 13, 2pm-3pm PT)
**Demo:**
1. Show last week's LP meetings auto-summarized
2. Walk through kanban board (drag-and-drop stages)
3. Preview Sunday digest email
4. Show "Today's Actions" panel with follow-up queue

#### Sprint Retrospective (Dec 13, 3pm-4pm PT)

### Dependencies & Risks

**Dependencies:**
- Google Calendar API access to all GP calendars
- Fireflies API tested with real meeting transcripts
- Email templates designed (HTML)

**Risks:**
| Risk | Impact | Mitigation |
|------|--------|------------|
| Calendar API doesn't detect all LP meetings | High | Add manual override to tag meetings as LP-related |
| Fireflies transcript quality varies | Medium | Build confidence score, flag low-quality summaries for review |
| Cron jobs not triggering reliably | Medium | Use Supabase cron extension + external monitoring |

### Success Metrics
- [ ] 100% of LP meetings from this week auto-summarized
- [ ] Sunday digest delivered on time with accurate data
- [ ] James uses kanban board daily
- [ ] Zero false positives (internal meetings tagged as LP meetings)

---

## Sprint 4: Network Mapping & Gmail Integration (Weeks 7-8)

**Dates:** December 16 - December 27, 2025  
**Goal:** Enable warm intro discovery and complete engagement history tracking  
**Commitment:** 32 story points

### Sprint Objectives
1. ✅ LinkedIn connections uploaded and matched to LP orgs
2. ✅ Gmail emails auto-logged to engagement history
3. ✅ James can identify warm intro paths for every LP
4. ✅ Complete interaction timeline visible in LP profiles

### Stories in Sprint

| Story ID | Story Title | Points | Assignee | Status |
|----------|-------------|--------|----------|--------|
| 6.1 | Build LinkedIn CSV upload interface | 5 | Dev 1 | 🔴 Not Started |
| 6.2 | Build network connection matching algorithm | 8 | Dev 2 | 🔴 Not Started |
| 6.3 | Display network connections in LP profile | 5 | Dev 1 | 🔴 Not Started |
| 6.4 | Build "Request Intro" email generator | 5 | Dev 2 | 🔴 Not Started |
| 8.1 | Scan Gmail for LP-related emails | 8 | Dev 2 | 🔴 Not Started |
| 8.2 | Display email threads in engagement timeline | 5 | Dev 1 | 🔴 Not Started |

**Total:** 36 points (over capacity - will defer 8.2 if needed)

### Key Milestones
- **Day 2 (Dec 17):** James uploads first LinkedIn CSV
- **Day 5 (Dec 20):** Network matching identifies first warm intro path
- **Day 7 (Dec 23):** Gmail integration running in production
- **Day 10 (Dec 27):** Complete engagement history visible for all LPs

### Sprint Ceremonies

#### Sprint Planning (Dec 16, 9am-11am PT)
**Agenda:**
1. James to bring LinkedIn connections CSV
2. Discuss matching algorithm logic
3. Review Gmail API quotas and rate limits

**Preparation Required:**
- James: Download LinkedIn connections CSV
- Dev 2: Research LinkedIn CSV format variations
- Dev 2: Gmail API rate limit documentation

#### Mid-Sprint Check-in (Dec 23, 2pm PT)
**Focus:** Network matching accuracy validation

#### Sprint Review (Dec 27, 2pm-3pm PT)
**Demo:**
1. Upload LinkedIn CSV (James's real connections)
2. Show matched connections for sample LP
3. Generate "Request Intro" email draft
4. Show engagement timeline with emails from last 30 days

#### Sprint Retrospective (Dec 27, 3pm-4pm PT)
**Special Topic:** Year-end reflections, plan for Q1 2026

### Dependencies & Risks

**Dependencies:**
- Gmail API read permissions granted
- LinkedIn CSV format documented
- Company name normalization strategy (e.g., "Hamilton FO" vs "Hamilton Family Office")

**Risks:**
| Risk | Impact | Mitigation |
|------|--------|------------|
| LinkedIn CSV format varies by user | Medium | Build flexible parser that handles multiple formats |
| Company name matching too strict (misses matches) | High | Use fuzzy matching (Levenshtein distance) + manual review UI |
| Gmail API historical email volume too large | Medium | Limit initial scan to last 90 days, then incremental |

### Success Metrics
- [ ] >80% of James's connections correctly matched to LP orgs
- [ ] At least 5 warm intro paths identified
- [ ] 100% of emails from last 30 days logged
- [ ] James sends 1 "Request Intro" email via the system

---

## Sprint 5: Follow-Up Automation & Polish (Weeks 9-10)

**Dates:** December 30, 2025 - January 10, 2026  
**Goal:** Automate follow-up reminders and improve draft quality through learning  
**Commitment:** 32 story points

### Sprint Objectives
1. ✅ Auto-flag stalled LPs (>25 days)
2. ✅ Auto-generate follow-up reminders based on stage
3. ✅ Track draft edits for learning
4. ✅ System proactively surfaces LPs needing attention

### Stories in Sprint

| Story ID | Story Title | Points | Assignee | Status |
|----------|-------------|--------|----------|--------|
| 5.1 | Implement days_since_contact auto-calculation | 3 | Dev 2 | 🔴 Not Started |
| 5.2 | Auto-flag stalled LPs | 3 | Dev 2 | 🔴 Not Started |
| 5.3 | Auto-generate follow-up reminders | 5 | Dev 2 | 🔴 Not Started |
| 3.5 | Implement draft learning from edits | 8 | Dev 2 | 🔴 Not Started |
| 3.6 | Build bulk draft approval interface | 5 | Dev 1 | 🔴 Not Started |
| 7.3 | Implement Sunday weekly digest email | 5 | Dev 1 | 🔴 Not Started |
| 7.4 | Implement Friday retro email | 5 | Dev 1 | 🔴 Not Started |

**Total:** 34 points (slightly over - will adjust if needed)

### Key Milestones
- **Day 2 (Dec 31):** Stalled LP detection working
- **Day 5 (Jan 3):** First auto-generated reminder created
- **Day 7 (Jan 6):** Draft learning tracking in place
- **Day 10 (Jan 10):** Both weekly emails tested and running

### Sprint Ceremonies

#### Sprint Planning (Dec 30, 9am-11am PT)
**Note:** Adjusted for holiday week, flexible schedule

**Agenda:**
1. Review follow-up cadence rules (1/10/10/25 day logic)
2. Discuss draft learning approach
3. Finalize email template designs

**Preparation Required:**
- James: Review draft edits from last 30 days
- Dev 2: Research pattern detection in text edits
- Dev 1: HTML email template mockups

#### Mid-Sprint Check-in (Jan 6, 2pm PT)
**Focus:** Validate stalled LP detection accuracy

#### Sprint Review (Jan 10, 2pm-3pm PT)
**Demo:**
1. Show stalled LPs flagged automatically
2. Demonstrate auto-reminder creation after interaction
3. Show edit history tracking on sample draft
4. Preview both weekly digest emails

#### Sprint Retrospective (Jan 10, 3pm-4pm PT)

### Dependencies & Risks

**Dependencies:**
- Cron job infrastructure tested and reliable
- Email templates reviewed by James
- Sufficient draft edit data collected (need 20+ drafts)

**Risks:**
| Risk | Impact | Mitigation |
|------|--------|------------|
| Not enough draft edits to learn patterns | Medium | Manually analyze first 10 edits to seed learning |
| Weekly email cron jobs don't trigger on holidays | Low | Use external scheduler (GitHub Actions) as backup |
| Stalled LP logic flags too many false positives | Medium | Add "snooze" feature to dismiss false positives |

### Success Metrics
- [ ] Zero LPs fall through cracks (all >40 days have reminder)
- [ ] Draft acceptance rate improves from baseline
- [ ] Weekly emails delivered on time for 2 consecutive weeks
- [ ] Bulk approval used to approve 5+ drafts at once

---

## Sprint 6: Analytics & Multi-Partner (Weeks 11-12)

**Dates:** January 13 - January 24, 2026  
**Goal:** Add analytics, multi-partner support, and final polish for team-wide rollout  
**Commitment:** 32 story points

### Sprint Objectives
1. ✅ Fundraising analytics dashboard live
2. ✅ Partner assignment and filtering working
3. ✅ Mobile-responsive design complete
4. ✅ **PHASE 3 COMPLETE** - System ready for full team usage

### Stories in Sprint

| Story ID | Story Title | Points | Assignee | Status |
|----------|-------------|--------|----------|--------|
| 7.5 | Build fundraising analytics dashboard | 13 | Dev 1 | 🔴 Not Started |
| 9.1 | Add partner assignment to organizations | 5 | Dev 2 | 🔴 Not Started |
| 9.2 | Build partner-specific follow-up queues | 5 | Dev 2 | 🔴 Not Started |
| 10.3 | Build mobile-responsive views | 8 | Dev 1 | 🔴 Not Started |
| 10.4 | Create onboarding documentation | 8 | Both | 🔴 Not Started |

**Total:** 39 points (over capacity - will prioritize in planning)

### Key Milestones
- **Day 3 (Jan 15):** Analytics dashboard showing real data
- **Day 5 (Jan 17):** Partner assignments working
- **Day 8 (Jan 22):** Mobile design tested on iOS/Android
- **Day 10 (Jan 24):** **PHASE 3 LAUNCH** - Onboard full team

### Sprint Ceremonies

#### Sprint Planning (Jan 13, 9am-11am PT)
**Agenda:**
1. Review analytics requirements with James
2. Discuss multi-partner rollout strategy
3. Prioritize: analytics vs. mobile vs. documentation

**Preparation Required:**
- James: List of key metrics for analytics dashboard
- Dev 1: Research charting libraries (Recharts vs Chart.js)
- Both: Mobile device testing setup

#### Mid-Sprint Check-in (Jan 20, 2pm PT)
**Focus:** Mobile responsive testing feedback

#### Sprint Review (Jan 24, 2pm-3pm PT)
**FINAL PHASE 3 DEMO:**
1. Analytics dashboard with fund progress, pipeline health
2. Filter pipeline by partner
3. Mobile walkthrough (James tests on iPhone)
4. Documentation site walkthrough
5. **Celebrate project completion!**

#### Sprint Retrospective (Jan 24, 3pm-4pm PT)
**Project Retrospective:**
1. What worked well over 6 sprints?
2. What would we do differently next time?
3. Key learnings for future projects
4. Team appreciation and recognition

### Dependencies & Risks

**Dependencies:**
- Real data from 2+ partners to test multi-partner features
- Mobile devices for testing (iOS + Android)
- Documentation hosting (Notion or GitHub Pages)

**Risks:**
| Risk | Impact | Mitigation |
|------|--------|------------|
| Analytics queries too slow with real data volume | Medium | Optimize with materialized views or caching |
| Mobile design requires more work than estimated | Medium | Defer nice-to-have mobile features to post-launch |
| Documentation incomplete by launch | Low | Release v1 docs, iterate based on team feedback |

### Success Metrics
- [ ] Analytics dashboard loads in <2 seconds
- [ ] 2+ partners successfully using system
- [ ] Mobile navigation works on all key pages
- [ ] Team completes onboarding in <30 minutes
- [ ] **James reports: "This system has 3x'd my LP outreach capacity"**

---

## Risk Management

### Overall Project Risks

| Risk | Probability | Impact | Mitigation Strategy | Owner |
|------|-------------|--------|---------------------|-------|
| **LLM costs exceed budget** | Medium | High | Monitor Replicate spending weekly, cache responses, optimize prompts | Dev 2 |
| **Gmail API rate limits hit** | Medium | Medium | Request quota increase proactively, implement exponential backoff | Dev 2 |
| **AI drafts don't match James's tone** | High | Critical | Iterate on prompts daily in Sprint 2, collect feedback continuously | Dev 2 + James |
| **Scope creep delays MVP** | Medium | High | Strict prioritization, defer all non-MVP features to Phase 2 | James (PO) |
| **Database performance issues at scale** | Low | Medium | Benchmark queries early, add indexes proactively | Dev 2 |
| **Team member unavailable** | Low | High | Cross-train on critical systems, maintain good documentation | Both Devs |
| **Third-party API downtime (Fireflies/Gmail)** | Low | Medium | Build graceful fallbacks, manual entry options | Dev 2 |

### Risk Review Cadence
- **Weekly:** Check Replicate API costs during standup
- **Bi-weekly:** Review blockers and risks in mid-sprint check-in
- **Monthly:** Deep dive on project risks with Product Owner

---

## Sprint Ceremonies Schedule

### Recurring Meetings

#### Daily Standup
- **Frequency:** Every weekday
- **Time:** 9:30am - 9:45am PT
- **Attendees:** Dev 1, Dev 2
- **Optional:** James (as needed)
- **Format:** 
  - Yesterday's progress
  - Today's plan
  - Blockers

#### Sprint Planning
- **Frequency:** First Monday of each sprint
- **Time:** 9am - 11am PT
- **Attendees:** Dev 1, Dev 2, James (Product Owner)
- **Outputs:**
  - Sprint goal defined
  - Stories committed and assigned
  - Tasks broken down
  - Dependencies identified

#### Mid-Sprint Check-in
- **Frequency:** Monday of week 2 in sprint
- **Time:** 2pm - 2:30pm PT
- **Attendees:** Dev 1, Dev 2, James (optional)
- **Purpose:**
  - Review burndown chart
  - Address blockers
  - Adjust scope if needed

#### Sprint Review (Demo)
- **Frequency:** Last Friday of each sprint
- **Time:** 2pm - 3pm PT
- **Attendees:** Dev 1, Dev 2, James (required), Partners (optional)
- **Purpose:**
  - Demo completed stories
  - Gather Product Owner feedback
  - Discuss next sprint priorities

#### Sprint Retrospective
- **Frequency:** Last Friday of each sprint
- **Time:** 3pm - 4pm PT
- **Attendees:** Dev 1, Dev 2
- **Purpose:**
  - Reflect on process
  - Identify improvements
  - Generate action items

#### Backlog Grooming
- **Frequency:** Wednesday of week 2 in sprint
- **Time:** 3pm - 4pm PT
- **Attendees:** Dev 1, Dev 2, James
- **Purpose:**
  - Refine upcoming stories
  - Clarify acceptance criteria
  - Estimate story points

---

## Communication Plan

### Team Communication Channels

**Slack Channels:**
- `#cosa-dev` - Development discussion and updates
- `#cosa-alerts` - Automated alerts (deploys, errors, API issues)
- `#cosa-feedback` - James and partners provide product feedback

**Standup Updates Format:**
```
✅ Yesterday: Completed Story 1.1 - Supabase setup
🔄 Today: Starting Story 1.2 - Auth implementation
🚫 Blockers: None
```

**Weekly Status Report (Fridays):**
Sent to James every Friday 5pm PT:
```
Sprint X - Week 1 Status
━━━━━━━━━━━━━━━━━━━━━
✅ Completed: [X stories, Y points]
🔄 In Progress: [X stories, Y points]
🎯 On track for sprint goal: Yes/No
⚠️  Risks: [Any concerns]
📅 Next week focus: [Key deliverables]
```

---

## Tools & Infrastructure

### Development Tools
- **Code Repository:** GitHub (private repo)
- **Project Management:** GitHub Projects or Linear
- **CI/CD:** Vercel (frontend), GitHub Actions (backend jobs)
- **Monitoring:** Sentry (errors), Supabase Dashboard (database)
- **Documentation:** Notion or GitHub Wiki

### Environments
1. **Local Development:** Each dev's laptop
2. **Staging:** Vercel preview deployments + Supabase staging project
3. **Production:** Vercel + Supabase production project

### Deployment Strategy
- **Frontend:** Auto-deploy to Vercel on merge to `main`
- **Database:** Manual migrations via Supabase dashboard (reviewed before apply)
- **Rollback Plan:** Vercel instant rollback, database migrations versioned

---

## Success Criteria by Phase

### Phase 1 Success (End of Sprint 2)
- [ ] James sends 10+ AI-drafted emails to real LPs
- [ ] Zero system downtime
- [ ] >60% of drafts approved without edits
- [ ] All LP data from Airtable migrated
- [ ] James reports: "I'm saving 5+ hours/week"

### Phase 2 Success (End of Sprint 4)
- [ ] 100% of LP meetings auto-summarized
- [ ] Network connections mapped for 50+ LP orgs
- [ ] Gmail engagement history complete (last 90 days)
- [ ] Zero LPs exceed 40-day follow-up window
- [ ] Weekly digests delivered on time for 4 consecutive weeks

### Phase 3 Success (End of Sprint 6)
- [ ] 2+ partners actively using system
- [ ] Analytics dashboard shows real-time fund progress
- [ ] Mobile app usable on iOS/Android
- [ ] Team onboarded and trained
- [ ] James reports: "This system has 3x'd my LP relationship capacity"

---

## Post-Launch Support Plan

### Week 1 After Launch (Nov 26 - Nov 29)
- **Hotfix window:** Devs on-call for critical bugs
- **Daily check-in:** 15-min call with James to gather feedback
- **Priority:** Fix any issues blocking James from using system

### Week 2-4 After Launch (Dec 2 - Dec 20)
- **Weekly feedback session:** 30 min with James on Fridays
- **Iteration:** Address top 3 usability issues
- **Monitoring:** Track email send success rate, draft acceptance rate

### Ongoing Maintenance
- **Monthly:** Review and tune LLM prompts
- **Monthly:** James uploads fresh LinkedIn connections CSV
- **Quarterly:** Evaluate new feature requests (Icebox prioritization)

---

## Metrics Dashboard

### Sprint Burndown
Track daily during each sprint:
- Stories completed vs. committed
- Story points burned vs. remaining
- Projected completion date

### Velocity Tracking
| Sprint | Committed Points | Completed Points | Velocity |
|--------|------------------|------------------|----------|
| Sprint 1 | 25 | TBD | TBD |
| Sprint 2 | 32 | TBD | TBD |
| Sprint 3 | 32 | TBD | TBD |
| Sprint 4 | 32 | TBD | TBD |
| Sprint 5 | 32 | TBD | TBD |
| Sprint 6 | 32 | TBD | TBD |

### Quality Metrics
- **Bug Rate:** Bugs per story completed (target: <0.2)
- **Test Coverage:** % of code covered by tests (target: >80%)
- **Deployment Success Rate:** % of deploys without rollback (target: >95%)
- **Code Review Turnaround:** Hours from PR open to merge (target: <24 hours)

---

## Appendix: Sprint Calendar

### November 2025
```
Week 1 (Nov 4-8)   │ Sprint 1 - Week 1
Week 2 (Nov 11-15) │ Sprint 1 - Week 2
Week 3 (Nov 18-22) │ Sprint 2 - Week 1
Week 4 (Nov 25-29) │ Sprint 2 - Week 2 ⭐ MVP LAUNCH
```

### December 2025
```
Week 1 (Dec 2-6)   │ Sprint 3 - Week 1
Week 2 (Dec 9-13)  │ Sprint 3 - Week 2
Week 3 (Dec 16-20) │ Sprint 4 - Week 1
Week 4 (Dec 23-27) │ Sprint 4 - Week 2 (Holiday adjustments)
Week 5 (Dec 30-31) │ Sprint 5 - Week 1 (Short week)
```

### January 2026
```
Week 1 (Jan 1-3)   │ Sprint 5 - Week 1 (Holiday adjustments)
Week 2 (Jan 6-10)  │ Sprint 5 - Week 2
Week 3 (Jan 13-17) │ Sprint 6 - Week 1
Week 4 (Jan 20-24) │ Sprint 6 - Week 2 ⭐ PHASE 3 COMPLETE
```

---

## Change Log

| Version | Date | Changes | Author |
|---------|------|---------|--------|
| 1.0 | Oct 30, 2025 | Initial sprint plan created | Black Ops VC Engineering |

---
