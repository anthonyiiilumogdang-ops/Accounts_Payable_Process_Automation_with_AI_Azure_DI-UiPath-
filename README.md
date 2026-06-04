# Accounts Payable Invoice Automation with Azure Document Intelligence - UiPath

An enterprise-grade Accounts Payable Invoice Automation bot built with UiPath REFramework and Azure Document Intelligence.

## Tech Stack
| Component | Tool |
|-----------|------|
| RPA Framework | UiPath REFramework |
| AI Extraction | Azure Document Intelligence (prebuilt-invoice model) |
| Queue Management | UiPath Orchestrator Queues |
| Credential Storage | UiPath Orchestrator Assets |
| Output | TSV report + Outlook email notification |

## Showcased Skills
| Skill | Details |
|-------|---------|
| REFramework | Full state machine implementation with Init, Get Transaction, Process, and End states |
| Orchestrator Queues | Queue creation, item upload, unique reference enforcement, auto-retry configuration |
| Orchestrator Assets | Secure credential and config storage retrieved at runtime |
| Azure API Integration | HTTP REST API call with polling loop using VB.NET Invoke Code |
| JSON Parsing | Deserialize and navigate nested JSON response from Azure Document Intelligence |
| Exception Handling | Business vs Application Exception classification per REFramework standard |
| VB.NET Scripting | File I/O, string manipulation, HTTP client usage inside Invoke Code activities |
| Modular Design | Separation of concerns across ExtractInvoiceData, WriteToExcel, SendEmailNotification workflows |
| Email Automation | Outlook Desktop integration with dynamic HTML email body |
| Git + GitHub | Version control with secret scanning awareness and clean commit history |
| Config Management | Centralized Config.xlsx pattern with Orchestrator asset name mapping |

## Features
- AI-powered extraction of Vendor Name, Invoice Number, Invoice Date, and Total Amount from PDF invoices
- REFramework with built-in retry logic and Business vs Application exception handling
- Orchestrator queue processing with unique reference enforcement (duplicate prevention)
- Automated Outlook email notification per processed invoice
- Audit log TSV output file with processing timestamp and status

## Architecture

    Orchestrator Queue
          |
    REFramework (InitAllSettings > GetTransactionData > Process > SetTransactionStatus)
          |
    ExtractInvoiceData.xaml --> Azure Document Intelligence API
          |
    WriteToExcel.xaml --> ProcessedInvoices.tsv
          |
    SendEmailNotification.xaml --> Outlook Desktop

## Project Structure

    AP_Invoice_Automation_UiPath/
    |-- Framework/
    |   |-- InitAllSettings.xaml
    |   |-- GetTransactionData.xaml
    |   |-- Process.xaml
    |   └-- SetTransactionStatus.xaml
    |-- Workflows/
    |   |-- ExtractInvoiceData.xaml
    |   |-- WriteToExcel.xaml
    |   └-- SendEmailNotification.xaml
    |-- Tests/
    |   └-- Test_ExtractInvoiceData.xaml
    └-- Data/
        └-- Config.xlsx

## Orchestrator Setup
- **Folder:** UiPath_Portfolio/AP_Invoice_Automation
- **Queue:** AP_Invoice_Queue (unique references enforced, 3 auto-retries)
- **Assets:** AzureEndpoint, AzureApiKey, OutputExcelPath, NotificationEmail

## Known Limitations
- **File format:** Output is saved as TSV with a .tsv extension. While it opens correctly in Excel, it is not a native .xlsx file. A future improvement would use UiPath Excel Activities to write a proper workbook.
- **Vendor name extraction:** Azure Document Intelligence occasionally returns vendor names with embedded newline characters for multi-line vendor fields. A string cleanup step has been added as a workaround.
- **Single invoice format tested:** Only tested with Canada Post PDF invoice format. Extraction accuracy may vary across different invoice layouts and vendors.
- **Synchronous polling:** The Azure Document Intelligence API call uses a polling loop with fixed 3-second intervals. For high-volume processing, an event-driven approach would be more efficient.
- **Local file dependency:** Invoice PDF files must be accessible on the local machine. A production implementation would integrate with SharePoint or a shared network drive.
- **Outlook Desktop required:** Email notifications require Outlook desktop app installed and logged in. SMTP-based sending would be more portable.
- **Community Edition constraints:** Built on UiPath Community Edition. Production deployment would require an Enterprise license for full Orchestrator features.
- **No Business Exception for missing fields:** If Azure fails to extract a field, the bot throws an application exception rather than a meaningful Business Exception.

## Portfolio Context
This project is part of a series demonstrating RPA + AI automation across multiple platforms:
- Automation Anywhere + IQ Bot
- Microsoft Power Automate + AI Builder
- **UiPath + Azure Document Intelligence (this project)**
