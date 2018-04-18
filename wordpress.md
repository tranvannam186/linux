Báo cáo: TÌm hiểu và cài đặt Wordpress trên Centos 7.
=====================================================

## 1. Các nội dung chính
---------------------

-   Khái niệm Wordpress.
-   Cài đặt Wordpress. 
## 2.Khái niệm Wordpress.
-   Wordpress:

là một phần mềm nguồn mở (Open Source Software) được viết bằng ngôn ngữ
lập trình website PHP (Hypertext Preprocessor) và sử dụng hệ quản trị cơ
sở dữ liệu MySQL. là một mã nguồn mở bằng ngôn ngữ PHP để hỗ trợ tạo
blog cá nhân, và nó được rất nhiều người sử dụng ủng hộ về tính dễ sử
dụng, nhiều tính năng hữu ích. Qua thời gian, số lượng người sử dụng
tăng lên, các cộng tác viên là những lập trình viên cũng tham gia đông
đảo để phát triển mã nguồn WordPress có thêm những tính năng tuyệt vời.

WordPress đã được xem như là một hệ quản trị nội dung (CMS – Content
Management System) vượt trội để hỗ trợ người dùng tạo ra nhiều thể loại
website khác nhau như blog, website tin tức/tạp chí, giới thiệu doanh
nghiệp, bán hàng – thương mại điện tử, thậm chí với các loại website có
độ phức tạp cao như đặt phòng khách sạn, thuê xe, đăng dự án bất động
sản. 
## 2.2 Vì sao Wordpress được nhiều người lựa chọn sử dụng? 
-   Dễ sử dụng
WordPress được phát triển nhằm phục vụ đối tượng người dùng phổ thông, không có nhiều kiến thức về lập trình website nâng cao

-   Cộng đồng hỗ trợ đông đảo

    Wordpress là một mã nguồn CMS mở phổ biến nhất thế giới, điều này
    cũng có nghĩa là bạn sẽ được cộng đồng người sử dụng WordPress hỗ
    trợ bạn các khó khăn gặp phải trong quá trình sử dụng.

-   Nhiều gói giao diện có sẵn

    Wordpress có hệ thống Theme đồ sộ, nhiều theme chuyên nghiệp có khả
    năng SEO tốt. Giúp cho bạn dễ dàng chọn lựa, bố trí hình ảnh, màu
    sắc cho trang web của bạn một cách đẹp mắt ...
-   Nhiều widget hỗ trợ

    - Wordpress có 23 Widget như: Thống kê số truy nhập blog Các bài mới
    nhất, Các bài viết nổi bật nhất Các comment mới nhất Liệt kê các
    chuyên mục Liệt kê các Trang Danh sách các liên kết Liệt kê số bài
    viết trong từng tháng
-   Dễ phát triển

    Wordpress cung cấp cho bạn rất nhiều hàm viết sẵn, cho phép bạn tùy
    ý phát triển website của mình một cách chuyên nghiệp WordPress là
    một mã nguồn mở nên bạn có thể dễ dàng hiểu được cách hoạt động của
    nó và phát triển thêm các tính năng
-   Hỗ trợ nhiều ngôn ngữ

    Được phát triển bằng nhiều ngôn ngữ (hỗ trợ tiếng việt)
-   Tính tùy biến trang website cao

    Wordpress cho phép bạn có thể điều chỉnh trang website theo ý muốn
    của bạn, không nhất thiết đó phải là một trang blog, mà nó còn có
    thể trở thành một trang web về bán hàng, một trang web đại diện cho
    công ty bằng việc kết hợp các theme được hỗ trợ cùng các widget có
    sẵn 
## 3.Điều kiện cài đặt wordpress trên Centos7: 
nếu muốn cài wordpress trên server thì phải cài đặt sẳn : apache(httpd),php,mysql sau đây là cách cài đặt wordpress:
### bước 1: cài đặt apache: 

>*yum installd update*

>*yum installd httpd* 
### bước 2: Cài đặt MySQL MySQL là một hệ quản
trị cơ sở dữ liệu mạnh mẽ được nhiều người sử dụng Để cài đặt mysql, bạn
hãy mở terminal và gõ câu lệnh:

>*yum install mysql-server libapache2-mod-auth-mysql php5-mysql*

Trong quá trình cài đặt MySQL, bạn sẽ nhận được yêu cầu thiết lập mật
khẩu cho tài khoản root. Mật khẩu này sẽ được sử dụng để bạn có thể quản
lý và truy xuất các dữ liệu sau này.

### Bước 3: Cài đặt PHP

PHP là một ngôn ngữ kịch bản mã nguồn mở được sử dụng rộng rãi để tạo
lên những trang web động Để cài đặt php, bạn hãy mở terminal và gõ câu
lệnh:

>*yum install php5 libapache2-mod-php5 php5-mcrypt* 
### Bước 4:tạo cơ sở dữ liệu:
Bước đầu tiên sẽ là sự chuẩn bị. WordPress sử dụng một cơ sở
dữ liệu quan hệ để quản lý thông tin trang web và người sử dụng. Chúng
ta đã có MariaDB đã được cài đặt, bạn cần tạo một cơ sở dữ liệu và một
tài khoản cho wordpress.

Đăng nhập vào tài khoản quản trị MySQL là root sử dụng lệnh.

>*[root@vinadata]\# mysql -u root -p*

Bạn sẽ được yêu cầu nhập vào mật khẩu, mật khẩu này được thiết lập khi
bạn cài đặt MySQL, khi mật khẩu được chấp nhận bạn sẽ chuyển đến màn
hình lệnh của MySQL.

>*[root@vinadata]\# mysql -u root -p*

Enter password:

Tạo một cơ sở dữ liệu mới để WordPress có thể điều chỉnh. Bạn có thể đặt
tên bất kì cho cơ sở dữ liệu này, ở đây được đặt là wordpress.

>*MariaDB [(none)]\> CREATE DATABASE wordpress;*

>*Query OK, 1 row affected (0.01 sec)*

>*MariaDB [(none)]\>*

Tiếp theo bạn tạo một tài khoản dùng để quản trị cơ sở dữ liệu WordPress
mới.

Chúng tôi đặt tên cho tài khoảng này là wordpressuser và cấp cho tài
khoản một mật khẩu là password, bạn có thể tùy chỉnh khác phức tạp hơn
để đảm bảo bảo mật hơn.

>*MariaDB [(none)]\> CREATE USER wordpressuser@localhost IDENTIFIED BY
‘password’;*

>*Query OK, 1 row affected (0.01 sec)*

Bạn đã tạo xong một cơ sở dữ liệu, một tài khoản. Tuy nhiên tài khoản
này chưa thể truy cập vào cơ sở dữ liệu bạn mới tạo. Chúng ta cần gán
quyền cho tài khoản mới tạo để có thể truy cập cơ sở dữ liệu này.

>*MariaDB [(none)]\> GRANT ALL PRIVILEGES ON wordpress.* TO
>wordpressuser@localhost IDENTIFIED BY ‘password’;

>*Query OK, 0 rows affected (0.00 sec)*

>*MariaDB [(none)]*

Bây giờ tài khoản đã có thể truy cập cơ sở dữ liệu, chung ta cần tải lại
quyền để MySQL có thể cập nhật quyền đã được thay đổi.
>* MariaDB[(none)]\> FLUSH PRIVILEGES;*

>*Query OK, 0 rows affected (0.00 sec)*

>*MariaDB [(none)]\> exit*
## 3.2 cài đặt wordpress: Trước khi tải về
WordPress, chúng ta cần cài đặt một **module PHP** và có thể làm
việc. Thiếu module này, WordPress sẽ không thay đổi được kích thước ảnh.
Chúng ta cài đặt gói này trực tiếp từ lệnh yum của CentOS .

>*[root@vinadata]\# yum install php-gd*

Khởi động lại Apache2 để kích hoạt module mới cài.

>*[root@vinadata]\# service httpd restart*

Tất cả đã sẵn sàng để tải về và cài đặt WordPress từ website. Rất hay là
WordPress luôn giữ nguyên liên kết cho gói mới nhất của mình cùng một
URL, lấy gói mới nhất của wordpress về :

>*[root@vinadata]\# cd \~*

>*[root@vinadata \~]\# wget http://wordpress.org/latest.tar.gz*

Giải nén gói vừa tải về

>*[root@vinadata \~]\# tar xzvf latest.tar.gz*

Một thư mục wordpress mới được tạo ra, tiếp theo là chuyển thư mục vừa
giải nén về thư mục gốc của Apache, để có thể phục vụ người dùng truy
cập từ bên ngoài. Sử dụng rsync giúp giữ nguyên quyền khi di chuyển.
>*[root@vinadata \~]\# rsync -avP \~/wordpress/ /var/www/html/*

Chúng ta cần tạo thêm một thư mục để lưu trữ những tài liệu được tải lên
trang wordpress. Sử dụng lệnh mkdir.

>*[root@vinadata \~]\# mkdir /var/www/html/wp-content/uploads*

Cung cấp lại đúng quyền và sở hữu trên tài liệu và thư mục wordpress để
tăng việc bảo mật. Sử dụng chown để gán quyền sở hữu .

>*[root@vinadata \~]\# chown -R apache:apache /var/www/html/\**

Với thay đổi này, máy chủ web có thể tạo và thay đổi tập tin WordPress,
và có cung có thể tải lên nội dung. 
## 3.3 cấu hình wordpress: 
Hầu hết những cấu hình yêu cầu để sử dụng WordPress được thực hiện qua giao diện
web. Tuy nhiên, chung ta cần thực hiện một số thao tác từ dòng lệnh để
chắc rằng WordPress có thể kết nối tới cơ sở dữ liệu MySQL chúng ta đã
tạo trước đó.

Chuyển đến thư mục gốc Apache nơi đã cài WordPress:
>*[root@vinadata~]\# cd /var/www/html*

Tệp cấu hình chính để WordPress sử dụng là wp-config.php. Có một tệp cấu
hình mẫu gần như khớp với cấu hình chúng ta cần được đi kèm mặc định.
Chúng ta chỉ cần sao chép ta một tệp mới và WordPress có thể dựa vào đó
để sử dụng.

>*[root@vinadata html]\# cp wp-config-sample.php wp-config.php*

Bây giờ chúng ta làm việc với tệp cấu hình này. Sử dụng một chương trình
biên tập

>*[root@vinadata html]\# vi wp-config.php*

Chúng ta cần thay đổi ở đây là thông số liên quan đến thông tin cơ sở dữ
liệu. Tìm đến tiêu đề thiết đặt MySQL và thay đổi biến
DB_NAME,DB_USER, và DB_PASSWORD để WordPress kết nối đúng và chứng
thực đúng đến cơ sở dữ liệu chúng ta đã tạo ra.

Điền vào giá trị của những thông số này với thông tin của cơ sở dữ liệu
bạn đã tạo, tương tự như:

>*MySQL settings – You can get this info from your web host*
>/** The name of the database for WordPress \*/ define(‘DB_NAME’,
‘wordpress‘);

>/** MySQL database username \*/ define(‘DB_USER’, ‘wordpressuser‘);

>/** MySQL database password */ define(‘DB\_PASSWORD’, ‘password‘);*

Đó là một vài thông tin chúng ta cần thay đổi, lưu lại và đóng tệp khi
hoàn thành.

# 4 – Hoàn thành cài đặt thông qua giao diện Web
----------------------------------------------

Tất cả đã được cấu hình xong, hoàn thành cấu hình WordPress thông qua
giao diện web . Trong trình duyệt, nhập vào tên miền máy chủ hoặc địa
chỉ IP.

>*http://server\_domain\_name\_or\_IP*
