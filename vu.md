VII. Thực hành với một project cơ bản (C#, mysql)
1. Chuẩn bị : 
- Đảm bảo đã cài đặt .NET sdk phiên bản thấp nhất là 6.0
- Đã cài đặt Docker
- IDE dễ sử dụng ví dụ : visual studio , visual studio code...
2. Cài đặt: 
Tạo một folder có tên là test-docker
Tiếp theo là tạo 1 project wepapp .net bằng CLI : 
dotnet new webapp -n testdocker 
![Alt text](image-1.png)
Tiếp theo mình sẽ run project mới tạo
 ![Alt text](image-2.png)
Run project : dotnet run --urls http://localhost:6060
Project đã chạy thành công 
 ![Alt text](image-3.png)
Tiếp theo tạo một Dockerfile để chứa thông tin của image
 ![Alt text](image-4.png)
Chạy câu lệnh: docker build -t testdocker 
kiểm tra xem tạo image đã thành công hay chưa : docker images
oke image của mình đã hiển thị 
 ![Alt text](image-5.png)
Kiểm tra trong docker desktop
 ![Alt text](image-6.png)
Tiếp theo sẽ build container từ image testdocker sau bằng câu lệnh: docker run testdocker ( tên image )
 ![Alt text](image-7.png)

Sau khi chạy câu lệnh docker run bây giờ mở đường dẫn http://localhost:80 chúng ta sẽ gặp 1 lỗi đó là port 80 vì khi khai báo dockerfile ta sử dụng cổng 80 làm đầu ra. để fix lỗi này thì mình đơn giản chỉ cần đổi lại port cho container 
 ![Alt text](image-8.png)
Lỗi error 500 khắc phục ?
 ![Alt text](image-9.png)
Sử dụng câu lệnh sau để đổi port cho container : 
docker run --publish 5000:80 testdocker
Thử load lại page
 ![Alt text](image-10.png)

Bây giờ chúng ta sẽ thử trường hợp update code khi container đang chạy. đầu tiên sẽ kiểm tra tất cả các container và image đang có bằng CLI hoặc xem trên docker desktop
với CLI trên PowerShell:
 ![Alt text](image-11.png)
với docker desktop: 
 ![Alt text](image-12.png)
Mình sẽ update code trong file: src/page/layout.cshtml 
<h1 class="display-4">Welcome E-tech</h1>
Tiếp theo mình sẽ tạo một image cùng tên như vậy nhưng mình sẽ gắn thêm tag cho image đó mình sử dụng 
docker build -t testdocker:v1 .
Khi build trong image sẽ hiện thêm một image nữa cùng tên nhưng khác phần tag : v1
 ![Alt text](image-13.png)

Ở container thì vẫn sẽ chạy bình thường nhưng chưa được update code lại 
 ![Alt text](image-14.png)
 ![Alt text](image-15.png)
 ![Alt text](image-16.png)
Tiếp theo sẽ sử dụng câu lệnh sau để chạy container có chứa image mới tạo :
docker run --publish 5000:80 testdocker:v1  (dùng dấu :v1 để docker biết mình đang sử dụng ở tag nào)
Vào phần container để kiểm tra xem container đã chạy trước khi run lệnh trên thì phải dừng container testdocker  trước tránh trường hợp khi run lên bị trùng port gây lỗi mình dùng câu lệnh trước docker run trên
docker stop testdocker 
oke rồi mình sẽ vào container trong docker desktop để kiếm tra  khi đó Tag v1 sẽ được thay thế vision cũ và mình sẽ xóa luôn container cũ sử dụng câu lệnh
docker rm testdocker 
 ![Alt text](image-17.png)
Truy cập vào http://localhost:5000/ xem trang web đã được thay đổi.
 ![Alt text](image-18.png)
Ở phần 3 sẽ tiếp tục nhưng sẽ có thêm phần Backend để gọi dữ liệu và giới thiệu về Volumes

