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

## 5. Data Requirements

### 5.1 Member Information

The system shall store the following information for each gym member:

- Member ID: A unique identifier assigned to each member.
- Full Name: The member's first and last name.
- Gender: The member's gender.
- Date of Birth: The member's birth date.
- Phone Number: The member's contact number.
- Email Address: The member's email address.
- Address: The member's residential address.
- Username: A unique username for login.
- Password: A securely stored password for account authentication.

### 5.2 Membership Information

The system shall maintain the following membership details:

- Membership ID: A unique identifier for each membership.
- Member ID: Identifies the member associated with the membership.
- Membership Plan: The selected membership plan.
- Start Date: The date the membership begins.
- Expiration Date: The date the membership ends.
- Membership Status: Indicates whether the membership is active, expired, or pending.

### 5.3 Payment Information

The system shall store the following payment details:

- Payment ID: A unique identifier for each payment.
- Member ID: Identifies the member who made the payment.
- Amount: The amount paid by the member.
- Payment Date: The date the payment was made.
- Payment Method: The method used for payment.
- Payment Status: Indicates whether the payment is successful, pending, or failed.

### 5.4 Data Validation and Constraints

- Each member shall have a unique Member ID.
- Each member shall have a unique username.
- Required member information shall not be left empty.
- Email addresses shall follow a valid email format.
- Phone numbers shall follow the accepted phone number format.
- Payment amounts shall be greater than zero.
- Each payment shall be associated with a valid member.
- Passwords shall be stored securely and shall not be stored as plain text.

### 5.5 Data Access and Security

- Members shall only be allowed to view their own personal information.
- Members shall only be allowed to view their own membership details.
- Members shall only be allowed to view their own payment history.
- The system shall protect member information from unauthorized access.
- Only authorized users shall be allowed to modify membership and payment records.

## 6. Non-Functional Requirements

### 6.1 Performance Requirements

- NFR-M01: The system shall display member information within 3 seconds under normal operating conditions.
- NFR-M02: The system shall process member login requests within 3 seconds.
- NFR-M03: The system shall display membership plans and payment history without unnecessary delays.

### 6.2 Security Requirements

- NFR-M04: The system shall protect member accounts through secure authentication.
- NFR-M05: The system shall store passwords using secure password hashing.
- NFR-M06: The system shall prevent members from accessing other members' personal and payment information.
- NFR-M07: The system shall protect member information against unauthorized access.

### 6.3 Usability Requirements

- NFR-M08: The system shall provide a simple and user-friendly interface.
- NFR-M09: The system shall provide clear messages for successful and unsuccessful operations.
- NFR-M10: The system shall allow members to navigate between available functions easily.

### 6.4 Reliability and Availability Requirements

- NFR-M11: The system shall maintain accurate member, membership, and payment records.
- NFR-M12: The system shall prevent data loss during normal system operations.
- NFR-M13: The system shall be available to members during the gym's operating hours, except during scheduled maintenance.

### 6.5 Compatibility Requirements

- NFR-M14: The system shall work with commonly used modern web browsers.
- NFR-M15: The system shall provide an interface that can be accessed on desktop computers, tablets, and smartphones.
