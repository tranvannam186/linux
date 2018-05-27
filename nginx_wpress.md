# Hướng dẫn cài đặt WordPress với nginx trên CentOS 6
 
### Về Wordpress
Wordpress là một trang web miễn phí và mã nguồn mở và công cụ viết blog sử dụng php và MySQL. Nó được tạo ra vào năm 2003 và từ đó mở rộng để quản lý 22% tất cả các trang web mới được tạo và có hơn 20.000 plugin để tùy chỉnh các chức năng của nó

### Cài đặt
Các bước trong bài hướng dẫn này yêu cầi người dùng có quyền root trên máy chủ ảo của bạn.

Khi bạn đã có người dùng và các software cần thiết thì bạn có thể bắt đầu cài đặt WordPress!

#### Bước 1—Download WordPress
Bạn có thể download WordPress trực tiếp từ website của WordPress
>>> wget http://wordpress.org/latest.tar.gz

Command này sẽ download gói WordPress đã được nén thẳng về thư mục chính của người dùng. Bạn có thể giải nén nó với dòng tiếp theo đây:

> tar -xzvf latest.tar.gz

### Bước 2—Tạo cơ sở dữ liệu và người dùng WordPress 
Sau khi giải nén file WordPress thì cũng ta sẽ ở trong thư mục gọi là WordPress trong thư mục chính

Bây giờ chúng ta cần thay đổi gear và tạo thư mục MySQL mới với WordPress.

Đăng nhập vào shell MySQL:

> mysql -u root -p

Đăng nhập sử dụng password root MySQL và sau đó chúng ta cần tạo cơ sở dữ liệu WorsPress, người dùng trong cơ sở dữ liệu đó và tạo cho người dùng đó password mới. Nên nhớ rằng tất cả command MySQL phải kết thúc với dấu chấm phẩy.

Đầu tiên tạo cơ sở dữl liệu ( tôi sẽ gọi của tôi là wordpress for simplicity's sake; bạn có thể thoải mái chọn tên mà bạn thích):

>CREATE DATABASE wordpress;
>Query OK, 1 row affected (0.00 sec)

 Sau đó chúng ta cần tạo người dùng mới. Bạn có thể thay thế cơ sở dữ liệu, tên và password tuỳ theo bạn muốn:

>CREATE USER wordpressuser@localhost;
>Query OK, 0 rows affected (0.00 sec)

Đặt password cho người dùng mới:

>SET PASSWORD FOR wordpressuser@localhost= PASSWORD("password");
>Query OK, 0 rows affected (0.00 sec)

Kết thúc bằng việc trao tất cả các quyền cho người dùng mới. Nếu không có command này thì người cài đặt WordPress sẽ không thể bắt đầu:

>GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost IDENTIFIED BY >'password';
>Query OK, 0 rows affected (0.00 sec)

Sau đó refresh lại MySQL:

>FLUSH PRIVILEGES;
>Query OK, 0 rows affected (0.00 sec)

Thoát khỏi shell MySQL:

>exit

### Bước 3—Thiết lập cấu hình WordPress 
Bước 1 là copy file cấu hình WordPress mẫu được đặt trong thư mục WordPress, bên trong file mà chúng ta sẽ chỉnh sửa, tạo một cấu hình WordPress có thể sử dụng được:

>p ~/wordpress/wp-config-sample.php ~/wordpress/wp-config.php

Sau đó mở cấu hình WordPress:

> sudo nano ~/wordpress/wp-config.php

Tìm phần chứa cái bên dưới và thay đổi nó thành tên chính xác cho cơ sở dữ liệu, tên người dùng và password của bạ:

>// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');
/** MySQL database username */
define('DB_USER', 'wordpressuser');
/** MySQL database password */
define('DB_PASSWORD', 'password');

Lưu và thoát.

Bước 4—Copy  File
Chúng ta đã gần như xong việc upload WordPress lên server. Chúng ta cần tạo thư mục để giữ các file WordPress:

>sudo mkdir -p /var/www/wordpress

Bước cuối cùng là chuyển file WordPress đã giải nén đến thư mục gốc của website.

>sudo cp -r ~/wordpress/* /var/www/wordpress

Chúng ta có thể sửa đổi quyền của/var/www để cho phép tự động cập nhật tự động các plugin Wordpress và chỉnh sửa tệp bằng SFTP. Nếu các bước này không được thực hiện, bạn có thể nhận được thông báo lỗi "To perform the requested action, connection information is required" khi cố gắng thực hiện một trong hai nhiệm vụ.

Đầu tiên chuyển sang thư mục web:

cd /var/www/Give ownership of the directory to the nginx user, replacing the "username" with the name of your server user. Trao quyền sở hữu thư mục cho người dùng nginx, thay thế "username" với tên của người dùng server của bạn.
>sudo chown nginx:nginx * -R
sudo usermod -a -G nginx username
sudo chmod 777 wordpress

Bước 5—Thiết lập khối server Nginx
Bây giờ chúng ta cần thiết lập máy chủ ảo WordPress. Cho dù WordPress có thêm một bước khi cài đặt nó, chúng ta có thể dễ dàng lấy được file cấu hình trên website của WordPress:

Mở file mặc định Nginx:

>sudo vi /etc/nginx/conf.d/default.conf

Cấu hình nên bao gồm những thay đổi bên dưới( chi tiết của thay đổi bên dưới thông tin cấu hình):


>server {
listen 80;
server_name _;
#charset koi8-r;
#access_log logs/host.access.log main;
location / {
root /var/www/wordpress;
index index.php index.html index.htm;
}error_page 404 /404.html;
location = /404.html {
root /usr/share/nginx/html;
}# redirect server error pages to the static page /50x.html
#
>error_page 500 502 503 504 /50x.html;
location = /50x.html {
root /usr/share/nginx/html;
}# proxy the PHP scripts to Apache listening on 127.0.0.1:80

>#location ~ \.php$ {
#proxy_pass http://127.0.0.1;
#}# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000

>location ~ \.php$ {
root /var/www/wordpress;
fastcgi_pass 127.0.0.1:9000;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
include fastcgi_params;
}# deny access to .htaccess files, if Apache's document root
>#concurs with nginx's one

>#location ~ /\.ht {
#deny all;
#}}

Đây là chi tiết của những thay đổi- có lẽ một vài thay đổi đã có hiệu lực:
Thêm index.php index line. trong dòng index.
Thay đổi root thành /var/www/wordpress;
Bỏ ghi chứ phần bắt đầu với "location ~ \.php$ {",
Thay đổi root để truy cập root tài liệu thật  /var/www/wordpress;
Thay đổi dòng fastcgi_param để hỗ trợ tham số PHP tìm tập lệnh PHP mà ta lưu trong thư mục gốc.
Lưu, thoát và khởi động  lại nginx để làm cho các thay đổi này có hiệu lực:

>sudo service nginx restart

Bước 6—Kết quả: Truy cập cài đặt WordPress
Khi mọi thứ đã xong, trang cài đặt online WordPress sẽ đợi bạn
