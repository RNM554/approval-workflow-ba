# UAT Test Cases

Internal Request Approval System (ASP.NET Core MVC, SQL Server).
Roles: Người đề nghị (requester), Người phê duyệt (approver), Quản trị viên (admin).
Demo accounts: `nhanvien` (requester), `truongphong` (approver, step 1), `giamdoc` (approver, step 2), `admin`. Password: `123`.

Status values: Not run / Pass / Fail. Only mark Pass after executing the steps.

## 1. Requests

| TC ID | Scenario | Steps | Expected result | Status |
| :--- | :--- | :--- | :--- | :--- |
| UAT-01 | Submit a leave request | 1. Log in as `nhanvien`.<br>2. "Tạo đề nghị mới" → "Nghỉ phép".<br>3. Fill "Từ ngày", "Đến ngày", "Nội dung".<br>4. Click "Gửi đề nghị". | Status is "Chờ duyệt". Request is at step 1 ("Trưởng phòng duyệt") with the configured approver. Due time = submit time + step deadline (hours). History shows "Gửi đề nghị...". Fields "Địa điểm" and "Kinh phí" are hidden for leave. | Not run |
| UAT-02 | Required fields when submitting | 1. Log in as `nhanvien`.<br>2. Choose "Công tác", leave "Địa điểm" blank, click "Gửi đề nghị". | Error "Vui lòng nhập địa điểm." Request is not submitted. For "Tổ chức hoạt động", blank cost gives "Vui lòng nhập kinh phí dự kiến." | Not run |
| UAT-03 | Save draft | 1. As `nhanvien`, fill a request partially.<br>2. Click "Lưu nháp". | Status "Nháp". No approval step is created. Request can be edited and sent later. | Not run |
| UAT-04 | End date before start date | 1. As `nhanvien`, set "Đến ngày" earlier than "Từ ngày".<br>2. Save. | Error "Ngày kết thúc phải sau hoặc bằng ngày bắt đầu." | Not run |
| UAT-05 | Attachment rules | 1. Attach a `.exe` file, then a file over 10 MB, then 6 files. | Each case is rejected with a clear message. A valid `.pdf` uploads and can be downloaded from the detail page. | Not run |
| UAT-06 | Cancel request | 1. Submit a request.<br>2. Before anyone processes it, click "Hủy đề nghị" and confirm. | Status "Đã hủy". Edit and cancel buttons disappear. After a step is approved, cancel is no longer offered. | Not run |

## 2. Approval

| TC ID | Scenario | Steps | Expected result | Status |
| :--- | :--- | :--- | :--- | :--- |
| UAT-07 | Approve and move to next step | 1. Submit a "Công tác" request as `nhanvien`.<br>2. Log in as `truongphong`, open it from "Cần tôi xử lý", click "Phê duyệt" and confirm.<br>3. Log in as `giamdoc` and approve. | After step 1 the request moves to "Giám đốc duyệt". After step 2 status is "Đã duyệt". Detail page shows each step, approver and processing hours. | Not run |
| UAT-08 | Reject requires a reason | 1. As `truongphong`, open a pending request, click "Từ chối", leave the box blank, confirm. | The dialog shows "Vui lòng nhập nội dung." and does not submit. With a reason, status becomes "Đã từ chối" and the requester sees the reason. | Not run |
| UAT-09 | Request edit, resubmit to the same step | 1. As `truongphong`, click "Yêu cầu chỉnh sửa" with a note.<br>2. As `nhanvien`, edit and click "Cập nhật và gửi lại". | Status "Chờ chỉnh sửa" then back to "Chờ duyệt" at the same step and the same approver. | Not run |
| UAT-10 | Not your turn | 1. While a request is at step 1, log in as `giamdoc` and open it. | No approve/reject buttons. Message says it is not your turn yet. | Not run |
| UAT-11 | Approver cannot process own request | 1. Log in as `truongphong`, submit a "Công tác" request.<br>2. Open the detail page. | Step 1 (assigned to self) is skipped and history records it. Request goes to `giamdoc`. No action buttons for the owner. A leave request with only the self-assigned step cannot be submitted ("Không có người phê duyệt phù hợp"). | Not run |

## 3. Administration

| TC ID | Scenario | Steps | Expected result | Status |
| :--- | :--- | :--- | :--- | :--- |
| UAT-12 | Edit a workflow already in use | 1. As `admin`, open "Quy trình phê duyệt", edit a workflow that has requests, change a step, save. | System creates a new version and deactivates the old one. Existing requests keep their old version. | Not run |
| UAT-13 | Lock an account | 1. As `admin`, lock `nhanvien` (warning appears if the user is assigned to steps).<br>2. In another browser where `nhanvien` is logged in, click any link. | The locked user is signed out immediately and cannot log in again. | Not run |
| UAT-14 | Delete rules | 1. As `admin`, try deleting a workflow, request type or user that already has requests. | No "Xóa" button for them; they can only be deactivated or locked. Unused ones can be deleted after confirmation. | Not run |

## 4. Monitoring and statistics

| TC ID | Scenario | Steps | Expected result | Status |
| :--- | :--- | :--- | :--- | :--- |
| UAT-15 | Overdue monitoring | 1. Make a pending request overdue (demo data or by editing its deadline).<br>2. Open "Hồ sơ quá hạn" as an approver. | The request is listed with hours overdue, most overdue first. Counters show pending, overdue and due-soon. | Not run |
| UAT-16 | Bottleneck statistics | 1. As `admin`, load demo data.<br>2. Open "Báo cáo và thống kê". | Steps are ranked by average processing hours. The slowest step is highlighted. Steps with fewer than 5 cases are marked "Mẫu nhỏ". Excel export matches the screen. | Not run |
| UAT-17 | Data scope by department | 1. Log in as `truongphong` (or another department head) and open statistics.<br>2. Compare with `admin`. | Header shows the department scope. Numbers cover only that department and its sub-departments; admin and the top of the department tree see the whole company. | Not run |

## 5. AI assistance (requires a Gemini API key in User Secrets)

| TC ID | Scenario | Steps | Expected result | Status |
| :--- | :--- | :--- | :--- | :--- |
| UAT-18 | Approver analysis | 1. As the current approver, open a pending request and click "Phân tích đề nghị". | A short summary and notes appear. Only the current approver sees the button. The AI never approves or rejects. | Not run |
| UAT-19 | AI unavailable | 1. Remove the API key or disconnect the network, click an AI button. | A friendly message appears; the rest of the system still works. | Not run |

## 6. JSON endpoints (called by the UI, cookie auth + anti-forgery token)

There is no public REST API. These endpoints are only used by the web pages.

| Endpoint | Method | Scenario | Expected response | Status |
| :--- | :--- | :--- | :--- | :--- |
| `/ThongBao/Lay` | GET | Logged-in user loads notifications | `200` JSON `{ items: [...] }` for that user only | Not run |
| `/ThongBao/Lay` | GET | Not logged in | Redirect to login (302); with header `X-Requested-With: XMLHttpRequest` expected 401 (to verify) | Not run |
| `/TroLyAi/KiemTra` | POST | Requester, no content entered | `200` JSON with `ok = false` and a message asking for content | Not run |
| `/ThongKe/NhanXet` | POST | Report with no data | `200` JSON with `ok = false`, "Chưa có dữ liệu..." | Not run |
