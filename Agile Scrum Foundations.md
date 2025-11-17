# Agile Scrum Foundations Cheat Sheet

## Table of Contents
- [What is Agile](#what-is-agile)
- [Agile Manifesto](#agile-manifesto)
- [What is Scrum](#what-is-scrum)
- [Scrum Framework](#scrum-framework)
- [Scrum Roles](#scrum-roles)
- [Scrum Events](#scrum-events)
- [Scrum Artifacts](#scrum-artifacts)
- [User Stories](#user-stories)
- [Estimation Techniques](#estimation-techniques)
- [Scrum Metrics](#scrum-metrics)
- [Best Practices](#best-practices)
- [Common Anti-Patterns](#common-anti-patterns)

---

## What is Agile

Agile is an iterative approach to project management and software development that helps teams deliver value to customers faster and with fewer headaches.

### Key Characteristics
- **Iterative Development**: Work in short cycles (sprints)
- **Customer Collaboration**: Continuous feedback and involvement
- **Adaptive Planning**: Embrace change even late in development
- **Continuous Improvement**: Regular retrospectives and adjustments
- **Cross-functional Teams**: Self-organizing and collaborative
- **Working Software**: Focus on delivering functional increments

### Benefits
✅ Faster time to market  
✅ Higher quality products  
✅ Better risk management  
✅ Increased customer satisfaction  
✅ Improved team morale  
✅ Greater flexibility and adaptability  
✅ Enhanced transparency and visibility  

---

## Agile Manifesto

### Four Core Values

1. **Individuals and interactions** over processes and tools
2. **Working software** over comprehensive documentation
3. **Customer collaboration** over contract negotiation
4. **Responding to change** over following a plan

> *While there is value in the items on the right, we value the items on the left more.*

### Twelve Principles

1. **Customer Satisfaction**: Deliver valuable software early and continuously
2. **Welcome Change**: Embrace changing requirements, even late in development
3. **Frequent Delivery**: Deliver working software frequently (weeks rather than months)
4. **Collaboration**: Business people and developers work together daily
5. **Motivated Individuals**: Build projects around motivated individuals
6. **Face-to-Face**: The most efficient communication method
7. **Working Software**: Primary measure of progress
8. **Sustainable Development**: Maintain a constant pace indefinitely
9. **Technical Excellence**: Continuous attention to good design
10. **Simplicity**: Maximize the amount of work not done
11. **Self-Organizing Teams**: Best architectures emerge from self-organizing teams
12. **Reflect and Adjust**: Regular reflection on how to become more effective

---

## What is Scrum

Scrum is a lightweight framework that helps people, teams and organizations generate value through adaptive solutions for complex problems.

### Scrum Definition
> "A framework within which people can address complex adaptive problems, while productively and creatively delivering products of the highest possible value."

### Scrum Pillars (Transparency, Inspection, Adaptation)

#### 1. Transparency
- All work is visible to those responsible for the outcome
- Common language and definition of "Done"
- Artifacts are openly shared

#### 2. Inspection
- Regular inspection of artifacts and progress
- Detect undesirable variances
- Inspect frequently but not so much it gets in the way

#### 3. Adaptation
- Adjust process if inspection reveals issues
- Make adjustments as soon as possible
- Empower teams to adapt

### Scrum Values

1. **Commitment**: Committed to achieving team goals
2. **Courage**: Do the right thing and work on tough problems
3. **Focus**: Focus on the sprint work and team goals
4. **Openness**: Open about work and challenges
5. **Respect**: Respect each other as capable, independent people

---

## Scrum Framework

```
Product Backlog → Sprint Planning → Sprint Backlog → Daily Scrum (15 min)
                                          ↓
                                    Sprint (1-4 weeks)
                                          ↓
                                    Increment
                                          ↓
                            Sprint Review → Sprint Retrospective
                                          ↓
                                    (Repeat)
```

### Sprint Cycle (Typically 2 weeks)

```
Week 1:
├── Monday: Sprint Planning (4 hours for 2-week sprint)
├── Daily: Daily Scrum (15 min)
├── Development Work
└── Friday: Mid-sprint check-in (optional)

Week 2:
├── Monday-Thursday: Development Work
├── Daily: Daily Scrum (15 min)
├── Friday: Sprint Review (2 hours)
└── Friday: Sprint Retrospective (1.5 hours)
```

---

## Scrum Roles

### 1. Product Owner (PO)

**Responsibilities:**
- Maximize the value of the product
- Manage the Product Backlog
- Order Product Backlog items
- Ensure backlog is transparent and understood
- Define acceptance criteria
- Accept or reject work results

**Key Activities:**
- Define product vision and strategy
- Gather requirements from stakeholders
- Prioritize features based on business value
- Make trade-off decisions
- Participate in sprint events
- Provide clarification to the team

**Skills:**
- Domain knowledge
- Stakeholder management
- Decision-making
- Communication
- Availability to the team

**Anti-Patterns to Avoid:**
❌ Micromanaging the Development Team  
❌ Not being available to answer questions  
❌ Changing priorities mid-sprint  
❌ Bypassing the Scrum Master  

---

### 2. Scrum Master (SM)

**Responsibilities:**
- Serve the Scrum Team and organization
- Help everyone understand Scrum theory and practice
- Remove impediments
- Facilitate Scrum events
- Coach team in self-organization
- Shield team from external interruptions

**Serving the Product Owner:**
- Help find techniques for effective Product Backlog management
- Help the Scrum Team understand clear Product Backlog items
- Facilitate stakeholder collaboration

**Serving the Development Team:**
- Coach in self-organization and cross-functionality
- Remove impediments to progress
- Facilitate Scrum events as needed
- Coach in organizational environments where Scrum isn't fully adopted

**Serving the Organization:**
- Lead and coach Scrum adoption
- Plan Scrum implementations
- Help employees and stakeholders understand empirical product development
- Work with other Scrum Masters to increase effectiveness

**Skills:**
- Facilitation
- Coaching
- Conflict resolution
- Servant leadership
- Process improvement

**Anti-Patterns to Avoid:**
❌ Acting as team manager  
❌ Making decisions for the team  
❌ Being a secretary or note-taker only  
❌ Being absent from team events  

---

### 3. Development Team (Developers)

**Responsibilities:**
- Deliver potentially releasable increment each sprint
- Self-organize to accomplish work
- Create the Sprint Backlog
- Adapt plans each day toward the Sprint Goal
- Hold each other accountable
- Quality belongs to everyone

**Characteristics:**
- **Self-organizing**: No one tells them how to turn backlog into increments
- **Cross-functional**: All skills needed to create increment
- **No titles**: Everyone is a Developer
- **No sub-teams**: Regardless of domains
- **Accountability**: Whole team accountable for success

**Optimal Team Size:**
- **Minimum**: 3 people (below this lacks interaction)
- **Maximum**: 9 people (above this requires too much coordination)
- **Ideal**: 5-7 people
- *Note: PO and SM not included in count unless doing dev work*

**Skills:**
- Technical expertise
- Collaboration
- Communication
- Problem-solving
- Continuous learning

**Anti-Patterns to Avoid:**
❌ Waiting for assignments  
❌ Working in silos  
❌ Not helping teammates  
❌ Gold-plating features  

---

## Scrum Events

All events are time-boxed and occur within the Sprint container.

### 1. Sprint

**Duration**: 1-4 weeks (typically 2 weeks)  
**Purpose**: Container for all other events, consistent duration

**Characteristics:**
- Fixed duration
- New Sprint starts immediately after previous ends
- No changes that endanger Sprint Goal
- Quality goals do not decrease
- Scope may be clarified and renegotiated with PO

**When to Cancel:**
- Only Product Owner can cancel
- Rare occurrence
- If Sprint Goal becomes obsolete
- Completed items are reviewed
- Incomplete items re-estimated and put back on backlog

**Sprint Goal:**
- Objective set for the Sprint
- Provides guidance to Development Team on why building increment
- Created during Sprint Planning
- Provides flexibility regarding functionality implemented

---

### 2. Sprint Planning

**Duration**: 8 hours for 1-month sprint (proportionally less for shorter sprints)  
**Participants**: Entire Scrum Team  
**Purpose**: Plan the work for the Sprint

**Two Main Questions:**

#### What can be delivered in the Increment?
- Development Team forecasts functionality
- Product Owner discusses objective and backlog items
- Entire team collaborates on understanding work
- Input: Product Backlog, latest Increment, projected capacity, past performance

#### How will the chosen work get done?
- Development Team decides how to build
- Product Backlog items + plan = Sprint Backlog
- Work is decomposed to units of one day or less
- Development Team self-organizes

**Outputs:**
- Sprint Goal
- Sprint Backlog
- Clear understanding of work

**Definition of Ready (DoR):**
Items should be:
- Clear
- Feasible
- Testable
- Estimated
- Valuable
- Small enough to complete in a sprint

---

### 3. Daily Scrum (Stand-up)

**Duration**: 15 minutes (time-boxed)  
**Time**: Same time and place every day  
**Participants**: Development Team (required), SM facilitates, PO may attend  
**Purpose**: Inspect progress toward Sprint Goal and adapt Sprint Backlog

**Three Questions (Classic Format):**
1. What did I do yesterday that helped meet the Sprint Goal?
2. What will I do today to help meet the Sprint Goal?
3. Do I see any impediment that prevents me or the team from meeting the Sprint Goal?

**Alternative Formats:**
- Walk the board (discuss each item on board)
- Focus on Sprint Goal progress
- Discuss blockers and plan for the day

**Best Practices:**
✅ Stand up (keeps it short)  
✅ Start on time  
✅ Stay focused on the three questions  
✅ Take detailed discussions offline  
✅ Update the task board  
✅ Make impediments visible  

**Anti-Patterns:**
❌ Status reporting to Scrum Master  
❌ Problem-solving sessions  
❌ Going over 15 minutes  
❌ People showing up late  
❌ Working on laptops during meeting  

---

### 4. Sprint Review

**Duration**: 4 hours for 1-month sprint (proportionally less for shorter sprints)  
**Participants**: Scrum Team + Key Stakeholders  
**Purpose**: Inspect the Increment and adapt Product Backlog

**Agenda:**
1. **Product Owner explains** what has been "Done" and not "Done"
2. **Development Team discusses** what went well, problems encountered, and how solved
3. **Development Team demonstrates** the work and answers questions
4. **Product Owner discusses** Product Backlog as it stands
5. **Entire group collaborates** on what to do next (input to Sprint Planning)
6. **Review** of timeline, budget, potential capabilities for next release

**Outputs:**
- Revised Product Backlog
- Feedback from stakeholders
- Potential Product Backlog items for next Sprint

**Best Practices:**
✅ Demo in production-like environment  
✅ Prepare demo in advance  
✅ Show working software, not slides  
✅ Encourage stakeholder participation  
✅ Collect feedback  
✅ Celebrate achievements  

**Key Questions:**
- Is the Sprint Goal achieved?
- What feedback do stakeholders have?
- What should we do next?
- How is the market or potential use changing?

---

### 5. Sprint Retrospective

**Duration**: 3 hours for 1-month sprint (proportionally less for shorter sprints)  
**Participants**: Scrum Team (Dev Team, SM, PO)  
**Purpose**: Inspect how the last Sprint went and plan improvements

**Three Main Questions:**
1. What went well in the Sprint?
2. What could be improved?
3. What will we commit to improve in the next Sprint?

**Common Formats:**

#### Start-Stop-Continue
- **Start**: What should we start doing?
- **Stop**: What should we stop doing?
- **Continue**: What should we keep doing?

#### Sailboat Retrospective
- **Wind** (helping us): What helped us move forward?
- **Anchor** (holding us back): What slowed us down?
- **Rocks** (risks): What risks do we face?
- **Island** (goal): Where are we heading?

#### 4Ls Retrospective
- **Liked**: What did we like?
- **Learned**: What did we learn?
- **Lacked**: What was missing?
- **Longed For**: What did we wish for?

#### Mad-Sad-Glad
- **Mad**: What made us angry/frustrated?
- **Sad**: What disappointed us?
- **Glad**: What made us happy?

**Outputs:**
- Identified improvements
- Action items with owners
- Plan for implementing improvements

**Best Practices:**
✅ Create a safe environment  
✅ Focus on improvement, not blame  
✅ Make it actionable  
✅ Limit improvement items (1-3 max)  
✅ Track action items  
✅ Rotate facilitation  
✅ Try different formats  

**Anti-Patterns:**
❌ Skipping retrospective  
❌ Same issues every time with no action  
❌ Blame game  
❌ Too many action items  
❌ No follow-up on previous actions  

---

## Scrum Artifacts

### 1. Product Backlog

**Definition**: Ordered list of everything needed in the product

**Characteristics:**
- Single source of requirements
- Continuously refined
- Owned by Product Owner
- Never complete (evolves as product and environment evolve)
- Ordered by value, risk, priority, and necessity

**Product Backlog Items (PBIs):**
- Features
- Functions
- Requirements
- Enhancements
- Fixes
- User Stories
- Epics
- Technical debt

**DEEP Backlog:**
- **D**etailed appropriately (more detail for higher priority items)
- **E**stimated (at least high priority items)
- **E**mergent (constantly evolving)
- **P**rioritized (ordered by value)

**Refinement (Grooming):**
- Ongoing activity
- Add details, estimates, and order
- Usually consumes no more than 10% of team capacity
- Development Team provides estimates
- Product Owner remains accountable

**Example Structure:**
```
Priority | ID | User Story | Story Points | Status
---------|----|-----------|--------------|---------
1        | 101| As a user, I want... | 5 | Ready
2        | 102| As a customer... | 8 | In Progress
3        | 103| As an admin... | 13 | Backlog
```

---

### 2. Sprint Backlog

**Definition**: Set of Product Backlog items selected for the Sprint, plus plan for delivering them

**Components:**
- Selected Product Backlog items
- Sprint Goal
- Plan for delivering them (tasks)
- Real-time picture of work

**Characteristics:**
- Owned by Development Team
- Can be modified during Sprint
- Visible to everyone
- Updated daily
- Highly visible, real-time picture of work

**Task Breakdown:**
- PBIs broken into tasks
- Tasks estimated in hours (optional)
- Tasks no larger than 1 day
- Burndown chart shows remaining work

**Example:**
```
User Story: As a user, I can login
├── Design login UI (4h)
├── Implement authentication (6h)
├── Add password validation (3h)
├── Write unit tests (4h)
└── Update documentation (2h)
Total: 19 hours
```

---

### 3. Increment

**Definition**: Sum of all Product Backlog items completed during a Sprint plus value of increments of all previous Sprints

**Characteristics:**
- Must be in useable condition
- Must meet Definition of Done
- Must be potentially releasable
- PO decides whether to release
- Builds on previous increments

**Definition of Done (DoD):**

A shared understanding of what it means for work to be complete.

**Example DoD:**
```
✅ Code complete
✅ Code reviewed
✅ Unit tests written and passing
✅ Integration tests passing
✅ Documentation updated
✅ Acceptance criteria met
✅ Deployed to staging
✅ Product Owner approved
✅ No known defects
```

**Levels of Done:**
- **Feature Level**: Individual story complete
- **Sprint Level**: All sprint stories complete
- **Release Level**: Ready for production

---

## User Stories

### INVEST Criteria

Good user stories should be:

- **I**ndependent: Can be developed in any order
- **N**egotiable: Details can be negotiated
- **V**aluable: Provides value to users/customers
- **E**stimable: Can be estimated by team
- **S**mall: Can be completed in one sprint
- **T**estable: Clear acceptance criteria

### User Story Format

```
As a [type of user]
I want [some goal/functionality]
So that [some reason/benefit]
```

**Examples:**

```
As a registered user
I want to reset my password
So that I can regain access if I forget it
```

```
As a customer
I want to track my order status
So that I know when to expect delivery
```

```
As an administrator
I want to generate sales reports
So that I can analyze business performance
```

### Acceptance Criteria

Define conditions that must be met for story to be accepted.

**Format 1: Given-When-Then (Gherkin)**
```
Given [context/precondition]
When [action/event]
Then [outcome/result]
```

**Example:**
```
Story: User Login

Acceptance Criteria:
1. Given I am on the login page
   When I enter valid credentials
   Then I should be logged in successfully

2. Given I am on the login page
   When I enter invalid credentials
   Then I should see an error message

3. Given I entered wrong password 3 times
   When I attempt to login again
   Then my account should be temporarily locked
```

**Format 2: Checklist**
```
Story: Password Reset

Acceptance Criteria:
☐ User can request password reset via email
☐ Reset link expires after 24 hours
☐ User must enter password twice for confirmation
☐ Password must meet security requirements
☐ User receives confirmation email after reset
☐ Old password no longer works after reset
```

### Story Splitting Techniques

#### 1. By Workflow Steps
```
Original: As a user, I want to purchase items
Split:
- Add items to cart
- Update cart quantities
- Apply discount codes
- Complete checkout
- Receive order confirmation
```

#### 2. By Business Rules
```
Original: Calculate shipping cost
Split:
- Calculate for domestic orders
- Calculate for international orders
- Apply free shipping threshold
```

#### 3. By Happy/Unhappy Path
```
Original: User login
Split:
- Happy path: successful login
- Unhappy path: invalid credentials
- Unhappy path: account locked
```

#### 4. By Data Types
```
Original: Import files
Split:
- Import CSV files
- Import Excel files
- Import JSON files
```

---

## Estimation Techniques

### 1. Story Points

**Characteristics:**
- Relative sizing (not time-based)
- Measures complexity, effort, and uncertainty
- Fibonacci sequence common: 1, 2, 3, 5, 8, 13, 21
- Team-specific (not comparable across teams)

**Fibonacci Scale:**
```
1 point:  Trivial, very well understood
2 points: Simple, mostly understood
3 points: Moderate complexity
5 points: Complex, some unknowns
8 points: Very complex, many unknowns
13 points: Extremely complex, lots of unknowns
21+ points: Too large, needs splitting
```

**Reference Stories:**
- Choose 2-3 stories as baselines
- Compare new stories to these references
- Use relative sizing

---

### 2. Planning Poker

**Process:**
1. Product Owner presents story
2. Team discusses and asks questions
3. Each team member selects a card (secretly)
4. All reveal cards simultaneously
5. Discuss differences (especially outliers)
6. Re-estimate until consensus

**Card Values:** 0, ½, 1, 2, 3, 5, 8, 13, 20, 40, 100, ?, ☕

**Special Cards:**
- **?** (Question mark): Need more information
- **☕** (Coffee): Need a break
- **∞** (Infinity): Story too large, needs splitting

**Benefits:**
✅ Engages entire team  
✅ Surfaces different perspectives  
✅ Builds shared understanding  
✅ Quick consensus  

---

### 3. T-Shirt Sizing

**Sizes:** XS, S, M, L, XL, XXL

**Good for:**
- High-level estimation
- Initial backlog estimation
- Non-technical stakeholders

**Example:**
```
XS (1-2 points): Change button color
S (3-5 points): Add new form field
M (5-8 points): Implement search feature
L (8-13 points): Build new dashboard
XL (13-21 points): Integrate payment gateway
XXL (21+ points): Complete redesign (needs splitting)
```

**Conversion to Story Points:**
```
XS → 1-2 points
S  → 3-5 points
M  → 5-8 points
L  → 8-13 points
XL → 13-21 points
```

---

### 4. Affinity Estimation

**Process:**
1. Write stories on cards/sticky notes
2. Place first story in middle
3. Place next story relative to first (larger/smaller/same)
4. Continue until all placed
5. Group into buckets
6. Assign point values to buckets

**Benefits:**
- Fast for large backlogs
- Visual and collaborative
- Good for workshops

---

### 5. Velocity

**Definition**: Amount of work completed in a sprint, measured in story points

**Calculation:**
```
Sprint Velocity = Sum of story points for completed stories

Example:
Sprint 1: 23 points
Sprint 2: 28 points
Sprint 3: 25 points
Average Velocity: (23 + 28 + 25) / 3 = 25.33 points per sprint
```

**Uses:**
- Forecast how much work can be done
- Plan releases
- Track team performance trends
- Set realistic Sprint Goals

**Important Notes:**
- Velocity is team-specific
- Stabilizes after 3-5 sprints
- Not a performance metric for individuals
- Should trend relatively stable over time

**Velocity Calculation:**
```
Yesterday's Weather: Use last sprint's velocity
Running Average: Average of last 3 sprints
Weighted Average: More weight on recent sprints
```

---

## Scrum Metrics

### 1. Burndown Chart

Shows remaining work over time in a sprint.

```
Remaining Work (Story Points)
↑
│    Ideal
│   ╱╲
│  ╱  ╲
│ ╱    ╲___ Actual
│╱         ╲___
└──────────────→ Time (Days)
```

**Ideal Line**: Straight line from start to zero  
**Actual Line**: Real progress

**Interpretations:**
- **Below ideal**: Ahead of schedule
- **Above ideal**: Behind schedule
- **Flat line**: No progress
- **Spike up**: New work added

---

### 2. Burnup Chart

Shows completed work over time.

```
Work (Story Points)
↑
│        ___─── Total Scope
│    ___─╱
│___─   ╱ Completed
│      ╱
│     ╱
└──────────────→ Time (Sprints)
```

**Benefits:**
- Shows scope changes
- Shows completed work
- Better for stakeholder communication

---

### 3. Velocity Chart

Tracks team velocity over sprints.

```
Story Points
↑
│  ██       Average
│  ██   ██  ─ ─ ─
│  ██ █ ██  ██
│█ ██ █ ██  ██ █
└─────────────────→ Sprints
 S1 S2 S3 S4 S5
```

**Uses:**
- Forecast future sprints
- Identify trends
- Plan releases

---

### 4. Cumulative Flow Diagram (CFD)

Shows work in different states over time.

```
Items
↑
│         Done
│        Testing
│       In Progress
│      To Do
└─────────────────→ Time
```

**Insights:**
- Bottlenecks (widening bands)
- Flow efficiency
- Work in progress

---

### 5. Lead Time & Cycle Time

**Lead Time**: Time from request to delivery  
**Cycle Time**: Time from starting work to delivery

```
Request → [Wait] → Start Work → [Development] → Done
├─────────── Lead Time ───────────────────────┤
             ├──────── Cycle Time ─────────┤
```

**Target**: Reduce both over time

---

### 6. Sprint Health Metrics

**Commitment Reliability:**
```
(Completed Story Points / Committed Story Points) × 100%

Example: (23 / 28) × 100% = 82%
Target: > 90%
```

**Scope Change:**
```
(Added + Removed Story Points / Total Sprint Points) × 100%

Target: < 10%
```

**Defect Rate:**
```
Defects Found / Stories Completed

Target: < 0.2 (1 defect per 5 stories)
```

---

## Best Practices

### Sprint Planning
✅ Ensure backlog is refined before planning  
✅ Entire team participates  
✅ Set realistic Sprint Goal  
✅ Don't over-commit  
✅ Break stories into tasks  
✅ Consider team capacity (holidays, meetings, etc.)  
✅ Have Definition of Ready  

### Daily Scrum
✅ Keep it under 15 minutes  
✅ Start on time  
✅ Stay focused on Sprint Goal  
✅ Update task board immediately after  
✅ Take detailed discussions offline  
✅ Make impediments visible  

### Sprint Review
✅ Demonstrate working software  
✅ Invite relevant stakeholders  
✅ Focus on collaboration, not just demo  
✅ Collect actionable feedback  
✅ Update Product Backlog based on feedback  
✅ Make it interactive  

### Sprint Retrospective
✅ Make it safe to speak openly  
✅ Focus on continuous improvement  
✅ Keep action items to 2-3 max  
✅ Assign owners to action items  
✅ Review previous action items  
✅ Vary the format  
✅ Celebrate successes  

### Product Backlog Management
✅ Keep it DEEP (Detailed, Estimated, Emergent, Prioritized)  
✅ Refine regularly (10% of sprint capacity)  
✅ Write clear acceptance criteria  
✅ Split large stories  
✅ Order by value and risk  
✅ Include technical debt  

### Team Collaboration
✅ Sit together when possible  
✅ Use visual boards  
✅ Communicate openly  
✅ Help teammates  
✅ Share knowledge  
✅ Pair program when beneficial  
✅ Code reviews  

### Technical Practices
✅ Continuous integration  
✅ Test-driven development (TDD)  
✅ Automated testing  
✅ Refactoring  
✅ Code reviews  
✅ Version control  
✅ Documentation  

---

## Common Anti-Patterns

### Team Anti-Patterns

❌ **Scrum Master as Team Manager**
- Scrum Master should coach, not manage
- Team should self-organize

❌ **Product Owner Not Available**
- PO must be accessible to team
- Delays decisions and clarifications

❌ **Team Not Cross-Functional**
- Handoffs create delays
- Everyone should be able to contribute

❌ **Working in Silos**
- Individual work instead of collaboration
- Reduces knowledge sharing

❌ **Cherry-Picking Stories**
- Taking only easy stories
- Leaves difficult work for later

---

### Sprint Anti-Patterns

❌ **No Sprint Goal**
- Team lacks focus and direction
- Makes it hard to make trade-offs

❌ **Scope Changes Mid-Sprint**
- Disrupts team focus
- Makes planning meaningless

❌ **Carrying Over Work**
- Incomplete stories regularly moved to next sprint
- Indicates poor estimation or over-commitment

❌ **Sprint Without Review/Retro**
- Missing opportunities for feedback
- No continuous improvement

❌ **Unfinished Work Counted as Done**
- Violates Definition of Done
- Technical debt accumulates

---

### Meeting Anti-Patterns

❌ **Long Daily Scrums**
- Over 15 minutes
- Detailed problem-solving discussions

❌ **Status Report to Manager**
- Daily Scrum becomes reporting session
- Team doesn't collaborate

❌ **No Time-Boxing**
- Meetings run too long
- Waste team's time

❌ **Skipping Meetings**
- Missing regular cadence
- Breaks Scrum framework

---

### Backlog Anti-Patterns

❌ **Too Much Detail Too Early**
- Wasted effort on low-priority items
- Things change before implementation

❌ **No Refinement**
- Team doesn't understand stories
- Poor estimates

❌ **Features Instead of User Stories**
- Lacks user perspective
- Missing "why"

❌ **No Acceptance Criteria**
- Unclear definition of done
- Scope creep

---

### Estimation Anti-Patterns

❌ **Hours Instead of Story Points**
- Estimation becomes commitment
- Less flexibility

❌ **Comparing Velocities Across Teams**
- Story points are team-specific
- Creates unhealthy competition

❌ **No Re-estimation**
- Initial estimates never refined
- Accumulates inaccuracy

❌ **Management Setting Estimates**
- Team not owning estimates
- Less accurate

---

### Cultural Anti-Patterns

❌ **Blame Culture**
- Fear of failure
- Reduced innovation

❌ **No Psychological Safety**
- People afraid to speak up
- Issues hidden

❌ **Lack of Trust**
- Micromanagement
- Reduced autonomy

❌ **Not Celebrating Success**
- Low morale
- Lack of motivation

---

## Scrum at Scale

### Scaling Frameworks

#### SAFe (Scaled Agile Framework)
- Large enterprise framework
- Multiple levels: Team, Program, Portfolio
- Structured approach with defined roles

#### LeSS (Large-Scale Scrum)
- Extends Scrum with minimum additions
- One Product Backlog
- One Definition of Done

#### Scrum@Scale
- Created by Jeff Sutherland
- Modular approach
- Scales through Scrum of Scrums

#### Nexus
- Created by Scrum.org
- 3-9 Scrum teams
- Nexus Integration Team

### Common Scaling Elements

**Scrum of Scrums:**
- Representatives from each team meet
- Coordinate dependencies
- Discuss inter-team impediments

**Communities of Practice:**
- Cross-team groups by specialty
- Share knowledge and standards
- Align on technical practices

**Product Owner Council:**
- Multiple Product Owners coordinate
- Align priorities across teams
- Manage dependencies

---

## Transitioning to Scrum

### Phase 1: Education
- Train entire organization
- Explain Agile values and principles
- Set expectations

### Phase 2: Pilot
- Start with one team
- Learn and adapt
- Document lessons learned

### Phase 3: Expansion
- Roll out to more teams
- Build internal coaching capability
- Share successes

### Phase 4: Optimization
- Continuous improvement
- Address organizational impediments
- Measure and adjust

### Common Challenges

**Resistance to Change:**
- Communicate benefits clearly
- Involve people in the change
- Show quick wins

**Lack of Management Support:**
- Educate leadership
- Show business value
- Start small and prove concept

**Distributed Teams:**
- Invest in collaboration tools
- Overlap working hours
- Occasional co-location

**Legacy Processes:**
- Gradually transition
- Run in parallel initially
- Retire old processes systematically

---

## Tools & Resources

### Physical Tools
- **Scrum Board**: Physical or digital Kanban board
- **Index Cards**: For user stories
- **Planning Poker Cards**: For estimation
- **Wall Charts**: Burndown, velocity

### Digital Tools

**Project Management:**
- Jira
- Azure DevOps
- Trello
- Asana
- Monday.com

**Collaboration:**
- Miro (virtual whiteboard)
- Mural
- Microsoft Teams
- Slack
- Zoom

**Estimation:**
- Planning Poker Online
- Scrum Poker Online
- PlanITpoker

**Documentation:**
- Confluence
- Notion
- SharePoint
- Wiki

---

## Key Terms Glossary

| Term | Definition |
|------|------------|
| **Acceptance Criteria** | Conditions that must be met for a story to be accepted |
| **Backlog Refinement** | Activity of adding detail and estimates to backlog items |
| **Burndown Chart** | Graph showing remaining work over time |
| **Daily Scrum** | 15-minute daily team synchronization meeting |
| **Definition of Done** | Shared understanding of what "complete" means |
| **Epic** | Large user story that needs to be broken down |
| **Impediment** | Obstacle preventing team progress |
| **Increment** | Sum of completed Product Backlog items in a Sprint |
| **Product Backlog** | Ordered list of everything needed in the product |
| **Product Owner** | Role responsible for maximizing product value |
| **Scrum Master** | Role responsible for promoting and supporting Scrum |
| **Sprint** | Time-boxed iteration (1-4 weeks) |
| **Sprint Backlog** | Product Backlog items selected for the Sprint plus plan |
| **Sprint Goal** | Objective for the Sprint |
| **Sprint Planning** | Event to plan the Sprint work |
| **Sprint Retrospective** | Event to inspect and adapt the process |
| **Sprint Review** | Event to inspect the Increment and adapt Product Backlog |
| **Stakeholder** | Person external to Scrum Team with interest in product |
| **Story Points** | Relative measure of effort, complexity, and uncertainty |
| **User Story** | Feature description from user's perspective |
| **Velocity** | Amount of work team completes in a Sprint |

---

## Quick Reference Card

### Sprint Checklist

**Sprint Planning:**
☐ Review and refine top backlog items  
☐ Set Sprint Goal  
☐ Select Product Backlog items  
☐ Create Sprint Backlog  
☐ Break items into tasks  

**During Sprint:**
☐ Daily Scrum every day  
☐ Update task board  
☐ Work toward Sprint Goal  
☐ Refine backlog (10% time)  

**Sprint Review:**
☐ Demo completed work  
☐ Gather stakeholder feedback  
☐ Update Product Backlog  
☐ Discuss next steps  

**Sprint Retrospective:**
☐ Discuss what went well  
☐ Identify improvements  
☐ Create action items  
☐ Assign owners  

---

### Story Writing Template

```
Title: [Brief description]

As a [user type]
I want [goal/functionality]
So that [benefit/value]

Acceptance Criteria:
1. [Specific, testable criteria]
2. [Specific, testable criteria]
3. [Specific, testable criteria]

Notes:
- [Additional context]
- [Technical considerations]
- [Dependencies]

Estimate: [Story points]
Priority: [High/Medium/Low]
```

---

### Retrospective Template

```
Sprint: [Number]
Date: [Date]
Attendees: [Team members]

What went well:
1. 
2. 
3. 

What could be improved:
1. 
2. 
3. 

Action Items:
1. [Action] - Owner: [Name] - Due: [Date]
2. [Action] - Owner: [Name] - Due: [Date]

Previous action items status:
☑ [Completed item]
☐ [In progress item]
```

---

## Additional Resources

### Books
- **"Scrum: The Art of Doing Twice the Work in Half the Time"** by Jeff Sutherland
- **"The Scrum Guide"** by Ken Schwaber & Jeff Sutherland (free)
- **"User Stories Applied"** by Mike Cohn
- **"Agile Estimating and Planning"** by Mike Cohn
- **"The Professional Product Owner"** by Don McGreal & Ralph Jocham

### Certifications
- **Certified ScrumMaster (CSM)** - Scrum Alliance
- **Professional Scrum Master (PSM)** - Scrum.org
- **Certified Scrum Product Owner (CSPO)** - Scrum Alliance
- **Professional Scrum Product Owner (PSPO)** - Scrum.org
- **SAFe Agilist** - Scaled Agile

### Websites
- **Scrum.org** - Official Scrum resources
- **Scrum Alliance** - Certification and community
- **Agile Alliance** - Agile practices and resources
- **Mountain Goat Software** - Mike Cohn's blog
- **Scrum Inc.** - Jeff Sutherland's company

---

**Remember**: Scrum is a framework, not a methodology. Adapt it to your context while respecting its core principles and values. The goal is to deliver value, not to follow rules perfectly.
