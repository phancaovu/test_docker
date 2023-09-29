VII. Thực hành với một project cơ bản (C#, mysql)
1. Chuẩn bị : 
- Đảm bảo đã cài đặt .NET sdk phiên bản thấp nhất là 6.0
- Đã cài đặt Docker
- IDE dễ sử dụng ví dụ : visual studio , visual studio code...
2. Cài đặt: 
Tạo một folder có tên là test-docker
Tiếp theo là tạo 1 project wepapp .net bằng CLI : 
dotnet new webapp -n testdocker 
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/03452656-74a5-4465-985b-1be572e217da)

Tiếp theo mình sẽ run project mới tạo
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/37968f46-7983-418f-bb7b-3fc1766e4a9a)

Run project : dotnet run --urls http://localhost:6060
Project đã chạy thành công 
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/0adea087-4fcc-42a7-b6da-ac957d9b1a05)

Tiếp theo tạo một Dockerfile để chứa thông tin của image
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/daffb778-ba6a-4ee1-9b59-253bd5a33c38)

Chạy câu lệnh: docker build -t testdocker 
kiểm tra xem tạo image đã thành công hay chưa : docker images
oke image của mình đã hiển thị 
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/c8312d43-cf99-4dd4-b8b1-9177fefa002b)

Kiểm tra trong docker desktop
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/b7546778-a889-494e-8915-b0bec851d58a)

Tiếp theo sẽ build container từ image testdocker sau bằng câu lệnh: docker run testdocker ( tên image )
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/ea1dc2bc-b1da-4fad-b703-ed11bce657c7)


Sau khi chạy câu lệnh docker run bây giờ mở đường dẫn http://localhost:80 chúng ta sẽ gặp 1 lỗi đó là port 80 vì khi khai báo dockerfile ta sử dụng cổng 80 làm đầu ra. để fix lỗi này thì mình đơn giản chỉ cần đổi lại port cho container 
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/737cd09e-86ab-482b-a652-e7ce849b6127)

Lỗi error 500 khắc phục ?
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/21315b01-a735-4187-9c17-8f8694c6c663)

Sử dụng câu lệnh sau để đổi port cho container : 
docker run --publish 5000:80 testdocker
Thử load lại page
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/3ed99648-2105-4786-9c3a-868f360dde7d)


Bây giờ chúng ta sẽ thử trường hợp update code khi container đang chạy. đầu tiên sẽ kiểm tra tất cả các container và image đang có bằng CLI hoặc xem trên docker desktop
với CLI trên PowerShell:
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/280175b7-eae9-4fcc-9c7a-a9440b3e0116)

với docker desktop: 
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/08aaefcc-d602-497f-b838-7b18bfd4810e)

Mình sẽ update code trong file: src/page/layout.cshtml 
<h1 class="display-4">Welcome E-tech</h1>
Tiếp theo mình sẽ tạo một image cùng tên như vậy nhưng mình sẽ gắn thêm tag cho image đó mình sử dụng 
docker build -t testdocker:v1 .
Khi build trong image sẽ hiện thêm một image nữa cùng tên nhưng khác phần tag : v1
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/4bfda0cf-d769-4d49-b872-4657666d00cf)


Ở container thì vẫn sẽ chạy bình thường nhưng chưa được update code lại 
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/b341d618-8972-475c-895e-70cc3f19e52d)
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/1feecc18-cb35-4224-8d32-f0353ce58809)


 
Tiếp theo sẽ sử dụng câu lệnh sau để chạy container có chứa image mới tạo :
docker run --publish 5000:80 testdocker:v1  (dùng dấu :v1 để docker biết mình đang sử dụng ở tag nào)
Vào phần container để kiểm tra xem container đã chạy trước khi run lệnh trên thì phải dừng container testdocker  trước tránh trường hợp khi run lên bị trùng port gây lỗi mình dùng câu lệnh trước docker run trên
docker stop testdocker 
oke rồi mình sẽ vào container trong docker desktop để kiếm tra  khi đó Tag v1 sẽ được thay thế vision cũ và mình sẽ xóa luôn container cũ sử dụng câu lệnh
docker rm testdocker 
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/55c5e1c4-1f91-4eea-9496-afeb70b12297)

Truy cập vào http://localhost:5000/ xem trang web đã được thay đổi.
 ![image](https://github.com/phancaovu/test_docker/assets/66910370/8029834f-4c8a-491c-be5c-d2cc836d113d)

Ở phần 3 sẽ tiếp tục nhưng sẽ có thêm phần Backend để gọi dữ liệu và giới thiệu về Volumes

