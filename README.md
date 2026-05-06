# Phân tích dữ liệu nhiệt độ đất liền toàn cầu sử dụng dữ liệu nhiệt độ của Berkeley Earth
## Tiền xử lý dữ liệu
### Đọc dữ liệu

<div align='center'>
  <img width="606" height="165" alt="image" src="https://github.com/user-attachments/assets/1fbf71ad-e51c-4bbd-905f-c9d9a3a3a3d3" , align='center'/>
</div>  
<p align='center'><i>Kết quả đọc thông tin dữ liệu</i></p>

Bộ dữ liệu nhiệt độ toàn cầu được sử dụng trong bài phân tích bao gồm 9 biến quan sát. Trong đó, biến thời gian “dt” hiện được lưu trữ dưới dạng chuỗi ký tự (character) và 8 biến còn lại có kiểu dữ liệu số (numeric). Do đặc thù của bài toán phân tích chuỗi thời gian, biến “dt” cần được chuyển đổi sang định dạng ngày tháng (date) chuẩn hóa để thiết lập trục thời gian. Bên cạnh đó, tập dữ liệu ghi nhận tỷ lệ thiếu dữ liệu tương đối cao ở một số biến, do vậy, việc áp dụng các kỹ thuật tiền xử lý phù hợp là cần thiết trước khi tiến hành các bước phân tích.
###Tiền xử lý dữ liệu
<div>
  <img width="609" height="112" alt="image" src="https://github.com/user-attachments/assets/3aecfa7a-133a-4881-abc9-1d636adeabc2" />
</div>
<p align='center'><i>Kết quả xử lý kiểu dữ liệu ngày tháng năm</i></p>

Nhằm khắc phục vấn đề sai lệch kiểu dữ liệu đã nêu ở phần trước, thuộc tính “dt” sẽ được ép kiểu từ chuỗi ký tự sang định dạng ngày tháng. Quá trình này bao gồm việc chuẩn hóa dữ liệu về định dạng thống nhất là “Năm-Tháng-Ngày” (YYYY-MM-DD), qua đó tạo cơ sở để trích xuất các đặc trưng thời gian trong các bước phân tích tiếp theo. 

Tập dữ liệu Berkeley Earth cung cấp các bản ghi lịch sử trải dài từ 1750 đến 2013. Dù vậy, những hạn chế về công nghệ đo lường trong giai đoạn đầu đã dẫn đến sự thiếu hụt đáng kể về mặt thông tin. Để khắc phục vấn đề này, sẽ tiến hành trích xuất (subset) và chỉ giữ lại chuỗi thời gian từ năm 1950 trở về sau. Bước tiền xử lý này không chỉ đảm bảo tính liên tục của dữ liệu mà còn hạn chế tối đa nhiễu do dữ liệu rỗng gây ra, qua đó nâng cao độ tin cậy của kết quả.
