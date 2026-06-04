# AP Invoice Automation with Azure Document Intelligence - UiPath

An enterprise-grade Accounts Payable Invoice Automation bot built with UiPath REFramework and Azure Document Intelligence (Form Recognizer).

## Tech Stack
- **RPA Framework:** UiPath REFramework
- **AI Extraction:** Azure Document Intelligence (prebuilt-invoice model)
- **Queue Management:** UiPath Orchestrator Queues
- **Output:** TSV report + Outlook email notification
- **Config Management:** Orchestrator Assets

## Features
- Extracts Vendor, Invoice #, Date, and Amount from PDF invoices
- REFramework with built-in retry logic and exception handling
- Orchestrator queue processing with unique reference enforcement
- Automated email notification per processed invoice
- Audit log output file

## Architecture
Orchestrator Queue → REFramework → Azure Document Intelligence API → Excel Output + Email Notification
