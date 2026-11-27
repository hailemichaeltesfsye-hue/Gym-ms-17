#Gym Management System-Receptionist Requirments Specification
|Metadata | Details|
|---|---|
|System Name |Gym Management System (`Gym-ms-17`)|
|Document Scope|Actor Requirments Specification (ARS)|
|Primary Actor|Receptionist|
|Related Actors|Gym Member,Cashier,Gym Manager/Admin|
|Document Version|1.0|
---
## 1.Actor Description and Operational Context 
### 1.1 Actor Profile
-**Actor Name**Receptionist
-**Summary:** A receptionist is a front-desk staff member responsible for registering membership plans, processing membership payments,generating payment receipts, recording gym attendance,checking membership status, and renewing expired memberships.
-**Operational Environment:** Operates through desktop workstations or tablets at the gym reception/front desk during gym operating hours.
### 1.2 Role-Based Access Control (RBAC) Matrix
To maintain member privacy,accurate financial records,and separation of responsibilities, the Receptionist operates under the foloowing access permission:
|System Resource|Create |Read|Update|Delete|Operational Constraints|
|---|---|---|---|---|---|
|Member Profiles|Yes|All Member|Personal informatio|No|can not modify financial records directly.|
|Membership Plans |No|Avaliable plans|No|No| can register available plans for members.|
|Membership Records|Yes|All Members|Renew Menbership|No|cannot change plan prices without Admin authorization.|
|Payment & Receipts|Yes|Member Payment History|No|No|cannot delete or modify complete payment transaction.|
|Attendance Records|Yes|Member Attendance|Authorized Corrections|No|Duplicate attendance record are not allowed.|
|Member Status |No|All Members|Through Renewal|No|Membershipstatus is updated according to membership dates and payment records.|
|Staff Accounts |No|No|No|No|Managed by Gym Manager /admin.|
|Trainer Fitness Records|No|No|No|No|Restricted to authorized trainers and management.|
---
## 2.Function Requirements
### 2.1 Authentication and Member Managment
#### FR-R01:Receptionist Authentication
-**priority:** High
-**Description:**The system shall allow receptionists to securely log in and log out using their authorized staff credentials.
-**Acceptance Criteria:**
-Valid credentials allow access to the receptionist dashboard.
-Invalid credentials display an authentication error.
-The system terminates or locks inactive sessions after a configured period.
-login and logout activites are recorded.
#### FR-R02:Register New Gym Member
-**priority:** High
-**Description:**The system shall allow receptionists to register new gym members by entering their personal information,including full name,phone number,gender,address,and other required details.
-**Acceptance Criteria:**
- the system generates a unique Member ID for each registered member.
-Required fields must be completed before registration.
-Duplicate member records are checked using appropriate identifying information.
-Successfully registered members are stored in the database.
#### FR-R03:View Registered Member Information
-**priority:** High
-**Description:**The system shall allow receptionists to view the personal information and mmbership details of registered gym members.
-**Acceptance Criteria:**
-Display member ID,full name,phone number and registration date.
-Display the member's current membership status.
-Only authorized information is displayed.
#### FR-R04:Update Member Personal Information
-**priority:** High
-**Description:**The system shall allow receptionists to update existing members' personal information when changes are requested.
-**Acceptance Criteria:**
-Receptionists can update permitted personal information such as phone number and address.
- The system validates updated information before saving.
-Changes are saved and reflected in the member profile.
#### FR-R05:Receptionist Authentication
-**priority:** Search Gym Member
-**Description:**The system shall allow receptionists to search for registered members using their full name or unique ID. 
-**Acceptance Criteria:**
-Search results display matching members records.
-The system displays an appropriate message when no matching member is found.
-search results can be opened to view member details.
## 2.2 Membership Management
#### FR-R06:Register Membership Plans
-**priority:** High
-**Description:**The system shall allow receptionists to assign an availlable membership plan to a registered gym member.
-**Acceptance Criteria:**
-Displays available membership plans and their prices.
-Allows selection of an approprite membership plan.
-Records memmbership start date and expiration date.
-Associates the selected plan with the correct member.
#### FR-R07:Check Membership Status and Expiration Date
-**priority:** High
-**Description:**The system shall allow receptionists to check the current membership status and expiration date of gym members.
-**Acceptance Criteria:**
-Display membership status such as `Active`,`Expired`,or `Suspended`.
-Displays membership start and expiration dates.
-Identifiesmmemberships that are approaching expiration.
-Membership status is update according to valid membership records and business rules.
#### FR-R08:Renew Expired Memberships
-**priority:** High
-**Description:**The system shall allow receptionists to renew expired gym memberships by selecting an available membership plan and recording the required payment.
-**Acceptance Criteria:**
-Receptionists can select an available renewal plan.
-The system calculates the new membership,expiration date.
-The renewal is confirmed after successfull payment.
-The updated membership status and dates are saved.
---
### 2.3 Payment and Receipt Management
#### FR-R09:Record Membership Payment
-**priority:** High
-**Description:**The system shall allow receptionists to record membership payments made by gym members.
-**Acceptance Criteria:**
-Records the member ID,Payment amount,Payment date,and payment method.
-Validates the payment before processing.
-Generates a unique transaction reference for each successful payment.
-Unsucessful payments must not be recorded as completed transactions.
#### FR-R010:Generate Payment Receipts
-**priority:** High
-**Description:**The system shall automatically generate a payment receipt after a successful membership payment.
-**Acceptance Criteria:**
-Each receipt contains a unique receipt number.
-Display member name, Member ID,Payment amount,payment date,and payment purpose.
-Receipt information corresponds to the successful payment transaction.
-A receipt is not generated for an unsuccessful payment.
#### FR-R011: Print or provide payment Receipts
-**priority:** High
-**Description:**The system shall allow receptionists to print or provide generated payment receipts to gym members.
-**Acceptance Criteria:**
-Receptioists can view generated receipts.
-The system supports printing receipts.
-The receipt can be provided in a suitable digital format when supported.
#### FR-R012:View Member Payment History
-**priority:** High
-**Description:**The system shall  receptionists to view the payment history of registered gym members.
-**Acceptance Criteria:**
-Displays previous successful payment transactions.
-Shows paymentdate,amount,payment method and receipt reference.
-Allows receptionists to identify previous membership payments.
### 2.4 Attendance Managment
#### FR-R013:Record Gym Member Attendance
-**priority:** High
-**Description:**The system shall  receptionists to record the attendance of gym members when they arrive at the gym.
-**Acceptance Criteria:**
-Attendance is recorded using the member ID or member profile.
-The system verifies the member's membership status before recording attendance .
-Records attendance date and check-in time.
-prevents duplicate attendance records for the same member during the same attendance session.
---
## 3.Date  Requirments (Core Receptionist Entities)
TOensure accurate member registration,membership management,payment processing,and attendance tracking,the receptionist workflows interact with the following core entities:
|Entity|Key Attributes|Integrity and Validation|Rules|
|---|---|---|
|`Member`|`memberId`,`fullName`,`phone number`,`gender`,`address`,`registrationDate`|`memberId` must be unique;required personal information must be valid.|
|`MembershipPlan`|`planId`,`planName`,`duration`,`price`,`descripition`|plan price must be non-negative;duration must be valid.|
|`Membership`|`membershipId`,`memberId`,`planId`,`startDate`,`endDate`,`status`|Must referenc an existing member and plan;`endDate`must be later than `startDate`.|
|`payment`|`paymentId`,`memberId`,`membershipId`,`amount`,`paymentDate`,`paymentMethod`,`status`|payment must reference a valid member and membership;completed transactions must not be duplicated.|
|`Receipt`|`receiptId`,`patmentId`,`receiptNumber`,`issuedDate`,`amount`|Each receipt must reference a successful payment and ave aunique receipt number.|
|`Attendance`|`attendanceId`,`memberId` ,`checkInDate`,`checkInTime`|Must referencce a valid member;dupicate check-ins for the same attendance session are prevented.
|`Receptionist`|`receptionistId`,`staffId`,`username`,`passwordHash`,`status`|Must be linked to a valid staff account with the Receptionist role.|
## 4. Non-Functional Requirements

- NFR-R01 (Security & Access Control): The system shall authenticate receptionists and enforce role-based access control to prevent unauthorized access to member and financial information.
- NFR-R02 (Data Privacy): Member personal information and payment records shall only be accessible to authorized staff members.
- NFR-R03 (Performance): Member searches and profile retrieval shall normally complete within 2 seconds under regular gym operating conditions.
- NFR-R04 (Data Integrity): Member registration, membership renewal, payment processing, and attendance recording shall maintain accurate and consistent database records.
- NFR-R05 (Usability): The receptionist interface shall provide a simple and user-friendly dashboard for managing members, payments, memberships, and attendance.
- NFR-R06 (Availability): The system shall be available during gym operating hours, except during scheduled maintenance or unexpected service interruptions.
- NFR-R07 (Auditability): Important receptionist activities, including member updates, payments, renewals, and attendance records, shall be logged with staff ID and timestamps.
- NFR-R08 (Reliability): The system shall prevent duplicate payment transactions and ensure that receipts are generated only for successful payments.
## 5. Detailed Use Case Specifications

### UC-R01: Login and Logout

- Primary Actor: Receptionist
- Trigger: Receptionist wants to access or leave the Gym Management System.
- Preconditions: Receptionist has an active staff account.
- Main Flow:
  1. Receptionist opens the Gym Management System.
  2. System displays the login page.
  3. Receptionist enters username and password.
  4. Receptionist clicks the Login button.
  5. System validates the credentials.
  6. System displays the receptionist dashboard.
  7. When finished, receptionist selects Logout.
  8. System terminates the authenticated session.
- Exception Flow:
  - E1 (Invalid Credentials): System displays an error message and requests valid credentials.
  - E2 (Inactive Account): System denies access to an inactive receptionist account.
- Postconditions: Receptionist is authenticated or the session is securely terminated.

---

### UC-R02: Register New Gym Member

- Primary Actor: Receptionist
- Secondary Actor: Gym Member
- Trigger: A new person requests gym membership.
- Preconditions: Receptionist is logged in.
- Main Flow:
  1. Receptionist opens the Member Management section.
  2. Receptionist selects Register New Member.
  3. System displays the registration form.
4. Receptionist enters the member's personal information.
  5. Receptionist submits the registration form.
  6. System validates the entered information.
  7. System generates a unique Member ID.
  8. System saves the new member information in the database.
  9. System displays a successful registration message.
- Exception Flows:
  - E1 (Missing Required Information): System highlights the required fields.
  - E2 (Duplicate Member): System identifies a possible existing member record and prevents unintended duplicate registration.
  - E3 (Invalid Information): System requests correction of invalid information.
- Postconditions: A new member record is successfully created.

---

### UC-R03: View, Search & Update Member Information

- Primary Actor: Receptionist
- Secondary Actor: Gym Member
- Trigger: Receptionist needs to find or update an existing member's information.
- Preconditions: Receptionist is logged in and the member is registered.
- Main Flow:
  1. Receptionist opens Member Management.
  2. Receptionist enters the member's name or Member ID in the search field.
  3. System searches the database.
  4. System displays matching member records.
  5. Receptionist selects the required member.
  6. System displays the member's personal and membership information.
7. If an update is needed, receptionist selects Edit.
  8. Receptionist modifies the permitted personal information.
  9. Receptionist saves the changes.
  10. System validates and updates the member record.
- Exception Flows:
  - E1 (Member Not Found): System displays a message indicating that no matching member exists.
  - E2 (Invalid Information): System rejects invalid updated information.
- Postconditions: The member information is displayed or successfully updated.

---

### UC-R04: Register Membership Plan

- Primary Actor: Receptionist
- Secondary Actor: Gym Member
- Trigger: A registered member wants to purchase a gym membership plan.
- Preconditions: Receptionist is logged in and the member exists in the system.
- Main Flow:
  1. Receptionist searches for and selects the member.
  2. Receptionist opens Membership Management.
  3. System displays available membership plans.
  4. Receptionist selects the appropriate membership plan.
  5. System displays the plan price and duration.
  6. Receptionist confirms the selected plan.
  7. System creates a membership record.
  8. System stores the membership start and expiration dates.
- Exception Flow:
  - E1 (Unavailable Plan): System displays a message that the selected plan is unavailable.
  - E2 (Invalid Member): System prevents membership registration for a non-existing member.
- Postconditions: The selected membership plan is associated with the member.
### UC-R04: Register Membership Plan

- Primary Actor: Receptionist
- Secondary Actor: Gym Member
- Trigger: A registered member wants to purchase a gym membership plan.
- Preconditions: Receptionist is logged in and the member exists in the system.
- Main Flow:
  1. Receptionist searches for and selects the member.
  2. Receptionist opens Membership Management.
  3. System displays available membership plans.
  4. Receptionist selects the appropriate membership plan.
  5. System displays the plan price and duration.
  6. Receptionist confirms the selected plan.
  7. System creates a membership record.
  8. System stores the membership start and expiration dates.
- Exception Flow:
  - E1 (Unavailable Plan): System displays a message that the selected plan is unavailable.
  - E2 (Invalid Member): System prevents membership registration for a non-existing member.
- Postconditions: The selected membership plan is associated with the member.

---

### UC-R05: Record Payment, Generate & Print Receipt

- Primary Actor: Receptionist
- Secondary Actor: Gym Member
- Trigger: A gym member makes a membership payment.
- Preconditions: Receptionist is logged in; the member and relevant membership record exist.
- Main Flow:
  1. Receptionist selects the member's membership record.
  2. System displays the amount due.
  3. Receptionist enters or confirms the payment amount.
4. Receptionist selects the payment method.
  5. Receptionist submits the payment information.
  6. System validates and processes the payment.
  7. System records the successful payment transaction.
  8. System automatically generates a unique payment receipt.
  9. Receptionist views the generated receipt.
  10. Receptionist prints or provides the receipt to the member.
- Exception Flows:
  - E1 (Invalid Payment Amount): System rejects the invalid amount.
  - E2 (Payment Failure): System displays a payment failure message and does not generate a successful payment receipt.
  - E3 (Receipt Generation Failure): System retains the successful payment record and allows an authorized retry of receipt generation.
- Postconditions: Successful payment is recorded and a corresponding receipt is generated.

---

### UC-R06: Check Membership Status & Renew Membership
- Primary Actor: Receptionist
- Secondary Actor: Gym Member
- Trigger: A member requests membership status information or wants to renew an expired membership.
- Preconditions: Receptionist is logged in and the member exists.
- Main Flow:
  1. Receptionist searches for the member using Member ID or name.
  2. System displays the member's membership information.
 3. Receptionist checks the membership status and expiration date.
  4. If the membership has expired, receptionist selects Renew Membership.
  5. System displays available renewal plans.
  6. Receptionist selects the renewal plan.
  7. System calculates the new membership period and amount due.
  8. Receptionist records the required payment.
  9. System confirms the successful payment.
  10. System updates the membership record and expiration date.
  11. System generates a payment receipt.
- Exception Flows:
  - E1 (Member Not Found): System displays an appropriate error message.
  - E2 (Payment Failure): System does not confirm the renewal.
  - E3 (Unavailable Renewal Plan): System requests selection of another available plan.
- Postconditions: The membership status and expiration date are updated after successful renewal payment.

---

### UC-R07: Record Gym Member Attendance

- Primary Actor: Receptionist
- Secondary Actor: Gym Member
- Trigger: A member arrives at the gym.
- Preconditions: Receptionist is logged in and the member is registered.
- Main Flow:
  1. Receptionist opens the Attendance Management section.
  2. Receptionist searches for the member using Member ID.
3. System retrieves the member's information.
  4. System checks the current membership status.
  5. If membership is active, receptionist records the member's attendance.
  6. System saves the attendance date and check-in time.
  7. System displays a successful attendance confirmation.
- Exception Flows:
  - E1 (Expired Membership): System displays an expired membership warning and prevents normal check-in.
  - E2 (Duplicate Attendance): System detects an existing attendance record for the same session and prevents duplication.
  - E3 (Member Not Found): System displays a member-not-found message.
- Postconditions: Member attendance is successfully recorded.

---

### UC-R08: View Member Payment History

- Primary Actor: Receptionist
- Secondary Actor: Gym Member
- Trigger: Receptionist needs to review a member's previous payments.
- Preconditions: Receptionist is logged in and the member has payment records.
- Main Flow:
  1. Receptionist opens Member Management.
  2. Receptionist searches for the member.
  3. Receptionist selects the member profile.
  4. Receptionist opens Payment History.
  5. System retrieves previous payment records.
  6. System displays payment dates, amounts, methods, and receipt references.
  7. Receptionist reviews the payment information.
- Exception Flow:
- E1 (No Payment Records): System displays a message indicating that no previous payments are available.
- Postconditions: The member's payment history is displayed.
## 6. Business Rules & Constraints

| Rule ID | Rule Name | Specification |
|---|---|---|
| BR-R01 | Unique Member Registration | Every registered gym member must have a unique Member ID. |
| BR-R02 | Required Member Information | Required member information must be validated before registration. |
| BR-R03 | Membership Eligibility | Only registered members can be assigned membership plans. |
| BR-R04 | Payment Validation | A payment must be validated and successfully processed before being marked as completed. |
| BR-R05 | Receipt Generation | Every successful payment must have a corresponding unique payment receipt. |
| BR-R06 | Membership Renewal | An expired membership becomes active again only after a valid renewal is confirmed. |
| BR-R07 | Attendance Eligibility | Only members with active membership status can complete normal gym check-in. |
| BR-R08 | Duplicate Attendance Prevention | A member cannot have duplicate attendance records for the same attendance session. |
| BR-R09 | Financial Record Integrity | Completed payment transactions cannot be directly deleted or modified by receptionists. |
| BR-R10 | Role-Based Access | Receptionists cannot access trainer-only fitness assessments or modify administrative system settings. |

---

## 7. Requirements Traceability Matrix
| Requirement ID | Requirement Title | Mapped Use Case(s) | Governing Business Rule(s) |
|---|---|---|---|
| FR-R01 | Receptionist Authentication | UC-R01 | BR-R10 |
| FR-R02 | Register New Gym Members | UC-R02 | BR-R01, BR-R02 |
| FR-R03 | View Member Information | UC-R03 | BR-R10 |
| FR-R04 | Update Member Information | UC-R03 | BR-R02, BR-R10 |
| FR-R05 | Search Gym Members | UC-R03, UC-R06, UC-R07 | BR-R01 |
| FR-R06 | Register Membership Plans | UC-R04 | BR-R03 |
| FR-R07 | Record Membership Payments | UC-R05, UC-R06 | BR-R04, BR-R09 |
| FR-R08 | Generate Payment Receipts | UC-R05, UC-R06 | BR-R05 |
| FR-R09 | Print or Provide Receipts | UC-R05 | BR-R05 |
| FR-R10 | Check Membership Status | UC-R06, UC-R07 | BR-R06, BR-R07 |
| FR-R11 | Record Gym Attendance | UC-R07 | BR-R07, BR-R08 |
| FR-R12 | View Member Payment History | UC-R08 | BR-R09 |
| FR-R13 | Renew Expired Memberships | UC-R06 | BR-R04, BR-R06 |
