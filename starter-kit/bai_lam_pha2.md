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
"ghi_chu": "hỗ trợ tìm chỗ"
}
]
```

**Link trò chuyện:** https://share.gemini.google/7kOK0moqmzqP

## 4.3. Báo giá vé bằng Chain-of-Thought

**KET QUA AI**

```
1.  **Tiền người lớn:**
    
    -   Phép tính:  $5 \times 1.200.000 = 6.000.000$ đồng.
        
2.  **Tiền trẻ em:**
    
    -   Có 2 trẻ em từ 5 đến 11 tuổi (8 tuổi và 10 tuổi) tính 70% giá người lớn; 1 bé 3 tuổi dưới 5 tuổi được miễn phí.
        
    -   Phép tính:  $2 \times (1.200.000 \times 70\%) = 1.680.000$ đồng.
        
3.  **Tiền vé (trước giảm):**
    
    -   Phép tính:  $6.000.000 + 1.680.000 = 7.680.000$ đồng.
        
4.  **Giảm giá nhóm:**
    
    -   Nhóm có 5 người lớn (đạt điều kiện $\ge 5$ người lớn), được giảm 5% trên tổng tiền vé.
        
    -   Phép tính:  $7.680.000 \times 5\% = 384.000$ đồng.
        
    -   Tiền vé sau giảm:  $7.680.000 - 384.000 = 7.296.000$ đồng.
        
5.  **Phụ thu:**
    
    -   Có 1 người lớn chọn khu ghế ưu tiên (phụ thu 300.000 đồng/người).
        
    -   Phép tính:  $1 \times 300.000 = 300.000$ đồng.
        
6.  **Tổng tiền:**
    
    -   Phép tính:  $7.296.000 + 300.000 = 7.596.000$ đồng.
        
[RESULT: 7.596.000 đồng]
```

**Link trò chuyện:** https://share.gemini.google/93IshNjluV85

### b) Kiểm tra kết quả AI

AI đúng 

## 4.4. Dây chuyền xử lý khiếu nại – kết quả Trạm 1

**KET QUA AI**

```
{ 
"loai_su_co": "khac", 
"so_khach": 0, 
"so_hiep_bi_anh_huong": null, 
"tom_tat": "Chưa có nội dung email khiếu nại được cung cấp." 
}
```

**Link trò chuyện:** https://share.gemini.google/l9fJthIwnzQx

