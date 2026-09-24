כן, אני מבין בדיוק. אתה רוצה קודם לקבוע את **המערכת הסופית של Agents ו־Skills ברמת Task**, ואז לממש אותה בצורה סדרתית:

Agent אחד


→ ה־Skills שהוא צריך


→ Review ואימות של החבילה


→ Pilot


→ Snapshot סופי


→ מעבר ל־Agent הבא

לא נכתוב קודם את כל ה־Skills ואז את כל ה־Agents. כל שלב יסתיים ב־**Agent עובד יחד עם כל היכולות הדרושות לו**.

# 1. יחידת העבודה

ההגדרה הבסיסית תהיה:

> **Task הוא יחידת עבודה המיועדת להסתיים ב־commit אחד קוהרנטי.**

Task יכול להיות:

SKELETON


INTERFACE


PRODUCTION\_CODE


UNIT\_TEST


INTEGRATION


REFACTORING


BUG\_FIX

ה־Task אינו חייב תמיד להסתיים ב־commit בפועל, אבל הוא צריך להיות:

- קטן מספיק לשיחת Copilot עצמאית.
- ניתן לבנייה ולבדיקה.
- בעל scope מוגדר.
- בעל dependencies מפורשים.
- בעל acceptance criteria.
- ללא החלטות Design פתוחות.
- קוהרנטי מספיק כדי להיות commit בפני עצמו.

# 2. המערכת הסופית שנבנה

אני ממליץ על חמישה Agents:

DP-Task-Planner


DP-Task-Test-Designer


DP-Task-Implementer


DP-Task-Reviewer


DP-Task-Orchestrator

ה־Orchestrator ייכתב אחרון, לאחר שכל העובדים שהוא צריך לתזמר כבר קיימים ועברו Pilot.

## ה־workflow הסופי

Approved Design


\+ Component Implementation Plan


\+ Component/Interface HLTP


                │


                ▼


        DP-Task-Orchestrator


                │


                ▼


          DP-Task-Planner


                │


       Task Contract + Plan


                │


                ▼


      DP-Task-Test-Designer


             optional


                │


        Task-local HLTP


                │


                ▼


        DP-Task-Implementer


                │


        Code + Tests + Evidence


                │


                ▼


         DP-Task-Reviewer


                │


          PASS / FIX\_REQUIRED


                │


        ┌───────┴────────┐


        │                │


        ▼                ▼


 Implementer Fix    Task Completion


        │                │


        └── Reviewer ◄───┘


                         │


                         ▼


                READY\_FOR\_COMMIT

---

# 3. Agent 1: `DP-Task-Planner`

זה יהיה ה־Agent הראשון שנבנה.

## אחריות

לקבל Task מתוך Implementation Plan ולהפוך אותו לחבילת עבודה ישימה, קצרה ומבוססת repository.

הוא לא כותב קוד.

## Skills

task-intake


task-repository-analysis


task-planning

בנוסף, ה־Planner רשאי להשתמש ב־`dp-component-skill-router` באופן סלקטיבי לצורך זיהוי וטעינת **domain constraints** בלבד. הוא אינו טוען את כל חומר ה־Domain Skill ואינו מבצע באמצעותו implementation.

## `task-intake`

אחראי על:

- זיהוי מטרת ה־Task.
- בדיקת גבולות ה־Task.
- זיהוי dependencies.
- חילוץ החלטות Design רלוונטיות.
- חילוץ non-goals.
- חילוץ acceptance criteria.
- זיהוי ambiguity או design gaps.
- קביעת readiness.

### Output

Task Contract

## `task-repository-analysis`

אחראי על exploration ממוקד:

- איתור declarations קונקרטיים.
- איתור owners ו־consumers.
- איתור initialization ו־runtime paths.
- איתור tests קיימים.
- איתור מימושים דומים.
- אימות APIs.
- זיהוי domain concerns ו־integration prerequisites.
- הפעלת `dp-component-skill-router` רק עבור Domain Skills הרלוונטיים להבנת constraints.
- הפקת evidence references.

הוא אינו מבצע exploration של כל ה־repository.

### Output

Task Repository Context

## `task-planning`

אחראי על:

- מיפוי הדרישות לשינויים.
- הגדרת סדר פעולות.
- הגדרת Required change areas.
- הגדרת Expected affected files or symbols כתחזית repository-grounded ולא כרשימת diff קשיחה.
- הגדרת Protected or excluded areas.
- הגדרת verification לכל שלב.
- זיהוי risks.
- קביעת scope boundary ל־diff.

### Output

Task Execution Plan

## תוצר Agent מלא

Task Work Package


├── Task Contract


├── Repository Context


├── Execution Plan

├── Relevant Domain Constraints / Skill References

└── Readiness Status

## סטטוסים

READY


BLOCKED


DESIGN\_CLARIFICATION\_REQUIRED


TASK\_SPLIT\_REQUIRED


DEPENDENCY\_NOT\_READY

---

# 4. Agent 2: `DP-Task-Test-Designer`

Agent נפרד עבור HLTP יהיה מוצדק אצלך, משום ש־HLTP הוא חלק מרכזי בתהליך וקיימת חשיבות לכך שהבדיקות ייגזרו מהחוזה ולא מהמימוש.

ה־Agent אינו מופעל על כל Task. הוא מופעל כאשר:

- ה־Task דורש tests חדשים.
- יש צורך לחלץ subset מתוך Component HLTP.
- behavior משתנה.
- acceptance criteria דורשים verification באמצעות unit tests.
- קיימת אי־בהירות לגבי כיסוי הבדיקות.

## Skill

task-unit-test-hltp

בשלב הראשון מספיק Skill מרכזי אחד. אין צורך לפצל מוקדם מדי.

## Mode 1: Slice HLTP קיים

Input:

Component HLTP


Interface HLTP


Task Contract


Execution Plan

Output:

Task-local HLTP


\`\`

נבחרים רק המבחנים השייכים ל־Task הנוכחי.

## Mode 2: השלמת HLTP חסר

Input:

Task Contract


Design decisions


Acceptance criteria


Repository test infrastructure


Existing tests

Output:

Task-local HLTP

## תוכן ה־HLTP

Test scope


Requirement mapping


Test cases


Preconditions


Stimulus


Expected result


Test doubles


Error cases


Boundary cases


Lifecycle cases


Deferred coverage


Completion criteria

## HLTP Semantic Authority

ה־`Task-local HLTP` הוא ה־authority לגבי מה חייב להיות מוכח. הוא מגדיר:

What must be proven

Required scenario

Preconditions

Stimulus

Expected behavior

Negative and boundary obligations

Lifecycle obligations

Deferred coverage

לאחר שה־Test Designer מחזיר `READY`, ה־HLTP קופא מבחינת המשמעות שלו. ה־Implementer רשאי לבחור את mechanics של מימוש הבדיקה, אך אינו רשאי לשנות בשקט את ה־obligations, את expected behavior, את requirement mapping או את היקף הכיסוי המחייב.

כאשר obligation אינו ישים, נדרש escalation במקום החלשה או reinterpretation.




BLOCKED


REQUIREMENT\_NOT\_TESTABLE


DESIGN\_CLARIFICATION\_REQUIRED

## מה הוא לא עושה

- לא מממש tests.
- לא משנה production code.
- לא מתאים את ה־HLTP למימוש שכבר נבחר.
- לא בודק private implementation details ללא צורך חוזי.
- לא ממציא requirements.
- לא מרחיב את הבדיקות ל־Tasks עתידיים.

---

# 5. Agent 3: `DP-Task-Implementer`

זה יהיה ה־Agent המרכזי לביצוע העבודה.

Agent אחד יממש גם production code וגם unit tests. ההבדל יהיה ב־Skills שיופעלו לפי סוג ה־Task.

## Skills

task-skeleton-implementation


task-production-code-implementation


task-unit-test-implementation


task-implementation-verification

בנוסף הוא ישתמש ב:

dp-component-skill-router

וב־DefensePro Skills המתאימים.

## `task-skeleton-implementation`

מיועד ל:

SKELETON


INTERFACE


STRUCTURAL\_WIRING

אחראי על:

- files ו־types.
- interfaces.
- members.
- constructors.
- inheritance.
- owner/consumer wiring.
- build registration.
- structural compilation.
- placeholders בטוחים ומפורשים בלבד.

הוא חייב למנוע Skeleton שמתקמפל אך משקר לגבי behavior.

## `task-production-code-implementation`

מיועד ל:

PRODUCTION\_CODE


INTEGRATION


BUG\_FIX


REFACTORING

אחראי על:

- ביצוע ה־Execution Plan.
- מימוש רק ה־behavior של ה־Task.
- שימוש ב־APIs הקונקרטיים.
- שמירת החלטות Design.
- שמירת ownership ו־lifecycle.
- verification הדרגתי.
- עצירה במקרה של Design/code conflict.

## `task-unit-test-implementation`

אחראי על:

- מימוש ה־Task-local HLTP.
- שימוש בתשתיות הבדיקה הקיימות.
- mapping בין test case למבחן קונקרטי.
- הרצת tests ממוקדים.
- דיווח על test case שלא ניתן לממש.
- הימנעות משינוי production code רק כדי להקל על הבדיקה.

### Test Obligation Boundary

ה־Implementer הוא authority לגבי **איך** לממש את הבדיקה:

Fixture selection

Mocks / stubs / test doubles

Concrete assertions

Test-framework integration

Setup and teardown mechanics

הוא אינו רשאי לשנות בשקט:

- את משמעות ה־test obligation.
- את ה־expected behavior.
- את ה־requirement mapping.
- את תנאי ההצלחה.
- את היקף הכיסוי המחייב.
- את ה־deferred coverage שאושר.

כאשר obligation אינו ישים, ה־Implementer מבצע escalation במקום להחליש אותו.

### Execution-Plan Change Areas

ה־Implementer משתמש בשלוש קטגוריות ה־Execution Plan כך:

**Required change areas** — שינויים שה־Task מחייב ישירות.

**Expected affected files or symbols** — תחזית repository-grounded של ה־Planner; זו guidance ולא רשימת diff קשיחה.

**Protected or excluded areas** — אזורים שאסור לשנות ללא escalation או עדכון מאושר.

שינוי בקובץ שלא הופיע ב־Expected list אינו scope violation אוטומטי אם הוא נדרש ישירות ל־acceptance criterion קיים, נשאר בתוך scope ואחריות ה־Task, אינו משנה החלטת Design, אינו מממש Task עתידי, ומתועד ב־Implementation Report.

## `task-implementation-verification`

אחראי על self-verification עובדתי:

Build


Focused tests


Relevant existing tests


Diff inspection


Acceptance-criteria mapping


Changed-files validation


Unrelated-change detection

הוא אינו מחליף Review עצמאי.

## תוצר Agent מלא

Task Implementation Package


├── Code changes


├── Test changes


├── Implementation Report


├── Acceptance Criteria Evidence


├── Build/Test Evidence


├── Deviations


└── Reviewer Handoff

## סטטוסים

IMPLEMENTED


PARTIALLY\_IMPLEMENTED


BLOCKED


DESIGN\_CODE\_CONFLICT


PLAN\_UPDATE\_REQUIRED


TEST\_INFRASTRUCTURE\_BLOCKED


READY\_FOR\_REVIEW

---

# 6. Agent 4: `DP-Task-Reviewer`

ה־Reviewer חייב להיות Agent עצמאי וב־fresh context.

## Skill

task-review

אפשר להתחיל עם Skill אחד מקיף ולא לפצל מראש ל־code review, test review ו־design compliance.

## Input

Task Contract


Repository Context


Execution Plan


Relevant Design Sections


Task-local HLTP


Current Diff


Changed Files


Implementation Report


Build/Test Evidence


Evidence identities / implementation identity

Applicable DefensePro Skills

## מידע שלא מועבר

- conversation המלא של ה־Implementer.
- reasoning פנימי.
- ניסיונות קוד קודמים.
- הצדקות שלא תועדו.
- טענות completion ללא evidence.

## סדר הבדיקות

TR01  Input package integrity


TR02  Correct repository baseline


TR03  Task dependency satisfaction


TR04  Scope compliance


TR05  Acceptance-criteria coverage


TR06  Approved Design compliance


TR07  Concrete API fidelity


TR08  Responsibility boundaries


TR09  Ownership and lifetime


TR10  Initialization and cleanup


TR11  Runtime-flow correctness


TR12  Error and failure behavior


TR13  Execution context and concurrency


TR14  Performance-sensitive paths


TR15  Infrastructure reuse


TR16  DefensePro Skill compliance


TR17  HLTP coverage


TR18  Unit-test correctness


TR19  Negative and boundary coverage


TR20  Build and test evidence


TR21  Diff hygiene


TR22  Future-task boundary preservation


TR23  Cross-artifact consistency


TR24  Task completion readiness

## Review discipline

Run checks in order (`TR01` → `TR24`)

→ **V1: stop at the first failed check in deterministic TR order**

→ return one finding


→ Implementer fixes only that finding


→ rerun required verification


→ Reviewer starts again at TR01

## Output

Task Review Result


├── PASS / FAIL / BLOCKED


├── First Failing Check


├── Finding


├── Evidence


├── Required Correction


├── Required Reverification


└── Checks Passed Before Failure


\`\`

## סטטוסים

PASS


FAIL


BLOCKED


INVALID\_REVIEW\_INPUT


DESIGN\_CLARIFICATION\_REQUIRED

---

# 7. Agent 5: `DP-Task-Orchestrator`

ה־Orchestrator ייכתב אחרון.

כך לא נמציא orchestration עבור Agents שעדיין אינם קיימים.

## Skills

task-orchestration


task-skill-routing


task-completion

## `task-orchestration`

אחראי על state machine של Task:

RECEIVED


→ PLANNING


→ TEST\_DESIGN


→ IMPLEMENTATION


→ REVIEW


→ FIX


→ COMPLETION


→ READY\_FOR\_COMMIT

הוא מטפל גם ב:

BLOCKED


DESIGN\_CLARIFICATION\_REQUIRED


TASK\_SPLIT\_REQUIRED

## `task-skill-routing`

לא ישכפל את `dp-component-skill-router`.

חלוקת האחריות:

task-skill-routing:


    מחליט איזה Task Agent ואיזה Task Skill להפעיל.


dp-component-skill-router:


    מחליט אילו DefensePro domain Skills נדרשים.

לדוגמה:

Task type: PRODUCTION\_CODE


    → DP-Task-Implementer


    → task-production-code-implementation


Concern: Controller broadcasts a value to Engines


    → dp-component-skill-router


    → defensepro-ipc


## `task-completion`

בודק:

- Planner החזיר READY.
- test obligations טופלו.
- Implementer החזיר READY\_FOR\_REVIEW.
- Reviewer החזיר PASS.
- required builds עברו.
- required tests עברו.
- evidence נבדק עבור ה־repository / implementation identity הנכונים.
- אין deviations פתוחים.
- אין unrelated changes.
- ה־commit boundary קוהרנטי.
- ה־Task הנוכחי complete ו־commit-ready.
- Known downstream contracts לא בוטלו או נשברו.

ה־Task Orchestrator אינו קובע האם ה־Task הבא מוכן לביצוע. readiness של Tasks עתידיים שייך בעתיד ל־Component Orchestrator.

## Resume Invariant

ה־Orchestrator חייב להיות מסוגל לשחזר את מצב ה־Task מתוך:

Task State

Artifacts

Repository / worktree state

Evidence identities

Conversation history אינה מקור אמת להמשך תקין.

### Output

READY\_FOR\_COMMIT


FIX\_REQUIRED


BLOCKED


HUMAN\_APPROVAL\_REQUIRED

## מה ה־Orchestrator לא עושה

- לא מתכנן בעצמו.
- לא כותב HLTP.
- לא משנה קוד.
- לא מבצע Review.
- לא מתקן findings.
- לא משנה Design.
- לא מבצע commit ללא הוראה מפורשת.

---

# 8. רשימת ה־Agents וה־Skills הסופית

Agent: DP-Task-Planner


Skills:


  \- task-intake


  \- task-repository-analysis


  \- task-planning


Agent: DP-Task-Test-Designer


Skills:


  \- task-unit-test-hltp


Agent: DP-Task-Implementer


Skills:


  \- task-skeleton-implementation


  \- task-production-code-implementation


  \- task-unit-test-implementation


  \- task-implementation-verification


Agent: DP-Task-Reviewer


Skills:


  \- task-review


Agent: DP-Task-Orchestrator


Skills:


  \- task-orchestration


  \- task-skill-routing


  \- task-completion

## Skills קיימים שיופעלו כתלויות

component-requirement-analysis


dp-component-skill-router


configuration-manager


defensepro-ipc


defensepro-shared-memory


dp-counters-manager


dp-counter-table


packet-flow-defensepro


session-table-defensepro


ssl-inspection-defensepro


system-internal-commands


table-printer

לא כל ה־Skills התשתיתיים הושלמו עדיין, ולכן כל Agent חייב לדעת להחזיר `BLOCKED` כאשר ה־Skill הדרוש אינו זמין או אינו מאומת.

---

# 9. שיטת העבודה לבניית כל חבילה

לכל Agent נעבוד באותו process.

1\. Define responsibility and boundaries


2\. Define input and output contracts


3\. Define status model


4\. Define relevant Skills


5\. Write each Skill completely


6\. Review each Skill independently


7\. Run Skill-level tests


8\. Write the Agent


9\. Review Agent-to-Skill integration


10\. Run Agent-level tests


11\. Run a real Pilot


12\. Fix using first-failure discipline


13\. Require clean final PASS


14\. Save immutable baseline snapshot


15\. Continue to the next Agent package


## כלל Baseline

לכל Phase:

baseline-v0


    remains unchanged


working-v1


    receives corrections


final-v1


    is saved only after PASS

לא עובדים ישירות על baseline מאושר.

---

# 10. לו"ז העבודה

הזמנים כאן הם הערכה בימי עבודה ממוקדים. עדיף להשתמש ב־Phases וב־Gates ולא להיצמד לתאריך אם Review חושף בעיה.

## Phase 0: Foundation

**משך משוער:** יום עד יומיים

### עבודה

- הקפאת כל ה־baselines הקיימים.
- בחירת שלושה Pilot Tasks קיימים: 
  - Structural Skeleton.
  - Production Behavior.
  - Unit Test.
- איסוף ה־Design, Implementation Plan וה־HLTP שלהם.
- הגדרת schema משותף ל־Task Package.
- הגדרת authority order וסטטוסים משותפים.
- הגדרת naming conventions.
- הגדרת Artifact Authority Model מינימלי.
- הגדרת Minimum Evidence Model.
- הגדרת Formal Status Semantics.
- הגדרת Resume Invariant.

### Foundation Contracts

חמשת החוזים הבאים הם חוזים לוגיים משותפים לכל חבילות ה־Agents. הם אינם Agent ואינם Skill עצמאי. ניתן לממש אותם כחמישה קבצים או כמסמך Foundation מאוחד, כל עוד חמשת החוזים נשמרים כישויות לוגיות נפרדות.

#### `task-package-schema`

מגדיר את המבנה המשותף של Task Work Package ושל artifacts העוברים בין ה־Agents.

#### `task-authority-model`

עבור כל artifact יוגדר:

Artifact Owner

Read-only consumers

Authority scope

Freeze point

המודל אינו כולל permission system, locking או revision infrastructure כללית.

יש להבחין בין **freeze** לבין **validity**. לדוגמה, `Task Review Result` יכול להיות immutable לאחר יצירתו אך תקף רק עבור implementation identity מסוים.

מיפוי ראשוני:

Task Contract → Planner authority על scope ו־acceptance criteria.

Task Repository Context → Planner authority על observations בבסיס repository מתועד.

Task Execution Plan → Planner authority על intended execution; אינו מחליף repository truth.

Task-local HLTP → Test Designer authority על test obligations וה־expected behavior.

Implementation Report → Implementer report, אך אינו proof עצמאי.

Build/Test Evidence → execution evidence המשויך ל־repository / implementation identity הנכונים.

Task Review Result → Reviewer authority על review status עבור implementation identity מסוים.

Task State → Orchestrator authority על workflow position ו־resumption.

#### `minimum-evidence-model`

כל Evidence מחייב לכל הפחות:

Type

Action or command

Target or worktree

Baseline or implementation identity

Result or exit code

Relevant output or raw-output reference

Requirement, criterion or check mapping

הצהרה כגון `Build passed` אינה Evidence מספק. Raw logs ארוכים אינם מועתקים לתוך artifacts; נשמר summary והפניה ל־raw output כאשר הוא זמין.

#### `task-status-model`

לכל status יוגדר:

Meaning

Owner

Pipeline effect

Can resume?

Resume target

Human intervention required?

Allowed next states

יש להבחין במפורש לפחות בין:

BLOCKED

DEPENDENCY\_NOT\_READY

DESIGN\_CLARIFICATION\_REQUIRED

DESIGN\_CODE\_CONFLICT

PLAN\_UPDATE\_REQUIRED

TASK\_SPLIT\_REQUIRED

TEST\_INFRASTRUCTURE\_BLOCKED

`BLOCKED` אינו catch-all כאשר קיים status ספציפי יותר.

#### `resume-invariant`

> Task State, artifacts and repository state are the source of truth for resumption. Correct continuation must not depend on conversation history.

רמת האימות מדורגת:

Foundation → define the invariant.

Manual Pipeline → one explicit session-restart test.

Orchestrator Package → full resume behavior tests.

Hardening → compaction, blocked recovery and partial implementation.

לא ייבנה Resume Agent.

### תוצרים

task-package-schema.md

task-status-model.md

task-authority-model.md

minimum-evidence-model.md

resume-invariant.md

pilot-task-skeleton/

pilot-task-production/

pilot-task-unit-test/

### Gate

FOUNDATION\_READY

ובנוסף:

ARTIFACT_AUTHORITIES_DEFINED

MINIMUM_EVIDENCE_DEFINED

STATUS_SEMANTICS_DEFINED

RESUME_INVARIANT_DEFINED

---

## Phase 1: `DP-Task-Planner` Package

**משך משוער:** 4 עד 6 ימים

### סדר

1\. task-intake


2\. task-repository-analysis


3\. task-planning


4\. DP-Task-Planner


5\. Integration tests


6\. Pilot on all three Task types

### בדיקות עיקריות

- אינו פותח Design מחדש.
- אינו ממציא דרישות.
- מזהה Task גדול מדי.
- מזהה dependency חסר.
- משתמש בקוד כסמכות עבור APIs.
- מפיק Plan שניתן לביצוע ללא exploration מחדש.
- ה־Plan מוגבל ל־commit הנוכחי.
- ה־Planner מזהה domain concerns ומפעיל `dp-component-skill-router` רק עבור constraints רלוונטיים.
- ה־Planner אינו טוען את כל Domain Skill ואינו מבצע implementation reasoning.
- Required / Expected / Protected change areas מסווגים במפורש.
- references מספיקים אך אינם מעתיקים מסמכים ארוכים.

### Gate

PLANNER\_PACKAGE\_PASS

DOMAIN_CONCERNS_ROUTED_SELECTIVELY

PLAN_USES_RELEVANT_DOMAIN_CONSTRAINTS

CHANGE_AREAS_CLASSIFIED

---

## Phase 2: `DP-Task-Test-Designer` Package

**משך משוער:** 3 עד 4 ימים

### סדר

1\. task-unit-test-hltp


2\. DP-Task-Test-Designer


3\. Test slicing from existing Component HLTP


4\. Missing-HLTP scenario


5\. No-test-change scenario


6\. Pilot

### בדיקות עיקריות

- HLTP נגזר מהדרישות ולא מהמימוש.
- לא משכפל את כל Component HLTP.
- משייך כל test ל־requirement או acceptance criterion.
- מזהה negative, boundary ו־lifecycle cases.
- מסמן במפורש coverage שנדחה ל־Task עתידי.
- אינו מממש tests.
- HLTP semantic authority נשמר.
- HLTP obligations קופאים לאחר READY.
- יודע לומר שאין צורך בשינוי tests.

### Gate

TEST\_DESIGNER\_PACKAGE\_PASS

HLTP_SEMANTIC_AUTHORITY_PRESERVED

TEST_OBLIGATIONS_FROZEN_AFTER_READY

---

## Phase 3: `DP-Task-Implementer` Package

**משך משוער:** 7 עד 10 ימים

זו תהיה החבילה הגדולה ביותר.

### סדר

1\. task-skeleton-implementation


2\. task-production-code-implementation


3\. task-unit-test-implementation


4\. task-implementation-verification


5\. DP-Task-Implementer


6\. DefensePro Skill-router integration


7\. Three separate Pilots

### Pilots

#### Pilot A: Skeleton

בודק:

- structural correctness.
- compilation.
- build registration.
- owner/consumer wiring.
- absence of fake behavior.

#### Pilot B: Production Code

בודק:

- behavioral correctness.
- Design compliance.
- API fidelity.
- focused verification.
- no future-task implementation.
- Required / Expected / Protected change-area boundaries respected.
- discovered file changes are reported when they differ from Expected areas.

#### Pilot C: Unit Tests

בודק:

- HLTP-to-test mapping.
- concrete test infrastructure.
- focused test execution.
- no production changes without escalation.
- test obligations are not silently weakened or reinterpreted.

### Gate

IMPLEMENTER\_PACKAGE\_PASS

TEST_OBLIGATIONS_NOT_SILENTLY_CHANGED

UNEXPECTED_CHANGE_AREAS_REPORTED

PROTECTED_AREAS_NOT_CHANGED_WITHOUT_ESCALATION

---

## Phase 4: `DP-Task-Reviewer` Package

**משך משוער:** 5 עד 7 ימים

### סדר

1\. Define reviewer input package


2\. Define TR01-TR24 suite


3\. Write task-review


4\. Write DP-Task-Reviewer


5\. Enforce read-only behavior


6\. Test first-failure loop


7\. Test fresh-context handoff


8\. Review all three Pilot implementations

### נדרש לבדוק

- Reviewer אינו נשען על טענת ה־Implementer.
- findings כוללים evidence.
- הוא עוצר בכשל הראשון לפי סדר TR01→TR24.
- הוא מתחיל מחדש לאחר תיקון.
- הוא מזהה scope creep.
- הוא מזהה Design deviation.
- הוא מזהה weak tests.
- הוא מאמת build/test evidence.
- PASS ניתן רק לאחר השלמת כל הבדיקות.

### Gate

REVIEWER\_PACKAGE\_PASS

FIRST_FAILED_CHECK_BY_TR_ORDER

---

## Phase 5: End-to-End Manual Pipeline

**משך משוער:** 4 עד 6 ימים

לפני Orchestrator, נפעיל את כל ה־Agents ידנית.

### Flow

Task selected manually


→ DP-Task-Planner


→ DP-Task-Test-Designer when needed


→ DP-Task-Implementer


→ DP-Task-Reviewer


→ Fix loop


→ Human approval

→ Commit

בנוסף יבוצע פעם אחת לפחות session restart יזום באמצע pipeline, וה־Task ימשיך מתוך ה־artifacts וה־Task State ללא הסתמכות על conversation history.

### מדדים

- מספר שיחות לכל Task.
- מספר premium requests.
- מספר פעמים שה־Design נטען.
- context duplication.
- invented API count.
- scope deviation count.
- build failures שהתגלו מאוחר.
- review findings לפי קטגוריה.
- מספר fix loops.
- זמן מ־Task intake עד ready-to-commit.
- גודל artifacts.
- אחוז מידע מיותר בכל handoff.
- הצלחת session-restart resume.
- evidence validity מול baseline / implementation identity.

### Gate

נדרשים לפחות שלושה סוגי Task מוצלחים:

SKELETON       PASS


PRODUCTION     PASS


UNIT\_TEST      PASS

ורצוי גם Task רביעי שמשלב production code ו־tests.

SESSION_RESTART_RESUME_PASS

MANUAL\_PIPELINE\_STABLE

---

## Phase 6: `DP-Task-Orchestrator` Package

**משך משוער:** 5 עד 7 ימים

### סדר

1\. task-orchestration


2\. task-skill-routing


3\. task-completion


4\. DP-Task-Orchestrator


5\. Agent delegation contracts


6\. State-transition tests


7\. Failure and resume tests


8\. End-to-end Pilot

### בדיקות עיקריות

- אינו מבצע בעצמו עבודת Agent אחר.
- אינו מדלג על Ready Gate.
- מפעיל Test Designer רק כשצריך.
- מפריד Task Skills מ־DefensePro Skills.
- מחזיר finding ל־Implementer.
- מחזיר שינוי Design לאדם.
- שומר artifacts בין שיחות.
- אינו טוען את כל ההיסטוריה.
- מבוסס על Artifact Authority Model ו־Task State.
- מאמת Evidence עבור repository / implementation identity הנכונים.
- אינו מסמן READY\_FOR\_COMMIT לפני Review PASS.
- יודע להמשיך לאחר session חדש לפי Resume Invariant.

### Gate

ORCHESTRATOR\_PACKAGE\_PASS

---

## Phase 7: Hardening

**משך משוער:** 4 עד 6 ימים

### עבודה

- הרצת pipeline על Tasks אמיתיים נוספים.
- חידוד boundaries.
- הסרת instructions כפולים.
- קיצור artifacts.
- בדיקת progressive disclosure.
- בדיקת behavior לאחר context compaction.
- התאמת Review Level לפי סיכון.
- בדיקת recovery אחרי BLOCKED.
- בדיקת partial implementation.
- בדיקת Task שנדרש לפצל.
- בדיקת validity של Evidence לאחר שינוי implementation identity.
- בדיקת artifact freeze ו־authority boundaries.
- בדיקת status transitions ו־resume targets.

### Gate סופי

TASK\_SYSTEM\_V1\_READY

---

# 11. סיכום זמנים

Phase 0  Foundation                         1–2 days


Phase 1  Planner package                    4–6 days


Phase 2  Test Designer package              3–4 days


Phase 3  Implementer package                7–10 days


Phase 4  Reviewer package                   5–7 days


Phase 5  Manual end-to-end pipeline         4–6 days


Phase 6  Orchestrator package               5–7 days


Phase 7  Hardening                          4–6 days

סה"כ:

Minimum focused effort: 33 working days


Conservative estimate:   48 working days

אבל נקבל ערך מעשי הרבה לפני הסוף:

- לאחר Phase 1: Planner שמכין Tasks.
- לאחר Phase 3: Planner + Test Designer + Implementer.
- לאחר Phase 4: pipeline עצמאי עם Review.
- לאחר Phase 6: orchestration אוטומטי.

---

# 12. כיצד נחסוך טוקנים וקרדיטים

## Progressive disclosure

כל Agent יקבל רק את מה שהוא צריך.

### Planner

Task from Implementation Plan


Relevant Design references


Relevant HLTP references


Current repository

Relevant domain constraints only (via `dp-component-skill-router`)

### Test Designer

Task Contract


Acceptance criteria


Relevant Component/Interface HLTP


Relevant test infrastructure


### Implementer

Task Contract


Repository Context


Execution Plan


Task-local HLTP


Relevant source files


Relevant domain Skills

ה־Implementer אינו מסתמך על סיכום Planner כתחליף לטעינת ה־Domain Skill הנדרש למימוש.

### Reviewer

Task Work Package


Relevant Design decisions


Current diff


Tests


Evidence


Relevant domain Skills

## Artifact references

לא מעתיקים Design מלא לכל Artifact.

משתמשים ב:

Document


Section


Decision ID


Requirement ID


File


Symbol


Observed contract

Evidence ID

Implementation identity

\`

## מספר שיחות צפוי

במצב הרגיל:

Planning:       1 request


Test design:    0 or 1 request


Implementation: 1 request


Review:         1 request


Fix:            only

בהמשך ה־Orchestrator יוכל לבצע delegation, אבל עדיין צריך לוודא שהוא אינו יוצר יותר premium requests ממה שנחסך.

---

# 13. מה לא נבנה ב־V1

לא נבנה כרגע:

DP-Component-Orchestrator


DP-Feature-Orchestrator


DP-Architecture-Agent


DP-Security-Reviewer


DP-Performance-Reviewer


DP-Integration-Agent


DP-Release-Agent


DP-Git-Agent

Security ו־Performance יהיו בתחילה checks או Skills שמופעלים מתוך `task-review` רק כאשר סיווג הסיכון דורש זאת.

בנוסף לא נבנה ב־V1:

Artifact permission system מורכב

Complex revision infrastructure

Evidence database

Raw-log duplication בתוך artifacts

Resume Agent

Severity engine או smart reviewer prioritization

Full downstream Task analysis

Component dependency scheduling

לאחר ש־Task System V1 יהיה יציב, השכבה הבאה תהיה:

DP-Component-Orchestrator


    ↓


Reads approved Implementation Plan


    ↓


Selects next executable Task


    ↓


Invokes DP-Task-Orchestrator


    ↓


Records commit completion


    ↓


Advances through dependency graph


\`\`

# עקרונות V1 שאושרו

העדכונים לעיל מחזקים את חוזי המערכת ואינם משנים את ארכיטקטורת חמשת ה־Agents, שנים עשר ה־Skills או את סדר בניית החבילות.

העקרונות המאושרים ל־V1 הם:

- Artifact Authority מינימלי ללא permission system.
- Minimum Evidence Contract.
- Status Semantics פורמליים.
- Resume Invariant חוצה מערכת.
- Hybrid Domain-Skill Model עבור Planner.
- HLTP כ־semantic authority לגבי test obligations.
- Required / Expected / Protected change areas ב־Execution Plan.
- Task Completion מתייחס ל־Task הנוכחי ול־downstream contract integrity, לא ל־readiness של ה־Task הבא.
- Reviewer משתמש ב־V1 ב־first failed check לפי סדר TR01→TR24.

# ההחלטה המומלצת

נקבע את ה־backlog הרשמי הבא:

A01  DP-Task-Planner


     S01 task-intake


     S02 task-repository-analysis


     S03 task-planning


A02  DP-Task-Test-Designer


     S04 task-unit-test-hltp


A03  DP-Task-Implementer


     S05 task-skeleton-implementation


     S06 task-production-code-implementation


     S07 task-unit-test-implementation


     S08 task-implementation-verification


A04  DP-Task-Reviewer


     S09 task-review


A05  DP-Task-Orchestrator


     S10 task-orchestration


     S11 task-skill-routing


     S12 task-completion

וסדר הביצוע יהיה:

A01 + S01–S03


→ A02 + S04


→ A03 + S05–S08


→ A04 + S09


→ Manual end-to-end validation


→ A05 + S10–S12


→ Full end-to-end validation

זה מיישם בדיוק את השיטה שביקשת: **Agent יחד עם ה־Skills הרלוונטיים שלו, השלמה ובדיקת החבילה, ורק אז מעבר לחבילה הבאה.**