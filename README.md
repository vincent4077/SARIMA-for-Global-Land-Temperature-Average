# 🌡️ Phân tích & Dự báo Nhiệt độ Toàn cầu (Berkeley Earth) với SARIMA

Dự án thực hiện phân tích chuỗi thời gian (Time Series Analysis) trên bộ dữ liệu nhiệt độ đất liền toàn cầu từ Berkeley Earth, nhằm xây dựng mô hình dự báo biến động khí hậu chính xác. Chi tiết phân tích xem trong file `Detail_Analysis.docx`

---

## 🚀 Điểm nhấn Dự án
* **Xử lý dữ liệu:** Lọc dữ liệu giai đoạn 1950 - 2013 để loại bỏ nhiễu từ các thiết bị đo lường thô sơ thế kỷ 18-19, đảm bảo tính liên tục và tin cậy.
* **Phân rã chuỗi thời gian:** Xác định rõ xu hướng (Trend) nóng lên toàn cầu từ sau năm 1975 và tính mùa vụ (Seasonality) lặp lại ổn định.
* **Mô hình tối ưu:** Lựa chọn mô hình $SARIMA(2,1,1)(1,1,1)$ dựa trên tối ưu hóa chỉ số AIC/BIC.
* **Độ chính xác cao:** Sai số dự báo **MAPE < 4%** trên cả tập huấn luyện và tập kiểm chứng.

---

## 🛠️ Quy trình thực hiện

### 1. Tiền xử lý & Làm sạch
* **Đọc thông tin dữ liệu:**

<div align='center'>
  <img width="606" height="165" alt="image" src="https://github.com/user-attachments/assets/1fbf71ad-e51c-4bbd-905f-c9d9a3a3a3d3" , align='center'/>
</div>  
<p align='center'><i>Kết quả đọc thông tin dữ liệu</i></p>

* **Chuẩn hóa:** Ép kiểu biến `dt` sang định dạng `Date`, trích xuất cột `Land Average Temperature`.

<div align='center'>
  <img width="609" height="112" alt="image" src="https://github.com/user-attachments/assets/3aecfa7a-133a-4881-abc9-1d636adeabc2" />
</div>
<p align='center'><i>Kết quả xử lý kiểu dữ liệu ngày tháng năm</i></p>

* **Lọc nhiễu:** Tập trung vào dữ liệu từ năm 1950 để giảm thiểu các giá trị khuyết thiếu (missing values) và đảm bảo chất lượng quan trắc hiện đại.

<div align='center'>
  <img width="1048" height="482" alt="image" src="https://github.com/user-attachments/assets/27369b27-f41c-4161-9947-82d808803043" />
</div>
<p align='center'><i>Lọc lấy dữ liệu từ năm 1950 trở đi</i></p>

<div align='center'>
  <img width="603" height="475" alt="image" src="https://github.com/user-attachments/assets/ba38f644-b95f-423a-9c26-30b638dad810" />
</div>
<p align='center'><i>Dữ liệu sau khi đã lọc</i></p>

### 2. Phân tích đặc trưng (EDA)
* **Phân rã (Decomposition):** Tách biệt các thành phần Trend, Seasonal và Residual. Kết quả cho thấy xu hướng tăng nhiệt độ rõ rệt theo thời gian.

<div align='center'>
  <img width="603" height="375" alt="image" src="https://github.com/user-attachments/assets/f3cb959d-a169-4b7b-a0f7-1c7663e6ab4b" />
</div>
<p align='center'><i>Biểu đồ phân rã</i></p>

* **Kiểm định tính dừng:** Sử dụng biểu đồ ACF/PACF kết hợp kiểm định **Augmented Dickey-Fuller (ADF)** để xác định bậc sai phân phù hợp ($d=1, D=1$).

<div align='center'>
  <img width="605" height="364" alt="image" src="https://github.com/user-attachments/assets/92a0946a-0ff4-4d13-8d34-76256d4ddbb8" />
</div>
<p align='center'><i>Biểu đồ AFC và PACF</i></p>

<div align='center'>
  <img width="604" height="131" alt="image" src="https://github.com/user-attachments/assets/bcda46a9-dc85-4d13-a7ef-7fa69ce96d96" />
</div>
<p align='center'><i>Kiểm tra Dickey-Fuller</i></p>

### 3. Lựa chọn & Huấn luyện mô hình
Sau khi thử nghiệm nhiều tổ hợp tham số, mô hình **$SARIMA(2,1,1)(1,1,1)$** được lựa chọn nhờ:
* Chỉ số **AIC** và **BIC** phù hợp nhất trong các mô hình đề xuất.

<div align='center'>
  <img width="601" height="289" alt="image" src="https://github.com/user-attachments/assets/0242895e-3e91-4e98-93d4-63c733d66643" />
  <p><i>Kết quả so sánh AIC và BIC</i></p>
</div>

* Các hệ số tự hồi quy (AR) và trung bình trượt (MA) đều có ý nghĩa thống kê cao ($p-value < 0.05$).

### 4. Đánh giá phần dư (Diagnostics)
* **Ljung-Box Test:** P-value cao ở mọi độ trễ, chứng minh phần dư là nhiễu trắng (White Noise).

<div align='center'>
  <img width="599" height="424" alt="image" src="https://github.com/user-attachments/assets/05c4e687-c8b2-42c6-9d66-602831bee71e" />
  <p><i>ACF và Ljung-Box</i></p>
</div>

* **Phân phối chuẩn:** Biểu đồ Q-Q và Histogram cho thấy phần dư xấp xỉ phân phối chuẩn, khẳng định tính không chệch của mô hình.
<div align='center'>
  <img width="619" height="356" alt="image" src="https://github.com/user-attachments/assets/1f93d3d4-1ecd-41b6-acd6-f884d03fc221" />
  <p><i>Q-Q plot và Histogram</i></p>
</div>
---

## 📈 Kết quả dự báo

<div align='center'>
  <img width="552" height="296" alt="image" src="https://github.com/user-attachments/assets/34c3b72f-4ea5-4190-9a89-6a1b84f727e2" />
  <p><i>Biểu đồ dự báo</i></p>
</div>

Mô hình thể hiện khả năng tổng quát hóa cực tốt, không xảy ra hiện tượng quá khớp (Overfitting):

<div align='center'>
  <img width="601" height="210" alt="image" src="https://github.com/user-attachments/assets/6b805071-e42e-4c26-bf07-b162e11e58cf" />
  <p><i>Kiểm tra dự báo so với dữ liệu gốc</i></p>
</div>

* **Chỉ số Theil’s U:** 0.199 ($< 1$), cho thấy mô hình SARIMA vượt trội hoàn toàn so với các phương pháp dự báo đơn giản.

> **Kết luận:** Mô hình cung cấp một công cụ đáng tin cậy để hiểu về quy luật biến đổi nhiệt độ lịch sử và dự báo xu hướng khí hậu trong tương lai ngắn và trung hạn.

---
*Dự án được thực hiện bằng R Markdown.*
