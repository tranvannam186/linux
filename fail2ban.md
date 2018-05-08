# Tìm hiểu về FAIL2BAN
## cơ chế hoạt động của fail2ban
Chương trình Fail2ban sẽ chạy nền dịch vụ và quét file log được chỉ định bởi người dùng hoặc sử dụng mẫu cấu hình mặc định. Sau đó Fail2ban sẽ sử dụng các khuôn mẫu mà Fail2ban quy định để lọc và trích xuất các thông tin cần thiết. Từ những thông tin đó mà Fail2ban sẽ quyết định khoá truy cập IP đang thực hiện hoạt động đáng ngờ, như đăng nhập SSH sai mật khẩu quá nhiều lần,…

Về cơ bản thì Fail2ban sẽ tạo ra bộ rule tường lửa iptables để chặn truy cập từ địa chỉ IP đã xác định trong 1 khoảng thời gian nhất định. Như vậy Fail2ban có khả năng giảm thiểu rất nhiều số hoạt động đăng nhập không chính xác vào dịch vụ hệ thống.

Lưu ý:

Fail2ban không khuyến khích hoạt động với các chương trình tường lửa khác như CSF, vì sẽ xung đột hoạt động điều khiển tường lửa ‘iptables’.
Fail2ban chỉ là dịch vụ chạy nền và tương tác với các rule tường lửa ‘iptables’ qua các cấu hình được chỉ định bởi người dùng.
Fail2ban không thể giải quyết vấn đề về mật khẩu yếu, không đủ mạnh hay phức tạp để thoát khỏi cuộc tấn công Brute Force.
Trang chủ Fail2ban : http://www.fail2ban.org/wiki/index.php/Main_Page
Xem thêm bài viết bảo mật dịch vụ SSH tại đây:

Top 13 cách bảo mật dịch vụ SSH trên Linux.
## 1. cài đặt chương trình fail2ban
Chương trình Fail2ban không có sẵn trên Repo gốc của CentOS, mà bạn có thể cài đặt chương trình qua Repo EPEL . Vậy chúng ta sẽ cài đặt chương trình Repository EPEL trên CentOS 7 trước
> yum install -y epel-release

Sau đó tiến hành cài đặt gói ‘Fail2ban’ bằng ‘yum’.
>yum install -y fail2ban
## 2. tiến hành cấu hình 
Bạn đã cài đặt Fail2ban xong, hãy mở file cấu hình các giá trị toàn cục (global config) của Fail2ban để xem các thông số mặc định như sau :
> vi /etc/fail2ban/jail.cofn
##### chú ý:   là các thông số trong file jail.confi
[DEFAULT]
ignoreip = 127.0.0.1
bantime = 600
findtime = 600
 maxretry = 3
 ##### giải thích:
  * ignoreip: danh sách các địa chỉ IP mà chúng ta không muốn bị khoá truy cập, có thể coi đây là 1 whitelist địa chỉ IP.
 * bantime: khoảng thời gian (giây) mà Fail2ban sẽ khoá truy cập từ địa chỉ (block) IP.
 * findtime: khoảng thời gian (giây) một IP phải login thành công.
 * maxretry: số lần login thất bại tối đa
 Nhìn chung thì cấu hình toàn cục của Fail2Ban ổn rồi, chúng ta không cần thiết phải cập nhật lại nội dung file này. Giờ điều chúng ta cần làm đó là tạo file cấu hình với nội dung chỉ định cho việc bảo vệ dịch vụ SSH trên VPS/Cloud Server với nội dung như dưới.
 ##### a. tạo file đè:
 > vi /etc/fail2ban/jail.local
 
 sau đó thêm vào file file.conf:
 [ssh-iptables]

enabled  = true
filter   = sshd
action   = iptables[name=SSH, port=22, protocol=tcp]
           sendmail-whois[name=SSH, dest=root, sender=tranvannam186@gmail.com]
logpath  = /var/log/secure
maxretry = 5
bantime  = 3600
##### b. giải thích
* enabled : kích hoạt chức năng cho phép Fail2ban bảo vệ dịch vụ được cấu hình trong * section [ssh-iptables]. Bạn có thể tắt phần cấu hình 1 section bằng giá trị “false”.
 * filter : bạn có thể tìm thấy tên của các mẫu file quy chuẩn hoạt động filter riêng của Fail2ban được cấu hình sẵn trong thư mục ‘/etc/fail2ban/filter.d/*‘. Ta sử dụng tên ‘ssh’, vì nó là alias name tương ứng file ‘/etc/fail2ban/filter.d/sshd.conf‘, nếu file này không tồn tại thì hoạt động bảo vệ port SSH sẽ không hoạt động.
 * action :
-+ fail2ban sẽ ban địa chỉ IP nếu thông tin lọc khớp với khuôn mẫu trong file ‘/etc/fail2ban/filter.d/sshd.conf‘, các hành vi ban địa chỉ IP được quy định tại ‘/etc/fail2ban/action.d/iptables.conf‘ .
-+ Nếu bạn sử dụng một port khác cho dịch vụ SSH lắng nghe trên server thì bạn có thể phải chỉ định lại thông tin ‘port=<port_number>’ trong phần cấu hình.
-+ Nếu bạn có địa chỉ mail, thì Fail2ban sẽ gửi email đến cho bạn khi địa chỉ IP bị ban. Bạn cũng nên lưu ý, việc gởi mail thành công hay không phụ thuộc nhiều vào dịch vụ MTA mặc định của hệ thống.
* logpath : đường dẫn file log Fail2ban sử dụng để lọc liên tục các thông báo đăng nhập sai.
* maxretry : số lần login thất bại tối đa cho phép.
* bantime : thời gian khoá truy cập từ IP đăng nhập sai quá số lần ‘maxretry’. 3600 giây = 1 giờ. Bạn có thể thay đổi giá trị theo mong muốn. 
### 3.Khởi động dịch vụ Fail2ban
> systemctl start fail2ban

> systemctl enable fail2ban
### 4. quản lí file:
kiểm tra xem có ip nào vi phạm vài bị BAN hay không :
#fail2ban-client status ssh-iptables

Status for the jail: ssh-iptables
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     3
|  `- File list:        /var/log/secure
`- Actions
   |- Currently banned: 1
   |- Total banned:     1
   `- Banned IP list:   123.231.112.221
  ###### ở đây thấy có 1 địa chỉ ip bị ban là :123.231.112.221.ta gỡ bỏ BAN bằng câu lệnh sau:
   > fail2ban-client set ssh-iptables unbanip 123.231.112.221





