# NW Fire Systems Tool & Inventory Management

---

## Overview

This project is a **Flask-based Python web application** that manages NW Fire Systems’ tool and inventory operations. It provides an interactive interface for users to submit forms, view tool logs, and manage parts, while handling all **Excel-based inventory data stored in Dropbox**. The app also integrates with the **Spectrum API** and **ServiceTrade API** to submit requisitions, transfer items, and keep inventory up-to-date.

**Key features include:**

- Web-based forms for submitting parts and service requests.
- Fetching and formatting tool logs for display.
- Updating tool status for requests and returns.
- Logging all tool transactions in a dedicated history sheet.
- Submitting job requisitions and transfers via API.
- Safe handling of Excel files with in-memory processing.
- Error tracking and reporting for inventory operations.

---

## User Experience

<details>
<summary><strong>Part Picker</strong></summary>

1. Select the form type to submit.
2. Enter required information in the displayed form.
3. Filter and select parts from Dropbox data.
4. Review the selection before submission.
5. The system sends an email to purchasing containing the submission information and a link with the submission ID.
6. Users can modify submitted lists (e.g., replace custom parts with existing parts).
7. The system runs the appropriate API call based on the form type and returns a **success or failure message** for Spectrum inventory updates.

</details>

<details>
<summary><strong>Tool Log</strong></summary>

- Allows users to **check out or return tools**.
- Displays the **current tool log**, including who has which tool and its location.

</details>

<details>
<summary><strong>Lists</strong></summary>

- Displays **Jobs List** and **Parts List**:
  - **Jobs List**: All active jobs and job numbers.
  - **Parts List**: All parts, part numbers, quantities, and locations.
- Users can update lists through API calls to **Spectrum** and **ServiceTrade**.
- Option to view **warehouse-specific inventory** details, including owner and location.

</details>

---

## Features

<details>
<summary><strong>Tool Log Management</strong></summary>

- Fetch raw tool logs from Dropbox and convert them into Python lists.
- Produce formatted display lists of tool records with consistent columns and date formatting.
- Update tool status (`IN` or `OUT`) with user, job, and date information.
- Maintain a separate **ToolHistory** sheet for all transactions in Dropbox.

</details>

<details>
<summary><strong>Inventory Operations</strong></summary>

- Add materials to service jobs via **Spectrum SOAP requests**.
- Submit job requisitions with batch and GUID tracking.
- Transfer items to or from warehouses while handling errors and updating local item status.
- Fetch up-to-date job and part information from **Spectrum** and **ServiceTrade**.

</details>

<details>
<summary><strong>Dropbox Integration</strong></summary>

- Download, update, and upload Excel files safely using in-memory processing.
- Display information from Dropbox to ensure **up-to-date data** is always available.
- Handle missing files or sheets gracefully.
- Ensure consistent Excel formatting and data integrity.

</details>

<details>
<summary><strong>Lists Viewing</strong></summary>

- Display part and job lists read from Dropbox.
- Update Dropbox lists by fetching the latest information via API calls.
- View specific warehouses with detailed inventory, owner, and location information.

</details>

<details>
<summary><strong>Error Handling</strong></summary>

- Track errors from **Spectrum API** and log them in affected items.
- Provide meaningful messages for failed inventory transactions.
- Maintain a clear record of all tool and inventory actions.

</details>

---

## Requirements

- **Python 3.10+**
- Packages:
  - `pandas`
  - `openpyxl`
  - `requests`
  - `zeep`
  - `lxml`
  - `dropbox`
  - `gunicorn` — Optional

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Dropbox File Paths

<details>
<summary><strong>Config</strong></summary>

- `/AppCache/config.json` — Contains all secrets/passwords

</details>

<details>
<summary><strong>Tool Inventory Paths</strong></summary>

- `/Tool Inventory/Pallet Stock.xlsx` — Contains overstock quantities to warn about ordering parts already overstocked  
- `/Tool Inventory/Assemblies.xlsx` — Contains parts that make up assemblies for modifying individual parts when an assembly is requested  
- `/Tool Inventory/Shelf Inventory.xlsx` — Contains warehouse part locations  
- `/Tool Inventory/Inventory All.xlsx` — Contains all parts and part numbers  
- `/Tool Inventory/NWFS Tool Log.xlsx` — Contains current tool log and request/return history

</details>

<details>
<summary><strong>Job Files</strong></summary>

- `/Job Files/Warehouses.xlsx` — Contains all warehouse names and their Spectrum numbers

</details>

<details>
<summary><strong>App Cache Paths</strong></summary>

- `/AppCache/valid_tokens.json` — Contains access token required to access site  
- `/AppCache/service_trade_token.json` — Contains ServiceTrade token for API access  
- `/AppCache/service_trade_last_sync.json` — Tracks last ServiceTrade job sync; only jobs modified after this date are synced  
- `/AppCache/blacklist.json` — Contains parts excluded from popularity sorting on part lists  
- `/AppCache/Items On-Hand Valuation.json` — Contains estimated part quantities  
- `/AppCache/submissions.json` — Stores all submission IDs and related information  
- `/AppCache/job_cache_spectrum.json` — Stores all Spectrum jobs to add to master  
- `/AppCache/job_cache_service_trade.json` — Stores all ServiceTrade jobs to add to master  
- `/AppCache/job_cache_active_spectrum.json` — Contains active Spectrum jobs displayed in jobs list  
- `/AppCache/job_cache_active_service_trade.json` — Contains active ServiceTrade jobs displayed in jobs list  
- `/AppCache/job_cache.json` — Master job cache to validate user-inputted jobs

</details>

<details>
<summary><strong>Form Database Paths</strong></summary>

- `/FormDatabase/Vehicle` — Excel files storing vehicle form submissions  
- `/FormDatabase/Stock` — Excel files storing stock form submissions  
- `/FormDatabase/Sprinkler And Fala` — Excel files storing sprinkler and FALA form submissions  
- `/FormDatabase/Service` — Excel files storing service form submissions  
- `/FormDatabase/csvfiles` — Stores CSV exports of submissions

</details>
