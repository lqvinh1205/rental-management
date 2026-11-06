# ĐẶC TẢ NGHIỆP VỤ & CHỨC NĂNG

## Hệ thống quản lý cửa hàng cho thuê quần áo và phụ kiện (Rental Management System)

**Phiên bản:** 1.0\
**Ngày:** 04/11/2026\
**Vai trò tài liệu:** Product Owner (PO) -- Business Requirements &
Functional Specification\
**Trạng thái:** Draft -- dùng làm baseline để xác nhận nghiệp vụ, thiết
kế UI/UX và phát triển MVP.

------------------------------------------------------------------------

# 1. Tổng quan sản phẩm

## 1.1. Bối cảnh

Cửa hàng kinh doanh dịch vụ cho thuê quần áo và phụ kiện, ví dụ: áo dài
nam/nữ, váy dự tiệc, vest, trang phục biểu diễn, túi, giày và phụ kiện
đi kèm.

Khác với hệ thống bán lẻ thông thường, cửa hàng không chỉ quản lý số
lượng tồn kho. Mỗi món hàng là một tài sản vật lý có thể được cho thuê
nhiều lần, có lịch sử sử dụng, lịch thuê, tình trạng, vị trí lưu trữ,
chi phí sửa chữa và hiệu suất khai thác riêng.

Hệ thống cần hỗ trợ toàn bộ vòng đời của một món hàng và toàn bộ vòng
đời của một đơn thuê: từ nhập hàng, tạo mẫu sản phẩm, quản lý từng món
vật lý, giữ lịch, xác nhận đơn, chuẩn bị, bàn giao, nhận trả, kiểm tra,
vệ sinh/sửa chữa đến đối soát tài chính và báo cáo.

## 1.2. Mục tiêu sản phẩm

1.  Quản lý chính xác từng món hàng vật lý bằng mã định danh/QR riêng.
2.  Ngăn ngừa đặt trùng lịch một món hàng.
3.  Rút ngắn thời gian tạo đơn, bàn giao và nhận trả.
4.  Kiểm soát tiền thuê, tiền cọc, khoản thu thêm, hoàn tiền và công nợ.
5.  Giảm thất thoát, nhầm lẫn và tranh chấp tình trạng sản phẩm.
6.  Theo dõi lịch sử khai thác, chi phí và hiệu quả đầu tư của từng tài
    sản.
7.  Cung cấp danh sách công việc cần xử lý trong ngày cho từng vai trò.
8.  Chuẩn hóa quy trình vận hành để cửa hàng có thể mở rộng quy mô và
    nhân sự.

## 1.3. Nguyên tắc thiết kế nghiệp vụ

-   Quản lý **từng món hàng thực tế**, không chỉ quản lý tổng số lượng.
-   Không cho phép một món hàng có hai lịch thuê bị chồng lấn.
-   Trạng thái "đã nhận trả" không đồng nghĩa với "sẵn sàng cho thuê".
-   Tiền cọc là khoản phải theo dõi riêng, không cộng vào doanh thu tiền
    thuê.
-   Các thông tin giá và chính sách quan trọng phải được lưu snapshot
    tại thời điểm xác nhận đơn.
-   Không xóa dữ liệu lịch sử liên quan đến đơn thuê, tài chính, bàn
    giao và tình trạng tài sản.
-   Các thay đổi quan trọng phải có người thực hiện, thời gian và nội
    dung thay đổi.
-   Các thao tác có ảnh hưởng đến lịch thuê hoặc tài chính phải được
    kiểm tra lại trước khi lưu.

## 1.4. Chỉ số thành công (Success Metrics)

  -----------------------------------------------------------------------
  Nhóm                                Chỉ số đề xuất
  ----------------------------------- -----------------------------------
  Vận hành                            Thời gian tạo một đơn thuê; thời
                                      gian bàn giao/nhận trả

  Khả dụng                            Số trường hợp đặt trùng lịch; tỷ lệ
                                      đơn phải đổi món do lỗi khả dụng

  Tài sản                             Tỷ lệ tài sản có QR; tỷ lệ thất
                                      lạc; số ngày nằm ở trạng thái không
                                      khai thác

  Tài chính                           Doanh thu thuê; tiền cọc đang giữ;
                                      công nợ; tỷ lệ hoàn cọc đúng hạn

  Khai thác                           Lượt thuê/tài sản; doanh thu/tài
                                      sản; tỷ lệ sử dụng tài sản

  Chất lượng                          Tỷ lệ đơn phát sinh hư hỏng; tỷ lệ
                                      trả trễ; tỷ lệ khiếu nại
  -----------------------------------------------------------------------

Các chỉ số trên cần được PO và chủ cửa hàng thống nhất cách tính trước
khi triển khai báo cáo chính thức.

------------------------------------------------------------------------

# 2. Phạm vi sản phẩm

## 2.1. Trong phạm vi

-   Đăng nhập, nhân sự và phân quyền.
-   Danh mục sản phẩm, danh mục biến thể và từng món hàng vật lý.
-   Quản lý QR/barcode và vị trí lưu trữ.
-   Nhập hàng, ghi nhận giá vốn và điều chỉnh tài sản.
-   Quản lý khách hàng.
-   Tạo báo giá/đơn thuê, giữ lịch, xác nhận, sửa và hủy đơn.
-   Lịch thuê và kiểm tra khả dụng theo khoảng thời gian.
-   Chuẩn bị đơn, bàn giao, nhận trả một phần/toàn bộ.
-   Kiểm tra tình trạng, ghi nhận hư hỏng, vệ sinh và sửa chữa.
-   Quản lý tiền thuê, tiền cọc, thanh toán, hoàn tiền, khấu trừ và công
    nợ.
-   Thông báo và danh sách công việc cần xử lý.
-   Báo cáo vận hành, doanh thu và hiệu suất tài sản.
-   Cấu hình chính sách thuê, thời gian đệm và thiết lập cửa hàng.

## 2.2. Ngoài phạm vi MVP

Các nội dung sau có thể xem xét ở giai đoạn mở rộng:

-   Website/app cho khách hàng tự đặt thuê.
-   Cổng thanh toán trực tuyến và đối soát ngân hàng tự động.
-   Tích hợp vận chuyển bên thứ ba.
-   Kế toán tổng hợp, hóa đơn điện tử và kê khai thuế chuyên sâu.
-   Chương trình thành viên, tích điểm, voucher nhiều điều kiện.
-   Định giá thuê tự động bằng AI.
-   Quản lý nhiều chi nhánh với luân chuyển tài sản phức tạp.
-   Đồng bộ đa kênh bán hàng.

------------------------------------------------------------------------

# 3. Thuật ngữ và mô hình dữ liệu nghiệp vụ

  -----------------------------------------------------------------------
  Thuật ngữ                           Định nghĩa
  ----------------------------------- -----------------------------------
  Product                             Mẫu sản phẩm dùng để mô tả một kiểu
                                      hàng, ví dụ Áo dài nữ An

  Product Variant                     Biến thể của mẫu sản phẩm, thường
                                      là tổ hợp size/màu

  Inventory Item / Asset              Một món hàng vật lý cụ thể, có mã
                                      riêng và có thể được cho thuê độc
                                      lập

  Rental Order                        Đơn thuê của khách hàng

  Rental Order Item                   Một dòng hàng trong đơn, tham chiếu
                                      đến một Inventory Item cụ thể

  Reservation                         Khoảng thời gian giữ/đặt một tài
                                      sản cho đơn thuê

  Rental Period                       Khoảng thời gian khách được sử dụng
                                      hàng theo thỏa thuận

  Buffer Time                         Khoảng thời gian đệm trước/sau lịch
                                      thuê để chuẩn bị, vệ sinh hoặc vận
                                      chuyển

  Handover                            Quy trình bàn giao hàng cho khách

  Return                              Quy trình nhận lại hàng từ khách

  Inspection                          Kiểm tra tình trạng tài sản

  Deposit                             Tiền cọc bảo đảm

  Rental Revenue                      Doanh thu từ phí thuê, không bao
                                      gồm tiền cọc

  Asset Status                        Trạng thái vận hành hiện tại của
                                      từng món hàng
  -----------------------------------------------------------------------

## 3.1. Quan hệ dữ liệu chính

-   Một Product có nhiều Product Variant.
-   Một Product Variant có nhiều Inventory Item.
-   Mỗi Inventory Item thuộc một biến thể và có một mã QR/ID duy nhất.
-   Một Rental Order có một hoặc nhiều Rental Order Item.
-   Mỗi Rental Order Item trong MVP phải gắn với một Inventory Item cụ
    thể khi xác nhận đơn.
-   Một Inventory Item có nhiều bản ghi lịch sử: đặt thuê, bàn giao,
    nhận trả, kiểm tra, vệ sinh, sửa chữa và điều chỉnh trạng thái.
-   Một Rental Order có thể có nhiều giao dịch tài chính: thu tiền, thu
    cọc, hoàn cọc, khấu trừ, hoàn tiền hoặc thu thêm.

## 3.2. Thông tin tối thiểu của Inventory Item

-   Mã tài sản duy nhất.
-   QR code/barcode.
-   Product và Product Variant tương ứng.
-   Size, màu sắc và các thuộc tính mô tả.
-   Giá nhập/giá vốn.
-   Ngày nhập.
-   Nhà cung cấp (nếu có).
-   Kho/vị trí lưu trữ.
-   Trạng thái hiện tại.
-   Tình trạng vật lý và ghi chú.
-   Hình ảnh tài sản.
-   Lịch sử thuê.
-   Lịch sử kiểm tra, vệ sinh, sửa chữa.
-   Tổng lượt thuê và doanh thu khai thác.

------------------------------------------------------------------------

# 4. Người dùng và phân quyền

## 4.1. Nhóm vai trò

  -----------------------------------------------------------------------
  Vai trò                             Trách nhiệm chính
  ----------------------------------- -----------------------------------
  Owner                               Chủ cửa hàng; xem toàn bộ dữ liệu,
                                      cấu hình và báo cáo

  Admin                               Quản trị hệ thống, nhân sự, danh
                                      mục và cấu hình

  Manager                             Điều phối vận hành, duyệt các
                                      trường hợp ngoại lệ

  Sales                               Tư vấn, quản lý khách hàng, tạo
                                      đơn, thu tiền theo quyền

  Warehouse / Operations              Chuẩn bị, bàn giao, nhận trả, kiểm
                                      kê và cập nhật vị trí

  Accountant                          Theo dõi giao dịch, cọc, hoàn tiền,
                                      công nợ và báo cáo tài chính
  -----------------------------------------------------------------------

Một nhân viên có thể được gán nhiều vai trò hoặc bộ quyền tùy quy mô cửa
hàng.

## 4.2. Ma trận quyền đề xuất

  Chức năng                     Owner/Admin           Manager        Sales    Warehouse             Accountant
  --------------------------- ------------- ----------------- ------------ ------------ ----------------------
  Xem dashboard                           ✓                 ✓   Theo quyền   Theo quyền                      ✓
  Quản lý nhân sự/quyền                   ✓   Có thể giới hạn          ---          ---                    ---
  Quản lý danh mục sản phẩm               ✓                 ✓          Xem          Xem                    Xem
  Tạo/sửa Inventory Item                  ✓                 ✓          ---   Theo quyền                    ---
  Tạo đơn thuê                            ✓                 ✓            ✓          ---                    Xem
  Xác nhận đơn                            ✓                 ✓   Theo quyền          ---                    ---
  Bàn giao/nhận trả                       ✓                 ✓   Theo quyền            ✓                    ---
  Ghi nhận hư hỏng                        ✓                 ✓   Theo quyền            ✓                    ---
  Thu tiền/cọc                            ✓                 ✓   Theo quyền          ---                      ✓
  Hoàn cọc/khấu trừ                       ✓             Duyệt          ---          ---   Thực hiện theo quyền
  Xem báo cáo tài chính                   ✓        Theo quyền          ---          ---                      ✓
  Cấu hình hệ thống                       ✓        Theo quyền          ---          ---                    ---

**Quy tắc:** quyền phải được kiểm tra ở cả giao diện và API. Các quyền
nhạy cảm như xác nhận đơn, sửa giá, hoàn tiền, miễn/giảm phí và điều
chỉnh trạng thái tài sản cần có quyền riêng.

------------------------------------------------------------------------

# 5. Kiến trúc chức năng và danh sách module

  -----------------------------------------------------------------------
  Mã                      Module                  Mục đích
  ----------------------- ----------------------- -----------------------
  M01                     Dashboard & Daily       Tổng quan và công việc
                          Operations              trong ngày

  M02                     Product Catalog         Quản lý mẫu sản phẩm,
                                                  biến thể và danh mục

  M03                     Inventory Assets        Quản lý từng món hàng
                                                  vật lý

  M04                     Stock & Procurement     Nhập hàng, điều chỉnh
                                                  và kiểm kê

  M05                     Customers               Hồ sơ và lịch sử khách
                                                  hàng

  M06                     Rental Orders           Báo giá, tạo đơn, xác
                                                  nhận, sửa và hủy

  M07                     Rental Calendar &       Lịch thuê và kiểm tra
                          Availability            khả dụng

  M08                     Preparation & Handover  Chuẩn bị và bàn giao
                                                  hàng

  M09                     Return & Inspection     Nhận trả và kiểm tra
                                                  tình trạng

  M10                     Cleaning & Repair       Vệ sinh, bảo trì và sửa
                                                  chữa

  M11                     Finance                 Thanh toán, cọc, hoàn
                                                  tiền, khấu trừ và công
                                                  nợ

  M12                     Notifications & Tasks   Nhắc việc và cảnh báo

  M13                     Reports & Analytics     Báo cáo kinh doanh và
                                                  hiệu suất tài sản

  M14                     Staff & Permissions     Nhân sự và phân quyền

  M15                     Settings                Thiết lập chính sách và
                                                  tham số cửa hàng
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 6. Đặc tả nghiệp vụ chi tiết theo module

# M01. Dashboard & Daily Operations

## Mục tiêu

Giúp chủ cửa hàng và nhân viên biết ngay tình hình kinh doanh và các
việc cần xử lý trong ngày.

## Chức năng

### M01.01 -- Dashboard tổng quan

Hiển thị các chỉ số: - Đơn thuê hôm nay. - Đơn cần bàn giao hôm nay. -
Đơn cần nhận trả hôm nay. - Đơn đang thuê. - Đơn quá hạn. - Tài sản đang
được thuê. - Tài sản đang vệ sinh/sửa chữa. - Tiền thuê đã thu trong
ngày. - Tiền cọc đang giữ. - Khoản cần hoàn/thu thêm. - Cảnh báo tài sản
không khả dụng.

Bộ lọc: - Hôm nay / 7 ngày / tháng này / khoảng ngày tùy chọn. - Chi
nhánh (nếu có). - Trạng thái đơn.

### M01.02 -- Daily Operations

Danh sách công việc được gom theo nhóm: 1. Cần chuẩn bị. 2. Cần bàn
giao. 3. Cần nhận trả. 4. Đã quá hạn. 5. Cần kiểm tra sau trả. 6. Cần vệ
sinh/sửa chữa. 7. Cần hoàn cọc hoặc thu khoản còn thiếu. 8. Cần xử lý
cảnh báo lịch thuê.

Mỗi công việc cần có: - Mã đơn hoặc mã tài sản. - Khách hàng. - Thời hạn
xử lý. - Mức độ ưu tiên. - Người phụ trách. - Trạng thái. - CTA dẫn đến
màn hình xử lý.

### M01.03 -- Notification Center

Thông báo được phân loại: - Sắp đến giờ bàn giao. - Sắp đến hạn trả. -
Quá hạn trả. - Đơn bị ảnh hưởng do tài sản hỏng. - Tài sản chưa hoàn tất
kiểm tra/vệ sinh. - Reservation sắp hết hạn. - Có khoản cọc cần hoàn. -
Có công nợ cần thu.

Người dùng có thể đánh dấu đã đọc, lọc theo loại và mở đối tượng liên
quan.

## Acceptance Criteria

-   Dashboard chỉ hiển thị dữ liệu theo quyền người dùng.
-   Các con số phải thống nhất với danh sách chi tiết tương ứng.
-   Mỗi công việc phải dẫn được đến đúng đơn/tài sản và hành động cần
    thực hiện.
-   Công việc quá hạn phải được nhận diện rõ ràng.

------------------------------------------------------------------------

# M02. Product Catalog

## Mục tiêu

Quản lý danh mục mẫu sản phẩm và các biến thể để chuẩn hóa dữ liệu hàng
hóa.

## M02.01 -- Danh sách Product

Thông tin hiển thị: - Mã mẫu. - Tên sản phẩm. - Danh mục. - Ảnh đại
diện. - Số biến thể. - Tổng số tài sản vật lý. - Số tài sản khả dụng. -
Giá thuê tham khảo. - Trạng thái kinh doanh.

Bộ lọc: danh mục, trạng thái, từ khóa, thương hiệu/nhà cung cấp (nếu
có).

## M02.02 -- Tạo/Sửa Product

Thông tin: - Tên sản phẩm. - Mã sản phẩm. - Danh mục. - Mô tả. - Ảnh. -
Hướng dẫn sử dụng/bảo quản. - Giá thuê mặc định hoặc bảng giá tham
khảo. - Trạng thái: đang kinh doanh/ngừng kinh doanh.

## M02.03 -- Product Variant

Cho phép cấu hình biến thể theo thuộc tính: - Size: S, M, L, XL, 36,
38... hoặc Free Size. - Màu sắc. - Kiểu dáng/phiên bản. - Thuộc tính mở
rộng do cửa hàng cấu hình.

Một biến thể không bắt buộc phải có tất cả thuộc tính. Hệ thống cần hỗ
trợ Free Size và các thuộc tính đặc thù của ngành hàng.

## Quy tắc nghiệp vụ

-   Product là mẫu, không đại diện cho một món hàng có thể bàn giao.
-   Variant là cấu hình size/màu, không thay thế Inventory Item.
-   Ngừng kinh doanh Product không được xóa lịch sử tài sản và đơn cũ.
-   Không cho xóa Product/Variant đang được tham chiếu bởi tài sản hoặc
    đơn thuê; chỉ cho phép ngừng sử dụng.

## Acceptance Criteria

-   Có thể tạo một mẫu với nhiều biến thể.
-   Có thể xem tổng số tài sản vật lý theo từng biến thể.
-   Có thể truy cập danh sách Inventory Item từ Product/Variant.
-   Việc đổi tên/mô tả không làm thay đổi snapshot của đơn đã xác nhận.

------------------------------------------------------------------------

# M03. Inventory Assets -- Quản lý từng món hàng vật lý

## Mục tiêu

Theo dõi từng món hàng độc lập trong suốt vòng đời khai thác.

## M03.01 -- Danh sách Inventory Items

Cột dữ liệu: - Mã tài sản. - QR. - Tên mẫu. - Size/màu. - Ảnh. - Giá
nhập. - Vị trí. - Trạng thái. - Lần thuê gần nhất. - Tổng lượt thuê.

Bộ lọc: - Product/Variant. - Size/màu. - Trạng thái. - Kho/vị trí. -
Khoảng giá nhập. - Ngày nhập. - Tài sản chưa có QR. - Tài sản cần bảo
trì.

## M03.02 -- Tạo Inventory Item

Thông tin bắt buộc: - Product. - Variant. - Mã tài sản (hệ thống sinh
hoặc người dùng nhập theo quyền). - Giá nhập. - Trạng thái ban đầu. -
Kho/vị trí.

Thông tin tùy chọn: - Ngày nhập. - Nhà cung cấp. - Ảnh thực tế. - Ghi
chú. - Thông tin hóa đơn/chứng từ.

Sau khi tạo, hệ thống sinh QR duy nhất để in và gắn lên món hàng.

## M03.03 -- Chi tiết Inventory Item

Các tab đề xuất:

**Overview** - Thông tin nhận diện. - Ảnh. - Trạng thái. - Giá nhập. -
Vị trí. - Thông tin Product/Variant.

**Calendar** - Lịch đã giữ. - Lịch thuê đã xác nhận. - Thời gian đang
thuê. - Thời gian đệm. - Lịch bảo trì/không khả dụng.

**Condition** - Tình trạng hiện tại. - Checklist kiểm tra. - Ảnh theo
từng lần kiểm tra. - Lịch sử hư hỏng.

**Repair & Cleaning** - Phiếu vệ sinh. - Phiếu sửa chữa. - Chi phí. -
Thời gian bắt đầu/kết thúc. - Người thực hiện.

**Performance** - Tổng lượt thuê. - Tổng doanh thu thuê gắn với tài
sản. - Tổng chi phí sửa chữa/vệ sinh nếu có. - Ngày không khả dụng. -
Hiệu suất khai thác.

**Audit History** - Ai thay đổi. - Thời điểm. - Giá trị trước/sau. - Lý
do thay đổi.

## M03.04 -- Quản lý trạng thái tài sản

Trạng thái đề xuất: - `AVAILABLE`: Sẵn sàng cho thuê. - `RESERVED`: Đã
được giữ lịch/đặt cho đơn. - `PREPARING`: Đang chuẩn bị. - `RENTED`:
Đang được khách thuê. - `RETURNED_PENDING_INSPECTION`: Đã nhận trả, chờ
kiểm tra. - `CLEANING`: Đang vệ sinh. - `REPAIRING`: Đang sửa chữa. -
`DAMAGED`: Hư hỏng, chưa thể cho thuê. - `LOST`: Thất lạc. - `RETIRED`:
Ngừng khai thác/thanh lý.

Trạng thái là trạng thái vận hành; lịch đặt trong tương lai phải được
xác định từ Reservation/Calendar, không chỉ dựa vào một trường trạng
thái hiện tại.

## Quy tắc nghiệp vụ

-   Mỗi Inventory Item có ID duy nhất.
-   Không gán một Inventory Item vào hai đơn có khoảng thời gian sử
    dụng/buffer chồng lấn.
-   Không chuyển tài sản từ `RETURNED_PENDING_INSPECTION` sang
    `AVAILABLE` nếu chưa hoàn tất kiểm tra và các bước cần thiết.
-   Tài sản đang `REPAIRING`, `DAMAGED`, `LOST` hoặc `RETIRED` không
    được chọn cho đơn mới.
-   Chuyển trạng thái thủ công cần quyền phù hợp và lý do.
-   Không xóa Inventory Item đã từng phát sinh giao dịch; dùng trạng
    thái ngừng khai thác.

## Acceptance Criteria

-   Quét QR mở đúng trang chi tiết tài sản.
-   Không tồn tại hai tài sản có cùng mã.
-   Lịch sử của tài sản xem được theo thời gian.
-   Tài sản không khả dụng không thể được xác nhận vào đơn mới.

------------------------------------------------------------------------

# M04. Stock & Procurement -- Nhập hàng và kiểm kê

## Mục tiêu

Ghi nhận nguồn hình thành tài sản và kiểm soát chênh lệch thực tế.

## Chức năng

### M04.01 -- Phiếu nhập hàng

Thông tin: - Mã phiếu. - Ngày nhập. - Nhà cung cấp. - Người tạo/người
nhận. - Danh sách Product/Variant. - Số lượng thực nhận. - Đơn giá
nhập. - Chi phí bổ sung (nếu có). - Ghi chú và chứng từ.

Khi xác nhận phiếu nhập, hệ thống tạo các Inventory Item tương ứng. Mỗi
món có mã/QR riêng.

### M04.02 -- Điều chỉnh tài sản

Cho phép: - Điều chỉnh vị trí. - Cập nhật thông tin mô tả. - Ghi nhận
thất lạc. - Ghi nhận hư hỏng. - Thanh lý/ngừng khai thác.

Mọi điều chỉnh phải có lý do và audit log.

### M04.03 -- Kiểm kê

-   Tạo phiên kiểm kê theo kho/vị trí/danh mục.
-   Quét QR hoặc tìm kiếm tài sản.
-   Ghi nhận có mặt/không tìm thấy/sai vị trí.
-   So sánh danh sách hệ thống với kết quả thực tế.
-   Tạo biên bản chênh lệch.
-   Người có quyền xác nhận điều chỉnh.

## Acceptance Criteria

-   Phiếu nhập được xác nhận sẽ sinh đúng số Inventory Item.
-   Giá nhập được lưu ở cấp tài sản.
-   Kiểm kê không tự động thay đổi dữ liệu tài sản nếu chưa được người
    có quyền xác nhận.

------------------------------------------------------------------------

# M05. Customers -- Quản lý khách hàng

## Mục tiêu

Lưu thông tin khách, lịch sử thuê và các vấn đề liên quan.

## M05.01 -- Hồ sơ khách hàng

Thông tin: - Họ tên. - Số điện thoại. - Email (không bắt buộc). - Địa
chỉ (không bắt buộc). - Ngày sinh (không bắt buộc). - Ghi chú. -
Ảnh/chứng từ theo chính sách cửa hàng. - Trạng thái khách hàng.

## M05.02 -- Lịch sử khách hàng

Hiển thị: - Tổng số đơn. - Tổng chi tiêu thuê. - Đơn đang thuê. - Đơn đã
hoàn tất. - Đơn quá hạn. - Lịch sử cọc/công nợ. - Sự cố hoặc ghi chú vận
hành.

## Quy tắc

-   Số điện thoại là tiêu chí tìm kiếm chính, nhưng cần cho phép xử lý
    trường hợp khách dùng chung số hoặc thay đổi số theo chính sách dữ
    liệu.
-   Không xóa khách hàng có đơn; chỉ được đánh dấu ngừng hoạt động/ẩn.
-   Dữ liệu giấy tờ cá nhân phải được giới hạn quyền truy cập và thời
    hạn lưu trữ theo chính sách cửa hàng/pháp luật áp dụng.

------------------------------------------------------------------------

# M06. Rental Orders -- Quản lý đơn thuê

## Mục tiêu

Quản lý vòng đời đơn thuê từ lúc tư vấn đến khi hoàn tất nghĩa vụ tài
chính và nhận trả hàng.

## M06.01 -- Danh sách đơn thuê

Cột dữ liệu: - Mã đơn. - Khách hàng. - Ngày tạo. - Thời gian thuê. -
Ngày/giờ bàn giao. - Ngày/giờ phải trả. - Tổng số món. - Tổng tiền
thuê. - Tiền đã thanh toán. - Tiền cọc. - Công nợ. - Trạng thái đơn. -
Nhân viên phụ trách.

Bộ lọc: - Trạng thái. - Ngày tạo. - Ngày bàn giao/trả. - Khách hàng. -
Nhân viên. - Khoản còn nợ. - Đơn quá hạn.

## M06.02 -- Tạo đơn thuê

Luồng tạo đơn đề xuất theo từng bước:

**Bước 1 -- Chọn khách hàng** - Tìm khách bằng tên/số điện thoại. - Chọn
khách cũ hoặc tạo khách mới. - Ghi chú yêu cầu của khách.

**Bước 2 -- Chọn thời gian** - Thời điểm nhận hàng. - Thời điểm trả
hàng. - Phương thức nhận/trả nếu cửa hàng có hỗ trợ. - Hệ thống tính
thời gian đệm theo cấu hình.

**Bước 3 -- Chọn sản phẩm/tài sản** - Tìm theo danh mục, Product,
Variant, size, màu. - Chỉ hiển thị tài sản khả dụng trong khoảng thời
gian. - Hiển thị lịch của từng tài sản. - Cho phép chọn một hoặc nhiều
Inventory Item. - Cảnh báo nếu tài sản có lịch bảo trì hoặc reservation
khác.

**Bước 4 -- Giá và chính sách** - Giá thuê theo ngày/khung thời gian. -
Số ngày tính tiền. - Giảm giá. - Phí phụ thu. - Tiền cọc yêu cầu. -
Chính sách trả trễ/hư hỏng. - Ghi chú thỏa thuận.

**Bước 5 -- Xác nhận** - Hiển thị tổng kết đơn. - Kiểm tra lại khả dụng
của tất cả tài sản. - Xác nhận giữ lịch hoặc xác nhận đơn theo quyền. -
Lưu snapshot giá và chính sách.

## M06.03 -- Trạng thái đơn

Trạng thái đề xuất: - `DRAFT`: Nháp, chưa giữ lịch chính thức. -
`PENDING_CONFIRMATION`: Chờ xác nhận/đặt cọc. - `CONFIRMED`: Đã xác
nhận. - `PREPARING`: Đang chuẩn bị. - `READY_FOR_HANDOVER`: Sẵn sàng bàn
giao. - `PARTIALLY_HANDED_OVER`: Đã bàn giao một phần. - `RENTED`: Đã
bàn giao đầy đủ/đang thuê. - `PARTIALLY_RETURNED`: Đã nhận trả một
phần. - `RETURNED`: Đã nhận trả đầy đủ, chờ hoàn tất kiểm tra. -
`COMPLETED`: Hoàn tất vận hành và tài chính theo quy định. -
`CANCELLED`: Đã hủy. - `OVERDUE`: Quá hạn; có thể là cờ cảnh báo hoặc
trạng thái suy ra từ thời gian, cần PO chốt cách triển khai.

Khuyến nghị: `OVERDUE` nên được tính như một trạng thái/cờ vận hành dựa
trên hạn trả và số món chưa trả, không làm mất trạng thái nghiệp vụ gốc.

## M06.04 -- Chi tiết đơn thuê

Các khu vực: - Tổng quan đơn. - Khách hàng. - Danh sách món thuê. - Lịch
bàn giao/nhận trả. - Thanh toán và cọc. - Hợp đồng/biên bản. - Ghi
chú. - Lịch sử thay đổi.

Mỗi dòng hàng cần có: - Inventory Item. - Giá thuê snapshot. - Khoảng
thời gian thuê. - Trạng thái bàn giao. - Trạng thái nhận trả. - Tình
trạng trước/sau thuê. - Chi phí phát sinh nếu có.

## M06.05 -- Sửa đơn

Cho phép sửa các trường phù hợp khi chưa bàn giao hoặc theo quyền: -
Thời gian thuê. - Tài sản. - Giá/giảm giá. - Tiền cọc. - Ghi chú. -
Thông tin khách.

Khi thay đổi thời gian/tài sản: - Kiểm tra lại khả dụng. - Kiểm tra xung
đột với đơn khác. - Tính lại tiền theo chính sách. - Hiển thị chênh lệch
cho người dùng xác nhận. - Lưu lịch sử trước/sau.

Sau khi đã bàn giao, các thay đổi ảnh hưởng đến hàng hoặc tài chính phải
đi qua quy trình điều chỉnh có kiểm soát, không sửa âm thầm dữ liệu gốc.

## M06.06 -- Hủy đơn

-   Chỉ người có quyền mới được hủy.
-   Bắt buộc chọn lý do.
-   Hệ thống xác định khoản cần hoàn/giữ theo chính sách.
-   Giải phóng reservation nếu đủ điều kiện.
-   Không xóa giao dịch đã thu; ghi nhận hoàn tiền/khấu trừ thành giao
    dịch mới.
-   Gửi thông báo cho người phụ trách nếu đơn đã được chuẩn bị.

## Quy tắc nghiệp vụ

-   Đơn nháp không giữ tài sản vô thời hạn.
-   Reservation tạm phải có thời gian hết hạn (`expires_at`).
-   Khi hết hạn mà chưa xác nhận, reservation được giải phóng tự động.
-   Khi xác nhận đơn, hệ thống phải kiểm tra khả dụng lần cuối trong
    transaction để tránh hai nhân viên cùng đặt một tài sản.
-   Không xác nhận đơn nếu có bất kỳ tài sản nào bị trùng lịch hoặc
    không đủ điều kiện.
-   Giá thuê, giảm giá, tiền cọc và chính sách được snapshot tại thời
    điểm xác nhận.
-   Một đơn có thể có nhiều giao dịch thanh toán.
-   Một đơn có thể bàn giao/nhận trả từng phần.

## Acceptance Criteria

-   Không thể xác nhận hai đơn chồng lịch trên cùng một Inventory Item.
-   Hệ thống hiển thị rõ tài sản nào không khả dụng và nguyên nhân.
-   Đơn đã xác nhận lưu được snapshot giá/chính sách.
-   Hủy đơn không xóa giao dịch tài chính cũ.
-   Đơn có thể theo dõi trạng thái từng món, không chỉ trạng thái tổng.

------------------------------------------------------------------------

# M07. Rental Calendar & Availability

## Mục tiêu

Cho nhân viên xem lịch khai thác và chọn đúng tài sản còn khả dụng.

## M07.01 -- Lịch tổng quan

Các chế độ xem: - Theo ngày. - Theo tuần. - Theo tháng. - Timeline theo
tài sản. - Lọc theo Product/Variant/danh mục/kho.

Mỗi lịch thể hiện: - Reservation tạm. - Đơn đã xác nhận. - Đang thuê. -
Thời gian đệm. - Vệ sinh/bảo trì/sửa chữa. - Khoảng thời gian tài sản
không khả dụng.

## M07.02 -- Kiểm tra khả dụng

Input: - Khoảng thời gian nhận/trả. - Product/Variant hoặc danh sách tài
sản. - Kho/vị trí nếu cần.

Output: - Tài sản khả dụng. - Tài sản bị trùng lịch. - Tài sản đang
sửa/chờ kiểm tra. - Lý do không khả dụng. - Thời gian gần nhất có thể
thuê.

## Quy tắc tính lịch

-   Sử dụng khoảng thời gian bắt đầu/kết thúc rõ ràng.
-   Hai khoảng thời gian bị coi là chồng lấn khi
    `existing_start < target_end` và `existing_end > target_start`.
-   Thời gian buffer phải được tính vào khoảng chiếm dụng tài sản.
-   Quy tắc buffer có thể cấu hình theo cửa hàng, danh mục hoặc loại sản
    phẩm nếu nghiệp vụ cần.
-   Lịch bảo trì/sửa chữa cũng chặn khả dụng.
-   Việc hiển thị "còn hàng" phải dựa trên lịch của Inventory Item,
    không dựa trên tổng quantity của Product.

## Acceptance Criteria

-   Calendar và màn hình tạo đơn dùng chung một logic availability.
-   Khoảng buffer được thể hiện trực quan.
-   Người dùng biết rõ lý do một tài sản không thể chọn.
-   Hệ thống kiểm tra lại khả dụng tại thời điểm xác nhận đơn.

------------------------------------------------------------------------

# M08. Preparation & Handover -- Chuẩn bị và bàn giao

## Mục tiêu

Đảm bảo đúng món, đúng số lượng, đúng tình trạng trước khi giao cho
khách.

## M08.01 -- Danh sách cần chuẩn bị

Hiển thị các đơn đã xác nhận sắp đến thời gian bàn giao: - Mã đơn. -
Khách hàng. - Thời gian hẹn. - Danh sách tài sản. - Trạng thái chuẩn
bị. - Người phụ trách.

## M08.02 -- Checklist chuẩn bị

Checklist có thể cấu hình theo danh mục: - Đúng sản phẩm/size/màu. -
Đúng mã QR. - Đã vệ sinh. - Đã kiểm tra tình trạng. - Đủ phụ kiện đi
kèm. - Đóng gói. - Sẵn sàng bàn giao.

Có thể chụp ảnh và ghi chú.

## M08.03 -- Bàn giao

-   Mở đơn hoặc quét QR tài sản.
-   Xác nhận từng món thực tế.
-   Ghi nhận thời điểm bàn giao.
-   Chụp ảnh tình trạng trước thuê.
-   Khách xác nhận đã nhận.
-   Nhân viên xác nhận hoàn tất.

Hệ thống hỗ trợ bàn giao một phần. Mỗi dòng hàng có trạng thái riêng.

## Quy tắc

-   Chỉ bàn giao tài sản thuộc đơn hợp lệ và đúng khách/đơn.
-   Không bàn giao tài sản đang hỏng, chưa vệ sinh hoặc không đúng trạng
    thái.
-   Khi bàn giao, lưu ảnh/checklist và người thực hiện.
-   Đơn chỉ chuyển sang `RENTED` khi toàn bộ tài sản đã được bàn giao;
    nếu chưa đủ thì dùng trạng thái bàn giao một phần.

## Acceptance Criteria

-   Nhân viên có thể quét QR để xác nhận đúng món.
-   Có thể bàn giao nhiều món trong một đơn.
-   Có thể bàn giao một phần và tiếp tục phần còn lại sau.
-   Lưu được bằng chứng bàn giao.

------------------------------------------------------------------------

# M09. Return & Inspection -- Nhận trả và kiểm tra

## Mục tiêu

Kiểm soát món hàng trả về, tình trạng thực tế và khoản phát sinh.

## M09.01 -- Danh sách cần nhận trả

Hiển thị: - Đơn đến hạn hôm nay. - Đơn sắp đến hạn. - Đơn quá hạn. - Đơn
đã trả một phần. - Đơn chưa hoàn tất nhận trả.

## M09.02 -- Nhận trả

-   Mở đơn hoặc quét QR.
-   Xác nhận từng món đã nhận.
-   Ghi nhận thời gian thực tế.
-   Ghi nhận trả đủ/trả thiếu.
-   Ghi nhận phụ kiện đi kèm.
-   Chụp ảnh tình trạng khi nhận.
-   Ghi nhận người nhận.

Hỗ trợ nhận trả một phần.

## M09.03 -- Kiểm tra sau thuê

Checklist đề xuất: - Đúng tài sản. - Đủ phụ kiện. - Không rách/rách
đường may. - Không bẩn/ố màu. - Không mất nút/khuy/chi tiết. - Không
hỏng khóa/dây kéo. - Không có mùi hoặc dấu hiệu cần xử lý. - Đánh giá
tổng thể. - Ảnh minh chứng. - Ghi chú.

Kết quả kiểm tra: - Đạt -- cần vệ sinh thông thường. - Đạt -- sẵn sàng
sau bước xử lý tiêu chuẩn. - Cần vệ sinh đặc biệt. - Cần sửa chữa. - Hư
hỏng cần xác minh/khấu trừ. - Mất tài sản/phụ kiện.

## M09.04 -- Xử lý trả trễ

-   Tính thời gian trễ theo chính sách.
-   Tính phí trả trễ nếu được cấu hình.
-   Cho phép nhân viên đề xuất miễn/giảm.
-   Trường hợp vượt ngưỡng phải Manager/Owner duyệt.
-   Ghi nhận phí phát sinh thành khoản riêng trong đơn.

## Quy tắc

-   Nhận trả không tự động chuyển tài sản sang `AVAILABLE`.
-   Tài sản sau nhận trả chuyển sang `RETURNED_PENDING_INSPECTION`.
-   Chỉ sau khi hoàn tất kiểm tra và vệ sinh/sửa chữa cần thiết mới được
    chuyển sang `AVAILABLE`.
-   Hư hỏng phải có mô tả, ảnh và người ghi nhận.
-   Phí hư hỏng/thiếu phụ kiện phải có căn cứ và được xác nhận theo
    chính sách.
-   Nhận trả một phần không được đóng toàn bộ đơn.

## Acceptance Criteria

-   Mỗi món có trạng thái nhận trả riêng.
-   Có thể nhận trả từng phần.
-   Tài sản chưa kiểm tra không được hiển thị là sẵn sàng.
-   Phí phát sinh được ghi nhận minh bạch, có người xác nhận.

------------------------------------------------------------------------

# M10. Cleaning & Repair -- Vệ sinh và sửa chữa

## Mục tiêu

Quản lý công việc phục hồi tài sản sau khi thuê hoặc khi phát sinh sự
cố.

## M10.01 -- Phiếu vệ sinh

Thông tin: - Mã phiếu. - Inventory Item. - Nguồn phát sinh (sau trả/định
kỳ/sự cố). - Loại vệ sinh. - Người phụ trách. - Thời gian bắt đầu/kết
thúc. - Chi phí. - Ghi chú. - Kết quả nghiệm thu.

## M10.02 -- Phiếu sửa chữa

Thông tin: - Inventory Item. - Mô tả lỗi. - Hình ảnh. - Mức độ hư
hỏng. - Đơn thuê liên quan (nếu có). - Đơn vị/người sửa. - Dự kiến hoàn
thành. - Chi phí. - Kết quả sửa chữa. - Người nghiệm thu.

## M10.03 -- Nghiệm thu

-   Nhân viên thực hiện đánh dấu hoàn thành.
-   Người có quyền kiểm tra kết quả.
-   Nếu đạt, tài sản được chuyển sang `AVAILABLE` khi không có
    reservation/lịch chặn khác.
-   Nếu không đạt, tiếp tục sửa chữa hoặc chuyển `DAMAGED`.

## Quy tắc

-   Tài sản đang vệ sinh/sửa chữa không được chọn cho đơn mới trong
    khoảng thời gian tương ứng.
-   Lịch sửa chữa phải chặn availability.
-   Chi phí vệ sinh/sửa chữa được gắn với tài sản để tính hiệu quả khai
    thác.
-   Không được tự động mở khả dụng chỉ vì phiếu sửa chữa đã được đánh
    dấu hoàn thành; cần nghiệm thu nếu chính sách yêu cầu.

------------------------------------------------------------------------

# M11. Finance -- Tài chính

## Mục tiêu

Theo dõi đầy đủ các khoản phải thu, đã thu, tiền cọc, hoàn cọc và chi
phí phát sinh.

## M11.01 -- Cấu trúc khoản tiền

Một đơn có thể bao gồm: - Tiền thuê. - Giảm giá. - Phụ phí. - Phí
giao/nhận (nếu có). - Tiền cọc. - Phí trả trễ. - Phí hư hỏng/mất phụ
kiện. - Khoản hoàn/điều chỉnh.

**Tiền thuê và tiền cọc phải được theo dõi riêng.** Tiền cọc không được
cộng vào doanh thu thuê.

## M11.02 -- Ghi nhận thanh toán

Thông tin: - Đơn liên quan. - Loại giao dịch. - Số tiền. - Phương thức:
tiền mặt/chuyển khoản/thẻ/khác. - Thời điểm. - Người ghi nhận. - Mã tham
chiếu/chứng từ. - Ghi chú.

Loại giao dịch: - Thu tiền thuê. - Thu cọc. - Thu phí phát sinh. - Hoàn
tiền thuê. - Hoàn cọc. - Khấu trừ cọc. - Điều chỉnh theo quyền.

## M11.03 -- Quản lý tiền cọc

Trạng thái cọc đề xuất: - Chưa thu. - Thu một phần. - Đã thu đủ. - Chờ
quyết toán. - Hoàn một phần. - Đã hoàn đủ. - Đã khấu trừ một phần/toàn
bộ.

Khi kết thúc đơn: 1. Kiểm tra tất cả tài sản đã nhận trả. 2. Kiểm tra hư
hỏng/thiếu phụ kiện/phí trả trễ. 3. Xác định khoản được hoàn. 4. Ghi
nhận khoản khấu trừ có lý do và bằng chứng. 5. Ghi nhận hoàn cọc. 6. Lưu
người thực hiện và thời điểm.

## M11.04 -- Công nợ

-   Tổng phải thu.
-   Tổng đã thu.
-   Số còn phải thu.
-   Hạn thanh toán.
-   Trạng thái công nợ.
-   Lịch sử nhắc thu.

## M11.05 -- Sổ giao dịch

Cho phép lọc theo: - Khoảng ngày. - Loại giao dịch. - Phương thức thanh
toán. - Đơn. - Khách hàng. - Nhân viên ghi nhận.

Không cho sửa/xóa giao dịch đã xác nhận theo cách làm mất dấu vết. Nếu
sai, tạo giao dịch điều chỉnh/hoàn phù hợp.

## Acceptance Criteria

-   Tổng tiền thuê, tiền cọc và phí phát sinh được tách biệt.
-   Mỗi giao dịch có người thực hiện, thời gian và phương thức.
-   Hoàn cọc/khấu trừ phải có lý do.
-   Số dư công nợ được tính từ các giao dịch đã ghi nhận.
-   Giao dịch điều chỉnh không xóa lịch sử cũ.

------------------------------------------------------------------------

# M12. Notifications & Tasks

## Mục tiêu

Chủ động nhắc việc, giảm phụ thuộc vào nhân viên tự nhớ.

## Loại thông báo

  -----------------------------------------------------------------------
  Sự kiện                 Người nhận đề xuất      Thời điểm
  ----------------------- ----------------------- -----------------------
  Đơn sắp bàn giao        Sales, Warehouse,       Trước giờ hẹn theo cấu
                          Manager                 hình

  Đơn sắp trả             Sales, Warehouse        Trước hạn trả

  Đơn quá hạn             Sales, Manager          Khi quá hạn và theo chu
                                                  kỳ nhắc

  Reservation sắp hết hạn Sales                   Trước khi hết hạn

  Tài sản bị hỏng ảnh     Manager, Sales          Ngay khi phát sinh
  hưởng lịch tương lai                            

  Phiếu vệ sinh/sửa chữa  Operations, Manager     Khi quá hạn
  quá hạn                                         

  Cọc cần quyết toán      Accountant, Manager     Khi đơn đủ điều kiện
                                                  quyết toán

  Công nợ đến hạn         Sales, Accountant       Theo lịch nhắc
  -----------------------------------------------------------------------

## Quy tắc

-   Thông báo cần liên kết đến đúng đối tượng.
-   Có trạng thái đã đọc/chưa đọc.
-   Có thể cấu hình người nhận và thời điểm nhắc.
-   Không gửi thông báo trùng lặp không cần thiết.
-   Cần phân biệt thông báo hệ thống và công việc cần hành động.

------------------------------------------------------------------------

# M13. Reports & Analytics

## Mục tiêu

Cung cấp dữ liệu để đánh giá hiệu quả kinh doanh và quyết định mua/bảo
trì/thanh lý tài sản.

## Báo cáo đề xuất

### M13.01 -- Doanh thu

-   Doanh thu thuê theo ngày/tuần/tháng.
-   Doanh thu theo danh mục/Product.
-   Doanh thu theo nhân viên.
-   Doanh thu theo phương thức thanh toán.
-   Phí phát sinh.
-   Khoản hoàn/điều chỉnh.
-   Tiền cọc đang giữ (hiển thị riêng, không tính là doanh thu).

### M13.02 -- Đơn thuê

-   Số đơn theo trạng thái.
-   Giá trị đơn trung bình.
-   Tỷ lệ hủy.
-   Tỷ lệ trả trễ.
-   Thời gian thuê trung bình.
-   Số đơn phát sinh hư hỏng.

### M13.03 -- Hiệu suất tài sản

-   Lượt thuê theo Inventory Item.
-   Doanh thu thuê theo tài sản.
-   Số ngày khả dụng.
-   Số ngày được thuê.
-   Tỷ lệ sử dụng.
-   Chi phí vệ sinh/sửa chữa.
-   Ngày ngừng khai thác.
-   Thời gian hoàn vốn ước tính nếu có đủ dữ liệu.

Công thức tỷ lệ sử dụng cần được thống nhất. Đề xuất:

`Utilization Rate = Rental Occupied Days / Eligible Available Days × 100%`

Trong đó: - Rental Occupied Days là số ngày/đơn vị thời gian tài sản bị
chiếm dụng bởi đơn thuê. - Eligible Available Days là tổng thời gian tài
sản đủ điều kiện khai thác trong kỳ, loại trừ thời gian ngừng khai thác
có lý do. - Cần tránh tính trùng ngày/giờ khi có nhiều khoảng chiếm
dụng.

### M13.04 -- Kho và tình trạng tài sản

-   Tổng tài sản theo danh mục.
-   Số lượng theo trạng thái.
-   Tài sản hư hỏng.
-   Tài sản đang sửa chữa.
-   Tài sản thất lạc/thanh lý.
-   Tài sản lâu không được thuê.

### M13.05 -- Công nợ và cọc

-   Công nợ theo khách hàng.
-   Khoản cọc đang giữ.
-   Khoản cọc chờ hoàn.
-   Khoản khấu trừ.
-   Giao dịch theo kỳ.

## Acceptance Criteria

-   Có bộ lọc thời gian và xuất dữ liệu.
-   Báo cáo phải ghi rõ định nghĩa chỉ số.
-   Tổng báo cáo phải đối chiếu được với dữ liệu đơn/giao dịch.
-   Không cộng tiền cọc vào doanh thu thuê.

------------------------------------------------------------------------

# M14. Staff & Permissions

## Chức năng

-   Tạo/sửa/ngừng hoạt động tài khoản nhân viên.
-   Gán vai trò.
-   Gán quyền theo module và hành động.
-   Giới hạn xem dữ liệu tài chính.
-   Xem lịch sử thao tác.
-   Khóa/mở tài khoản.

## Quy tắc

-   Nhân viên nghỉ việc không được xóa lịch sử thao tác.
-   Tài khoản bị khóa không thể đăng nhập.
-   Các thao tác nhạy cảm phải ghi nhận actor và timestamp.
-   Không cấp quyền cao hơn cần thiết cho vai trò.

------------------------------------------------------------------------

# M15. Settings

## Nhóm cấu hình

### Cửa hàng

-   Tên, logo, địa chỉ, thông tin liên hệ.
-   Múi giờ.
-   Đơn vị tiền tệ.
-   Định dạng ngày giờ.

### Chính sách thuê

-   Cách tính ngày thuê.
-   Giờ nhận/trả mặc định.
-   Thời gian buffer.
-   Quy tắc tính phí trả trễ.
-   Quy tắc đặt cọc.
-   Chính sách hủy/hoàn tiền.
-   Chính sách hư hỏng/mất phụ kiện.

### Kho

-   Danh sách kho.
-   Khu vực/kệ/vị trí.
-   Quy tắc mã tài sản.
-   Mẫu QR.

### Checklist

-   Checklist chuẩn bị.
-   Checklist bàn giao.
-   Checklist nhận trả.
-   Checklist kiểm tra theo danh mục.

### Thông báo

-   Thời điểm nhắc.
-   Người nhận.
-   Kênh thông báo (trong hệ thống/email/SMS/ứng dụng nếu tích hợp).

------------------------------------------------------------------------

# 7. Quy trình nghiệp vụ đầu-cuối (End-to-End Workflows)

## WF01 -- Nhập hàng và đưa tài sản vào khai thác

1.  Nhân viên tạo phiếu nhập.
2.  Chọn Product/Variant hoặc tạo mẫu mới.
3.  Nhập số lượng thực nhận và giá nhập.
4.  Hệ thống tạo Inventory Item riêng cho từng món.
5.  Sinh mã tài sản và QR.
6.  Nhân viên kiểm tra ngoại quan, chụp ảnh và gán vị trí.
7.  Người có quyền xác nhận phiếu nhập.
8.  Tài sản được chuyển sang `AVAILABLE` nếu đủ điều kiện khai thác.

**Ngoại lệ:** hàng lỗi, thiếu, sai biến thể hoặc chưa hoàn thiện phải
được ghi nhận riêng và không đưa vào khả dụng.

## WF02 -- Tư vấn và tạo đơn thuê

1.  Sales tìm/tạo khách hàng.
2.  Chọn thời gian nhận và trả.
3.  Chọn Product/Variant.
4.  Hệ thống tìm Inventory Item khả dụng.
5.  Sales chọn tài sản cụ thể.
6.  Hệ thống tính giá, cọc và phụ phí.
7.  Sales lưu nháp hoặc tạo reservation có thời hạn.
8.  Khách xác nhận và thanh toán theo chính sách.
9.  Hệ thống kiểm tra availability lần cuối.
10. Đơn chuyển `CONFIRMED`; lịch được giữ chính thức.

**Ngoại lệ:** tài sản vừa được đặt bởi nhân viên khác, tài sản hỏng hoặc
lịch bị thay đổi. Hệ thống phải từ chối xác nhận và yêu cầu chọn tài sản
khác.

## WF03 -- Chuẩn bị và bàn giao

1.  Đơn được đưa vào danh sách chuẩn bị.
2.  Warehouse lấy hàng theo mã/QR.
3.  Thực hiện checklist vệ sinh và phụ kiện.
4.  Đánh dấu sẵn sàng.
5.  Khi khách đến, quét QR từng món.
6.  Kiểm tra đúng khách, đúng đơn, đúng tài sản.
7.  Chụp ảnh và xác nhận tình trạng.
8.  Khách/nhân viên xác nhận bàn giao.
9.  Cập nhật trạng thái từng dòng hàng và đơn.

## WF04 -- Nhận trả và kiểm tra

1.  Đơn xuất hiện trong danh sách cần nhận trả.
2.  Nhân viên mở đơn/quét QR.
3.  Xác nhận món thực tế được trả.
4.  Ghi nhận giờ trả và tình trạng ban đầu.
5.  Chụp ảnh.
6.  Thực hiện checklist kiểm tra.
7.  Nếu đạt: chuyển sang vệ sinh tiêu chuẩn.
8.  Nếu lỗi: tạo phiếu vệ sinh đặc biệt/sửa chữa/hư hỏng.
9.  Tính phí trả trễ hoặc khoản phát sinh nếu có.
10. Cập nhật trạng thái đơn theo số món đã trả.

## WF05 -- Quyết toán tiền cọc

1.  Xác nhận tất cả tài sản đã được nhận trả.
2.  Xác nhận kiểm tra tình trạng.
3.  Xác định phí trả trễ/hư hỏng/thiếu phụ kiện.
4.  Đối chiếu số tiền đã thu.
5.  Xác định số tiền cọc được hoàn.
6.  Người có quyền duyệt khoản khấu trừ/miễn giảm.
7.  Accountant ghi nhận hoàn cọc.
8.  Đơn hoàn tất khi thỏa điều kiện vận hành và tài chính.

## WF06 -- Sửa chữa và đưa tài sản trở lại khai thác

1.  Tạo phiếu sửa chữa từ kết quả kiểm tra hoặc phát hiện độc lập.
2.  Tài sản chuyển `REPAIRING` hoặc `DAMAGED`.
3.  Ghi nhận người sửa, chi phí và dự kiến hoàn thành.
4.  Nhân viên đánh dấu sửa xong.
5.  Người có quyền nghiệm thu.
6.  Nếu đạt, kiểm tra lịch đặt tương lai.
7.  Chuyển `AVAILABLE` nếu không còn lịch chặn hoặc điều kiện khác.

## WF07 -- Công việc cuối ngày

1.  Kiểm tra đơn bàn giao chưa hoàn tất.
2.  Kiểm tra đơn đến hạn trả/quá hạn.
3.  Kiểm tra tài sản đã nhận trả nhưng chưa kiểm tra.
4.  Kiểm tra phiếu vệ sinh/sửa chữa đang tồn.
5.  Kiểm tra khoản thu/cọc/công nợ.
6.  Đối chiếu giao dịch trong ngày.
7.  Ghi nhận các vấn đề cần bàn giao ca.

------------------------------------------------------------------------

# 8. Business Rules -- Quy tắc nghiệp vụ cốt lõi

  -----------------------------------------------------------------------
  Mã                                  Quy tắc
  ----------------------------------- -----------------------------------
  BR-001                              Mỗi Inventory Item phải có mã định
                                      danh duy nhất

  BR-002                              Một Inventory Item không được có
                                      hai khoảng chiếm dụng chồng lấn

  BR-003                              Availability phải tính cả rental
                                      period, buffer và lịch bảo trì

  BR-004                              Phải kiểm tra availability lại ngay
                                      trước khi xác nhận đơn

  BR-005                              Reservation nháp phải có thời hạn
                                      hết hiệu lực

  BR-006                              Đơn hết hạn reservation phải giải
                                      phóng tài sản theo cơ chế an toàn

  BR-007                              Tài sản vừa nhận trả phải chờ kiểm
                                      tra

  BR-008                              Tài sản đang vệ sinh/sửa
                                      chữa/hỏng/thất lạc/ngừng khai thác
                                      không khả dụng

  BR-009                              Bàn giao và nhận trả được quản lý ở
                                      cấp từng dòng hàng

  BR-010                              Đơn có thể bàn giao/nhận trả một
                                      phần

  BR-011                              Tiền cọc được theo dõi riêng, không
                                      tính vào doanh thu thuê

  BR-012                              Giá và chính sách được snapshot khi
                                      đơn xác nhận

  BR-013                              Giao dịch tài chính đã xác nhận
                                      không bị xóa/sửa mất dấu vết

  BR-014                              Hủy đơn phải có lý do và xử lý
                                      reservation/tài chính tương ứng

  BR-015                              Thay đổi lịch/tài sản phải kiểm tra
                                      xung đột

  BR-016                              Hư hỏng phải có ghi nhận, người
                                      thực hiện và bằng chứng phù hợp

  BR-017                              Mọi thay đổi trạng thái quan trọng
                                      phải có audit log

  BR-018                              Không xóa dữ liệu đã phát sinh giao
                                      dịch; sử dụng trạng thái ngừng hoạt
                                      động

  BR-019                              Tài sản hỏng ảnh hưởng đơn tương
                                      lai phải tạo cảnh báo

  BR-020                              Chỉ người có quyền mới được miễn
                                      phí, sửa giá, hoàn tiền hoặc khấu
                                      trừ cọc
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 9. Yêu cầu phi chức năng (Non-functional Requirements)

## 9.1. Bảo mật và phân quyền

-   Xác thực người dùng.
-   Phân quyền theo vai trò và hành động.
-   Không cho phép truy cập dữ liệu ngoài phạm vi quyền.
-   Ghi log thao tác nhạy cảm.
-   Bảo vệ thông tin cá nhân và chứng từ khách hàng.

## 9.2. Tính toàn vẹn dữ liệu

-   Tạo/xác nhận đơn và giữ lịch phải có cơ chế chống race condition.
-   Các thao tác nhiều bước quan trọng phải đảm bảo transaction hoặc cơ
    chế bù trừ.
-   Không để một tài sản được xác nhận cho hai đơn cùng thời gian.
-   Tính toán tiền phải dùng quy tắc làm tròn thống nhất.

## 9.3. Hiệu năng

Mục tiêu ban đầu để đội kỹ thuật kiểm chứng: - Danh sách phổ biến phản
hồi trong khoảng 2--3 giây với dữ liệu vận hành thông thường. - Tìm kiếm
theo mã QR/mã tài sản phản hồi gần như tức thời trong điều kiện mạng ổn
định. - Calendar cần phân trang/lọc theo nhóm tài sản để không tải toàn
bộ lịch sử không cần thiết.

Các ngưỡng cần được xác nhận bằng kiểm thử tải và quy mô dữ liệu dự
kiến.

## 9.4. Khả dụng và phục hồi

-   Có cơ chế backup dữ liệu.
-   Có log lỗi và theo dõi tác vụ nền.
-   Các tác vụ tự động như hết hạn reservation/thông báo phải có retry
    và giám sát.
-   Có quy trình phục hồi khi thao tác thanh toán hoặc đồng bộ thất bại.

## 9.5. Thiết bị và giao diện

-   Desktop là môi trường chính cho quản trị, báo cáo và tạo đơn phức
    tạp.
-   Tablet/mobile cần tối ưu cho quét QR, bàn giao, nhận trả và kiểm kê.
-   Các thao tác tại kho cần ít bước, nút bấm rõ, hỗ trợ thao tác một
    tay khi có thể.
-   Giao diện phải có loading, empty, error, success và confirmation
    states.

------------------------------------------------------------------------

# 10. Danh sách màn hình (Screen Inventory) đề xuất

  Mã màn hình   Tên màn hình              Mục đích
  ------------- ------------------------- ------------------------
  SCR-001       Login                     Đăng nhập
  SCR-002       Dashboard                 Tổng quan
  SCR-003       Daily Operations          Công việc trong ngày
  SCR-004       Notifications             Trung tâm thông báo
  SCR-010       Product List              Danh sách mẫu sản phẩm
  SCR-011       Product Create/Edit       Tạo/sửa mẫu
  SCR-012       Product Detail            Chi tiết mẫu
  SCR-013       Variant Management        Quản lý biến thể
  SCR-020       Inventory List            Danh sách tài sản
  SCR-021       Inventory Create          Tạo tài sản
  SCR-022       Inventory Detail          Chi tiết tài sản
  SCR-023       QR Scanner                Quét QR
  SCR-024       Inventory Calendar        Lịch tài sản
  SCR-030       Stock Receipt List        Danh sách phiếu nhập
  SCR-031       Stock Receipt Create      Tạo phiếu nhập
  SCR-032       Stocktake                 Kiểm kê
  SCR-040       Customer List             Danh sách khách
  SCR-041       Customer Detail           Chi tiết khách
  SCR-050       Rental Order List         Danh sách đơn thuê
  SCR-051       Rental Order Create       Tạo đơn thuê
  SCR-052       Rental Order Detail       Chi tiết đơn
  SCR-053       Rental Order Edit         Điều chỉnh đơn
  SCR-054       Rental Contract/Receipt   Hợp đồng/biên nhận
  SCR-060       Rental Calendar           Lịch thuê tổng
  SCR-061       Availability Search       Tìm tài sản khả dụng
  SCR-070       Preparation Queue         Danh sách chuẩn bị
  SCR-071       Handover                  Bàn giao
  SCR-072       Return Queue              Danh sách nhận trả
  SCR-073       Return & Inspection       Nhận trả/kiểm tra
  SCR-080       Cleaning Queue            Danh sách vệ sinh
  SCR-081       Repair List               Danh sách sửa chữa
  SCR-082       Repair Detail             Chi tiết phiếu sửa
  SCR-090       Transactions              Sổ giao dịch
  SCR-091       Deposit Management        Quản lý cọc
  SCR-092       Receivables               Công nợ
  SCR-100       Reports Overview          Tổng quan báo cáo
  SCR-101       Revenue Report            Báo cáo doanh thu
  SCR-102       Asset Performance         Hiệu suất tài sản
  SCR-103       Rental Report             Báo cáo đơn thuê
  SCR-110       Staff List                Danh sách nhân viên
  SCR-111       Role & Permission         Vai trò/phân quyền
  SCR-120       Store Settings            Thiết lập cửa hàng
  SCR-121       Rental Policies           Chính sách thuê
  SCR-122       Checklist Settings        Cấu hình checklist

Danh sách này là inventory ở mức sản phẩm; PO và UX Designer có thể gộp
màn hình thành tab/drawer/modal nếu giảm thao tác mà không làm mất khả
năng hoàn thành nghiệp vụ.

------------------------------------------------------------------------

# 11. Ưu tiên triển khai

## 11.1. MVP -- Bắt buộc để vận hành

1.  Đăng nhập và phân quyền cơ bản.
2.  Product, Variant và Inventory Item.
3.  QR cho từng tài sản.
4.  Khách hàng.
5.  Tạo đơn thuê và kiểm tra availability.
6.  Reservation có thời hạn.
7.  Xác nhận đơn và snapshot giá.
8.  Rental Calendar.
9.  Bàn giao và nhận trả từng món.
10. Checklist kiểm tra sau trả.
11. Ghi nhận thanh toán và tiền cọc.
12. Dashboard công việc trong ngày.
13. Trạng thái tài sản và audit log cơ bản.

## 11.2. Phase 2 -- Kiểm soát vận hành và tài chính

1.  Phiếu nhập và kiểm kê.
2.  Vệ sinh/sửa chữa/nghiệm thu.
3.  Hoàn cọc, khấu trừ và công nợ đầy đủ.
4.  Thông báo tự động.
5.  Báo cáo doanh thu và hiệu suất tài sản.
6.  Hợp đồng/biên bản và xuất dữ liệu.

## 11.3. Phase 3 -- Mở rộng

1.  Nhiều chi nhánh.
2.  Khách tự đặt online.
3.  Thanh toán trực tuyến.
4.  Tích hợp SMS/email/Zalo.
5.  CRM/loyalty.
6.  Báo cáo nâng cao và dự báo nhu cầu.

------------------------------------------------------------------------

# 12. Các quyết định nghiệp vụ PO cần xác nhận

Các nội dung dưới đây cần chủ cửa hàng/PO quyết định trước khi khóa đặc
tả:

  -----------------------------------------------------------------------
  Mã                      Câu hỏi cần quyết định  Đề xuất mặc định
  ----------------------- ----------------------- -----------------------
  DEC-001                 Cách tính ngày thuê:    Cấu hình được; MVP chọn
                          theo 24 giờ hay ngày    một quy tắc thống nhất
                          lịch?                   

  DEC-002                 Thời gian buffer        Cấu hình mặc định toàn
                          trước/sau thuê?         cửa hàng, cho phép
                                                  override có quyền

  DEC-003                 Đơn nháp có giữ hàng    Không; chỉ reservation
                          không?                  có expiry mới giữ

  DEC-004                 Reservation hết hạn sau Cấu hình theo cửa hàng
                          bao lâu?                

  DEC-005                 Có bắt buộc thu cọc để  Cấu hình theo chính
                          xác nhận?               sách

  DEC-006                 Có cho thuê khi khách   Mặc định cảnh báo/chặn
                          còn công nợ?            theo quyền

  DEC-007                 Ai được giảm giá/miễn   Sales trong hạn mức;
                          phí?                    vượt hạn mức cần duyệt

  DEC-008                 Ai được hoàn cọc/khấu   Accountant thực hiện,
                          trừ?                    Manager/Owner duyệt
                                                  theo ngưỡng

  DEC-009                 Phí trả trễ tính theo   Cấu hình quy tắc và mức
                          giờ hay ngày?           phí

  DEC-010                 Hàng trả ngoài giờ xử   Ghi nhận thời điểm nhận
                          lý thế nào?             thực tế, kiểm tra sau

  DEC-011                 Có cho thay tài sản sau Có quy trình điều chỉnh
                          khi đã bàn giao một     và lưu lịch sử
                          phần?                   

  DEC-012                 Checklist có khác nhau  Có; cấu hình theo
                          theo danh mục?          category

  DEC-013                 Có cần lưu ảnh          Chỉ triển khai khi có
                          CCCD/giấy tờ khách?     nhu cầu và chính sách
                                                  bảo mật rõ

  DEC-014                 Có quản lý giao         Để Phase 2 nếu chưa
                          hàng/nhận hàng tận nơi? phải quy trình cốt lõi

  DEC-015                 Có nhiều chi nhánh/kho  MVP một cửa hàng/kho;
                          ngay từ đầu?            thiết kế dữ liệu không
                                                  khóa cứng
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 13. Rủi ro nghiệp vụ và biện pháp kiểm soát

  -----------------------------------------------------------------------
  Rủi ro                  Tác động                Kiểm soát đề xuất
  ----------------------- ----------------------- -----------------------
  Đặt trùng lịch          Không có hàng giao      Transaction kiểm tra
                          khách                   availability khi xác
                                                  nhận

  Nhân viên chọn sai món  Giao sai size/màu/tài   QR scan tại bàn giao
                          sản                     

  Hàng trả về nhưng chưa  Giao tiếp hàng lỗi      Trạng thái chờ kiểm tra
  kiểm tra                                        bắt buộc

  Quên hoàn cọc           Khiếu nại, mất uy tín   Task và cảnh báo cọc
                                                  chờ quyết toán

  Sửa/xóa giao dịch       Sai lệch tài chính      Ledger/audit, giao dịch
                                                  điều chỉnh

  Hư hỏng ảnh hưởng lịch  Không có hàng thay thế  Cảnh báo các
  sau                                             reservation tương lai

  Reservation treo        Tài sản bị khóa không   Expiry và job giải
                          cần thiết               phóng có giám sát

  Mất QR/tem              Chậm vận hành           Tìm theo mã, in lại QR
                                                  nhưng giữ nguyên ID

  Nhân viên vượt quyền    Rủi ro tài chính/dữ     RBAC và audit log
                          liệu                    

  Báo cáo sai định nghĩa  Quyết định kinh doanh   Chuẩn hóa metric và đối
                          sai                     soát dữ liệu
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 14. Yêu cầu UX/UI từ góc nhìn PO

## 14.1. Nguyên tắc điều hướng

-   Sidebar chia theo nhóm công việc: Tổng quan, Đơn thuê, Lịch thuê,
    Sản phẩm & Kho, Vận hành, Tài chính, Báo cáo, Quản trị.
-   Ưu tiên các tác vụ thường xuyên: Tạo đơn, xem lịch, bàn giao, nhận
    trả, quét QR.
-   Không tạo trang riêng nếu một tab/drawer/modal giúp hoàn thành tác
    vụ nhanh hơn và vẫn dễ hiểu.
-   Tránh đưa toàn bộ dữ liệu vào một màn hình dài; dùng phân nhóm, tab
    và progressive disclosure.

## 14.2. Dashboard

-   Tập trung vào công việc cần xử lý, không chỉ là biểu đồ.
-   Các KPI cần click để mở danh sách chi tiết.
-   Cảnh báo quan trọng phải có nguyên nhân và hành động tiếp theo.

## 14.3. Rental Order Create

Đây là một trong những màn hình quan trọng nhất: - Luồng theo bước rõ
ràng. - Khách hàng và thời gian thuê ở phần đầu. - Tìm tài sản theo
size/màu/danh mục. - Lịch khả dụng hiển thị ngay trong quá trình chọn. -
Tổng tiền, cọc và phí phát sinh luôn dễ quan sát. - Cảnh báo xung đột
phải chỉ rõ món nào và vì sao.

## 14.4. Rental Calendar

-   Ưu tiên timeline theo từng Inventory Item.
-   Có thể lọc theo danh mục, biến thể và kho.
-   Phân biệt trực quan reservation, confirmed, rented, cleaning và
    repair.
-   Không chỉ dùng màu; cần có nhãn/trạng thái hỗ trợ khả năng tiếp cận.

## 14.5. Handover/Return

-   Tối ưu cho tablet/mobile.
-   Nút quét QR dễ truy cập.
-   Mỗi món là một dòng checklist độc lập.
-   Có thể lưu tiến trình bàn giao/nhận trả một phần.
-   Ảnh và xác nhận phải gắn với đúng món và đúng thời điểm.

## 14.6. Các trạng thái giao diện bắt buộc

Mỗi màn hình cần thiết kế: - Loading. - Empty state. - Error state. -
Permission denied. - Validation error. - Success confirmation. - Data
unavailable. - Conflict state (đặc biệt với lịch thuê). - Unsaved
changes. - Partial completion.

------------------------------------------------------------------------

# 15. Definition of Ready & Definition of Done

## 15.1. Definition of Ready (DoR)

Một chức năng sẵn sàng đưa vào Sprint khi: - Mục tiêu và người dùng đã
rõ. - Luồng chính và luồng ngoại lệ đã mô tả. - Business rules đã xác
định. - Acceptance Criteria có thể kiểm thử. - UI/UX hoặc wireframe đã
được thống nhất nếu cần. - API/data dependency đã được nhận diện. -
Quyền truy cập đã được xác định.

## 15.2. Definition of Done (DoD)

Một chức năng được xem là hoàn thành khi: - Đáp ứng Acceptance
Criteria. - Kiểm tra quyền truy cập ở frontend và backend. - Có
validation và xử lý lỗi. - Có audit log cho thao tác nhạy cảm. - Không
phá vỡ các quy tắc lịch thuê/tài chính. - Có kiểm thử luồng chính và
ngoại lệ. - UI có đủ trạng thái loading/empty/error/success. - Được
PO/đại diện nghiệp vụ nghiệm thu.

------------------------------------------------------------------------

# 16. Backlog cấp Epic đề xuất

  Epic    Tên                       Kết quả mong đợi
  ------- ------------------------- -----------------------------------------
  EP-01   Identity & Access         Người dùng đăng nhập và được phân quyền
  EP-02   Product Catalog           Quản lý mẫu và biến thể
  EP-03   Asset Inventory           Quản lý từng món hàng vật lý và QR
  EP-04   Stock Management          Nhập hàng, kiểm kê và điều chỉnh
  EP-05   Customer Management       Hồ sơ và lịch sử khách
  EP-06   Rental Order Management   Tạo và quản lý đơn thuê
  EP-07   Availability & Calendar   Không đặt trùng và xem lịch
  EP-08   Handover Operations       Chuẩn bị và bàn giao
  EP-09   Return & Inspection       Nhận trả và kiểm tra
  EP-10   Cleaning & Repair         Vệ sinh, sửa chữa và nghiệm thu
  EP-11   Payment & Deposit         Thu tiền, cọc, hoàn tiền và công nợ
  EP-12   Tasks & Notifications     Nhắc việc và cảnh báo
  EP-13   Reporting                 Báo cáo vận hành và tài sản
  EP-14   Administration            Nhân sự, quyền và cấu hình

------------------------------------------------------------------------

# 17. User Stories mẫu và Acceptance Criteria

## US-01 -- Tạo mẫu sản phẩm

**As a** Admin\
**I want** tạo mẫu sản phẩm và các biến thể size/màu\
**So that** cửa hàng có cấu trúc danh mục nhất quán.

Acceptance Criteria: - Có thể tạo Product với tên và danh mục bắt
buộc. - Có thể khai báo nhiều biến thể. - Có thể đánh dấu Free Size. -
Không tạo trùng mã Product. - Product đã được sử dụng không thể xóa
cứng.

## US-02 -- Tạo tài sản vật lý

**As a** Warehouse Staff\
**I want** tạo từng Inventory Item và QR riêng\
**So that** mỗi món có thể được nhận diện và theo dõi độc lập.

Acceptance Criteria: - Tạo tài sản phải chọn Product/Variant. - Mã tài
sản không trùng. - Hệ thống sinh QR. - Lưu được giá nhập và vị trí. - Có
thể in/tải tem QR.

## US-03 -- Tìm tài sản khả dụng

**As a** Sales\
**I want** tìm các tài sản còn khả dụng theo khoảng thời gian thuê\
**So that** tôi không tư vấn món đã được đặt.

Acceptance Criteria: - Kết quả phụ thuộc thời gian nhận/trả. - Tính cả
buffer và lịch bảo trì. - Tài sản không khả dụng có lý do rõ ràng. - Dữ
liệu được kiểm tra lại lúc xác nhận đơn.

## US-04 -- Xác nhận đơn thuê

**As a** Sales/Manager\
**I want** xác nhận đơn thuê sau khi kiểm tra lịch và điều kiện thanh
toán\
**So that** tài sản được giữ chắc chắn cho khách.

Acceptance Criteria: - Hệ thống kiểm tra xung đột lần cuối. - Nếu có
xung đột, không cho xác nhận. - Giá/chính sách được snapshot. -
Reservation được chuyển thành lịch đã xác nhận. - Ghi nhận người xác
nhận và thời gian.

## US-05 -- Bàn giao một phần

**As a** Warehouse Staff\
**I want** bàn giao từng món trong đơn\
**So that** trường hợp khách nhận hàng nhiều đợt vẫn được theo dõi chính
xác.

Acceptance Criteria: - Có thể xác nhận từng dòng hàng. - Có thể lưu tiến
trình một phần. - Mỗi món có thời gian và người bàn giao. - Trạng thái
đơn phản ánh đúng số món đã bàn giao.

## US-06 -- Nhận trả và kiểm tra

**As a** Warehouse Staff\
**I want** nhận trả và ghi nhận tình trạng từng món\
**So that** cửa hàng kiểm soát được hư hỏng và thời điểm tài sản sẵn
sàng trở lại.

Acceptance Criteria: - Có thể nhận trả một phần. - Lưu giờ nhận và ảnh
tình trạng. - Tài sản chuyển sang chờ kiểm tra. - Không tự động trở
thành khả dụng trước khi hoàn tất quy trình.

## US-07 -- Quyết toán cọc

**As a** Accountant\
**I want** xem số cọc, khoản khấu trừ và số tiền cần hoàn\
**So that** cửa hàng quyết toán minh bạch với khách.

Acceptance Criteria: - Tiền cọc tách khỏi doanh thu. - Có thể ghi nhận
hoàn một phần/toàn bộ. - Khấu trừ cần lý do và người có quyền. - Lưu
lịch sử giao dịch.

## US-08 -- Xem hiệu suất tài sản

**As a** Owner\
**I want** xem lượt thuê, doanh thu và chi phí theo từng tài sản\
**So that** tôi biết món nào khai thác tốt và món nào cần thay thế/thanh
lý.

Acceptance Criteria: - Có bộ lọc thời gian. - Hiển thị lượt thuê và
doanh thu theo Inventory Item. - Hiển thị chi phí sửa chữa/vệ sinh nếu
đã ghi nhận. - Công thức chỉ số được mô tả rõ.

------------------------------------------------------------------------

# 18. Câu hỏi mở cần làm rõ trong buổi Discovery

1.  Cửa hàng có một hay nhiều kho/chi nhánh ở giai đoạn đầu?
2.  Thời gian thuê được tính theo ngày lịch hay theo số giờ/24 giờ?
3.  Có cho phép đặt trước nhiều tháng không?
4.  Có cần giữ lịch khi khách chỉ mới hỏi giá không?
5.  Khách cần đặt cọc bao nhiêu phần trăm hoặc số tiền cố định?
6.  Có cho khách thanh toán nhiều lần không?
7.  Có giao hàng tận nơi và thu phí vận chuyển không?
8.  Có cho phép khách đổi size/mẫu sau khi đã xác nhận hoặc nhận một
    phần hàng không?
9.  Trả hàng ngoài giờ có được tính là trả đúng hạn không?
10. Phí trả trễ tính theo giờ, ngày hay mức cố định?
11. Hư hỏng được định giá theo bảng phí hay nhân viên đề xuất?
12. Ai có quyền miễn/giảm phí và ngưỡng phê duyệt?
13. Có cần lưu ảnh giấy tờ tùy thân của khách không?
14. Có cần ký hợp đồng điện tử hoặc ký trực tiếp trên thiết bị?
15. Có cần theo dõi phụ kiện đi kèm như một tài sản riêng có QR hay chỉ
    là checklist?
16. Có quản lý hàng thuê theo combo/set không?
17. Một món có thể có nhiều lịch giá theo ngày thường/cuối tuần/mùa cao
    điểm không?
18. Có cần tích hợp kế toán, hóa đơn, ngân hàng hoặc nền tảng nhắn tin
    ngay trong MVP?

------------------------------------------------------------------------

# 19. Kết luận và hướng sử dụng tài liệu

Tài liệu này là baseline nghiệp vụ ở góc nhìn Product Owner, không phải
thiết kế database hay đặc tả API cuối cùng.

Trình tự sử dụng đề xuất: 1. PO/Owner xác nhận mục tiêu, phạm vi và các
quyết định nghiệp vụ ở mục 12 và 18. 2. PO cùng UX Designer chuyển
module và workflow thành Information Architecture, user flow và screen
specification. 3. UX Designer thiết kế các màn hình trọng yếu:
Dashboard, Rental Order Create, Rental Calendar, Inventory Detail,
Handover và Return/Inspection. 4. PO bổ sung Acceptance Criteria chi
tiết cho từng story trước khi đưa vào Sprint. 5. Tech Lead xác nhận mô
hình dữ liệu, transaction, concurrency, audit và tích hợp. 6. QA chuyển
Business Rules và Acceptance Criteria thành test scenarios.

**Thứ tự ưu tiên xác nhận nghiệp vụ:** Availability/Calendar → Rental
Order → Handover/Return → Deposit/Finance → Asset Lifecycle. Đây là các
phần có rủi ro vận hành và tài chính cao nhất nếu hiểu sai nghiệp vụ.
