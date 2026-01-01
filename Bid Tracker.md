
# 📌 Bid Intake & Duplicate Detection Automation (Microsoft 365)

## 📖 Overview

This project implements an **end-to-end bid intake and tracking automation** for construction companies using **Microsoft 365** tools.

The solution automatically:

* Captures bid request emails
* Prevents duplicates
* Detects updated/revised bids
* Creates structured SharePoint folders
* Uses AI-assisted subject similarity when metadata changes
* Maintains a clean audit trail in a SharePoint List

The design balances **automation + human oversight**, making it reliable for real-world vendor communication patterns.

---

## 🛠️ Tech Stack

* **Outlook (Shared Mailbox)**
* **Power Automate (Cloud Flows + Child Flow)**
* **SharePoint Lists**
* **SharePoint Document Library**
* **AI Builder / Azure OpenAI (for subject similarity analysis)**

---

## 🎯 Trigger Conditions

* Email **must contain attachments**
* Email **subject contains keyword:**

  ```
  "Bid Request"
  ```

---

## 🧠 Core Design Logic

### 1️⃣ Hash-Based Duplicate Detection (Primary)

To detect exact duplicates reliably—even when subjects or senders vary—the system generates a **hash code** using:

* Attachment file names
* Attachment sizes
* Sorted in ascending order to ensure consistency

**Purpose:**

* Catch exact re-sends
* Handle forwarded or resent emails
* Avoid subject-based false positives

---

### 2️⃣ Exact Duplicate Handling

If the **generated hash already exists** in the SharePoint List:

✔ Considered a **duplicate bid**
✔ Create a **new uniquely named folder**
✔ Update the existing SharePoint List item with:

* Folder link
* Received date
* Subject
* Hash code

---

### 3️⃣ Intelligent Updated-Bid Detection (AI-Assisted)

If the **hash is NOT found**, a **child flow** is triggered:

**Child Flow Logic:**

* Filters SharePoint records from the **last 15 days**
* Uses an AI model to compare **subject similarity**
* Produces a **confidence score (0–100)**

**Decision Rule:**

* **Confidence ≥ 85%** → Treated as an **updated/revised bid**

  * Create a **sub-folder inside the existing bid folder**
  * Update SharePoint List with new folder link and received timestamp

---

### 4️⃣ New Bid Creation

If:

* Hash does not exist
* AND AI confidence < 85%

✔ Treated as a **new bid**

Actions:

* Create a new bid folder
* Upload all attachments
* Insert a new record into SharePoint List with:

  * Bid ID
  * Folder link
  * Subject
  * Received date
  * Hash code

---

## 🗂️ Folder Structure Design

```text
Bids
 ├─ BID-2026-014
 │   ├─ V1
 │   │   └─ Attachments
 │   ├─ V2
 │   │   └─ Revised Documents
```

* Ensures traceability
* Supports bid revisions
* Easy navigation for estimators and PMs

---

## 🧩 Key Features

* ✅ Duplicate prevention (hash-based)
* ✅ AI-assisted fuzzy matching for revised bids
* ✅ Version-aware folder creation
* ✅ SharePoint List as system of record
* ✅ Child flow separation for scalability
* ✅ Safe automation with fallback logic

---

## 🚀 Why This Approach Works

| Challenge                     | Solution                       |
| ----------------------------- | ------------------------------ |
| Subject changes               | AI similarity scoring          |
| Resent emails                 | Hash-based detection           |
| Forwarded emails              | Attachment-based fingerprint   |
| Vendor behavior inconsistency | Human-in-the-loop thresholds   |
| Scalability                   | Child flows + clean data model |

---


## 👤 Author

**Mohammad Asim**
Business Automation Engineer
Power Automate | SharePoint | AI-driven workflows

---

## 🏁 Final Note

This project demonstrates **real-world automation design**, not just flow creation—handling ambiguity, duplicates, and evolving vendor behavior using **both deterministic logic and AI**.

---
