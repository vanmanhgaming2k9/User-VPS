# Hướng Dẫn Tạo User Và Quản Lý Quyền Đăng Nhập VPS Windows

## 1. Các lệnh đã sử dụng

| Lệnh | Ý nghĩa | Chức năng |
|-----------------|-----------------------------|------------------------------------------------------------|
| `net user nullaMC phong@123 /add` | Tạo user mới | Tạo tài khoản người dùng `nullaMC` với mật khẩu `phong@123` |
| `net localgroup "Remote Desktop Users" nullaMC /add` | Thêm user vào nhóm RDP | Cho phép user `nullaMC` có quyền đăng nhập từ xa qua Remote Desktop |
| `reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\SpecialAccounts\UserList" /v nullaMC /t REG_DWORD /d 1 /f` | Thêm user vào danh sách đặc biệt trong đăng nhập Windows | Đánh dấu user `nullaMC` trong registry để điều chỉnh quyền hiển thị hoặc đăng nhập |
| `reg add "HKU\.DEFAULT\Software\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Shell /t REG_SZ /d "cmd.exe" /f` | Đổi shell mặc định của hệ thống | Đặt `cmd.exe` làm shell mặc định thay cho explorer.exe trong hồ sơ người dùng mặc định |

### Mục đích

- Tạo user `nullaMC` có thể đăng nhập VPS Windows qua Remote Desktop.
- Giới hạn quyền user bằng cách thay đổi shell mặc định sang command prompt (cmd.exe), hạn chế sử dụng giao diện Windows thông thường.
- Sử dụng registry để quản lý hiển thị và quyền của user.

---

## 2. Bảng toàn bộ quyền user trên Windows và cách thêm/xóa

| Quyền/Nhóm Người Dùng      | Mô tả quyền                                                                 | Lệnh thêm user vào nhóm                      | Lệnh xóa user khỏi nhóm                    |
|----------------------------|-----------------------------------------------------------------------------|----------------------------------------------|--------------------------------------------|
| Administrators             | Toàn quyền quản trị hệ thống, có thể thay đổi mọi thiết lập, cài đặt phần mềm | `net localgroup Administrators username /add` | `net localgroup Administrators username /delete` |
| Remote Desktop Users       | Quyền đăng nhập qua Remote Desktop (RDP)                                    | `net localgroup "Remote Desktop Users" username /add` | `net localgroup "Remote Desktop Users" username /delete` |
| Users                      | Quyền người dùng bình thường, có thể sử dụng ứng dụng và thay đổi thiết lập cá nhân | `net localgroup Users username /add`         | `net localgroup Users username /delete`     |
| Guests                     | Quyền hạn chế hơn Users, thường dùng cho người tạm thời                     | `net localgroup Guests username /add`        | `net localgroup Guests username /delete`    |
| Backup Operators           | Có quyền sao lưu và phục hồi hệ thống                                       | `net localgroup "Backup Operators" username /add` | `net localgroup "Backup Operators" username /delete` |
| Power Users                | Quyền gần giống Admin, nhưng hạn chế hơn, không có toàn quyền                | `net localgroup "Power Users" username /add` | `net localgroup "Power Users" username /delete` |
| Account Operators          | Quản lý user và nhóm (tạo, xóa user, đặt mật khẩu)                          | `net localgroup "Account Operators" username /add` | `net localgroup "Account Operators" username /delete` |
| Network Configuration Operators | Quản lý cấu hình mạng                                                | `net localgroup "Network Configuration Operators" username /add` | `net localgroup "Network Configuration Operators" username /delete` |
| Remote Management Users    | Cho phép remote quản lý dịch vụ                                              | `net localgroup "Remote Management Users" username /add` | `net localgroup "Remote Management Users" username /delete` |
| Cryptographic Operators    | Quản lý khóa mã hóa và chứng chỉ                                             | `net localgroup "Cryptographic Operators" username /add` | `net localgroup "Cryptographic Operators" username /delete` |

---

### Cách sử dụng lệnh:

- **Thêm user vào nhóm**:

```cmd
net localgroup "Tên nhóm" username /add
