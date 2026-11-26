# Gym Member Requirements

## 1. Actor Description

**Actor Name:** Gym Member

**Description:** A person who registers with the gym
and uses the system to manage their membership,
view membership plans, and check payment history.

## 2. Functional Requirements

- FR-M01: The system shall allow gym members to register.
- FR-M02: The system shall allow gym members to log in and log out.
- FR-M03: The system shall allow members to view available membership plans.
- FR-M04: The system shall allow members to view their membership status.
- FR-M05: The system shall allow members to renew their membership.
- FR-M06: The system shall allow members to view their payment history.
- FR-M07: The system shall allow members to update their personal information.

## 3. Use Cases

1. Register Account
2. Login
3. View Membership Plans
4. View Membership Status
5. Renew Membership
6. View Payment History
7. Update Profile

## 4. Requirement Priorities and Acceptance Criteria

### FR-M01: Member Registration

**Priority:** High

**Acceptance Criteria:**
- The system shall allow new members to enter their required registration information.
- The system shall validate the submitted information.
- The system shall create a member account when all required information is valid.
- The system shall display an appropriate error message when required information is missing or invalid.

### FR-M02: Member Login and Logout

**Priority:** High

**Acceptance Criteria:**
- The system shall allow registered members to log in using valid credentials.
- The system shall reject incorrect login credentials.
- The system shall allow authenticated members to log out.
- The system shall prevent unauthenticated users from accessing member-only functions.

### FR-M03: View Membership Plans

**Priority:** Medium

**Acceptance Criteria:**
- The system shall display all available membership plans.
- Each plan shall display its name, duration, and price.
- Members shall be able to view the details of available plans.

### FR-M04: View Membership Status

**Priority:** High

**Acceptance Criteria:**
- The system shall display the member's current membership status.
- The system shall display the membership start date and expiration date.
- The system shall indicate whether the membership is active or expired.

### FR-M05: Renew Membership

**Priority:** High

**Acceptance Criteria:**
- The system shall allow members to request membership renewal.
- The system shall display available renewal options.
- The system shall update the membership expiration date after successful payment confirmation.
- The system shall display a renewal confirmation or failure message.

### FR-M06: View Payment History

**Priority:** Medium

**Acceptance Criteria:**
- The system shall display the member's previous payment records.
- Each payment record shall include the payment date, amount, and payment status.
- Members shall only be able to view their own payment history.

### FR-M07: Update Personal Information

**Priority:** Medium

**Acceptance Criteria:**
- The system shall allow members to update their permitted personal information.
- The system shall validate updated information before saving it.
- The system shall save valid changes.
- The system shall display a confirmation message after successful updates.
