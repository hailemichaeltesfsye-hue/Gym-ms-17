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

## 7. Detailed Use Case Specifications

### 7.1 Use Case: Register Account

**Use Case ID:** UC-M01  
**Primary Actor:** Gym Member  
**Description:** Allows a new gym member to create an account in the system.

**Preconditions:**
- The member does not already have an account.
- The system is available.

**Main Flow:**
1. The member opens the registration page.
2. The system displays the registration form.
3. The member enters the required personal information.
4. The member submits the registration form.
5. The system validates the submitted information.
6. The system checks whether the username and email are already registered.
7. The system creates the member account.
8. The system displays a successful registration message.

**Alternative Flows:**
- If required information is missing, the system asks the member to complete it.
- If the submitted information is invalid, the system displays an appropriate error message.
- If the username or email already exists, the system asks the member to use different information.

**Postconditions:**
- A new member account is created successfully.
- The member's information is stored in the system.

### 7.2 Use Case: Login

**Use Case ID:** UC-M02  
**Primary Actor:** Gym Member  
**Description:** Allows registered members to access their accounts.

**Preconditions:**
- The member has a registered account.
- The system is available.

**Main Flow:**
1. The member opens the login page.
2. The system displays the login form.
3. The member enters their username and password.
4. The member submits the login form.
5. The system verifies the login credentials.
6. The system grants access to the member's account.
7. The system displays the member dashboard.

**Alternative Flows:**
- If the username or password is incorrect, the system displays an error message.
- If required login information is missing, the system asks the member to enter it.

**Postconditions:**
- The member is successfully logged in.
- The member can access authorized account functions.

### 7.3 Use Case: Logout

**Use Case ID:** UC-M03  
**Primary Actor:** Gym Member  
**Description:** Allows a logged-in member to securely end their session.

**Preconditions:**
- The member is logged in.

**Main Flow:**
1. The member selects the logout option.
2. The system ends the member's session.
3. The system redirects the member to the login page.
4. The system displays a logout confirmation message.

**Alternative Flows:**
- If the session has already expired, the system redirects the member to the login page.

**Postconditions:**
- The member's session is ended.
- The member must log in again to access protected functions.

### 7.4 Use Case: View Membership Plans

**Use Case ID:** UC-M04  
**Primary Actor:** Gym Member  
**Description:** Allows members to view available gym membership plans.

**Preconditions:**
- The system is available.
- Membership plans have been added to the system.

**Main Flow:**
1. The member opens the membership plans page.
2. The system retrieves available membership plans.
3. The system displays the available plans.
4. The member selects a plan to view its details.
5. The system displays the selected plan's name, duration, and price.

**Alternative Flows:**
- If no membership plans are available, the system displays an appropriate message.

**Postconditions:**
- The member has viewed the available membership plans.
- No membership information is changed.

### 7.5 Use Case: View Membership Status

**Use Case ID:** UC-M05  
**Primary Actor:** Gym Member  
**Description:** Allows members to check their current membership information and status.

**Preconditions:**
- The member is logged in.
- The member has membership information recorded in the system.

**Main Flow:**
1. The member opens the membership status page.
2. The system retrieves the member's membership information.
3. The system displays the membership plan.
4. The system displays the membership start date and expiration date.
5. The system displays the current membership status.

**Alternative Flows:**
- If the member has no membership record, the system displays a message indicating that no membership is available.

**Postconditions:**
- The member has viewed their membership information.
- No membership information is changed.

### 7.6 Use Case: Renew Membership

**Use Case ID:** UC-M06  
**Primary Actor:** Gym Member  
**Description:** Allows members to request renewal of their gym membership.

**Preconditions:**
- The member is logged in.
- The member has an existing membership record.
- Renewal options are available.

**Main Flow:**
1. The member opens the membership renewal page.
2. The system displays available renewal options.
3. The member selects a renewal option.
4. The system displays the renewal details and required payment amount.
5. The member confirms the renewal request.
6. The system processes or records the payment request.
7. The payment is confirmed successfully.
8. The system updates the membership expiration date.
9. The system displays a renewal confirmation message.

**Alternative Flows:**
- If no renewal options are available, the system displays an appropriate message.
- If payment fails, the system displays a payment failure message.
- If payment is pending, the system keeps the renewal request pending until confirmation.

**Postconditions:**
- After successful payment confirmation, the membership expiration date is updated.
- The renewal payment record is stored in the system.

### 7.7 Use Case: View Payment History

**Use Case ID:** UC-M07  
**Primary Actor:** Gym Member  
**Description:** Allows members to view their previous payment records.

**Preconditions:**
- The member is logged in.

**Main Flow:**
1. The member opens the payment history page.
2. The system retrieves the member's payment records.
3. The system displays the available payment records.
4. Each record displays the payment date, amount, payment method, and payment status.
5. The member reviews their payment history.

**Alternative Flows:**
- If no payment records are available, the system displays a message indicating that no payment history exists.

**Postconditions:**
- The member has viewed their payment history.
- Payment records remain unchanged.

### 7.8 Use Case: Update Profile

**Use Case ID:** UC-M08  
**Primary Actor:** Gym Member  
**Description:** Allows members to update their personal information.

**Preconditions:**
- The member is logged in.
- The member has an existing account.

**Main Flow:**
1. The member opens the profile page.
2. The system displays the member's current personal information.
3. The member selects the information they want to update.
4. The member enters the new information.
5. The member submits the changes.
6. The system validates the updated information.
7. The system saves the valid changes.
8. The system displays a successful update message.

**Alternative Flows:**
- If the submitted information is invalid, the system displays an error message.
- If required information is missing, the system asks the member to complete it.

**Postconditions:**
- The member's valid updated information is saved.
- The updated information is available in the member's profile.
