# Gym Cashier Requirements

## 1. Actor Description

**Actor Name:** Cashier

**Description:** A front-desk financial staff member responsible for managing point-of-sale (POS) billing, collecting membership and walk-in payments, issuing official receipts, handling authorized refunds or adjustments, and reconciling daily cash drawer transactions within the Gym Management System.

**Role Scope & Access Boundaries:**
- **Authorized Access:** Point-of-Sale (POS) checkout, member billing lookup, membership payment processing, receipt generation, refund requests, transaction history search, and shift reconciliation reports.
- **Restricted Access:** Cannot alter system-wide pricing rules, delete historical financial audit logs, modify member workout/trainer records, or manage system administrator accounts.

---

## 2. Functional Requirements

### 2.1 Authentication & Shift Management
- **FR-C01 (Cashier Authentication):** The system shall allow cashiers to securely log in and log out using role-based credentials so that all financial actions are tied to an accountable staff account.
- **FR-C02 (Open & Close Cash Shift):** The system shall allow the cashier to open a register shift by recording an opening cash float and close the shift by entering the physical end-of-shift cash count.

### 2.2 Member Lookup & Pricing Verification
- **FR-C03 (Member Billing Lookup):** The system shall allow the cashier to search for gym members by Member ID, full name, phone number, or membership card/QR code to verify identity, current membership status, expiration date, and any outstanding balance.
- **FR-C04 (View Plans & Fee Schedule):** The system shall allow the cashier to view active membership plans, renewal rates, walk-in day-pass fees, add-on services (e.g., locker rental, personal training sessions), applicable taxes, and eligible promotions.

### 2.3 Payment Processing & Checkout
- **FR-C05 (Process Membership Registration & Renewal Payments):** The system shall allow the cashier to collect payment for new member registrations, plan upgrades, and membership renewals, automatically updating the member's status to active and extending their validity period upon confirmation.
- **FR-C06 (Multi-Method & Split Payment Support):** The system shall support multiple payment methods—including cash, debit/credit card, bank transfer, and mobile/QR payments—as well as split payments across methods, automatically calculating change due for cash transactions.
- **FR-C07 (Process Walk-In & Add-On Sales):** The system shall allow the cashier to bill non-members for single-day passes and bill members for ancillary services or gym merchandise without requiring a full membership subscription.
- **FR-C08 (Apply Discounts & Promo Codes):** The system shall allow the cashier to apply valid promotional codes or pre-approved concession discounts during checkout while recording the discount reason and cashier ID.

### 2.4 Receipts, Refunds & Adjustments
- **FR-C09 (Generate & Issue Official Receipts):** The system shall automatically generate an itemized receipt (printable and digital) for every completed payment, displaying the unique transaction ID, date and time, cashier name, member details, itemized charges, tax, discount, payment method, and total paid.
- **FR-C10 (Process Voids & Refunds):** The system shall allow the cashier to void an erroneous same-shift transaction or process an eligible refund with a mandatory reason code, enforcing supervisor/manager approval when the refund exceeds the cashier's authorization threshold.

### 2.5 Transaction Tracking & Daily Reconciliation
- **FR-C11 (Search & Review Transaction History):** The system shall allow the cashier to view and filter transactions by date, shift ID, receipt number, member name, payment method, or transaction status (Paid, Refunded, Voided).
- **FR-C12 (Generate Shift & End-of-Day Reconciliation Report):** The system shall generate a shift summary report comparing expected totals per payment method (opening float + cash sales - cash refunds) against the cashier's counted cash, highlighting any surplus or shortage variance.

---

## 3. Non-Functional Requirements

- **NFR-C01 (Financial Auditability & Integrity):** All payment, discount, void, and refund records shall be immutable once finalized and logged with a timestamp and cashier ID to prevent unauthorized tampering.
- **NFR-C02 (Checkout Performance):** Payment recording and receipt generation shall complete within 3 seconds under standard operating conditions and prevent duplicate charges if a submission is retried.
- **NFR-C03 (Usability):** The POS screen shall provide clear validation feedback, keyboard/barcode-scanner compatibility, and minimal clicks per transaction to avoid front-desk queues during peak gym hours.

---

## 4. Use Cases

### UC-C01: Cashier Login and Open Shift
- **Description:** The cashier authenticates into the system at the start of their workday and initializes the cash register float.
- **Preconditions:** The cashier has an active staff account and no unclosed shift assigned to their terminal.
- **Main Flow:**
  1. Cashier enters credentials and logs into the system.
  2. Cashier selects **Open Shift** and counts the starting cash drawer float.
  3. Cashier enters the opening amount and confirms.
  4. System records the shift start timestamp and unlocks POS billing features.

### UC-C02: Verify Member and Check Billing Status
- **Description:** The cashier looks up a member at the front desk to check membership validity or pending dues.
- **Preconditions:** Cashier is logged in with an active shift.
- **Main Flow:**
  1. Cashier searches using Member ID, phone number, name, or card scan.
  2. System displays the member's profile, active/expired status, plan expiry date, and unpaid invoices.
  3. Cashier informs the member of their status or proceeds to payment collection.

### UC-C03: Process Membership Registration or Renewal Payment
- **Description:** The cashier collects payment for a new membership or renews an existing member's plan.
- **Preconditions:** Member profile exists (or is created at registration) and a membership plan is selected.
- **Main Flow:**
  1. Cashier selects the chosen membership plan and billing duration.
  2. Cashier applies any valid promo code or approved discount.
  3. System calculates the subtotal, tax, and final payable amount.
  4. Cashier selects the payment method(s) and records the tendered amount.
  5. System confirms the payment, calculates change (if cash), activates/extends the membership, and triggers receipt generation.

### UC-C04: Process Walk-In Day Pass or Add-On Payment
- **Description:** The cashier bills a guest for a single-day gym pass or bills a member for an add-on item/service.
- **Preconditions:** Cashier has an open shift.
- **Main Flow:**
  1. Cashier selects **Walk-In Pass** or the requested add-on item/service.
  2. Cashier enters guest name/contact (for day passes) or links the member profile.
  3. Cashier collects payment via the selected payment method.
  4. System logs the sale and generates a payment receipt.

### UC-C05: Issue Official Payment Receipt
- **Description:** The cashier prints or sends an itemized proof of payment to the customer.
- **Preconditions:** A payment transaction has been completed and confirmed by the system.
- **Main Flow:**
  1. System compiles all transaction line items, taxes, payment methods, and reference ID.
  2. Cashier chooses to print a physical copy, email/SMS a digital receipt, or reprint an existing receipt from history.
  3. System records the receipt issuance event.

### UC-C06: Process Refund or Void Transaction
- **Description:** The cashier reverses an erroneous charge or refunds an eligible payment according to gym policy.
- **Preconditions:** Original transaction exists in the system; manager approval is provided if required by policy.
- **Main Flow:**
  1. Cashier locates the original transaction using the receipt number or member profile.
  2. Cashier selects **Void** (if same-shift error) or **Refund** and enters the reason.
  3. If the amount exceeds the cashier limit, a supervisor enters authorization credentials.
  4. System updates the transaction status, adjusts expected drawer/payment totals, updates membership status if applicable, and issues a refund/void slip.

### UC-C07: Close Shift and Generate Daily Reconciliation Report
- **Description:** At the end of the shift, the cashier reconciles physical cash and digital payments against system totals.
- **Preconditions:** Cashier has an open shift with recorded transactions.
- **Main Flow:**
  1. Cashier selects **Close Shift** and counts the physical cash in the drawer.
  2. Cashier enters the counted cash amount into the system.
  3. System compares counted cash against expected cash (`Opening Float + Cash Payments - Cash Refunds`) and totals card/transfer/mobile payments.
  4. System generates the end-of-shift reconciliation report noting any variance and locks the shift.

---

## 5. Business Rules & Constraints

1. **Shift Accountability:** No payment or refund transaction can be processed unless the cashier has an active, open shift.
2. **Receipt Immutability:** Finalized transactions cannot be edited or deleted; corrections must be handled strictly through logged void or refund workflows.
3. **Instant Membership Synchronization:** A membership activation or renewal takes effect immediately upon payment confirmation.
4. **Dual-Control Refunds:** Refunds above the configured threshold require manager/admin authorization before the system executes the reversal.
