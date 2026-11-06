# HANDOFF — Rental Management (cập nhật 05/11/2026)

File này dùng để phiên Claude Code mới đọc vào và làm tiếp công việc dở dang.

## Trạng thái hiện tại

**Đã có**
- BMAD Method v6.12.4 đã được cài, gồm `_bmad/` và các skill trong `.claude/skills/bmad-*`. Agent giao tiếp và viết tài liệu bằng tiếng Việt.
- Spec gốc của PO: `rental_management_po_specification_vi.md` (v1.0). **Không sửa file này.**
- Phụ lục đã chốt: `rental_management_spec_addendum_v1.1.md`. Nếu có chỗ khác với spec gốc thì làm theo phụ lục. Dự án đang ở giai đoạn ý tưởng nên mọi quyết định đã được chốt theo best practice, đánh dấu `[Đã chốt – BP]`.
- Phiên brainstorm (đã hoàn tất): `_bmad-output/brainstorming/brainstorm-rental-spec-gaps-2026-11-05/`, gồm `.memlog.md` và `brainstorm.html`.

**Stitch**
- Project `5206496596090164327`, tên "Rental Management – Cửa hàng cho thuê trang phục".
- Design system "Atelier Ops", id `assets/17955589590259641591`. Màu chính #7A2E4A, điểm nhấn #C9A46A, font Be Vietnam Pro, bo góc 8px. Mỗi trạng thái có màu riêng và luôn đi kèm icon cùng nhãn chữ.
- Bản sao local (PNG + HTML) nằm ở `design/stitch/`.

## Màn hình đã có trên Stitch (bản mới nhất)

| # | Màn hình | Thiết bị | Screen ID |
|---|---|---|---|
| 01 | Dashboard / Kanban vận hành | Desktop | 4372054201634a9b9906653c8a0ef759 |
| 02 | Tạo đơn thuê (bước 3 – chọn món) | Desktop | e0938b51f68049d29b31bab1852ab8a8 |
| 03 | Lịch thuê timeline theo tài sản | Desktop | 27c6c9e511524d5ea7f7b1076dfd4a6b |
| 04 | Chi tiết đơn thuê | Desktop | 09e4b11fc3bc41d89be314003ed23462 |
| 05 | Chi tiết tài sản (tab Hiệu suất) | Desktop | bf23e8900069480296c06d1e6b14795a |
| 06 | Bàn giao | Tablet | 1c6f63ddd52d423fba92498242a1215a |
| 07 | Nhận trả & kiểm tra | Mobile | 6fc166e88ced4dfaa2a547771edd168b |
| 08 | Quyết toán cọc | Desktop | e11d344c0f8447eaa037343a848b7c4b |
| 15 | Login | Desktop | e5a8fdd8d043498fb5d299b51769d7fc |

Số thứ tự đã dành sẵn cho các màn còn thiếu: 09 Lịch thử đồ, 10 Sổ giao dịch & công nợ, 11 Danh sách tài sản + tem QR, 12 Quét QR (mobile), 13 Gia hạn/Đổi món (drawer), 14 Cài đặt chính sách thuê.

**Lưu ý kỹ thuật (05/11/2026, phiên 2):**
- `generate_screen_from_text` bị timeout phía client sau khoảng 60 giây. Màn phức tạp không kịp trả kết quả, nên không lấy được screen ID. 8 lượt đã timeout (7 lượt song song và 1 lượt Lịch thử đồ); các lượt này có thể vẫn đã sinh màn trên Stitch, cần kiểm tra trên web UI.
- `list_screens`/`get_project` KHÔNG liệt kê đủ màn hình, kể cả màn Login vừa tạo. `get_screen` theo ID thì vẫn chạy được.
- Không gọi generate song song.
- Gọi curl tới URL tải về sẽ bị 401. Cách lưu bản local: lấy HTML bằng `download_file`, ghi ra file, rồi render PNG bằng `google-chrome --headless=new --force-device-scale-factor=2 --window-size=1280,<height/2> --virtual-time-budget=8000 --screenshot=...`.
- Màn 15 Login còn một lỗi nhỏ cần sửa: chân trang có dòng "KHO - Q1 · PRODUCTION" gợi ý một địa điểm cụ thể.

Trong project còn các bản cũ, được tạo trước mỗi lượt sửa. Chỉ xóa các bản đó khi người dùng đồng ý.

## Quy tắc khi vẽ UI (bắt buộc khớp với phụ lục v1.1)

- MVP chỉ có **một cửa hàng**. Không hiển thị bộ chọn chi nhánh; dùng nhãn "Cửa hàng chính".
- Không giữ CCCD gốc và không nhận cọc thế chấp. Cọc thu bằng tiền mặt hoặc chuyển khoản VietQR khi bàn giao. Ảnh giấy tờ chỉ lưu khi khách đồng ý và tự xóa sau 30 ngày.
- "Phí bảo vệ hư hỏng nhỏ (10%)" **không phải bảo hiểm**. Không dùng chữ "bảo hiểm" hay "hạn mức chi trả".
- Tiền cọc luôn hiển thị tách riêng và **không tính vào doanh thu**.
- Mã đơn có dạng `DT-2611-xxxx`. Năm hiển thị là 2026. Tiền tệ VND, viết dạng 1.250.000đ.
- Trạng thái đơn được suy ra từ trạng thái của các dòng hàng. OVERDUE và AT_RISK là cờ, không phải trạng thái.
- Khi gọi `generate_screen_from_text`, truyền `designSystem: assets/17955589590259641591` và `projectId: 5206496596090164327`.

## Việc tiếp theo (theo thứ tự)

1. **Vẽ thêm các màn hình còn thiếu trên Stitch.** Dùng các công cụ `mcp__stitch__*`, vì phiên mới sẽ tải chúng trực tiếp.
   - Lịch thử đồ (Fitting) – desktop (phụ lục 11.1)
   - Sổ giao dịch & công nợ – desktop (mục 4, M11.04–05)
   - Danh sách tài sản + in tem QR – desktop (M03.01)
   - Quét QR toàn cục – mobile (SCR-023, 6.9)
   - Gia hạn thuê / Đổi món (drawer) – desktop (3.1, 3.2)
   - Cài đặt chính sách thuê – desktop (M15, mục 5, DEC-016…032)
   - Login – desktop (SCR-001)
2. Sau mỗi màn hình mới: tải PNG và HTML về `design/stitch/`, rồi kiểm tra các quy tắc ở mục trên.
3. Chạy `/bmad-prd` với đầu vào là spec v1.0 kèm phụ lục v1.1.
4. Chạy `/bmad-ux` để tạo DESIGN.md và EXPERIENCE.md, tham chiếu các màn hình Stitch.
5. Chạy `/bmad-architecture`, rồi `/bmad-create-epics-and-stories`, rồi `/bmad-sprint-planning`.
