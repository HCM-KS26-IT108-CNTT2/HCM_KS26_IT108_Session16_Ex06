# Green-Tech: Bản đặc tả không gian cộng tác

Dự án **Trang trại thông minh (Green-Tech)**. Tài liệu này mô tả cấu trúc Folder, cấu trúc Channel và quy định phân quyền của dự án.

## 1. Mục tiêu

- **Lưu trữ:** code cảm biến luôn an toàn nhờ Hybrid Workflow (làm việc trên Local, tự động sao lưu lên Cloud).
- **Chia sẻ:** nhà đầu tư chỉ xem báo cáo, đối tác thiết bị chỉ bình luận góp ý.
- **Giao tiếp:** thảo luận lỗi cảm biến được chuyển thành Task có Deadline trong Enterprise Workspace.

---

## 2. Cấu trúc Folder

```
GreenTech/
├── 01_Source/
│   ├── sensors/            # Code cảm biến (chỉ Dev được sửa)
│   ├── backend/
│   ├── .env.example        # File mẫu, KHÔNG chứa mật khẩu thật
│   └── .gitignore          # Loại trừ .env, node_modules, target/
├── 02_Reports/
│   ├── Investor/           # Báo cáo cho nhà đầu tư
│   └── Internal/           # Báo cáo nội bộ
├── 03_Partners/            # Thông số thiết bị, tài liệu trao đổi với đối tác
├── 04_Docs/                # SRS, hướng dẫn, test case
└── 99_Archive/             # Tài liệu hoàn thành, chờ bàn giao
```

### Hybrid Workflow cho đội Dev

| Bước | Thao tác |
|---|---|
| 1. Local | Sửa code bằng VS Code trong `D:\GreenTech\01_Source\` (tốc độ cao, mất mạng vẫn làm việc được) |
| 2. Auto Sync | Nhấn `Ctrl + S`, Google Drive for Desktop / OneDrive Client tự phát hiện thay đổi và đẩy lên Cloud (Delta Sync, chỉ gửi phần thay đổi) |
| 3. Cloud | Dữ liệu được sao lưu ở nhiều Data Center, có Version History để khôi phục, cả nhóm dùng chung một liên kết |

**Quy tắc đồng bộ:**

- Bật **Files On-Demand** để tiết kiệm dung lượng SSD.
- Chờ biểu tượng **tích xanh (Synced)** trước khi gập máy hoặc tắt nguồn.
- **KHÔNG sync:** `node_modules`, `target/`, file game/phim, file cache nặng.
- Mỗi tài liệu chỉ có **1 file duy nhất**, đặt tên chuẩn (không dùng kiểu `final_v2_sua`). Dùng **Version History** để quản lý phiên bản.
- **File `.env`:** chứa mật khẩu cảm biến, không đưa vào thư mục có chia sẻ cho người ngoài. Chỉ giữ `.env.example` trong thư mục chung.

---

## 3. Cấu trúc Channel (Lark Suite / Slack / Teams)

```
GREEN-TECH WORKSPACE
├── #announcements    # Thông báo chính thức: deadline, lịch họp (chỉ trưởng dự án đăng)
├── #sensor-dev       # Thảo luận code, kỹ thuật cảm biến
├── #sensor-bugs      # Báo lỗi cảm biến, mỗi lỗi chuyển thành Task
├── #docs-resources   # Link Cloud, tài liệu chung
├── #partners         # Trao đổi với đối tác thiết bị
└── #random           # Giao lưu, tán gẫu
```

| Kênh | Nội dung | Quy định |
|---|---|---|
| `#announcements` | Deadline, lịch họp, quyết định | Chỉ trưởng dự án đăng, thành viên chỉ đọc và thả emoji xác nhận |
| `#sensor-dev` | Code, review, hỏi đáp kỹ thuật | Viết tin đủ ngữ cảnh, luôn Reply in Thread |
| `#sensor-bugs` | Báo lỗi cảm biến | Mỗi lỗi chuyển thành Task (xem mục dưới) |
| `#docs-resources` | Link thư mục Cloud, tài liệu | Chỉ chia sẻ 1 liên kết Cloud, không đăng dữ liệu nhạy cảm (mật khẩu, `.env`) |
| `#partners` | Trao đổi với đối tác | Không đăng thông tin nội bộ nhạy cảm |
| `#random` | Chuyện ngoài công việc | Mọi nội dung tán gẫu đưa vào đây |

### Quy tắc giao tiếp chung

- Viết **"1 Tin Trọn Vẹn"**: vấn đề, link, ảnh chụp lỗi, mong muốn cụ thể (tránh lỗi No-Hello).
- Luôn **Reply in Thread**, **@mention** đúng người, không spam `@all` / `@channel`.
- Dùng **Schedule Send** để hẹn giờ gửi ngoài giờ nghỉ, **Do Not Disturb** khi cần tập trung.
- Thả emoji (👍, 👀, ✅) để xác nhận đã nhận tin.
- Không ép đối phương phải phản hồi ngay.

### Chuyển lỗi cảm biến thành Task

1. Thảo luận ở `#sensor-bugs` và xác định được lỗi.
2. Nhấn **Create Task** (1-click) trên tin nhắn lỗi.
3. Đặt **Người phụ trách (Assignee)**, **Hạn nộp (Due Date)** và bật nhắc nhở tự động.
4. Cập nhật tiến độ trong thread của Task.

Ví dụ: "Cảm biến độ ẩm đọc sai giá trị" → Task, Assignee: Dev A, Deadline: 17h thứ Sáu.

---

## 4. Quy định phân quyền

| Đối tượng | Mức quyền | Phạm vi | Ghi chú |
|---|---|---|---|
| Trưởng dự án | **Owner** | Restricted | Quản trị toàn bộ |
| Đội Dev | **Editor** | Restricted qua email | Chỉ `01_Source`, `04_Docs`; tắt quyền tự chia sẻ tiếp |
| Nhà đầu tư | **Viewer** | Restricted qua email | Chỉ `02_Reports/Investor`; tắt tải/in/sao chép; đặt ngày hết hạn |
| Đối tác cung cấp thiết bị | **Commenter** (dùng Suggesting Mode) | Restricted qua email | Chỉ `03_Partners` và tài liệu cần góp ý; không sửa trực tiếp |
| Người ngoài khác | Không cấp quyền | Không chia sẻ | |

### Quy tắc bảo mật

- **Quy tắc Cờ Đỏ:** TUYỆT ĐỐI không kết hợp **Editor + Anyone with the link**.
- Ưu tiên **Restricted** qua email cụ thể, áp dụng nguyên tắc **đặc quyền tối thiểu (Least Privilege)**.
- **File `.env`:**
  - Không chia sẻ cho nhà đầu tư và đối tác.
  - Chỉ cấp **Viewer** cho đúng Dev cần dùng, Restricted qua email, tắt tải/in/sao chép.
  - Nên dùng công cụ quản lý bí mật hoặc biến môi trường thay vì lưu mật khẩu dạng văn bản.
  - Đổi (rotate) mật khẩu ngay nếu nghi ngờ bị lộ.
- Đặt **Set Expiration Date** cho quyền tạm thời (nhà đầu tư, đối tác).
- Thực hiện **Access Audit** định kỳ, thu hồi quyền khi không còn cần.

---

## 5. Bàn giao khi kết thúc dự án (Transfer Ownership)

1. Rà soát toàn bộ quyền (Access Audit), thu hồi quyền của người không còn tham gia.
2. Chuyển tài liệu hoàn thành vào `99_Archive`.
3. Chủ sở hữu dùng **Transfer Ownership** để nhượng toàn bộ quyền quản trị cho người phụ trách mới, đảm bảo dữ liệu không bị xóa khi tài khoản cá nhân rời nhóm.
4. Đổi mật khẩu cảm biến trong `.env` và bàn giao riêng qua kênh an toàn.

---

## 6. Kiểm thử: lỡ xóa sạch folder `sensors`

Quy trình khôi phục nhanh nhất:

1. **Báo ngay** ở `#sensor-dev` để mọi người dừng sửa file. Tạm dừng sync trên máy người xóa nếu cần.
2. **Khôi phục từ Trash:** mở Drive trên web, vào **Trash**, tìm folder `sensors`, chọn **Restore**. Folder trở về đúng vị trí cũ (nhanh nhất, 1 thao tác).
3. **Version History:** nếu file bị xóa nội dung hoặc ghi đè, mở file, chọn **Version history**, chọn bản trước khi xóa rồi **Restore**.
4. **Dự phòng:** nếu quá thời hạn Trash, lấy từ bản local của đồng đội hoặc liên hệ quản trị viên để khôi phục.
5. **Xác minh:** chờ tích xanh (Synced), so sánh số lượng file, chạy thử code cảm biến.
6. **Phòng ngừa:** hạn chế quyền Editor, ghi nhận sự cố ở `#sensor-dev`, rà soát Access Audit.
