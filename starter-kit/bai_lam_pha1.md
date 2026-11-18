## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khán giả [Ten_Khach_Hang], số điện thoại [SDT_Ca_Nhan], số CCCD 079201009876, email quan.nguyen@example.com, đặt 3 vé gói SVT trận bán kết Cúp Sao Vàng ngày 18/12/2026 cho 2 người lớn và 1 trẻ em 9 tuổi. Khán giả đã chuyển khoản đặt cọc 1.500.000 đồng. Khán giả yêu cầu 3 ghế ngồi cạnh nhau gần lối đi.
```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
Câu 1: dựa trên file chính sách hoàn/hủy vé mà tôi gửi hãy trả lời câu hỏi trên của tôi. Nếu các thông tin có trong chính sách thì không tự bịa 
Câu 2: khi bạn trả lời cho tôi thì ghi rõ câu trả lời lấy từ mục hoặc điều khoản nào trong file chính sách hoàn/hủy.
```

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**PROMPT**

```text 
C-R-E-A-T-E

[Context]: Bạn là một chuyên gia phân tích tin nhắn khách hàng, bạn có nhiệm vụ chuyển các tin nhắn thành định dạng JSON

[Request]: Bạn hãy chuyển TN1, TN2, TN3 trong thẻ <tin_nhan> thành một mảng JSON 3 đối tượng, đúng thứ tự, đúng 6 trường như sau "ma_goi", "ngay_su_kien",  "nguoi_lon", "tre_em", "loai_ve", "ghi_chu". Như tin nhắn mẫu 

[Explanation]: Khi chuyển thành JSON bạn cần bám theo quy tắc chuẩn hóa trong thẻ <quy_chuan>. Bạn ccótheer xem qua thử 2 ví dụ mẫu mà tôi đã thực hiện trong thẻ <vi_du> 

<quy_chuan> 
	BẢNG MÃ GÓI
	SVT – Sân Vận Động: vé trận đấu
	FZ – Fan Zone: khu cổ vũ
	VIP – Khán đài VIP

	QUY TẮC CHUẨN HOÁ (5 quy tắc)
	1. ma_goi lấy theo bảng mã gói.
	2. ngay_su_kien dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
	3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
	4. loai_ve chỉ nhận "thuong", "vip" hoặc null.
	5. ghi_chu chỉ ghi yêu cầu đặc biệt về chỗ ngồi, hỗ trợ di chuyển hoặc ăn uống; không có thì null.
	
	Câu trả lời không chào hỏi, không giải thích chỉ có dữ liệu JSON trả về

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

[Action]: Đọc dữ liệu của TN khách hàng mà tôi đưa => Chuẩn hóa về đúng các trường dữ liệu => Trả đúng về định dạng JSON

[Tone]: chuyên nghiệp, ngắn gọn, xúc tích

[Extras]: Định dạng đầu ra chỉ là dữ liệu JSON

<tin_nhan>
	TN1: Mình muốn đặt gói Sân Vận Động trận tứ kết ngày 20/12 nha, 2 người lớn với 1 bé 8 tuổi, vé thường.

	TN2: Cho 3 người đi Fan Zone trận ngày 05/01/2027 ạ. Một người cần hỗ trợ xe lăn.

	TN3: Trận chung kết Cúp Sao Vàng tháng 12 còn vé không ạ? Nhà em 4 người lớn, có một người khiếm thị cần hỗ trợ tìm chỗ, lấy vé VIP.
</tim_nhan>
```

## 4.3. Báo giá vé bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
Bạn hãy tính tổng tiền cho yêu cầu báo giá dưới đây dựa trên bảng giá và quy tắc đđượccung cấp dưới đây.

Yêu cầu suy luận từng bước theo thứ tự tiền người lớn → tiền trẻ em → tiền vé → giảm giá nhóm → phụ thu → tổng tiền, mỗi bước ghi phép tính; dòng cuối có dạng [RESULT: <tổng tiền> đồng].

	GIÁ VÉ (đồng/người lớn)
	SVT – Sân Vận Động: 1.200.000
	FZ – Fan Zone: 800.000
	VIP – Khán đài VIP: 2.500.000

	QUY TẮC TÍNH GIÁ
	1. Trẻ em từ 5 đến 11 tuổi tính 70% giá người lớn. Trẻ dưới 5 tuổi miễn phí.
	2. Nhóm từ 5 người lớn trở lên được giảm 5% trên tiền vé (tiền người lớn + tiền trẻ em).
	3. Phụ thu khu ghế ưu tiên: 300.000 đồng/người. Phụ thu không được giảm giá.
	4. Tổng tiền = tiền vé sau giảm + phụ thu.

	YÊU CẦU BÁO GIÁ
	Nhóm cổ động viên đi xem trận bán kết, mua gói SVT, gồm 5 người lớn, 2 trẻ em (8 tuổi và 10 tuổi) và 1 bé 3 tuổi. Có 1 người lớn chọn khu ghế ưu tiên.
```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
	Bạn là trợ lý AI chuyên phân tích email khiếu nại của khán giả giải Cúp Sao Vàng. 
	Hãy đọc nội dung email khiếu nại dưới đây và trích xuất thông tin thành DUY NHẤT một đối tượng JSON hợp lệ, không kèm theo bất kỳ lời giải thích hay văn bản nào khác. 
	Cấu trúc JSON bắt buộc gồm đúng 4 trường sau: 
	{ "loai_su_co": "<chọn 1 trong 4 giá trị: 'tran_gio' | 'sai_khu_vuc' | 'nhan_vien' | 'khac'>", "so_khach": <số nguyên chỉ số lượng khách bị ảnh hưởng>, "so_hiep_bi_anh_huong": <số nguyên chỉ số hiệp bị ảnh hưởng, hoặc null nếu không áp dụng>, "tom_tat": "<tóm tắt nội dung khiếu nại, độ dài không quá 20 từ>" } Nội dung email: <email> [Dán nội dung email khiếu nại vào đây] </email>
```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

	 1.**Tính hợp lệ của cú pháp JSON (JSON Syntax):** Kiểm tra xem kết quả AI trả về có đúng chuẩn JSON thuần túy hay không (đầy đủ dấu ngoặc nhọn `{}`, dấu ngoặc kép `""`, không bị kẹp thêm văn bản giải thích thừa ở đầu/cuối làm hỏng cấu trúc khi đưa vào Trạm 2). 

	 2.**Độ chính xác và hợp lệ của 4 trường dữ liệu:** Kiểm tra xem có đủ đúng 4 khóa (`loai_su_co`, `so_khach`, `so_hiep_bi_anh_huong`, `tom_tat`) không; giá trị của `loai_su_co` có nằm đúng trong 4 nhãn quy định (`"sai_khu_vuc"`), kiểu dữ liệu số nguyên (`2`) có chuẩn xác và `tom_tat` có đảm bảo dưới 20 từ hay không.
	    

### c) Câu lệnh viết tiếp

**CAU LENH**

```
	Hãy viết tiếp ngay từ đúng chỗ bị ngắt: "...Ban tổ chức Cúp Sao Vàng xin hoàn lại Quý khán giả khoản". 
	Tuyệt đối không viết lại hay lặp lại bất kỳ từ nào của phần nội dung đã xuất hiện trước đó, chỉ hoàn thành nốt phần còn lại của email.
```
