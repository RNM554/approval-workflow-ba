# Use Case Specifications

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

## 2. Detailed Use Case: Approve Request
* **Actor:** Approver (Manager / Director)
* **Pre-condition:** Approver is logged in and the request is currently assigned to their step in the workflow.
* **Main Flow:**
  1. Approver selects a pending request from their assigned list.
  2. System displays full request details, attachments, and processing history.
  3. Approver reviews the information and clicks "Approve".
  4. Approver enters an optional comment and confirms.
  5. System saves the action, updates the request status to "Approved" (or forwards it to the next step if applicable), and logs the timestamp.
* **Alternative Flows:**
  * **Reject:** Approver clicks "Reject". System requires a mandatory reason. Status changes to "Rejected". Workflow terminates.
  * **Request Revision:** Approver clicks "Request Revision". System requires details of missing info. Status changes to "Revision Required" and routes back to the Requester.

## 3. Detailed Use Case: Configure Approval Workflow
* **Actor:** System Admin
* **Main Flow:**
  1. Admin navigates to "Workflow Management" and clicks "Add New".
  2. Admin selects a Request Type (e.g., Purchasing Proposal).
  3. Admin defines sequential approval steps (e.g., Step 1: Department Manager, Step 2: Director).
  4. For each step, Admin sets the assigned Role and maximum processing time (SLA in hours).
  5. Admin clicks "Save". System validates and activates the new workflow version.
