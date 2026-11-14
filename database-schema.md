# Relational Database Architecture
*RDBMS: Microsoft SQL Server*

## Core Entities & Relationships

### 1. User & Access Management
* **`NguoiDung` (Users):** `MaNguoiDung (PK)`, `TenDangNhap`, `MatKhau`, `HoTen`, `Email`, `MaPhongBan (FK)`, `MaChucDanh (FK)`, `TrangThai (Bit)`.
* **`VaiTro` (Roles):** `MaVaiTro (PK)`, `TenVaiTro` (e.g., Requester, Approver, Admin).
* **`NguoiDungVaiTro`:** Mapping table for Many-to-Many relationship between Users and Roles.
* **`PhongBan` (Departments):** `MaPhongBan (PK)`, `TenPhongBan`, `MaPhongBan_Cha (FK)` for hierarchical structure.

### 2. Workflow Configuration
* **`LoaiDeNghi` (Request Types):** `MaLoai (PK)`, `TenLoai`, `MaKyThuat`.
* **`QuyTrinhPheDuyet` (Workflows):** `MaQuyTrinh (PK)`, `TenQuyTrinh`, `MaLoai (FK)`, `PhienBan (Int)`, `TrangThaiApDung (Bit)`.
* **`BuocQuyTrinh` (Workflow Steps):** `MaBuoc (PK)`, `MaQuyTrinh (FK)`, `TenBuoc`, `ThuTuBuoc`, `MaNguoiDung_XuLy (FK)`, `ThoiHanXuLyGio (Int)`.

### 3. Request Processing (Transactional Data)
* **`HoSoDeNghi` (Requests):** 
  * `MaHoSo (PK)`, `SoHoSo`, `MaLoai (FK)`, `MaNguoiDung_Tao (FK)`, `MaQuyTrinh (FK)`, `MaBuocHienTai (FK)`.
  * `NoiDung (Nvarchar(max))`, `KinhPhi (Float)`.
  * `TrangThai (Nvarchar(50))`: Draft, Pending, Revision Required, Approved, Rejected.
  * `NgayTao (Datetime)`.
* **`BuocXuLyHoSo` (Approval Actions):** `MaBuocXuLy (PK)`, `MaHoSo (FK)`, `MaBuoc (FK)`, `ThoiDiemNhan (Datetime)`, `HanXuLy (Datetime)`, `KetQua`, `YKien`, `QuaHan (Bit)`.
* **`LichSuXuLy` (Audit Trail):** Logs every system action with `NoiDung (Nvarchar(max))` and `ThoiDiem (Datetime)`.
* **`TepDinhKem` (Attachments):** `MaTep (PK)`, `MaHoSo (FK)`, `TenTep`, `DuLieuTep (Varbinary(max))`. *(Note: Files are stored directly in DB using SQL Server's Varbinary type).*
