# Hướng dẫn cài đặt WordPress trên CentOS 7
=======================================

## Giới thiệu
_________________________________
WordPress là một web mã nguồn mở miễn phí và công cụ viết blog sử dụng PHP và MySQL. WordPress hiện là CMS phổ biến nhất (Content Management System) trên Internet, và có hơn 20.000 plugins để mở rộng chức năng của nó. Điều đó khiến cho WordPress là một lựa chọn tuyệt vời để có được một trang web sử dụng một cách nhanh chóng và dễ dàng.

Trong hướng dẫn này, chúng tôi sẽ hướng dẫn để có được một trang WordPress thiết lập với máy chủ web Apache trên CentOS 7.

### Yêu cầu đầu tiên
Trước khi thực hiện tiếp những hướng dẫn bên dưới, bạn cần hoàn thành 1 số bước trước.

Chúng ta cần phải có 1 cụm LAMP (Linux, Apache, MySQL, and PHP) được cài đặt trên máy chủ CentOS 7. Nếu bạn chưa cài đặt những thành phần này, bạn có thể tham khảo bài Hướng dẫn cài đặt Linux, Apache, MySQL, PHP (LAMP) kết hợp trên CentOS 7.

##### Bước một – Tạo cơ sở dữ liệu MySQL và User cho WordPress
Bước đầu tiên sẽ là sự chuẩn bị. WordPress sử dụng một cơ sở dữ liệu quan hệ để quản lý thông tin trang web và người sử dụng. Chúng ta đã có MariaDB đã được cài đặt, bạn cần tạo một cơ sở dữ liệu và một tài khoản cho wordpress.

Đăng nhập vào tài khoản quản trị MySQL là root sử dụng lệnh.

>*[root@vinadata]# mysql -u root -p*

Bạn sẽ được yêu cầu nhập vào mật khẩu, mật khẩu này được thiết lập khi bạn cài đặt MySQL, khi mật khẩu được chấp nhận bạn sẽ chuyển đến màn hình lệnh của MySQL.

>*[root@vinadata]# mysql -u root -p*

>*Enter password:*

Tạo một cơ sở dữ liệu mới để WordPress có thể điều chỉnh. Bạn có thể đặt tên bất kì cho cơ sở dữ liệu này, ở đây được đặt là wordpress.

>*MariaDB [(none)]> CREATE DATABASE wordpress;*

>*Query OK, 1 row affected (0.01 sec)*

>*MariaDB [(none)]>*

Tiếp theo bạn tạo một tài khoản dùng để quản trị cơ sở dữ liệu WordPress mới.

Chúng tôi đặt tên cho tài khoảng này là wordpressuser và cấp cho tài khoản một mật khẩu là password, bạn có thể tùy chỉnh khác phức tạp hơn để đảm bảo bảo mật hơn.

>*MariaDB [(none)]> CREATE USER wordpressuser@localhost IDENTIFIED BY ‘password’;*

>*Query OK, 1 row affected (0.01 sec)*

Bạn đã tạo xong một cơ sở dữ liệu, một tài khoản. Tuy nhiên tài khoản này chưa thể truy cập vào cơ sở dữ liệu bạn mới tạo. Chúng ta cần gán quyền cho tài khoản mới tạo để có thể truy cập cơ sở dữ liệu này.

>*MariaDB [(none)]> GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost IDENTIFIED BY ‘password’;*

>*Query OK, 0 rows affected (0.00 sec)*

>*MariaDB [(none)]>*

Bây giờ tài khoản đã có thể truy cập cơ sở dữ liệu, chung ta cần tải lại quyền để MySQL có thể cập nhật quyền đã được thay đổi.

>*MariaDB [(none)]> FLUSH PRIVILEGES;*

>*Query OK, 0 rows affected (0.00 sec)*

>*MariaDB [(none)]> exit*

 

### Bước hai – Cài đặt WordPress
Trước khi tải về WordPress, chúng ta cần cài đặt một module PHP và có thể làm việc. Thiếu module này, WordPress sẽ không thay đổi được kích thước ảnh. Chúng ta cài đặt gói này trực tiếp từ lệnh yum của CentOS .

>*[root@vinadata]# yum install php-gd*

Khởi động lại Apache2 để kích hoạt module mới cài.

>*[root@vinadata]# service httpd restart*

Tất cả đã sẵn sàng để tải về và cài đặt WordPress từ website. Rất hay là WordPress luôn giữ nguyên liên kết cho gói mới nhất của mình cùng một URL, lấy gói mới nhất của wordpress về :

>*[root@vinadata]# cd ~*

>*[root@vinadata ~]# wget http://wordpress.org/latest.tar.gz*

Giải nén gói vừa tải về

>*[root@vinadata ~]# tar xzvf latest.tar.gz*

Một thư mục wordpress mới được tạo ra, tiếp theo là chuyển thư mục vừa giải nén về thư mục gốc của Apache, để có thể phục vụ người dùng truy cập từ bên ngoài. Sử dụng rsync giúp giữ nguyên quyền khi di chuyển.

>*[root@vinadata ~]# rsync -avP ~/wordpress/ /var/www/html/>*

Chúng ta cần tạo thêm một thư mục để lưu trữ những tài liệu được tải lên trang wordpress. Sử dụng lệnh mkdir.

>*[root@vinadata ~]# mkdir /var/www/html/wp-content/uploads*

Cung cấp lại đúng quyền và sở hữu trên tài liệu và thư mục wordpress để tăng việc bảo mật. Sử dụng chown để gán quyền sở hữu .

>*[root@vinadata ~]# chown -R apache:apache /var/www/html/**

Với thay đổi này, máy chủ web có thể tạo và thay đổi tập tin WordPress, và có cung có thể tải lên nội dung.

### Bước ba – Cấu hình WordPress
Hầu hết những cấu hình yêu cầu để sử dụng WordPress được thực hiện qua giao diện web. Tuy nhiên, chung ta cần thực hiện một số thao tác từ dòng lệnh để chắc rằng WordPress có thể kết nối tới cơ sở dữ liệu MySQL chúng ta đã tạo trước đó.

Chuyển đến thư mục gốc Apache nơi đã cài WordPress:

>*[root@vinadata ~]# cd /var/www/html*

Tệp cấu hình chính để WordPress sử dụng là wp-config.php. Có một tệp cấu hình mẫu gần như khớp với cấu hình chúng ta cần được đi kèm mặc định. Chúng ta chỉ cần sao chép ta một tệp mới và WordPress có thể dựa vào đó để sử dụng.

>*[root@vinadata html]# cp wp-config-sample.php wp-config.php*

Bây giờ chúng ta làm việc với tệp cấu hình này. Sử dụng một chương trình biên tập

>*[root@vinadata html]# vi wp-config.php*

Chúng ta cần thay đổi ở đây là thông số liên quan đến thông tin cơ sở dữ liệu. Tìm đến tiêu đề thiết đặt MySQL và thay đổi biến DB_NAME,DB_USER, và DB_PASSWORD để WordPress kết nối đúng và chứng thực đúng đến cơ sở dữ liệu chúng ta đã tạo ra.

Điền vào giá trị của những thông số này với thông tin của cơ sở dữ liệu bạn đã tạo, tương tự như:

>*// ** MySQL settings – You can get this info from your web host ** //*
>*/** The name of the database for WordPress */*
>*define(‘DB_NAME’, ‘wordpress‘);*

>*/** MySQL database username */*
>*define(‘DB_USER’, ‘wordpressuser‘);*

>*/** MySQL database password */*
>*define(‘DB_PASSWORD’, ‘password‘);*

Đó là một vài thông tin chúng ta cần thay đổi, lưu lại và đóng tệp khi hoàn thành.

### Bước bốn – Hoàn thành cài đặt thông qua giao diện Web
Tất cả đã được cấu hình xong, hoàn thành cấu hình WordPress thông qua giao diện web . Trong trình duyệt, nhập vào tên miền máy chủ hoặc địa chỉ IP.

>*http://server_domain_name_or_IP*



