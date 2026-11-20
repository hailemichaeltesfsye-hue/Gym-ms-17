# Gym Management System — Cashier Requirements Specification

| Metadata | Details |
| :--- | :--- |
| **System Name** | Gym Management System (`Gym-ms-17`) |
| **Document Scope** | Actor Requirements Specification (ARS) |
| **Primary Actor** | Cashier (Front-Desk Billing & Point-of-Sale Staff) |
| **Related Actors** | Gym Member, Gym Manager / Admin, Payment Gateway |
| **Document Version** | 2.0 |

---

## 1. Actor Description & Operational Context

### 1.1 Actor Profile
- **Actor Name:** Cashier
- **Summary:** An authorized front-desk financial staff member responsible for managing Point-of-Sale (POS) checkout, collecting membership and walk-in payments, verifying member billing eligibility, applying approved discounts, issuing official receipts, handling authorized voids/refunds, and performing end-of-shift cash drawer reconciliation.
- **Operational Environment:** Operates primarily on a front-desk workstation or tablet POS connected to a receipt printer, barcode/QR scanner, and card/mobile payment terminal during active gym business hours.

### 1.2 Role-Based Access Control (RBAC) Matrix
To enforce separation of duties and protect financial integrity, the Cashier actor operates under strict role boundaries:

| System Resource | Create | Read | Update | Delete | Operational Constraints |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Cash Shifts** | Yes | Own Shifts | Close Own | No | Cannot modify closed shift totals or another cashier's active shift. |
| **Member Profiles** | Limited | Yes | Billing Contact Only | No | Can view status/balances; cannot alter medical, workout, or trainer logs. |
| **Membership Plans & Pricing** | No | Yes | No | No | Read-only access to active plans, rates, taxes, and promotions. |
| **Invoices & Line Items** | Yes | Yes | Void (Unpaid) | No | Paid invoices are locked; corrections require credit notes/refunds. |
| **Payments & Receipts** | Yes | Yes | No | No | Strictly append-only ledger; receipts can be reprinted with a `COPY` watermark. |
| **Refunds & Voids** | Request / Small | Yes | No | No | High-value refunds or post-shift adjustments require Manager authorization. |
| **Financial Audit Logs** | Auto | Own Shift | No | No | System-generated; completely immutable by all front-desk staff. |

---

## 2. Functional Requirements

### 2.1 Authentication & Cash Shift Management

#### FR-C01: Cashier Authentication & Session Security
- **Priority:** High
- **Description:** The system shall authenticate cashiers using unique staff credentials (username/email and password, with optional PIN/badge quick-lock) and enforce role-based access control to POS and billing modules.
- **Acceptance Criteria:**
  - Invalid credentials return a generic authentication error without exposing account existence.
  - Inactive POS sessions automatically lock after a configurable idle timeout (e.g., 10 minutes) without losing an open shift state.
  - Every login, logout, and terminal lock event is recorded with timestamp and terminal ID.

#### FR-C02: Open Cash Shift (Register Initialization)
- **Priority:** High
- **Description:** The system shall require the cashier to open a cash shift by recording the starting cash drawer float before processing any financial transactions.
- **Acceptance Criteria:**
  - The system blocks payment collection if the cashier does not have an active, open shift.
  - Opening float must be a non-negative numeric amount (`>= 0.00`).
  - Only one active shift may be open per cashier terminal at any given time.

#### FR-C03: Cash Drawer Drop & Pay-In / Pay-Out Logging
- **Priority:** Medium
- **Description:** The system shall allow the cashier to record mid-shift cash movements (such as safe drops for excess cash or petty-cash payouts) with a mandatory reason note.
- **Acceptance Criteria:**
  - Safe drops and payouts adjust the expected drawer cash balance in real time.
  - Pay-outs exceeding configured cash-on-hand or petty-cash limits require supervisor approval.

#### FR-C04: Close Cash Shift & Count Verification
- **Priority:** High
- **Description:** The system shall allow the cashier to close an active shift by entering the counted physical cash breakdown (denominations or total) and reviewing non-cash settlement totals.
- **Acceptance Criteria:**
  - The system calculates expected cash as: `Opening Float + Cash Sales + Cash Pay-Ins - Cash Refunds - Cash Pay-Outs`.
  - Any discrepancy (`Counted Cash - Expected Cash != 0`) highlights a surplus or shortage variance and requires a mandatory explanation note before finalizing shift closure.
  - Once closed, the shift is locked and cannot accept new transactions.

---

### 2.2 Member Lookup, Billing Status & Catalog Access

#### FR-C05: Fast Member Lookup & Verification
- **Priority:** High
- **Description:** The system shall allow the cashier to search and retrieve member accounts in real time using Member ID, full name, phone number, email, or barcode/QR membership scan.
- **Acceptance Criteria:**
  - Displays member identity photo (if available), membership status (`Active`, `Expired`, `Suspended`, `Pending Payment`), current plan name, expiration date, and any overdue balance.
  - Partial text search returns matching candidates sorted by relevance within 2 seconds.

#### FR-C06: View Active Membership Plans, Add-Ons & Pricing Rules
- **Priority:** High
- **Description:** The system shall display all currently active membership tiers, duration options, walk-in day-pass fees, ancillary services (e.g., locker rental, personal training packages), retail items, applicable taxes, and active promotions.
- **Acceptance Criteria:**
  - Inactive or archived plans are hidden from new sales selection.
  - Prices automatically reflect configured tax rates (inclusive or exclusive) based on gym billing settings.

---

### 2.3 Point-of-Sale (POS) Billing & Payment Processing

#### FR-C07: Process New Membership & Renewal Payments
- **Priority:** High
- **Description:** The system shall allow the cashier to create an invoice and collect payment for new member sign-ups, membership renewals, and plan upgrades/downgrades.
- **Acceptance Criteria:**
  - Upon successful full payment confirmation, the system automatically activates a `Pending` membership or extends an `Active`/`Expired` membership's validity end date without manual admin intervention.
  - Prevents accidental double-billing of an already-settled invoice.

#### FR-C08: Process Walk-In Day Passes & Ancillary Sales
- **Priority:** High
- **Description:** The system shall allow the cashier to bill non-member guests for single-day/short-term passes and bill registered members for add-on services or gym merchandise.
- **Acceptance Criteria:**
  - Walk-in pass checkout captures minimal guest identification (full name and phone/ID reference) without requiring full long-term membership onboarding.
  - Generates a time-bound access pass code or QR token upon payment confirmation.

#### FR-C09: Multi-Method & Split Payment Processing
- **Priority:** High
- **Description:** The system shall support payment collection via Cash, Credit/Debit Card, Bank Transfer, Mobile/QR Wallet, or a combination (split payment) across multiple methods in a single transaction.
- **Acceptance Criteria:**
  - For cash payments, tendered amount must be `>=` cash portion due, and the system accurately calculates and displays change due.
  - For split payments, the sum of all tendered payment legs must exactly equal the invoice total before checkout can complete.
  - Card/mobile terminal transactions record an external authorization/reference ID.

#### FR-C10: Apply Promo Codes & Authorized Discounts
- **Priority:** Medium
- **Description:** The system shall allow the cashier to apply valid promotional codes or staff-authorized discounts (percentage or fixed amount) to eligible invoice line items.
- **Acceptance Criteria:**
  - Validates promo code active dates, usage limits, and plan eligibility before applying.
  - Ad-hoc/manual discounts exceeding the cashier's permitted percentage cap require supervisor PIN/credential override.
  - Discount amount, type, and approving staff ID are permanently logged on the invoice.

#### FR-C11: Handle Pending Invoices & Partial Payments (If Enabled by Policy)
- **Priority:** Medium
- **Description:** The system shall allow the cashier to look up unpaid or partially paid member invoices and collect installment/balance settlements.
- **Acceptance Criteria:**
  - Updates remaining balance (`Total Due - Total Paid`) after each installment.
  - Marks the invoice `Paid` only when the remaining balance reaches `0.00`.

---

### 2.4 Receipts, Voids & Refunds

#### FR-C12: Generate, Print & Deliver Official Receipts
- **Priority:** High
- **Description:** The system shall automatically generate a sequentially numbered, itemized receipt upon payment completion, supporting thermal/A4 printing and digital delivery (Email/SMS/PDF).
- **Acceptance Criteria:**
  - Every receipt displays: Gym Name/Address, Receipt Number, Transaction Timestamp, Shift ID, Cashier Name/ID, Customer Name/Member ID, Itemized Line Items, Discounts, Tax Breakdown, Payment Method(s), Amount Tendered, Change Given, and Net Total Paid.
  - Reprinting an existing historical receipt appends a clear `"REPRINT / COPY"` label and logs the reprint event.

#### FR-C13: Void Erroneous Same-Shift Transactions
- **Priority:** High
- **Description:** The system shall allow the cashier to void an unpaid invoice or reverse an erroneous transaction created within the current open shift prior to shift closure, subject to authorization rules.
- **Acceptance Criteria:**
  - Requires selection of a standardized reason code (e.g., *Wrong Plan Selected*, *Duplicate Entry*, *Customer Cancelled Before Completion*) and optional notes.
  - Reverses any associated membership extension triggered by the voided payment.
  - Preserves the voided record in history with status `Voided` (never hard-deleted).

#### FR-C14: Process Refunds & Credit Notes
- **Priority:** High
- **Description:** The system shall allow the cashier to initiate and process full or partial refunds for eligible completed transactions in accordance with gym refund policies.
- **Acceptance Criteria:**
  - Refunds cannot exceed the original net paid amount across all prior refunds on that transaction.
  - Refunds exceeding the cashier's authorization threshold or requested after the original shift has closed require Manager/Admin credential approval.
  - Generates a formal Refund/Credit Receipt linked to the original receipt number and deducts the refunded amount from the appropriate payment method total.

---

### 2.5 Transaction Monitoring & Reporting

#### FR-C15: Search & Inspect Transaction History
- **Priority:** Medium
- **Description:** The system shall provide a searchable transaction log allowing the cashier to filter by date range, shift ID, receipt number, member name/ID, payment method, and status (`Paid`, `Partial`, `Voided`, `Refunded`).
- **Acceptance Criteria:**
  - Selecting any transaction displays full line-item details, payment reference IDs, and linked refund/void records.
  - Results load within 2 seconds for standard shift/daily queries.

#### FR-C16: Generate End-of-Shift & Daily Financial Summary Reports
- **Priority:** High
- **Description:** The system shall generate a structured Shift Reconciliation & Daily Sales Report summarizing gross sales, discounts, voids, refunds, net revenue by category, totals by payment method, and cash drawer variance.
- **Acceptance Criteria:**
  - Report can be viewed on-screen, printed for physical sign-off, or exported as PDF/CSV.
  - Clearly separates Cash, Card, Bank Transfer, and Mobile/QR totals for accounting verification.

---

## 3. Data Requirements (Core Cashier Entities)

To ensure clean database design and API consistency, cashier workflows interact with the following core entities:

| Entity | Key Attributes | Integrity & Validation Rules |
| :--- | :--- | :--- |
| **`CashShift`** | `shiftId`, `cashierId`, `terminalId`, `openedAt`, `closedAt`, `openingFloat`, `expectedCash`, `countedCash`, `variance`, `varianceNote`, `status` (`Open`/`Closed`) | Only one `Open` shift per terminal; `variance = countedCash - expectedCash`. |
| **`Invoice`** | `invoiceId`, `invoiceNumber`, `memberId` (nullable for walk-ins), `guestName`, `shiftId`, `subtotal`, `discountTotal`, `taxTotal`, `grandTotal`, `balanceDue`, `status` | `grandTotal = subtotal - discountTotal + taxTotal`; `balanceDue >= 0`. |
| **`PaymentTransaction`** | `transactionId`, `invoiceId`, `shiftId`, `cashierId`, `paymentMethod`, `amount`, `tenderedAmount`, `changeGiven`, `externalRefNo`, `idempotencyKey`, `paidAt`, `status` | `idempotencyKey` prevents duplicate submissions; immutable once `Completed`. |
| **`Receipt`** | `receiptNumber`, `transactionId`, `issuedAt`, `reprintCount`, `deliveryChannel` (`Print`/`Email`/`SMS`) | `receiptNumber` must be strictly sequential and gap-free. |
| **`RefundOrVoid`** | `adjustmentId`, `originalTransactionId`, `shiftId`, `type` (`Void`/`Refund`), `amount`, `reasonCode`, `notes`, `requestedBy`, `approvedBy`, `createdAt` | `sum(refunds) <= originalTransaction.amount`; requires `approvedBy` above threshold. |

---

## 4. Non-Functional Requirements

- **NFR-C01 (Financial Auditability & Immutability):** All finalized payments, voids, refunds, discounts, and shift closures shall be written to an append-only audit log recording `staffId`, `timestamp`, `terminalId`, and before/after states. Financial records shall never be physically deleted.
- **NFR-C02 (Transaction Atomicity & Idempotency):** Payment confirmation, invoice settlement, receipt generation, and membership activation shall execute within an atomic database transaction. Every payment request shall use an idempotency key so network retries cannot double-charge a customer.
- **NFR-C03 (Performance SLA):** Member lookup and POS item addition shall respond in `< 2.0 seconds`; payment recording and receipt rendering shall complete in `< 3.0 seconds` under normal operating load.
- **NFR-C04 (Usability & Front-Desk Ergonomics):** The POS interface shall support full keyboard navigation, barcode/QR scanner auto-focus, clear inline validation errors, and high-contrast visual indicators for membership status (`Active` = Green, `Expiring Soon` = Amber, `Expired/Overdue` = Red).
- **NFR-C05 (Currency & Precision):** All monetary calculations shall use exact fixed-point decimal arithmetic (2 decimal places, standard half-up rounding) to avoid floating-point discrepancies in drawer reconciliation.
- **NFR-C06 (Data Security):** Card details must never be stored in plain text by the gym system (PCI-compliant tokenization via payment terminal/gateway); cashier sessions must transmit exclusively over HTTPS/TLS.

---

## 5. Detailed Use Case Specifications

### UC-C01: Open Cash Shift
- **Primary Actor:** Cashier
- **Trigger:** Cashier starts their scheduled front-desk shift.
- **Preconditions:** Cashier has valid staff credentials; no `Open` shift exists on the assigned terminal.
- **Main Flow:**
  1. Cashier logs into the POS workstation.
  2. System detects no active shift and prompts **Open Cash Shift**.
  3. Cashier counts the physical starting float in the cash drawer and enters the amount.
  4. Cashier clicks **Confirm Open Shift**.
  5. System creates a `CashShift` record (`status = Open`), logs the timestamp, and unlocks all POS billing modules.
- **Exception Flow:**
  - *E1 (Previous Shift Unclosed):* System alerts the cashier that an unclosed shift exists on the terminal and requires closing or supervisor handover before opening a new shift.
- **Postconditions:** Active shift session is initialized and ready for transactions.

---

### UC-C02: Verify Member Billing Status
- **Primary Actor:** Cashier
- **Trigger:** Member approaches the front desk to inquire about renewal, dues, or plan status.
- **Preconditions:** Cashier is logged in with an `Open` shift.
- **Main Flow:**
  1. Cashier scans the member's card/QR code or searches by name, phone, or Member ID.
  2. System retrieves the member profile, active/expired plan, expiration date, and any open invoices.
  3. Cashier reviews the status and informs the member or selects **Renew / Pay Balance**.
- **Alternative Flow:**
  - *A1 (No Match Found):* System displays "No member found" and offers quick shortcuts to **Register New Member** or **Bill as Walk-In Guest**.
- **Postconditions:** Member billing eligibility is verified.

---

### UC-C03: Process Membership Registration or Renewal Payment
- **Primary Actor:** Cashier
- **Secondary Actors:** Gym Member, Payment Gateway (for card/QR payments)
- **Trigger:** Member requests to pay for a new membership or renew/upgrade an existing plan.
- **Preconditions:** Cashier has an `Open` shift; member record exists (or is newly created).
- **Main Flow:**
  1. Cashier opens POS checkout linked to the member's profile.
  2. Cashier selects the membership plan (e.g., Monthly, Quarterly, Annual) or selects an existing unpaid invoice.
  3. *(Optional)* Cashier enters a promo code or applies an authorized discount; system validates and recalculates totals.
  4. Cashier selects payment method(s) (Cash, Card, Bank Transfer, Mobile/QR, or Split).
  5. For Cash, cashier enters tendered amount; system displays change due. For Card/QR, system awaits terminal/gateway confirmation reference.
  6. Cashier confirms payment.
  7. System atomically records the `PaymentTransaction`, marks the `Invoice` as `Paid`, activates/extends the member's subscription dates, and triggers **UC-C05 (Issue Official Receipt)**.
- **Exception Flows:**
  - *E1 (Insufficient Cash Tendered):* System disables checkout confirmation and highlights the shortfall amount.
  - *E2 (Card/Gateway Declined):* System displays the decline message, keeps the invoice `Unpaid`, and allows retrying or switching to another payment method without duplicating charges.
  - *E3 (Invalid/Expired Promo Code):* System rejects the promo code with a clear reason and preserves original pricing.
- **Postconditions:** Payment is recorded, membership is active/extended, and an official receipt is generated.

---

### UC-C04: Process Walk-In Day Pass & Add-On Sale
- **Primary Actor:** Cashier
- **Trigger:** Non-member guest requests a single-day gym pass, or a member purchases an add-on service/item.
- **Preconditions:** Cashier has an `Open` shift.
- **Main Flow:**
  1. Cashier selects **Walk-In Day Pass** or adds ancillary items/services to the POS cart.
  2. For walk-ins, cashier enters guest name and contact number; for members, cashier links the member profile.
  3. Cashier collects payment via supported payment methods.
  4. System completes checkout, issues an itemized receipt, and (for day passes) outputs a same-day access pass/QR code.
- **Postconditions:** Walk-in/ancillary revenue is logged to the active shift without creating a recurring membership.

---

### UC-C05: Issue & Reprint Official Receipt
- **Primary Actor:** Cashier
- **Trigger:** A payment or refund transaction completes, or a customer requests a duplicate receipt.
- **Preconditions:** A valid `PaymentTransaction` or `RefundOrVoid` record exists.
- **Main Flow:**
  1. System generates a sequential `Receipt` containing full itemized breakdown, taxes, payment method, and cashier/shift identifiers.
  2. Cashier selects **Print Receipt** and/or **Send Digital Receipt (Email/SMS)**.
  3. System dispatches the receipt and records the delivery event.
- **Alternative Flow:**
  - *A1 (Historical Reprint):* Cashier locates an older transaction in history and clicks **Reprint**; system prints the receipt clearly stamped `"REPRINT / COPY"` and increments `reprintCount`.
- **Postconditions:** Customer holds valid proof of payment; receipt issuance is audited.

---

### UC-C06: Process Void or Refund
- **Primary Actor:** Cashier
- **Secondary Actor:** Gym Manager / Supervisor (when approval is required)
- **Trigger:** Duplicate charge, billing error, or approved membership/service cancellation.
- **Preconditions:** Original transaction exists in the system; cashier has an `Open` shift.
- **Main Flow:**
  1. Cashier searches and opens the original transaction by receipt number or member profile.
  2. Cashier selects **Void** (if same-shift error) or **Refund** and specifies the amount and mandatory reason code.
  3. System checks authorization rules: if amount exceeds cashier limit or belongs to a closed shift, system prompts for Supervisor/Manager approval credentials.
  4. Manager enters credentials to authorize (when required).
  5. System records the `RefundOrVoid` entry, updates the original invoice/membership status if applicable, adjusts shift expected cash (if cash refund), and issues a Void/Refund confirmation slip.
- **Exception Flow:**
  - *E1 (Exceeds Remaining Refundable Amount):* System blocks submission if requested refund exceeds `Original Paid Amount - Previously Refunded Amount`.
- **Postconditions:** Financial adjustment is permanently audited and shift totals remain accurate.

---

### UC-C07: Close Shift & End-of-Day Reconciliation
- **Primary Actor:** Cashier
- **Trigger:** End of cashier shift or gym closing time.
- **Preconditions:** Cashier has an `Open` shift; all in-progress checkouts are completed or cancelled.
- **Main Flow:**
  1. Cashier selects **Close Shift**.
  2. System summarizes non-cash totals (Card, Bank Transfer, Mobile/QR) and prompts for physical cash count (blind count or guided denomination count).
  3. Cashier enters counted cash in the drawer.
  4. System computes expected cash and displays variance (`Counted - Expected`).
  5. If variance is non-zero, cashier enters a mandatory variance explanation note.
  6. Cashier confirms closure; system locks the shift (`status = Closed`) and generates the printable/exportable **Shift Reconciliation Report**.
- **Postconditions:** Shift is finalized, drawer accountability is documented, and management can review the daily financial summary.

---

## 6. Business Rules & Constraints

| Rule ID | Rule Name | Specification |
| :--- | :--- | :--- |
| **BR-C01** | **Shift Gatekeeping** | No sale, payment, cash movement, or refund can be executed unless the logged-in cashier has an active `Open` shift. |
| **BR-C02** | **Append-Only Financial Ledger** | Finalized transactions and receipts are immutable; errors may only be corrected via logged voids, refunds, or credit notes. |
| **BR-C03** | **Atomic Membership Sync** | A membership status update or validity extension must only occur upon verified payment completion, and must roll back if a same-shift payment is voided. |
| **BR-C04** | **Dual-Control Thresholds** | Ad-hoc discounts above the cashier cap, cash pay-outs, and refunds above the threshold (or from prior closed shifts) strictly require Manager/Admin authorization. |
| **BR-C05** | **Sequential Receipt Numbering** | Receipt numbers must increment monotonically without gaps (`REC-YYYYMMDD-XXXX`) for audit compliance. |
| **BR-C06** | **Refund Method Integrity** | Refunds should return via the original payment method (or cash/bank transfer per policy) and must never exceed the net original amount paid. |

---

## 7. Requirements Traceability Matrix

| Requirement ID | Requirement Title | Mapped Use Case(s) | Governing Business Rule(s) |
| :--- | :--- | :--- | :--- |
| **FR-C01** | Cashier Authentication & Session Security | UC-C01 | BR-C01 |
| **FR-C02** | Open Cash Shift | UC-C01 | BR-C01 |
| **FR-C03** | Cash Drawer Drop & Pay-In/Pay-Out | UC-C07 | BR-C01, BR-C04 |
| **FR-C04** | Close Cash Shift & Count Verification | UC-C07 | BR-C01, BR-C02 |
| **FR-C05** | Fast Member Lookup & Verification | UC-C02 | — |
| **FR-C06** | View Active Plans, Add-Ons & Pricing | UC-C03, UC-C04 | — |
| **FR-C07** | Process Membership & Renewal Payments | UC-C03 | BR-C01, BR-C03 |
| **FR-C08** | Process Walk-In & Ancillary Sales | UC-C04 | BR-C01 |
| **FR-C09** | Multi-Method & Split Payment Processing | UC-C03, UC-C04 | BR-C02 |
| **FR-C10** | Apply Promo Codes & Discounts | UC-C03 | BR-C04 |
| **FR-C11** | Pending Invoices & Partial Payments | UC-C03 | BR-C02, BR-C03 |
| **FR-C12** | Generate, Print & Deliver Receipts | UC-C05 | BR-C02, BR-C05 |
| **FR-C13** | Void Erroneous Same-Shift Transactions | UC-C06 | BR-C02, BR-C03, BR-C04 |
| **FR-C14** | Process Refunds & Credit Notes | UC-C06 | BR-C02, BR-C04, BR-C06 |
| **FR-C15** | Search & Inspect Transaction History | UC-C05, UC-C06 | BR-C02 |
| **FR-C16** | End-of-Shift & Daily Summary Reports | UC-C07 | BR-C02 |
