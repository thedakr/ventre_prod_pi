# Critical Architecture Notes: Don't Forget These!

**Date:** 2025-11-04
**Purpose:** Flag critical technical requirements that MUST be designed into MVP
**Owner:** AI Engineer (with PM/PI)

---

## 🚨 CRITICAL REQUIREMENT #1: Two-Tier Agent System

### The Challenge

**Participants are at different stages** in their entrepreneurial journey within the same cohort:
- Participant A: Validating problem
- Participant B: Building MVP
- Participant C: Figuring out pricing
- Participant D: Just starting with idea exploration

**Group meetings need to be valuable for ALL participants**, even though they're at different stages.

**Unifying elements:**
- Weekly management reports (action taken relevant to YOUR stage)
- Project reviews (progress, barriers, next actions)
- Peer learning and feedback
- Methodology principles (shared language across all stages)

### The Solution: Two-Tier Agent Architecture

**We need TWO types of agents working together:**

---

#### **Tier 1: Individual Participant Agents**

**Scope:** Know ONE participant's project deeply

**Agents:**
- Chief of Staff (for this participant)
- Market Discovery specialist (for this participant's customer)
- Methodology Coach (for this participant's stage)
- [Future: Product/Tech, Finance, etc.]

**What They Know:**
- This participant's business idea
- This participant's stage in methodology
- This participant's progress (conversation history)
- This participant's weekly management reports
- This participant's project reviews
- This participant's barriers and next actions

**What They DON'T Know:**
- Other participants' projects
- Group cohort dynamics
- Cross-participant patterns
- Facilitator context

**Persona:** "Your personal entrepreneurial team"

---

#### **Tier 2: Cohort/Group Agents**

**Scope:** Know ALL participants' projects, facilitate group dynamics

**Agents Needed:**
- **Cohort Facilitator Agent** - Orchestrates group sessions, manages discussion flow
- **Cross-Project Pattern Agent** - Identifies themes across multiple ventures
- **Peer Learning Agent** - Suggests connections between participants who could help each other
- [Potentially more as we discover needs]

**What They Know:**
- All participants' business ideas (at high level)
- All participants' current stages
- All participants' recent progress (from reports/reviews)
- Group dynamics (who's engaged, who's struggling, who's thriving)
- Cohort schedule and milestones
- Facilitator guidance and methodology

**What They DON'T Know:**
- Deep history of individual conversations (that's Tier 1's job)
- Granular details of each venture (too much context)

**Persona:** "Your cohort guide and connector"

---

### How They Work Together

#### **Individual Work (Between Sessions):**
```
Participant → Chief of Staff (Tier 1) → Routes to specialists (Tier 1)
              ↓
         Gets personalized guidance for THEIR venture
```

#### **Group Sessions:**
```
Facilitator opens discussion
    ↓
Cohort Facilitator Agent (Tier 2) knows:
- All participants' recent progress
- Who should share what
- What questions to ask
- What connections to draw
    ↓
Individual Participants share artifacts
    ↓
Cohort Facilitator Agent + Humans facilitate peer feedback
    ↓
Participants get help toward ACTION (using insights from peers + cohort agent)
    ↓
After session, individual agents (Tier 1) help implement next steps
```

#### **Reporting Artifacts:**
```
Participant works with Tier 1 agents all week
    ↓
Weekly Management Report generated (Tier 1 helps participant create)
    ↓
Report shared with Cohort Facilitator Agent (Tier 2)
    ↓
Cohort Agent synthesizes across all participants
    ↓
Group session structured around key themes/opportunities
```

---

### Technical Implementation Questions (For Engineer)

#### Data Architecture:
- **How do we structure data so Tier 1 and Tier 2 agents have appropriate context?**
  - Separate databases?
  - Same database with access controls?
  - Context windows with different scopes?

#### Agent Communication:
- **Do Tier 1 and Tier 2 agents communicate with each other, or only through user?**
  - Option A: Tier 1 agent publishes summary → Tier 2 reads
  - Option B: User explicitly shares with Tier 2
  - Option C: Tier 2 queries Tier 1 when needed

#### Context Management:
- **How do we prevent Tier 2 from having too much context (can't fit in prompt)?**
  - Summarization layer?
  - Only recent activity?
  - Embeddings/vector search for relevant context?

#### MVP Scope:
- **Do we build Tier 2 for MVP, or start with human facilitators + Tier 1 agents only?**
  - MVP v1: Human facilitators, Tier 1 agents only (simpler!)
  - MVP v2: Add basic Cohort Facilitator Agent (Tier 2)
  - Full vision: Both tiers working seamlessly

---

### Why This Matters

**Without two-tier system:**
- Individual agents can't facilitate group discussions (don't know others' contexts)
- Group facilitation requires human to manually synthesize all participants
- Scaling requires humans to maintain mental model of entire cohort

**With two-tier system:**
- Individual agents provide deep, personalized support
- Cohort agents enable group learning at scale
- Path to agentified facilitator (Phase 2 of vision)
- Foundation for synthetic cohort (Phase 3 of vision)

---

### Analogy: Sports Team

**Tier 1 Agents = Position Coaches**
- Quarterback coach works deeply with QB
- Knows every throw, every read, every improvement area
- Doesn't coach the whole team

**Tier 2 Agents = Head Coach**
- Sees whole field
- Understands how all positions work together
- Calls plays based on team dynamics
- Doesn't know every granular detail of each player

**Both are essential. Neither can do the other's job.**

---

## 🚨 CRITICAL REQUIREMENT #2: Meeting Cadence & Structure

### The Requirement

**2x per week group meetings** (e.g., Tuesday/Thursday)

**Why:**
- Keeps momentum high for participants
- More frequent check-ins = more accountability
- Faster feedback loops = faster learning
- 4-week cohort = 8 total group sessions (not 4)

**Implication for agents:**
- Weekly management reports become bi-weekly? Or still weekly?
- Project reviews happen more frequently?
- Cohort Facilitator Agent (Tier 2) needs to track twice-weekly rhythm

**Calendar:**
```
Week 1: Tuesday (Session 1), Thursday (Session 2)
Week 2: Tuesday (Session 3), Thursday (Session 4)
Week 3: Tuesday (Session 5), Thursday (Session 6)
Week 4: Tuesday (Session 7), Thursday (Session 8)
```

**Design Implication:**
- Session 1 = Orientation (how this works, meet peers, introduce agents)
- Sessions 2-7 = Action cycles (share progress, get help, commit to next actions)
- Session 8 = Cohort conclusion (celebrate progress, next steps beyond cohort)

---

## 🚨 CRITICAL REQUIREMENT #3: Artifact-Driven Workflow

### The Requirement

**Participants create tangible artifacts** that structure their work:

1. **Weekly Management Reports**
   - Action taken relevant to participant's current stage
   - Progress indicators
   - Completed during the week (with Tier 1 agent support)
   - Shared with cohort for group discussion

2. **Project Reviews**
   - Comprehensive status of venture
   - What action has been undertaken
   - Barriers needing help
   - Current next action plan
   - Used to GET HELP TOWARD ACTION in group sessions

**Agents must:**
- Help participants CREATE these artifacts (Tier 1 agents)
- Use these artifacts to FACILITATE group discussion (Tier 2 agents)
- Structure prompts around "action toward business creation"

**NOT traditional homework:**
- Not "read this chapter and answer questions"
- Not "complete this worksheet"
- It's: "What did you DO to move your business forward? What do you need help with next?"

---

## 🚨 CRITICAL REQUIREMENT #4: Methodology Fidelity

### The Requirement

**Agents must teach methodology correctly**, based on:
- 500+ documents from partner entrepreneurs
- Expert knowledge extraction (4 structured sessions)
- Methodology framework created by Methodology & Cohort Facilitator

**Validation:**
- Methodology & Cohort Facilitator tests agent responses against methodology
- Methodology experts review agent prompts
- Ongoing monitoring for drift

**Implication:**
- Agent prompts will evolve as methodology extraction progresses (Phases 1-3)
- Initial agents (Cohort 1) will be based on preliminary understanding
- Later cohorts will have refined prompts based on deeper methodology knowledge

**Engineer needs to:**
- Design prompts to be EASILY UPDATABLE (not hardcoded)
- Version control for prompts (track what methodology version each cohort used)
- A/B testing capability (test refined prompts against original)

---

## 🚨 CRITICAL REQUIREMENT #5: Stage-Agnostic Design

### The Requirement

**Participants enter at different stages**, and agents must meet them where they are:

**Possible stages:**
- Idea exploration (no clear idea yet)
- Problem validation (testing if problem is real)
- Customer discovery (finding and talking to potential customers)
- Solution design (figuring out what to build)
- MVP building (creating first version)
- Market testing (getting first customers)
- Pivoting (changing direction based on learning)
- Scaling (growing what works)

**Chief of Staff agent must:**
- Assess participant's current stage
- Route to appropriate specialist
- Track progression through stages (not linear!)
- Adapt guidance based on where participant is

**Specialist agents must:**
- Provide stage-appropriate advice
  - Market Discovery for idea stage: "Let's explore problems"
  - Market Discovery for MVP stage: "Let's test pricing"
- Not assume everyone is at same stage
- Not require completing previous stages (methodology may not be linear)

**Group sessions must:**
- Allow diverse stages to coexist
- Find common threads (methodology principles, action orientation)
- Use artifacts as unifying language

---

## 🚨 CRITICAL REQUIREMENT #6: Action Orientation

### The Requirement

**Every interaction must drive toward ACTION**, not just learning.

**Bad:**
- "Let me teach you about customer discovery" (lecture)
- "Read this article about market research" (passive)
- "Here's a framework to consider" (theoretical)

**Good:**
- "Who are 3 potential customers you could talk to THIS WEEK?" (action)
- "Let's draft the interview questions you'll ask them" (concrete)
- "After you talk to them, bring back what you learned and we'll decide next steps" (cycle)

**Agents must:**
- Always end with clear next action
- Make actions specific and time-bound
- Help participants DO things, not just think about things
- Use language like:
  - "Let's..."
  - "Your next step is..."
  - "By Thursday, you'll..."
  - "Here's how to do that..."

**Artifacts reflect actions taken:**
- Weekly Management Report: "This week I interviewed 5 potential customers. Here's what I learned..."
- Project Review: "I completed X, I'm stuck on Y, my next action is Z"

---

## 🚨 CRITICAL REQUIREMENT #7: Data Collection & Analytics Infrastructure

### The Requirement

**We need objective data** to measure participant progression, agent effectiveness, and hypothesis validation.

**Why This Matters:**
- Self-reported surveys are subjective
- Behavioral data shows actual usage patterns
- Enables data-driven iteration and improvement
- Validates whether agents actually help (not just novelty)

---

### **What Data to Capture**

#### **Must-Have for MVP:**

**1. Agent Conversations:**
- Participant ID, agent ID, timestamp, duration
- Message count, conversation depth
- Full conversation log (participant + agent messages)
- Week number, session context (pre/post group session)

**2. Artifacts Created:**
- Weekly management reports, project reviews
- Timestamp, completeness score
- Action count (how many actions mentioned?)
- External validation detected (talked to customers? Y/N)
- Full text of artifact

**3. Session Attendance:**
- Which of 8 sessions each participant attended
- Post-session feedback (helpfulness ratings)

**4. Milestones & Progression:**
- Venture stage (idea → validation → building → testing, etc.)
- Key achievements (first customer conversation, MVP launched, first sale)
- Actions planned vs. actions completed

**5. Agent Routing:**
- Chief of Staff stage assessments
- Which specialists were recommended
- Did participant follow routing?

---

### **Data Schema (Recommended)**

**Hybrid Approach: Event Log + Structured Tables**

**Event Log (capture everything):**
```sql
CREATE TABLE events (
  event_id UUID PRIMARY KEY,
  participant_id UUID,
  event_type VARCHAR(50), -- 'conversation', 'artifact', 'milestone', etc.
  timestamp TIMESTAMP,
  metadata JSONB -- flexible for event-specific data
);
```

**Structured Tables (queryable):**
```sql
CREATE TABLE conversations (
  conversation_id UUID PRIMARY KEY,
  participant_id UUID,
  agent_id VARCHAR(50),
  start_time TIMESTAMP,
  duration_seconds INT,
  message_count INT
);

CREATE TABLE artifacts (
  artifact_id UUID PRIMARY KEY,
  participant_id UUID,
  artifact_type VARCHAR(50),
  created_timestamp TIMESTAMP,
  completeness_score FLOAT,
  action_count INT,
  full_text TEXT
);

CREATE TABLE milestones (
  milestone_id UUID PRIMARY KEY,
  participant_id UUID,
  milestone_type VARCHAR(50),
  timestamp TIMESTAMP,
  description TEXT
);
```

**See DATA-ANALYTICS-PLAN.md for complete schema details.**

---

### **Export Functionality**

**For Cohort 1 (Manual Analysis):**
- [ ] Export all data to CSV format
- [ ] One CSV per table (conversations.csv, artifacts.csv, milestones.csv, etc.)
- [ ] Export button in admin interface or API endpoint

**Analysis Workflow:**
- CSV → Google Sheets / Excel / Python
- Calculate progression scores
- Visualize patterns
- AI-assisted analysis (feed data to LLM)

---

### **Key Metrics to Enable**

**The data above must support calculating:**

1. **Venture Progression Score (0-20 points):**
   - Stage advancement (5 pts)
   - External validation actions (5 pts)
   - Artifact consistency (3 pts)
   - Milestone achievement (5 pts)
   - Agent-driven action (2 pts)

2. **Agent Effectiveness:**
   - Usage rate (% who used each agent)
   - Usage depth (avg messages per conversation)
   - Return rate (% who came back 3+ times)
   - Correlation with outcomes

3. **Completion Rates:**
   - Attendance completion (≥6 of 8 sessions)
   - Engagement completion (attendance + artifacts + actions)
   - Outcome completion (attendance + progression ≥10)

4. **Hypothesis Validation:**
   - H1: Latent entrepreneurs exist (did they develop viable ideas?)
   - H2: Methodology fits demographic (was it accessible?)
   - H3: AI transforms practice (did agents drive progress?)

**See DATA-ANALYTICS-PLAN.md for complete metric definitions.**

---

### **Privacy & Consent**

**Critical Considerations:**
- [ ] Participants consent to data collection (informed consent form)
- [ ] Anonymize data for analysis (use participant IDs, not names)
- [ ] Secure storage (encrypt sensitive conversation data)
- [ ] Retention policy (how long do we keep logs?)
- [ ] IRB approval if this is research (consult legal/compliance)

**Underserved populations may have trust concerns** - be transparent about data use.

---

### **MVP Scope Decisions**

**Must Build for Cohort 1:**
- [x] Log all agent conversations (with timestamps, message counts)
- [x] Log artifact creation (management reports, project reviews)
- [x] Track session attendance
- [x] Store milestones/progression data
- [x] Export functionality (CSV)

**Should Build (If Time Permits):**
- [ ] Auto-calculate progression score
- [ ] Basic analytics queries (e.g., "show participants with score ≥15")
- [ ] NLP analysis of artifacts (detect actions, sentiment)

**Defer to Cohort 2:**
- [ ] Real-time dashboard (monitor during cohort)
- [ ] Automated alerts (flag inactive participants)
- [ ] Advanced analytics (predictive models)

---

### **Technical Questions for Engineer**

Before implementing, resolve:
- What database will you use? (Supabase, PostgreSQL, Firebase?)
- Event log vs. structured tables vs. both?
- How to handle conversation text storage? (full logs vs. summaries?)
- Export format preferences? (CSV, JSON, both?)
- Privacy/security approach? (encryption, access controls?)

**Recommendation:** Start simple (event log + CSV export), iterate to structured tables in Cohort 2.

---

### **Why This is Critical Requirement #7**

**Without data collection:**
- No objective measure of success (only subjective surveys)
- Can't validate which agents work (just guessing)
- Can't prove hypotheses (anecdotal only)
- Can't iterate effectively (don't know what to improve)

**With data collection:**
- Know which participants progressed (objectively)
- Know which agents drove outcomes (data-driven)
- Validate or refute hypotheses (evidence-based)
- Improve for Cohort 2 (targeted iterations)

**This isn't optional analytics - it's core to the learning mission of Cohort 1.**

---

## Design Principles Summary

When engineer designs agent architecture, these principles MUST be satisfied:

1. ✅ **Two-tier system** (individual + cohort agents)
2. ✅ **2x/week rhythm** (agent prompts account for this cadence)
3. ✅ **Artifact-driven** (agents create/use weekly reports & project reviews)
4. ✅ **Methodology fidelity** (prompts based on extracted methodology, easily updatable)
5. ✅ **Stage-agnostic** (agents adapt to where participant is, not prescriptive path)
6. ✅ **Action-oriented** (every interaction drives toward doing, not just learning)
7. ✅ **Data collection** (log everything for objective measurement and learning)

**If ANY of these are missing, the MVP won't work as intended.**

---

## Next Steps for Engineer

**Before starting development:**
- [ ] Review this document with PM/PI
- [ ] Decide MVP scope for two-tier system (both tiers or just Tier 1?)
- [ ] Design data architecture for context management
- [ ] Prototype Chief of Staff prompt (stage assessment + routing)
- [ ] Prototype artifact generation (weekly report template)
- [ ] Validate technical approach with PM before full implementation

**Questions to resolve:**
- How do we handle Tier 2 context window limits?
- Do we build Cohort Facilitator Agent for MVP, or wait until Cohort 2?
- How do agents access methodology knowledge (RAG? Fine-tuning? Long prompts?)
- What's the update process for agent prompts as methodology evolves?

---

**Last Updated:** 2025-11-04
**Owner:** AI Engineer + PM/PI
**Status:** ACTIVE - Must be incorporated into MVP design

---

**🚨 DON'T START BUILDING WITHOUT REVIEWING THESE REQUIREMENTS 🚨**
