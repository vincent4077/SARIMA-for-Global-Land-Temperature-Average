# Phân tích dữ liệu nhiệt độ đất liền toàn cầu
## Tiền xử lý dữ liệu
### Đọc dữ liệu

<div align='center'>
  <img width="606" height="165" alt="image" src="https://github.com/user-attachments/assets/1fbf71ad-e51c-4bbd-905f-c9d9a3a3a3d3" , align='center'/>
</div>  

<p>
  Bộ dữ liệu nhiệt độ toàn cầu được sử dụng trong bài phân tích bao gồm 9 biến quan sát. Trong đó, biến thời gian “dt” hiện được lưu trữ dưới dạng chuỗi ký tự (character) và 8 biến còn lại có kiểu dữ liệu số (numeric). Do đặc thù của bài toán phân tích chuỗi thời gian, biến “dt” cần được chuyển đổi sang định dạng ngày tháng (date) chuẩn hóa để thiết lập trục thời gian. Bên cạnh đó, tập dữ liệu ghi nhận tỷ lệ thiếu dữ liệu tương đối cao ở một số biến, do vậy, việc áp dụng các kỹ thuật tiền xử lý phù hợp là cần thiết trước khi tiến hành các bước phân tích.
</p>
  
