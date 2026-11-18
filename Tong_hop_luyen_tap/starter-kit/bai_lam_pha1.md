## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khán giả [TEN_KHACH_HANG], số điện thoại [SO_DIEN_THOAI_KHACH_HANG], số CCCD [SO_CCCD_KHACH_HANG], email [EMAIL_KHACH_HANG] , đặt 3 vé gói SVT trận bán kết Cúp Sao Vàng ngày 18/12/2026 cho 2 người lớn và 1 trẻ em 9 tuổi. Khán giả đã chuyển khoản đặt cọc 1.500.000 đồng. Khán giả yêu cầu 3 ghế ngồi cạnh nhau gần lối đi.

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
Câu 1: dựa trên file chính sách hoàn/huỷ vé hãy trả lời câu hỏi trên của tôi . Nếu các thông tin không có trong file chính sách trên thì hãy trả lời là "Chính sách và điều khoản này không tồn tại" tuyêth đối không được bịa thêm ra.
Câu 2: khi bạn trả lời câu hỏi của tôi thì hãy ghi rõ câu trả lời lấy từ mục hoặc điều khoản nào trong file.
```

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**PROMPT**

```text
[Context] Bạn là một chuyên viên phân tích tin nhắn khách hàng , bạn có nhiệm vụ chuyển các tin nhắn thành định dạng JSON.

[Request] Bạn hãy chuyển dữ liệu TN1, TN2, TN3 trong thẻ <tin_nhan> thành một mảng JSON 3 đối tượng, đúng thứ tự, đúng 6 trường như sau "ma_goi", "ngay_su_kien", "nguoi_lon", "tre_em", "loai_ve", "ghi_chu".

[Explanation] khi chuyển thành JSON bạn cần bám theo 5 quy tắc chuẩn hoá trong thẻ <quy_chuan>. Bạn có thể trả lời đúng theo ví dụ tôi cho ở thẻ <vi_du>.
<quy_chuẩn>
SVT – Sân Vận Động: vé trận đấu
FZ – Fan Zone: khu cổ vũ
VIP – Khán đài VIP

QUY TẮC CHUẨN HOÁ (5 quy tắc)
1. ma_goi lấy theo bảng mã gói.
2. ngay_su_kien dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
4. loai_ve chỉ nhận "thuong", "vip" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về chỗ ngồi, hỗ trợ di chuyển hoặc ăn uống; không có thì null.
</quy_chuan>

<vi_du>
Tin nhắn mẫu 1:
Đặt giúp mình 2 vé VIP trận chung kết ngày 15/11/2026, 2 người lớn và 1 bé. Cần chỗ ngồi cạnh lối đi.
Kết quả đúng:
{"ma_goi": "VIP", "ngay_su_kien": "2026-11-15", "nguoi_lon": 2, "tre_em": 1, "loai_ve": "vip", "ghi_chu": "chỗ ngồi cạnh lối đi"}

Tin nhắn mẫu 2:
Cho mình một vé Fan Zone, ngày nào cũng được, mình đi một mình.
Kết quả đúng:
{"ma_goi": "FZ", "ngay_su_kien": null, "nguoi_lon": 1, "tre_em": 0, "loai_ve": "thuong", "ghi_chu": null}
</vi_du>

câu trả lời không chào hỏi, không giải thích chỉ có dữ liệu JSON trả về

[Action] đọc dữ liệu của tin nhắn khách hàng mà tôi đưa => chuẩn hoá về đúng các trường dữ liệu => trả đúng về định dạng JSON.

[Tone] chuyên nghiệp, ngắn gọn, xúc tích

[Extras] định dạng đầu ra chỉ trả về JSON.
<tin_nhan>
    N1: Mình muốn đặt gói Sân Vận Động trận tứ kết ngày 20/12 nha, 2 người lớn với 1 bé 8 tuổi, vé thường.

    TN2: Cho 3 người đi Fan Zone trận ngày 05/01/2027 ạ. Một người cần hỗ trợ xe lăn.

    TN3: Trận chung kết Cúp Sao Vàng tháng 12 còn vé không ạ? Nhà em 4 người lớn, có một người khiếm thị cần hỗ trợ tìm chỗ, lấy vé VIP.
</tin_nhan>

```

## 4.3. Báo giá vé bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
[Context] bạn là nhân viên bán vé chuyên nghiệp, bạn có nhiệm vụ tính tiền vé để cho khách hàng thanh toán
[Request] bạn hãy tính tổng tiền vé theo yêu cầu ở thẻ <yeu_cau> và dòng cuối có dạng [RESULT: <tổng tiền> đồng].

[Explanation] bạn hãy tính tiền vé theo bảng giá ở trong thẻ <bang_gia> và theo quy tắc tính giá trong thẻ <quy_tac>

<bang_gia> 
BẢNG GIÁ VÉ (đồng/người lớn)
SVT – Sân Vận Động: 1.200.000
FZ – Fan Zone: 800.000
VIP – Khán đài VIP: 2.500.000
</bang_gia>

<quy_tac>
QUY TẮC TÍNH GIÁ
1. Trẻ em từ 5 đến 11 tuổi tính 70% giá người lớn. Trẻ dưới 5 tuổi miễn phí.
2. Nhóm từ 5 người lớn trở lên được giảm 5% trên tiền vé (tiền người lớn + tiền trẻ em).
3. Phụ thu khu ghế ưu tiên: 300.000 đồng/người. Phụ thu không được giảm giá.
4. Tổng tiền = tiền vé sau giảm + phụ thu.
</quy_tac>

Tuyệt đối không bịa thêm bảng giá khác hay thêm quy tắc khác
[Action] đọc dữ liệu => suy luận từng bước theo thứ tự tiền người lớn → tiền trẻ em → tiền vé → giảm giá nhóm → phụ thu → tổng tiền
mỗi bước ghi phép tính
[Tone] nghiêm chỉnh, chuyên nghiệp ,ngắn gọn,  đúng trọng tâm
[Extras] ở dòng cuối trả về theo dạng [RESULT: <tổng tiền> đồng]
<yeu_cau>
YÊU CẦU BÁO GIÁ
Nhóm cổ động viên đi xem trận bán kết, mua gói SVT, gồm 5 người lớn, 2 trẻ em (8 tuổi và 10 tuổi) và 1 bé 3 tuổi. Có 1 người lớn chọn khu ghế ưu tiên.
</yeu_cau>

```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
bạn hãy đọc email, trả về chỉ một đối tượng JSON gồm đúng 4 trường:
{"loai_su_co": một trong "tran_gio" | "sai_khu_vuc" | "nhan_vien" | "khac", "so_khach": số nguyên, "so_hiep_bi_anh_huong": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}
```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. xem đã đúng định dạng JSON chưa

2. xem đã đủ 4 trường (bao gồm: "loai_su_co", "so_khach", "so_hiep_bi_anh_huong", "tom_tat") hay chưa

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
câu lệnh trên đang dừng ở "Ban tổ chức Cúp Sao Vàng xin hoàn lại Quý khán giả khoản" bạn hãy tiếp tục câu lệnh trên mà không lặp lại phần đã có
```
