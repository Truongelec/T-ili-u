							
#   SSL 
### Khái niệm : 
SSL – Secure Socket Layer là giao thức bảo mật được sử dụng để mã hóa thông tin truyền giữ máy chủ web và trình duyệt.
Các cách chứng thực SSL: 
- Xác minh qua mail
- Xác minh qua File
- Xác minh qua DNS

### Các loại SSL phổ biến:

**DV - Domain Validated** - Xác thực tên Domain và Website được mã hóa an toàn khi trao đổi dữ liệu
**OV - Organization Validation** - Xác thực tổ chức / doanh nghiệp đang sở hữu website đó
**EV- Extend Validation** - SSL có độ bảo mật cao nhất
**Wildcard SSL** - Bảo mật tên miền chính và tất cả tên miền phụ của nó
ví dụ: example.com → mail.example.com → abc.example.com
**SANs SSL** - cho phép bảo vệ nhiều tên miền bằng một chứng chỉ
**SNI SSL** - cho phép nhiều website chia sẻ một địa chỉ IP- thường sử dụng chung với SANs SSL

### Các loại file trong SSL
**1. CSR**
- file chứa thông tin về yêu cầu cấp chứng chỉ SSL gồm thông tin như tên miền, tổ chức, quốc qua, public key,….
**2. CRT**
- file chứa chứng chỉ SSL đã được cấp, chứa thông tin về chủ sở hữu, pulic key và các thông tin liên quan chứng chỉ.
**3.Private key**
-  file chứa khóa riêng tương ứng với khóa công khai trong chứng chỉ.
- Phải bảo mật tuyệt đối vì sẽ bị giả mạo website nêu bị lộ.
**4. Bundle**
- file chứa cả chứng chỉ chính, Intermediate và private key
- đơn giản hóa quá trình cài đặt chứng chỉ, đặc biệtt khi cần cài đặt chứng chỉ trung gian
**5. Intermediate**
- Đây là một chứng chỉ trung gian được sử dụng để tạo chuỗi chứng nhận. Nó xác nhận rằng chứng chỉ SSL của bạn đã được cấp bởi một cơ quan cấp chứng chỉ đáng tin cậy
-  giúp trình duyệt xác minh tính hợp lệ của chứng chỉ SSL
**6. PEM **
- Định dạng văn bản phổ biến nhất cho chứng chỉ SSL, CSR và khóa riêng.
**7. DER** 
- Định dạng nhị phân, thường được sử dụng để lưu trữ chứng chỉ.
**8. PKCS**
- Định dạng chứa cả chứng chỉ, khóa riêng và thông tin bổ sung, thường được sử dụng để xuất khẩu chứng chỉ từ các công cụ quản lý.


## Notes:
Dùng công cụ **openssl**  để chuyển hóa giữa các định dạng file 
vd: 

     openssl pkcs12 -export -out mycert.pfx -inkey mykey.pem -in mycert.pem -certfile cacert.pem
-SSL được gia hạn tối đa **13 tháng**.
- openssl x509 -enddate -noout -in /path/to/your/certificate.crt
 Gia hạn tự động SSL:
- Dùng phần mềm quản lý Hosting
- viết scipt , cerbot.
# DNS:
 Domain Name System:
   - Dùng để thay đổi địa chỉ thành tên miền.
 Cách hoạt động
1. Người dùng nhập tên miền vào trình duyệt
2. Trìnhh duyệt gửi request về DNS server
3. DNS server kiểm tra ip tương ứng vơi tên miền
4. DNS server trả về địa chỉ IP
5. Trình duyệt kết nối dến máy chủ chứa trang web

Các loại record
- **A Record**: Trỏ tên miền về IPv4
- **AAAA record** : trỏ tên miền về IPv6
- **Cname** : tạo biệt danh cho tên miền
 ** vd :    example.com   →   abc.example.com**
- **MX record**: Xác định máy chủ gửi mail
  + sử dụng cho dịch vụ Mail
- **NS record**: Xác định máy chủ tên miền  chịu trách nhiệm cho vùng
**vd: example.com → ns1.example.com**
- **TXT record**: xác thực tên miền, lưu trữ thông tin văn bản
- **SOA record** : thông tin về vùng DNS

- **DKIM** (DomainKeys Identified Mail) 
  - Xác thực email bằng cách sử dụng chữ ký kỹ thuật số. Khi một email được gửi đi, nó sẽ được ký bằng một cặp khóa công khai và khóa riêng.
  - Khóa công khai được đăng trên DNS của người gửi, trong khi khóa riêng được giữ bí mật.
  - Khi email đến máy chủ nhận, máy chủ này sẽ so sánh chữ ký trong email với khóa công khai được tìm thấy trên DNS. Nếu hai chữ ký khớp nhau, điều đó có nghĩa là email đó đến từ người gửi đã được xác thực.

- **DMARC** 
  - là một giao thức xây dựng trên nền tảng của DKIM và SPF (Sender Policy Framework).
  - Cho phép người quản trị tên miền xác định các chính sách xác thực email và nhận báo cáo về việc tuân thủ các chính sách đó.
## Notes:
 - 1 website có thể có nhiều record A, CNAME, MX,...
 - CNAME không thể trỏ về Root Domain
 - Chỉ có 1 SOA record





# DOMAIN:

Domain, hay còn gọi là tên miền, là một địa chỉ duy nhất trên Internet được gán cho một máy tính hoặc một mạng. 
Các cấp độ domain (Domain Levels)
Có nhiều cách phân loại domain, nhưng phổ biến nhất là dựa trên TLD:

+ **Generic Top-Level Domain (gTLD)**: Các TLD chung cho nhiều mục đích, ví dụ:
         .com: Thương mại
         .net: Mạng
         .org: Tổ chức phi lợi nhuận
         .info: Thông tin
         .edu: Giáo dục
         .gov: Chính phủ
+ **Country-code Top-Level Domain (ccTLD)**: Các TLD đại diện cho một quốc gia hoặc vùng lãnh thổ, ví dụ:
        .vn: Việt Nam
        .us: Hoa Kỳ
        .uk: Vương quốc Anh
+ **Sponsored Top-Level Domain (sTLD)**: Các TLD được tài trợ bởi các tổ chức cụ thể, 
         .aero (hàng không) 
         .biz (doanh nghiệp) 
         .coop (hợp tác xã).




	
# Mail server
## Các Giao Thức Chính:
   - **SMTP** (Simple Mail Transfer Protocol): Dùng để gửi email. Port 25
    - **POP3** (Post Office Protocol version 3): Dùng để nhận email và tải về máy tính của người dùng.
    - **IMAP** (Internet Message Access Protocol): Dùng để nhận email và quản lý trực tiếp trên máy chủ, đồng bộ hóa giữa nhiều thiết bị.
## Các Thành Phần Chính Của Mail Server:
   - **MTA (Mail Transfer Agent)**: Chịu trách nhiệm chuyển tiếp email giữa các máy chủ.
   - **MDA (Mail Delivery Agent)**: Chịu trách nhiệm giao email đến hộp thư của người nhận.
    - **MUA (Mail User Agent)**: Là phần mềm mà người dùng tương tác trực tiếp (ví dụ: Outlook, Gmail).
    - **Hộp thư**: Nơi lưu trữ các email của từng người dùng.
    - **Danh bạ**: Danh sách các địa chỉ email.
    - **Bộ lọc spam**: Loại bỏ email rác.
  - **Virus scanner**: Quét và loại bỏ virus trong email.
  - **Webmail**: Giao diện web cho phép người dùng truy cập email từ bất kỳ trình duyệt nào.
VD:

        example.com.	IN	MX	10	mail.example.com.
        example.com.	IN	MX	20	alt1.mail.example.com.
        example.com.	IN	MX	30	alt2.mail.example.com.
- example.com: Tên miền của công ty.
- IN: Loại bản ghi (Internet).
- MX: Kiểu bản ghi MX (Mail Exchanger).
    10, 20, 30: Priority (Ưu tiên). Số càng nhỏ, ưu tiên càng cao.
- mail.example.com, alt1.mail.example.com, alt2.mail.example.com: Tên các máy chủ email.

**Cách hoạt động**:

Khi một email được gửi đến địa chỉ [đã xoá địa chỉ email], máy chủ gửi thư sẽ lần lượt tìm kiếm các máy chủ email trong danh sách MX Record theo thứ tự ưu tiên.

    Đầu tiên: Nó sẽ cố gắng gửi email đến mail.example.com (có ưu tiên cao nhất).
    Nếu không thể: Nó sẽ chuyển sang alt1.mail.example.com.
    Nếu vẫn không thể: Nó sẽ chuyển sang alt2.mail.example.com.

Lợi ích của việc cấu hình như vậy:

    Độ ổn định: Nếu mail.example.com gặp sự cố, email vẫn có thể được gửi đến các máy chủ dự phòng.
    Cân bằng tải: Lưu lượng email được phân tán đều trên các máy chủ.
    Linh hoạt: Bạn có thể dễ dàng thêm hoặc xóa các MX Record để điều chỉnh cấu hình.

# Virtual Host
Virtual host (máy chủ ảo) là một kỹ thuật cho phép nhiều website khác nhau chạy trên cùng một máy chủ vật lý duy nhất. Mỗi website sẽ có một địa chỉ domain riêng, các file cấu hình riêng và các thư mục riêng, nhưng chúng đều được phục vụ bởi cùng một phần cứng.
    - Website 1: www.example.com - Một blog cá nhân.
    - Website 2: www.shop.example.com - Một cửa hàng trực tuyến.



    <VirtualHost *:80>
        ServerName www.example.com
        DocumentRoot /var/www/html/example
    </VirtualHost>
    <VirtualHost *:80>
        ServerName www.shop.example.com
        DocumentRoot /var/www/html/shop
    </VirtualHost>
    
**<VirtualHost :80>**: Bắt đầu một khối cấu hình cho một virtual host, ở đây là cổng 80 (HTTP).
**ServerName**: Chỉ định tên miền của website.
**DocumentRoot**: Xác định thư mục gốc chứa các file của website.

