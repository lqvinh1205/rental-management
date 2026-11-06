# PHỤ LỤC ĐẶC TẢ v1.1 — XỬ LÝ GAP LOGIC

## Hệ thống quản lý cửa hàng cho thuê quần áo và phụ kiện

**Áp dụng cho:** `rental_management_po_specification_vi.md` v1.0 (04/11/2026)\
**Ngày:** 05/11/2026\
**Nguồn:** phiên brainstorm BMAD `_bmad-output/brainstorming/brainstorm-rental-spec-gaps-2026-11-05/`\
**Trạng thái:** Đã chốt làm baseline. Dự án đang ở giai đoạn ý tưởng, chưa có PO/Owner thực tế, nên các mục ghi **[Đã chốt – BP]** được chốt theo best practice phổ biến của ngành cho thuê thời trang hiện nay. Các giá trị này vẫn có thể điều chỉnh khi có dữ liệu vận hành thật, và mọi tham số số đều được thiết kế để cấu hình được (M15).

Phụ lục này không thay thế spec v1.0. Mỗi mục ghi rõ phần spec mà nó **bổ sung**, **sửa** hoặc **thay thế**. Nếu có mâu thuẫn, phụ lục được ưu tiên hơn v1.0. Mục 11 bổ sung các thực hành chuẩn của ngành mà v1.0 chưa đề cập.

------------------------------------------------------------------------

# 0. Ba nguyên nhân gốc

Phần lớn gap trong spec v1.0 bắt nguồn từ ba nguyên nhân sau. Sửa ở gốc sẽ đóng được nhiều gap cùng lúc.

| # | Nguyên nhân gốc | Hướng xử lý |
|---|---|---|
| R1 | Spec trộn **trạng thái vật lý của tài sản** với **khả dụng theo lịch**. Ví dụ: `RESERVED` vừa là trạng thái vừa là lịch. | Tách thành hai trục. *Asset Status* chỉ mô tả món đồ đang ở đâu và trong tình trạng gì. *Availability* luôn được suy ra từ Reservation và Asset Block (mục 2). |
| R2 | Tài chính được mô tả bằng **trường trạng thái** chứ không bằng **sổ cái**. | Mọi số dư (cọc đang giữ, công nợ, đã thu) đều suy ra từ ledger giao dịch. Trạng thái cọc và công nợ là giá trị tính toán, không nhập tay (mục 4). |
| R3 | MVP được cắt theo **tên module**, không theo **luồng end-to-end**. | Đã có thu cọc nhưng hoàn cọc nằm ở Phase 2, nên không đơn nào hoàn tất được. Tương tự, đã có `CLEANING` nhưng module vệ sinh ở Phase 2, nên tài sản bị kẹt. Cần cắt lại MVP theo một luồng mỏng nhưng khép kín (mục 7). |

------------------------------------------------------------------------

# 1. Vòng đời & State machine

## 1.1. Asset Status — *sửa M03.04*

**Bỏ** `RESERVED` khỏi trạng thái tài sản. Việc món đồ có lịch giữ hay không được xác định hoàn toàn từ Reservation.

**Thêm** hai trạng thái:
- `INTAKE`: món mới nhập hoặc mới tạo, chưa qua kiểm tra ngoại quan. Không khả dụng.
- `AT_VENDOR`: món đang ở tiệm giặt hoặc tiệm sửa bên ngoài.

**Giữ lại** `PREPARING` với nghĩa hẹp: món đã được lấy ra khỏi kệ và gắn với một đơn cụ thể.

| Từ \ Đến | AVAILABLE | PREPARING | RENTED | RETURNED_PENDING_INSPECTION | CLEANING | REPAIRING | AT_VENDOR | DAMAGED | LOST | RETIRED |
|---|---|---|---|---|---|---|---|---|---|---|
| INTAKE | Nghiệm thu nhập | — | — | — | ✓ | ✓ | — | ✓ | — | ✓ |
| AVAILABLE | — | Chuẩn bị đơn | Bàn giao nhanh¹ | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| PREPARING | Hủy/đổi món² | — | Bàn giao | — | — | — | — | ✓ | — | — |
| RENTED | — | — | — | Nhận trả | — | — | — | — | Quá ngưỡng (1.6) | — |
| RETURNED_PENDING_INSPECTION | Đạt + danh mục không cần giặt³ | — | — | — | Kiểm tra | Kiểm tra | ✓ | Kiểm tra | Kiểm tra | — |
| CLEANING | Hoàn tất vệ sinh | — | — | — | — | ✓ | ✓ | ✓ | — | — |
| REPAIRING / AT_VENDOR | Nghiệm thu đạt | — | — | — | ✓ | ✓ | ✓ | Không đạt | — | ✓ |
| DAMAGED | — | — | — | — | — | ✓ | ✓ | — | — | ✓ |
| LOST | — | — | — | Tìm thấy (4.7) | — | — | — | — | — | ✓ |

¹ Chỉ dùng cho luồng khách vãng lai (mục 3.6). Hệ thống vẫn ghi nhận bước PREPARING ngầm.\
² Tài sản quay về `AVAILABLE` nếu được trả lại kệ, có ghi audit.\
³ Cấu hình theo danh mục bằng cờ `requires_cleaning`.

**Quy tắc:**
- Các chuyển trạng thái không có trong bảng đều bị **chặn**.
- Chuyển trạng thái thủ công, tức là không đi qua một nghiệp vụ, cần quyền `asset.status.override`, phải có lý do và ghi audit log.
- Chuyển thủ công sang `AVAILABLE` từ `CLEANING`, `REPAIRING`, `DAMAGED` hoặc `AT_VENDOR` cần quyền Manager trở lên.
- **Không cho chuyển** sang `LOST`, `RETIRED` hoặc `DAMAGED` khi món còn reservation tương lai. Người dùng phải xử lý trước bằng luồng đổi món (3.2); xem mục 2.6.

## 1.2. Reservation — *bổ sung mới*

```
HOLD ──(xác nhận đơn)──► CONFIRMED ──(bàn giao dòng)──► CONSUMED
  │                         │
  ├─(hết expires_at)──► EXPIRED     ├─(hủy/đổi món)──► RELEASED
  └─(hủy/đổi món)────► RELEASED
```

- Reservation được tạo cho **từng dòng hàng** (một Inventory Item). `expires_at` đặt ở cấp đơn và áp dụng cho mọi HOLD của đơn đó.
- **HOLD có tính độc quyền**: HOLD chặn availability cho tới `expires_at`. Hai đơn không thể cùng HOLD một món trong các khoảng chồng lấn. Quy tắc này thay cho cách hiểu "chỉ kiểm tra khi xác nhận".
- Có thể gia hạn HOLD. **[Đã chốt – BP]** Đề xuất: tối đa 2 lần, mỗi lần dài bằng thời hạn mặc định. Gia hạn từ lần thứ 3 trở đi cần quyền Manager.
- Lúc tạo Reservation, hệ thống lưu snapshot `buffer_before` và `buffer_after` vào chính reservation đó. Nếu sau này đổi cấu hình buffer thì chỉ reservation mới bị ảnh hưởng.

## 1.3. Rental Order Item — *bổ sung mới*

```
PENDING ─► ALLOCATED ─► PREPARED ─► HANDED_OVER ─► RETURNED ─► INSPECTED
   │           │            │             │
   └───────────┴────────────┴─► CANCELLED └─► LOST
```

| Trạng thái | Ý nghĩa |
|---|---|
| PENDING | Dòng ở cấp Variant, chưa gán món cụ thể. Chỉ có ở đơn DRAFT. |
| ALLOCATED | Đã gán Inventory Item và có Reservation HOLD hoặc CONFIRMED. |
| PREPARED | Checklist chuẩn bị đã đạt. |
| HANDED_OVER | Đã bàn giao, có ảnh, có xác nhận và có thời điểm thực tế. |
| RETURNED | Đã nhận trả và có thời điểm thực tế. |
| INSPECTED | Đã có kết quả kiểm tra. Các phí phát sinh của dòng đã được tạo, kể cả phí 0đ. |
| CANCELLED | Dòng bị hủy hoặc bị thay bằng dòng khác (đổi món). |
| LOST | Khách làm mất hoặc quá ngưỡng quá hạn. |

## 1.4. Rental Order Status — *sửa M06.03*

Trạng thái đơn được **suy ra từ trạng thái các dòng hàng** (không tính dòng CANCELLED). Không ai được set tay, ngoại trừ `DRAFT`, `CANCELLED` và `COMPLETED`.

| Trạng thái đơn | Điều kiện |
|---|---|
| DRAFT | Chưa có HOLD, hoặc HOLD đã hết hạn. |
| PENDING_CONFIRMATION | Mọi dòng đã ALLOCATED và có HOLD còn hiệu lực. |
| CONFIRMED | Mọi reservation đã CONFIRMED và chưa dòng nào PREPARED. |
| PREPARING | Có ít nhất một dòng PREPARED nhưng chưa phải tất cả. |
| READY_FOR_HANDOVER | Mọi dòng đã PREPARED. |
| PARTIALLY_HANDED_OVER | Có dòng HANDED_OVER nhưng chưa phải tất cả. |
| RENTED | Mọi dòng đã HANDED_OVER. |
| PARTIALLY_RETURNED | Có dòng RETURNED hoặc LOST nhưng chưa phải tất cả. |
| RETURNED | Mọi dòng đã RETURNED, INSPECTED hoặc LOST. |
| COMPLETED | Đạt toàn bộ điều kiện ở mục 1.5. Hệ thống tự chuyển, hoặc Accountant bấm hoàn tất. |
| CANCELLED | Hủy đơn hợp lệ (mục 3.5). |

- Khi HOLD hết hạn, đơn PENDING_CONFIRMATION quay về **DRAFT** và giữ nguyên các dòng. Các dòng chuyển về PENDING. Sales có thể HOLD lại nếu món còn trống.
- `OVERDUE` là **cờ** chứ không phải trạng thái (đây là chốt cho khuyến nghị ở M06.03). Cờ được bật khi `now > planned_return + grace_period` và đơn vẫn còn dòng HANDED_OVER.
- Thêm cờ `AT_RISK` (xem 2.5).

## 1.5. Điều kiện COMPLETED — *bổ sung WF05*

Đơn chỉ được COMPLETED khi đạt **tất cả** điều kiện sau:
1. Mọi dòng không bị hủy đã ở INSPECTED hoặc LOST.
2. Mọi phí phát sinh (trễ, hư hỏng, mất) đã ở trạng thái chốt: đã chấp nhận, đã miễn có duyệt, hoặc đã giải quyết tranh chấp.
3. Số dư cọc bằng 0, nghĩa là cọc đã được hoàn, khấu trừ hoặc cấn trừ hết.
4. Công nợ của đơn bằng 0, hoặc đã được **xóa nợ (write-off)** bởi người có quyền `receivable.writeoff`.

Đơn đã COMPLETED vẫn có thể phát sinh giao dịch điều chỉnh, ví dụ khi hư hỏng phát hiện muộn (4.6). Trong trường hợp đó, đơn được mở lại ở dạng cờ `REOPENED_FINANCE` và trạng thái vận hành giữ nguyên.

## 1.6. Quá hạn và leo thang — *bổ sung M09.04*

| Tham số | Đề xuất mặc định **[Đã chốt – BP]** |
|---|---|
| `grace_period` (ân hạn) | 60 phút |
| Nhắc quá hạn | Ngay khi quá hạn, sau đó mỗi 24 giờ |
| Ngưỡng nghi thất lạc | Quá hạn 7 ngày và không liên lạc được. Manager xác nhận để chuyển dòng sang LOST. |
| Trần phí trễ | Không vượt giá trị đền bù của món |

------------------------------------------------------------------------

# 2. Availability engine — *bổ sung M07, BR-002, BR-003*

## 2.1. Khoảng chiếm dụng (occupancy)

```
occupancy_start = planned_handover − buffer_before   (lưu snapshot)
occupancy_end   = planned_return   + buffer_after     (lưu snapshot)
```

- Hai khoảng occupancy **không được chồng lấn**, kể cả phần buffer. Công thức chồng lấn của spec vẫn giữ: `a.start < b.end AND a.end > b.start`. Hai khoảng chạm nhau ở biên được coi là hợp lệ.
- Chỉ các reservation ở trạng thái `HOLD` (còn hạn), `CONFIRMED` hoặc `CONSUMED` (dòng đang được thuê) mới chặn availability.
- Asset Block (2.3) cũng chặn availability.
- Asset Status `LOST`, `RETIRED`, `DAMAGED` và `INTAKE` chặn availability **vô thời hạn**.

## 2.2. Thời gian tính tiền và thời gian chiếm dụng

- Occupancy luôn tính theo **giờ kế hoạch** (planned), không theo giờ thực tế.
- Khách nhận hàng trễ thì **không** được dời hạn trả. **[Đã chốt – BP]**
- Phí trễ tính theo `actual_return − planned_return − grace_period`.
- Khách trả sớm: mặc định **không hoàn** tiền thuê **[Đã chốt – BP]**. Occupancy được rút ngắn khi dòng chuyển RETURNED, để món sớm có thể cho thuê lại.

## 2.3. Asset Block — *thực thể mới*

`asset_id`, `start`, `end` (có thể để trống, nghĩa là mở), `reason` (MAINTENANCE / REPAIR / CLEANING / PHOTOSHOOT / OTHER), `source_ref` (phiếu sửa, phiếu vệ sinh…), `created_by`.

- Phiếu sửa chữa hoặc vệ sinh tự tạo Block. `end` bằng ngày dự kiến hoàn thành (ETA). Nếu chưa có ETA thì Block **mở**, tức là chặn vô thời hạn.
- Khi Block mở hoặc ETA bị lùi mà chạm vào một reservation tương lai, hệ thống phát sinh cảnh báo theo mục 2.6.

## 2.4. Ràng buộc kỹ thuật chống trùng

- Bảng occupancy (gồm reservation và block) phải có **exclusion constraint ở tầng DB** trên `(asset_id, tstzrange(start,end))`, hoặc dùng cơ chế khóa tương đương. Ràng buộc áp dụng **ngay khi tạo HOLD**, không chỉ khi xác nhận.
- Khi xác nhận đơn, hệ thống kiểm tra lại toàn bộ trong cùng một transaction (BR-004 giữ nguyên).

## 2.5. Cờ AT_RISK (đơn kế tiếp có nguy cơ)

Một đơn B được gắn cờ `AT_RISK` khi đơn A trước đó trên cùng tài sản đang OVERDUE và:
`now + buffer_after(A) > occupancy_start(B)`. Nói cách khác, nếu A trả ngay bây giờ thì vẫn không kịp chuẩn bị cho B.

Khi cờ bật, hệ thống tạo **task bắt buộc** cho Sales và Manager (không chỉ gửi thông báo), kèm CTA "Đổi món" (3.2).

## 2.6. Tài sản hỏng hoặc mất ảnh hưởng lịch tương lai — *sửa BR-019*

Khi tài sản chuyển sang DAMAGED, REPAIRING, LOST hoặc RETIRED, hoặc khi có Block mở mà vẫn còn reservation tương lai:
1. Hệ thống liệt kê mọi đơn bị ảnh hưởng.
2. Mỗi đơn nhận một **task "Cần đổi món"**. Task có danh sách món thay thế cùng variant và còn trống, sắp xếp theo mục 3.7.
3. Task chỉ tự đóng khi dòng hàng đã được gán món khác, hoặc đơn đã hủy.

## 2.7. Hiển thị khả dụng

- Kết quả hiển thị **gộp theo variant** trước, ví dụ "Size M – Đỏ: 3/5 khả dụng". Người dùng có thể mở rộng để xem từng món kèm lý do không khả dụng.
- "Thời gian gần nhất có thể thuê" = min(`occupancy_end` của khoảng đang chặn, ETA của Block). Nếu Block mở thì hiển thị "chưa xác định".

------------------------------------------------------------------------

# 3. Luồng thay đổi đơn — *bổ sung M06*

## 3.1. Gia hạn thuê (Extension) — **thiếu hoàn toàn trong v1.0**

- Áp dụng cho đơn ở trạng thái RENTED hoặc PARTIALLY_RETURNED, ở cấp từng dòng hoặc toàn đơn.
- Các bước: nhập hạn trả mới → kiểm tra occupancy mới không chồng lấn → tính tiền thêm theo bảng giá snapshot → hiển thị chênh lệch → xác nhận.
- Nếu chồng lấn với đơn kế tiếp, hệ thống **chặn**, kèm thông tin đơn nào bị chặn. Manager có thể ép gia hạn nếu đồng thời đổi món cho đơn kế tiếp.
- Cọc bổ sung: **[Đã chốt – BP]** mặc định không yêu cầu.
- Gia hạn tạo một **phiên bản snapshot mới** của dòng hàng và giữ lại lịch sử phiên bản cũ.

## 3.2. Đổi món trước bàn giao (Reassign)

- Thay Inventory Item của một dòng bằng món khác.
- Cùng variant: không đổi giá.
- Khác variant: tính lại giá và hiển thị chênh lệch.
- Reservation cũ chuyển RELEASED, reservation mới được tạo và kiểm tra trong cùng một transaction.
- Đây cũng là luồng dùng để giải quyết task ở 2.5 và 2.6.

## 3.3. Đổi size/mẫu sau bàn giao — *chốt DEC-011*

- **Không sửa dòng gốc.** Dòng cũ đi qua luồng nhận trả và kiểm tra bình thường. Sau đó thêm một dòng mới vào cùng đơn, dòng mới cũng phải kiểm tra khả dụng.
- Tiền thuê: **[Đã chốt – BP]** mặc định áp giá dòng mới cho thời gian còn lại, dòng cũ tính đến lúc trả.

## 3.4. Khách không đến nhận (No-show)

- Sau `planned_handover + no_show_threshold` (**[Đã chốt – BP]**, đề xuất 24 giờ), hệ thống tạo task cho Sales.
- Sales có thể hủy đơn với lý do chuẩn `NO_SHOW`. Việc giữ hoặc hoàn cọc áp theo chính sách hủy (3.5).
- Hệ thống không tự hủy đơn.

## 3.5. Hủy đơn — *sửa M06.06*

- **Chặn hủy** khi đơn đã có ít nhất một dòng HANDED_OVER. Trường hợp này phải dùng luồng nhận trả sớm.
- Danh mục lý do hủy được chuẩn hóa: KHÁCH_HỦY, NO_SHOW, HẾT_HÀNG, HƯ_HỎNG_TÀI_SẢN, SAI_SÓT_NỘI_BỘ, KHÁC (bắt buộc ghi chú).
- Chính sách hoàn theo khoảng cách tới giờ nhận **[Đã chốt – BP]**. Ví dụ: hủy trước 72 giờ hoàn 100% tiền thuê; từ 24 đến 72 giờ hoàn 50%; dưới 24 giờ hoặc no-show không hoàn tiền thuê. Tiền cọc luôn hoàn, trừ khi chính sách quy định giữ.
- Tài sản đang PREPARING quay về `AVAILABLE` sau khi Warehouse xác nhận đã trả lại kệ.

## 3.6. Fast-path khách vãng lai

Toàn bộ luồng chạy trên một màn hình: chọn hoặc tạo khách → quét QR món → hệ thống tự lấy thời gian bắt đầu là hiện tại → nhập hạn trả → thu tiền và cọc → bàn giao. Ở tầng dữ liệu, luồng này vẫn đi đủ các trạng thái và kiểm tra availability như bình thường.

## 3.7. Dòng hàng cấp Variant và gợi ý món

- Ở trạng thái DRAFT, dòng hàng có thể chỉ chọn Variant. Khi HOLD, hệ thống tự gán món cụ thể, người dùng có thể đổi lại.
- Thứ tự gợi ý món: món vừa khít khoảng trống lịch nhất (giảm phân mảnh) → món có ít lượt thuê hơn → món có vị trí gần hơn.

## 3.8. Sửa đơn đã xác nhận — *bổ sung M06.05*

Mọi thay đổi về thời gian, tài sản, giá hoặc cọc đều tạo một **phiên bản snapshot mới**, có diff trước/sau và người duyệt nếu thay đổi vượt hạn mức. Giảm giá đã được duyệt sẽ phải duyệt lại nếu tổng tiền thay đổi.

------------------------------------------------------------------------

# 4. Tài chính — sổ cái — *sửa M11*

## 4.1. Giao dịch

- Trạng thái giao dịch: `PENDING` → `CONFIRMED` → (`REVERSED` bằng một giao dịch đối ứng).
- Chuyển khoản luôn bắt đầu ở `PENDING`, cho tới khi có người xác minh tiền đã về.
- Tiền mặt và thẻ được tạo ở `CONFIRMED`.
- Chỉ giao dịch CONFIRMED mới được tính vào số dư.

## 4.2. Phân bổ thanh toán

Một lần thu (`payment`) có thể được phân bổ (`allocation`) vào nhiều khoản, ví dụ 2.000.000đ = 1.200.000đ tiền thuê + 800.000đ cọc. Báo cáo doanh thu chỉ đọc phần allocation loại tiền thuê.

Ghi nhận cả hai số: *số tiền khách đưa* và *số tiền áp dụng*. Tiền thối lại không phải là giao dịch.

## 4.3. Loại giao dịch (bổ sung)

Ngoài các loại đã có ở M11.02, thêm các loại sau:
- **Cấn trừ cọc** (`DEPOSIT_APPLY`): chuyển tiền cọc sang trả tiền thuê hoặc phí phát sinh còn thiếu.
- **Giữ lại cọc** (`DEPOSIT_HOLDBACK`): tạm giữ một phần cọc trong lúc chờ kiểm tra chuyên sâu.
- **Tịch thu cọc** (`DEPOSIT_FORFEIT`): áp dụng khi no-show hoặc vi phạm, ghi nhận là *thu nhập khác*, không tính vào doanh thu thuê.
- **Xóa nợ** (`WRITE_OFF`).

## 4.4. Trạng thái cọc — *thay thế M11.03*

Trạng thái cọc được **suy ra** từ ledger:
`số dư cọc = thu cọc − hoàn cọc − cấn trừ − khấu trừ − tịch thu`.

Các nhãn hiển thị (Chưa thu / Thu một phần / Đang giữ / Giữ lại chờ kiểm tra / Đã quyết toán) được tính từ số dư này, không ai nhập tay.

## 4.5. Thứ tự quyết toán (WF05)

1. Tổng hợp mọi phí đã chốt của đơn.
2. Cấn trừ phí vào tiền cọc.
3. Nếu cọc **không đủ**, phần thiếu thành **công nợ** (M11.04). Hạn thanh toán **[Đã chốt – BP]**, đề xuất 7 ngày.
4. Nếu cọc **còn dư**, hoàn cho khách. Phương thức hoàn **[Đã chốt – BP]**: mặc định giống lúc thu; nếu hoàn bằng phương thức khác thì phải ghi thông tin tài khoản nhận.

## 4.6. Hoàn cọc tại quầy và hư hỏng phát hiện muộn

- Cho phép hoàn cọc ngay sau khi kiểm tra nhanh. Với danh mục có `holdback_percent` (**[Đã chốt – BP]**, ví dụ váy cưới giữ lại 20%), hệ thống giữ lại phần tương ứng cho tới khi vệ sinh xong hoặc hết `inspection_window` (đề xuất 48 giờ).
- Hư hỏng phát hiện sau khi cọc đã quyết toán nhưng vẫn trong `inspection_window`: tạo phí phát sinh và công nợ, gắn cờ `REOPENED_FINANCE` (1.5). Quá `inspection_window` thì cửa hàng tự chịu chi phí, ghi vào chi phí sửa chữa của tài sản.

## 4.7. Mất tài sản và tìm thấy lại

- Dòng hàng chuyển LOST, tài sản chuyển LOST. Phí đền bù bằng `replacement_value` của món **[Đã chốt – BP]**, mặc định là giá vốn.
- Nếu tìm thấy lại: tài sản chuyển RETURNED_PENDING_INSPECTION. Phí đền bù được **đảo** bằng giao dịch điều chỉnh (hoàn toàn bộ hoặc một phần theo kết quả kiểm tra), và phí trễ được tính lại.

## 4.8. Tranh chấp phí hư hỏng

Phí ở trạng thái `PROPOSED` có thể chuyển sang `DISPUTED`. Trong lúc tranh chấp, hệ thống tự giữ lại phần cọc tương ứng. Manager chốt kết quả (`ACCEPTED`, `REDUCED` hoặc `WAIVED`), kèm ghi chú và bộ ảnh trước/sau.

## 4.9. Quy tắc số — *bổ sung NFR 9.2*

- Làm tròn từng dòng tiền tới **1.000đ** theo quy tắc half-up **[Đã chốt – BP]**. Tổng đơn bằng tổng các dòng đã làm tròn.
- Giá đã **bao gồm thuế**. MVP không xuất hóa đơn VAT.
- Giảm giá ở cấp đơn được **phân bổ pro-rata** theo giá dòng, để báo cáo doanh thu theo tài sản đúng (M13.03).

## 4.10. Ghi nhận doanh thu — *bổ sung M13.01*

Báo cáo có hai góc nhìn và phải ghi rõ đang dùng góc nào:
- **Thực thu (cash):** theo ngày giao dịch CONFIRMED, dùng để đối chiếu quỹ.
- **Doanh thu kỳ (accrual):** phân bổ tiền thuê theo các ngày của kỳ thuê, dùng cho hiệu suất tài sản.

Dashboard tách riêng "tiền thuê đã thu" và "tiền cọc đã thu". Không được gộp thành một số "tiền thu trong ngày".

------------------------------------------------------------------------

# 5. Giá & chính sách

| Mục | Quy tắc đề xuất |
|---|---|
| Thứ tự ưu tiên giá | Item override > Variant > Product (bảng giá mặc định) |
| Đơn vị tính (DEC-001) | **[Đã chốt – BP]** Đề xuất: theo **ngày lịch**, nhận và trả theo giờ mặc định của cửa hàng. Cho phép gói **theo giờ** cho danh mục chụp ảnh. |
| Thời lượng tối thiểu | 1 đơn vị tính |
| Bậc giá (Q17) | SHOULD: bậc theo thời lượng (1 / 3 / 7 ngày). COULD: giá mùa cao điểm có ngày hiệu lực. |
| Đặt trước tối đa (Q3) | **[Đã chốt – BP]** Đề xuất 6 tháng |
| Cọc (Q5) | **[Đã chốt – BP]** Đề xuất theo **từng món**: max(số cố định của variant, % giá vốn). Cọc đơn = tổng cọc các món. |
| Giờ mở cửa | Giờ hẹn nhận và trả phải nằm trong giờ mở cửa. Ngoài giờ cần quyền override (liên quan DEC-010). |
| Khách có công nợ (DEC-006) | Hiện cảnh báo. Muốn xác nhận đơn mới phải có quyền `order.confirm_with_debt`. |
| Khách BLOCKED | Không tạo được đơn mới. Trạng thái khách gồm ACTIVE / WARNING / BLOCKED / INACTIVE. |

------------------------------------------------------------------------

# 6. Quyền, phê duyệt, phụ kiện, task, dữ liệu

## 6.1. Danh mục permission — *thay "Theo quyền" ở 4.2*

Các ô ghi "Theo quyền" trong ma trận 4.2 được thay bằng danh mục permission cụ thể, gán được cho từng vai trò. Danh mục tối thiểu:

`order.create`, `order.hold.extend`, `order.confirm`, `order.confirm_with_debt`, `order.cancel`, `order.edit_after_confirm`, `price.override`, `discount.within_limit`, `discount.over_limit`, `fee.waive`, `payment.record`, `payment.verify_transfer`, `deposit.refund`, `deposit.deduct`, `receivable.writeoff`, `asset.create`, `asset.status.override`, `asset.retire`, `handover.perform`, `return.perform`, `inspection.perform`, `repair.qc`, `stocktake.approve`, `report.finance.view`, `customer.pii.view`, `settings.manage`, `staff.manage`.

Ngoài ra, cần tách Owner và Admin thành hai cột riêng. **[Đã chốt – BP]** Admin có được xem báo cáo tài chính hay không.

## 6.2. Approval Request — *thực thể chung*

Dùng chung cho các nghiệp vụ: giảm giá vượt hạn mức, miễn phí, khấu trừ cọc vượt ngưỡng, override buffer hoặc giờ mở cửa, chuyển trạng thái tài sản thủ công, gia hạn HOLD lần 3 trở đi.

- Trạng thái: PENDING → APPROVED / REJECTED / EXPIRED.
- Có hai cách duyệt: duyệt từ xa qua thông báo, hoặc Manager **nhập PIN** ngay trên thiết bị tại quầy.

## 6.3. Phụ kiện đi kèm — *chốt Q15*

- **MVP:** phụ kiện là **danh sách cấu thành theo Variant** (tên, số lượng, giá đền bù). Phụ kiện xuất hiện tự động trong checklist bàn giao và nhận trả. Thiếu phụ kiện thì hệ thống tạo phí đền bù.
- Phụ kiện có giá trị cao (túi, giày) nên được quản lý như một Inventory Item riêng.
- Set/combo (Q16) để sang Phase 2.

## 6.4. Task — *sửa M01.02, M12*

- Task được **suy ra từ trạng thái**. Ví dụ: một dòng RETURNED sinh task "Cần kiểm tra"; dòng chuyển INSPECTED thì task **tự đóng**.
- Người dùng không cần đóng task bằng tay, ngoại trừ các task kiểu "ghi chú bàn giao ca".
- Thông báo được dedupe theo cặp (đối tượng, loại) và nhắc theo chu kỳ cấu hình được.

## 6.5. Snapshot & toàn vẹn — *bổ sung BR-012*

Lúc xác nhận đơn, hệ thống lưu snapshot các thông tin sau: giá, giảm giá, cọc, chính sách hủy, chính sách trễ, chính sách hư hỏng, **thuộc tính variant** (tên, size, màu), **buffer** và **phiên bản checklist**. Mỗi lần kiểm tra cũng ghi lại `checklist_version` đã dùng.

## 6.6. Thời gian

Mọi tính toán (quá hạn, ngày tính tiền, KPI "hôm nay") đều theo **múi giờ cửa hàng** (Asia/Ho_Chi_Minh). Dữ liệu được lưu dạng timestamptz.

Định nghĩa KPI trên dashboard:
- "Đơn thuê hôm nay" là đơn có `planned_handover` trong ngày. Đơn *tạo* trong ngày là một KPI riêng.
- "Doanh thu" là tiền thuê đã thu (4.10), không gồm cọc.

## 6.7. Nhập hàng — *giải quyết mâu thuẫn WF01 và M04.01*

- Ở bước 4–5 của WF01, Inventory Item được tạo với trạng thái `INTAKE`, có mã và QR ngay để in tem.
- Khi phiếu nhập được xác nhận (bước 7), các món đạt chuyển sang `AVAILABLE`. Món lỗi, thiếu hoặc sai variant chuyển `DAMAGED`, hoặc bị loại khỏi phiếu.
- Chi phí bổ sung của phiếu được **phân bổ vào giá vốn từng món theo tỷ lệ đơn giá**.
- Với MVP (khi chưa có phiếu nhập), món tạo trực tiếp (M03.02) cũng bắt đầu ở `INTAKE`. Người dùng bấm "Đưa vào khai thác" sau khi chụp ảnh và gán vị trí.

## 6.8. Kiểm kê — *bổ sung M04.03*

Danh sách kỳ vọng **loại trừ** các món đang RENTED hoặc AT_VENDOR, và các món này được hiển thị ở một nhóm riêng. Món quét được nhưng hệ thống ghi đang RENTED được gắn cờ "bất thường" để Manager xử lý.

## 6.9. Bàn giao / nhận trả — *bổ sung M08, M09*

- Quét QR một món không thuộc đơn đang mở: hệ thống **chặn** và hiển thị "món này thuộc đơn X / đang ở trạng thái Y".
- Khi nhận trả, hệ thống tự định tuyến món về đúng đơn của khách nếu khách có nhiều đơn.
- Xác nhận của khách ở MVP: **chữ ký trên màn hình** cùng họ tên người nhận hoặc người trả thực tế (có thể là người được ủy quyền).
- Màn hình kiểm tra hiển thị **ảnh trước thuê và sau thuê cạnh nhau**.
- Ảnh được nén ở phía thiết bị và upload chạy nền. Upload thất bại không được chặn việc lưu tiến trình bàn giao.

## 6.10. Bảo mật bổ sung — *bổ sung 9.1*

- Khóa tài khoản sau 5 lần đăng nhập sai. Timeout phiên **[Đã chốt – BP]**. Owner bắt buộc dùng 2FA.
- Ghi audit cả các lượt **xem** ảnh giấy tờ của khách, không chỉ các lượt sửa.
- URL trong QR yêu cầu đăng nhập và không lộ dữ liệu ra ngoài.
- Trước khi khóa tài khoản một nhân viên nghỉ việc, phải chuyển giao các đơn và task họ đang phụ trách.

## 6.11. Mapping kết quả kiểm tra → trạng thái — *sửa M09.03*

Gộp hai kết quả "Đạt" trong spec thành một.

| Kết quả | Asset Status | Tác động |
|---|---|---|
| Đạt | `CLEANING`, hoặc `AVAILABLE` nếu danh mục có `requires_cleaning = false` | — |
| Cần vệ sinh đặc biệt | `CLEANING` hoặc `AT_VENDOR` | Có thể phát sinh phí vệ sinh đặc biệt |
| Cần sửa chữa | `REPAIRING` | Tạo phiếu sửa và Asset Block (2.3) |
| Hư hỏng cần xác minh | `DAMAGED` | Phí PROPOSED (4.8) |
| Mất tài sản/phụ kiện | `LOST` (với tài sản) | Phí đền bù (4.7, 6.3) |

------------------------------------------------------------------------

# 7. Điều chỉnh phạm vi MVP — *sửa mục 11.1*

Mục tiêu: MVP chạy được **trọn một vòng** từ nhập món, thuê, trả, kiểm tra, vệ sinh, quyết toán cọc đến COMPLETED.

| Thay đổi | Lý do |
|---|---|
| **Thêm** vệ sinh-lite: nút "Hoàn tất vệ sinh" chuyển tài sản từ CLEANING sang AVAILABLE, chưa cần phiếu vệ sinh đầy đủ | Nếu không có, tài sản kẹt ở CLEANING (R3) |
| **Thêm** hoàn cọc, khấu trừ cọc, cấn trừ cơ bản, phí trễ và hư hỏng | Nếu không có, không đơn nào COMPLETED được |
| **Thêm** job nền giải phóng HOLD hết hạn, có retry và log | Reservation có thời hạn đã là MVP #6 nhưng job lại nằm ở Phase 2 |
| **Thêm** gia hạn thuê (3.1), đổi món trước bàn giao (3.2), task "Cần đổi món" (2.6) | Luồng thường gặp, rủi ro cao |
| **Thêm** sửa chữa-lite: trạng thái REPAIRING kèm Asset Block có ETA, chưa cần nghiệm thu đầy đủ | Repair phải chặn availability ngay từ MVP |
| **Bỏ** "phương thức nhận/trả" ở bước 2 tạo đơn | Giao hàng nằm ngoài MVP (DEC-014) |
| **Giữ Phase 2** | Phiếu nhập và kiểm kê, phiếu vệ sinh/sửa đầy đủ, thông báo đa kênh, tranh chấp phí, Approval từ xa (MVP chỉ dùng PIN tại quầy) |
| **Won't (MVP)** | Offline mode, đa chi nhánh, ký điện tử có giá trị pháp lý, set/combo, cọc thế chấp giấy tờ (Phase 2: bản ghi phi tiền mặt), quỹ tiền mặt theo ca |

------------------------------------------------------------------------

# 8. Business Rules bổ sung

| Mã | Quy tắc |
|---|---|
| BR-021 | Asset Status chỉ mô tả trạng thái vật lý. Khả dụng luôn suy ra từ Reservation và Asset Block. |
| BR-022 | Reservation HOLD có tính độc quyền và có hiệu lực chặn ngay khi tạo. |
| BR-023 | Buffer được snapshot vào reservation. Đổi cấu hình buffer không ảnh hưởng reservation đã có. |
| BR-024 | Trạng thái đơn được suy ra từ trạng thái dòng hàng. |
| BR-025 | Không hủy đơn đã có dòng bàn giao. Dùng luồng trả sớm thay thế. |
| BR-026 | Đơn quá hạn kéo dài occupancy động và gắn cờ AT_RISK cho đơn kế tiếp. |
| BR-027 | Không chuyển tài sản sang LOST, RETIRED hoặc DAMAGED khi còn reservation tương lai chưa xử lý. |
| BR-028 | Số dư cọc và công nợ được suy ra từ ledger, không nhập tay. |
| BR-029 | Phí vượt cọc trở thành công nợ. Cọc tịch thu là thu nhập khác, không phải doanh thu thuê. |
| BR-030 | Chuyển khoản chỉ được tính vào số dư sau khi có người xác minh. |
| BR-031 | Giảm giá cấp đơn được phân bổ pro-rata về dòng hàng. |
| BR-032 | Mọi thay đổi sau xác nhận tạo phiên bản snapshot mới và lưu diff. |
| BR-033 | Task được suy ra từ trạng thái và tự đóng khi điều kiện không còn. |
| BR-034 | Mọi tính toán thời gian theo múi giờ cửa hàng. |

------------------------------------------------------------------------

# 9. Quyết định đã chốt (bổ sung mục 12)

Các quyết định DEC-001 đến DEC-015 của spec v1.0 cũng được chốt theo cột "Đề xuất mặc định" của v1.0, kèm các điều chỉnh sau:
- **DEC-001:** tính theo ngày lịch.
- **DEC-003:** đơn DRAFT không giữ hàng.
- **DEC-004:** HOLD hết hạn sau 24 giờ.
- **DEC-005:** bắt buộc thu tối thiểu 30% tiền thuê để xác nhận; cọc thu khi bàn giao.
- **DEC-013:** chỉ lưu số CCCD và ảnh khi khách đồng ý (xem 11.7).
- **DEC-014:** giao hàng để Phase 2.
- **DEC-015:** MVP chạy một cửa hàng, nhưng dữ liệu có sẵn `store_id`.

| Mã | Câu hỏi | Quyết định |
|---|---|---|
| DEC-016 | Ân hạn trả trễ | 60 phút |
| DEC-017 | Giới hạn số lần gia hạn HOLD | 2 lần; từ lần 3 cần Manager duyệt |
| DEC-018 | Ngưỡng no-show | 24 giờ sau giờ hẹn |
| DEC-019 | Chính sách hoàn tiền khi hủy theo mốc thời gian | 100% / 50% / 0% ở các mốc 72 giờ / 24 giờ |
| DEC-020 | Trả sớm có hoàn tiền không | Không |
| DEC-021 | Nhận hàng trễ có dời hạn trả không | Không |
| DEC-022 | Ngưỡng quá hạn coi là thất lạc | 7 ngày + Manager xác nhận |
| DEC-023 | Trần phí trễ | ≤ giá trị đền bù |
| DEC-024 | Giữ lại cọc (hold-back) và cửa sổ kiểm tra | Theo danh mục; 48 giờ |
| DEC-025 | Phương thức hoàn cọc | Giống lúc thu |
| DEC-026 | Hạn thanh toán công nợ | 7 ngày |
| DEC-027 | Làm tròn | 1.000đ, half-up, theo dòng |
| DEC-028 | Công thức cọc | max(cố định variant, % giá vốn) theo món |
| DEC-029 | Đặt trước tối đa | 6 tháng |
| DEC-030 | Gia hạn thuê có yêu cầu cọc bổ sung | Không |
| DEC-031 | Giá trị đền bù khi mất | Giá vốn của món |
| DEC-032 | Admin có xem báo cáo tài chính | Không (chỉ Owner và Accountant) |

------------------------------------------------------------------------

# 10. Truy vết gap → mục xử lý

| Phần spec v1.0 | Gap | Mục xử lý |
|---|---|---|
| M03.04 | `RESERVED` mâu thuẫn với nguyên tắc lấy lịch từ Calendar; thiếu ma trận chuyển trạng thái; thiếu trạng thái INTAKE | 1.1 |
| M06 / M07 | Không có state machine cho Reservation; không rõ HOLD có độc quyền không | 1.2, 2.4 |
| M06.03 | Trạng thái đơn và trạng thái dòng có thể lệch nhau; OVERDUE chưa được chốt | 1.3, 1.4 |
| WF05 | Điều kiện COMPLETED mơ hồ | 1.5 |
| M09.04 | Không có ân hạn, trần phí, leo thang | 1.6 |
| M07 | Buffer có được chồng nhau không; tính theo giờ kế hoạch hay thực tế; không có thực thể lịch bảo trì | 2.1–2.3 |
| BR-019 | Mới có cảnh báo, chưa có luồng xử lý | 2.5, 2.6 |
| M06 | Thiếu gia hạn, đổi món, no-show, khách vãng lai; hủy đơn đã bàn giao | 3.1–3.8 |
| M11 | Cọc quản lý bằng trạng thái; thiếu phân bổ thanh toán, cấn trừ, phí vượt cọc, xác minh chuyển khoản | 4.1–4.10 |
| DEC-001/005/006, Q3/Q5/Q17 | Mô hình giá và cọc | 5 |
| Ma trận 4.2 | Nhiều ô "Theo quyền" | 6.1, 6.2 |
| Q15 | Phụ kiện | 6.3 |
| M01.02 | Task không tự đóng | 6.4 |
| WF01 vs M04.01 | Thời điểm tạo Inventory Item mâu thuẫn | 6.7 |
| M09.03 | Hai kết quả "Đạt" trùng nhau; thiếu mapping sang trạng thái | 6.11 |
| Mục 11.1 | MVP không khép kín được vòng đời đơn | 7 |

------------------------------------------------------------------------

# 11. Best practice ngành cho thuê thời trang được áp dụng

Spec v1.0 chưa đề cập các thực hành dưới đây. Chúng được rút ra từ cách vận hành phổ biến của các cửa hàng cho thuê áo cưới, áo dài và vest tại Việt Nam, cùng các nền tảng rental quốc tế như Booqable, Current RMS và Rent the Runway. Mỗi mục ghi phạm vi áp dụng: MVP, Phase 2 (P2) hoặc Phase 3 (P3).

## 11.1. Lịch thử đồ (Fitting Appointment) — MVP

- Khách thường thử đồ trước khi chốt đơn, nhất là đồ cưới. Lịch thử là một Reservation loại `FITTING`, ngắn (mặc định 1 giờ, không có buffer). Lịch thử chặn availability của món trong khung giờ đó.
- Một lịch thử có thể gắn nhiều món. Sau buổi thử, Sales chuyển các món khách chọn thành đơn có HOLD ngay trên màn hình đó.
- Dashboard có nhóm "Lịch thử hôm nay".

## 11.2. Chỉnh sửa form tạm (Alteration) — MVP

- Các yêu cầu như lên lai, bóp eo, nới ngực được ghi thành **dòng dịch vụ** trên đơn: mô tả, phí và hạn hoàn thành.
- Dòng dịch vụ tạo task cho thợ. Đơn chỉ được READY_FOR_HANDOVER khi mọi dòng dịch vụ đã xong.
- Đơn có alteration tự cộng thêm buffer trước bàn giao, mặc định 24 giờ.
- Khi nhận trả, kiểm tra có thêm mục "Khôi phục form gốc". Hệ thống tự tạo phiếu sửa loại `RESTORE`, phiếu này chặn availability cho tới khi xong.

## 11.3. Phí bảo vệ hư hỏng nhỏ (Damage Waiver) — MVP

- Khách có thể mua thêm phí bảo vệ, mặc định 10% tiền thuê, tối thiểu 50.000đ.
- Phí bảo vệ **miễn** phí cho các hư hỏng nhỏ có trong danh mục: bẩn thông thường, sứt chỉ, mất nút. Các hư hỏng lớn, mất món và mất phụ kiện vẫn tính phí bình thường.
- Phí bảo vệ là doanh thu dịch vụ, không phải doanh thu thuê, và được hoàn khi hủy đơn theo cùng tỷ lệ với tiền thuê.
- Lý do áp dụng: cách này giảm mạnh tranh chấp ở quầy (4.8) và là thực hành chuẩn của các nền tảng rental.

## 11.4. Hạng tình trạng tài sản (Condition Grade) — MVP

- Mỗi món có hạng **A** (như mới), **B** (tốt, có dấu vết nhẹ) hoặc **C** (cũ, chỉ dùng cho thuê giá rẻ hoặc chụp ảnh).
- Hạng do kiểm tra cập nhật và cần lý do khi hạ hạng.
- Giá thuê có thể đặt hệ số theo hạng, ví dụ A 100%, B 85%, C 65%. Hệ số này áp dụng sau thứ tự ưu tiên giá ở mục 5.
- Ngưỡng gợi ý thanh lý: hạng C kèm tổng chi phí sửa lớn hơn 50% giá vốn, hoặc 180 ngày không có lượt thuê.

## 11.5. Thanh toán VietQR & đối soát — MVP (bán tự động), P2 (tự động)

- Mỗi khoản cần thu sinh một **mã VietQR động**, có sẵn số tiền và nội dung chuyển khoản `<mã đơn>-<mã giao dịch>`.
- Giao dịch được tạo ở trạng thái PENDING (4.1). Ở MVP, Accountant xác minh bằng cách đối chiếu nội dung chuyển khoản. Ở P2, xác minh tự động qua webhook của ngân hàng hoặc cổng thanh toán.
- Mọi API ghi giao dịch yêu cầu **idempotency key** để tránh ghi trùng khi mạng lỗi hoặc người dùng bấm hai lần.

## 11.6. Cọc theo mức tin cậy khách hàng — P2

- Hệ thống tính điểm tin cậy cho khách từ số đơn hoàn tất, số lần trả trễ, số lần hư hỏng và công nợ.
- Khách thân thiết (ví dụ ≥ 3 đơn hoàn tất, không trễ quá 1 lần) được giảm cọc 50%. Khách WARNING phải cọc 150%.
- MVP chỉ hiển thị lịch sử khách. Sales có thể giảm cọc bằng quyền `deposit.adjust`.

## 11.7. Dữ liệu cá nhân theo Nghị định 13/2023/NĐ-CP — MVP

- Chỉ thu CCCD khi chính sách cửa hàng yêu cầu, và phải có **ghi nhận đồng ý** của khách (checkbox kèm thời điểm, lưu trên biên nhận).
- Ảnh giấy tờ được lưu ở vùng lưu trữ mã hóa. Chỉ người có quyền `customer.pii.view` được xem, và mọi lượt xem đều ghi audit (6.10).
- Ảnh giấy tờ tự xóa sau **30 ngày** kể từ khi đơn COMPLETED, trừ khi đơn đang tranh chấp hoặc còn công nợ. Hồ sơ khách và lịch sử đơn vẫn được giữ.

## 11.8. Biên nhận số & liên lạc khách — MVP (link), P2 (Zalo ZNS)

- Sau mỗi lần xác nhận, bàn giao hoặc quyết toán, hệ thống sinh **biên nhận PDF** có snapshot chính sách và chữ ký khách. Biên nhận có link xem công khai, link dùng token và hết hạn sau 90 ngày.
- MVP: nút "Gửi qua Zalo/SMS" mở ứng dụng với tin nhắn soạn sẵn kèm link.
- P2: tự động gửi qua Zalo ZNS các tin xác nhận đơn, nhắc nhận hàng trước 24 giờ, nhắc trả trước 24 giờ, báo quá hạn và báo kết quả hoàn cọc.

## 11.9. Quy trình theo pipeline (Kanban vận hành) — MVP

Daily Operations (M01.02) được trình bày dạng **Kanban theo vòng đời**: Thử đồ → Chờ xác nhận → Chuẩn bị/Chỉnh sửa → Sẵn sàng giao → Đang thuê → Quá hạn → Chờ kiểm tra → Vệ sinh/Sửa → Chờ quyết toán.

Các thẻ được suy ra từ trạng thái (6.4) chứ không kéo thả tay. Kéo thẻ chỉ là lối tắt để mở màn hình nghiệp vụ tương ứng.

## 11.10. Kiến trúc dữ liệu khuyến nghị cho đội kỹ thuật

- **PostgreSQL** cho dữ liệu chính, dùng `tstzrange` kết hợp exclusion constraint cho occupancy (2.4).
- **Ledger** tài chính chỉ ghi thêm (append-only). Không có thao tác UPDATE hay DELETE trên giao dịch đã CONFIRMED (BR-013).
- **Audit log** lưu dạng sự kiện (actor, action, entity, before/after, reason, timestamp) cho mọi thay đổi trạng thái (BR-017).
- **Job nền** (hết hạn HOLD, cờ OVERDUE và AT_RISK, nhắc việc) chạy định kỳ mỗi phút, idempotent, có retry và dashboard giám sát (9.4).
- Ảnh được lưu trên object storage. Ảnh được nén phía client, có giới hạn chiều dài và giữ metadata thời điểm chụp.

## 11.11. Trả lời các câu hỏi Discovery (mục 18 v1.0)

| # | Trả lời chốt |
|---|---|
| 1 | Một cửa hàng, một kho, nhiều vị trí kệ. Dữ liệu có sẵn `store_id`. |
| 2 | Ngày lịch. Có gói theo giờ cho danh mục chụp ảnh (mục 5). |
| 3 | Tối đa 6 tháng (DEC-029). |
| 4 | Không giữ lịch khi khách mới hỏi giá. Chỉ HOLD 24 giờ hoặc lịch thử (11.1). |
| 5 | Theo món: max(số cố định, % giá vốn) (DEC-028). Xác nhận đơn cần trả trước ≥ 30% tiền thuê. |
| 6 | Có, khách được thanh toán nhiều lần (4.2). |
| 7 | Không giao hàng trong MVP (P2). |
| 8 | Có đổi món: trước bàn giao (3.2) và sau bàn giao (3.3). |
| 9 | Trả ngoài giờ qua hình thức gửi lại: ghi nhận giờ thực tế lúc nhân viên nhận, nhưng **không tính trễ** nếu khách có bằng chứng gửi trước hạn. Ân hạn 60 phút. |
| 10 | Theo ngày. Ân hạn 60 phút, tính theo tiền thuê/ngày của món, trần bằng giá trị đền bù (DEC-023). |
| 11 | Có bảng phí hư hỏng theo danh mục. Nhân viên đề xuất mức ngoài bảng thì cần Manager duyệt. |
| 12 | Sales giảm tới 10%; Manager tới 30%; trên 30% hoặc miễn phí cần Owner. |
| 13 | Chỉ lưu khi khách đồng ý; tự xóa sau 30 ngày (11.7). |
| 14 | Ký trên màn hình tablet và lưu vào biên nhận PDF. Chưa dùng ký số pháp lý. |
| 15 | Phụ kiện thường: danh sách cấu thành. Phụ kiện giá trị cao: Inventory Item riêng (6.3). |
| 16 | Set/combo để Phase 2. |
| 17 | Bậc giá theo thời lượng ở MVP. Giá mùa/cuối tuần ở P2. |
| 18 | Không tích hợp kế toán hay hóa đơn trong MVP. VietQR bán tự động (11.5). |
