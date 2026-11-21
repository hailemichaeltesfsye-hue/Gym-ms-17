# Gym Management System — Trainer Requirements Specification

| Metadata | Details |
| :--- | :--- |
| **System Name** | Gym Management System (`Gym-ms-17`) |
| **Document Scope** | Actor Requirements Specification (ARS) |
| **Primary Actor** | Trainer (Certified Fitness Instructor & Personal Coach) |
| **Related Actors** | Gym Member, Cashier, Gym Manager / Admin |
| **Document Version** | 2.0 |

---

## 1. Actor Description & Operational Context

### 1.1 Actor Profile
- **Actor Name:** Trainer
- **Summary:** A certified gym staff member responsible for guiding assigned gym members through safe and effective fitness programs, conducting physical assessments, designing and updating workout and basic nutrition guidelines, managing personal training (PT) and group class schedules, recording session attendance and exercise performance, and tracking member fitness progress.
- **Operational Environment:** Operates across desktop workstations and mobile/tablet devices on the gym floor, studio spaces, or remote scheduling portals during gym operating hours.

### 1.2 Role-Based Access Control (RBAC) Matrix
To enforce separation of duties, member privacy, and financial integrity, the Trainer actor operates under strict role boundaries:

| System Resource | Create | Read | Update | Delete | Operational Constraints |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Trainer Profile & Availability** | No | Own | Own (Schedule/Bio) | No | Certifications and employment status are verified/managed by Gym Manager / Admin. |
| **Assigned Member Profiles** | No | Assigned / Class Roster | Fitness Notes Only | No | Can view active membership/PT eligibility status; cannot view or alter billing/payment details. |
| **Fitness Assessments** | Yes | Assigned Members | Own (Before Lock) | No | Finalized assessments are time-stamped and retained for historical progress comparison. |
| **Workout & Nutrition Plans** | Yes | Assigned Members | Own / Assigned | Archive Only | Plans are archived rather than hard-deleted once assigned to preserve training history. |
| **PT Sessions & Group Classes** | PT Slots | Own Schedule / Assigned Classes | Reschedule / Cancel Own | No | Cannot exceed room capacity or book overlapping sessions; cancellations follow cutoff rules. |
| **Session Attendance & Logs** | Yes | Own Sessions | Same-Day Correction | No | Marking a PT session `Completed` deducts 1 session credit; post-cutoff edits require Manager approval. |
| **Invoices, Payments & Refunds** | No | No | No | No | Strictly restricted to Cashier and Gym Manager / Admin roles. |

---

## 2. Functional Requirements

### 2.1 Authentication, Profile & Availability Management

#### FR-T01: Trainer Authentication & Session Security
- **Priority:** High
- **Description:** The system shall authenticate trainers using unique staff credentials (email/username and password, with optional MFA) and restrict access exclusively to authorized coaching, scheduling, and assigned member modules.
- **Acceptance Criteria:**
  - Invalid credentials return a generic authentication error without exposing account existence.
  - Inactive sessions automatically lock or expire after a configurable idle timeout.
  - Every login, logout, and failed login attempt is logged with timestamp and device/IP metadata.

#### FR-T02: Manage Trainer Profile, Specialties & Certifications
- **Priority:** Medium
- **Description:** The system shall allow trainers to view and update their public coaching profile (bio, specialties, profile photo) and view certification expiration alerts managed by administration.
- **Acceptance Criteria:**
  - Trainers can edit non-credential fields (bio, training focus areas, display photo).
  - Certification records (e.g., CPR/AED, CPT) display expiry warnings at 30 days and 7 days prior to expiration, but cannot be self-approved by the trainer.

#### FR-T03: Manage Working Hours & Booking Availability
- **Priority:** High
- **Description:** The system shall allow trainers to define recurring weekly working hours, break blocks, and one-off unavailability/leave exceptions used for personal training bookings.
- **Acceptance Criteria:**
  - Availability slots prevent overlapping schedules across personal training and assigned group classes.
  - Blocking an existing time slot alerts the trainer if confirmed member sessions conflict with the requested change.

---

### 2.2 Assigned Member Roster & Fitness Assessments

#### FR-T04: View Assigned Members & Training Package Eligibility
- **Priority:** High
- **Description:** The system shall allow trainers to search and view their assigned personal training clients and class participants, including membership status and remaining PT session credits.
- **Acceptance Criteria:**
  - Displays member name, contact preference, membership status (`Active`, `Expiring Soon`, `Expired`, `Suspended`), active PT package name, and remaining session balance.
  - Masks sensitive financial data (card details, payment totals, invoice history).
  - Highlights any declared health waivers, injuries, or medical clearance flags prominently on the member summary.

#### FR-T05: Conduct & Record Physical Fitness Assessments
- **Priority:** High
- **Description:** The system shall allow trainers to record structured baseline and periodic fitness assessments for assigned members (e.g., height, weight, body fat %, resting heart rate, strength/cardio benchmarks, mobility notes, and goal targets).
- **Acceptance Criteria:**
  - Automatically calculates derived metrics such as BMI from recorded height and weight.
  - Enforces valid numeric ranges for physiological measurements (e.g., weight `> 0`, heart rate within realistic bounds).
  - Supports attaching assessment timestamps and trainer attribution automatically.

#### FR-T06: Track Member Progress & Historical Trends
- **Priority:** Medium
- **Description:** The system shall provide a chronological comparison view of a member's past fitness assessments, body measurements, and workout adherence metrics.
- **Acceptance Criteria:**
  - Displays delta values (e.g., weight change, strength gain) between the baseline, previous, and latest assessments.
  - Supports visual trend summaries for key metrics over selectable date ranges (30 days, 90 days, 6 months, 1 year).

---

### 2.3 Workout & Nutrition Plan Management

#### FR-T07: Create & Customize Workout Plans
- **Priority:** High
- **Description:** The system shall allow trainers to build structured workout plans from an exercise library or custom exercises, specifying days/splits, target muscle groups, sets, reps, load/intensity, rest intervals, and coaching cues.
- **Acceptance Criteria:**
  - Supports saving reusable workout templates for common goals (e.g., *Fat Loss*, *Hypertrophy*, *Beginner Mobility*).
  - Validates that every exercise entry includes at least target sets/duration and execution instructions.

#### FR-T08: Assign & Update Member Workout Routines
- **Priority:** High
- **Description:** The system shall allow trainers to assign a workout plan to an assigned member with effective start/end dates and adjust exercise parameters as the member progresses.
- **Acceptance Criteria:**
  - Only one primary active workout plan is flagged as `Current` per member (unless multi-split concurrent plans are explicitly enabled), while prior plans remain archived in history.
  - Updates made to an assigned plan version immediately reflect in the member's portal without mutating historical logged workouts.

#### FR-T09: Create & Assign Basic Nutrition / Meal Guidelines
- **Priority:** Medium
- **Description:** The system shall allow trainers to record non-medical nutrition guidelines (daily calorie targets, macronutrient split, hydration goals, and general meal suggestions) tailored to a member's fitness goal.
- **Acceptance Criteria:**
  - Displays a standard disclaimer indicating guidelines are general fitness recommendations and not medical nutrition therapy.
  - Records any member-declared food allergies or dietary restrictions before saving recommendations.

---

### 2.4 Personal Training (PT) & Group Class Scheduling

#### FR-T10: View Trainer Schedule & Daily Calendar
- **Priority:** High
- **Description:** The system shall provide daily, weekly, and monthly calendar views showing all scheduled personal training sessions, assigned group classes, and blocked availability periods.
- **Acceptance Criteria:**
  - Distinguishes visually between `Scheduled`, `Completed`, `Cancelled`, and `No-Show` sessions as well as PT vs. Group Class events.
  - Loads current-week schedule data within 2 seconds.

#### FR-T11: Schedule, Reschedule & Cancel Personal Training Sessions
- **Priority:** High
- **Description:** The system shall allow trainers to book, reschedule, or cancel 1-on-1 or small-group personal training sessions with eligible assigned members.
- **Acceptance Criteria:**
  - Blocks scheduling if the member's membership is `Expired`/`Suspended` or if remaining PT session credits equal `0` (unless an admin override is present).
  - Prevents double-booking the trainer, the member, or an exclusive studio/zone resource at the same time.
  - Cancellation within the gym's late-cancellation window prompts confirmation and applies the configured session-credit deduction rule.

#### FR-T12: View & Manage Group Class Roster & Waitlist
- **Priority:** Medium
- **Description:** The system shall allow trainers assigned to a group class to inspect the enrolled participant list, room capacity, and active waitlist prior to and during class.
- **Acceptance Criteria:**
  - Displays current headcount vs. maximum class capacity (e.g., `18 / 20`).
  - Automatically reflects waitlist promotions processed by the system when an enrolled member cancels before cutoff.

---

### 2.5 Session Attendance, Workout Logging & Feedback

#### FR-T13: Mark Member Session & Class Attendance
- **Priority:** High
- **Description:** The system shall allow trainers to mark attendance for scheduled PT sessions (`Present / Completed`, `No-Show`, `Cancelled`) and group class participants (`Checked-In`, `Absent`).
- **Acceptance Criteria:**
  - Marking a PT session as `Completed` (or `No-Show` past policy cutoff) atomically decrements 1 credit from the member's active PT package and prevents duplicate deductions for the same session ID.
  - Requires a reason note when cancelling a session on the trainer's side.

#### FR-T14: Log In-Session Exercise Performance
- **Priority:** Medium
- **Description:** The system shall allow trainers to record actual sets, reps, weights lifted, durations, and perceived exertion (RPE) completed by a member during a training session.
- **Acceptance Criteria:**
  - Pre-populates planned sets/reps from the member's active workout plan for fast floor entry.
  - Automatically flags personal bests (PRs) achieved during the session.

#### FR-T15: Record Post-Session Coaching Notes & Member Feedback
- **Priority:** Medium
- **Description:** The system shall allow trainers to log post-session observations, form corrections, injury precautions, and readiness feedback visible to the member (or marked `Trainer-Internal Only`).
- **Acceptance Criteria:**
  - Supports toggling note visibility between `Shared with Member` and `Internal Staff Only`.
  - Appends timestamp and trainer ID to every note entry.

---

### 2.6 Communication & Reporting

#### FR-T16: Send Training Updates & Session Reminders
- **Priority:** Low
- **Description:** The system shall trigger notifications to assigned members when a workout plan is updated, a PT session is scheduled/rescheduled/cancelled, or an upcoming session reminder is due.
- **Acceptance Criteria:**
  - Uses system notification channels (in-app/email/SMS per member preferences) without exposing the trainer's personal phone number unless permitted.
  - Logs delivery status of schedule-change notifications.

#### FR-T17: Generate Trainer Session & Utilization Summary
- **Priority:** Medium
- **Description:** The system shall allow trainers to view and export a summary of their completed PT sessions, no-shows, group classes taught, and active client count over a selected date range.
- **Acceptance Criteria:**
  - Clearly breaks down `Completed`, `No-Show`, and `Cancelled` counts for payroll/commission verification with management.
  - Supports filtering by date range and exporting summary data as CSV/PDF.

---

## 3. Data Requirements (Core Trainer Entities)

To ensure clean database design and API consistency, trainer workflows interact with the following core entities:

| Entity | Key Attributes | Integrity & Validation Rules |
| :--- | :--- | :--- |
| **`TrainerProfile`** | `trainerId`, `staffId`, `bio`, `specialties[]`, `certifications[]`, `certExpiryDate`, `status` (`Active`/`Inactive`) | Linked 1:1 to a valid staff user with `Trainer` role; `certExpiryDate` monitored for compliance. |
| **`TrainerAvailability`** | `availabilityId`, `trainerId`, `dayOfWeek`, `specificDate` (nullable), `startTime`, `endTime`, `isBlocked`, `reason` | `endTime > startTime`; overlapping active availability blocks for the same trainer are disallowed. |
| **`TrainingSession`** | `sessionId`, `sessionType` (`PT`/`GroupClass`), `trainerId`, `memberId` (nullable for class), `classId` (nullable), `scheduledStart`, `scheduledEnd`, `capacity`, `status`, `creditDeducted` | `scheduledEnd > scheduledStart`; no overlapping `Scheduled` sessions for the same `trainerId`; `creditDeducted` is idempotent. |
| **`FitnessAssessment`** | `assessmentId`, `memberId`, `trainerId`, `assessedAt`, `weightKg`, `heightCm`, `bmi`, `bodyFatPct`, `restingHr`, `benchmarksJson`, `notes` | All physical metrics must be positive and within physiological bounds; `bmi` auto-derived. |
| **`WorkoutPlan`** | `planId`, `memberId`, `trainerId`, `title`, `goal`, `startDate`, `endDate`, `exercisesJson`, `status` (`Draft`/`Active`/`Archived`) | Only one `Active` primary plan per member unless concurrent plans are enabled; archived plans are read-only. |
| **`SessionAttendanceLog`** | `logId`, `sessionId`, `memberId`, `trainerId`, `attendanceStatus` (`Completed`/`NoShow`/`Cancelled`), `performanceJson`, `sharedNote`, `internalNote`, `recordedAt` | Unique per `(sessionId, memberId)`; locked after payroll/day cutoff unless unlocked by Admin. |

---

## 4. Non-Functional Requirements

- **NFR-T01 (Member Health & Data Privacy):** Member body metrics, medical flags, injury notes, and internal coaching remarks shall only be accessible to the assigned trainer(s) and authorized Gym Managers/Admins, never to unauthorized members or front-desk billing views.
- **NFR-T02 (Schedule Conflict Prevention & Atomicity):** Session booking, rescheduling, attendance confirmation, and PT session-credit deduction shall execute within an atomic database transaction so concurrent bookings cannot double-book a trainer or over-deduct member credits.
- **NFR-T03 (Performance SLA):** Trainer daily calendars, assigned member lists, and workout plan editors shall load in `< 2.0 seconds`; attendance submission and session logging shall complete in `< 1.5 seconds` under normal gym operating load.
- **NFR-T04 (Mobile & Gym-Floor Usability):** The trainer interface shall be responsive for tablet and mobile screens with touch-friendly controls for rapid in-session set/rep logging and one-tap attendance marking.
- **NFR-T05 (Auditability & Traceability):** All session status transitions (`Scheduled -> Completed / Cancelled / No-Show`), PT credit deductions, and post-cutoff edits shall be recorded in an immutable audit trail with `staffId`, `timestamp`, and `reason`.
- **NFR-T06 (Session Security & Transport):** All trainer portal API calls and client sessions shall transmit exclusively over HTTPS/TLS with strict server-side RBAC enforcement on every endpoint.

---

## 5. Detailed Use Case Specifications

### UC-T01: Configure Trainer Working Hours & Availability
- **Primary Actor:** Trainer
- **Trigger:** Trainer sets up weekly shifts or needs to block off time for leave/training.
- **Preconditions:** Trainer is authenticated with an `Active` staff account.
- **Main Flow:**
  1. Trainer opens **My Availability** in the trainer portal.
  2. Trainer selects recurring weekdays and defines start/end working hours, or picks a specific date to add a block-out period.
  3. Trainer submits the availability update.
  4. System validates that no time inversions or conflicting scheduled sessions exist, saves the `TrainerAvailability` records, and updates open booking slots.
- **Exception Flow:**
  - *E1 (Existing Session Conflict):* System detects confirmed PT sessions or group classes within the proposed blocked window, prevents saving the override blindly, and prompts the trainer to reschedule/cancel conflicting sessions first.
- **Postconditions:** Updated booking availability is immediately enforced across member and staff scheduling views.

---

### UC-T02: Review Member Eligibility & Conduct Fitness Assessment
- **Primary Actor:** Trainer
- **Secondary Actor:** Gym Member
- **Trigger:** An assigned member begins a new program or arrives for a scheduled progress check-in.
- **Preconditions:** Trainer is logged in; member exists and is assigned to the trainer.
- **Main Flow:**
  1. Trainer opens the member's coaching profile and verifies active membership status, declared health/injury flags, and PT credit eligibility.
  2. Trainer navigates to **Fitness Assessments** and selects **New Assessment**.
  3. Trainer enters measured values (weight, height, body fat %, resting heart rate, strength/cardio test scores) and qualitative posture/mobility notes.
  4. System validates input bounds, computes derived metrics (e.g., BMI, deltas vs. prior assessment), and saves the timestamped `FitnessAssessment` record.
- **Exception Flows:**
  - *E1 (Unresolved Medical/Waiver Flag):* System displays a prominent safety banner indicating missing waiver or medical clearance before proceeding with strenuous baseline testing.
  - *E2 (Invalid Measurement Input):* System highlights out-of-range fields inline and blocks saving until corrected.
- **Postconditions:** Baseline or progress assessment is permanently recorded and reflected in the member's progress history.

---

### UC-T03: Create & Assign Personalized Workout & Nutrition Plan
- **Primary Actor:** Trainer
- **Secondary Actor:** Gym Member
- **Trigger:** Trainer completes an assessment or updates an existing client's training cycle.
- **Preconditions:** Trainer is authenticated and assigned to the member; member has an active gym membership.
- **Main Flow:**
  1. Trainer selects **Create Workout Plan** (from scratch or an existing template) for the assigned member.
  2. Trainer defines plan title, training goal, start/end dates, training days, exercises, sets, reps, load targets, and coaching instructions.
  3. *(Optional)* Trainer attaches general daily calorie, macro, and hydration guidelines along with dietary allergy acknowledgments.
  4. Trainer clicks **Publish & Assign Plan**.
  5. System archives any previous `Active` plan for that member, activates the new `WorkoutPlan`, and triggers a notification to the member.
- **Postconditions:** Member sees the updated routine in their portal; historical plans remain preserved in the archive.

---

### UC-T04: Schedule, Reschedule, or Cancel Personal Training Session
- **Primary Actor:** Trainer
- **Secondary Actor:** Gym Member
- **Trigger:** Member requests a PT appointment or trainer adjusts an upcoming booking.
- **Preconditions:** Trainer is logged in; member has an `Active` membership and available PT session credits.
- **Main Flow:**
  1. Trainer opens **Schedule** and selects an open availability slot.
  2. Trainer selects the assigned member, session duration, and training zone/location.
  3. System verifies: (a) trainer has no overlapping session, (b) member has no overlapping session, and (c) member's remaining PT credits cover the booking.
  4. Trainer confirms the booking; system creates the `TrainingSession` (`status = Scheduled`) and sends a booking confirmation to the member.
- **Alternative & Exception Flows:**
  - *A1 (Reschedule/Cancel):* Trainer selects an existing `Scheduled` session, chooses a new valid slot or **Cancel**, enters a mandatory reason, and confirms; system updates status and notifies the member.
  - *E1 (Zero PT Credits / Expired Membership):* System blocks PT booking confirmation and prompts the member/trainer to settle a renewal or PT package purchase with the **Cashier**.
  - *E2 (Time Slot Conflict):* System rejects the booking and displays the conflicting session details.
- **Postconditions:** Calendar is updated atomically without double-booking.

---

### UC-T05: Conduct Group Class & Record Attendance
- **Primary Actor:** Trainer
- **Secondary Actor:** Gym Member(s)
- **Trigger:** Scheduled group fitness class start time arrives.
- **Preconditions:** Trainer is assigned as the instructor for the scheduled group class.
- **Main Flow:**
  1. Trainer opens the **Class Roster** for the scheduled session.
  2. System displays enrolled members, waitlist count, and capacity limits.
  3. Trainer marks each participant as `Checked-In` or `Absent` as members arrive.
  4. Upon class completion, trainer submits final attendance.
  5. System locks the attendance log and updates class participation metrics.
- **Alternative Flow:**
  - *A1 (Walk-In Member Spot Available):* If enrolled headcount is below capacity after waitlist cutoffs, trainer or front desk adds an eligible `Active` member to the remaining open spot before submitting.
- **Postconditions:** Accurate group class attendance is stored for member engagement and trainer reporting.

---

### UC-T06: Log In-Session Performance & Post-Session Feedback
- **Primary Actor:** Trainer
- **Secondary Actor:** Gym Member
- **Trigger:** Trainer completes or conducts a 1-on-1 personal training session.
- **Preconditions:** A valid `Scheduled` PT `TrainingSession` exists for the trainer and member.
- **Main Flow:**
  1. Trainer opens the active PT session and marks attendance as `Completed`.
  2. System pre-loads exercises from the member's `Active` workout plan.
  3. Trainer records completed sets, reps, weights, RPE, and any personal records achieved.
  4. Trainer enters `Shared with Member` coaching feedback and optional `Internal Staff Only` safety/progress notes.
  5. Trainer saves and closes the session.
  6. System atomically saves the `SessionAttendanceLog`, marks `TrainingSession.status = Completed`, and deducts 1 PT credit from the member's package.
- **Exception Flow:**
  - *E1 (Member No-Show):* Trainer marks `No-Show` after the grace window; system logs the attendance status, applies the gym's no-show credit rule, and skips workout performance logging.
- **Postconditions:** Session performance is stored in member history and PT package balance is accurately updated.

---

### UC-T07: Review Trainer Session & Utilization Summary
- **Primary Actor:** Trainer
- **Trigger:** End of week, payroll cutoff, or trainer performance review.
- **Preconditions:** Trainer is authenticated; historical session records exist.
- **Main Flow:**
  1. Trainer navigates to **Session Reports** and selects a start and end date.
  2. System aggregates all PT sessions and group classes conducted by the trainer within the period, grouped by status (`Completed`, `No-Show`, `Cancelled`).
  3. Trainer reviews individual session logs for accuracy and optionally exports the summary as CSV or PDF.
- **Postconditions:** Trainer has an auditable record of completed coaching activity matching management payroll logs.

---

## 6. Business Rules & Constraints

| Rule ID | Rule Name | Specification |
| :--- | :--- | :--- |
| **BR-T01** | **Eligibility & Credit Gatekeeping** | A trainer cannot book or conduct a paid Personal Training session for a member whose membership is not `Active` or whose remaining PT session credits equal `0` (unless overridden by an Admin). |
| **BR-T02** | **Single-Booking Integrity** | A trainer cannot be scheduled for two overlapping PT sessions or group classes simultaneously; every booking must fall within active `TrainerAvailability` hours. |
| **BR-T03** | **Idempotent Credit Deduction** | Completing a PT session (or marking a policy-eligible `No-Show`) must deduct at most one (`1`) credit per `sessionId`, regardless of retries or duplicate form submissions. |
| **BR-T04** | **Safety & Medical Boundary** | Trainers must acknowledge any active member injury/medical flags before assigning plans or assessments, and nutrition entries must carry non-medical fitness disclaimers. |
| **BR-T05** | **Immutable Historical Coaching Logs** | Completed workout logs, assessments, and past active plans cannot be hard-deleted; corrections after the same-day edit window require Manager/Admin authorization and audit logging. |
| **BR-T06** | **Financial Separation of Duties** | Trainers have zero access to POS checkout, cash shifts, invoices, discounts, or refund processing; all financial settlements must flow through the **Cashier** role. |

---

## 7. Requirements Traceability Matrix

| Requirement ID | Requirement Title | Mapped Use Case(s) | Governing Business Rule(s) |
| :--- | :--- | :--- | :--- |
| **FR-T01** | Trainer Authentication & Session Security | UC-T01 | BR-T06 |
| **FR-T02** | Manage Trainer Profile & Certifications | UC-T01 | BR-T04 |
| **FR-T03** | Manage Working Hours & Availability | UC-T01 | BR-T02 |
| **FR-T04** | View Assigned Members & Eligibility | UC-T02, UC-T04 | BR-T01, BR-T04, BR-T06 |
| **FR-T05** | Conduct & Record Fitness Assessments | UC-T02 | BR-T04, BR-T05 |
| **FR-T06** | Track Member Progress & Trends | UC-T02 | BR-T05 |
| **FR-T07** | Create & Customize Workout Plans | UC-T03 | BR-T04 |
| **FR-T08** | Assign & Update Member Workout Routines | UC-T03 | BR-T04, BR-T05 |
| **FR-T09** | Create & Assign Basic Nutrition Guidelines | UC-T03 | BR-T04 |
| **FR-T10** | View Trainer Schedule & Daily Calendar | UC-T04, UC-T05 | BR-T02 |
| **FR-T11** | Schedule, Reschedule & Cancel PT Sessions | UC-T04 | BR-T01, BR-T02 |
| **FR-T12** | Manage Group Class Roster & Waitlist | UC-T05 | BR-T02 |
| **FR-T13** | Mark Session & Class Attendance | UC-T05, UC-T06 | BR-T03, BR-T05 |
| **FR-T14** | Log In-Session Exercise Performance | UC-T06 | BR-T05 |
| **FR-T15** | Record Coaching Notes & Member Feedback | UC-T06 | BR-T05 |
| **FR-T16** | Send Training Updates & Reminders | UC-T03, UC-T04 | — |
| **FR-T17** | Generate Trainer Session & Utilization Summary | UC-T07 | BR-T03, BR-T05, BR-T06 |
