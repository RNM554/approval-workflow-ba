# Use Case Specifications

## Overview
Develop a system that allows users to create and submit internal requests such as leave of absence, business trip registration, or event organization proposals using predefined forms. The system supports setting up approval steps, identifying handlers, requesting additional information, and tracking the history of document processing. Managers can monitor overdue documents, gather statistics on processing times, and identify steps that frequently cause delays in the workflow.

---

## 1. RACI Matrix (Role-Based Access Control)
| Function / Use Case | Employee (Requester) | Manager (Approver) | System Admin |
| :--- | :--- | :--- | :--- |
| **Create Request** | Responsible (R) | Informed (I) | - |
| **Update/Cancel Request** | Responsible (R) | - | - |
| **Approve/Reject Request** | Informed (I) | Responsible (R) | - |
| **View Own Requests** | Accountable (A) | - | - |
| **View Assigned Requests** | - | Accountable (A) | - |
| **View Statistical Reports** | - | Accountable (A) | - |
| **Manage Request Types & Workflows**| - | - | Responsible (R) |
| **Manage Users & Roles** | - | - | Responsible (R) |

---

## 2. Core Detailed Use Cases

### 2.1 Detailed Use Case: Create Request
- **Actor:** Employee (Requester)
- **Pre-condition:** Requester has successfully logged into the system.
- **Main Flow:**
  1. Requester selects "Create new request" on the main screen. System displays the available request types.
  2. Requester selects a request type. System displays the corresponding form.
  3. Requester fills in the fields (can attach files) and clicks "Submit request".
  4. System saves the new information into the database, displays the new request with "Pending" status at the top of the request list, and displays a success message.
- **Alternative Flows:**
  - **Save Draft:** At step 3, the Requester inputs information and clicks "Save Draft". System saves the entered data and displays the request with "Draft" status.
  - **Invalid Input:** At step 3, the Requester misses mandatory information or inputs invalid data. System highlights the error on the corresponding field and asks for re-entry.

### 2.2 Detailed Use Case: Approve Request
- **Actor:** Approver (Manager / Director)
- **Pre-condition:** Approver is logged in and the request is currently assigned to their step in the workflow.
- **Main Flow:**
  1. Approver selects a pending request from their assigned list.
  2. System displays full request details, attachments, and processing history.
  3. Approver reviews the information and clicks "Approve".
  4. Approver enters an optional comment and confirms.
  5. System saves the action, updates the request status to "Approved" (or forwards it to the next step if applicable), and logs the timestamp.
- **Alternative Flows:**
  - **Reject:** Approver clicks "Reject". System requires a mandatory reason. Status changes to "Rejected". Workflow terminates.
  - **Request Revision:** Approver clicks "Request Revision". System requires details of missing info. Status changes to "Revision Required" and routes back to the Requester.

### 2.3 Detailed Use Case: Configure Approval Workflow
- **Actor:** System Admin
- **Pre-condition:** Admin is logged into the system.
- **Main Flow:**
  1. Admin navigates to "Workflow Management" and clicks "Add New".
  2. Admin selects a Request Type (e.g., Purchasing Proposal).
  3. Admin defines sequential approval steps (e.g., Step 1: Department Manager, Step 2: Director).
  4. For each step, Admin sets the assigned Role and maximum processing time (SLA in hours).
  5. Admin clicks "Save". System validates and activates the new workflow version.
- **Alternative Flows:**
  - **Invalid Configuration:** At step 4, the Admin enters invalid data (e.g., negative SLA hours). System displays an error and prompts for correction.
