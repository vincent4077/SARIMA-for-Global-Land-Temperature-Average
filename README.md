# Phân tích dữ liệu nhiệt độ đất liền toàn cầu sử dụng dữ liệu nhiệt độ của Berkeley Earth
## 1. Tiền xử lý dữ liệu
### a. Đọc dữ liệu

<div align='center'>
  <img width="606" height="165" alt="image" src="https://github.com/user-attachments/assets/1fbf71ad-e51c-4bbd-905f-c9d9a3a3a3d3" , align='center'/>
</div>  
<p align='center'><i>Kết quả đọc thông tin dữ liệu</i></p>

Bộ dữ liệu nhiệt độ toàn cầu được sử dụng trong bài phân tích bao gồm 9 biến quan sát. Trong đó, biến thời gian “dt” hiện được lưu trữ dưới dạng chuỗi ký tự (character) và 8 biến còn lại có kiểu dữ liệu số (numeric). Do đặc thù của bài toán phân tích chuỗi thời gian, biến “dt” cần được chuyển đổi sang định dạng ngày tháng (date) chuẩn hóa để thiết lập trục thời gian. Bên cạnh đó, tập dữ liệu ghi nhận tỷ lệ thiếu dữ liệu tương đối cao ở một số biến, do vậy, việc áp dụng các kỹ thuật tiền xử lý phù hợp là cần thiết trước khi tiến hành các bước phân tích.
### b. Làm sạch dữ liệu

<div align='center'>
  <img width="609" height="112" alt="image" src="https://github.com/user-attachments/assets/3aecfa7a-133a-4881-abc9-1d636adeabc2" />
</div>
<p align='center'><i>Kết quả xử lý kiểu dữ liệu ngày tháng năm</i></p>

Nhằm khắc phục vấn đề sai lệch kiểu dữ liệu đã nêu ở phần trước, thuộc tính “dt” sẽ được ép kiểu từ chuỗi ký tự sang định dạng ngày tháng. Quá trình này bao gồm việc chuẩn hóa dữ liệu về định dạng thống nhất là “Năm-Tháng-Ngày” (YYYY-MM-DD), qua đó tạo cơ sở để trích xuất các đặc trưng thời gian trong các bước phân tích tiếp theo. 

Tập dữ liệu Berkeley Earth cung cấp các bản ghi lịch sử trải dài từ 1750 đến 2013. Dù vậy, những hạn chế về công nghệ đo lường trong giai đoạn đầu đã dẫn đến sự thiếu hụt đáng kể về mặt thông tin. Để khắc phục vấn đề này, sẽ tiến hành trích xuất (subset) và chỉ giữ lại chuỗi thời gian từ năm 1950 trở về sau. Bước tiền xử lý này không chỉ đảm bảo tính liên tục của dữ liệu mà còn hạn chế tối đa nhiễu do dữ liệu rỗng gây ra, qua đó nâng cao độ tin cậy của kết quả.

<div align='center'>
  <img width="1048" height="482" alt="image" src="https://github.com/user-attachments/assets/27369b27-f41c-4161-9947-82d808803043" />
</div>
<p align='center'><i>Lọc lấy dữ liệu từ năm 1950 trở đi</i></p>

Việc thu hẹp chuỗi thời gian từ năm 1950 trở đi đã giải quyết hoàn toàn tình trạng khuyết thiếu thông tin. Do định hướng của bài phân tích là đánh giá sự biến động nhiệt độ, bước tiền xử lý cuối cùng sẽ tiến hành lọc chỉ duy nhất biến 'Land Average Temperature' để tiến thuận tiện cho việc phân tích, chọn mô hình và dự báo.

<div align='center'>
  <img width="603" height="475" alt="image" src="https://github.com/user-attachments/assets/ba38f644-b95f-423a-9c26-30b638dad810" />
</div>
<p align='center'><i>Dữ liệu sau khi đã lọc</i></p>

Để tránh việc phải lấy lại dữ liệu cho một số trường hợp về sau chúng tôi sẽ sử dụng một biến “temp_ts” và hàm “ts()” để lấy riêng cột Land Average Temperature cho việc phân tích dữ liệu và mô hình hóa. Việc này giúp đảm bảo tính toàn vẹn cho dữ liệu đã được xử lý sẽ được sử dụng cho các mục đích sau này.
## 2. Phân tích dữ liệu
### a. Quan sát dữ liệu sử dụng

<div align='center'>
  <img width="585" height="357" alt="image" src="https://github.com/user-attachments/assets/df414297-896d-496e-a5c2-dd5c9e362b10" />
</div>
<p align='center'><i>Biểu đồ dữ liệu nhiệt độ</i></p>

Quan sát đồ thị ban đầu cho thấy biến động nhiệt độ tồn tại yếu tố mùa vụ cùng với xu hướng tăng dần qua giai đoạn 1950 - 2013. Dù vậy, việc hiển thị toàn bộ chuỗi thời gian trên một trục đồ thị duy nhất tạo ra độ nhiễu (noise) cao do mật độ quan sát dày đặc, làm hạn chế khả năng đánh giá trực quan. Để khắc phục vấn đề này và đo lường chính xác hơn mức độ ảnh hưởng của từng yếu tố, bài phân tích sẽ tiến hành áp dụng kỹ thuật phân rã chuỗi thời gian. 

Thông qua biểu đồ phân rã, dữ liệu được chia ra thành 4 biểu đồ lần lượt là biểu đồ của dữ liệu, biểu đồ xu hướng, biểu đồ mùa vụ và phần cuối cùng là nhiễu trắng. Dựa trên biểu đồ có thể thấy thành phần xu hướng (trend) của biểu đồ khắc họa rõ rệt sự tăng trưởng của nhiệt độ từ năm 1950 đến năm 2013. Cụ thể từ năm 1950 đến 1975 không có hướng tăng trưởng rõ ràng nhưng sau giai đoạn này xu hướng tăng trưởng xuất hiện rõ ràng và tăng liên tục tới năm 2013. Quan sát thành phần mùa vụ cho thấy dữ liệu có tính chu kỳ lặp lại khá đều đặn qua từng năm. Ngoài ra dữ liệu  gốc cho thấy dữ liệu dao động đều nhau, không xuất hiện các dao động to nhỏ khác nhau.

<div align='center'>
  <img width="603" height="375" alt="image" src="https://github.com/user-attachments/assets/f3cb959d-a169-4b7b-a0f7-1c7663e6ab4b" />
</div>
<p align='center'><i>Biểu đồ phân rã</i></p>

Bên cạnh thành phần mùa vụ và xu hướng, nhiễu trắng (hay phần dư) trong biểu đồ  phần lớn dao động ổn định quanh mức 0 với biên độ nhỏ cho thấy yếu tố mùa vụ và xu hướng chi phối nền tảng của bộ dữ liệu. Mặc dù vậy trong nhiễu trắng vẫn xuất hiện một số điểm cực trị bất thường xuất hiện phản ánh các yếu tố khí hậu biến động ngẫu nhiên xuất hiện bất ngờ trong một khoảng thời gian ngắn.

Hình phía dưới là hai đồ thị ACF(bên trái) và PACF(bên phải) của dữ liệu khi chưa sử dụng các phép  biến đổi. Trong đồ thị ACF ta dễ dàng nhận thấy sự đi lên và đi xuống liên tục của biểu đồ cho thấy tính mùa vụ của chuỗi dữ liệu thời gian. Mặt khác với đồ thị PACF ta có thể thấy một hệ số tương quan rất lớn xuất hiện tại lag 1 và giảm chậm dần củng cố cho yếu tố xu hướng của dữ liệu.

<div align='center'>
  <img width="605" height="364" alt="image" src="https://github.com/user-attachments/assets/92a0946a-0ff4-4d13-8d34-76256d4ddbb8" />
</div>
<p align='center'><i>Biểu đồ AFC và PACF</i></p>

Bằng việc quan sát hai đồ thị ACF và PACF, chuỗi dữ liệu thời gian hiện tại có thể nói rằng dữ liệu chưa có tính dừng. Tuy nhiên để có một cái nhìn đa chiều chúng ta sẽ sử dụng Dickey Fuller để kiểm tra tính dừng của dữ liệu

<div align='center'>
  <img width="604" height="131" alt="image" src="https://github.com/user-attachments/assets/bcda46a9-dc85-4d13-a7ef-7fa69ce96d96" />
</div>
<p align='center'><i>Kiểm tra Dickey-Fuller</i></p>

Với kiểm tra Dickey Fuller, kết quả hoàn toàn ngược lại khi chúng ta quan sát bằng biểu đồ ACF và PACF. Điều này dẫn tới khả năng dữ liệu có tính dừng nhưng bị ảnh hưởng mạnh bởi yếu tố mùa vụ. Tuy nhiên cũng không loại trừ trường hợp kết quả bị trái ngược do xu hướng đi lên của dữ liệu chưa thực sự quá rõ ràng. Dù trong trường hợp nào, dữ liệu cũng bị ảnh hưởng mạnh mẽ bởi tính mùa vụ vì vậy mô hình SARIMA là phù hợp với bài toán. Đồng thời để đảm bảo tính chính xác trong việc xác định tham số mô hình, xem xét các trường hợp khác nhau là điều cần thiết trong việc biến đổi dữ liệu và xác định mô hình.

### b. Biến đổi dữ liệu và đề xuất mô hình
Dựa trên những gì đã phân tích sẽ đề xuất sử dụng hàm ```diff()``` của R để lấy sai phân cho hai trường hợp. Đầu tiên sẽ lấy sai phân mùa vụ ```diff(lag=12``` để loại bỏ yếu tố mùa vụ vì dữ liệu phân tích nặng tính mùa vụ. Với trường hợp đầu tiên sẽ chỉ sai phân một lần duy nhất để loại bỏ mùa vụ vì dựa trên kiểm tra Dickey-Fuller dữ liệu đã có tính dừng. Với trường hợp thứ hai sau khi đã loại bỏ mùa vụ sử dụng kết quả đó cùng hàm ```diff(lag=1)``` để loại bỏ xu hướng. Hai kết quả thu được sẽ được sử dụng để chọn tham số cho mô hình SARIMA.
<p align='center'>
  <img width="45%" height="449" alt="image" src="https://github.com/user-attachments/assets/8cd24f4b-3ddb-4ac8-b5d4-328a2d37f2bf" />
  <img width="45%" height="473" alt="image" src="https://github.com/user-attachments/assets/3f328a7f-b335-4b84-99cf-18d78645121c" /> 
</p>
Mô hình SARIMA sẽ có dạng:
<h3 align='center'><i>SARIMA(p,d,q)(P,D,Q)</i></h3>
Trong đó (p,d,q) là các yếu tố ngắn hạn, còn (P,D,Q) là các yếu tố mùa vụ. Dựa trên phân tích ta sẽ có 2 trường hợp d=0, D=1 cho trường hợp chỉ sai phân mùa vụ và d=1, D=1 cho trường hợp sai phân 2 lần. 
