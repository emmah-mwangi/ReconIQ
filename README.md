# ReconIQ — Bank Reconciliation Information System

> A student-built accounting information system that compares bank statement transactions with cashbook records, identifies differences, and produces a reconciliation report.

**Project type:** Software Engineering / Information Systems Development  
**Domain:** Accounting Information Systems, Financial Operations and Audit Analytics  
**Status:** Planning and initial development  
**Currency for sample data:** KES

## 1. Project description

ReconIQ is a lightweight Bank Reconciliation Information System designed to help a business compare transactions recorded in its bank statement with transactions recorded in its cashbook.

Users import or enter the two sets of records. ReconIQ validates the data, applies defined matching rules, identifies matched and unmatched transactions, flags possible duplicates and amount differences, and produces a reconciliation summary.

The first release focuses on a clear, explainable reconciliation workflow. It uses sample CSV/Excel files and synthetic data; it does not connect to a live bank or process real payments.

## 2. Problem statement

Businesses may record the same financial activity in two places: the bank statement and the cashbook. Differences can arise because of timing, omissions, duplicate entries, data-entry mistakes, bank charges or other adjustments.

Comparing the records manually can be repetitive and may make it difficult to identify and track exceptions consistently. A business therefore needs a simple system that can compare the records using documented rules, highlight items requiring investigation, and present the results in an understandable report.

ReconIQ addresses this problem by automating the initial comparison and exception-identification process. It supports, but does not replace, the accountant's review and approval.

## 3. Project objectives

### Main objective

To design and develop a Bank Reconciliation Information System that assists users in comparing bank statement records with cashbook records and identifying reconciliation differences.

### Specific objectives

1. Allow users to import bank statement and cashbook data from CSV files, with Excel support where practical.
2. Validate imported data for missing fields, invalid dates, invalid amounts and duplicate identifiers.
3. Standardise key fields such as dates, references and transaction descriptions.
4. Match transactions using transparent, documented matching rules.
5. Identify unmatched records, possible duplicates and amount differences.
6. Generate a reconciliation summary and an exception report.
7. Store reconciliation runs and their results so they can be reviewed later.
8. Test the system using synthetic datasets containing both normal and deliberately problematic records.

## 4. Target users

- **Accountants / bookkeepers:** import records, run reconciliations and investigate differences.
- **Finance officers:** review reconciliation summaries and outstanding items.
- **Internal auditors:** inspect exception records and supporting reconciliation history.
- **System administrator:** manage access and basic system configuration in a future release.

For the initial prototype, the primary user is an accountant or bookkeeper. Auditor and administrator functionality may be represented in the design before full implementation.

## 5. Scope and limitations

### In scope for the first release (MVP)

- Import bank statement and cashbook CSV files.
- Validate required columns and data types.
- Standardise dates, references and amounts.
- Match records using exact reference and amount, with date checks.
- Prevent one record from being matched more than once.
- Classify unmatched bank and cashbook entries.
- Flag possible duplicate records and amount mismatches.
- Display and export a reconciliation summary and exception report.
- Retain a basic history of reconciliation runs.

### Out of scope for the first release

- Direct integration with commercial banks, M-Pesa or payment gateways.
- Initiating, receiving or transferring money.
- Full general ledger or enterprise resource planning functionality.
- Automatic posting of adjusting journal entries.
- Tax filing, eTIMS certification or regulatory submissions.
- Machine-learning fraud detection.
- Multi-company cloud hosting and production-grade deployment.

### Important limitations

- The system is a student prototype and is not intended for production accounting.
- It uses synthetic or appropriately anonymised sample data only.
- A flagged transaction is an exception for investigation, not proof of fraud or error.
- Timing differences may be legitimate. Users must review items such as deposits in transit and outstanding payments.
- Initial matching rules may not identify every valid match, especially where references are missing or transactions are split or grouped.
- The system's output depends on the accuracy and consistency of the source files.

## 6. Proposed technology stack

| Technology | Intended use |
|---|---|
| Python | Core application and reconciliation logic |
| Pandas | Importing, cleaning and analysing tabular data |
| Streamlit | Lightweight user interface for uploading files and viewing results |
| SQLite | Local database for reconciliation runs and results in the MVP |
| SQL | Database queries and reporting |
| Excel / CSV | Source transaction files and sample datasets |
| Power Query | Optional data preparation and comparison prototype |
| Power BI | Optional visual dashboard using exported results |
| pytest | Automated tests for validation and matching rules |
| Git and GitHub | Version control, collaboration and project documentation |
| Draw.io | UML diagrams, process maps and architecture diagrams |
| Figma | Optional interface wireframes |

**Implementation note:** Start with CSV, Python, Pandas and SQLite. Add Streamlit once the reconciliation logic works. Power BI and Power Query are supporting tools, not prerequisites for the first working version. PostgreSQL can replace SQLite if the project later requires a server-based database.

## 7. Expected system features

| ID | Feature | Expected behaviour |
|---|---|---|
| F01 | Import files | Accept bank statement and cashbook CSV files. |
| F02 | Validate data | Check required columns, dates, amounts and identifiers. |
| F03 | Standardise records | Normalise dates, references and amount formats. |
| F04 | Match transactions | Match eligible records using documented rules. |
| F05 | Identify unmatched items | List bank-only and cashbook-only records. |
| F06 | Identify exceptions | Flag possible duplicates, missing references and amount differences. |
| F07 | Reconciliation summary | Show totals and counts for matched and unmatched records. |
| F08 | Export reports | Export results and exceptions to CSV or Excel. |
| F09 | Reconciliation history | Store a basic record of each reconciliation run. |

### Initial matching rules

1. **Exact match:** reference and amount agree, with transaction date within the configured date tolerance.
2. **Reference mismatch or missing reference:** send the item for review rather than forcing a match.
3. **Amount mismatch:** flag the records as an exception.
4. **Potential duplicate:** flag repeated source identifiers or repeated transaction combinations for review.
5. **Unmatched:** classify records that have no eligible counterpart.

The matching engine must not match a source transaction more than once. The initial release will favour explainable matching over speculative fuzzy matching.

### Amount convention

Before matching, both files must be converted to a common signed-amount convention. For example, money received can be positive and money paid out negative. The import process must document how each source's debit and credit columns are converted. This avoids treating opposite bank and cashbook sign conventions as genuine differences.

## 8. Development methodology

ReconIQ will use an **Agile, iterative approach**, adapted for an individual student project.

The work will be divided into small increments. Each increment will produce a reviewable deliverable, and requirements or implementation details can be refined after testing.

| Iteration | Focus | Deliverable |
|---|---|---|
| 1 | Planning and analysis | Project charter, stakeholder analysis, requirements and process maps |
| 2 | System design | UML diagrams, data dictionary and database design |
| 3 | Data import and validation | Working CSV import and validation functions |
| 4 | Reconciliation engine | Tested matching and exception-classification logic |
| 5 | Interface and storage | Streamlit interface and SQLite persistence |
| 6 | Reporting and quality assurance | Exportable reports, test results and final documentation |

Progress will be tracked using GitHub Issues or a project board. Changes will be committed regularly, and each major feature will have test cases.

## 9. Proposed repository structure

```text
reconiq-bank-reconciliation/
├── README.md
├── .gitignore
├── requirements.txt
├── docs/
│   ├── project-charter.md
│   ├── business-case.md
│   ├── stakeholder-analysis.md
│   ├── requirements/
│   │   ├── functional-requirements.md
│   │   ├── non-functional-requirements.md
│   │   └── domain-requirements.md
│   ├── process-maps/
│   ├── uml/
│   │   ├── use-case-diagram/
│   │   ├── sequence-diagram/
│   │   └── interaction-diagram/
│   ├── architecture/
│   └── test-plan.md
├── data/
│   ├── sample/
│   └── processed/
├── src/
│   └── reconiq/
│       ├── __init__.py
│       ├── import_data.py
│       ├── validation.py
│       ├── cleaning.py
│       ├── matching.py
│       ├── exceptions.py
│       ├── reporting.py
│       └── database.py
├── app/
│   └── streamlit_app.py
├── database/
│   └── schema.sql
├── tests/
│   ├── test_import_data.py
│   ├── test_validation.py
│   ├── test_matching.py
│   └── test_exceptions.py
├── power-bi/
├── reports/
│   ├── reconciliation/
│   └── case-study/
└── screenshots/
```

## 10. Getting started

### Prerequisites

- Python 3.11 or later
- Git
- A code editor such as Visual Studio Code

### Setup

```bash
# Clone the repository
git clone <your-github-repository-url>

# Move into the project folder
cd reconiq-bank-reconciliation

# Create a virtual environment
python -m venv .venv

# Activate it on Windows
.venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Run the application

The application entry point will be added when the first working interface is implemented.

```bash
streamlit run app/streamlit_app.py
```

### Run tests

```bash
pytest
```

## 11. Success criteria

The MVP will be considered successful when:

- It imports valid bank and cashbook sample files.
- It rejects or clearly reports invalid input.
- It matches eligible transactions according to documented rules.
- It does not match a transaction more than once.
- It identifies unmatched and exception records correctly against the test dataset.
- It produces a reconciliation summary and exportable exception report.
- Automated tests cover the main matching and validation scenarios.
- The project documentation and UML diagrams are consistent with the implemented system.

## 12. Disclaimer

ReconIQ is an educational software engineering and accounting systems project. It is not a certified accounting product, audit tool or banking integration. All demonstration data should be synthetic or properly anonymised. Reconciliation findings require review by an appropriately authorised person.
