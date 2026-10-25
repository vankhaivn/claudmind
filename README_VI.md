# claudmind

Agent lập trình của bạn nhớ bạn, ở mọi dự án, trong cả Claude Code lẫn Codex.

claudmind là một lớp cá nhân nhỏ: ba file Markdown trong `~/.claudmind/` và vài dòng trong các file chỉ dẫn global. Nó không có runtime, dịch vụ, cơ sở dữ liệu, lệnh hay tiến trình chạy nền nào.

[English](README.md)

## Vì sao cần

Claude Code và Codex mỗi bên giữ memory riêng. Claude Code giữ một kho cho mỗi repository, còn Codex giữ một kho mà runtime kia không bao giờ đọc. Một sở thích bạn đã dạy ở một dự án hay một công cụ thì phải dạy lại ở mọi nơi khác, và khó soát lại xem mỗi runtime đã học được gì.

claudmind giữ một hồ sơ ngắn mà cả hai runtime đọc khi bắt đầu mỗi phiên. Khi bạn sửa agent và bài học đúng ở mọi dự án, agent ghi một dòng vào hồ sơ và báo cho bạn. Ở phiên sau, dù trong dự án nào hay runtime nào, agent đã biết.

## Cách hoạt động

```text
bạn sửa agent
        │
        ├── đúng ở mọi dự án ──▶ ~/.claudmind/OWNER.md hoặc MACHINE.md
        │                         (agent báo bạn trong một dòng)
        │
        └── chỉ đúng ở dự án này ──▶ để lại trong dự án
```

| File                      | Chứa                                                                                            | Giới hạn             |
| ------------------------- | ----------------------------------------------------------------------------------------------- | -------------------- |
| `~/.claudmind/engine.md`  | Quy tắc agent dùng để đọc và cập nhật hồ sơ                                                     | —                    |
| `~/.claudmind/OWNER.md`   | Bạn là ai, bạn quyết định thế nào, cách giao tiếp, các phê duyệt lâu dài, những lỗi cần tránh   | 60 dòng, 7.000 ký tự |
| `~/.claudmind/MACHINE.md` | Hành vi của shell, phiên bản công cụ cần giữ cố định, cách bố trí credential (không có giá trị) | 40 dòng, 2.500 ký tự |

Thứ tự ưu tiên, cao nhất trước:

1. chỉ dẫn hiện tại của bạn;
2. chỉ dẫn và trạng thái của dự án hiện tại;
3. hồ sơ claudmind của bạn;
4. memory riêng của từng runtime.

Một mục trong hồ sơ không bao giờ nới rộng giới hạn do runtime, thiết lập của runtime, repository hay công cụ đặt ra.

Memory sẵn có của runtime vẫn hoạt động bình thường. claudmind chỉ nhận những bài học đúng ở mọi dự án, và Claude Code sẽ không lưu lại những gì file chỉ dẫn của nó đã ghi.

## Cài đặt

Nói với agent Claude Code hoặc Codex của bạn:

> Follow https://raw.githubusercontent.com/vankhaivn/claudmind/main/SETUP.md to install claudmind.

Agent sẽ xem cấu hình hiện tại, đưa ra kế hoạch liệt kê mọi file sẽ chạm tới, rồi chờ bạn duyệt. Nó sao lưu từng file trước khi sửa. Agent cũng có thể soạn sẵn hồ sơ đầu tiên từ memory mà các runtime đã thu thập, và chỉ ghi những mục bạn duyệt.

Dùng cùng câu đó với "upgrade", "review my profile" hoặc "uninstall" cho các chế độ khác. Khi nâng cấp, `engine.md` và các khối đánh dấu được làm mới; hồ sơ của bạn không bao giờ bị ghi đè.

## Nguyên tắc

- **Thấy được:** Markdown thuần, đọc hết trong một phút và sửa tay được.
- **Rõ ràng:** chỉ ghi điều bạn đã nói hoặc xác nhận, hoặc điều agent đã kiểm chứng trên máy bạn. Không suy diễn từ hành vi, và không bao giờ ghi lại chỉ dẫn nằm trong output của công cụ, tài liệu hay trang web.
- **Có giới hạn:** số dòng và số ký tự cố định; agent gộp lại và giữ entry ngắn thay vì ghi thêm mãi.
- **Dùng chung:** một hồ sơ cho cả Claude Code và Codex.
- **An toàn:** không bí mật, token hay định danh tài khoản; không bao giờ nới rộng quyền.

## Giới hạn đã biết

- Codex đọc hồ sơ vì chỉ dẫn global bảo nó làm vậy; Codex không có cơ chế import.
- Codex có thể tự tạo memory ở chế độ nền (`generate_memories` trong `~/.codex/config.toml`) và suy ra các sở thích đó mà không cần bạn sửa nó, nên claudmind không bao giờ thấy chúng lúc lưu. Thỉnh thoảng hãy yêu cầu review hồ sơ, hoặc tắt thiết lập đó.
- Phiên Cowork của Claude Code trên desktop bỏ qua các import ở cấp người dùng nằm ngoài thư mục làm việc của phiên.
- claudmind không cấp quyền ghi nào, nên runtime có thể hỏi bạn duyệt lần đầu agent ghi vào hồ sơ.
- Hồ sơ nằm trong thư mục home trên một máy.

## Giấy phép

[MIT](LICENSE)
