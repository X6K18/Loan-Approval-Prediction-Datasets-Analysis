# Codebook: Loan Approval Prediction Dataset

## 1. Tổng quan (Overview)
Bộ dữ liệu này được sử dụng để dự đoán liệu một hồ sơ vay vốn của khách hàng có được phê duyệt hay không dựa trên các yếu tố về nhân khẩu học, thu nhập, điểm tín dụng và giá trị tài sản thế chấp.

- **Số lượng bản ghi:** 4269
- **Số lượng cột:** 13
- **Biến mục tiêu:** `loan_status`

---

## 2. Chi tiết các biến (Variables Dictionary)

| STT | Tên biến | Kiểu dữ liệu | Ý nghĩa | Ghi chú |
|:---:|:---|:---:|:---|:---|
| 0 | **loan_id** | Integer | Mã định danh duy nhất của khoản vay | Thường được loại bỏ khi huấn luyện mô hình. |
| 1 | **no_of_dependents** | Integer | Số lượng người phụ thuộc | Con cái, cha mẹ hoặc người thân sống dựa vào thu nhập người vay. |
| 2 | **education** | String/Cat | Trình độ học vấn | Giá trị: `Graduate` (Đã tốt nghiệp), `Not Graduate` (Chưa tốt nghiệp). |
| 3 | **self_employed** | String/Cat | Tình trạng tự doanh | `Yes` nếu làm tự do/kinh doanh riêng, `No` nếu làm thuê. |
| 4 | **income_annum** | Integer | Thu nhập hàng năm | Tổng thu nhập của người vay trong một năm. |
| 5 | **loan_amount** | Integer | Số tiền vay | Số tiền mà người vay đề nghị ngân hàng giải ngân. |
| 6 | **loan_term** | Integer | Kỳ hạn vay | Thời gian trả nợ (thường tính bằng năm). |
| 7 | **cibil_score** | Integer | Điểm tín dụng CIBIL | Chỉ số đo lường uy tín tín dụng (thường từ 300 - 900). |
| 8 | **residential_assets_value** | Integer | Giá trị bất động sản nhà ở | Giá trị căn nhà hoặc đất nền thuộc quyền sở hữu cá nhân. |
| 9 | **commercial_assets_value** | Integer | Giá trị bất động sản thương mại | Giá trị văn phòng, kho bãi hoặc mặt bằng kinh doanh. |
| 10 | **luxury_assets_value** | Integer | Giá trị tài sản xa xỉ | Bao gồm xe hơi sang trọng, trang sức, đồ cổ, v.v. |
| 11 | **bank_asset_value** | Integer | Giá trị tài sản ngân hàng | Tiền gửi tiết kiệm, cổ phiếu hoặc các khoản đầu tư thanh khoản cao. |
| 12 | **loan_status** | String/Cat | Trạng thái phê duyệt (Target) | `Approved` (Phê duyệt) hoặc `Rejected` (Từ chối). |

---

## 3. Phân tích vai trò của các nhóm biến

### A. Nhóm Điểm Tín Dụng (Quan trọng nhất)
- **cibil_score**: Đây là biến có trọng số cao nhất. Trong thực tế, các ngân hàng thường có "điểm cắt" (cutoff score). Ví dụ: Nếu điểm CIBIL dưới 600, khả năng bị từ chối là rất cao bất kể tài sản.


[Image of credit score ranges and their meanings]


### B. Nhóm Năng lực Tài chính & Tài sản thế chấp
- **income_annum** vs **loan_amount**: Tỷ lệ nợ trên thu nhập (Debt-to-Income ratio) được tính từ hai biến này.
- **Tổng giá trị tài sản**: Tổng hợp từ `residential`, `commercial`, `luxury`, và `bank assets`. Đây là cơ sở để ngân hàng thanh lý thu hồi vốn nếu có rủi ro nợ xấu.

### C. Nhóm Rủi ro Nhân khẩu học
- **no_of_dependents**: Càng nhiều người phụ thuộc thì khả năng tài chính dự phòng cho các biến cố càng thấp.
- **loan_term**: Kỳ hạn vay càng dài, rủi ro biến động kinh tế càng lớn, nhưng áp lực trả nợ hàng tháng lại thấp hơn.

---

## 4. Các bước xử lý dữ liệu gợi ý (Preprocessing)
1. **Loại bỏ khoảng trắng:** Kiểm tra xem các giá trị String có khoảng trắng thừa không (ví dụ: " Approved" thay vì "Approved").
2. **Mã hóa (Encoding):** - Chuyển `education` (Graduate=1, Not Graduate=0).
   - Chuyển `self_employed` (Yes=1, No=0).
   - Chuyển `loan_status` (Approved=1, Rejected=0).
3. **Tính toán biến mới (Feature Engineering):** - `total_assets_value` = Tổng tất cả các cột tài sản.
   - `loan_to_income_ratio` = `loan_amount` / `income_annum`.