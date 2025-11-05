# Data & Analytics Plan: Measuring Cohort Success

**Date:** 2025-11-04
**Purpose:** Define data collection, metrics, and analysis approach for Vibe Entrepreneurship
**Owner:** PM/PI + Methodology & Cohort Facilitator
**Technical Implementation:** AI Engineer

---

## Purpose

To **objectively measure** participant progression, agent effectiveness, and hypothesis validation using data from the web portal and cohort activities.

**Why This Matters:**
- Self-reported surveys are subjective (participants may not know what "progress" is)
- Behavioral data shows actual usage, not just stated preferences
- Objective metrics enable comparison across cohorts
- Data-driven iteration improves agents and methodology

---

## Data Collection Infrastructure

### **Web Portal Analytics (What to Capture)**

#### **1. Agent Interaction Data**

**For every agent conversation:**
```json
{
  "conversation_id": "uuid",
  "participant_id": "uuid",
  "agent_id": "chief_of_staff | market_discovery | methodology_coach",
  "agent_tier": "individual | cohort",
  "timestamp_start": "2025-11-15T14:30:00Z",
  "timestamp_end": "2025-11-15T14:47:00Z",
  "duration_seconds": 1020,
  "message_count": 12,
  "participant_messages": ["text of participant messages"],
  "agent_messages": ["text of agent responses"],
  "session_type": "pre_cohort | between_sessions | post_cohort",
  "week_number": 2,
  "routing_source": "chief_of_staff_recommendation | participant_direct"
}
```

**Why:** Track which agents are used, when, for how long, conversation depth, and routing patterns.

---

#### **2. Venture Progression Data**

**Tracked over time for each participant:**
```json
{
  "participant_id": "uuid",
  "timestamp": "2025-11-15T14:30:00Z",
  "venture_stage": "idea_exploration | problem_validation | customer_discovery | solution_design | mvp_building | market_testing | pivoting | scaling",
  "stage_source": "chief_of_staff_assessment | self_reported | facilitator_observation",
  "milestones_achieved": [
    {
      "milestone_type": "talked_to_customer | built_mvp | first_sale | validated_problem | defined_target_market",
      "timestamp": "2025-11-12T10:00:00Z",
      "description": "Talked to 5 potential customers about problem"
    }
  ],
  "barriers_identified": [
    {
      "barrier_type": "resource | knowledge | motivation | external",
      "description": "Don't know how to reach customers",
      "timestamp": "2025-11-13T16:00:00Z"
    }
  ],
  "actions_planned": [
    {
      "action": "Interview 3 customers by Thursday",
      "planned_timestamp": "2025-11-13T16:00:00Z",
      "due_date": "2025-11-16"
    }
  ],
  "actions_completed": [
    {
      "action": "Interview 3 customers by Thursday",
      "completed_timestamp": "2025-11-15T14:30:00Z",
      "outcome": "Learned customers actually need X, not Y"
    }
  ]
}
```

**Why:** Understand progression trajectory - forward movement, stagnation, or regression.

---

#### **3. Artifact Data**

**For each artifact (management reports, project reviews):**
```json
{
  "artifact_id": "uuid",
  "participant_id": "uuid",
  "artifact_type": "weekly_management_report | project_review",
  "week_number": 2,
  "created_timestamp": "2025-11-15T14:30:00Z",
  "completeness_score": 0.85,
  "sections_completed": ["actions_taken", "progress", "barriers"],
  "sections_skipped": ["next_steps"],
  "action_count": 3,
  "external_validation_detected": true,
  "sentiment": "confident | frustrated | confused | neutral",
  "agent_references": ["market_discovery", "chief_of_staff"],
  "full_text": "text content of artifact"
}
```

**Why:** Artifacts proxy for real-world action. Track quality, consistency, and content.

---

#### **4. Engagement Patterns**

**Per participant, aggregated:**
```json
{
  "participant_id": "uuid",
  "cohort_id": "cohort_1",
  "login_days_per_week": [3, 5, 4, 6],
  "total_logins": 18,
  "sessions_attended": [1, 2, 3, 5, 6, 7, 8],
  "sessions_missed": [4],
  "agent_usage_frequency": "daily | multiple_per_week | weekly | rare",
  "agents_used": {
    "chief_of_staff": 15,
    "market_discovery": 8,
    "methodology_coach": 5
  },
  "agent_diversity_score": 3,
  "peer_community_messages": 12,
  "peer_responses_given": 7
}
```

**Why:** Understand engagement styles. Some may succeed with low attendance but high agent usage.

---

#### **5. Agent Routing & Recommendations**

**Track Chief of Staff decisions:**
```json
{
  "routing_event_id": "uuid",
  "participant_id": "uuid",
  "timestamp": "2025-11-15T14:30:00Z",
  "stage_assessment": "customer_discovery",
  "confidence_score": 0.85,
  "specialist_recommended": "market_discovery",
  "reasoning": "Participant needs to validate customer need",
  "participant_followed_routing": true,
  "routing_effectiveness": "returned_to_specialist | one_time_only | unknown"
}
```

**Why:** Validate Chief of Staff assesses stages correctly and routes effectively.

---

#### **6. Session Attendance & Feedback**

**For each group session:**
```json
{
  "session_id": "uuid",
  "session_number": 3,
  "week_number": 2,
  "date": "2025-11-14",
  "attendees": ["participant_1", "participant_2", ...],
  "absent": ["participant_5"],
  "cohort_agent_used": true,
  "post_session_feedback": [
    {
      "participant_id": "participant_1",
      "helpfulness_rating": 4,
      "peer_learning_rating": 5,
      "methodology_clarity_rating": 4,
      "will_take_action": true,
      "comments": "Great discussion on customer validation"
    }
  ]
}
```

**Why:** Correlate session attendance with outcomes, understand session effectiveness.

---

### **Data Storage Architecture**

**Recommended Approach: Hybrid (Event Log + Structured Tables)**

#### **Event Log (Raw Capture):**
```sql
CREATE TABLE events (
  event_id UUID PRIMARY KEY,
  participant_id UUID,
  cohort_id UUID,
  event_type VARCHAR(50), -- 'agent_conversation', 'artifact_created', 'session_attended', etc.
  timestamp TIMESTAMP,
  metadata JSONB -- flexible schema for event-specific data
);
```

**Why:** Don't lose any data, flexible for future analytics needs.

---

#### **Structured Tables (Queryable):**

**Conversations:**
```sql
CREATE TABLE conversations (
  conversation_id UUID PRIMARY KEY,
  participant_id UUID,
  agent_id VARCHAR(50),
  agent_tier VARCHAR(20),
  start_time TIMESTAMP,
  end_time TIMESTAMP,
  duration_seconds INT,
  message_count INT,
  week_number INT,
  routing_source VARCHAR(50)
);
```

**Milestones:**
```sql
CREATE TABLE milestones (
  milestone_id UUID PRIMARY KEY,
  participant_id UUID,
  milestone_type VARCHAR(50),
  timestamp TIMESTAMP,
  description TEXT,
  week_number INT
);
```

**Artifacts:**
```sql
CREATE TABLE artifacts (
  artifact_id UUID PRIMARY KEY,
  participant_id UUID,
  artifact_type VARCHAR(50),
  created_timestamp TIMESTAMP,
  completeness_score FLOAT,
  action_count INT,
  external_validation_detected BOOLEAN,
  sentiment VARCHAR(20),
  full_text TEXT
);
```

**Progression Snapshots:**
```sql
CREATE TABLE progression_snapshots (
  snapshot_id UUID PRIMARY KEY,
  participant_id UUID,
  timestamp TIMESTAMP,
  venture_stage VARCHAR(50),
  progression_score FLOAT -- calculated metric
);
```

---

### **Export & Analysis (Cohort 1 Approach)**

**For Cohort 1, use simple export workflow:**

1. **Export to CSV:** Database → CSV files (conversations, milestones, artifacts, etc.)
2. **Import to Analysis Tools:** Google Sheets, Excel, Python/Pandas
3. **Visualize:** Charts, dashboards (Google Data Studio, Tableau, or spreadsheet)
4. **AI-Assisted Analysis:** Export data → Claude/GPT → "What patterns do you see?"

**Future (Cohort 3+):**
- Build custom dashboard (real-time participant progression visualizations)
- Automated reporting (weekly email with cohort health metrics)
- Predictive models (identify at-risk participants early)

---

## Key Metrics & Formulas

### **Primary Metric: Venture Progression Score**

**Purpose:** Objective measure of participant progress from start to end of cohort.

**Formula (0-20 points):**

1. **Stage Advancement (0-5 points):**
   - Moved forward 2+ stages: 5 points
   - Moved forward 1 stage: 3 points
   - Stayed in same stage but made progress: 2 points
   - No movement: 0 points

2. **External Validation Actions (0-5 points):**
   - 5+ customer conversations, tests, or MVPs: 5 points
   - 3-4 actions: 4 points
   - 1-2 actions: 2 points
   - 0 actions (only planning): 0 points

3. **Artifact Consistency (0-3 points):**
   - Created 4 weekly reports + 4 project reviews: 3 points
   - Created 3 reports + 3 reviews: 2 points
   - Created 1-2 artifacts: 1 point
   - No artifacts: 0 points

4. **Milestone Achievement (0-5 points):**
   - Major milestone (first sale, MVP launched, pivot decision): 5 points
   - Moderate milestone (validated problem, defined market): 3 points
   - Minor milestone (completed research, drafted plan): 1 point
   - No milestones: 0 points

5. **Agent-Driven Action (0-2 points):**
   - 3+ actions directly attributed to agent conversations: 2 points
   - 1-2 actions: 1 point
   - No agent-driven actions: 0 points

**Total: 0-20 points**

**Progression Categories:**
- **15-20 points:** Significant progress
- **10-14 points:** Moderate progress
- **5-9 points:** Minimal progress
- **0-4 points:** No progress / regression

---

### **Agent Effectiveness Metrics**

**Per agent (Chief of Staff, Market Discovery, Methodology Coach):**

1. **Usage Rate:** `(participants who used agent / total participants) * 100`
2. **Usage Depth:** `average messages per conversation`
3. **Return Rate:** `(participants who used agent 3+ times / participants who used agent) * 100`
4. **Action Correlation:** Correlation between agent usage and actions taken
5. **Outcome Correlation:** Correlation between agent usage and progression score

**Example Output:**
```
Market Discovery Agent:
- Usage Rate: 93% (14/15 participants)
- Usage Depth: 9.2 messages/conversation
- Return Rate: 67% (9/14 returned 3+ times)
- Action Correlation: r = 0.71 (strong positive)
- Outcome Correlation: +3.2 points on progression score
```

---

### **Hypothesis Validation Metrics**

#### **Hypothesis 1: Latent entrepreneurs exist in poor populations**

**Measures:**
- [ ] 70%+ participants developed viable business concepts
- [ ] 70%+ participants demonstrated entrepreneurial capacity (action, problem-solving, persistence)
- [ ] Qualitative: Barriers were external (resources) not internal (capability)

**Data Sources:**
- Progression scores (did they move forward?)
- Milestone achievements (did they accomplish something?)
- Artifact analysis (evidence of entrepreneurial thinking?)
- Pre/post surveys on income, employment status

---

#### **Hypothesis 2: Methodology is uniquely suited for underserved populations**

**Measures:**
- [ ] 75%+ participants report methodology is understandable
- [ ] 75%+ participants report methodology is relevant to their context
- [ ] 75%+ participants can apply methodology principles

**Data Sources:**
- Weekly pulse surveys ("Was this week's methodology understandable? Relevant?")
- Post-cohort survey (1-5 scale: "How well did methodology fit your needs?")
- Methodology fidelity analysis (did agents teach it correctly?)
- Comparison to other programs (if data available)

---

#### **Hypothesis 3: AI can transform entrepreneurship practice for this demographic**

**Measures:**
- [ ] 75%+ participants report agents were valuable (not just interesting)
- [ ] Evidence of agent-driven action (artifacts reference agent conversations)
- [ ] Participants used agents independently (not just when prompted)
- [ ] Agent usage correlates with better outcomes

**Data Sources:**
- Agent usage data (conversations, frequency, depth)
- Progression score correlation with agent usage
- Artifact analysis (do reports reference agent insights?)
- Post-cohort survey ("How valuable were agents? Would you continue using?")

---

### **Completion Metrics (Three Definitions)**

**1. Attendance Completion:**
- `(sessions attended / 8 total sessions) * 100`
- Target: 60%+ attend ≥6 of 8 sessions (minimum bar)

**2. Engagement Completion:**
- Attended sessions + created artifacts + took actions
- Formula: `attendance ≥6 AND artifacts ≥4 AND actions ≥2`
- Target: 50%+ (realistic bar for hard work?)

**3. Outcome Completion:**
- Attended + meaningful progress
- Formula: `attendance ≥6 AND progression_score ≥10`
- Target: 25%+ (aspirational? or too low?)

**⚠️ Uncertainty Flag:** These targets are provisional. We'll refine after Cohort 1.

---

## Analysis Approaches

### **Post-Cohort 1 Analysis Workflow**

**Week 1 Post-Cohort:**
1. **Export all data** to CSV (conversations, artifacts, milestones, attendance)
2. **Calculate progression scores** for each participant
3. **Calculate agent effectiveness metrics** for each agent
4. **Segment participants** by starting stage, outcome, engagement pattern

**Week 2 Post-Cohort:**
5. **Correlation analysis:**
   - Agent usage vs. progression score
   - Attendance vs. outcomes
   - Artifact quality vs. outcomes
   - Starting stage vs. progression
6. **Qualitative synthesis:**
   - Review artifacts for themes
   - Analyze post-cohort survey responses
   - Facilitator observations
7. **Hypothesis testing:**
   - Did we validate H1, H2, H3?
   - What evidence supports or refutes each?

**Week 3 Post-Cohort:**
8. **Synthesis report:** "What We Learned from Cohort 1"
9. **Recommendations:** What to change for Cohort 2
10. **Share with team:** Methodology & Cohort Facilitator, AI Engineer, partners

---

### **Key Questions to Answer**

**About Participants:**
- Which participants made most progress? What characterized them?
- Which participants struggled? What barriers did they face?
- Did starting stage affect outcomes?
- Did engagement patterns predict success?

**About Agents:**
- Which agents were most used? Most valuable?
- Did Chief of Staff correctly assess stages?
- Did specialists drive action or just provide information?
- What failure modes did we observe?

**About Methodology:**
- Did methodology translate well with agent support?
- Was methodology accessible to underserved populations?
- What parts were most/least effective?
- Did we maintain fidelity or drift?

**About Cohort Format:**
- Did 2x/week cadence work?
- Did artifacts unify participants at different stages?
- Did peer learning happen?
- Did facilitators find cohort agents helpful (if implemented)?

---

## Dashboard Mockup (Post-Cohort 1)

```
VIBE ENTREPRENEURSHIP - COHORT 1 RESULTS
========================================

PARTICIPANT PROGRESSION:
┌─────────────────────────────────────┐
│ Significant progress:  4 (27%)  ████│
│ Moderate progress:     7 (47%)  ███████│
│ Minimal progress:      3 (20%)  ███│
│ No progress:           1 (7%)   █│
└─────────────────────────────────────┘

AGENT EFFECTIVENESS:
┌───────────────────────────────────────────────────┐
│ Agent               Usage  Depth  Return  Outcome │
│ Chief of Staff      100%   12.4   85%     +2.8   │
│ Market Discovery    93%    9.2    67%     +3.2   │
│ Methodology Coach   73%    6.1    40%     +1.5   │
└───────────────────────────────────────────────────┘

ACTION METRICS:
- 87% took real-world action (13/15)
- Average 3.4 actions per participant
- 60% of actions referenced agent conversations

HYPOTHESIS VALIDATION:
✅ H1: Latent entrepreneurship (80% showed viable ideas + capacity)
✅ H2: Methodology fit (75% found it understandable and relevant)
✅ H3: AI effectiveness (agent usage correlated with +2.8 progression points)

COMPLETION RATES:
- Attendance: 73% (11/15 attended ≥6 sessions)
- Engagement: 53% (8/15 attended + artifacts + actions)
- Outcome: 33% (5/15 attended + progression ≥10)

TOP INSIGHTS:
1. Market Discovery agent drove most action (+3.2 points)
2. Participants who used agents 10+ times had 2x better outcomes
3. Starting stage didn't matter - all stages showed progress
4. Artifact creation strongly predicted success (r = 0.82)
5. Methodology Coach underutilized - need better onboarding
```

---

## Technical Implementation Notes (For Engineer)

### **MVP Data Collection Requirements:**

**Must-Have (Cohort 1):**
- [ ] Log all agent conversations (participant messages, agent responses, timestamps)
- [ ] Log artifact creation (weekly reports, project reviews, completeness)
- [ ] Log session attendance (who attended which sessions)
- [ ] Track agent routing (Chief of Staff recommendations)
- [ ] Store participant milestones (achievements logged by participant or facilitator)
- [ ] Export functionality (CSV export of all data)

**Should-Have (Cohort 1):**
- [ ] Calculate progression score programmatically (auto-score based on logged data)
- [ ] Basic analytics queries (e.g., "show all participants with progression ≥15")
- [ ] NLP analysis of artifacts (action count, external validation detected, sentiment)

**Nice-to-Have (Defer to Cohort 2):**
- [ ] Real-time dashboard (track participant progress during cohort)
- [ ] Automated alerts (flag participants who haven't logged in for 5 days)
- [ ] Advanced NLP (extract themes, patterns from conversations)

---

### **Data Privacy & Consent:**

**Critical:**
- [ ] Participants consent to data collection and analysis
- [ ] Anonymize data for analysis (use participant IDs, not names)
- [ ] Secure storage (encrypt sensitive data)
- [ ] Retention policy (how long do we keep conversation logs?)

**IRB Considerations:**
- If this is research, may need Institutional Review Board approval
- Consult with legal/compliance on data privacy (especially underserved populations)

---

## Evolution Roadmap

### **Cohort 1: Manual Analysis**
- Export CSV, analyze in spreadsheets/Python
- Basic visualizations
- AI-assisted pattern detection (feed data to LLM)

### **Cohort 2-3: Semi-Automated**
- Automated progression score calculation
- Basic dashboard (participant list with scores)
- Cohort comparison (Cohort 1 vs. Cohort 2 outcomes)

### **Cohort 4+: Full Analytics Platform**
- Real-time dashboard for facilitators
- Predictive models (who's at risk? who's thriving?)
- Automated insights (weekly report emails)
- Cross-cohort analysis (what patterns emerge across all cohorts?)

---

## Success = Learning

**Remember:** The goal isn't perfect data. The goal is **learning what works**.

**If Cohort 1 data shows:**
- Participants made progress → agents are effective!
- Participants struggled → we know what to improve!
- Data is incomplete → we know what to capture better in Cohort 2!

**All outcomes are valuable if we learn from them.**

---

**Last Updated:** 2025-11-04
**Next Review:** After Cohort 1 completion (add actual data, refine metrics based on learnings)

---

**Questions for Engineer?**
- What data is feasible to capture in MVP timeline?
- What schema makes most sense for your tech stack?
- What export format do you recommend?
