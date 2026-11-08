# Domain Requirements

## 1. Purpose
This section describes the accounting and banking assumptions that should govern how ReconIQ interprets transactions. It distinguishes standard accounting principles from project-specific assumptions and proposed rules.

## 2. Domain Rules

### DR-001: Bank statement records represent external ledger activity
- Accounting principle: A bank statement records transactions processed by the financial institution, including deposits, withdrawals, fees, and transfers.
- ReconIQ application: The system should treat bank records as the external source of truth for transaction activity at the bank account level.
- Example: A bank statement includes a debit of KES 12,500 for a supplier payment and a credit of KES 50,000 for a customer deposit.
- Expected system behaviour: Imported bank entries are loaded as external transactions and compared to internal cashbook records.

### DR-002: Cashbook records represent internal accounting activity
- Accounting principle: The cashbook is the entity’s internal record of receipts and payments.
- ReconIQ application: The system should treat the cashbook as the internal ledger from which reconciliations are derived.
- Example: A cashbook entry records a supplier payment of KES 12,500 and a sales receipt of KES 50,000.
- Expected system behaviour: Cashbook transactions are compared directly with bank statement entries for the same period.

### DR-003: Debit and credit representation must be consistent
- Accounting principle: Debits and credits are opposite balance effects depending on the account type; the same transaction can appear with a different sign in different systems.
- ReconIQ application: ReconIQ should convert both bank and cashbook amounts into a common signed format before matching.
- Example: A bank debit appears as -12,500 while the cashbook payment is recorded as 12,500 in one source and -12,500 in another.
- Expected system behaviour: All comparisons are performed on a standardised signed amount convention.

### DR-004: Signed amounts are required for reconciliation
- Accounting principle: Reconciliation logic should compare equivalent transaction values using sign-aware arithmetic rather than raw presentation values.
- ReconIQ application: The system should convert values to a signed representation, preserving direction of movement.
- Example: A deposit of KES 80,000 is represented as +80,000; a withdrawal of KES 30,000 is represented as -30,000.
- Expected system behaviour: A matching algorithm compares amounts using sign-adjusted, normalised values.

### DR-005: Transaction date and value date may differ
- Accounting principle: A transaction may be posted on a different date from the value date used by the bank.
- ReconIQ application: ReconIQ should compare transaction dates and value dates where available, while allowing a defined tolerance for timing differences.
- Example: A cashbook entry is dated 10 June but the bank statement shows the same transaction on 12 June.
- Expected system behaviour: Transactions within a configured date window may still match if all other data aligns.

### DR-006: Reference numbers are the main linkage keys
- Accounting principle: Accounting records often use references such as invoice numbers, cheque numbers, bank transfer references, or customer identifiers.
- ReconIQ application: ReconIQ should prioritise reference matching when references are present and valid.
- Example: Bank statement reference “INV-1042” matches the cashbook reference “INV-1042”.
- Expected system behaviour: Transactions with consistent references are matched before fallback estimation rules.

### DR-007: Matching tolerances are needed for practical reconciliation
- Accounting principle: Real-world finance data rarely aligns perfectly; minor date or amount variations may still indicate the same transaction.
- ReconIQ application: ReconIQ should support user-defined matching tolerances for amount variance and date variation.
- Example: A payment is recorded as KES 49,950 in the bank and KES 50,000 in the cashbook; a tolerance of KES 100 allows a match.
- Expected system behaviour: Matching rules report a match only when the variance falls within the configured tolerance.

### DR-008: Duplicate identification should prevent repeated matches
- Accounting principle: Duplicate transactions should not be counted as separate valid reconciling items.
- ReconIQ application: ReconIQ should detect duplicates based on repeated reference numbers, date/amount combinations, or source identifiers.
- Example: Two identical bank entries exist for the same supplier payment on the same amount and date.
- Expected system behaviour: Duplicate items are flagged and excluded from valid matching or labelled for manual review.

### DR-009: Unmatched transactions require review
- Accounting principle: A reconciliation is not complete when one side contains transactions without corresponding items on the other side.
- ReconIQ application: ReconIQ should classify unresolved items as unmatched bank items or unmatched cashbook items.
- Example: A bank deposit appears without an equivalent cashbook entry.
- Expected system behaviour: The reconciliation output separates unmatched items to enable follow-up review.

### DR-010: Amount discrepancies indicate possible errors or timing issues
- Accounting principle: When the same transaction appears on both sides but with different amounts, the variance may represent a fee, posting delay, or error.
- ReconIQ application: ReconIQ should flag amount mismatches separately from unmatched items.
- Example: Bank statement shows KES 15,000 while cashbook records KES 14,800.
- Expected system behaviour: The transaction is not treated as a clean match; it is classified as an amount discrepancy requiring review.

### DR-011: Reconciliation status should be explicit
- Accounting principle: A transaction or reconciliation run should be clearly marked as matched, unmatched, duplicate, discrepancy, or pending review.
- ReconIQ application: ReconIQ should assign a reconciliation status to each transaction record.
- Example: A transaction is marked “Matched”, “Unmatched”, or “Amount Difference”.
- Expected system behaviour: The final output allows users to review the state of each item and the overall run.

### DR-012: Auditability and traceability are required
- Accounting principle: Reconciliation evidence must be reviewable by internal or external auditors.
- ReconIQ application: ReconIQ should store linkage information, result categories, and run metadata so that each classification is traceable.
- Example: A transaction may be linked to a reconciliation run dated 15 June 2026 and identified as a duplicate by rule ID DUP-01.
- Expected system behaviour: Each result can be traced to the source file, transaction, and decision logic used.

## 3. Project-specific assumptions
The following assumptions are project-specific and should be treated as proposed rules, not universal accounting doctrine:
- A single-file, CSV/Excel-based workflow is sufficient for the MVP.
- Matching will primarily use reference, amount, and date checks.
- The project focuses on reconciliation review rather than automatic posting of correcting journal entries.
- Exact matching is preferred but not solely sufficient; tolerance-based matching may support practical use.
