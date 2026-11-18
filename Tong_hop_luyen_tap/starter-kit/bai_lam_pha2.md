## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**KET QUA AI**

```
[
{
"ma_goi": "SVT",
"ngay_su_kien": "2026-12-20",
"nguoi_lon": 2,
"tre_em": 1,
"loai_ve": "thuong",
"ghi_chu": null
},
{
"ma_goi": "FZ",
"ngay_su_kien": "2027-01-05",
"nguoi_lon": 3,
"tre_em": 0,
"loai_ve": "thuong",
"ghi_chu": "hỗ trợ xe lăn"
},
{
"ma_goi": "VIP",
"ngay_su_kien": null,
"nguoi_lon": 4,
"tre_em": 0,
"loai_ve": "vip",
"ghi_chu": "hỗ trợ tìm chỗ cho người khiếm thị"
}
]
```

**Link trò chuyện:https://share.gemini.google/3euGQkrt2q89**

## 4.3. Báo giá vé bằng Chain-of-Thought

**KET QUA AI**

```text
Tiền người lớn:
5 người lớn × 1.200.000 = 6.000.000 đồng

Tiền trẻ em:

2 trẻ em (8 tuổi và 10 tuổi thuộc nhóm từ 5 đến 11 tuổi, tính 70% giá người lớn): 2 × (1.200.000 × 70%) = 2 × 840.000 = 1.680.000 đồng

1 bé 3 tuổi (dưới 5 tuổi): 0 đồng

Tổng tiền trẻ em: 1.680.000 + 0 = 1.680.000 đồng

Tiền vé (trước giảm):
6.000.000 + 1.680.000 = 7.680.000 đồng

Giảm giá nhóm:

Nhóm có 5 người lớn (đạt điều kiện từ 5 người lớn trở lên) nên được giảm 5% tiền vé: 7.680.000 × 5% = 384.000 đồng

Tiền vé sau giảm: 7.680.000 - 384.000 = 7.296.000 đồng

Phụ thu khu ghế ưu tiên:
1 người lớn × 300.000 = 300.000 đồng

Tổng tiền thanh toán:
7.296.000 + 300.000 = 7.596.000 đồng

[RESULT: 7.596.000 đồng]
```

**Link trò chuyện:https://share.gemini.google/pYfyfjlCcbb6**

### b) Kiểm tra kết quả AI

AI trả lời đúng

## 4.4. Dây chuyền xử lý khiếu nại – kết quả Trạm 1

**KET QUA AI**

```text
```json
{
  "loai_su_co": "sai_khu_vuc",
  "so_khach": 2,
  "so_hiep_bi_anh_huong": 2,
  "tom_tat": "Khán giả mua vé gói SVT bị xếp sai sang khu không mái che khi trời mưa trong 2 hiệp đầu."
}

```
```

**Link trò chuyện:https://share.gemini.google/2HtBSBkoIcoP**
